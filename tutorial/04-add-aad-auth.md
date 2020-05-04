<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="15c41-101">В этом упражнении вы будете расширяем приложение из предыдущего упражнения для поддержки проверки подлинности с помощью Azure AD.</span><span class="sxs-lookup"><span data-stu-id="15c41-101">In this exercise you will extend the application from the previous exercise to support authentication with Azure AD.</span></span> <span data-ttu-id="15c41-102">Это необходимо для получения необходимого маркера доступа OAuth для вызова Microsoft Graph.</span><span class="sxs-lookup"><span data-stu-id="15c41-102">This is required to obtain the necessary OAuth access token to call the Microsoft Graph.</span></span> <span data-ttu-id="15c41-103">Для этого необходимо интегрировать [библиотеку проверки подлинности Microsoft (MSAL) для iOS](https://github.com/AzureAD/microsoft-authentication-library-for-objc) в приложение.</span><span class="sxs-lookup"><span data-stu-id="15c41-103">To do this, you will integrate the [Microsoft Authentication Library (MSAL) for iOS](https://github.com/AzureAD/microsoft-authentication-library-for-objc) into the application.</span></span>

1. <span data-ttu-id="15c41-104">Создайте новый файл со **списком свойств** в проекте **Графтуториал** с именем **ауссеттингс. plist**.</span><span class="sxs-lookup"><span data-stu-id="15c41-104">Create a new **Property List** file in the **GraphTutorial** project named **AuthSettings.plist**.</span></span>
1. <span data-ttu-id="15c41-105">Добавьте следующие элементы в файл **корневого** словаря.</span><span class="sxs-lookup"><span data-stu-id="15c41-105">Add the following items to the file in the **Root** dictionary.</span></span>

    | <span data-ttu-id="15c41-106">Key</span><span class="sxs-lookup"><span data-stu-id="15c41-106">Key</span></span> | <span data-ttu-id="15c41-107">Тип</span><span class="sxs-lookup"><span data-stu-id="15c41-107">Type</span></span> | <span data-ttu-id="15c41-108">Значение</span><span class="sxs-lookup"><span data-stu-id="15c41-108">Value</span></span> |
    |-----|------|-------|
    | `AppId` | <span data-ttu-id="15c41-109">String</span><span class="sxs-lookup"><span data-stu-id="15c41-109">String</span></span> | <span data-ttu-id="15c41-110">Идентификатор приложения на портале Azure</span><span class="sxs-lookup"><span data-stu-id="15c41-110">The application ID from the Azure portal</span></span> |
    | `GraphScopes` | <span data-ttu-id="15c41-111">Массив</span><span class="sxs-lookup"><span data-stu-id="15c41-111">Array</span></span> | <span data-ttu-id="15c41-112">Два строковых значения: `User.Read` и`Calendars.Read`</span><span class="sxs-lookup"><span data-stu-id="15c41-112">Two String values: `User.Read` and `Calendars.Read`</span></span> |

    ![Снимок экрана: файл Ауссеттингс. plist в Xcode](./images/auth-settings.png)

> [!IMPORTANT]
> <span data-ttu-id="15c41-114">Если вы используете систему управления версиями (например, Git), то теперь будет полезно исключить файл **ауссеттингс. plist** из системы управления версиями, чтобы избежать случайной утечки идентификатора приложения.</span><span class="sxs-lookup"><span data-stu-id="15c41-114">If you're using source control such as git, now would be a good time to exclude the **AuthSettings.plist** file from source control to avoid inadvertently leaking your app ID.</span></span>

## <a name="implement-sign-in"></a><span data-ttu-id="15c41-115">Реализация входа</span><span class="sxs-lookup"><span data-stu-id="15c41-115">Implement sign-in</span></span>

<span data-ttu-id="15c41-116">В этом разделе описывается настройка проекта для MSAL, создание класса диспетчера проверки подлинности и обновление приложения для входа и выхода.</span><span class="sxs-lookup"><span data-stu-id="15c41-116">In this section you will configure the project for MSAL, create an authentication manager class, and update the app to sign in and sign out.</span></span>

### <a name="configure-project-for-msal"></a><span data-ttu-id="15c41-117">Настройка проекта для MSAL</span><span class="sxs-lookup"><span data-stu-id="15c41-117">Configure project for MSAL</span></span>

1. <span data-ttu-id="15c41-118">Добавьте новую группу цепочки ключей в возможности проекта.</span><span class="sxs-lookup"><span data-stu-id="15c41-118">Add a new keychain group to your project's capabilities.</span></span>
    1. <span data-ttu-id="15c41-119">Выберите проект **графтуториал** , а затем **подписывать & возможности**.</span><span class="sxs-lookup"><span data-stu-id="15c41-119">Select the **GraphTutorial** project, then **Signing & Capabilities**.</span></span>
    1. <span data-ttu-id="15c41-120">Выберите пункт **+ возможность**, а затем дважды щелкните **общий доступ к цепочке ключей**.</span><span class="sxs-lookup"><span data-stu-id="15c41-120">Select **+ Capability**, then double-click **Keychain Sharing**.</span></span>
    1. <span data-ttu-id="15c41-121">Добавьте группу цепочки ключей со значением `com.microsoft.adalcache`.</span><span class="sxs-lookup"><span data-stu-id="15c41-121">Add a keychain group with the value `com.microsoft.adalcache`.</span></span>

1. <span data-ttu-id="15c41-122">Нажмите кнопку **info. plist** и выберите **Открыть как**, а затем **Исходный код**.</span><span class="sxs-lookup"><span data-stu-id="15c41-122">Control click **Info.plist** and select **Open As**, then **Source Code**.</span></span>
1. <span data-ttu-id="15c41-123">Добавьте следующий `<dict>` элемент в элемент.</span><span class="sxs-lookup"><span data-stu-id="15c41-123">Add the following inside the `<dict>` element.</span></span>

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

1. <span data-ttu-id="15c41-124">Откройте **аппделегате. SWIFT** и добавьте приведенный ниже оператор import в начало файла.</span><span class="sxs-lookup"><span data-stu-id="15c41-124">Open **AppDelegate.swift** and add the following import statement at the top of the file.</span></span>

    ```Swift
    import MSAL
    ```

1. <span data-ttu-id="15c41-125">Добавьте к классу `AppDelegate` следующую функцию:</span><span class="sxs-lookup"><span data-stu-id="15c41-125">Add the following function to the `AppDelegate` class.</span></span>

    :::code language="swift" source="../demo/GraphTutorial/GraphTutorial/AppDelegate.swift" id="HandleMsalResponseSnippet":::

### <a name="create-authentication-manager"></a><span data-ttu-id="15c41-126">Создание диспетчера проверки подлинности</span><span class="sxs-lookup"><span data-stu-id="15c41-126">Create authentication manager</span></span>

1. <span data-ttu-id="15c41-127">Создайте новый **файл SWIFT** в проекте **Графтуториал** с именем **AuthenticationManager. SWIFT**.</span><span class="sxs-lookup"><span data-stu-id="15c41-127">Create a new **Swift File** in the **GraphTutorial** project named **AuthenticationManager.swift**.</span></span> <span data-ttu-id="15c41-128">Добавьте указанный ниже код в файл.</span><span class="sxs-lookup"><span data-stu-id="15c41-128">Add the following code to the file.</span></span>

    :::code language="swift" source="../demo/GraphTutorial/GraphTutorial/AuthenticationManager.swift" id="AuthManagerSnippet":::

### <a name="add-sign-in-and-sign-out"></a><span data-ttu-id="15c41-129">Добавление входа и выхода</span><span class="sxs-lookup"><span data-stu-id="15c41-129">Add sign-in and sign-out</span></span>

1. <span data-ttu-id="15c41-130">Откройте **сигнинвиевконтроллер. SWIFT** и замените его содержимое приведенным ниже кодом.</span><span class="sxs-lookup"><span data-stu-id="15c41-130">Open **SignInViewController.swift** and replace its contents with the following code.</span></span>

    :::code language="swift" source="../demo/GraphTutorial/GraphTutorial/SignInViewController.swift" id="SignInViewSnippet":::

1. <span data-ttu-id="15c41-131">Откройте **велкомевиевконтроллер. SWIFT** и замените существующую `signOut` функцию на приведенную ниже.</span><span class="sxs-lookup"><span data-stu-id="15c41-131">Open **WelcomeViewController.swift** and replace the existing `signOut` function with the following.</span></span>

    :::code language="swift" source="../demo/GraphTutorial/GraphTutorial/WelcomeViewController.swift" id="SignOutSnippet":::

1. <span data-ttu-id="15c41-132">Сохраните изменения и перезапустите приложение в симуляторе.</span><span class="sxs-lookup"><span data-stu-id="15c41-132">Save your changes and restart the application in Simulator.</span></span>

<span data-ttu-id="15c41-133">Если вы входите в приложение, в окне вывода в Xcode должен отображаться маркер доступа.</span><span class="sxs-lookup"><span data-stu-id="15c41-133">If you sign in to the app, you should see an access token displayed in the output window in Xcode.</span></span>

![Снимок экрана: окно вывода в Xcode, в котором отображается маркер доступа](./images/access-token-output.png)

## <a name="get-user-details"></a><span data-ttu-id="15c41-135">Получение сведений о пользователе</span><span class="sxs-lookup"><span data-stu-id="15c41-135">Get user details</span></span>

<span data-ttu-id="15c41-136">В этом разделе описывается создание вспомогательного класса для хранения всех вызовов Microsoft Graph и обновление `WelcomeViewController` для использования этого нового класса для получения пользователя, выполнившего вход в систему.</span><span class="sxs-lookup"><span data-stu-id="15c41-136">In this section you will create a helper class to hold all of the calls to Microsoft Graph and update the `WelcomeViewController` to use this new class to get the logged-in user.</span></span>

1. <span data-ttu-id="15c41-137">Создайте новый **файл SWIFT** в проекте **Графтуториал** с именем **графманажер. SWIFT**.</span><span class="sxs-lookup"><span data-stu-id="15c41-137">Create a new **Swift File** in the **GraphTutorial** project named **GraphManager.swift**.</span></span> <span data-ttu-id="15c41-138">Добавьте указанный ниже код в файл.</span><span class="sxs-lookup"><span data-stu-id="15c41-138">Add the following code to the file.</span></span>

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

1. <span data-ttu-id="15c41-139">Откройте **велкомевиевконтроллер. SWIFT** и добавьте приведенный `import` ниже оператор в начало файла.</span><span class="sxs-lookup"><span data-stu-id="15c41-139">Open **WelcomeViewController.swift** and add the following `import` statement at the top of the file.</span></span>

    ```Swift
    import MSGraphClientModels
    ```

1. <span data-ttu-id="15c41-140">Добавьте в `WelcomeViewController` класс следующее свойство.</span><span class="sxs-lookup"><span data-stu-id="15c41-140">Add the following property to the `WelcomeViewController` class.</span></span>

    ```Swift
    private let spinner = SpinnerViewController()
    ```

1. <span data-ttu-id="15c41-141">Замените существующий `viewDidLoad` код на следующий.</span><span class="sxs-lookup"><span data-stu-id="15c41-141">Replace the existing `viewDidLoad` with the following code.</span></span>

    :::code language="swift" source="../demo/GraphTutorial/GraphTutorial/WelcomeViewController.swift" id="ViewDidLoadSnippet":::

<span data-ttu-id="15c41-142">Если вы сохраните изменения и перезапустите приложение, после того как вы обновите пользовательский интерфейс, отобразите отображаемое имя и адрес электронной почты пользователя.</span><span class="sxs-lookup"><span data-stu-id="15c41-142">If you save your changes and restart the app now, after sign-in the UI is updated with the user's display name and email address.</span></span>
