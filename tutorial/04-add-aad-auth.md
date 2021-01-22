<!-- markdownlint-disable MD002 MD041 -->

В этом упражнении вы расширим приложение из предыдущего упражнения, чтобы поддерживать проверку подлинности с помощью Azure AD. Это необходимо для получения необходимого маркера доступа OAuth для вызова Microsoft Graph. Для этого в приложение будет интегрирована библиотека проверки подлинности [Майкрософт (MSAL) для iOS.](https://github.com/AzureAD/microsoft-authentication-library-for-objc)

1. Создайте файл **списка свойств** в **проекте GraphTutorial** **с именем AuthSettings.plist.**
1. Добавьте следующие элементы в файл в **корневом словаре.**

    | Key | Тип | Значение |
    |-----|------|-------|
    | `AppId` | String | ИД приложения на портале Azure |
    | `GraphScopes` | Массив | Три строковые значения: `User.Read` , `MailboxSettings.Read` и `Calendars.ReadWrite` |

    ![Снимок экрана: файл AuthSettings.plist в Xcode](images/auth-settings.png)

> [!IMPORTANT]
> Если вы используете управление исходным кодом, например git, пришло бы время исключить файл **AuthSettings.plist** из системы управления исходным кодом, чтобы не допустить случайной утечки вашего ИД приложения.

## <a name="implement-sign-in"></a>Реализация входов

В этом разделе мы настроим проект для MSAL, создадим класс диспетчера проверки подлинности и обновим приложение для входов и выходов.

### <a name="configure-project-for-msal"></a>Настройка проекта для MSAL

1. Добавьте новую группу ключей в возможности проекта.
    1. Выберите проект **GraphTutorial,** а затем **& подписываю.**
    1. Select **+ Capability**, then double-click **Keychain Sharing**.
    1. Добавьте группу ключей с этим `com.microsoft.adalcache` значением.

1. Control click **Info.plist** and select **Open As**, then **Source Code**.
1. Добавьте в элемент `<dict>` следующее:

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

1. Откройте **файл AppDelegate.swift** и добавьте в верхней части файла следующую выписку по импорту.

    ```Swift
    import MSAL
    ```

1. Добавьте к классу `AppDelegate` следующую функцию:

    :::code language="swift" source="../demo/GraphTutorial/GraphTutorial/AppDelegate.swift" id="HandleMsalResponseSnippet":::

### <a name="create-authentication-manager"></a>Создание диспетчера проверки подлинности

1. Создайте новый **swift-файл** в **проекте GraphTutorial** **с именем AuthenticationManager.swift.** Добавьте указанный ниже код в файл.

    :::code language="swift" source="../demo/GraphTutorial/GraphTutorial/AuthenticationManager.swift" id="AuthManagerSnippet":::

### <a name="add-sign-in-and-sign-out"></a>Добавление и выход из нее

1. Откройте **SignInViewController.swift** и замените его содержимое следующим кодом.

    :::code language="swift" source="../demo/GraphTutorial/GraphTutorial/SignInViewController.swift" id="SignInViewSnippet":::

1. Откройте **WelcomeViewController.swift** и замените существующую `signOut` функцию на следующую.

    :::code language="swift" source="../demo/GraphTutorial/GraphTutorial/WelcomeViewController.swift" id="SignOutSnippet":::

1. Сохраните изменения и перезапустите приложение в симуляторе.

При входе в приложение маркер доступа должен отображаться в окне выходных данных в Xcode.

![Снимок экрана: окно вывода в Xcode с маркером доступа](images/access-token-output.png)

## <a name="get-user-details"></a>Получить сведения о пользователе

В этом разделе вы создадим дополнительный класс для удержания всех вызовов Microsoft Graph и обновим его, чтобы использовать этот новый класс для получения во `WelcomeViewController` входе пользователя.

1. Создайте новый **swift-файл** в **проекте GraphTutorial** **с именем GraphManager.swift.** Добавьте указанный ниже код в файл.

    ```Swift
    import Foundation
    import MSGraphClientSDK
    import MSGraphClientModels

    class GraphManager {

        // Implement singleton pattern
        static let instance = GraphManager()

        private let client: MSHTTPClient?

        public var userTimeZone: String

        private init() {
            client = MSClientFactory.createHTTPClient(with: AuthenticationManager.instance)
            userTimeZone = "UTC"
        }

        public func getMe(completion: @escaping(MSGraphUser?, Error?) -> Void) {
            // GET /me
            let select = "$select=displayName,mail,mailboxSettings,userPrincipalName"
            let meRequest = NSMutableURLRequest(url: URL(string: "\(MSGraphBaseURL)/me?\(select)")!)
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

1. Откройте **Файл WelcomeViewController.swift** и добавьте в верхнюю часть файла следующий `import` выписку.

    ```Swift
    import MSGraphClientModels
    ```

1. Добавьте в класс следующее `WelcomeViewController` свойство.

    ```Swift
    private let spinner = SpinnerViewController()
    ```

1. Замените `viewDidLoad` существующий следующим кодом.

    :::code language="swift" source="../demo/GraphTutorial/GraphTutorial/WelcomeViewController.swift" id="ViewDidLoadSnippet":::

Если вы сохраните изменения и перезапустите приложение, после того как вы войдите в пользовательский интерфейс, обновите его отображаемого имени и адрес электронной почты.
