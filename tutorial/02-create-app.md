<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="0a44a-101">Начните с создания нового проекта Swift.</span><span class="sxs-lookup"><span data-stu-id="0a44a-101">Begin by creating a new Swift project.</span></span>

1. <span data-ttu-id="0a44a-102">Откройте Xcode.</span><span class="sxs-lookup"><span data-stu-id="0a44a-102">Open Xcode.</span></span> <span data-ttu-id="0a44a-103">В меню **"Файл"** выберите **"Новый"** и **"Проект".**</span><span class="sxs-lookup"><span data-stu-id="0a44a-103">On the **File** menu, select **New**, then **Project**.</span></span>
1. <span data-ttu-id="0a44a-104">Выберите шаблон **приложения и** выберите **"Далее".**</span><span class="sxs-lookup"><span data-stu-id="0a44a-104">Choose the **App** template and select **Next**.</span></span>

    ![Снимок экрана: диалоговое окно выбора шаблона Xcode](images/xcode-select-template.png)

1. <span data-ttu-id="0a44a-106">Установите для **имени продукта** и `GraphTutorial` для **языка** swift . </span><span class="sxs-lookup"><span data-stu-id="0a44a-106">Set the **Product Name** to `GraphTutorial` and the **Language** to **Swift**.</span></span>
1. <span data-ttu-id="0a44a-107">Заполните оставшиеся поля и выберите **"Далее".**</span><span class="sxs-lookup"><span data-stu-id="0a44a-107">Fill in the remaining fields and select **Next**.</span></span>
1. <span data-ttu-id="0a44a-108">Выберите расположение для проекта и выберите **"Создать".**</span><span class="sxs-lookup"><span data-stu-id="0a44a-108">Choose a location for the project and select **Create**.</span></span>

## <a name="install-dependencies"></a><span data-ttu-id="0a44a-109">Установка зависимостей</span><span class="sxs-lookup"><span data-stu-id="0a44a-109">Install dependencies</span></span>

<span data-ttu-id="0a44a-110">Прежде чем двигаться дальше, установите некоторые дополнительные зависимости, которые вы будете использовать позже.</span><span class="sxs-lookup"><span data-stu-id="0a44a-110">Before moving on, install some additional dependencies that you will use later.</span></span>

- <span data-ttu-id="0a44a-111">[Microsoft Authentication Library (MSAL) для iOS](https://github.com/AzureAD/microsoft-authentication-library-for-objc) для проверки подлинности в Azure AD.</span><span class="sxs-lookup"><span data-stu-id="0a44a-111">[Microsoft Authentication Library (MSAL) for iOS](https://github.com/AzureAD/microsoft-authentication-library-for-objc) for authenticating to with Azure AD.</span></span>
- <span data-ttu-id="0a44a-112">[Microsoft Graph SDK для Objective C](https://github.com/microsoftgraph/msgraph-sdk-objc) для вызовов Microsoft Graph.</span><span class="sxs-lookup"><span data-stu-id="0a44a-112">[Microsoft Graph SDK for Objective C](https://github.com/microsoftgraph/msgraph-sdk-objc) for making calls to Microsoft Graph.</span></span>
- <span data-ttu-id="0a44a-113">[Microsoft Graph Models SDK for Objective C](https://github.com/microsoftgraph/msgraph-sdk-objc-models) for strongly-typed objects representing Microsoft Graph resources like users or events.</span><span class="sxs-lookup"><span data-stu-id="0a44a-113">[Microsoft Graph Models SDK for Objective C](https://github.com/microsoftgraph/msgraph-sdk-objc-models) for strongly-typed objects representing Microsoft Graph resources like users or events.</span></span>

1. <span data-ttu-id="0a44a-114">Выйти из Xcode.</span><span class="sxs-lookup"><span data-stu-id="0a44a-114">Quit Xcode.</span></span>
1. <span data-ttu-id="0a44a-115">Откройте терминал и измените каталог на расположение проекта **GraphTutorial.**</span><span class="sxs-lookup"><span data-stu-id="0a44a-115">Open Terminal and change the directory to the location of your **GraphTutorial** project.</span></span>
1. <span data-ttu-id="0a44a-116">Чтобы создать Podfile, запустите следующую команду.</span><span class="sxs-lookup"><span data-stu-id="0a44a-116">Run the following command to create a Podfile.</span></span>

    ```Shell
    pod init
    ```

1. <span data-ttu-id="0a44a-117">Откройте Podfile и добавьте следующие строки сразу после `use_frameworks!` строки.</span><span class="sxs-lookup"><span data-stu-id="0a44a-117">Open the Podfile and add the following lines just after the `use_frameworks!` line.</span></span>

    ```Ruby
    pod 'MSAL', '~> 1.1.13'
    pod 'MSGraphClientSDK', ' ~> 1.0.0'
    pod 'MSGraphClientModels', '~> 1.3.0'
    ```

1. <span data-ttu-id="0a44a-118">Сохраните Podfile и запустите следующую команду, чтобы установить зависимости.</span><span class="sxs-lookup"><span data-stu-id="0a44a-118">Save the Podfile, then run the following command to install the dependencies.</span></span>

    ```Shell
    pod install
    ```

1. <span data-ttu-id="0a44a-119">После выполнения команды откройте созданное **пространство GraphTutorial.xcworkspace** в Xcode.</span><span class="sxs-lookup"><span data-stu-id="0a44a-119">Once the command completes, open the newly created **GraphTutorial.xcworkspace** in Xcode.</span></span>

## <a name="design-the-app"></a><span data-ttu-id="0a44a-120">Проектирование приложения</span><span class="sxs-lookup"><span data-stu-id="0a44a-120">Design the app</span></span>

<span data-ttu-id="0a44a-121">В этом разделе вы создадим представления для приложения: страницу для входов, навигатор панели вкладок, страницу приветствия и страницу календаря.</span><span class="sxs-lookup"><span data-stu-id="0a44a-121">In this section you will create the views for the app: a sign in page, a tab bar navigator, a welcome page, and a calendar page.</span></span> <span data-ttu-id="0a44a-122">Вы также создадим наложение индикатора активности.</span><span class="sxs-lookup"><span data-stu-id="0a44a-122">You'll also create an activity indicator overlay.</span></span>

### <a name="create-sign-in-page"></a><span data-ttu-id="0a44a-123">Страница "Создание страницы для входов"</span><span class="sxs-lookup"><span data-stu-id="0a44a-123">Create sign in page</span></span>

1. <span data-ttu-id="0a44a-124">**Развертка папки GraphTutorial** в Xcode и выберите **ViewController.swift.**</span><span class="sxs-lookup"><span data-stu-id="0a44a-124">Expand the **GraphTutorial** folder in Xcode, then select **ViewController.swift**.</span></span>
1. <span data-ttu-id="0a44a-125">В **инспекторе файлов** **измените** имя файла на `SignInViewController.swift` .</span><span class="sxs-lookup"><span data-stu-id="0a44a-125">In the **File Inspector**, change the **Name** of the file to `SignInViewController.swift`.</span></span>

    ![Снимок экрана: инспектор файлов](images/rename-file.png)

1. <span data-ttu-id="0a44a-127">Откройте **SignInViewController.swift** и замените его содержимое следующим кодом.</span><span class="sxs-lookup"><span data-stu-id="0a44a-127">Open **SignInViewController.swift** and replace its contents with the following code.</span></span>

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

1. <span data-ttu-id="0a44a-128">Откройте **main.storyboard.**</span><span class="sxs-lookup"><span data-stu-id="0a44a-128">Open **Main.storyboard**.</span></span> <span data-ttu-id="0a44a-129">Раз **развернуть сцену контроллера представления,** а затем выберите **контроллер представления.**</span><span class="sxs-lookup"><span data-stu-id="0a44a-129">Expand **View Controller Scene**, then select **View Controller**.</span></span>

    ![Снимок экрана: Xcode с выбранным контроллером просмотра](images/storyboard-select-view-controller.png)

1. <span data-ttu-id="0a44a-131">Выберите identity **Inspector,** а затем измените **выпадающий** класс на **SignInViewController.**</span><span class="sxs-lookup"><span data-stu-id="0a44a-131">Select the **Identity Inspector**, then change the **Class** dropdown to **SignInViewController**.</span></span>

    ![Снимок экрана инспектора удостоверений](images/change-class.png)

1. <span data-ttu-id="0a44a-133">Выберите **библиотеку,** а затем перетащите **кнопку** на контроллер **просмотра для входов.**</span><span class="sxs-lookup"><span data-stu-id="0a44a-133">Select the **Library**, then drag a **Button** onto the **Sign In View Controller**.</span></span>

    ![Снимок экрана: библиотека в Xcode](images/add-button-to-view.png)

1. <span data-ttu-id="0a44a-135">После нажатия кнопки выберите **"Attributes Inspector"** (Инспектор атрибутов) и измените **заголовок** кнопки на `Sign In` .</span><span class="sxs-lookup"><span data-stu-id="0a44a-135">With the button selected, select the **Attributes Inspector** and change the **Title** of the button to `Sign In`.</span></span>

    ![Снимок экрана с полем "Заголовок" в инспекторе атрибутов в Xcode](images/set-button-title.png)

1. <span data-ttu-id="0a44a-137">Нажатие кнопки "Выровнять" в нижней части storyboard. </span><span class="sxs-lookup"><span data-stu-id="0a44a-137">With the button selected, select the **Align** button at the bottom of the storyboard.</span></span> <span data-ttu-id="0a44a-138">Выберите и **горизонтально** в  контейнере, и по вертикали в ограничениях контейнера, оставьте их значения 0, а затем выберите **"Добавить 2 ограничения".**</span><span class="sxs-lookup"><span data-stu-id="0a44a-138">Select both the **Horizontally in container** and **Vertically in container** constraints, leave their values as 0, then select **Add 2 constraints**.</span></span>

    ![Снимок экрана с настройками ограничений выравнивания в Xcode](images/add-alignment-constraints.png)

1. <span data-ttu-id="0a44a-140">Выберите контроллер **просмотра для входов,** а затем **выберите инспектор подключений.**</span><span class="sxs-lookup"><span data-stu-id="0a44a-140">Select the **Sign In View Controller**, then select the **Connections Inspector**.</span></span>
1. <span data-ttu-id="0a44a-141">В **области "Received Actions"** перетащите незаполненный круг рядом с **кнопкой signIn.**</span><span class="sxs-lookup"><span data-stu-id="0a44a-141">Under **Received Actions**, drag the unfilled circle next to **signIn** onto the button.</span></span> <span data-ttu-id="0a44a-142">Выберите **"Сенсорный экран" во** всплывающее меню.</span><span class="sxs-lookup"><span data-stu-id="0a44a-142">Select **Touch Up Inside** on the pop-up menu.</span></span>

    ![Снимок экрана: перетаскивание действия signIn на кнопку в Xcode](images/connect-sign-in-button.png)

### <a name="create-tab-bar"></a><span data-ttu-id="0a44a-144">Создание панели вкладок</span><span class="sxs-lookup"><span data-stu-id="0a44a-144">Create tab bar</span></span>

1. <span data-ttu-id="0a44a-145">Выберите **библиотеку,** а затем перетащите контроллер **панели вкладок** в storyboard.</span><span class="sxs-lookup"><span data-stu-id="0a44a-145">Select the **Library**, then drag a **Tab Bar Controller** onto the storyboard.</span></span>
1. <span data-ttu-id="0a44a-146">Выберите контроллер **просмотра для входов,** а затем **выберите инспектор подключений.**</span><span class="sxs-lookup"><span data-stu-id="0a44a-146">Select the **Sign In View Controller**, then select the **Connections Inspector**.</span></span>
1. <span data-ttu-id="0a44a-147">В **области "Триггеры"** перетащите незаполненный круг рядом с вручную на контроллер панели вкладок **в** storyboard. </span><span class="sxs-lookup"><span data-stu-id="0a44a-147">Under **Triggered Segues**, drag the unfilled circle next to **manual** onto the **Tab Bar Controller** on the storyboard.</span></span> <span data-ttu-id="0a44a-148">Выберите **"Представить" модально** во всплывающее меню.</span><span class="sxs-lookup"><span data-stu-id="0a44a-148">Select **Present Modally** in the pop-up menu.</span></span>

    ![Снимок экрана: перетаскивание вручную в новый контроллер панели вкладок в Xcode](images/add-segue.png)

1. <span data-ttu-id="0a44a-150">Выберите только что добавленную сегу, а затем — **инспектор атрибутов.**</span><span class="sxs-lookup"><span data-stu-id="0a44a-150">Select the segue you just added, then select the **Attributes Inspector**.</span></span> <span data-ttu-id="0a44a-151">Установите для **поля "Идентификатор"** и `userSignedIn` **"Презентация"** полноэкранный **режим.**</span><span class="sxs-lookup"><span data-stu-id="0a44a-151">Set the **Identifier** field to `userSignedIn`, and set **Presentation** to **Full Screen**.</span></span>

    ![Снимок экрана с полем идентификатора в инспекторе атрибутов в Xcode](images/set-segue-identifier.png)

1. <span data-ttu-id="0a44a-153">Выберите сцену **"Элемент 1"** и выберите **"Connections Inspector" (Инспектор подключений).**</span><span class="sxs-lookup"><span data-stu-id="0a44a-153">Select the **Item 1 Scene**, then select the **Connections Inspector**.</span></span>
1. <span data-ttu-id="0a44a-154">В **области "Триггеры"** перетащите незаполненный круг рядом с вручную на контроллер просмотра для входов **в** storyboard. </span><span class="sxs-lookup"><span data-stu-id="0a44a-154">Under **Triggered Segues**, drag the unfilled circle next to **manual** onto the **Sign In View Controller** on the storyboard.</span></span> <span data-ttu-id="0a44a-155">Выберите **"Представить" модально** во всплывающее меню.</span><span class="sxs-lookup"><span data-stu-id="0a44a-155">Select **Present Modally** in the pop-up menu.</span></span>
1. <span data-ttu-id="0a44a-156">Выберите только что добавленную сегу, а затем — **инспектор атрибутов.**</span><span class="sxs-lookup"><span data-stu-id="0a44a-156">Select the segue you just added, then select the **Attributes Inspector**.</span></span> <span data-ttu-id="0a44a-157">Установите для **поля "Идентификатор"** и `userSignedOut` **"Презентация"** полноэкранный **режим.**</span><span class="sxs-lookup"><span data-stu-id="0a44a-157">Set the **Identifier** field to `userSignedOut`, and set **Presentation** to **Full Screen**.</span></span>

### <a name="create-welcome-page"></a><span data-ttu-id="0a44a-158">Создание страницы приветствия</span><span class="sxs-lookup"><span data-stu-id="0a44a-158">Create welcome page</span></span>

1. <span data-ttu-id="0a44a-159">Выберите файл **Assets.xcassets.**</span><span class="sxs-lookup"><span data-stu-id="0a44a-159">Select the **Assets.xcassets** file.</span></span>
1. <span data-ttu-id="0a44a-160">В меню **"Редактор"** выберите **"Добавить новый актив"** и **"Набор изображений".**</span><span class="sxs-lookup"><span data-stu-id="0a44a-160">On the **Editor** menu, select **Add New Asset**, then **Image Set**.</span></span>
1. <span data-ttu-id="0a44a-161">Выберите новый ресурс **Image** и используйте **attribute Inspector,** чтобы установить его **имя.** `DefaultUserPhoto`</span><span class="sxs-lookup"><span data-stu-id="0a44a-161">Select the new **Image** asset and use the **Attribute Inspector** to set its **Name** to `DefaultUserPhoto`.</span></span>
1. <span data-ttu-id="0a44a-162">Добавьте любое изображение, которое вы хотите выступать в качестве фотографии профиля пользователя по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="0a44a-162">Add any image you like to serve as a default user profile photo.</span></span>

    ![Снимок экрана: представление ресурсов "Набор изображений" в Xcode](images/add-default-user-photo.png)

1. <span data-ttu-id="0a44a-164">Создайте файл **класса Cocoa Touch в** папке **GraphTutorial** с именем `WelcomeViewController` .</span><span class="sxs-lookup"><span data-stu-id="0a44a-164">Create a new **Cocoa Touch Class** file in the **GraphTutorial** folder named `WelcomeViewController`.</span></span> <span data-ttu-id="0a44a-165">Выберите **UIViewController** в **подклассе** поля.</span><span class="sxs-lookup"><span data-stu-id="0a44a-165">Choose **UIViewController** in the **Subclass of** field.</span></span>
1. <span data-ttu-id="0a44a-166">Откройте **WelcomeViewController.swift** и замените его содержимое следующим кодом.</span><span class="sxs-lookup"><span data-stu-id="0a44a-166">Open **WelcomeViewController.swift** and replace its contents with the following code.</span></span>

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

1. <span data-ttu-id="0a44a-167">Откройте **main.storyboard.**</span><span class="sxs-lookup"><span data-stu-id="0a44a-167">Open **Main.storyboard**.</span></span> <span data-ttu-id="0a44a-168">Выберите сцену **"Элемент 1"** и выберите инспектор **удостоверений.**</span><span class="sxs-lookup"><span data-stu-id="0a44a-168">Select the **Item 1 Scene**, then select the **Identity Inspector**.</span></span> <span data-ttu-id="0a44a-169">Измените **значение класса** на **WelcomeViewController.**</span><span class="sxs-lookup"><span data-stu-id="0a44a-169">Change the **Class** value to **WelcomeViewController**.</span></span>
1. <span data-ttu-id="0a44a-170">С помощью **библиотеки добавьте** следующие элементы в сцену **"Элемент 1".**</span><span class="sxs-lookup"><span data-stu-id="0a44a-170">Using the **Library**, add the following items to the **Item 1 Scene**.</span></span>

    - <span data-ttu-id="0a44a-171">Одно **представление изображения**</span><span class="sxs-lookup"><span data-stu-id="0a44a-171">One **Image View**</span></span>
    - <span data-ttu-id="0a44a-172">Две **метки**</span><span class="sxs-lookup"><span data-stu-id="0a44a-172">Two **Labels**</span></span>
    - <span data-ttu-id="0a44a-173">Одна **кнопка**</span><span class="sxs-lookup"><span data-stu-id="0a44a-173">One **Button**</span></span>
1. <span data-ttu-id="0a44a-174">Используя **Connections Inspector,** сделайте следующие подключения.</span><span class="sxs-lookup"><span data-stu-id="0a44a-174">Using the **Connections Inspector**, make the following connections.</span></span>

    - <span data-ttu-id="0a44a-175">**Привяжете выход userDisplayName** к первой метке.</span><span class="sxs-lookup"><span data-stu-id="0a44a-175">Link the **userDisplayName** outlet to the first label.</span></span>
    - <span data-ttu-id="0a44a-176">**Привяжете выход userEmail** к второй метке.</span><span class="sxs-lookup"><span data-stu-id="0a44a-176">Link the **userEmail** outlet to the second label.</span></span>
    - <span data-ttu-id="0a44a-177">**Привяжете выход userProfilePhoto** к представлению изображения.</span><span class="sxs-lookup"><span data-stu-id="0a44a-177">Link the **userProfilePhoto** outlet to the image view.</span></span>
    - <span data-ttu-id="0a44a-178">Связать **полученное действие signOut** с кнопкой **"Касание вверх" внутри.**</span><span class="sxs-lookup"><span data-stu-id="0a44a-178">Link the **signOut** received action to the button's **Touch Up Inside**.</span></span>

1. <span data-ttu-id="0a44a-179">Выберите представление изображения, а затем выберите **инспектор размеров.**</span><span class="sxs-lookup"><span data-stu-id="0a44a-179">Select the image view, then select the **Size Inspector**.</span></span>
1. <span data-ttu-id="0a44a-180">Установите для **ширины** **и высоты** 196.</span><span class="sxs-lookup"><span data-stu-id="0a44a-180">Set the **Width** and **Height** to 196.</span></span>
1. <span data-ttu-id="0a44a-181">Используйте **кнопку** "Выровнять", чтобы добавить ограничение по горизонтали **в контейнере** со значением 0.</span><span class="sxs-lookup"><span data-stu-id="0a44a-181">Use the **Align** button to add the **Horizontally in container** constraint with a value of 0.</span></span>
1. <span data-ttu-id="0a44a-182">Используйте **кнопку "Добавить новые ограничения"** (рядом с кнопкой **"Выравнивание"),** чтобы добавить следующие ограничения:</span><span class="sxs-lookup"><span data-stu-id="0a44a-182">Use the **Add New Constraints** button (next to the **Align** button) to add the following constraints:</span></span>

    - <span data-ttu-id="0a44a-183">Выравнивание вверху: безопасная область, значение: 0</span><span class="sxs-lookup"><span data-stu-id="0a44a-183">Align Top to: Safe Area, value: 0</span></span>
    - <span data-ttu-id="0a44a-184">Bottom Space to: User Display Name, value: Standard</span><span class="sxs-lookup"><span data-stu-id="0a44a-184">Bottom Space to: User Display Name, value: Standard</span></span>
    - <span data-ttu-id="0a44a-185">Высота, значение: 196</span><span class="sxs-lookup"><span data-stu-id="0a44a-185">Height, value: 196</span></span>
    - <span data-ttu-id="0a44a-186">Ширина, значение: 196</span><span class="sxs-lookup"><span data-stu-id="0a44a-186">Width, value: 196</span></span>

    ![Снимок экрана с новыми настройками ограничений в Xcode](./images/add-new-constraints.png)

1. <span data-ttu-id="0a44a-188">Выберите первую метку, а затем  с помощью кнопки **"Выравнивание"** добавьте ограничение по горизонтали в контейнере со значением 0.</span><span class="sxs-lookup"><span data-stu-id="0a44a-188">Select the first label, then use the **Align** button to add the **Horizontally in container** constraint with a value of 0.</span></span>
1. <span data-ttu-id="0a44a-189">Используйте **кнопку "Добавить новые ограничения",** чтобы добавить следующие ограничения:</span><span class="sxs-lookup"><span data-stu-id="0a44a-189">Use the **Add New Constraints** button to add the following constraints:</span></span>

    - <span data-ttu-id="0a44a-190">Top Space to: User Profile Photo, value: Standard</span><span class="sxs-lookup"><span data-stu-id="0a44a-190">Top Space to: User Profile Photo, value: Standard</span></span>
    - <span data-ttu-id="0a44a-191">Bottom Space to: User Email, value: Standard</span><span class="sxs-lookup"><span data-stu-id="0a44a-191">Bottom Space to: User Email, value: Standard</span></span>

1. <span data-ttu-id="0a44a-192">Выберите вторую метку, а затем **— инспектор атрибутов.**</span><span class="sxs-lookup"><span data-stu-id="0a44a-192">Select the second label, then select the **Attributes Inspector**.</span></span>
1. <span data-ttu-id="0a44a-193">Измените **цвет на** **темно-серый и** измените **шрифт** на **System 12.0.**</span><span class="sxs-lookup"><span data-stu-id="0a44a-193">Change the **Color** to **Dark Gray Color**, and change the **Font** to **System 12.0**.</span></span>
1. <span data-ttu-id="0a44a-194">Используйте **кнопку** "Выровнять", чтобы добавить ограничение по горизонтали **в контейнере** со значением 0.</span><span class="sxs-lookup"><span data-stu-id="0a44a-194">Use the **Align** button to add the **Horizontally in container** constraint with a value of 0.</span></span>
1. <span data-ttu-id="0a44a-195">Используйте **кнопку "Добавить новые ограничения",** чтобы добавить следующие ограничения:</span><span class="sxs-lookup"><span data-stu-id="0a44a-195">Use the **Add New Constraints** button to add the following constraints:</span></span>

    - <span data-ttu-id="0a44a-196">Top Space to: User Display Name, value: Standard</span><span class="sxs-lookup"><span data-stu-id="0a44a-196">Top Space to: User Display Name, value: Standard</span></span>
    - <span data-ttu-id="0a44a-197">Bottom Space to: Sign Out, value: 14</span><span class="sxs-lookup"><span data-stu-id="0a44a-197">Bottom Space to: Sign Out, value: 14</span></span>

1. <span data-ttu-id="0a44a-198">Выберите кнопку, а затем — **инспектор атрибутов.**</span><span class="sxs-lookup"><span data-stu-id="0a44a-198">Select the button, then select the **Attributes Inspector**.</span></span>
1. <span data-ttu-id="0a44a-199">Измените **заголовок** на `Sign Out` .</span><span class="sxs-lookup"><span data-stu-id="0a44a-199">Change the **Title** to `Sign Out`.</span></span>
1. <span data-ttu-id="0a44a-200">Используйте **кнопку** "Выровнять", чтобы добавить ограничение по горизонтали **в контейнере** со значением 0.</span><span class="sxs-lookup"><span data-stu-id="0a44a-200">Use the **Align** button to add the **Horizontally in container** constraint with a value of 0.</span></span>
1. <span data-ttu-id="0a44a-201">Используйте **кнопку "Добавить новые ограничения",** чтобы добавить следующие ограничения:</span><span class="sxs-lookup"><span data-stu-id="0a44a-201">Use the **Add New Constraints** button to add the following constraints:</span></span>

    - <span data-ttu-id="0a44a-202">Top Space to: User Email, value: 14</span><span class="sxs-lookup"><span data-stu-id="0a44a-202">Top Space to: User Email, value: 14</span></span>

1. <span data-ttu-id="0a44a-203">Выберите элемент панели вкладок в нижней части сцены, а затем выберите **инспектор атрибутов.**</span><span class="sxs-lookup"><span data-stu-id="0a44a-203">Select the tab bar item at the bottom of the scene, then select the **Attributes Inspector**.</span></span> <span data-ttu-id="0a44a-204">Измените **заголовок** на `Me` .</span><span class="sxs-lookup"><span data-stu-id="0a44a-204">Change the **Title** to `Me`.</span></span>

<span data-ttu-id="0a44a-205">После этого сцена приветствия должна выглядеть примерно так:</span><span class="sxs-lookup"><span data-stu-id="0a44a-205">The welcome scene should look similar to this once you're done.</span></span>

![Снимок экрана макета сцены приветствия](images/welcome-scene-layout.png)

### <a name="create-calendar-page"></a><span data-ttu-id="0a44a-207">Страница "Создание календаря"</span><span class="sxs-lookup"><span data-stu-id="0a44a-207">Create calendar page</span></span>

1. <span data-ttu-id="0a44a-208">Создайте файл **класса Cocoa Touch в** папке **GraphTutorial** с именем `CalendarViewController` .</span><span class="sxs-lookup"><span data-stu-id="0a44a-208">Create a new **Cocoa Touch Class** file in the **GraphTutorial** folder named `CalendarViewController`.</span></span> <span data-ttu-id="0a44a-209">Выберите **UIViewController** в **подклассе** поля.</span><span class="sxs-lookup"><span data-stu-id="0a44a-209">Choose **UIViewController** in the **Subclass of** field.</span></span>
1. <span data-ttu-id="0a44a-210">Откройте **CalendarViewController.swift** и замените его содержимое следующим кодом.</span><span class="sxs-lookup"><span data-stu-id="0a44a-210">Open **CalendarViewController.swift** and replace its contents with the following code.</span></span>

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

1. <span data-ttu-id="0a44a-211">Откройте **main.storyboard.**</span><span class="sxs-lookup"><span data-stu-id="0a44a-211">Open **Main.storyboard**.</span></span> <span data-ttu-id="0a44a-212">Выберите сцену **"Элемент 2"** и выберите инспектор **удостоверений.**</span><span class="sxs-lookup"><span data-stu-id="0a44a-212">Select the **Item 2 Scene**, then select the **Identity Inspector**.</span></span> <span data-ttu-id="0a44a-213">Измените **значение класса** на **CalendarViewController.**</span><span class="sxs-lookup"><span data-stu-id="0a44a-213">Change the **Class** value to **CalendarViewController**.</span></span>
1. <span data-ttu-id="0a44a-214">С помощью **библиотеки добавьте** представление **текста в** сцену **"Элемент 2".**</span><span class="sxs-lookup"><span data-stu-id="0a44a-214">Using the **Library**, add a **Text View** to the **Item 2 Scene**.</span></span>
1. <span data-ttu-id="0a44a-215">Выберите только что добавленное текстовое представление.</span><span class="sxs-lookup"><span data-stu-id="0a44a-215">Select the text view you just added.</span></span> <span data-ttu-id="0a44a-216">В меню **"Редактор"** выберите **"Встраить"** и "Прокрутите **представление".**</span><span class="sxs-lookup"><span data-stu-id="0a44a-216">On the **Editor** menu, choose **Embed In**, then **Scroll View**.</span></span>
1. <span data-ttu-id="0a44a-217">Измейте представление прокрутки и текст, чтобы занять весь экран.</span><span class="sxs-lookup"><span data-stu-id="0a44a-217">Resize the scroll view and text view to take up the entire screen.</span></span>
1. <span data-ttu-id="0a44a-218">С помощью **Connections Inspector** подключите выход **calendarJSON** к представлению текста.</span><span class="sxs-lookup"><span data-stu-id="0a44a-218">Using the **Connections Inspector**, connect the **calendarJSON** outlet to the text view.</span></span>
1. <span data-ttu-id="0a44a-219">Выберите элемент панели вкладок в нижней части сцены, а затем выберите **инспектор атрибутов.**</span><span class="sxs-lookup"><span data-stu-id="0a44a-219">Select the tab bar item at the bottom of the scene, then select the **Attributes Inspector**.</span></span> <span data-ttu-id="0a44a-220">Измените **заголовок** на `Calendar` .</span><span class="sxs-lookup"><span data-stu-id="0a44a-220">Change the **Title** to `Calendar`.</span></span>
1. <span data-ttu-id="0a44a-221">В меню **"Редактор"** выберите **"Разрешить** проблемы с автоматическим макетом", а затем выберите "Добавить отсутствующие **ограничения"** под всеми представлениями в контроллере **представления календаря.**</span><span class="sxs-lookup"><span data-stu-id="0a44a-221">On the **Editor** menu, select **Resolve Auto Layout Issues**, then select **Add Missing Constraints** underneath **All Views in Calendar View Controller**.</span></span>

<span data-ttu-id="0a44a-222">После этого сцена календаря должна выглядеть примерно так:</span><span class="sxs-lookup"><span data-stu-id="0a44a-222">The calendar scene should look similar to this once you're done.</span></span>

![Снимок экрана с макетом сцены "Календарь"](images/calendar-scene-layout.png)

### <a name="create-activity-indicator"></a><span data-ttu-id="0a44a-224">Создание индикатора активности</span><span class="sxs-lookup"><span data-stu-id="0a44a-224">Create activity indicator</span></span>

1. <span data-ttu-id="0a44a-225">Создайте файл **класса Cocoa Touch в** папке **GraphTutorial** с именем `SpinnerViewController` .</span><span class="sxs-lookup"><span data-stu-id="0a44a-225">Create a new **Cocoa Touch Class** file in the **GraphTutorial** folder named `SpinnerViewController`.</span></span> <span data-ttu-id="0a44a-226">Выберите **UIViewController** в **подклассе** поля.</span><span class="sxs-lookup"><span data-stu-id="0a44a-226">Choose **UIViewController** in the **Subclass of** field.</span></span>
1. <span data-ttu-id="0a44a-227">Откройте **SpinnerViewController.swift** и замените его содержимое следующим кодом.</span><span class="sxs-lookup"><span data-stu-id="0a44a-227">Open **SpinnerViewController.swift** and replace its contents with the following code.</span></span>

    :::code language="swift" source="../demo/GraphTutorial/GraphTutorial/SpinnerViewController.swift" id="SpinnerSnippet":::

## <a name="test-the-app"></a><span data-ttu-id="0a44a-228">Тестирование приложения</span><span class="sxs-lookup"><span data-stu-id="0a44a-228">Test the app</span></span>

<span data-ttu-id="0a44a-229">Сохраните изменения и запустите приложение.</span><span class="sxs-lookup"><span data-stu-id="0a44a-229">Save your changes and launch the app.</span></span> <span data-ttu-id="0a44a-230">Вы должны иметь возможность перемещаться между  экранами  с помощью кнопок "Вход" и "Выход" и панели вкладок.</span><span class="sxs-lookup"><span data-stu-id="0a44a-230">You should be able to move between the screens using the **Sign In** and **Sign Out** buttons and the tab bar.</span></span>

![Снимки экрана приложения](images/app-screens.png)
