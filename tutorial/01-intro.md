<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="096c4-101">В этом руководстве рассказывается, как создать приложение для iOS с помощью Swift, которое использует API Microsoft Graph для получения сведений календаря для пользователя.</span><span class="sxs-lookup"><span data-stu-id="096c4-101">This tutorial teaches you how to build an iOS app with Swift that uses the Microsoft Graph API to retrieve calendar information for a user.</span></span>

> [!TIP]
> <span data-ttu-id="096c4-102">Если вы предпочитаете просто скачать завершенный учебник, вы можете скачать или клонировать [репозиторий GitHub.](https://github.com/microsoftgraph/msgraph-training-ios-swift)</span><span class="sxs-lookup"><span data-stu-id="096c4-102">If you prefer to just download the completed tutorial, you can download or clone the [GitHub repository](https://github.com/microsoftgraph/msgraph-training-ios-swift).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="096c4-103">Предварительные условия</span><span class="sxs-lookup"><span data-stu-id="096c4-103">Prerequisites</span></span>

<span data-ttu-id="096c4-104">Прежде чем приступить к этому учебнику, на компьютере разработчика должны быть установлены следующие следующую установку.</span><span class="sxs-lookup"><span data-stu-id="096c4-104">Before you start this tutorial, you should have the following installed on your development machine.</span></span>

- [<span data-ttu-id="096c4-105">Xcode</span><span class="sxs-lookup"><span data-stu-id="096c4-105">Xcode</span></span>](https://developer.apple.com/xcode/)
- [<span data-ttu-id="096c4-106">CocoaPods</span><span class="sxs-lookup"><span data-stu-id="096c4-106">CocoaPods</span></span>](https://cocoapods.org)

<span data-ttu-id="096c4-107">У вас также должна быть личная учетная запись Майкрософт с почтовым ящиком на Outlook.com или учетная запись Майкрософт для работы или учебного заведения.</span><span class="sxs-lookup"><span data-stu-id="096c4-107">You should also have either a personal Microsoft account with a mailbox on Outlook.com, or a Microsoft work or school account.</span></span> <span data-ttu-id="096c4-108">Если у вас нет учетной записи Майкрософт, существует несколько вариантов получения бесплатной учетной записи:</span><span class="sxs-lookup"><span data-stu-id="096c4-108">If you don't have a Microsoft account, there are a couple of options to get a free account:</span></span>

- <span data-ttu-id="096c4-109">Вы можете [зарегистрироваться для новой личной учетной записи Майкрософт.](https://signup.live.com/signup?wa=wsignin1.0&rpsnv=12&ct=1454618383&rver=6.4.6456.0&wp=MBI_SSL_SHARED&wreply=https://mail.live.com/default.aspx&id=64855&cbcxt=mai&bk=1454618383&uiflavor=web&uaid=b213a65b4fdc484382b6622b3ecaa547&mkt=E-US&lc=1033&lic=1)</span><span class="sxs-lookup"><span data-stu-id="096c4-109">You can [sign up for a new personal Microsoft account](https://signup.live.com/signup?wa=wsignin1.0&rpsnv=12&ct=1454618383&rver=6.4.6456.0&wp=MBI_SSL_SHARED&wreply=https://mail.live.com/default.aspx&id=64855&cbcxt=mai&bk=1454618383&uiflavor=web&uaid=b213a65b4fdc484382b6622b3ecaa547&mkt=E-US&lc=1033&lic=1).</span></span>
- <span data-ttu-id="096c4-110">Вы можете зарегистрироваться в программе для разработчиков [Office 365,](https://developer.microsoft.com/office/dev-program) чтобы получить бесплатную подписку на Office 365.</span><span class="sxs-lookup"><span data-stu-id="096c4-110">You can [sign up for the Office 365 Developer Program](https://developer.microsoft.com/office/dev-program) to get a free Office 365 subscription.</span></span>

> [!NOTE]
> <span data-ttu-id="096c4-111">Это руководство было написано с использованием Xcode версии 12.3 и CocoaPods версии 1.10.1. Действия в этом руководстве могут работать с другими версиями, но не были протестированы.</span><span class="sxs-lookup"><span data-stu-id="096c4-111">This tutorial was written using Xcode version 12.3 and CocoaPods version 1.10.1 The steps in this guide may work with other versions, but that has not been tested.</span></span>

## <a name="feedback"></a><span data-ttu-id="096c4-112">Отзывы</span><span class="sxs-lookup"><span data-stu-id="096c4-112">Feedback</span></span>

<span data-ttu-id="096c4-113">Поделитесь с этим учебником отзывами в [репозитории GitHub.](https://github.com/microsoftgraph/msgraph-training-ios-swift)</span><span class="sxs-lookup"><span data-stu-id="096c4-113">Please provide any feedback on this tutorial in the [GitHub repository](https://github.com/microsoftgraph/msgraph-training-ios-swift).</span></span>
