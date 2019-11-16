<!-- markdownlint-disable MD002 MD041 -->

В этом упражнении вы будете расширяем приложение из предыдущего упражнения для поддержки проверки подлинности с помощью Azure AD. Это необходимо для получения необходимого маркера доступа OAuth для вызова Microsoft Graph. Для этого необходимо интегрировать [библиотеку проверки подлинности Microsoft (MSAL) для iOS](https://github.com/AzureAD/microsoft-authentication-library-for-objc) в приложение.

1. Создайте новый файл со **списком свойств** в проекте **Графтуториал** с именем **ауссеттингс. plist**.
1. Добавьте следующие элементы в файл **корневого** словаря.

    | Key | Тип | Значение |
    |-----|------|-------|
    | `AppId` | String | Идентификатор приложения на портале Azure |
    | `GraphScopes` | Массив | Два строковых значения: `User.Read` и`Calendars.Read` |

    ![Снимок экрана: файл Ауссеттингс. plist в Xcode](./images/auth-settings.png)

> [!IMPORTANT]
> Если вы используете систему управления версиями (например, Git), то теперь будет полезно исключить файл **ауссеттингс. plist** из системы управления версиями, чтобы избежать случайной утечки идентификатора приложения.

## <a name="implement-sign-in"></a>Реализация входа

В этом разделе описывается настройка проекта для MSAL, создание класса диспетчера проверки подлинности и обновление приложения для входа и выхода.

### <a name="configure-project-for-msal"></a>Настройка проекта для MSAL

1. Добавьте новую группу цепочки ключей в возможности проекта.
    1. Выберите проект **графтуториал** , а затем **подписывать & возможности**.
    1. Выберите пункт **+ возможность**, а затем дважды щелкните **общий доступ к цепочке ключей**.
    1. Добавьте группу цепочки ключей со значением `com.microsoft.adalcache`.

1. Нажмите кнопку **info. plist** и выберите **Открыть как**, а затем **Исходный код**.
1. Добавьте следующий `<dict>` элемент в элемент.

    ```xml
    <key>CFBundleURLTypes</key>
    <array>
      <dict>
        <key>CFBundleURLSchemes</key>
        <array>
          <string>msauth.$(PRODUCT_BUNDLE_IDENTIFIER)</string>
        </array>
      </dict>
    </array>
    <key>LSApplicationQueriesSchemes</key>
    <array>
        <string>msauthv2</string>
        <string>msauthv3</string>
    </array>
    ```

1. Откройте **аппделегате. SWIFT** и добавьте приведенный ниже оператор import в начало файла.

    ```Swift
    import MSAL
    ```

1. Добавьте к классу `AppDelegate` следующую функцию:

    ```Swift
    func application(_ app: UIApplication, open url: URL, options: [UIApplication.OpenURLOptionsKey : Any] = [:]) -> Bool {

        guard let sourceApplication = options[UIApplication.OpenURLOptionsKey.sourceApplication] as? String else {
            return false
        }

        return MSALPublicClientApplication.handleMSALResponse(url, sourceApplication: sourceApplication)
    }
    ```

### <a name="create-authentication-manager"></a>Создание диспетчера проверки подлинности

1. Создайте новый **файл SWIFT** в проекте **Графтуториал** с именем **AuthenticationManager. SWIFT**. Добавьте указанный ниже код в файл.

    ```Swift
    import Foundation
    import MSAL
    import MSGraphClientSDK

    // Implement the MSAuthenticationProvider interface so
    // this class can be used as an auth provider for the Graph SDK
    class AuthenticationManager: NSObject, MSAuthenticationProvider {

        // Implement singleton pattern
        static let instance = AuthenticationManager()

        private let publicClient: MSALPublicClientApplication?
        private let appId: String
        private let graphScopes: Array<String>

        private override init() {
            // Get app ID and scopes from AuthSettings.plist
            let bundle = Bundle.main
            let authConfigPath = bundle.path(forResource: "AuthSettings", ofType: "plist")!
            let authConfig = NSDictionary(contentsOfFile: authConfigPath)!

            self.appId = authConfig["AppId"] as! String
            self.graphScopes = authConfig["GraphScopes"] as! Array<String>

            do {
                // Create the MSAL client
                try self.publicClient = MSALPublicClientApplication(clientId: self.appId)
            } catch {
                print("Error creating MSAL public client: \(error)")
                self.publicClient = nil
            }
        }

        // Required function for the MSAuthenticationProvider interface
        func getAccessToken(for authProviderOptions: MSAuthenticationProviderOptions!, andCompletion completion: ((String?, Error?) -> Void)!) {
            getTokenSilently(completion: completion)
        }

        public func getTokenInteractively(parentView: UIViewController, completion: @escaping(_ accessToken: String?, Error?) -> Void) {
            let webParameters = MSALWebviewParameters(parentViewController: parentView)
            let interactiveParameters = MSALInteractiveTokenParameters(scopes: self.graphScopes,
                                                                       webviewParameters: webParameters)

            // Call acquireToken to open a browser so the user can sign in
            publicClient?.acquireToken(with: interactiveParameters, completionBlock: {
                (result: MSALResult?, error: Error?) in
                guard let tokenResult = result, error == nil else {
                    print("Error getting token interactively: \(String(describing: error))")
                    completion(nil, error)
                    return
                }

                print("Got token interactively: \(tokenResult.accessToken)")
                completion(tokenResult.accessToken, nil)
            })
        }

        public func getTokenSilently(completion: @escaping(_ accessToken: String?, Error?) -> Void) {
            // Check if there is an account in the cache
            var userAccount: MSALAccount?

            do {
                userAccount = try publicClient?.allAccounts().first
            } catch {
                print("Error getting account: \(error)")
            }

            if (userAccount != nil) {
                // Attempt to get token silently
                let silentParameters = MSALSilentTokenParameters(scopes: self.graphScopes, account: userAccount!)
                publicClient?.acquireTokenSilent(with: silentParameters, completionBlock: {
                    (result: MSALResult?, error: Error?) in
                    guard let tokenResult = result, error == nil else {
                        print("Error getting token silently: \(String(describing: error))")
                        completion(nil, error)
                        return
                    }

                    print("Got token silently: \(tokenResult.accessToken)")
                    completion(tokenResult.accessToken, nil)
                })
            } else {
                print("No account in cache")
                completion(nil, NSError(domain: "AuthenticationManager",
                                        code: MSALError.interactionRequired.rawValue, userInfo: nil))
            }
        }

        public func signOut() -> Void {
            do {
                // Remove all accounts from the cache
                let accounts = try publicClient?.allAccounts()

                try accounts!.forEach({
                    (account: MSALAccount) in
                    try publicClient?.remove(account)
                })
            } catch {
                print("Sign out error: \(String(describing: error))")
            }
        }
    }
    ```

### <a name="add-sign-in-and-sign-out"></a>Добавление входа и выхода

1. Откройте **сигнинвиевконтроллер. SWIFT** и замените его содержимое приведенным ниже кодом.

    ```Swift
    import UIKit

    class SignInViewController: UIViewController {

        private let spinner = SpinnerViewController()

        override func viewDidLoad() {
            super.viewDidLoad()
            // Do any additional setup after loading the view.

            // See if a user is already signed in
            spinner.start(container: self)

            AuthenticationManager.instance.getTokenSilently {
                (token: String?, error: Error?) in

                DispatchQueue.main.async {
                    self.spinner.stop()

                    guard let _ = token, error == nil else {
                        // If there is no token or if there's an error,
                        // no user is signed in, so stay here
                        return
                    }

                    // Since we got a token, a user is signed in
                    // Go to welcome page
                    self.performSegue(withIdentifier: "userSignedIn", sender: nil)
                }
            }
        }

        @IBAction func signIn() {
            spinner.start(container: self)

            // Do an interactive sign in
            AuthenticationManager.instance.getTokenInteractively(parentView: self) {
                (token: String?, error: Error?) in

                DispatchQueue.main.async {
                    self.spinner.stop()

                    guard let _ = token, error == nil else {
                        // Show the error and stay on the sign-in page
                        let alert = UIAlertController(title: "Error signing in",
                                                      message: error.debugDescription,
                                                      preferredStyle: .alert)

                        alert.addAction(UIAlertAction(title: "OK", style: .default, handler: nil))
                        self.present(alert, animated: true)
                        return
                    }

                    // Signed in successfully
                    // Go to welcome page
                    self.performSegue(withIdentifier: "userSignedIn", sender: nil)
                }
            }
        }
    }
    ```

1. Откройте **велкомевиевконтроллер. SWIFT** и замените существующую `signOut` функцию на приведенную ниже.

    ```Swift
    @IBAction func signOut() {
        AuthenticationManager.instance.signOut()
        self.performSegue(withIdentifier: "userSignedOut", sender: nil)
    }
    ```

1. Сохраните изменения и перезапустите приложение в симуляторе.

Если вы входите в приложение, в окне вывода в Xcode должен отображаться маркер доступа.

![Снимок экрана: окно вывода в Xcode, в котором отображается маркер доступа](./images/access-token-output.png)

## <a name="get-user-details"></a>Получение сведений о пользователе

В этом разделе описывается создание вспомогательного класса для хранения всех вызовов Microsoft Graph и обновление `WelcomeViewController` для использования этого нового класса для получения пользователя, выполнившего вход в систему.

1. Создайте новый **файл SWIFT** в проекте **Графтуториал** с именем **графманажер. SWIFT**. Добавьте указанный ниже код в файл.

    ```Swift
    import Foundation
    import MSGraphClientSDK
    import MSGraphClientModels

    class GraphManager {

        // Implement singleton pattern
        static let instance = GraphManager()

        private let client: MSHTTPClient?

        private init() {
            client = MSClientFactory.createHTTPClient(with: AuthenticationManager.instance)
        }

        public func getMe(completion: @escaping(MSGraphUser?, Error?) -> Void) {
            // GET /me
            let meRequest = NSMutableURLRequest(url: URL(string: "\(MSGraphBaseURL)/me")!)
            let meDataTask = MSURLSessionDataTask(request: meRequest, client: self.client, completion: {
                (data: Data?, response: URLResponse?, graphError: Error?) in
                guard let meData = data, graphError == nil else {
                    completion(nil, graphError)
                    return
                }

                do {
                    // Deserialize response as a user
                    let user = try MSGraphUser(data: meData)
                    completion(user, nil)
                } catch {
                    completion(nil, error)
                }
            })

            // Execute the request
            meDataTask?.execute()
        }
    }
    ```

1. Откройте **велкомевиевконтроллер. SWIFT** и добавьте приведенный `import` ниже оператор в начало файла.

    ```Swift
    import MSGraphClientModels
    ```

1. Добавьте в `WelcomeViewController` класс следующее свойство.

    ```Swift
    private let spinner = SpinnerViewController()
    ```

1. Замените существующий `viewDidLoad` код на следующий.

    ```Swift
    override func viewDidLoad() {
        super.viewDidLoad()

        // Do any additional setup after loading the view.

        self.spinner.start(container: self)

        // Get the signed-in user
        self.userProfilePhoto.image = UIImage(imageLiteralResourceName: "DefaultUserPhoto")

        GraphManager.instance.getMe {
            (user: MSGraphUser?, error: Error?) in

            DispatchQueue.main.async {
                self.spinner.stop()

                guard let currentUser = user, error == nil else {
                    print("Error getting user: \(String(describing: error))")
                    return
                }

                // Set display name
                self.userDisplayName.text = currentUser.displayName ?? "Mysterious Stranger"
                self.userDisplayName.sizeToFit()

                // AAD users have email in the mail attribute
                // Personal accounts have email in the userPrincipalName attribute
                self.userEmail.text = currentUser.mail ?? currentUser.userPrincipalName ?? ""
                self.userEmail.sizeToFit()
            }
        }
    }
    ```

Если вы сохраните изменения и перезапустите приложение, после того как вы обновите пользовательский интерфейс, отобразите отображаемое имя и адрес электронной почты пользователя.
