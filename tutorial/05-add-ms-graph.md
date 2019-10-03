<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="c0266-101">В этом упражнении вы добавите Microsoft Graph в приложение.</span><span class="sxs-lookup"><span data-stu-id="c0266-101">In this exercise you will incorporate the Microsoft Graph into the application.</span></span> <span data-ttu-id="c0266-102">Для этого приложения вы будете использовать [пакет SDK Microsoft Graph для задания](https://github.com/microsoftgraph/msgraph-sdk-objc) с, чтобы совершать вызовы в Microsoft Graph.</span><span class="sxs-lookup"><span data-stu-id="c0266-102">For this application, you will use the [Microsoft Graph SDK for Objective C](https://github.com/microsoftgraph/msgraph-sdk-objc) to make calls to Microsoft Graph.</span></span>

## <a name="get-calendar-events-from-outlook"></a><span data-ttu-id="c0266-103">Получение событий календаря из Outlook</span><span class="sxs-lookup"><span data-stu-id="c0266-103">Get calendar events from Outlook</span></span>

<span data-ttu-id="c0266-104">В этом разделе описывается расширение `GraphManager` класса для добавления функции, позволяющей получить события и обновление `CalendarViewController` , чтобы использовать эти новые функции.</span><span class="sxs-lookup"><span data-stu-id="c0266-104">In this section you will extend the `GraphManager` class to add a function to get the user's events and update `CalendarViewController` to use these new functions.</span></span>

1. <span data-ttu-id="c0266-105">Откройте **графманажер. SWIFT** и добавьте в `GraphManager` класс следующий метод.</span><span class="sxs-lookup"><span data-stu-id="c0266-105">Open **GraphManager.swift** and add the following method to the `GraphManager` class.</span></span>

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
    > <span data-ttu-id="c0266-106">Рассмотрите, какие действия `getEvents` выполняет код.</span><span class="sxs-lookup"><span data-stu-id="c0266-106">Consider what the code in `getEvents` is doing.</span></span>
    >
    > - <span data-ttu-id="c0266-107">URL-адрес, который будет вызываться — это `/v1.0/me/events`.</span><span class="sxs-lookup"><span data-stu-id="c0266-107">The URL that will be called is `/v1.0/me/events`.</span></span>
    > - <span data-ttu-id="c0266-108">Параметр `select` запроса позволяет ограничить поля, возвращаемые для каждого события, только теми, которые будут использоваться приложением.</span><span class="sxs-lookup"><span data-stu-id="c0266-108">The `select` query parameter limits the fields returned for each events to just those the app will actually use.</span></span>
    > - <span data-ttu-id="c0266-109">Параметр `orderby` запроса сортирует результаты по дате и времени создания, начиная с самого последнего элемента.</span><span class="sxs-lookup"><span data-stu-id="c0266-109">The `orderby` query parameter sorts the results by the date and time they were created, with the most recent item being first.</span></span>

1. <span data-ttu-id="c0266-110">Откройте **календарвиевконтроллер. SWIFT** и замените все его содержимое приведенным ниже кодом.</span><span class="sxs-lookup"><span data-stu-id="c0266-110">Open **CalendarViewController.swift** and replace its entire contents with the following code.</span></span>

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

<span data-ttu-id="c0266-111">Теперь вы можете запустить приложение, войти и нажать в меню элемент Навигация по **календарю** .</span><span class="sxs-lookup"><span data-stu-id="c0266-111">You can now run the app, sign in, and tap the **Calendar** navigation item in the menu.</span></span> <span data-ttu-id="c0266-112">В приложении должен появиться дамп JSON событий.</span><span class="sxs-lookup"><span data-stu-id="c0266-112">You should see a JSON dump of the events in the app.</span></span>

## <a name="display-the-results"></a><span data-ttu-id="c0266-113">Отображение результатов</span><span class="sxs-lookup"><span data-stu-id="c0266-113">Display the results</span></span>

<span data-ttu-id="c0266-114">Теперь вы можете заменить дамп JSON на какой-то способ отобразить результаты в удобном для пользователя виде.</span><span class="sxs-lookup"><span data-stu-id="c0266-114">Now you can replace the JSON dump with something to display the results in a user-friendly manner.</span></span> <span data-ttu-id="c0266-115">В этом разделе будет изменена `getEvents` функция, возвращающая строго типизированные объекты, и изменено `CalendarViewController` , чтобы использовать представление таблицы для отображения событий.</span><span class="sxs-lookup"><span data-stu-id="c0266-115">In this section, you will modify the `getEvents` function to return strongly-typed objects, and modify `CalendarViewController` to use a table view to render the events.</span></span>

### <a name="update-getevents"></a><span data-ttu-id="c0266-116">Обновление событий</span><span class="sxs-lookup"><span data-stu-id="c0266-116">Update getEvents</span></span>

1. <span data-ttu-id="c0266-117">Откройте **графманажер. SWIFT**.</span><span class="sxs-lookup"><span data-stu-id="c0266-117">Open **GraphManager.swift**.</span></span> <span data-ttu-id="c0266-118">Замените объявление `getEvents` функции следующим.</span><span class="sxs-lookup"><span data-stu-id="c0266-118">Change the `getEvents` function declaration to the following.</span></span>

    ```Swift
    public func getEvents(completion: @escaping([MSGraphEvent]?, Error?) -> Void)
    ```

1. <span data-ttu-id="c0266-119">Замените строку `completion(eventsData, nil)` указанным ниже кодом.</span><span class="sxs-lookup"><span data-stu-id="c0266-119">Replace the `completion(eventsData, nil)` line with the following code.</span></span>

    ```Swift
    do {
        // Deserialize response as events collection
        let eventsCollection = try MSCollection(data: eventsData)
        var eventArray: [MSGraphEvent] = []

        eventsCollection.value.forEach({
            (rawEvent: Any) in
            // Convert JSON to a dictionary
            guard let eventDict = rawEvent as? [String: Any] else {
                return
            }

            // Deserialize event from the dictionary
            let event = MSGraphEvent(dictionary: eventDict)!
            eventArray.append(event)
        })

        // Return the array
        completion(eventArray, nil)
    } catch {
        completion(nil, error)
    }
    ```

### <a name="update-calendarviewcontroller"></a><span data-ttu-id="c0266-120">Обновление Календарвиевконтроллер</span><span class="sxs-lookup"><span data-stu-id="c0266-120">Update CalendarViewController</span></span>

1. <span data-ttu-id="c0266-121">Создайте новый файл **класса Touch Cocoa** в проекте **графтуториал** с именем `CalendarTableViewCell.swift`.</span><span class="sxs-lookup"><span data-stu-id="c0266-121">Create a new **Cocoa Touch Class** file in the **GraphTutorial** project named `CalendarTableViewCell.swift`.</span></span> <span data-ttu-id="c0266-122">Выберите **уитаблевиевцелл** в **подклассе** поля.</span><span class="sxs-lookup"><span data-stu-id="c0266-122">Choose **UITableViewCell** in the **Subclass of** field.</span></span>
1. <span data-ttu-id="c0266-123">Откройте **календартаблевиевцелл. SWIFT** и добавьте в `CalendarTableViewCell` класс приведенный ниже код.</span><span class="sxs-lookup"><span data-stu-id="c0266-123">Open **CalendarTableViewCell.swift** and add the following code to the `CalendarTableViewCell` class.</span></span>

    ```Swift
    @IBOutlet var subjectLabel: UILabel!
    @IBOutlet var organizerLabel: UILabel!
    @IBOutlet var durationLabel: UILabel!

    var subject: String? {
        didSet {
            subjectLabel.text = subject
        }
    }

    var organizer: String? {
        didSet {
            organizerLabel.text = organizer
        }
    }

    var duration: String? {
        didSet {
            durationLabel.text = duration
        }
    }
    ```

1. <span data-ttu-id="c0266-124">Откройте файл **Main. Storyboard** и перейдите к **сцене календаря**.</span><span class="sxs-lookup"><span data-stu-id="c0266-124">Open **Main.storyboard** and locate the **Calendar Scene**.</span></span> <span data-ttu-id="c0266-125">Выберите **представление** в **сцене календаря** и удалите его.</span><span class="sxs-lookup"><span data-stu-id="c0266-125">Select the **View** in the **Calendar Scene** and delete it.</span></span>

    ![Снимок экрана с представлением в сцене календаря](./images/view-in-calendar-scene.png)

1. <span data-ttu-id="c0266-127">Добавьте **представление таблицы** из **библиотеки** в **сцену в календаре**.</span><span class="sxs-lookup"><span data-stu-id="c0266-127">Add a **Table View** from the **Library** to the **Calendar Scene**.</span></span>
1. <span data-ttu-id="c0266-128">Выберите представление таблицы, а затем выберите **инспектор атрибутов**.</span><span class="sxs-lookup"><span data-stu-id="c0266-128">Select the table view, then select the **Attributes Inspector**.</span></span> <span data-ttu-id="c0266-129">Задайте для **ячеек прототипа** значение **1**.</span><span class="sxs-lookup"><span data-stu-id="c0266-129">Set **Prototype Cells** to **1**.</span></span>
1. <span data-ttu-id="c0266-130">Используйте **библиотеку** , чтобы добавить три **метки** к ячейке прототипа.</span><span class="sxs-lookup"><span data-stu-id="c0266-130">Use the **Library** to add three **Labels** to the prototype cell.</span></span>
1. <span data-ttu-id="c0266-131">Выберите ячейку прототип, а затем выберите **инспектор удостоверений**.</span><span class="sxs-lookup"><span data-stu-id="c0266-131">Select the prototype cell, then select the **Identity Inspector**.</span></span> <span data-ttu-id="c0266-132">Замените **Class** на **календартаблевиевцелл**.</span><span class="sxs-lookup"><span data-stu-id="c0266-132">Change **Class** to **CalendarTableViewCell**.</span></span>
1. <span data-ttu-id="c0266-133">Выберите **инспектор атрибутов** и **идентификатор** набора `EventCell`.</span><span class="sxs-lookup"><span data-stu-id="c0266-133">Select the **Attributes Inspector** and set **Identifier** to `EventCell`.</span></span>
1. <span data-ttu-id="c0266-134">Выбрав **евентцелл** , выберите **инспектор подключений** и подключение `durationLabel`, `organizerLabel`а `subjectLabel` также метки, добавленные в ячейку в раскадровке.</span><span class="sxs-lookup"><span data-stu-id="c0266-134">With the **EventCell** selected, select the **Connections Inspector** and connect `durationLabel`, `organizerLabel`, and `subjectLabel` to the labels you added to the cell on the storyboard.</span></span>

    ![Снимок экрана с макетом ячеек прототипа](./images/prototype-cell-layout.png)

1. <span data-ttu-id="c0266-136">Откройте **календарвиевконтроллер. SWIFT** и замените его содержимое приведенным ниже кодом.</span><span class="sxs-lookup"><span data-stu-id="c0266-136">Open **CalendarViewController.swift** and replace its contents with the following code.</span></span>

    ```Swift
    import UIKit
    import MSGraphClientModels

    class CalendarViewController: UITableViewController {

        private let tableCellIdentifier = "EventCell"
        private let spinner = SpinnerViewController()
        private var events: [MSGraphEvent]?

        override func viewDidLoad() {
            super.viewDidLoad()

            // Do any additional setup after loading the view.
            self.spinner.start(container: self)

            GraphManager.instance.getEvents {
                (eventArray: [MSGraphEvent]?, error: Error?) in
                DispatchQueue.main.async {
                    self.spinner.stop()

                    guard let events = eventArray, error == nil else {
                        // Show the error
                        let alert = UIAlertController(title: "Error getting events",
                                                      message: error.debugDescription,
                                                      preferredStyle: .alert)

                        alert.addAction(UIAlertAction(title: "OK", style: .default, handler: nil))
                        self.present(alert, animated: true)
                        return
                    }

                    self.events = events
                    self.tableView.reloadData()
                }
            }
        }

        // Number of sections, always 1
        override func numberOfSections(in tableView: UITableView) -> Int {
            return 1
        }

        // Return the number of events in the table
        override func tableView(_ tableView: UITableView, numberOfRowsInSection section: Int) -> Int {
            return events?.count ?? 0
        }

        override func tableView(_ tableView: UITableView, cellForRowAt indexPath: IndexPath) -> UITableViewCell {
            let cell = tableView.dequeueReusableCell(withIdentifier: tableCellIdentifier, for: indexPath) as! CalendarTableViewCell

            // Get the event that corresponds to the row
            let event = events?[indexPath.row]

            // Configure the cell
            cell.subject = event?.subject
            cell.organizer = event?.organizer?.emailAddress?.name

            // Build a duration string
            let duration = "\(self.formatGraphDateTime(dateTime: event?.start)) to \(self.formatGraphDateTime(dateTime: event?.end))"
            cell.duration = duration

            return cell
        }

        private func formatGraphDateTime(dateTime: MSGraphDateTimeTimeZone?) -> String {
            guard let graphDateTime = dateTime else {
                return ""
            }

            // Create a formatter to parse Graph's date format
            let isoFormatter = DateFormatter()
            isoFormatter.dateFormat = "yyyy-MM-dd'T'HH:mm:ss.SSSSSSS"

            // Specify the timezone
            isoFormatter.timeZone = TimeZone(identifier: graphDateTime.timeZone!)

            let date = isoFormatter.date(from: graphDateTime.dateTime)

            // Output like 5/5/2019, 2:00 PM
            let dateFormatter = DateFormatter()
            dateFormatter.dateStyle = .short
            dateFormatter.timeStyle = .short

            return dateFormatter.string(from: date!)
        }
    }
    ```

1. <span data-ttu-id="c0266-137">Запустите приложение и войдите в систему, а затем коснитесь вкладки **Календарь** . Вы должны увидеть список событий.</span><span class="sxs-lookup"><span data-stu-id="c0266-137">Run the app, sign in, and tap the **Calendar** tab. You should see the list of events.</span></span>

    ![Снимок экрана с таблицей событий](./images/calendar-list.png)
