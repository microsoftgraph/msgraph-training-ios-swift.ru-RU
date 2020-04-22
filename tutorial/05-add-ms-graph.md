<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="4d270-101">В этом упражнении вы добавите Microsoft Graph в приложение.</span><span class="sxs-lookup"><span data-stu-id="4d270-101">In this exercise you will incorporate the Microsoft Graph into the application.</span></span> <span data-ttu-id="4d270-102">Для этого приложения вы будете использовать [пакет SDK Microsoft Graph для задания](https://github.com/microsoftgraph/msgraph-sdk-objc) с, чтобы совершать вызовы в Microsoft Graph.</span><span class="sxs-lookup"><span data-stu-id="4d270-102">For this application, you will use the [Microsoft Graph SDK for Objective C](https://github.com/microsoftgraph/msgraph-sdk-objc) to make calls to Microsoft Graph.</span></span>

## <a name="get-calendar-events-from-outlook"></a><span data-ttu-id="4d270-103">Получение событий календаря из Outlook</span><span class="sxs-lookup"><span data-stu-id="4d270-103">Get calendar events from Outlook</span></span>

<span data-ttu-id="4d270-104">В этом разделе описывается расширение `GraphManager` класса для добавления функции, позволяющей получить события и обновление `CalendarViewController` , чтобы использовать эти новые функции.</span><span class="sxs-lookup"><span data-stu-id="4d270-104">In this section you will extend the `GraphManager` class to add a function to get the user's events and update `CalendarViewController` to use these new functions.</span></span>

1. <span data-ttu-id="4d270-105">Откройте **графманажер. SWIFT** и добавьте в `GraphManager` класс следующий метод.</span><span class="sxs-lookup"><span data-stu-id="4d270-105">Open **GraphManager.swift** and add the following method to the `GraphManager` class.</span></span>

    ```Swift
    public func getEvents(completion: @escaping(Data?, Error?) -> Void) {
        // GET /me/events?$select='subject,organizer,start,end'$orderby=createdDateTime DESC
        // Only return these fields in results
        let select = "$select=subject,organizer,start,end"
        // Sort results by when they were created, newest first
        let orderBy = "$orderby=createdDateTime+DESC"
        let eventsRequest = NSMutableURLRequest(url: URL(string: "\(MSGraphBaseURL)/me/events?\(select)&\(orderBy)")!)
        let eventsDataTask = MSURLSessionDataTask(request: eventsRequest, client: self.client, completion: {
            (data: Data?, response: URLResponse?, graphError: Error?) in
            guard let eventsData = data, graphError == nil else {
                completion(nil, graphError)
                return
            }

            // TEMPORARY
            completion(eventsData, nil)
        })

        // Execute the request
        eventsDataTask?.execute()
    }
    ```

    > [!NOTE]
    > <span data-ttu-id="4d270-106">Рассмотрите, какие действия `getEvents` выполняет код.</span><span class="sxs-lookup"><span data-stu-id="4d270-106">Consider what the code in `getEvents` is doing.</span></span>
    >
    > - <span data-ttu-id="4d270-107">URL-адрес, который будет вызываться — это `/v1.0/me/events`.</span><span class="sxs-lookup"><span data-stu-id="4d270-107">The URL that will be called is `/v1.0/me/events`.</span></span>
    > - <span data-ttu-id="4d270-108">Параметр `select` запроса позволяет ограничить поля, возвращаемые для каждого события, только теми, которые будут использоваться приложением.</span><span class="sxs-lookup"><span data-stu-id="4d270-108">The `select` query parameter limits the fields returned for each events to just those the app will actually use.</span></span>
    > - <span data-ttu-id="4d270-109">Параметр `orderby` запроса сортирует результаты по дате и времени создания, начиная с самого последнего элемента.</span><span class="sxs-lookup"><span data-stu-id="4d270-109">The `orderby` query parameter sorts the results by the date and time they were created, with the most recent item being first.</span></span>

1. <span data-ttu-id="4d270-110">Откройте **календарвиевконтроллер. SWIFT** и замените все его содержимое приведенным ниже кодом.</span><span class="sxs-lookup"><span data-stu-id="4d270-110">Open **CalendarViewController.swift** and replace its entire contents with the following code.</span></span>

    ```Swift
    import UIKit
    import MSGraphClientModels

    class CalendarViewController: UIViewController {

        @IBOutlet var calendarJSON: UITextView!

        private let spinner = SpinnerViewController()

        override func viewDidLoad() {
            super.viewDidLoad()

            // Do any additional setup after loading the view.
            self.spinner.start(container: self)

            GraphManager.instance.getEvents {
                (data: Data?, error: Error?) in
                DispatchQueue.main.async {
                    self.spinner.stop()

                    guard let eventsData = data, error == nil else {
                        self.calendarJSON.text = error.debugDescription
                        return
                    }

                    let jsonString = String(data: eventsData, encoding: .utf8)
                    self.calendarJSON.text = jsonString
                    self.calendarJSON.sizeToFit()
                }
            }
        }
    }
    ```

<span data-ttu-id="4d270-111">Теперь вы можете запустить приложение, войти и нажать в меню элемент Навигация по **календарю** .</span><span class="sxs-lookup"><span data-stu-id="4d270-111">You can now run the app, sign in, and tap the **Calendar** navigation item in the menu.</span></span> <span data-ttu-id="4d270-112">В приложении должен появиться дамп JSON событий.</span><span class="sxs-lookup"><span data-stu-id="4d270-112">You should see a JSON dump of the events in the app.</span></span>

## <a name="display-the-results"></a><span data-ttu-id="4d270-113">Отображение результатов</span><span class="sxs-lookup"><span data-stu-id="4d270-113">Display the results</span></span>

<span data-ttu-id="4d270-114">Теперь вы можете заменить дамп JSON на какой-то способ отобразить результаты в удобном для пользователя виде.</span><span class="sxs-lookup"><span data-stu-id="4d270-114">Now you can replace the JSON dump with something to display the results in a user-friendly manner.</span></span> <span data-ttu-id="4d270-115">В этом разделе будет изменена `getEvents` функция, возвращающая строго типизированные объекты, и изменено `CalendarViewController` , чтобы использовать представление таблицы для отображения событий.</span><span class="sxs-lookup"><span data-stu-id="4d270-115">In this section, you will modify the `getEvents` function to return strongly-typed objects, and modify `CalendarViewController` to use a table view to render the events.</span></span>

1. <span data-ttu-id="4d270-116">Откройте **графманажер. SWIFT**.</span><span class="sxs-lookup"><span data-stu-id="4d270-116">Open **GraphManager.swift**.</span></span> <span data-ttu-id="4d270-117">Замените имеющуюся функцию `getEvents` указанным ниже кодом.</span><span class="sxs-lookup"><span data-stu-id="4d270-117">Replace the existing `getEvents` function with the following.</span></span>

    :::code language="swift" source="../demo/GraphTutorial/GraphTutorial/GraphManager.swift" id="GetEventsSnippet" highlight="1,17-38":::

1. <span data-ttu-id="4d270-118">Создайте новый файл **класса Touch Cocoa** в проекте **графтуториал** с именем `CalendarTableViewCell.swift`.</span><span class="sxs-lookup"><span data-stu-id="4d270-118">Create a new **Cocoa Touch Class** file in the **GraphTutorial** project named `CalendarTableViewCell.swift`.</span></span> <span data-ttu-id="4d270-119">Выберите **уитаблевиевцелл** в **подклассе** поля.</span><span class="sxs-lookup"><span data-stu-id="4d270-119">Choose **UITableViewCell** in the **Subclass of** field.</span></span>

1. <span data-ttu-id="4d270-120">Откройте **календартаблевиевцелл. SWIFT** и добавьте в `CalendarTableViewCell` класс приведенный ниже код.</span><span class="sxs-lookup"><span data-stu-id="4d270-120">Open **CalendarTableViewCell.swift** and add the following code to the `CalendarTableViewCell` class.</span></span>

    :::code language="swift" source="../demo/GraphTutorial/GraphTutorial/CalendarTableViewCell.swift" id="PropertiesSnippet":::

1. <span data-ttu-id="4d270-121">Откройте файл **Main. Storyboard** и перейдите к **сцене календаря**.</span><span class="sxs-lookup"><span data-stu-id="4d270-121">Open **Main.storyboard** and locate the **Calendar Scene**.</span></span> <span data-ttu-id="4d270-122">Выберите **представление** в **сцене календаря** и удалите его.</span><span class="sxs-lookup"><span data-stu-id="4d270-122">Select the **View** in the **Calendar Scene** and delete it.</span></span>

    ![Снимок экрана с представлением в сцене календаря](./images/view-in-calendar-scene.png)

1. <span data-ttu-id="4d270-124">Добавьте **представление таблицы** из **библиотеки** в **сцену в календаре**.</span><span class="sxs-lookup"><span data-stu-id="4d270-124">Add a **Table View** from the **Library** to the **Calendar Scene**.</span></span>
1. <span data-ttu-id="4d270-125">Выберите представление таблицы, а затем выберите **инспектор атрибутов**.</span><span class="sxs-lookup"><span data-stu-id="4d270-125">Select the table view, then select the **Attributes Inspector**.</span></span> <span data-ttu-id="4d270-126">Задайте для **ячеек прототипа** значение **1**.</span><span class="sxs-lookup"><span data-stu-id="4d270-126">Set **Prototype Cells** to **1**.</span></span>
1. <span data-ttu-id="4d270-127">Используйте **библиотеку** , чтобы добавить три **метки** к ячейке прототипа.</span><span class="sxs-lookup"><span data-stu-id="4d270-127">Use the **Library** to add three **Labels** to the prototype cell.</span></span>
1. <span data-ttu-id="4d270-128">Выберите ячейку прототип, а затем выберите **инспектор удостоверений**.</span><span class="sxs-lookup"><span data-stu-id="4d270-128">Select the prototype cell, then select the **Identity Inspector**.</span></span> <span data-ttu-id="4d270-129">Замените **Class** на **календартаблевиевцелл**.</span><span class="sxs-lookup"><span data-stu-id="4d270-129">Change **Class** to **CalendarTableViewCell**.</span></span>
1. <span data-ttu-id="4d270-130">Выберите **инспектор атрибутов** и **идентификатор** набора `EventCell`.</span><span class="sxs-lookup"><span data-stu-id="4d270-130">Select the **Attributes Inspector** and set **Identifier** to `EventCell`.</span></span>
1. <span data-ttu-id="4d270-131">Выбрав **евентцелл** , выберите **инспектор подключений** и подключение `durationLabel`, `organizerLabel`а `subjectLabel` также метки, добавленные в ячейку в раскадровке.</span><span class="sxs-lookup"><span data-stu-id="4d270-131">With the **EventCell** selected, select the **Connections Inspector** and connect `durationLabel`, `organizerLabel`, and `subjectLabel` to the labels you added to the cell on the storyboard.</span></span>
1. <span data-ttu-id="4d270-132">Задайте свойства и ограничения для этих трех меток, как показано ниже.</span><span class="sxs-lookup"><span data-stu-id="4d270-132">Set the properties and constraints on the three labels as follows.</span></span>

    - <span data-ttu-id="4d270-133">**Метка темы**</span><span class="sxs-lookup"><span data-stu-id="4d270-133">**Subject Label**</span></span>
        - <span data-ttu-id="4d270-134">Добавить ограничение: начальное пространство в представлении содержимого начальное поле, значение: 0</span><span class="sxs-lookup"><span data-stu-id="4d270-134">Add constraint: Leading space to Content View Leading Margin, value: 0</span></span>
        - <span data-ttu-id="4d270-135">Add Constraint: замыкающий пробел для конечного поля представления содержимого, значение: 0</span><span class="sxs-lookup"><span data-stu-id="4d270-135">Add constraint: Trailing space to Content View Trailing Margin, value: 0</span></span>
        - <span data-ttu-id="4d270-136">Добавить ограничение: Верхняя область для представления контента верхнее поле, значение: 0</span><span class="sxs-lookup"><span data-stu-id="4d270-136">Add constraint: Top space to Content View Top Margin, value: 0</span></span>
    - <span data-ttu-id="4d270-137">**Метка "Организатор"**</span><span class="sxs-lookup"><span data-stu-id="4d270-137">**Organizer Label**</span></span>
        - <span data-ttu-id="4d270-138">Шрифт: System 12,0</span><span class="sxs-lookup"><span data-stu-id="4d270-138">Font: System 12.0</span></span>
        - <span data-ttu-id="4d270-139">Добавить ограничение: начальное пространство в представлении содержимого начальное поле, значение: 0</span><span class="sxs-lookup"><span data-stu-id="4d270-139">Add constraint: Leading space to Content View Leading Margin, value: 0</span></span>
        - <span data-ttu-id="4d270-140">Add Constraint: замыкающий пробел для конечного поля представления содержимого, значение: 0</span><span class="sxs-lookup"><span data-stu-id="4d270-140">Add constraint: Trailing space to Content View Trailing Margin, value: 0</span></span>
        - <span data-ttu-id="4d270-141">Добавление ограничения: Верхняя область для подписи темы внизу, значение: Standard</span><span class="sxs-lookup"><span data-stu-id="4d270-141">Add constraint: Top space to Subject Label Bottom, value: Standard</span></span>
    - <span data-ttu-id="4d270-142">**Метка длительности**</span><span class="sxs-lookup"><span data-stu-id="4d270-142">**Duration Label**</span></span>
        - <span data-ttu-id="4d270-143">Шрифт: System 12,0</span><span class="sxs-lookup"><span data-stu-id="4d270-143">Font: System 12.0</span></span>
        - <span data-ttu-id="4d270-144">Цвет: темно-серый цвет</span><span class="sxs-lookup"><span data-stu-id="4d270-144">Color: Dark Gray Color</span></span>
        - <span data-ttu-id="4d270-145">Добавить ограничение: начальное пространство в представлении содержимого начальное поле, значение: 0</span><span class="sxs-lookup"><span data-stu-id="4d270-145">Add constraint: Leading space to Content View Leading Margin, value: 0</span></span>
        - <span data-ttu-id="4d270-146">Add Constraint: замыкающий пробел для конечного поля представления содержимого, значение: 0</span><span class="sxs-lookup"><span data-stu-id="4d270-146">Add constraint: Trailing space to Content View Trailing Margin, value: 0</span></span>
        - <span data-ttu-id="4d270-147">Добавить ограничение: Верхняя область для метки организатора внизу, значение: Standard</span><span class="sxs-lookup"><span data-stu-id="4d270-147">Add constraint: Top space to Organizer Label Bottom, value: Standard</span></span>
        - <span data-ttu-id="4d270-148">Добавить ограничение: нижнее пространство для представления содержимого нижнее поле, значение: 8</span><span class="sxs-lookup"><span data-stu-id="4d270-148">Add constraint: Bottom space to Content View Bottom Margin, value: 8</span></span>

    ![Снимок экрана с макетом ячеек прототипа](./images/prototype-cell-layout.png)

1. <span data-ttu-id="4d270-150">Откройте **календарвиевконтроллер. SWIFT** и замените его содержимое приведенным ниже кодом.</span><span class="sxs-lookup"><span data-stu-id="4d270-150">Open **CalendarViewController.swift** and replace its contents with the following code.</span></span>

    :::code language="swift" source="../demo/GraphTutorial/GraphTutorial/CalendarViewController.swift" id="CalendarViewSnippet":::

1. <span data-ttu-id="4d270-151">Запустите приложение и войдите в систему, а затем коснитесь вкладки **Календарь** . Вы должны увидеть список событий.</span><span class="sxs-lookup"><span data-stu-id="4d270-151">Run the app, sign in, and tap the **Calendar** tab. You should see the list of events.</span></span>

    ![Снимок экрана с таблицей событий](./images/calendar-list.png)
