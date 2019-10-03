<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="08f78-101">Начните с создания нового проекта SWIFT.</span><span class="sxs-lookup"><span data-stu-id="08f78-101">Begin by creating a new Swift project.</span></span>

1. <span data-ttu-id="08f78-102">Откройте Xcode.</span><span class="sxs-lookup"><span data-stu-id="08f78-102">Open Xcode.</span></span> <span data-ttu-id="08f78-103">В меню **файл** выберите пункт **создать**, а затем — **проект**.</span><span class="sxs-lookup"><span data-stu-id="08f78-103">On the **File** menu, select **New**, then **Project**.</span></span>
1. <span data-ttu-id="08f78-104">Выберите шаблон **приложения с одним представлением** и нажмите кнопку **Далее**.</span><span class="sxs-lookup"><span data-stu-id="08f78-104">Choose the **Single View App** template and select **Next**.</span></span>

    ![Снимок экрана с диалоговым окном выбора шаблона Xcode](./images/xcode-select-template.png)

1. <span data-ttu-id="08f78-106">Задайте для **имени продукта** значение `GraphTutorial` **SWIFT**и **язык** .</span><span class="sxs-lookup"><span data-stu-id="08f78-106">Set the **Product Name** to `GraphTutorial` and the **Language** to **Swift**.</span></span>
1. <span data-ttu-id="08f78-107">Заполните оставшиеся поля и нажмите кнопку **Далее**.</span><span class="sxs-lookup"><span data-stu-id="08f78-107">Fill in the remaining fields and select **Next**.</span></span>
1. <span data-ttu-id="08f78-108">Выберите расположение для проекта и нажмите кнопку **создать**.</span><span class="sxs-lookup"><span data-stu-id="08f78-108">Choose a location for the project and select **Create**.</span></span>

## <a name="install-dependencies"></a><span data-ttu-id="08f78-109">Установка зависимостей</span><span class="sxs-lookup"><span data-stu-id="08f78-109">Install dependencies</span></span>

<span data-ttu-id="08f78-110">Перед перемещением установите некоторые дополнительные зависимости, которые будут использоваться позже.</span><span class="sxs-lookup"><span data-stu-id="08f78-110">Before moving on, install some additional dependencies that you will use later.</span></span>

- <span data-ttu-id="08f78-111">[Библиотека проверки подлинности Microsoft (MSAL) для iOS](https://github.com/AzureAD/microsoft-authentication-library-for-objc) для проверки подлинности в Azure AD.</span><span class="sxs-lookup"><span data-stu-id="08f78-111">[Microsoft Authentication Library (MSAL) for iOS](https://github.com/AzureAD/microsoft-authentication-library-for-objc) for authenticating to with Azure AD.</span></span>
- <span data-ttu-id="08f78-112">[Поставщик проверки подлинности MSAL для цели C](https://github.com/microsoftgraph/msgraph-sdk-objc-auth) , чтобы подключить MSAL с помощью пакета SDK Microsoft Graph.</span><span class="sxs-lookup"><span data-stu-id="08f78-112">[MSAL Authentication Provider for Objective C](https://github.com/microsoftgraph/msgraph-sdk-objc-auth) to connect MSAL with the Microsoft Graph SDK.</span></span>
- <span data-ttu-id="08f78-113">[Пакет SDK Microsoft Graph для задания на языке C](https://github.com/microsoftgraph/msgraph-sdk-objc) для совершения вызовов в Microsoft Graph.</span><span class="sxs-lookup"><span data-stu-id="08f78-113">[Microsoft Graph SDK for Objective C](https://github.com/microsoftgraph/msgraph-sdk-objc) for making calls to Microsoft Graph.</span></span>
- <span data-ttu-id="08f78-114">[Пакет SDK моделей Microsoft Graph для задания с](https://github.com/microsoftgraph/msgraph-sdk-objc-models) строго типизированными объектами, представляющими ресурсы Microsoft Graph, такие как "Пользователи" или "события".</span><span class="sxs-lookup"><span data-stu-id="08f78-114">[Microsoft Graph Models SDK for Objective C](https://github.com/microsoftgraph/msgraph-sdk-objc-models) for strongly-typed objects representing Microsoft Graph resources like users or events.</span></span>

1. <span data-ttu-id="08f78-115">Выйдите из Xcode.</span><span class="sxs-lookup"><span data-stu-id="08f78-115">Quit Xcode.</span></span>
1. <span data-ttu-id="08f78-116">Откройте терминал и измените каталог на путь к проекту **графтуториал** .</span><span class="sxs-lookup"><span data-stu-id="08f78-116">Open Terminal and change the directory to the location of your **GraphTutorial** project.</span></span>
1. <span data-ttu-id="08f78-117">Выполните следующую команду, чтобы создать Podfile.</span><span class="sxs-lookup"><span data-stu-id="08f78-117">Run the following command to create a Podfile.</span></span>

    ```Shell
    pod init
    ```

1. <span data-ttu-id="08f78-118">Откройте Podfile и добавьте следующие строки сразу после `use_frameworks!` строки.</span><span class="sxs-lookup"><span data-stu-id="08f78-118">Open the Podfile and add the following lines just after the `use_frameworks!` line.</span></span>

    ```Ruby
    pod 'MSAL', '~> 0.3.0'
    pod 'MSGraphMSALAuthProvider', '~> 0.1.1'
    pod 'MSGraphClientSDK', ' ~> 0.1.3'
    pod 'MSGraphClientModels', '~> 0.1.1'
    ```

1. <span data-ttu-id="08f78-119">Сохраните Podfile, а затем выполните следующую команду, чтобы установить зависимости.</span><span class="sxs-lookup"><span data-stu-id="08f78-119">Save the Podfile, then run the following command to install the dependencies.</span></span>

    ```Shell
    pod install
    ```

1. <span data-ttu-id="08f78-120">После выполнения команды откройте недавно созданный **графтуториал. xcworkspace** в Xcode.</span><span class="sxs-lookup"><span data-stu-id="08f78-120">Once the command completes, open the newly created **GraphTutorial.xcworkspace** in Xcode.</span></span>

## <a name="design-the-app"></a><span data-ttu-id="08f78-121">Проектирование приложения</span><span class="sxs-lookup"><span data-stu-id="08f78-121">Design the app</span></span>

<span data-ttu-id="08f78-122">В этом разделе вы создадите представления приложения: страница входа, навигатор панели вкладок, страница приветствия и страница календаря.</span><span class="sxs-lookup"><span data-stu-id="08f78-122">In this section you will create the views for the app: a sign in page, a tab bar navigator, a welcome page, and a calendar page.</span></span> <span data-ttu-id="08f78-123">Вы также создадите наложение индикатора действий.</span><span class="sxs-lookup"><span data-stu-id="08f78-123">You'll also create an activity indicator overlay.</span></span>

### <a name="create-sign-in-page"></a><span data-ttu-id="08f78-124">Страница "Создание входа"</span><span class="sxs-lookup"><span data-stu-id="08f78-124">Create sign in page</span></span>

1. <span data-ttu-id="08f78-125">Разверните папку **графтуториал** в Xcode, а затем выберите **ViewController. SWIFT**.</span><span class="sxs-lookup"><span data-stu-id="08f78-125">Expand the **GraphTutorial** folder in Xcode, then select **ViewController.swift**.</span></span>
1. <span data-ttu-id="08f78-126">В **инспекторе файлов**измените **имя** файла на `SignInViewController.swift`.</span><span class="sxs-lookup"><span data-stu-id="08f78-126">In the **File Inspector**, change the **Name** of the file to `SignInViewController.swift`.</span></span>

    ![Снимок экрана: инспектор файлов](./images/rename-file.png)

1. <span data-ttu-id="08f78-128">Откройте **сигнинвиевконтроллер. SWIFT** и замените его содержимое приведенным ниже кодом.</span><span class="sxs-lookup"><span data-stu-id="08f78-128">Open **SignInViewController.swift** and replace its contents with the following code.</span></span>

    ```Swift
    import UIKit

    class SignInViewController: UIViewController {

        override func viewDidLoad() {
            super.viewDidLoad()
            // Do any additional setup after loading the view.
        }

        @IBAction func signIn() {
            self.performSegue(withIdentifier: "userSignedIn", sender: nil)
        }
    }
    ```

1. <span data-ttu-id="08f78-129">Откройте файл **Main. Storyboard** .</span><span class="sxs-lookup"><span data-stu-id="08f78-129">Open the **Main.storyboard** file.</span></span>
1. <span data-ttu-id="08f78-130">Разверните панель **View Controller сцена**и выберите **View Controller (просмотреть контроллер**).</span><span class="sxs-lookup"><span data-stu-id="08f78-130">Expand **View Controller Scene**, then select **View Controller**.</span></span>

    ![Снимок экрана Xcode с выбранным контроллером просмотра](./images/storyboard-select-view-controller.png)

1. <span data-ttu-id="08f78-132">Выберите **инспектор удостоверений**, а затем измените раскрывающееся меню **Class** на **сигнинвиевконтроллер**.</span><span class="sxs-lookup"><span data-stu-id="08f78-132">Select the **Identity Inspector**, then change the **Class** dropdown to **SignInViewController**.</span></span>

    ![Снимок экрана: инспектор удостоверений](./images/change-class.png)

1. <span data-ttu-id="08f78-134">Выберите **библиотеку**, а затем перетащите **кнопку** на **Контроллер входа в систему**.</span><span class="sxs-lookup"><span data-stu-id="08f78-134">Select the **Library**, then drag a **Button** onto the **Sign In View Controller**.</span></span>

    ![Снимок экрана библиотеки в Xcode](./images/add-button-to-view.png)

1. <span data-ttu-id="08f78-136">Когда выбрана кнопка, выберите **инспектор атрибутов** и измените **название** кнопки на `Sign In`.</span><span class="sxs-lookup"><span data-stu-id="08f78-136">With the button selected, select the **Attributes Inspector** and change the **Title** of the button to `Sign In`.</span></span>

    ![Снимок экрана: поле Title (название) в инспекторе атрибутов в Xcode](./images/set-button-title.png)

1. <span data-ttu-id="08f78-138">Выберите **Контроллер входа в систему**, а затем выберите **инспектор подключений**.</span><span class="sxs-lookup"><span data-stu-id="08f78-138">Select the **Sign In View Controller**, then select the **Connections Inspector**.</span></span>
1. <span data-ttu-id="08f78-139">В разделе **полученное действие**перетащите незаполненный круг рядом с кнопкой **войти** на кнопку.</span><span class="sxs-lookup"><span data-stu-id="08f78-139">Under **Received Actions**, drag the unfilled circle next to **signIn** onto the button.</span></span> <span data-ttu-id="08f78-140">Во всплывающем меню выберите пункт " **находящиеся вверх** ".</span><span class="sxs-lookup"><span data-stu-id="08f78-140">Select **Touch Up Inside** on the pop-up menu.</span></span>

    ![Снимок экрана: перетаскивание действия Signing на кнопку в Xcode](./images/connect-sign-in-button.png)

1. <span data-ttu-id="08f78-142">В меню **Редактор** выберите **устранить проблемы с автоматическим макетом**, а затем выберите **добавить недостающие ограничения** под **все представления в контроллере входа в систему**.</span><span class="sxs-lookup"><span data-stu-id="08f78-142">On the **Editor** menu, select **Resolve Auto Layout Issues**, then select **Add Missing Constraints** underneath **All Views in Sign In View Controller**.</span></span>

### <a name="create-tab-bar"></a><span data-ttu-id="08f78-143">Создание панели вкладок</span><span class="sxs-lookup"><span data-stu-id="08f78-143">Create tab bar</span></span>

1. <span data-ttu-id="08f78-144">Выберите **библиотеку**, а затем перетащите **контроллер панели вкладок** на раскадровку.</span><span class="sxs-lookup"><span data-stu-id="08f78-144">Select the **Library**, then drag a **Tab Bar Controller** onto the storyboard.</span></span>
1. <span data-ttu-id="08f78-145">Выберите **Контроллер входа в систему**, а затем выберите **инспектор подключений**.</span><span class="sxs-lookup"><span data-stu-id="08f78-145">Select the **Sign In View Controller**, then select the **Connections Inspector**.</span></span>
1. <span data-ttu-id="08f78-146">В разделе **активированный сегуес**перетащите незаполненный кружок рядом с пунктом **вручную** на **контроллер панели вкладок** в раскадровке.</span><span class="sxs-lookup"><span data-stu-id="08f78-146">Under **Triggered Segues**, drag the unfilled circle next to **manual** onto the **Tab Bar Controller** on the storyboard.</span></span> <span data-ttu-id="08f78-147">Выберите пункт **Показывать модально** во всплывающем меню.</span><span class="sxs-lookup"><span data-stu-id="08f78-147">Select **Present Modally** in the pop-up menu.</span></span>

    ![Снимок экрана: перетаскивание сегуе вручную на новый контроллер панели вкладок в Xcode](./images/add-segue.png)

1. <span data-ttu-id="08f78-149">Выберите только что добавленные сегуе, а затем выберите **инспектор атрибутов**.</span><span class="sxs-lookup"><span data-stu-id="08f78-149">Select the segue you just added, then select the **Attributes Inspector**.</span></span> <span data-ttu-id="08f78-150">Задайте для \*\*\*\* `userSignedIn`поля идентификатора значение.</span><span class="sxs-lookup"><span data-stu-id="08f78-150">Set the **Identifier** field to `userSignedIn`.</span></span>

    ![Снимок экрана с полем идентификатора в инспекторе атрибутов в Xcode](./images/set-segue-identifier.png)

1. <span data-ttu-id="08f78-152">Выберите **сцену 1**, а затем выберите **инспектор подключений**.</span><span class="sxs-lookup"><span data-stu-id="08f78-152">Select the **Item 1 Scene**, then select the **Connections Inspector**.</span></span>
1. <span data-ttu-id="08f78-153">В разделе **запущенные сегуес**перетащите незаполненный кружок рядом с пунктом " **вручную** " на **Контроллер входа** на экран раскадровки.</span><span class="sxs-lookup"><span data-stu-id="08f78-153">Under **Triggered Segues**, drag the unfilled circle next to **manual** onto the **Sign In View Controller** on the storyboard.</span></span> <span data-ttu-id="08f78-154">Выберите пункт **Показывать модально** во всплывающем меню.</span><span class="sxs-lookup"><span data-stu-id="08f78-154">Select **Present Modally** in the pop-up menu.</span></span>
1. <span data-ttu-id="08f78-155">Выберите только что добавленные сегуе, а затем выберите **инспектор атрибутов**.</span><span class="sxs-lookup"><span data-stu-id="08f78-155">Select the segue you just added, then select the **Attributes Inspector**.</span></span> <span data-ttu-id="08f78-156">Задайте для \*\*\*\* `userSignedOut`поля идентификатора значение.</span><span class="sxs-lookup"><span data-stu-id="08f78-156">Set the **Identifier** field to `userSignedOut`.</span></span>

### <a name="create-welcome-page"></a><span data-ttu-id="08f78-157">Создание страницы приветствия</span><span class="sxs-lookup"><span data-stu-id="08f78-157">Create welcome page</span></span>

1. <span data-ttu-id="08f78-158">Выберите файл **Assets. кскассетс** .</span><span class="sxs-lookup"><span data-stu-id="08f78-158">Select the **Assets.xcassets** file.</span></span>
1. <span data-ttu-id="08f78-159">В меню **Редактор** выберите **Добавить ресурсы**, а затем — **новый набор изображений**.</span><span class="sxs-lookup"><span data-stu-id="08f78-159">On the **Editor** menu, select **Add Assets**, then **New Image Set**.</span></span>
1. <span data-ttu-id="08f78-160">Выберите новый ресурс **изображения** и используйте **инспектор атрибутов** , чтобы присвоить \*\*\*\* ему имя `DefaultUserPhoto`.</span><span class="sxs-lookup"><span data-stu-id="08f78-160">Select the new **Image** asset and use the **Attribute Inspector** to set its **Name** to `DefaultUserPhoto`.</span></span>
1. <span data-ttu-id="08f78-161">Добавьте любое изображение, которое будет использоваться как фотография профиля пользователя по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="08f78-161">Add any image you like to serve as a default user profile photo.</span></span>

    ![Снимок экрана с представлением "ресурс" набора изображений в Xcode](./images/add-default-user-photo.png)

1. <span data-ttu-id="08f78-163">Создайте новый файл **класса Touch Cocoa** в папке **графтуториал** с именем `WelcomeViewController`.</span><span class="sxs-lookup"><span data-stu-id="08f78-163">Create a new **Cocoa Touch Class** file in the **GraphTutorial** folder named `WelcomeViewController`.</span></span> <span data-ttu-id="08f78-164">Выберите **уивиевконтроллер** в **подклассе** поля.</span><span class="sxs-lookup"><span data-stu-id="08f78-164">Choose **UIViewController** in the **Subclass of** field.</span></span>
1. <span data-ttu-id="08f78-165">Откройте **велкомевиевконтроллер. SWIFT** и замените его содержимое приведенным ниже кодом.</span><span class="sxs-lookup"><span data-stu-id="08f78-165">Open **WelcomeViewController.swift** and replace its contents with the following code.</span></span>

    ```Swift
    import UIKit

    class WelcomeViewController: UIViewController {

        @IBOutlet var userProfilePhoto: UIImageView!
        @IBOutlet var userDisplayName: UILabel!
        @IBOutlet var userEmail: UILabel!

        override func viewDidLoad() {
            super.viewDidLoad()

            // Do any additional setup after loading the view.

            // TEMPORARY
            self.userProfilePhoto.image = UIImage(imageLiteralResourceName: "DefaultUserPhoto")
            self.userDisplayName.text = "Default User"
            self.userEmail.text = "default@contoso.com"
        }

        @IBAction func signOut() {
            self.performSegue(withIdentifier: "userSignedOut", sender: nil)
        }
    }
    ```

1. <span data-ttu-id="08f78-166">Откройте **главное. раскадровка**.</span><span class="sxs-lookup"><span data-stu-id="08f78-166">Open **Main.storyboard**.</span></span> <span data-ttu-id="08f78-167">Выберите **сцену 1**, а затем выберите **инспектор удостоверений**.</span><span class="sxs-lookup"><span data-stu-id="08f78-167">Select the **Item 1 Scene**, then select the **Identity Inspector**.</span></span> <span data-ttu-id="08f78-168">Измените значение свойства **Class** на **велкомевиевконтроллер**.</span><span class="sxs-lookup"><span data-stu-id="08f78-168">Change the **Class** value to **WelcomeViewController**.</span></span>
1. <span data-ttu-id="08f78-169">С помощью **библиотеки**добавьте указанные ниже элементы в сцену " **элемент 1**".</span><span class="sxs-lookup"><span data-stu-id="08f78-169">Using the **Library**, add the following items to the **Item 1 Scene**.</span></span>

    - <span data-ttu-id="08f78-170">Одно **представление изображений**</span><span class="sxs-lookup"><span data-stu-id="08f78-170">One **Image View**</span></span>
    - <span data-ttu-id="08f78-171">Две **метки**</span><span class="sxs-lookup"><span data-stu-id="08f78-171">Two **Labels**</span></span>
    - <span data-ttu-id="08f78-172">Одна **кнопка**</span><span class="sxs-lookup"><span data-stu-id="08f78-172">One **Button**</span></span>

1. <span data-ttu-id="08f78-173">Выберите представление изображения, а затем выберите **Инспектор размеров**.</span><span class="sxs-lookup"><span data-stu-id="08f78-173">Select the image view, then select the **Size Inspector**.</span></span>
1. <span data-ttu-id="08f78-174">Задайте **ширину** и **высоту** 196.</span><span class="sxs-lookup"><span data-stu-id="08f78-174">Set the **Width** and **Height** to 196.</span></span>
1. <span data-ttu-id="08f78-175">Выберите вторую метку, а затем щелкните **инспектор атрибутов**.</span><span class="sxs-lookup"><span data-stu-id="08f78-175">Select the second label, then select the **Attributes Inspector**.</span></span>
1. <span data-ttu-id="08f78-176">Измените **Цвет** на **темный серый цвет**и замените **Шрифт** на **системный 12,0**.</span><span class="sxs-lookup"><span data-stu-id="08f78-176">Change the **Color** to **Dark Gray Color**, and change the **Font** to **System 12.0**.</span></span>
1. <span data-ttu-id="08f78-177">Нажмите кнопку, а затем выберите **инспектор атрибутов**.</span><span class="sxs-lookup"><span data-stu-id="08f78-177">Select the button, then select the **Attributes Inspector**.</span></span>
1. <span data-ttu-id="08f78-178">Измените **заголовок** на `Sign Out`.</span><span class="sxs-lookup"><span data-stu-id="08f78-178">Change the **Title** to `Sign Out`.</span></span>
1. <span data-ttu-id="08f78-179">С помощью **инспектора подключений**создайте следующие подключения.</span><span class="sxs-lookup"><span data-stu-id="08f78-179">Using the **Connections Inspector**, make the following connections.</span></span>

    - <span data-ttu-id="08f78-180">Свяжите розетку **userDisplayName** с первой меткой.</span><span class="sxs-lookup"><span data-stu-id="08f78-180">Link the **userDisplayName** outlet to the first label.</span></span>
    - <span data-ttu-id="08f78-181">Свяжите розетку **userEmail** с второй меткой.</span><span class="sxs-lookup"><span data-stu-id="08f78-181">Link the **userEmail** outlet to the second label.</span></span>
    - <span data-ttu-id="08f78-182">Свяжите розетку **усерпрофилефото** с представлением изображения.</span><span class="sxs-lookup"><span data-stu-id="08f78-182">Link the **userProfilePhoto** outlet to the image view.</span></span>
    - <span data-ttu-id="08f78-183">Связывание действия, полученного при **выйдите** , с помощью кнопки **вверх**на кнопке.</span><span class="sxs-lookup"><span data-stu-id="08f78-183">Link the **signOut** received action to the button's **Touch Up Inside**.</span></span>

1. <span data-ttu-id="08f78-184">Выберите элемент Панель вкладок в нижней части сцены, а затем выберите **инспектор атрибутов**.</span><span class="sxs-lookup"><span data-stu-id="08f78-184">Select the tab bar item at the bottom of the scene, then select the **Attributes Inspector**.</span></span> <span data-ttu-id="08f78-185">Измените **заголовок** на `Me`.</span><span class="sxs-lookup"><span data-stu-id="08f78-185">Change the **Title** to `Me`.</span></span>
1. <span data-ttu-id="08f78-186">В меню **Редактор** выберите команду **устранить проблемы с автоматическим макетом**, а затем выберите **добавить недостающие ограничения** **в все представления на контроллере приветствия**.</span><span class="sxs-lookup"><span data-stu-id="08f78-186">On the **Editor** menu, select **Resolve Auto Layout Issues**, then select **Add Missing Constraints** underneath **All Views in Welcome View Controller**.</span></span>

<span data-ttu-id="08f78-187">По завершении настройки сцена приветствия должен выглядеть так, как показано ниже.</span><span class="sxs-lookup"><span data-stu-id="08f78-187">The welcome scene should look similar to this once you're done.</span></span>

![Снимок экрана с макетом "Начальная сцена"](./images/welcome-scene-layout.png)

### <a name="create-calendar-page"></a><span data-ttu-id="08f78-189">Создание страницы календаря</span><span class="sxs-lookup"><span data-stu-id="08f78-189">Create calendar page</span></span>

1. <span data-ttu-id="08f78-190">Создайте новый файл **класса Touch Cocoa** в папке **графтуториал** с именем `CalendarViewController`.</span><span class="sxs-lookup"><span data-stu-id="08f78-190">Create a new **Cocoa Touch Class** file in the **GraphTutorial** folder named `CalendarViewController`.</span></span> <span data-ttu-id="08f78-191">Выберите **уивиевконтроллер** в **подклассе** поля.</span><span class="sxs-lookup"><span data-stu-id="08f78-191">Choose **UIViewController** in the **Subclass of** field.</span></span>
1. <span data-ttu-id="08f78-192">Откройте **календарвиевконтроллер. SWIFT** и замените его содержимое приведенным ниже кодом.</span><span class="sxs-lookup"><span data-stu-id="08f78-192">Open **CalendarViewController.swift** and replace its contents with the following code.</span></span>

    ```Swift
    import UIKit

    class CalendarViewController: UIViewController {

        @IBOutlet var calendarJSON: UITextView!

        override func viewDidLoad() {
            super.viewDidLoad()

            // Do any additional setup after loading the view.

            // TEMPORARY
            calendarJSON.text = "Calendar"
            calendarJSON.sizeToFit()
        }
    }
    ```

1. <span data-ttu-id="08f78-193">Откройте **главное. раскадровка**.</span><span class="sxs-lookup"><span data-stu-id="08f78-193">Open **Main.storyboard**.</span></span> <span data-ttu-id="08f78-194">Выберите **сцену 2 элемента 2**, а затем выберите **инспектор удостоверений**.</span><span class="sxs-lookup"><span data-stu-id="08f78-194">Select the **Item 2 Scene**, then select the **Identity Inspector**.</span></span> <span data-ttu-id="08f78-195">Измените значение свойства **Class** на **календарвиевконтроллер**.</span><span class="sxs-lookup"><span data-stu-id="08f78-195">Change the **Class** value to **CalendarViewController**.</span></span>
1. <span data-ttu-id="08f78-196">С помощью **библиотеки**добавьте **текстовое представление** в **сцену "элемент 2**".</span><span class="sxs-lookup"><span data-stu-id="08f78-196">Using the **Library**, add a **Text View** to the **Item 2 Scene**.</span></span>
1. <span data-ttu-id="08f78-197">Выберите только что добавленное текстовое представление.</span><span class="sxs-lookup"><span data-stu-id="08f78-197">Select the text view you just added.</span></span> <span data-ttu-id="08f78-198">В **редакторе**выберите **внедрить в**, а затем **прокрутить представление**.</span><span class="sxs-lookup"><span data-stu-id="08f78-198">On the **Editor**, choose **Embed In**, then **Scroll View**.</span></span>
1. <span data-ttu-id="08f78-199">С помощью **инспектора подключений**Подключите розетку **календаржсон** к текстовому представлению.</span><span class="sxs-lookup"><span data-stu-id="08f78-199">Using the **Connections Inspector**, connect the **calendarJSON** outlet to the text view.</span></span>
1. 1. <span data-ttu-id="08f78-200">Выберите элемент Панель вкладок в нижней части сцены, а затем выберите **инспектор атрибутов**.</span><span class="sxs-lookup"><span data-stu-id="08f78-200">Select the tab bar item at the bottom of the scene, then select the **Attributes Inspector**.</span></span> <span data-ttu-id="08f78-201">Измените **заголовок** на `Calendar`.</span><span class="sxs-lookup"><span data-stu-id="08f78-201">Change the **Title** to `Calendar`.</span></span>
1. <span data-ttu-id="08f78-202">В меню **Редактор** выберите команду **устранить проблемы с автоматическим макетом**, а затем выберите **добавить недостающие ограничения** **в все представления на контроллере приветствия**.</span><span class="sxs-lookup"><span data-stu-id="08f78-202">On the **Editor** menu, select **Resolve Auto Layout Issues**, then select **Add Missing Constraints** underneath **All Views in Welcome View Controller**.</span></span>

<span data-ttu-id="08f78-203">По завершении настройки сцена должна выглядеть так, как показано ниже.</span><span class="sxs-lookup"><span data-stu-id="08f78-203">The calendar scene should look similar to this once you're done.</span></span>

![Снимок экрана с макетом "сцена" в календаре](./images/calendar-scene-layout.png)

### <a name="create-activity-indicator"></a><span data-ttu-id="08f78-205">Создание индикатора активности</span><span class="sxs-lookup"><span data-stu-id="08f78-205">Create activity indicator</span></span>

1. <span data-ttu-id="08f78-206">Создайте новый файл **класса Touch Cocoa** в папке **графтуториал** с именем `SpinnerViewController`.</span><span class="sxs-lookup"><span data-stu-id="08f78-206">Create a new **Cocoa Touch Class** file in the **GraphTutorial** folder named `SpinnerViewController`.</span></span> <span data-ttu-id="08f78-207">Выберите **уивиевконтроллер** в **подклассе** поля.</span><span class="sxs-lookup"><span data-stu-id="08f78-207">Choose **UIViewController** in the **Subclass of** field.</span></span>
1. <span data-ttu-id="08f78-208">Откройте **спиннервиевконтроллер. SWIFT** и замените его содержимое приведенным ниже кодом.</span><span class="sxs-lookup"><span data-stu-id="08f78-208">Open **SpinnerViewController.swift** and replace its contents with the following code.</span></span>

    ```Swift
    import UIKit

    class SpinnerViewController: UIViewController {

        var spinner = UIActivityIndicatorView(style: .whiteLarge)

        override func loadView() {
            view = UIView()
            view.backgroundColor = UIColor(white: 0, alpha: 0.7)

            spinner.translatesAutoresizingMaskIntoConstraints = false
            spinner.startAnimating()
            view.addSubview(spinner)

            spinner.centerXAnchor.constraint(equalTo: view.centerXAnchor).isActive = true
            spinner.centerYAnchor.constraint(equalTo: view.centerYAnchor).isActive = true
        }

        public func start(container: UIViewController) {
            container.addChild(self)
            self.view.frame = container.view.frame
            container.view.addSubview(self.view)
            self.didMove(toParent: container)
        }

        public func stop() {
            self.willMove(toParent: nil)
            self.view.removeFromSuperview()
            self.removeFromParent()
        }
    }
    ```

## <a name="test-the-app"></a><span data-ttu-id="08f78-209">Тестирование приложения</span><span class="sxs-lookup"><span data-stu-id="08f78-209">Test the app</span></span>

<span data-ttu-id="08f78-210">Сохраните изменения и запустите приложение.</span><span class="sxs-lookup"><span data-stu-id="08f78-210">Save your changes and launch the app.</span></span> <span data-ttu-id="08f78-211">Вы должны иметь возможность перемещаться между экранами с помощью кнопок **входа** и **выхода,** а также с помощью панели вкладок.</span><span class="sxs-lookup"><span data-stu-id="08f78-211">You should be able to move between the screens using the **Sign In** and **Sign Out** buttons and the tab bar.</span></span>

![Снимки экрана приложения](./images/app-screens.png)
