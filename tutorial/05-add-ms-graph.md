<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="c4152-101">В этом упражнении вы включаете Microsoft Graph в приложение.</span><span class="sxs-lookup"><span data-stu-id="c4152-101">In this exercise you will incorporate the Microsoft Graph into the application.</span></span> <span data-ttu-id="c4152-102">Для этого приложения для вызова Microsoft Graph используется [SDK Microsoft Graph](https://github.com/microsoftgraph/msgraph-sdk-objc) для Objective C.</span><span class="sxs-lookup"><span data-stu-id="c4152-102">For this application, you will use the [Microsoft Graph SDK for Objective C](https://github.com/microsoftgraph/msgraph-sdk-objc) to make calls to Microsoft Graph.</span></span>

## <a name="get-calendar-events-from-outlook"></a><span data-ttu-id="c4152-103">Получить события календаря из Outlook</span><span class="sxs-lookup"><span data-stu-id="c4152-103">Get calendar events from Outlook</span></span>

<span data-ttu-id="c4152-104">В этом разделе мы расширим класс, чтобы добавить функцию для получения событий пользователя за текущую неделю, и обновим, чтобы `GraphManager` `CalendarViewController` использовать эти новые функции.</span><span class="sxs-lookup"><span data-stu-id="c4152-104">In this section you will extend the `GraphManager` class to add a function to get the user's events for the current week and update `CalendarViewController` to use these new functions.</span></span>

1. <span data-ttu-id="c4152-105">Откройте **GraphManager.swift** и добавьте в класс следующий `GraphManager` метод.</span><span class="sxs-lookup"><span data-stu-id="c4152-105">Open **GraphManager.swift** and add the following method to the `GraphManager` class.</span></span>

    ```Swift
    public func getCalendarView(viewStart: String,
                                viewEnd: String,
                                completion: @escaping(Data?, Error?) -> Void) {
        // GET /me/calendarview
        // Set start and end of the view
        let start = "startDateTime=\(viewStart)"
        let end = "endDateTime=\(viewEnd)"
        // Only return these fields in results
        let select = "$select=subject,organizer,start,end"
        // Sort results by when they were created, newest first
        let orderBy = "$orderby=start/dateTime"
        // Request at most 25 results
        let top = "$top=25"

        let eventsRequest = NSMutableURLRequest(url: URL(string: "\(MSGraphBaseURL)/me/calendarview?\(start)&\(end)&\(select)&\(orderBy)&\(top)")!)

        // Add the Prefer: outlook.timezone header to get start and end times
        // in user's time zone
        eventsRequest.addValue("outlook.timezone=\"\(self.userTimeZone)\"", forHTTPHeaderField: "Prefer")

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
    > <span data-ttu-id="c4152-106">Подумайте, что делает `getCalendarView` код.</span><span class="sxs-lookup"><span data-stu-id="c4152-106">Consider what the code in `getCalendarView` is doing.</span></span>
    >
    > - <span data-ttu-id="c4152-107">Будет вызван URL-адрес `/v1.0/me/calendarview` .</span><span class="sxs-lookup"><span data-stu-id="c4152-107">The URL that will be called is `/v1.0/me/calendarview`.</span></span>
    >   - <span data-ttu-id="c4152-108">Параметры `startDateTime` `endDateTime` и параметры запроса определяют начало и конец представления календаря.</span><span class="sxs-lookup"><span data-stu-id="c4152-108">The `startDateTime` and `endDateTime` query parameters define the start and end of the calendar view.</span></span>
    >   - <span data-ttu-id="c4152-109">Параметр запроса ограничивает поля, возвращаемые для каждого события, только теми, которые `select` будут фактически использовать представление.</span><span class="sxs-lookup"><span data-stu-id="c4152-109">The `select` query parameter limits the fields returned for each events to just those the view will actually use.</span></span>
    >   - <span data-ttu-id="c4152-110">Параметр `orderby` запроса сортировать результаты по времени начала.</span><span class="sxs-lookup"><span data-stu-id="c4152-110">The `orderby` query parameter sorts the results by start time.</span></span>
    >   - <span data-ttu-id="c4152-111">Параметр `top` запроса запрашивает 25 результатов на страницу.</span><span class="sxs-lookup"><span data-stu-id="c4152-111">The `top` query parameter requests 25 results per page.</span></span>
    >   - <span data-ttu-id="c4152-112">Заглавная запись заставляет Microsoft Graph возвращать время начала и окончания каждого события в `Prefer: outlook.timezone` часовом поясе пользователя.</span><span class="sxs-lookup"><span data-stu-id="c4152-112">the `Prefer: outlook.timezone` header causes the Microsoft Graph to return the start and end times of each event in the user's time zone.</span></span>

1. <span data-ttu-id="c4152-113">Создайте новый **swift-файл** в **проекте GraphTutorial** **с именем GraphToIana.swift.**</span><span class="sxs-lookup"><span data-stu-id="c4152-113">Create a new **Swift File** in the **GraphTutorial** project named **GraphToIana.swift**.</span></span> <span data-ttu-id="c4152-114">Добавьте указанный ниже код в файл.</span><span class="sxs-lookup"><span data-stu-id="c4152-114">Add the following code to the file.</span></span>

    :::code language="swift" source="../demo/GraphTutorial/GraphTutorial/GraphToIana.swift" id="GraphToIanaSnippet":::

    <span data-ttu-id="c4152-115">Это простой поиск идентификатора часового пояса IANA на основе имени часового пояса, возвращаемого Microsoft Graph.</span><span class="sxs-lookup"><span data-stu-id="c4152-115">This does a simple lookup to find an IANA time zone identifier based on the time zone name returned by Microsoft Graph.</span></span>

1. <span data-ttu-id="c4152-116">Откройте **CalendarViewController.swift** и замените его все содержимое следующим кодом.</span><span class="sxs-lookup"><span data-stu-id="c4152-116">Open **CalendarViewController.swift** and replace its entire contents with the following code.</span></span>

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

            // Calculate the start and end of the current week
            let timeZone = GraphToIana.getIanaIdentifier(graphIdentifer: GraphManager.instance.userTimeZone)
            let now = Date()
            var calendar = Calendar(identifier: .gregorian)
            calendar.timeZone = TimeZone(identifier: timeZone)!

            let startOfWeek = calendar.dateComponents([.calendar, .yearForWeekOfYear, .weekOfYear], from: now).date!
            let endOfWeek = calendar.date(byAdding: .day, value: 7, to: startOfWeek)!

            // Convert start and end to ISO 8601 strings
            let isoFormatter = ISO8601DateFormatter()
            let viewStart = isoFormatter.string(from: startOfWeek)
            let viewEnd = isoFormatter.string(from: endOfWeek)

            GraphManager.instance.getCalendarView(viewStart: viewStart, viewEnd: viewEnd) {
                (data: Data?, error: Error?) in
                DispatchQueue.main.async {
                    self.spinner.stop()

                    // TEMPORARY
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

<span data-ttu-id="c4152-117">Теперь вы можете запустить приложение, войти и к пункту навигации **"Календарь"** в меню.</span><span class="sxs-lookup"><span data-stu-id="c4152-117">You can now run the app, sign in, and tap the **Calendar** navigation item in the menu.</span></span> <span data-ttu-id="c4152-118">В приложении должен отыменить дамп событий в JSON.</span><span class="sxs-lookup"><span data-stu-id="c4152-118">You should see a JSON dump of the events in the app.</span></span>

## <a name="display-the-results"></a><span data-ttu-id="c4152-119">Отображение результатов</span><span class="sxs-lookup"><span data-stu-id="c4152-119">Display the results</span></span>

<span data-ttu-id="c4152-120">Теперь вы можете заменить дамп JSON на что-то, чтобы отобразить результаты в удобном для пользователя виде.</span><span class="sxs-lookup"><span data-stu-id="c4152-120">Now you can replace the JSON dump with something to display the results in a user-friendly manner.</span></span> <span data-ttu-id="c4152-121">В этом разделе вы измените функцию, чтобы она возвращала строго типные объекты, и измените для отображения событий представление `getCalendarView` `CalendarViewController` таблицы.</span><span class="sxs-lookup"><span data-stu-id="c4152-121">In this section, you will modify the `getCalendarView` function to return strongly-typed objects, and modify `CalendarViewController` to use a table view to render the events.</span></span>

1. <span data-ttu-id="c4152-122">Откройте **GraphManager.swift.**</span><span class="sxs-lookup"><span data-stu-id="c4152-122">Open **GraphManager.swift**.</span></span> <span data-ttu-id="c4152-123">Замените имеющуюся функцию `getCalendarView` указанным ниже кодом.</span><span class="sxs-lookup"><span data-stu-id="c4152-123">Replace the existing `getCalendarView` function with the following.</span></span>

    :::code language="swift" source="../demo/GraphTutorial/GraphTutorial/GraphManager.swift" id="GetEventsSnippet" highlight="3,28-49":::

1. <span data-ttu-id="c4152-124">Создайте новый файл **класса Cocoa Touch в** **проекте GraphTutorial** с именем `CalendarTableViewController.swift` .</span><span class="sxs-lookup"><span data-stu-id="c4152-124">Create a new **Cocoa Touch Class** file in the **GraphTutorial** project named `CalendarTableViewController.swift`.</span></span> <span data-ttu-id="c4152-125">Выберите **UITableViewController** в **подклассе** поля.</span><span class="sxs-lookup"><span data-stu-id="c4152-125">Choose **UITableViewController** in the **Subclass of** field.</span></span>

1. <span data-ttu-id="c4152-126">Откройте **CalendarTableViewController.swift** и замените его содержимое на следующее.</span><span class="sxs-lookup"><span data-stu-id="c4152-126">Open **CalendarTableViewController.swift** and replace its contents with the following.</span></span>

    :::code language="swift" source="../demo/GraphTutorial/GraphTutorial/CalendarTableViewController.swift" id="CalendarTableViewControllerSnippet":::

1. <span data-ttu-id="c4152-127">Создайте новый файл **класса Cocoa Touch в** **проекте GraphTutorial** с именем `CalendarTableViewCell.swift` .</span><span class="sxs-lookup"><span data-stu-id="c4152-127">Create a new **Cocoa Touch Class** file in the **GraphTutorial** project named `CalendarTableViewCell.swift`.</span></span> <span data-ttu-id="c4152-128">Выберите **UITableViewCell** в **подклассе** поля.</span><span class="sxs-lookup"><span data-stu-id="c4152-128">Choose **UITableViewCell** in the **Subclass of** field.</span></span>

1. <span data-ttu-id="c4152-129">Откройте **CalendarTableViewCell.swift** и добавьте в класс следующие `CalendarTableViewCell` свойства.</span><span class="sxs-lookup"><span data-stu-id="c4152-129">Open **CalendarTableViewCell.swift** and add the following properties to the `CalendarTableViewCell` class.</span></span>

    :::code language="swift" source="../demo/GraphTutorial/GraphTutorial/CalendarTableViewCell.swift" id="PropertiesSnippet":::

1. <span data-ttu-id="c4152-130">Откройте **main.storyboard и** найдите **сцену календаря.**</span><span class="sxs-lookup"><span data-stu-id="c4152-130">Open **Main.storyboard** and locate the **Calendar Scene**.</span></span> <span data-ttu-id="c4152-131">Удалите представление прокрутки из корневого представления.</span><span class="sxs-lookup"><span data-stu-id="c4152-131">Delete the scroll view from the root view.</span></span>
1. <span data-ttu-id="c4152-132">С помощью **библиотеки добавьте** в **верхнюю** часть представления панели навигации.</span><span class="sxs-lookup"><span data-stu-id="c4152-132">Using the **Library**, add a **Navigation Bar** to the top of the view.</span></span>
1. <span data-ttu-id="c4152-133">Дважды щелкните **заголовок** на панели навигации и обновите его до `Calendar` .</span><span class="sxs-lookup"><span data-stu-id="c4152-133">Double-click the **Title** in the navigation bar and update it to `Calendar`.</span></span>
1. <span data-ttu-id="c4152-134">С помощью **библиотеки добавьте** элемент кнопки **панели** в правую часть панели навигации.</span><span class="sxs-lookup"><span data-stu-id="c4152-134">Using the **Library**, add a **Bar Button Item** to the right-hand side of the navigation bar.</span></span>
1. <span data-ttu-id="c4152-135">Выберите новую кнопку панели, а затем — **инспектор атрибутов.**</span><span class="sxs-lookup"><span data-stu-id="c4152-135">Select the new bar button, then select the **Attributes Inspector**.</span></span> <span data-ttu-id="c4152-136">Измените **изображение** на **"плюс"**.</span><span class="sxs-lookup"><span data-stu-id="c4152-136">Change **Image** to **plus**.</span></span>
1. <span data-ttu-id="c4152-137">Добавьте представление **контейнера** из **библиотеки в** представление под панели навигации.</span><span class="sxs-lookup"><span data-stu-id="c4152-137">Add a **Container View** from the **Library** to the view under the navigation bar.</span></span> <span data-ttu-id="c4152-138">Измейте представление контейнера, чтобы занять все оставшееся место в представлении.</span><span class="sxs-lookup"><span data-stu-id="c4152-138">Resize the container view to take all of the remaining space in the view.</span></span>
1. <span data-ttu-id="c4152-139">Установите ограничения для панели навигации и представления контейнера следующим образом.</span><span class="sxs-lookup"><span data-stu-id="c4152-139">Set constraints on the navigation bar and container view as follows.</span></span>

    - <span data-ttu-id="c4152-140">**Панель навигации**</span><span class="sxs-lookup"><span data-stu-id="c4152-140">**Navigation Bar**</span></span>
        - <span data-ttu-id="c4152-141">Добавить ограничение: высота, значение: 44</span><span class="sxs-lookup"><span data-stu-id="c4152-141">Add constraint: Height, value: 44</span></span>
        - <span data-ttu-id="c4152-142">Add constraint: Leading space to Safe Area, value: 0</span><span class="sxs-lookup"><span data-stu-id="c4152-142">Add constraint: Leading space to Safe Area, value: 0</span></span>
        - <span data-ttu-id="c4152-143">Добавление ограничения: после пробела в безопасную область, значение: 0</span><span class="sxs-lookup"><span data-stu-id="c4152-143">Add constraint: Trailing space to Safe Area, value: 0</span></span>
        - <span data-ttu-id="c4152-144">Add constraint: Top space to Safe Area, value: 0</span><span class="sxs-lookup"><span data-stu-id="c4152-144">Add constraint: Top space to Safe Area, value: 0</span></span>
    - <span data-ttu-id="c4152-145">**Представление контейнера**</span><span class="sxs-lookup"><span data-stu-id="c4152-145">**Container View**</span></span>
        - <span data-ttu-id="c4152-146">Add constraint: Leading space to Safe Area, value: 0</span><span class="sxs-lookup"><span data-stu-id="c4152-146">Add constraint: Leading space to Safe Area, value: 0</span></span>
        - <span data-ttu-id="c4152-147">Добавление ограничения: после пробела в безопасную область, значение: 0</span><span class="sxs-lookup"><span data-stu-id="c4152-147">Add constraint: Trailing space to Safe Area, value: 0</span></span>
        - <span data-ttu-id="c4152-148">Добавление ограничения: верхнее место на панели навигации снизу, значение: 0</span><span class="sxs-lookup"><span data-stu-id="c4152-148">Add constraint: Top space to Navigation Bar Bottom, value: 0</span></span>
        - <span data-ttu-id="c4152-149">Добавление ограничения: нижнее пространство в безопасной области, значение: 0</span><span class="sxs-lookup"><span data-stu-id="c4152-149">Add constraint: Bottom space to Safe Area, value: 0</span></span>

1. <span data-ttu-id="c4152-150">Найдите второй контроллер представления, добавленный в storyboard при добавлении представления контейнера.</span><span class="sxs-lookup"><span data-stu-id="c4152-150">Locate the second view controller added to the storyboard when you added the container view.</span></span> <span data-ttu-id="c4152-151">Он подключается к сцене **календаря** с помощью встраиваемой segue.</span><span class="sxs-lookup"><span data-stu-id="c4152-151">It is connected to the **Calendar Scene** by an embed segue.</span></span> <span data-ttu-id="c4152-152">Выберите этот контроллер и используйте **identity Inspector,** чтобы изменить **класс** на **CalendarTableViewController.**</span><span class="sxs-lookup"><span data-stu-id="c4152-152">Select this controller and use the **Identity Inspector** to change **Class** to **CalendarTableViewController**.</span></span>
1. <span data-ttu-id="c4152-153">Удалите **представление** из **контроллера представления таблицы календаря.**</span><span class="sxs-lookup"><span data-stu-id="c4152-153">Delete the **View** from the **Calendar Table View Controller**.</span></span>
1. <span data-ttu-id="c4152-154">Добавьте представление **таблицы** из **библиотеки в** контроллер **представления таблицы календаря.**</span><span class="sxs-lookup"><span data-stu-id="c4152-154">Add a **Table View** from the **Library** to the **Calendar Table View Controller**.</span></span>
1. <span data-ttu-id="c4152-155">Выберите представление таблицы, а затем выберите **инспектор атрибутов.**</span><span class="sxs-lookup"><span data-stu-id="c4152-155">Select the table view, then select the **Attributes Inspector**.</span></span> <span data-ttu-id="c4152-156">Установите **для прототипа ячеек** **1**.</span><span class="sxs-lookup"><span data-stu-id="c4152-156">Set **Prototype Cells** to **1**.</span></span>
1. <span data-ttu-id="c4152-157">Перетащите нижний край прототипа ячейки, чтобы предоставить вам большую область для работы.</span><span class="sxs-lookup"><span data-stu-id="c4152-157">Drag the bottom edge of the prototype cell to give you a larger area to work with.</span></span>
1. <span data-ttu-id="c4152-158">Используйте **библиотеку,** чтобы добавить три **метки в** прототип ячейки.</span><span class="sxs-lookup"><span data-stu-id="c4152-158">Use the **Library** to add three **Labels** to the prototype cell.</span></span>
1. <span data-ttu-id="c4152-159">Выберите прототип ячейки, а затем выберите **identity Inspector.**</span><span class="sxs-lookup"><span data-stu-id="c4152-159">Select the prototype cell, then select the **Identity Inspector**.</span></span> <span data-ttu-id="c4152-160">Измените **класс** **на CalendarTableViewCell.**</span><span class="sxs-lookup"><span data-stu-id="c4152-160">Change **Class** to **CalendarTableViewCell**.</span></span>
1. <span data-ttu-id="c4152-161">Выберите **"Attributes Inspector"** (Инспектор атрибутов) и за **установите для идентификатора (Identifier)** `EventCell` (Идентификатор).</span><span class="sxs-lookup"><span data-stu-id="c4152-161">Select the **Attributes Inspector** and set **Identifier** to `EventCell`.</span></span>
1. <span data-ttu-id="c4152-162">Выбрав **EventCell,** выберите **Connections Inspector** и подключите его, а также метки, добавленные в ячейку `durationLabel` в `organizerLabel` `subjectLabel` storyboard.</span><span class="sxs-lookup"><span data-stu-id="c4152-162">With the **EventCell** selected, select the **Connections Inspector** and connect `durationLabel`, `organizerLabel`, and `subjectLabel` to the labels you added to the cell on the storyboard.</span></span>
1. <span data-ttu-id="c4152-163">Установите свойства и ограничения для трех меток следующим образом.</span><span class="sxs-lookup"><span data-stu-id="c4152-163">Set the properties and constraints on the three labels as follows.</span></span>

    - <span data-ttu-id="c4152-164">**Метка темы**</span><span class="sxs-lookup"><span data-stu-id="c4152-164">**Subject Label**</span></span>
        - <span data-ttu-id="c4152-165">Add constraint: Leading space to Content View Leading Margin, value: 0</span><span class="sxs-lookup"><span data-stu-id="c4152-165">Add constraint: Leading space to Content View Leading Margin, value: 0</span></span>
        - <span data-ttu-id="c4152-166">Добавление ограничения: в окн. место в поле "Точка с представлением содержимого", значение: 0</span><span class="sxs-lookup"><span data-stu-id="c4152-166">Add constraint: Trailing space to Content View Trailing Margin, value: 0</span></span>
        - <span data-ttu-id="c4152-167">Add constraint: Top space to Content View Top Margin, value: 0</span><span class="sxs-lookup"><span data-stu-id="c4152-167">Add constraint: Top space to Content View Top Margin, value: 0</span></span>
    - <span data-ttu-id="c4152-168">**Метка организатора**</span><span class="sxs-lookup"><span data-stu-id="c4152-168">**Organizer Label**</span></span>
        - <span data-ttu-id="c4152-169">Шрифт: System 12.0</span><span class="sxs-lookup"><span data-stu-id="c4152-169">Font: System 12.0</span></span>
        - <span data-ttu-id="c4152-170">Добавить ограничение: высота, значение: 15</span><span class="sxs-lookup"><span data-stu-id="c4152-170">Add constraint: Height, value: 15</span></span>
        - <span data-ttu-id="c4152-171">Add constraint: Leading space to Content View Leading Margin, value: 0</span><span class="sxs-lookup"><span data-stu-id="c4152-171">Add constraint: Leading space to Content View Leading Margin, value: 0</span></span>
        - <span data-ttu-id="c4152-172">Добавление ограничения: в окн. место в поле "Точка с представлением содержимого", значение: 0</span><span class="sxs-lookup"><span data-stu-id="c4152-172">Add constraint: Trailing space to Content View Trailing Margin, value: 0</span></span>
        - <span data-ttu-id="c4152-173">Add constraint: Top space to Subject Label Bottom, value: Standard</span><span class="sxs-lookup"><span data-stu-id="c4152-173">Add constraint: Top space to Subject Label Bottom, value: Standard</span></span>
    - <span data-ttu-id="c4152-174">**Метка длительности**</span><span class="sxs-lookup"><span data-stu-id="c4152-174">**Duration Label**</span></span>
        - <span data-ttu-id="c4152-175">Шрифт: System 12.0</span><span class="sxs-lookup"><span data-stu-id="c4152-175">Font: System 12.0</span></span>
        - <span data-ttu-id="c4152-176">Цвет: темно-серый цвет</span><span class="sxs-lookup"><span data-stu-id="c4152-176">Color: Dark Gray Color</span></span>
        - <span data-ttu-id="c4152-177">Добавить ограничение: высота, значение: 15</span><span class="sxs-lookup"><span data-stu-id="c4152-177">Add constraint: Height, value: 15</span></span>
        - <span data-ttu-id="c4152-178">Add constraint: Leading space to Content View Leading Margin, value: 0</span><span class="sxs-lookup"><span data-stu-id="c4152-178">Add constraint: Leading space to Content View Leading Margin, value: 0</span></span>
        - <span data-ttu-id="c4152-179">Добавление ограничения: в окн. место в поле "Точка с представлением содержимого", значение: 0</span><span class="sxs-lookup"><span data-stu-id="c4152-179">Add constraint: Trailing space to Content View Trailing Margin, value: 0</span></span>
        - <span data-ttu-id="c4152-180">Add constraint: Top space to Organizer Label Bottom, value: Standard</span><span class="sxs-lookup"><span data-stu-id="c4152-180">Add constraint: Top space to Organizer Label Bottom, value: Standard</span></span>
        - <span data-ttu-id="c4152-181">Добавление ограничения: нижнее пространство для нижнего поля представления содержимого, значение: 0</span><span class="sxs-lookup"><span data-stu-id="c4152-181">Add constraint: Bottom space to Content View Bottom Margin, value: 0</span></span>

1. <span data-ttu-id="c4152-182">Выберите **EventCell,** а затем выберите **инспектор размеров.**</span><span class="sxs-lookup"><span data-stu-id="c4152-182">Select the **EventCell**, then select the **Size Inspector**.</span></span> <span data-ttu-id="c4152-183">Включить **автоматически для** **высоты строки.**</span><span class="sxs-lookup"><span data-stu-id="c4152-183">Enable **Automatic** for **Row Height**.</span></span>

    ![Снимок экрана: контроллеры представления таблиц календаря и календаря](images/calendar-table-storyboard.png)

1. <span data-ttu-id="c4152-185">Откройте **CalendarViewController.swift** и замените его содержимое следующим кодом.</span><span class="sxs-lookup"><span data-stu-id="c4152-185">Open **CalendarViewController.swift** and replace its contents with the following code.</span></span>

    :::code language="swift" source="../demo/GraphTutorial/GraphTutorial/CalendarViewController.swift" id="CalendarViewSnippet":::

1. <span data-ttu-id="c4152-186">Запустите приложение, во войти и коснитесь вкладки **"Календарь".** Должен отлиться список событий.</span><span class="sxs-lookup"><span data-stu-id="c4152-186">Run the app, sign in, and tap the **Calendar** tab. You should see the list of events.</span></span>

    ![Снимок экрана с таблицей событий](images/calendar-list.png)
