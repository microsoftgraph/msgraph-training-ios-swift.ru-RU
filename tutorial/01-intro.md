<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="73d07-101">В этом руководстве рассказывается, как создать собственное приложение, которое использует API Microsoft Graph для получения сведений о календаре для пользователя.</span><span class="sxs-lookup"><span data-stu-id="73d07-101">This tutorial teaches you how to build an React Native app that uses the Microsoft Graph API to retrieve calendar information for a user.</span></span>

> [!TIP]
> <span data-ttu-id="73d07-102">Если вы предпочитаете просто скачать заполненный учебник, вы можете скачать или клонировать [репозиторий GitHub](https://github.com/microsoftgraph/msgraph-training-ios-swift).</span><span class="sxs-lookup"><span data-stu-id="73d07-102">If you prefer to just download the completed tutorial, you can download or clone the [GitHub repository](https://github.com/microsoftgraph/msgraph-training-ios-swift).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="73d07-103">Необходимые компоненты</span><span class="sxs-lookup"><span data-stu-id="73d07-103">Prerequisites</span></span>

<span data-ttu-id="73d07-104">Прежде чем приступить к работе с этим руководством, на вашем компьютере для разработки должны быть установлены следующие компоненты.</span><span class="sxs-lookup"><span data-stu-id="73d07-104">Before you start this tutorial, you should have the following installed on your development machine.</span></span>

- [<span data-ttu-id="73d07-105">Xcode</span><span class="sxs-lookup"><span data-stu-id="73d07-105">Xcode</span></span>](https://developer.apple.com/xcode/)
- [<span data-ttu-id="73d07-106">CocoaPods</span><span class="sxs-lookup"><span data-stu-id="73d07-106">CocoaPods</span></span>](https://cocoapods.org)

<span data-ttu-id="73d07-107">Кроме того, у вас также должна быть личная учетная запись Майкрософт с почтовым ящиком на Outlook.com или рабочей или учебной учетной записью Майкрософт.</span><span class="sxs-lookup"><span data-stu-id="73d07-107">You should also have either a personal Microsoft account with a mailbox on Outlook.com, or a Microsoft work or school account.</span></span> <span data-ttu-id="73d07-108">Если у вас нет учетной записи Майкрософт, у вас есть несколько вариантов для получения бесплатной учетной записи:</span><span class="sxs-lookup"><span data-stu-id="73d07-108">If you don't have a Microsoft account, there are a couple of options to get a free account:</span></span>

- <span data-ttu-id="73d07-109">Вы можете [зарегистрироваться для создания новой личной учетной записи Майкрософт](https://signup.live.com/signup?wa=wsignin1.0&rpsnv=12&ct=1454618383&rver=6.4.6456.0&wp=MBI_SSL_SHARED&wreply=https://mail.live.com/default.aspx&id=64855&cbcxt=mai&bk=1454618383&uiflavor=web&uaid=b213a65b4fdc484382b6622b3ecaa547&mkt=E-US&lc=1033&lic=1).</span><span class="sxs-lookup"><span data-stu-id="73d07-109">You can [sign up for a new personal Microsoft account](https://signup.live.com/signup?wa=wsignin1.0&rpsnv=12&ct=1454618383&rver=6.4.6456.0&wp=MBI_SSL_SHARED&wreply=https://mail.live.com/default.aspx&id=64855&cbcxt=mai&bk=1454618383&uiflavor=web&uaid=b213a65b4fdc484382b6622b3ecaa547&mkt=E-US&lc=1033&lic=1).</span></span>
- <span data-ttu-id="73d07-110">Вы можете [зарегистрироваться в программе для разработчиков office 365](https://developer.microsoft.com/office/dev-program) , чтобы получить бесплатную подписку на Office 365.</span><span class="sxs-lookup"><span data-stu-id="73d07-110">You can [sign up for the Office 365 Developer Program](https://developer.microsoft.com/office/dev-program) to get a free Office 365 subscription.</span></span>

> [!NOTE]
> <span data-ttu-id="73d07-111">Это руководство было написано с помощью Xcode версии 11,4 и CocoaPods версии 1.9.1 действия, описанные в этом руководстве, могут работать с другими версиями, но не тестировались.</span><span class="sxs-lookup"><span data-stu-id="73d07-111">This tutorial was written using Xcode version 11.4 and CocoaPods version 1.9.1 The steps in this guide may work with other versions, but that has not been tested.</span></span>

## <a name="feedback"></a><span data-ttu-id="73d07-112">Отзывы</span><span class="sxs-lookup"><span data-stu-id="73d07-112">Feedback</span></span>

<span data-ttu-id="73d07-113">Сообщите о нем в [репозиторий GitHub](https://github.com/microsoftgraph/msgraph-training-ios-swift).</span><span class="sxs-lookup"><span data-stu-id="73d07-113">Please provide any feedback on this tutorial in the [GitHub repository](https://github.com/microsoftgraph/msgraph-training-ios-swift).</span></span>
