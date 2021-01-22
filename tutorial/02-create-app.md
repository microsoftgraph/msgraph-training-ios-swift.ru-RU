<!-- markdownlint-disable MD002 MD041 -->

Начните с создания нового проекта Swift.

1. Откройте Xcode. В меню **"Файл"** выберите **"Новый"** и **"Проект".**
1. Выберите шаблон **приложения и** выберите **"Далее".**

    ![Снимок экрана: диалоговое окно выбора шаблона Xcode](images/xcode-select-template.png)

1. Установите для **имени продукта** и `GraphTutorial` для **языка** swift . 
1. Заполните оставшиеся поля и выберите **"Далее".**
1. Выберите расположение для проекта и выберите **"Создать".**

## <a name="install-dependencies"></a>Установка зависимостей

Прежде чем двигаться дальше, установите некоторые дополнительные зависимости, которые вы будете использовать позже.

- [Microsoft Authentication Library (MSAL) для iOS](https://github.com/AzureAD/microsoft-authentication-library-for-objc) для проверки подлинности в Azure AD.
- [Microsoft Graph SDK для Objective C](https://github.com/microsoftgraph/msgraph-sdk-objc) для вызовов Microsoft Graph.
- [Microsoft Graph Models SDK for Objective C](https://github.com/microsoftgraph/msgraph-sdk-objc-models) for strongly-typed objects representing Microsoft Graph resources like users or events.

1. Выйти из Xcode.
1. Откройте терминал и измените каталог на расположение проекта **GraphTutorial.**
1. Чтобы создать Podfile, запустите следующую команду.

    ```Shell
    pod init
    ```

1. Откройте Podfile и добавьте следующие строки сразу после `use_frameworks!` строки.

    ```Ruby
    pod 'MSAL', '~> 1.1.13'
    pod 'MSGraphClientSDK', ' ~> 1.0.0'
    pod 'MSGraphClientModels', '~> 1.3.0'
    ```

1. Сохраните Podfile и запустите следующую команду, чтобы установить зависимости.

    ```Shell
    pod install
    ```

1. После выполнения команды откройте созданное **пространство GraphTutorial.xcworkspace** в Xcode.

## <a name="design-the-app"></a>Проектирование приложения

В этом разделе вы создадим представления для приложения: страницу для входов, навигатор панели вкладок, страницу приветствия и страницу календаря. Вы также создадим наложение индикатора активности.

### <a name="create-sign-in-page"></a>Страница "Создание страницы для входов"

1. **Развертка папки GraphTutorial** в Xcode и выберите **ViewController.swift.**
1. В **инспекторе файлов** **измените** имя файла на `SignInViewController.swift` .

    ![Снимок экрана: инспектор файлов](images/rename-file.png)

1. Откройте **SignInViewController.swift** и замените его содержимое следующим кодом.

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

1. Откройте **main.storyboard.** Раз **развернуть сцену контроллера представления,** а затем выберите **контроллер представления.**

    ![Снимок экрана: Xcode с выбранным контроллером просмотра](images/storyboard-select-view-controller.png)

1. Выберите identity **Inspector,** а затем измените **выпадающий** класс на **SignInViewController.**

    ![Снимок экрана инспектора удостоверений](images/change-class.png)

1. Выберите **библиотеку,** а затем перетащите **кнопку** на контроллер **просмотра для входов.**

    ![Снимок экрана: библиотека в Xcode](images/add-button-to-view.png)

1. После нажатия кнопки выберите **"Attributes Inspector"** (Инспектор атрибутов) и измените **заголовок** кнопки на `Sign In` .

    ![Снимок экрана с полем "Заголовок" в инспекторе атрибутов в Xcode](images/set-button-title.png)

1. Нажатие кнопки "Выровнять" в нижней части storyboard.  Выберите и **горизонтально** в  контейнере, и по вертикали в ограничениях контейнера, оставьте их значения 0, а затем выберите **"Добавить 2 ограничения".**

    ![Снимок экрана с настройками ограничений выравнивания в Xcode](images/add-alignment-constraints.png)

1. Выберите контроллер **просмотра для входов,** а затем **выберите инспектор подключений.**
1. В **области "Received Actions"** перетащите незаполненный круг рядом с **кнопкой signIn.** Выберите **"Сенсорный экран" во** всплывающее меню.

    ![Снимок экрана: перетаскивание действия signIn на кнопку в Xcode](images/connect-sign-in-button.png)

### <a name="create-tab-bar"></a>Создание панели вкладок

1. Выберите **библиотеку,** а затем перетащите контроллер **панели вкладок** в storyboard.
1. Выберите контроллер **просмотра для входов,** а затем **выберите инспектор подключений.**
1. В **области "Триггеры"** перетащите незаполненный круг рядом с вручную на контроллер панели вкладок **в** storyboard.  Выберите **"Представить" модально** во всплывающее меню.

    ![Снимок экрана: перетаскивание вручную в новый контроллер панели вкладок в Xcode](images/add-segue.png)

1. Выберите только что добавленную сегу, а затем — **инспектор атрибутов.** Установите для **поля "Идентификатор"** и `userSignedIn` **"Презентация"** полноэкранный **режим.**

    ![Снимок экрана с полем идентификатора в инспекторе атрибутов в Xcode](images/set-segue-identifier.png)

1. Выберите сцену **"Элемент 1"** и выберите **"Connections Inspector" (Инспектор подключений).**
1. В **области "Триггеры"** перетащите незаполненный круг рядом с вручную на контроллер просмотра для входов **в** storyboard.  Выберите **"Представить" модально** во всплывающее меню.
1. Выберите только что добавленную сегу, а затем — **инспектор атрибутов.** Установите для **поля "Идентификатор"** и `userSignedOut` **"Презентация"** полноэкранный **режим.**

### <a name="create-welcome-page"></a>Создание страницы приветствия

1. Выберите файл **Assets.xcassets.**
1. В меню **"Редактор"** выберите **"Добавить новый актив"** и **"Набор изображений".**
1. Выберите новый ресурс **Image** и используйте **attribute Inspector,** чтобы установить его **имя.** `DefaultUserPhoto`
1. Добавьте любое изображение, которое вы хотите выступать в качестве фотографии профиля пользователя по умолчанию.

    ![Снимок экрана: представление ресурсов "Набор изображений" в Xcode](images/add-default-user-photo.png)

1. Создайте файл **класса Cocoa Touch в** папке **GraphTutorial** с именем `WelcomeViewController` . Выберите **UIViewController** в **подклассе** поля.
1. Откройте **WelcomeViewController.swift** и замените его содержимое следующим кодом.

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

1. Откройте **main.storyboard.** Выберите сцену **"Элемент 1"** и выберите инспектор **удостоверений.** Измените **значение класса** на **WelcomeViewController.**
1. С помощью **библиотеки добавьте** следующие элементы в сцену **"Элемент 1".**

    - Одно **представление изображения**
    - Две **метки**
    - Одна **кнопка**
1. Используя **Connections Inspector,** сделайте следующие подключения.

    - **Привяжете выход userDisplayName** к первой метке.
    - **Привяжете выход userEmail** к второй метке.
    - **Привяжете выход userProfilePhoto** к представлению изображения.
    - Связать **полученное действие signOut** с кнопкой **"Касание вверх" внутри.**

1. Выберите представление изображения, а затем выберите **инспектор размеров.**
1. Установите для **ширины** **и высоты** 196.
1. Используйте **кнопку** "Выровнять", чтобы добавить ограничение по горизонтали **в контейнере** со значением 0.
1. Используйте **кнопку "Добавить новые ограничения"** (рядом с кнопкой **"Выравнивание"),** чтобы добавить следующие ограничения:

    - Выравнивание вверху: безопасная область, значение: 0
    - Bottom Space to: User Display Name, value: Standard
    - Высота, значение: 196
    - Ширина, значение: 196

    ![Снимок экрана с новыми настройками ограничений в Xcode](./images/add-new-constraints.png)

1. Выберите первую метку, а затем  с помощью кнопки **"Выравнивание"** добавьте ограничение по горизонтали в контейнере со значением 0.
1. Используйте **кнопку "Добавить новые ограничения",** чтобы добавить следующие ограничения:

    - Top Space to: User Profile Photo, value: Standard
    - Bottom Space to: User Email, value: Standard

1. Выберите вторую метку, а затем **— инспектор атрибутов.**
1. Измените **цвет на** **темно-серый и** измените **шрифт** на **System 12.0.**
1. Используйте **кнопку** "Выровнять", чтобы добавить ограничение по горизонтали **в контейнере** со значением 0.
1. Используйте **кнопку "Добавить новые ограничения",** чтобы добавить следующие ограничения:

    - Top Space to: User Display Name, value: Standard
    - Bottom Space to: Sign Out, value: 14

1. Выберите кнопку, а затем — **инспектор атрибутов.**
1. Измените **заголовок** на `Sign Out` .
1. Используйте **кнопку** "Выровнять", чтобы добавить ограничение по горизонтали **в контейнере** со значением 0.
1. Используйте **кнопку "Добавить новые ограничения",** чтобы добавить следующие ограничения:

    - Top Space to: User Email, value: 14

1. Выберите элемент панели вкладок в нижней части сцены, а затем выберите **инспектор атрибутов.** Измените **заголовок** на `Me` .

После этого сцена приветствия должна выглядеть примерно так:

![Снимок экрана макета сцены приветствия](images/welcome-scene-layout.png)

### <a name="create-calendar-page"></a>Страница "Создание календаря"

1. Создайте файл **класса Cocoa Touch в** папке **GraphTutorial** с именем `CalendarViewController` . Выберите **UIViewController** в **подклассе** поля.
1. Откройте **CalendarViewController.swift** и замените его содержимое следующим кодом.

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

1. Откройте **main.storyboard.** Выберите сцену **"Элемент 2"** и выберите инспектор **удостоверений.** Измените **значение класса** на **CalendarViewController.**
1. С помощью **библиотеки добавьте** представление **текста в** сцену **"Элемент 2".**
1. Выберите только что добавленное текстовое представление. В меню **"Редактор"** выберите **"Встраить"** и "Прокрутите **представление".**
1. Измейте представление прокрутки и текст, чтобы занять весь экран.
1. С помощью **Connections Inspector** подключите выход **calendarJSON** к представлению текста.
1. Выберите элемент панели вкладок в нижней части сцены, а затем выберите **инспектор атрибутов.** Измените **заголовок** на `Calendar` .
1. В меню **"Редактор"** выберите **"Разрешить** проблемы с автоматическим макетом", а затем выберите "Добавить отсутствующие **ограничения"** под всеми представлениями в контроллере **представления календаря.**

После этого сцена календаря должна выглядеть примерно так:

![Снимок экрана с макетом сцены "Календарь"](images/calendar-scene-layout.png)

### <a name="create-activity-indicator"></a>Создание индикатора активности

1. Создайте файл **класса Cocoa Touch в** папке **GraphTutorial** с именем `SpinnerViewController` . Выберите **UIViewController** в **подклассе** поля.
1. Откройте **SpinnerViewController.swift** и замените его содержимое следующим кодом.

    :::code language="swift" source="../demo/GraphTutorial/GraphTutorial/SpinnerViewController.swift" id="SpinnerSnippet":::

## <a name="test-the-app"></a>Тестирование приложения

Сохраните изменения и запустите приложение. Вы должны иметь возможность перемещаться между  экранами  с помощью кнопок "Вход" и "Выход" и панели вкладок.

![Снимки экрана приложения](images/app-screens.png)
