<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="47223-101">В этом упражнении вы расширим приложение из предыдущего упражнения, чтобы поддерживать проверку подлинности с помощью Azure AD.</span><span class="sxs-lookup"><span data-stu-id="47223-101">In this exercise you will extend the application from the previous exercise to support authentication with Azure AD.</span></span> <span data-ttu-id="47223-102">Это необходимо для получения необходимого маркера доступа OAuth для вызова Microsoft Graph.</span><span class="sxs-lookup"><span data-stu-id="47223-102">This is required to obtain the necessary OAuth access token to call the Microsoft Graph.</span></span> <span data-ttu-id="47223-103">Для этого в приложение будет интегрирована библиотека проверки подлинности [Майкрософт (MSAL) для iOS.](https://github.com/AzureAD/microsoft-authentication-library-for-objc)</span><span class="sxs-lookup"><span data-stu-id="47223-103">To do this, you will integrate the [Microsoft Authentication Library (MSAL) for iOS](https://github.com/AzureAD/microsoft-authentication-library-for-objc) into the application.</span></span>

1. <span data-ttu-id="47223-104">Создайте файл **списка свойств** в **проекте GraphTutorial** **с именем AuthSettings.plist.**</span><span class="sxs-lookup"><span data-stu-id="47223-104">Create a new **Property List** file in the **GraphTutorial** project named **AuthSettings.plist**.</span></span>
1. <span data-ttu-id="47223-105">Добавьте следующие элементы в файл в **корневом словаре.**</span><span class="sxs-lookup"><span data-stu-id="47223-105">Add the following items to the file in the **Root** dictionary.</span></span>

    | <span data-ttu-id="47223-106">Key</span><span class="sxs-lookup"><span data-stu-id="47223-106">Key</span></span> | <span data-ttu-id="47223-107">Тип</span><span class="sxs-lookup"><span data-stu-id="47223-107">Type</span></span> | <span data-ttu-id="47223-108">Значение</span><span class="sxs-lookup"><span data-stu-id="47223-108">Value</span></span> |
    |-----|------|-------|
    | `AppId` | <span data-ttu-id="47223-109">String</span><span class="sxs-lookup"><span data-stu-id="47223-109">String</span></span> | <span data-ttu-id="47223-110">ИД приложения на портале Azure</span><span class="sxs-lookup"><span data-stu-id="47223-110">The application ID from the Azure portal</span></span> |
    | `GraphScopes` | <span data-ttu-id="47223-111">Массив</span><span class="sxs-lookup"><span data-stu-id="47223-111">Array</span></span> | <span data-ttu-id="47223-112">Три строковые значения: `User.Read` , `MailboxSettings.Read` и `Calendars.ReadWrite`</span><span class="sxs-lookup"><span data-stu-id="47223-112">Three String values: `User.Read`, `MailboxSettings.Read`, and `Calendars.ReadWrite`</span></span> |

    ![Снимок экрана: файл AuthSettings.plist в Xcode](images/auth-settings.png)

> [!IMPORTANT]
> <span data-ttu-id="47223-114">Если вы используете управление исходным кодом, например git, пришло бы время исключить файл **AuthSettings.plist** из системы управления исходным кодом, чтобы не допустить случайной утечки вашего ИД приложения.</span><span class="sxs-lookup"><span data-stu-id="47223-114">If you're using source control such as git, now would be a good time to exclude the **AuthSettings.plist** file from source control to avoid inadvertently leaking your app ID.</span></span>

## <a name="implement-sign-in"></a><span data-ttu-id="47223-115">Реализация входов</span><span class="sxs-lookup"><span data-stu-id="47223-115">Implement sign-in</span></span>

<span data-ttu-id="47223-116">В этом разделе мы настроим проект для MSAL, создадим класс диспетчера проверки подлинности и обновим приложение для входов и выходов.</span><span class="sxs-lookup"><span data-stu-id="47223-116">In this section you will configure the project for MSAL, create an authentication manager class, and update the app to sign in and sign out.</span></span>

### <a name="configure-project-for-msal"></a><span data-ttu-id="47223-117">Настройка проекта для MSAL</span><span class="sxs-lookup"><span data-stu-id="47223-117">Configure project for MSAL</span></span>

1. <span data-ttu-id="47223-118">Добавьте новую группу ключей в возможности проекта.</span><span class="sxs-lookup"><span data-stu-id="47223-118">Add a new keychain group to your project's capabilities.</span></span>
    1. <span data-ttu-id="47223-119">Выберите проект **GraphTutorial,** а затем **& подписываю.**</span><span class="sxs-lookup"><span data-stu-id="47223-119">Select the **GraphTutorial** project, then **Signing & Capabilities**.</span></span>
    1. <span data-ttu-id="47223-120">Select **+ Capability**, then double-click **Keychain Sharing**.</span><span class="sxs-lookup"><span data-stu-id="47223-120">Select **+ Capability**, then double-click **Keychain Sharing**.</span></span>
    1. <span data-ttu-id="47223-121">Добавьте группу ключей с этим `com.microsoft.adalcache` значением.</span><span class="sxs-lookup"><span data-stu-id="47223-121">Add a keychain group with the value `com.microsoft.adalcache`.</span></span>

1. <span data-ttu-id="47223-122">Control click **Info.plist** and select **Open As**, then **Source Code**.</span><span class="sxs-lookup"><span data-stu-id="47223-122">Control click **Info.plist** and select **Open As**, then **Source Code**.</span></span>
1. <span data-ttu-id="47223-123">Добавьте в элемент `<dict>` следующее:</span><span class="sxs-lookup"><span data-stu-id="47223-123">Add the following inside the `<dict>` element.</span></span>

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

1. <span data-ttu-id="47223-124">Откройте **файл AppDelegate.swift** и добавьте в верхней части файла следующую выписку по импорту.</span><span class="sxs-lookup"><span data-stu-id="47223-124">Open **AppDelegate.swift** and add the following import statement at the top of the file.</span></span>

    ```Swift
    import MSAL
    ```

1. <span data-ttu-id="47223-125">Добавьте к классу `AppDelegate` следующую функцию:</span><span class="sxs-lookup"><span data-stu-id="47223-125">Add the following function to the `AppDelegate` class.</span></span>

    :::code language="swift" source="../demo/GraphTutorial/GraphTutorial/AppDelegate.swift" id="HandleMsalResponseSnippet":::

### <a name="create-authentication-manager"></a><span data-ttu-id="47223-126">Создание диспетчера проверки подлинности</span><span class="sxs-lookup"><span data-stu-id="47223-126">Create authentication manager</span></span>

1. <span data-ttu-id="47223-127">Создайте новый **swift-файл** в **проекте GraphTutorial** **с именем AuthenticationManager.swift.**</span><span class="sxs-lookup"><span data-stu-id="47223-127">Create a new **Swift File** in the **GraphTutorial** project named **AuthenticationManager.swift**.</span></span> <span data-ttu-id="47223-128">Добавьте указанный ниже код в файл.</span><span class="sxs-lookup"><span data-stu-id="47223-128">Add the following code to the file.</span></span>

    :::code language="swift" source="../demo/GraphTutorial/GraphTutorial/AuthenticationManager.swift" id="AuthManagerSnippet":::

### <a name="add-sign-in-and-sign-out"></a><span data-ttu-id="47223-129">Добавление и выход из нее</span><span class="sxs-lookup"><span data-stu-id="47223-129">Add sign-in and sign-out</span></span>

1. <span data-ttu-id="47223-130">Откройте **SignInViewController.swift** и замените его содержимое следующим кодом.</span><span class="sxs-lookup"><span data-stu-id="47223-130">Open **SignInViewController.swift** and replace its contents with the following code.</span></span>

    :::code language="swift" source="../demo/GraphTutorial/GraphTutorial/SignInViewController.swift" id="SignInViewSnippet":::

1. <span data-ttu-id="47223-131">Откройте **WelcomeViewController.swift** и замените существующую `signOut` функцию на следующую.</span><span class="sxs-lookup"><span data-stu-id="47223-131">Open **WelcomeViewController.swift** and replace the existing `signOut` function with the following.</span></span>

    :::code language="swift" source="../demo/GraphTutorial/GraphTutorial/WelcomeViewController.swift" id="SignOutSnippet":::

1. <span data-ttu-id="47223-132">Сохраните изменения и перезапустите приложение в симуляторе.</span><span class="sxs-lookup"><span data-stu-id="47223-132">Save your changes and restart the application in Simulator.</span></span>

<span data-ttu-id="47223-133">При входе в приложение маркер доступа должен отображаться в окне выходных данных в Xcode.</span><span class="sxs-lookup"><span data-stu-id="47223-133">If you sign in to the app, you should see an access token displayed in the output window in Xcode.</span></span>

![Снимок экрана: окно вывода в Xcode с маркером доступа](images/access-token-output.png)

## <a name="get-user-details"></a><span data-ttu-id="47223-135">Получить сведения о пользователе</span><span class="sxs-lookup"><span data-stu-id="47223-135">Get user details</span></span>

<span data-ttu-id="47223-136">В этом разделе вы создадим дополнительный класс для удержания всех вызовов Microsoft Graph и обновим его, чтобы использовать этот новый класс для получения во `WelcomeViewController` входе пользователя.</span><span class="sxs-lookup"><span data-stu-id="47223-136">In this section you will create a helper class to hold all of the calls to Microsoft Graph and update the `WelcomeViewController` to use this new class to get the logged-in user.</span></span>

1. <span data-ttu-id="47223-137">Создайте новый **swift-файл** в **проекте GraphTutorial** **с именем GraphManager.swift.**</span><span class="sxs-lookup"><span data-stu-id="47223-137">Create a new **Swift File** in the **GraphTutorial** project named **GraphManager.swift**.</span></span> <span data-ttu-id="47223-138">Добавьте указанный ниже код в файл.</span><span class="sxs-lookup"><span data-stu-id="47223-138">Add the following code to the file.</span></span>

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

1. <span data-ttu-id="47223-139">Откройте **Файл WelcomeViewController.swift** и добавьте в верхнюю часть файла следующий `import` выписку.</span><span class="sxs-lookup"><span data-stu-id="47223-139">Open **WelcomeViewController.swift** and add the following `import` statement at the top of the file.</span></span>

    ```Swift
    import MSGraphClientModels
    ```

1. <span data-ttu-id="47223-140">Добавьте в класс следующее `WelcomeViewController` свойство.</span><span class="sxs-lookup"><span data-stu-id="47223-140">Add the following property to the `WelcomeViewController` class.</span></span>

    ```Swift
    private let spinner = SpinnerViewController()
    ```

1. <span data-ttu-id="47223-141">Замените `viewDidLoad` существующий следующим кодом.</span><span class="sxs-lookup"><span data-stu-id="47223-141">Replace the existing `viewDidLoad` with the following code.</span></span>

    :::code language="swift" source="../demo/GraphTutorial/GraphTutorial/WelcomeViewController.swift" id="ViewDidLoadSnippet":::

<span data-ttu-id="47223-142">Если вы сохраните изменения и перезапустите приложение, после того как вы войдите в пользовательский интерфейс, обновите его отображаемого имени и адрес электронной почты.</span><span class="sxs-lookup"><span data-stu-id="47223-142">If you save your changes and restart the app now, after sign-in the UI is updated with the user's display name and email address.</span></span>
