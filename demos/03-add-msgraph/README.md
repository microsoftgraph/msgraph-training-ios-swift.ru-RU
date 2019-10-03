# <a name="how-to-run-the-completed-project"></a><span data-ttu-id="5e12c-101">Выполнение завершенного проекта</span><span class="sxs-lookup"><span data-stu-id="5e12c-101">How to run the completed project</span></span>

## <a name="prerequisites"></a><span data-ttu-id="5e12c-102">Предварительные требования</span><span class="sxs-lookup"><span data-stu-id="5e12c-102">Prerequisites</span></span>

<span data-ttu-id="5e12c-103">Чтобы запустить завершенный проект в этой папке, вам потребуются следующие компоненты:</span><span class="sxs-lookup"><span data-stu-id="5e12c-103">To run the completed project in this folder, you need the following:</span></span>

- [<span data-ttu-id="5e12c-104">Xcode</span><span class="sxs-lookup"><span data-stu-id="5e12c-104">Xcode</span></span>](https://developer.apple.com/xcode/)
- [<span data-ttu-id="5e12c-105">CocoaPods</span><span class="sxs-lookup"><span data-stu-id="5e12c-105">CocoaPods</span></span>](https://cocoapods.org)
- <span data-ttu-id="5e12c-106">Личная учетная запись Майкрософт с почтовым ящиком на Outlook.com или рабочей или учебной учетной записью Майкрософт.</span><span class="sxs-lookup"><span data-stu-id="5e12c-106">Either a personal Microsoft account with a mailbox on Outlook.com, or a Microsoft work or school account.</span></span>

> <span data-ttu-id="5e12c-107">**Примечание:** Это руководство было написано с помощью Xcode версии 10,3 и CocoaPods версии 1.7.5.</span><span class="sxs-lookup"><span data-stu-id="5e12c-107">**Note:** This tutorial was written using Xcode version 10.3 and CocoaPods version 1.7.5.</span></span> <span data-ttu-id="5e12c-108">Действия, описанные в этом руководстве, могут работать с другими версиями, но не тестировались.</span><span class="sxs-lookup"><span data-stu-id="5e12c-108">The steps in this guide may work with other versions, but that has not been tested.</span></span>

<span data-ttu-id="5e12c-109">Если у вас нет учетной записи Майкрософт, у вас есть несколько вариантов для получения бесплатной учетной записи:</span><span class="sxs-lookup"><span data-stu-id="5e12c-109">If you don't have a Microsoft account, there are a couple of options to get a free account:</span></span>

- <span data-ttu-id="5e12c-110">Вы можете [зарегистрироваться для создания новой личной учетной записи Майкрософт](https://signup.live.com/signup?wa=wsignin1.0&rpsnv=12&ct=1454618383&rver=6.4.6456.0&wp=MBI_SSL_SHARED&wreply=https://mail.live.com/default.aspx&id=64855&cbcxt=mai&bk=1454618383&uiflavor=web&uaid=b213a65b4fdc484382b6622b3ecaa547&mkt=E-US&lc=1033&lic=1).</span><span class="sxs-lookup"><span data-stu-id="5e12c-110">You can [sign up for a new personal Microsoft account](https://signup.live.com/signup?wa=wsignin1.0&rpsnv=12&ct=1454618383&rver=6.4.6456.0&wp=MBI_SSL_SHARED&wreply=https://mail.live.com/default.aspx&id=64855&cbcxt=mai&bk=1454618383&uiflavor=web&uaid=b213a65b4fdc484382b6622b3ecaa547&mkt=E-US&lc=1033&lic=1).</span></span>
- <span data-ttu-id="5e12c-111">Вы можете [зарегистрироваться в программе для разработчиков office 365](https://developer.microsoft.com/office/dev-program) , чтобы получить бесплатную подписку на Office 365.</span><span class="sxs-lookup"><span data-stu-id="5e12c-111">You can [sign up for the Office 365 Developer Program](https://developer.microsoft.com/office/dev-program) to get a free Office 365 subscription.</span></span>

## <a name="register-a-web-application-with-the-azure-active-directory-admin-center"></a><span data-ttu-id="5e12c-112">Регистрация веб-приложения с помощью центра администрирования Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="5e12c-112">Register a web application with the Azure Active Directory admin center</span></span>

1. <span data-ttu-id="5e12c-113">Откройте браузер и перейдите к [Центру администрирования Azure Active Directory](https://aad.portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="5e12c-113">Open a browser and navigate to the [Azure Active Directory admin center](https://aad.portal.azure.com).</span></span> <span data-ttu-id="5e12c-114">Войдите с помощью **личной учетной записи** (т.е. учетной записи Microsoft) или **рабочей (учебной) учетной записи**.</span><span class="sxs-lookup"><span data-stu-id="5e12c-114">Login using a **personal account** (aka: Microsoft Account) or **Work or School Account**.</span></span>

1. <span data-ttu-id="5e12c-115">Выберите **Azure Active Directory** в левой панели навигации, а затем выберите **Регистрация приложений** в разделе **Управление**.</span><span class="sxs-lookup"><span data-stu-id="5e12c-115">Select **Azure Active Directory** in the left-hand navigation, then select **App registrations** under **Manage**.</span></span>

    ![<span data-ttu-id="5e12c-116">Снимок экрана с регистрациями приложений</span><span class="sxs-lookup"><span data-stu-id="5e12c-116">A screenshot of the App registrations</span></span> ](/tutorial/images/aad-portal-app-registrations.png)

1. <span data-ttu-id="5e12c-117">Выберите **Новая регистрация**.</span><span class="sxs-lookup"><span data-stu-id="5e12c-117">Select **New registration**.</span></span> <span data-ttu-id="5e12c-118">На странице**Зарегистрировать приложение** задайте необходимые значения следующим образом.</span><span class="sxs-lookup"><span data-stu-id="5e12c-118">On the **Register an application** page, set the values as follows.</span></span>

    - <span data-ttu-id="5e12c-119">Введите **имя** `iOS Swift Graph Tutorial`.</span><span class="sxs-lookup"><span data-stu-id="5e12c-119">Set **Name** to `iOS Swift Graph Tutorial`.</span></span>
    - <span data-ttu-id="5e12c-120">Введите **поддерживаемые типы учетных записей** для **учетных записей в любом каталоге организаций и личных учетных записей Microsoft**.</span><span class="sxs-lookup"><span data-stu-id="5e12c-120">Set **Supported account types** to **Accounts in any organizational directory and personal Microsoft accounts**.</span></span>
    - <span data-ttu-id="5e12c-121">В разделе **URI перенаправления**измените раскрывающийся список на **общедоступный клиент (мобильный & Настольный)** и `msauth.YOUR_BUNDLE_ID://auth`задайте для `YOUR_BUNDLE_ID` него значение, заменив идентификатором пакета для своего приложения.</span><span class="sxs-lookup"><span data-stu-id="5e12c-121">Under **Redirect URI**, change the dropdown to **Public client (mobile & desktop)**, and set the value to `msauth.YOUR_BUNDLE_ID://auth`, replacing `YOUR_BUNDLE_ID` with the bundle ID for your app.</span></span>

    ![Снимок страницы "регистрация приложения"](/tutorial/images/aad-register-an-app.png)

1. <span data-ttu-id="5e12c-123">Выберите **Зарегистрировать**.</span><span class="sxs-lookup"><span data-stu-id="5e12c-123">Choose **Register**.</span></span> <span data-ttu-id="5e12c-124">На странице **учебника SWIFT Graph для iOS** СКОПИРУЙТЕ значение **идентификатора Application (Client)** и сохраните его, он понадобится на следующем шаге.</span><span class="sxs-lookup"><span data-stu-id="5e12c-124">On the **iOS Swift Graph Tutorial** page, copy the value of the **Application (client) ID** and save it, you will need it in the next step.</span></span>

    ![Снимок экрана с ИДЕНТИФИКАТОРом приложения для новой регистрации приложения](/tutorial/images/aad-application-id.png)

## <a name="configure-the-sample"></a><span data-ttu-id="5e12c-126">Настройка примера</span><span class="sxs-lookup"><span data-stu-id="5e12c-126">Configure the sample</span></span>

1. <span data-ttu-id="5e12c-127">Переименуйте `AuthSettings.plist.example` файл в `AuthSettings.plist`.</span><span class="sxs-lookup"><span data-stu-id="5e12c-127">Rename the `AuthSettings.plist.example` file to `AuthSettings.plist`.</span></span>
1. <span data-ttu-id="5e12c-128">Измените `AuthSettings.plist` файл и внесите следующие изменения.</span><span class="sxs-lookup"><span data-stu-id="5e12c-128">Edit the `AuthSettings.plist` file and make the following changes.</span></span>
    1. <span data-ttu-id="5e12c-129">Замените `YOUR_APP_ID_HERE` **идентификатором приложения** , полученным на портале регистрации приложений.</span><span class="sxs-lookup"><span data-stu-id="5e12c-129">Replace `YOUR_APP_ID_HERE` with the **Application Id** you got from the App Registration Portal.</span></span>
1. <span data-ttu-id="5e12c-130">Откройте терминал в каталоге **графтуториал** и выполните следующую команду.</span><span class="sxs-lookup"><span data-stu-id="5e12c-130">Open Terminal in the **GraphTutorial** directory and run the following command.</span></span>

    ```Shell
    pod install
    ```

## <a name="run-the-sample"></a><span data-ttu-id="5e12c-131">Запуск приложения</span><span class="sxs-lookup"><span data-stu-id="5e12c-131">Run the sample</span></span>

<span data-ttu-id="5e12c-132">В Xcode выберите меню **продукт** и **выполните команду выполнить**.</span><span class="sxs-lookup"><span data-stu-id="5e12c-132">In Xcode, select the **Product** menu, then select **Run**.</span></span>
