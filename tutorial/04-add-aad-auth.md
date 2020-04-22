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

    :::code language="swift" source="../demo/GraphTutorial/GraphTutorial/AppDelegate.swift" id="HandleMsalResponseSnippet":::

### <a name="create-authentication-manager"></a>Создание диспетчера проверки подлинности

1. Создайте новый **файл SWIFT** в проекте **Графтуториал** с именем **AuthenticationManager. SWIFT**. Добавьте указанный ниже код в файл.

    :::code language="swift" source="../demo/GraphTutorial/GraphTutorial/AuthenticationManager.swift" id="AuthManagerSnippet":::

### <a name="add-sign-in-and-sign-out"></a>Добавление входа и выхода

1. Откройте **сигнинвиевконтроллер. SWIFT** и замените его содержимое приведенным ниже кодом.

    :::code language="swift" source="../demo/GraphTutorial/GraphTutorial/SignInViewController.swift" id="SignInViewSnippet":::

1. Откройте **велкомевиевконтроллер. SWIFT** и замените существующую `signOut` функцию на приведенную ниже.

    :::code language="swift" source="../demo/GraphTutorial/GraphTutorial/WelcomeViewController.swift" id="SignOutSnippet":::

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

    :::code language="swift" source="../demo/GraphTutorial/GraphTutorial/WelcomeViewController.swift" id="ViewDidLoadSnippet":::

Если вы сохраните изменения и перезапустите приложение, после того как вы обновите пользовательский интерфейс, отобразите отображаемое имя и адрес электронной почты пользователя.
