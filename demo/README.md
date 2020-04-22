# <a name="how-to-run-the-completed-project"></a><span data-ttu-id="48be3-101">Выполнение завершенного проекта</span><span class="sxs-lookup"><span data-stu-id="48be3-101">How to run the completed project</span></span>

## <a name="prerequisites"></a><span data-ttu-id="48be3-102">Необходимые компоненты</span><span class="sxs-lookup"><span data-stu-id="48be3-102">Prerequisites</span></span>

<span data-ttu-id="48be3-103">Чтобы запустить завершенный проект в этой папке, вам потребуются следующие компоненты:</span><span class="sxs-lookup"><span data-stu-id="48be3-103">To run the completed project in this folder, you need the following:</span></span>

- [<span data-ttu-id="48be3-104">Xcode</span><span class="sxs-lookup"><span data-stu-id="48be3-104">Xcode</span></span>](https://developer.apple.com/xcode/)
- [<span data-ttu-id="48be3-105">CocoaPods</span><span class="sxs-lookup"><span data-stu-id="48be3-105">CocoaPods</span></span>](https://cocoapods.org)
- <span data-ttu-id="48be3-106">Личная учетная запись Майкрософт с почтовым ящиком на Outlook.com или рабочей или учебной учетной записью Майкрософт.</span><span class="sxs-lookup"><span data-stu-id="48be3-106">Either a personal Microsoft account with a mailbox on Outlook.com, or a Microsoft work or school account.</span></span>

> <span data-ttu-id="48be3-107">**Примечание:** Это руководство было написано с помощью Xcode версии 11,4 и CocoaPods версии 1.9.1.</span><span class="sxs-lookup"><span data-stu-id="48be3-107">**Note:** This tutorial was written using Xcode version 11.4 and CocoaPods version 1.9.1.</span></span> <span data-ttu-id="48be3-108">Действия, описанные в этом руководстве, могут работать с другими версиями, но не тестировались.</span><span class="sxs-lookup"><span data-stu-id="48be3-108">The steps in this guide may work with other versions, but that has not been tested.</span></span>

<span data-ttu-id="48be3-109">Если у вас нет учетной записи Майкрософт, у вас есть несколько вариантов для получения бесплатной учетной записи:</span><span class="sxs-lookup"><span data-stu-id="48be3-109">If you don't have a Microsoft account, there are a couple of options to get a free account:</span></span>

- <span data-ttu-id="48be3-110">Вы можете [зарегистрироваться для создания новой личной учетной записи Майкрософт](https://signup.live.com/signup?wa=wsignin1.0&rpsnv=12&ct=1454618383&rver=6.4.6456.0&wp=MBI_SSL_SHARED&wreply=https://mail.live.com/default.aspx&id=64855&cbcxt=mai&bk=1454618383&uiflavor=web&uaid=b213a65b4fdc484382b6622b3ecaa547&mkt=E-US&lc=1033&lic=1).</span><span class="sxs-lookup"><span data-stu-id="48be3-110">You can [sign up for a new personal Microsoft account](https://signup.live.com/signup?wa=wsignin1.0&rpsnv=12&ct=1454618383&rver=6.4.6456.0&wp=MBI_SSL_SHARED&wreply=https://mail.live.com/default.aspx&id=64855&cbcxt=mai&bk=1454618383&uiflavor=web&uaid=b213a65b4fdc484382b6622b3ecaa547&mkt=E-US&lc=1033&lic=1).</span></span>
- <span data-ttu-id="48be3-111">Вы можете [зарегистрироваться в программе для разработчиков office 365](https://developer.microsoft.com/office/dev-program) , чтобы получить бесплатную подписку на Office 365.</span><span class="sxs-lookup"><span data-stu-id="48be3-111">You can [sign up for the Office 365 Developer Program](https://developer.microsoft.com/office/dev-program) to get a free Office 365 subscription.</span></span>

## <a name="register-a-web-application-with-the-azure-active-directory-admin-center"></a><span data-ttu-id="48be3-112">Регистрация веб-приложения с помощью центра администрирования Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="48be3-112">Register a web application with the Azure Active Directory admin center</span></span>

1. <span data-ttu-id="48be3-113">Откройте браузер и перейдите к [Центру администрирования Azure Active Directory](https://aad.portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="48be3-113">Open a browser and navigate to the [Azure Active Directory admin center](https://aad.portal.azure.com).</span></span> <span data-ttu-id="48be3-114">Войдите с помощью **личной учетной записи** (т.е. учетной записи Microsoft) или **рабочей (учебной) учетной записи**.</span><span class="sxs-lookup"><span data-stu-id="48be3-114">Login using a **personal account** (aka: Microsoft Account) or **Work or School Account**.</span></span>

1. <span data-ttu-id="48be3-115">Выберите **Azure Active Directory** на панели навигации слева, затем выберите **Регистрация приложений** в разделе **Управление**.</span><span class="sxs-lookup"><span data-stu-id="48be3-115">Select **Azure Active Directory** in the left-hand navigation, then select **App registrations** under **Manage**.</span></span>

    ![<span data-ttu-id="48be3-116">Снимок экрана с регистрациями приложений</span><span class="sxs-lookup"><span data-stu-id="48be3-116">A screenshot of the App registrations</span></span> ](/tutorial/images/aad-portal-app-registrations.png)

1. <span data-ttu-id="48be3-117">Выберите **Новая регистрация**.</span><span class="sxs-lookup"><span data-stu-id="48be3-117">Select **New registration**.</span></span> <span data-ttu-id="48be3-118">На странице**Зарегистрировать приложение** задайте необходимые значения следующим образом.</span><span class="sxs-lookup"><span data-stu-id="48be3-118">On the **Register an application** page, set the values as follows.</span></span>

    - <span data-ttu-id="48be3-119">Введите **имя** `iOS Swift Graph Tutorial`.</span><span class="sxs-lookup"><span data-stu-id="48be3-119">Set **Name** to `iOS Swift Graph Tutorial`.</span></span>
    - <span data-ttu-id="48be3-120">Введите **поддерживаемые типы учетных записей** для **учетных записей в любом каталоге организаций и личных учетных записей Microsoft**.</span><span class="sxs-lookup"><span data-stu-id="48be3-120">Set **Supported account types** to **Accounts in any organizational directory and personal Microsoft accounts**.</span></span>
    - <span data-ttu-id="48be3-121">Оставьте поле **URI перенаправления** пустым.</span><span class="sxs-lookup"><span data-stu-id="48be3-121">Leave **Redirect URI** empty.</span></span>

    ![Снимок страницы "регистрация приложения"](/tutorial/images/aad-register-an-app.png)

1. <span data-ttu-id="48be3-123">Нажмите кнопку **Зарегистрировать**.</span><span class="sxs-lookup"><span data-stu-id="48be3-123">Choose **Register**.</span></span> <span data-ttu-id="48be3-124">На странице **учебника SWIFT Graph для iOS** СКОПИРУЙТЕ значение **идентификатора Application (Client)** и сохраните его, он понадобится на следующем шаге.</span><span class="sxs-lookup"><span data-stu-id="48be3-124">On the **iOS Swift Graph Tutorial** page, copy the value of the **Application (client) ID** and save it, you will need it in the next step.</span></span>

    ![Снимок экрана с ИДЕНТИФИКАТОРом приложения для новой регистрации приложения](/tutorial/images/aad-application-id.png)

1. <span data-ttu-id="48be3-126">Выберите **Проверка подлинности** в разделе **Управление**.</span><span class="sxs-lookup"><span data-stu-id="48be3-126">Select **Authentication** under **Manage**.</span></span> <span data-ttu-id="48be3-127">Выберите **Добавить платформу**, а затем **iOS/macOS**.</span><span class="sxs-lookup"><span data-stu-id="48be3-127">Select **Add a platform**, then **iOS / macOS**.</span></span>

1. <span data-ttu-id="48be3-128">Введите идентификатор пакета приложения и выберите configure ( **настроить**), а затем нажмите кнопку **Готово**.</span><span class="sxs-lookup"><span data-stu-id="48be3-128">Enter your app's Bundle ID and select **Configure**, then select **Done**.</span></span>

## <a name="configure-the-sample"></a><span data-ttu-id="48be3-129">Настройка примера</span><span class="sxs-lookup"><span data-stu-id="48be3-129">Configure the sample</span></span>

1. <span data-ttu-id="48be3-130">Переименуйте `AuthSettings.plist.example` файл в `AuthSettings.plist`.</span><span class="sxs-lookup"><span data-stu-id="48be3-130">Rename the `AuthSettings.plist.example` file to `AuthSettings.plist`.</span></span>
1. <span data-ttu-id="48be3-131">Измените `AuthSettings.plist` файл и внесите следующие изменения.</span><span class="sxs-lookup"><span data-stu-id="48be3-131">Edit the `AuthSettings.plist` file and make the following changes.</span></span>
    1. <span data-ttu-id="48be3-132">Замените `YOUR_APP_ID_HERE` **идентификатором приложения** , полученным на портале регистрации приложений.</span><span class="sxs-lookup"><span data-stu-id="48be3-132">Replace `YOUR_APP_ID_HERE` with the **Application Id** you got from the App Registration Portal.</span></span>
1. <span data-ttu-id="48be3-133">Откройте терминал в каталоге **графтуториал** и выполните следующую команду.</span><span class="sxs-lookup"><span data-stu-id="48be3-133">Open Terminal in the **GraphTutorial** directory and run the following command.</span></span>

    ```Shell
    pod install
    ```

## <a name="run-the-sample"></a><span data-ttu-id="48be3-134">Запуск приложения</span><span class="sxs-lookup"><span data-stu-id="48be3-134">Run the sample</span></span>

<span data-ttu-id="48be3-135">В Xcode выберите меню **продукт** и **выполните команду выполнить**.</span><span class="sxs-lookup"><span data-stu-id="48be3-135">In Xcode, select the **Product** menu, then select **Run**.</span></span>
