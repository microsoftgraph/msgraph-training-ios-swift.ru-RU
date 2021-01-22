<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="b92a7-101">В этом упражнении вы создадим новое приложение Azure AD с помощью Центра администрирования Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="b92a7-101">In this exercise you will create a new Azure AD native application using the Azure Active Directory admin center.</span></span>

1. <span data-ttu-id="b92a7-102">Откройте браузер и перейдите в [Центр администрирования Azure Active Directory](https://aad.portal.azure.com) и войдите с помощью **личной учетной записи** (т.е. учетной записи Майкрософт) или **рабочей или учебной учетной записи**.</span><span class="sxs-lookup"><span data-stu-id="b92a7-102">Open a browser and navigate to the [Azure Active Directory admin center](https://aad.portal.azure.com) and login using a **personal account** (aka: Microsoft Account) or **Work or School Account**.</span></span>

1. <span data-ttu-id="b92a7-103">Выберите **Azure Active Directory** на панели навигации слева, затем выберите **Регистрация приложений** в разделе **Управление**.</span><span class="sxs-lookup"><span data-stu-id="b92a7-103">Select **Azure Active Directory** in the left-hand navigation, then select **App registrations** under **Manage**.</span></span>

    ![<span data-ttu-id="b92a7-104">Снимок экрана с регистрацией приложений</span><span class="sxs-lookup"><span data-stu-id="b92a7-104">A screenshot of the App registrations</span></span> ](images/aad-portal-app-registrations.png)

1. <span data-ttu-id="b92a7-105">Выберите **Новая регистрация**.</span><span class="sxs-lookup"><span data-stu-id="b92a7-105">Select **New registration**.</span></span> <span data-ttu-id="b92a7-106">На странице **Зарегистрировать приложение** задайте необходимые значения следующим образом.</span><span class="sxs-lookup"><span data-stu-id="b92a7-106">On the **Register an application** page, set the values as follows.</span></span>

    - <span data-ttu-id="b92a7-107">Введите **имя** `iOS Swift Graph Tutorial`.</span><span class="sxs-lookup"><span data-stu-id="b92a7-107">Set **Name** to `iOS Swift Graph Tutorial`.</span></span>
    - <span data-ttu-id="b92a7-108">Введите **поддерживаемые типы учетных записей** для **учетных записей в любом каталоге организаций и личных учетных записей Microsoft**.</span><span class="sxs-lookup"><span data-stu-id="b92a7-108">Set **Supported account types** to **Accounts in any organizational directory and personal Microsoft accounts**.</span></span>
    - <span data-ttu-id="b92a7-109">Оставьте поле **URI перенаправления** пустым.</span><span class="sxs-lookup"><span data-stu-id="b92a7-109">Leave **Redirect URI** empty.</span></span>

    ![Снимок экрана: страница "Регистрация приложения"](images/aad-register-an-app.png)

1. <span data-ttu-id="b92a7-111">Нажмите **Зарегистрировать**.</span><span class="sxs-lookup"><span data-stu-id="b92a7-111">Select **Register**.</span></span> <span data-ttu-id="b92a7-112">На странице **учебника swift Graph** для iOS скопируйте значение ИД приложения **(клиента)** и сохраните его, оно потребуется вам на следующем этапе.</span><span class="sxs-lookup"><span data-stu-id="b92a7-112">On the **iOS Swift Graph Tutorial** page, copy the value of the **Application (client) ID** and save it, you will need it in the next step.</span></span>

    ![Снимок экрана: ИД нового приложения для регистрации](images/aad-application-id.png)

1. <span data-ttu-id="b92a7-114">Выберите **Проверка подлинности** в разделе **Управление**.</span><span class="sxs-lookup"><span data-stu-id="b92a7-114">Select **Authentication** under **Manage**.</span></span> <span data-ttu-id="b92a7-115">Выберите **"Добавить платформу",** а затем **iOS или macOS.**</span><span class="sxs-lookup"><span data-stu-id="b92a7-115">Select **Add a platform**, then **iOS / macOS**.</span></span>

1. <span data-ttu-id="b92a7-116">Введите ИД пакета приложения и выберите **"Настроить",** а затем выберите **"Готово".**</span><span class="sxs-lookup"><span data-stu-id="b92a7-116">Enter your app's Bundle ID and select **Configure**, then select **Done**.</span></span>
