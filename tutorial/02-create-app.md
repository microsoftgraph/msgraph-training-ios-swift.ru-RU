<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="fc9ee-101">Начните с создания нового проекта SWIFT.</span><span class="sxs-lookup"><span data-stu-id="fc9ee-101">Begin by creating a new Swift project.</span></span>

1. <span data-ttu-id="fc9ee-102">Откройте Xcode.</span><span class="sxs-lookup"><span data-stu-id="fc9ee-102">Open Xcode.</span></span> <span data-ttu-id="fc9ee-103">В меню **файл** выберите пункт **создать**, а затем — **проект**.</span><span class="sxs-lookup"><span data-stu-id="fc9ee-103">On the **File** menu, select **New**, then **Project**.</span></span>
1. <span data-ttu-id="fc9ee-104">Выберите шаблон **приложения с одним представлением** и нажмите кнопку **Далее**.</span><span class="sxs-lookup"><span data-stu-id="fc9ee-104">Choose the **Single View App** template and select **Next**.</span></span>

    ![Снимок экрана с диалоговым окном выбора шаблона Xcode](./images/xcode-select-template.png)

1. <span data-ttu-id="fc9ee-106">Задайте для **имени продукта** значение `GraphTutorial` **SWIFT**и **язык** .</span><span class="sxs-lookup"><span data-stu-id="fc9ee-106">Set the **Product Name** to `GraphTutorial` and the **Language** to **Swift**.</span></span>
1. <span data-ttu-id="fc9ee-107">Заполните оставшиеся поля и нажмите кнопку **Далее**.</span><span class="sxs-lookup"><span data-stu-id="fc9ee-107">Fill in the remaining fields and select **Next**.</span></span>
1. <span data-ttu-id="fc9ee-108">Выберите расположение для проекта и нажмите кнопку **создать**.</span><span class="sxs-lookup"><span data-stu-id="fc9ee-108">Choose a location for the project and select **Create**.</span></span>

## <a name="install-dependencies"></a><span data-ttu-id="fc9ee-109">Установка зависимостей</span><span class="sxs-lookup"><span data-stu-id="fc9ee-109">Install dependencies</span></span>

<span data-ttu-id="fc9ee-110">Перед перемещением установите некоторые дополнительные зависимости, которые будут использоваться позже.</span><span class="sxs-lookup"><span data-stu-id="fc9ee-110">Before moving on, install some additional dependencies that you will use later.</span></span>

- <span data-ttu-id="fc9ee-111">[Библиотека проверки подлинности Microsoft (MSAL) для iOS](https://github.com/AzureAD/microsoft-authentication-library-for-objc) для проверки подлинности в Azure AD.</span><span class="sxs-lookup"><span data-stu-id="fc9ee-111">[Microsoft Authentication Library (MSAL) for iOS](https://github.com/AzureAD/microsoft-authentication-library-for-objc) for authenticating to with Azure AD.</span></span>
- <span data-ttu-id="fc9ee-112">[Пакет SDK Microsoft Graph для задания на языке C](https://github.com/microsoftgraph/msgraph-sdk-objc) для совершения вызовов в Microsoft Graph.</span><span class="sxs-lookup"><span data-stu-id="fc9ee-112">[Microsoft Graph SDK for Objective C](https://github.com/microsoftgraph/msgraph-sdk-objc) for making calls to Microsoft Graph.</span></span>
- <span data-ttu-id="fc9ee-113">[Пакет SDK моделей Microsoft Graph для задания с](https://github.com/microsoftgraph/msgraph-sdk-objc-models) строго типизированными объектами, представляющими ресурсы Microsoft Graph, такие как "Пользователи" или "события".</span><span class="sxs-lookup"><span data-stu-id="fc9ee-113">[Microsoft Graph Models SDK for Objective C](https://github.com/microsoftgraph/msgraph-sdk-objc-models) for strongly-typed objects representing Microsoft Graph resources like users or events.</span></span>

1. <span data-ttu-id="fc9ee-114">Выйдите из Xcode.</span><span class="sxs-lookup"><span data-stu-id="fc9ee-114">Quit Xcode.</span></span>
1. <span data-ttu-id="fc9ee-115">Откройте терминал и измените каталог на путь к проекту **графтуториал** .</span><span class="sxs-lookup"><span data-stu-id="fc9ee-115">Open Terminal and change the directory to the location of your **GraphTutorial** project.</span></span>
1. <span data-ttu-id="fc9ee-116">Выполните следующую команду, чтобы создать Podfile.</span><span class="sxs-lookup"><span data-stu-id="fc9ee-116">Run the following command to create a Podfile.</span></span>

    ```Shell
    pod init
    ```

1. <span data-ttu-id="fc9ee-117">Откройте Podfile и добавьте следующие строки сразу после `use_frameworks!` строки.</span><span class="sxs-lookup"><span data-stu-id="fc9ee-117">Open the Podfile and add the following lines just after the `use_frameworks!` line.</span></span>

    ```Ruby
    pod 'MSAL', '~> 1.1.1'
    pod 'MSGraphClientSDK', ' ~> 1.0.0'
    pod 'MSGraphClientModels', '~> 1.3.0'
    ```

1. <span data-ttu-id="fc9ee-118">Сохраните Podfile, а затем выполните следующую команду, чтобы установить зависимости.</span><span class="sxs-lookup"><span data-stu-id="fc9ee-118">Save the Podfile, then run the following command to install the dependencies.</span></span>

    ```Shell
    pod install
    ```

1. <span data-ttu-id="fc9ee-119">После выполнения команды откройте недавно созданный **графтуториал. xcworkspace** в Xcode.</span><span class="sxs-lookup"><span data-stu-id="fc9ee-119">Once the command completes, open the newly created **GraphTutorial.xcworkspace** in Xcode.</span></span>

## <a name="design-the-app"></a><span data-ttu-id="fc9ee-120">Проектирование приложения</span><span class="sxs-lookup"><span data-stu-id="fc9ee-120">Design the app</span></span>

<span data-ttu-id="fc9ee-121">В этом разделе вы создадите представления приложения: страница входа, навигатор панели вкладок, страница приветствия и страница календаря.</span><span class="sxs-lookup"><span data-stu-id="fc9ee-121">In this section you will create the views for the app: a sign in page, a tab bar navigator, a welcome page, and a calendar page.</span></span> <span data-ttu-id="fc9ee-122">Вы также создадите наложение индикатора действий.</span><span class="sxs-lookup"><span data-stu-id="fc9ee-122">You'll also create an activity indicator overlay.</span></span>

### <a name="create-sign-in-page"></a><span data-ttu-id="fc9ee-123">Страница "Создание входа"</span><span class="sxs-lookup"><span data-stu-id="fc9ee-123">Create sign in page</span></span>

1. <span data-ttu-id="fc9ee-124">Разверните папку **графтуториал** в Xcode, а затем выберите **ViewController. SWIFT**.</span><span class="sxs-lookup"><span data-stu-id="fc9ee-124">Expand the **GraphTutorial** folder in Xcode, then select **ViewController.swift**.</span></span>
1. <span data-ttu-id="fc9ee-125">В **инспекторе файлов**измените **имя** файла на `SignInViewController.swift`.</span><span class="sxs-lookup"><span data-stu-id="fc9ee-125">In the **File Inspector**, change the **Name** of the file to `SignInViewController.swift`.</span></span>

    ![Снимок экрана: инспектор файлов](./images/rename-file.png)

1. <span data-ttu-id="fc9ee-127">Откройте **сигнинвиевконтроллер. SWIFT** и замените его содержимое приведенным ниже кодом.</span><span class="sxs-lookup"><span data-stu-id="fc9ee-127">Open **SignInViewController.swift** and replace its contents with the following code.</span></span>

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

1. <span data-ttu-id="fc9ee-128">Откройте файл **Main. Storyboard** .</span><span class="sxs-lookup"><span data-stu-id="fc9ee-128">Open the **Main.storyboard** file.</span></span>
1. <span data-ttu-id="fc9ee-129">Разверните панель **View Controller сцена**и выберите **View Controller (просмотреть контроллер**).</span><span class="sxs-lookup"><span data-stu-id="fc9ee-129">Expand **View Controller Scene**, then select **View Controller**.</span></span>

    ![Снимок экрана Xcode с выбранным контроллером просмотра](./images/storyboard-select-view-controller.png)

1. <span data-ttu-id="fc9ee-131">Выберите **инспектор удостоверений**, а затем измените раскрывающееся меню **Class** на **сигнинвиевконтроллер**.</span><span class="sxs-lookup"><span data-stu-id="fc9ee-131">Select the **Identity Inspector**, then change the **Class** dropdown to **SignInViewController**.</span></span>

    ![Снимок экрана: инспектор удостоверений](./images/change-class.png)

1. <span data-ttu-id="fc9ee-133">Выберите **библиотеку**, а затем перетащите **кнопку** на **Контроллер входа в систему**.</span><span class="sxs-lookup"><span data-stu-id="fc9ee-133">Select the **Library**, then drag a **Button** onto the **Sign In View Controller**.</span></span>

    ![Снимок экрана библиотеки в Xcode](./images/add-button-to-view.png)

1. <span data-ttu-id="fc9ee-135">Когда выбрана кнопка, выберите **инспектор атрибутов** и измените **название** кнопки на `Sign In`.</span><span class="sxs-lookup"><span data-stu-id="fc9ee-135">With the button selected, select the **Attributes Inspector** and change the **Title** of the button to `Sign In`.</span></span>

    ![Снимок экрана: поле Title (название) в инспекторе атрибутов в Xcode](./images/set-button-title.png)

1. <span data-ttu-id="fc9ee-137">Выполнив выбранную кнопку, нажмите кнопку **Выровнять** в нижней части раскадровки.</span><span class="sxs-lookup"><span data-stu-id="fc9ee-137">With the button selected, select the **Align** button at the bottom of the storyboard.</span></span> <span data-ttu-id="fc9ee-138">Выберите **горизонтально в контейнере** и **вертикально в** ограничениях контейнера, оставьте значения 0, а затем выберите **Добавить 2 ограничения**.</span><span class="sxs-lookup"><span data-stu-id="fc9ee-138">Select both the **Horizontally in container** and **Vertically in container** constraints, leave their values as 0, then select **Add 2 constraints**.</span></span>

    ![Снимок экрана с параметрами ограничений выравнивания в Xcode](./images/add-alignment-constraints.png)

1. <span data-ttu-id="fc9ee-140">Выберите **Контроллер входа в систему**, а затем выберите **инспектор подключений**.</span><span class="sxs-lookup"><span data-stu-id="fc9ee-140">Select the **Sign In View Controller**, then select the **Connections Inspector**.</span></span>
1. <span data-ttu-id="fc9ee-141">В разделе **полученное действие**перетащите незаполненный круг рядом с кнопкой **войти** на кнопку.</span><span class="sxs-lookup"><span data-stu-id="fc9ee-141">Under **Received Actions**, drag the unfilled circle next to **signIn** onto the button.</span></span> <span data-ttu-id="fc9ee-142">Во всплывающем меню выберите пункт " **находящиеся вверх** ".</span><span class="sxs-lookup"><span data-stu-id="fc9ee-142">Select **Touch Up Inside** on the pop-up menu.</span></span>

    ![Снимок экрана: перетаскивание действия Signing на кнопку в Xcode](./images/connect-sign-in-button.png)

### <a name="create-tab-bar"></a><span data-ttu-id="fc9ee-144">Создание панели вкладок</span><span class="sxs-lookup"><span data-stu-id="fc9ee-144">Create tab bar</span></span>

1. <span data-ttu-id="fc9ee-145">Выберите **библиотеку**, а затем перетащите **контроллер панели вкладок** на раскадровку.</span><span class="sxs-lookup"><span data-stu-id="fc9ee-145">Select the **Library**, then drag a **Tab Bar Controller** onto the storyboard.</span></span>
1. <span data-ttu-id="fc9ee-146">Выберите **Контроллер входа в систему**, а затем выберите **инспектор подключений**.</span><span class="sxs-lookup"><span data-stu-id="fc9ee-146">Select the **Sign In View Controller**, then select the **Connections Inspector**.</span></span>
1. <span data-ttu-id="fc9ee-147">В разделе **активированный сегуес**перетащите незаполненный кружок рядом с пунктом **вручную** на **контроллер панели вкладок** в раскадровке.</span><span class="sxs-lookup"><span data-stu-id="fc9ee-147">Under **Triggered Segues**, drag the unfilled circle next to **manual** onto the **Tab Bar Controller** on the storyboard.</span></span> <span data-ttu-id="fc9ee-148">Выберите пункт **Показывать модально** во всплывающем меню.</span><span class="sxs-lookup"><span data-stu-id="fc9ee-148">Select **Present Modally** in the pop-up menu.</span></span>

    ![Снимок экрана: перетаскивание сегуе вручную на новый контроллер панели вкладок в Xcode](./images/add-segue.png)

1. <span data-ttu-id="fc9ee-150">Выберите только что добавленные сегуе, а затем выберите **инспектор атрибутов**.</span><span class="sxs-lookup"><span data-stu-id="fc9ee-150">Select the segue you just added, then select the **Attributes Inspector**.</span></span> <span data-ttu-id="fc9ee-151">Задайте для **Identifier** `userSignedIn`поля идентификатора значение, а для параметра **Презентация** — в **полноэкранном режиме**.</span><span class="sxs-lookup"><span data-stu-id="fc9ee-151">Set the **Identifier** field to `userSignedIn`, and set **Presentation** to **Full Screen**.</span></span>

    ![Снимок экрана с полем идентификатора в инспекторе атрибутов в Xcode](./images/set-segue-identifier.png)

1. <span data-ttu-id="fc9ee-153">Выберите **сцену 1**, а затем выберите **инспектор подключений**.</span><span class="sxs-lookup"><span data-stu-id="fc9ee-153">Select the **Item 1 Scene**, then select the **Connections Inspector**.</span></span>
1. <span data-ttu-id="fc9ee-154">В разделе **запущенные сегуес**перетащите незаполненный кружок рядом с пунктом " **вручную** " на **Контроллер входа** на экран раскадровки.</span><span class="sxs-lookup"><span data-stu-id="fc9ee-154">Under **Triggered Segues**, drag the unfilled circle next to **manual** onto the **Sign In View Controller** on the storyboard.</span></span> <span data-ttu-id="fc9ee-155">Выберите пункт **Показывать модально** во всплывающем меню.</span><span class="sxs-lookup"><span data-stu-id="fc9ee-155">Select **Present Modally** in the pop-up menu.</span></span>
1. <span data-ttu-id="fc9ee-156">Выберите только что добавленные сегуе, а затем выберите **инспектор атрибутов**.</span><span class="sxs-lookup"><span data-stu-id="fc9ee-156">Select the segue you just added, then select the **Attributes Inspector**.</span></span> <span data-ttu-id="fc9ee-157">Задайте для **Identifier** `userSignedOut`поля идентификатора значение, а для параметра **Презентация** — в **полноэкранном режиме**.</span><span class="sxs-lookup"><span data-stu-id="fc9ee-157">Set the **Identifier** field to `userSignedOut`, and set **Presentation** to **Full Screen**.</span></span>

### <a name="create-welcome-page"></a><span data-ttu-id="fc9ee-158">Создание страницы приветствия</span><span class="sxs-lookup"><span data-stu-id="fc9ee-158">Create welcome page</span></span>

1. <span data-ttu-id="fc9ee-159">Выберите файл **Assets. кскассетс** .</span><span class="sxs-lookup"><span data-stu-id="fc9ee-159">Select the **Assets.xcassets** file.</span></span>
1. <span data-ttu-id="fc9ee-160">В меню **Редактор** выберите **Добавить ресурсы**, а затем — **новый набор изображений**.</span><span class="sxs-lookup"><span data-stu-id="fc9ee-160">On the **Editor** menu, select **Add Assets**, then **New Image Set**.</span></span>
1. <span data-ttu-id="fc9ee-161">Выберите новый ресурс **изображения** и используйте **инспектор атрибутов** , чтобы присвоить **Name** ему имя `DefaultUserPhoto`.</span><span class="sxs-lookup"><span data-stu-id="fc9ee-161">Select the new **Image** asset and use the **Attribute Inspector** to set its **Name** to `DefaultUserPhoto`.</span></span>
1. <span data-ttu-id="fc9ee-162">Добавьте любое изображение, которое будет использоваться как фотография профиля пользователя по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="fc9ee-162">Add any image you like to serve as a default user profile photo.</span></span>

    ![Снимок экрана с представлением "ресурс" набора изображений в Xcode](./images/add-default-user-photo.png)

1. <span data-ttu-id="fc9ee-164">Создайте новый файл **класса Touch Cocoa** в папке **графтуториал** с именем `WelcomeViewController`.</span><span class="sxs-lookup"><span data-stu-id="fc9ee-164">Create a new **Cocoa Touch Class** file in the **GraphTutorial** folder named `WelcomeViewController`.</span></span> <span data-ttu-id="fc9ee-165">Выберите **уивиевконтроллер** в **подклассе** поля.</span><span class="sxs-lookup"><span data-stu-id="fc9ee-165">Choose **UIViewController** in the **Subclass of** field.</span></span>
1. <span data-ttu-id="fc9ee-166">Откройте **велкомевиевконтроллер. SWIFT** и замените его содержимое приведенным ниже кодом.</span><span class="sxs-lookup"><span data-stu-id="fc9ee-166">Open **WelcomeViewController.swift** and replace its contents with the following code.</span></span>

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

1. <span data-ttu-id="fc9ee-167">Откройте **главное. раскадровка**.</span><span class="sxs-lookup"><span data-stu-id="fc9ee-167">Open **Main.storyboard**.</span></span> <span data-ttu-id="fc9ee-168">Выберите **сцену 1**, а затем выберите **инспектор удостоверений**.</span><span class="sxs-lookup"><span data-stu-id="fc9ee-168">Select the **Item 1 Scene**, then select the **Identity Inspector**.</span></span> <span data-ttu-id="fc9ee-169">Измените значение свойства **Class** на **велкомевиевконтроллер**.</span><span class="sxs-lookup"><span data-stu-id="fc9ee-169">Change the **Class** value to **WelcomeViewController**.</span></span>
1. <span data-ttu-id="fc9ee-170">С помощью **библиотеки**добавьте указанные ниже элементы в сцену " **элемент 1**".</span><span class="sxs-lookup"><span data-stu-id="fc9ee-170">Using the **Library**, add the following items to the **Item 1 Scene**.</span></span>

    - <span data-ttu-id="fc9ee-171">Одно **представление изображений**</span><span class="sxs-lookup"><span data-stu-id="fc9ee-171">One **Image View**</span></span>
    - <span data-ttu-id="fc9ee-172">Две **метки**</span><span class="sxs-lookup"><span data-stu-id="fc9ee-172">Two **Labels**</span></span>
    - <span data-ttu-id="fc9ee-173">Одна **кнопка**</span><span class="sxs-lookup"><span data-stu-id="fc9ee-173">One **Button**</span></span>
1. <span data-ttu-id="fc9ee-174">С помощью **инспектора подключений**создайте следующие подключения.</span><span class="sxs-lookup"><span data-stu-id="fc9ee-174">Using the **Connections Inspector**, make the following connections.</span></span>

    - <span data-ttu-id="fc9ee-175">Свяжите розетку **userDisplayName** с первой меткой.</span><span class="sxs-lookup"><span data-stu-id="fc9ee-175">Link the **userDisplayName** outlet to the first label.</span></span>
    - <span data-ttu-id="fc9ee-176">Свяжите розетку **userEmail** с второй меткой.</span><span class="sxs-lookup"><span data-stu-id="fc9ee-176">Link the **userEmail** outlet to the second label.</span></span>
    - <span data-ttu-id="fc9ee-177">Свяжите розетку **усерпрофилефото** с представлением изображения.</span><span class="sxs-lookup"><span data-stu-id="fc9ee-177">Link the **userProfilePhoto** outlet to the image view.</span></span>
    - <span data-ttu-id="fc9ee-178">Связывание действия, полученного при **выйдите** , с помощью кнопки **вверх**на кнопке.</span><span class="sxs-lookup"><span data-stu-id="fc9ee-178">Link the **signOut** received action to the button's **Touch Up Inside**.</span></span>

1. <span data-ttu-id="fc9ee-179">Выберите представление изображения, а затем выберите **Инспектор размеров**.</span><span class="sxs-lookup"><span data-stu-id="fc9ee-179">Select the image view, then select the **Size Inspector**.</span></span>
1. <span data-ttu-id="fc9ee-180">Задайте **ширину** и **высоту** 196.</span><span class="sxs-lookup"><span data-stu-id="fc9ee-180">Set the **Width** and **Height** to 196.</span></span>
1. <span data-ttu-id="fc9ee-181">Используйте кнопку **Выровнять** , чтобы добавить **горизонтальное значение в** поле ограничение контейнера со значением 0.</span><span class="sxs-lookup"><span data-stu-id="fc9ee-181">Use the **Align** button to add the **Horizontally in container** constraint with a value of 0.</span></span>
1. <span data-ttu-id="fc9ee-182">Используйте кнопку **Добавить новые ограничения** (рядом с кнопкой **Выровнять** ), чтобы добавить следующие ограничения:</span><span class="sxs-lookup"><span data-stu-id="fc9ee-182">Use the **Add New Constraints** button (next to the **Align** button) to add the following constraints:</span></span>

    - <span data-ttu-id="fc9ee-183">Выровнять по верхнему краю: Safe область, значение: 0</span><span class="sxs-lookup"><span data-stu-id="fc9ee-183">Align Top to: Safe Area, value: 0</span></span>
    - <span data-ttu-id="fc9ee-184">Нижняя область: отображаемое имя пользователя, значение: Standard</span><span class="sxs-lookup"><span data-stu-id="fc9ee-184">Bottom Space to: User Display Name, value: Standard</span></span>
    - <span data-ttu-id="fc9ee-185">Высота, значение: 196</span><span class="sxs-lookup"><span data-stu-id="fc9ee-185">Height, value: 196</span></span>
    - <span data-ttu-id="fc9ee-186">Ширина, значение: 196</span><span class="sxs-lookup"><span data-stu-id="fc9ee-186">Width, value: 196</span></span>

    ![Снимок экрана с новыми параметрами ограничений в Xcode](./images/add-new-constraints.png)

1. <span data-ttu-id="fc9ee-188">Выберите первую метку, а затем с помощью кнопки **Выровнять** добавьте **горизонтальную часть в поле ограничение контейнера** со значением 0.</span><span class="sxs-lookup"><span data-stu-id="fc9ee-188">Select the first label, then use the **Align** button to add the **Horizontally in container** constraint with a value of 0.</span></span>
1. <span data-ttu-id="fc9ee-189">Используйте кнопку **Добавить новые ограничения** , чтобы добавить следующие ограничения:</span><span class="sxs-lookup"><span data-stu-id="fc9ee-189">Use the **Add New Constraints** button to add the following constraints:</span></span>

    - <span data-ttu-id="fc9ee-190">Верхняя область: фотография профиля пользователя, значение: Standard</span><span class="sxs-lookup"><span data-stu-id="fc9ee-190">Top Space to: User Profile Photo, value: Standard</span></span>
    - <span data-ttu-id="fc9ee-191">Нижнее пространство для: адрес электронной почты пользователя, значение: Standard</span><span class="sxs-lookup"><span data-stu-id="fc9ee-191">Bottom Space to: User Email, value: Standard</span></span>

1. <span data-ttu-id="fc9ee-192">Выберите вторую метку, а затем щелкните **инспектор атрибутов**.</span><span class="sxs-lookup"><span data-stu-id="fc9ee-192">Select the second label, then select the **Attributes Inspector**.</span></span>
1. <span data-ttu-id="fc9ee-193">Измените **Цвет** на **темный серый цвет**и замените **Шрифт** на **системный 12,0**.</span><span class="sxs-lookup"><span data-stu-id="fc9ee-193">Change the **Color** to **Dark Gray Color**, and change the **Font** to **System 12.0**.</span></span>
1. <span data-ttu-id="fc9ee-194">Используйте кнопку **Выровнять** , чтобы добавить **горизонтальное значение в** поле ограничение контейнера со значением 0.</span><span class="sxs-lookup"><span data-stu-id="fc9ee-194">Use the **Align** button to add the **Horizontally in container** constraint with a value of 0.</span></span>
1. <span data-ttu-id="fc9ee-195">Используйте кнопку **Добавить новые ограничения** , чтобы добавить следующие ограничения:</span><span class="sxs-lookup"><span data-stu-id="fc9ee-195">Use the **Add New Constraints** button to add the following constraints:</span></span>

    - <span data-ttu-id="fc9ee-196">Верхняя область: отображаемое имя пользователя, значение: Standard</span><span class="sxs-lookup"><span data-stu-id="fc9ee-196">Top Space to: User Display Name, value: Standard</span></span>
    - <span data-ttu-id="fc9ee-197">Нижняя область: выход, значение: 14</span><span class="sxs-lookup"><span data-stu-id="fc9ee-197">Bottom Space to: Sign Out, value: 14</span></span>

1. <span data-ttu-id="fc9ee-198">Нажмите кнопку, а затем выберите **инспектор атрибутов**.</span><span class="sxs-lookup"><span data-stu-id="fc9ee-198">Select the button, then select the **Attributes Inspector**.</span></span>
1. <span data-ttu-id="fc9ee-199">Измените **заголовок** на `Sign Out`.</span><span class="sxs-lookup"><span data-stu-id="fc9ee-199">Change the **Title** to `Sign Out`.</span></span>
1. <span data-ttu-id="fc9ee-200">Используйте кнопку **Выровнять** , чтобы добавить **горизонтальное значение в** поле ограничение контейнера со значением 0.</span><span class="sxs-lookup"><span data-stu-id="fc9ee-200">Use the **Align** button to add the **Horizontally in container** constraint with a value of 0.</span></span>
1. <span data-ttu-id="fc9ee-201">Используйте кнопку **Добавить новые ограничения** , чтобы добавить следующие ограничения:</span><span class="sxs-lookup"><span data-stu-id="fc9ee-201">Use the **Add New Constraints** button to add the following constraints:</span></span>

    - <span data-ttu-id="fc9ee-202">Верхняя область: адрес электронной почты пользователя, значение: 14</span><span class="sxs-lookup"><span data-stu-id="fc9ee-202">Top Space to: User Email, value: 14</span></span>

1. <span data-ttu-id="fc9ee-203">Выберите элемент Панель вкладок в нижней части сцены, а затем выберите **инспектор атрибутов**.</span><span class="sxs-lookup"><span data-stu-id="fc9ee-203">Select the tab bar item at the bottom of the scene, then select the **Attributes Inspector**.</span></span> <span data-ttu-id="fc9ee-204">Измените **заголовок** на `Me`.</span><span class="sxs-lookup"><span data-stu-id="fc9ee-204">Change the **Title** to `Me`.</span></span>

<span data-ttu-id="fc9ee-205">По завершении настройки сцена приветствия должен выглядеть так, как показано ниже.</span><span class="sxs-lookup"><span data-stu-id="fc9ee-205">The welcome scene should look similar to this once you're done.</span></span>

![Снимок экрана с макетом "Начальная сцена"](./images/welcome-scene-layout.png)

### <a name="create-calendar-page"></a><span data-ttu-id="fc9ee-207">Создание страницы календаря</span><span class="sxs-lookup"><span data-stu-id="fc9ee-207">Create calendar page</span></span>

1. <span data-ttu-id="fc9ee-208">Создайте новый файл **класса Touch Cocoa** в папке **графтуториал** с именем `CalendarViewController`.</span><span class="sxs-lookup"><span data-stu-id="fc9ee-208">Create a new **Cocoa Touch Class** file in the **GraphTutorial** folder named `CalendarViewController`.</span></span> <span data-ttu-id="fc9ee-209">Выберите **уивиевконтроллер** в **подклассе** поля.</span><span class="sxs-lookup"><span data-stu-id="fc9ee-209">Choose **UIViewController** in the **Subclass of** field.</span></span>
1. <span data-ttu-id="fc9ee-210">Откройте **календарвиевконтроллер. SWIFT** и замените его содержимое приведенным ниже кодом.</span><span class="sxs-lookup"><span data-stu-id="fc9ee-210">Open **CalendarViewController.swift** and replace its contents with the following code.</span></span>

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

1. <span data-ttu-id="fc9ee-211">Откройте **главное. раскадровка**.</span><span class="sxs-lookup"><span data-stu-id="fc9ee-211">Open **Main.storyboard**.</span></span> <span data-ttu-id="fc9ee-212">Выберите **сцену 2 элемента 2**, а затем выберите **инспектор удостоверений**.</span><span class="sxs-lookup"><span data-stu-id="fc9ee-212">Select the **Item 2 Scene**, then select the **Identity Inspector**.</span></span> <span data-ttu-id="fc9ee-213">Измените значение свойства **Class** на **календарвиевконтроллер**.</span><span class="sxs-lookup"><span data-stu-id="fc9ee-213">Change the **Class** value to **CalendarViewController**.</span></span>
1. <span data-ttu-id="fc9ee-214">С помощью **библиотеки**добавьте **текстовое представление** в **сцену "элемент 2**".</span><span class="sxs-lookup"><span data-stu-id="fc9ee-214">Using the **Library**, add a **Text View** to the **Item 2 Scene**.</span></span>
1. <span data-ttu-id="fc9ee-215">Выберите только что добавленное текстовое представление.</span><span class="sxs-lookup"><span data-stu-id="fc9ee-215">Select the text view you just added.</span></span> <span data-ttu-id="fc9ee-216">В меню **Редактор** выберите **внедрить в**, а затем **прокрутить представление**.</span><span class="sxs-lookup"><span data-stu-id="fc9ee-216">On the **Editor** menu, choose **Embed In**, then **Scroll View**.</span></span>
1. <span data-ttu-id="fc9ee-217">С помощью **инспектора подключений**Подключите розетку **календаржсон** к текстовому представлению.</span><span class="sxs-lookup"><span data-stu-id="fc9ee-217">Using the **Connections Inspector**, connect the **calendarJSON** outlet to the text view.</span></span>
1. <span data-ttu-id="fc9ee-218">Выберите элемент Панель вкладок в нижней части сцены, а затем выберите **инспектор атрибутов**.</span><span class="sxs-lookup"><span data-stu-id="fc9ee-218">Select the tab bar item at the bottom of the scene, then select the **Attributes Inspector**.</span></span> <span data-ttu-id="fc9ee-219">Измените **заголовок** на `Calendar`.</span><span class="sxs-lookup"><span data-stu-id="fc9ee-219">Change the **Title** to `Calendar`.</span></span>
1. <span data-ttu-id="fc9ee-220">В меню **Редактор** выберите команду **устранить проблемы с автоматическим макетом**, а затем выберите **добавить недостающие ограничения** **в все представления на контроллере приветствия**.</span><span class="sxs-lookup"><span data-stu-id="fc9ee-220">On the **Editor** menu, select **Resolve Auto Layout Issues**, then select **Add Missing Constraints** underneath **All Views in Welcome View Controller**.</span></span>

<span data-ttu-id="fc9ee-221">По завершении настройки сцена должна выглядеть так, как показано ниже.</span><span class="sxs-lookup"><span data-stu-id="fc9ee-221">The calendar scene should look similar to this once you're done.</span></span>

![Снимок экрана с макетом "сцена" в календаре](./images/calendar-scene-layout.png)

### <a name="create-activity-indicator"></a><span data-ttu-id="fc9ee-223">Создание индикатора активности</span><span class="sxs-lookup"><span data-stu-id="fc9ee-223">Create activity indicator</span></span>

1. <span data-ttu-id="fc9ee-224">Создайте новый файл **класса Touch Cocoa** в папке **графтуториал** с именем `SpinnerViewController`.</span><span class="sxs-lookup"><span data-stu-id="fc9ee-224">Create a new **Cocoa Touch Class** file in the **GraphTutorial** folder named `SpinnerViewController`.</span></span> <span data-ttu-id="fc9ee-225">Выберите **уивиевконтроллер** в **подклассе** поля.</span><span class="sxs-lookup"><span data-stu-id="fc9ee-225">Choose **UIViewController** in the **Subclass of** field.</span></span>
1. <span data-ttu-id="fc9ee-226">Откройте **спиннервиевконтроллер. SWIFT** и замените его содержимое приведенным ниже кодом.</span><span class="sxs-lookup"><span data-stu-id="fc9ee-226">Open **SpinnerViewController.swift** and replace its contents with the following code.</span></span>

    :::code language="swift" source="../demo/GraphTutorial/GraphTutorial/SpinnerViewController.swift" id="SpinnerSnippet":::

## <a name="test-the-app"></a><span data-ttu-id="fc9ee-227">Тестирование приложения</span><span class="sxs-lookup"><span data-stu-id="fc9ee-227">Test the app</span></span>

<span data-ttu-id="fc9ee-228">Сохраните изменения и запустите приложение.</span><span class="sxs-lookup"><span data-stu-id="fc9ee-228">Save your changes and launch the app.</span></span> <span data-ttu-id="fc9ee-229">Вы должны иметь возможность перемещаться между экранами с помощью кнопок **входа** и **выхода,** а также с помощью панели вкладок.</span><span class="sxs-lookup"><span data-stu-id="fc9ee-229">You should be able to move between the screens using the **Sign In** and **Sign Out** buttons and the tab bar.</span></span>

![Снимки экрана приложения](./images/app-screens.png)
