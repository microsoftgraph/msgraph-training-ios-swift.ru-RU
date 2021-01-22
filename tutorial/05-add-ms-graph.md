<!-- markdownlint-disable MD002 MD041 -->

В этом упражнении вы включаете Microsoft Graph в приложение. Для этого приложения для вызова Microsoft Graph используется [SDK Microsoft Graph](https://github.com/microsoftgraph/msgraph-sdk-objc) для Objective C.

## <a name="get-calendar-events-from-outlook"></a>Получить события календаря из Outlook

В этом разделе мы расширим класс, чтобы добавить функцию для получения событий пользователя за текущую неделю, и обновим, чтобы `GraphManager` `CalendarViewController` использовать эти новые функции.

1. Откройте **GraphManager.swift** и добавьте в класс следующий `GraphManager` метод.

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
    > Подумайте, что делает `getCalendarView` код.
    >
    > - Будет вызван URL-адрес `/v1.0/me/calendarview` .
    >   - Параметры `startDateTime` `endDateTime` и параметры запроса определяют начало и конец представления календаря.
    >   - Параметр запроса ограничивает поля, возвращаемые для каждого события, только теми, которые `select` будут фактически использовать представление.
    >   - Параметр `orderby` запроса сортировать результаты по времени начала.
    >   - Параметр `top` запроса запрашивает 25 результатов на страницу.
    >   - Заглавная запись заставляет Microsoft Graph возвращать время начала и окончания каждого события в `Prefer: outlook.timezone` часовом поясе пользователя.

1. Создайте новый **swift-файл** в **проекте GraphTutorial** **с именем GraphToIana.swift.** Добавьте указанный ниже код в файл.

    :::code language="swift" source="../demo/GraphTutorial/GraphTutorial/GraphToIana.swift" id="GraphToIanaSnippet":::

    Это простой поиск идентификатора часового пояса IANA на основе имени часового пояса, возвращаемого Microsoft Graph.

1. Откройте **CalendarViewController.swift** и замените его все содержимое следующим кодом.

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

Теперь вы можете запустить приложение, войти и к пункту навигации **"Календарь"** в меню. В приложении должен отыменить дамп событий в JSON.

## <a name="display-the-results"></a>Отображение результатов

Теперь вы можете заменить дамп JSON на что-то, чтобы отобразить результаты в удобном для пользователя виде. В этом разделе вы измените функцию, чтобы она возвращала строго типные объекты, и измените для отображения событий представление `getCalendarView` `CalendarViewController` таблицы.

1. Откройте **GraphManager.swift.** Замените имеющуюся функцию `getCalendarView` указанным ниже кодом.

    :::code language="swift" source="../demo/GraphTutorial/GraphTutorial/GraphManager.swift" id="GetEventsSnippet" highlight="3,28-49":::

1. Создайте новый файл **класса Cocoa Touch в** **проекте GraphTutorial** с именем `CalendarTableViewController.swift` . Выберите **UITableViewController** в **подклассе** поля.

1. Откройте **CalendarTableViewController.swift** и замените его содержимое на следующее.

    :::code language="swift" source="../demo/GraphTutorial/GraphTutorial/CalendarTableViewController.swift" id="CalendarTableViewControllerSnippet":::

1. Создайте новый файл **класса Cocoa Touch в** **проекте GraphTutorial** с именем `CalendarTableViewCell.swift` . Выберите **UITableViewCell** в **подклассе** поля.

1. Откройте **CalendarTableViewCell.swift** и добавьте в класс следующие `CalendarTableViewCell` свойства.

    :::code language="swift" source="../demo/GraphTutorial/GraphTutorial/CalendarTableViewCell.swift" id="PropertiesSnippet":::

1. Откройте **main.storyboard и** найдите **сцену календаря.** Удалите представление прокрутки из корневого представления.
1. С помощью **библиотеки добавьте** в **верхнюю** часть представления панели навигации.
1. Дважды щелкните **заголовок** на панели навигации и обновите его до `Calendar` .
1. С помощью **библиотеки добавьте** элемент кнопки **панели** в правую часть панели навигации.
1. Выберите новую кнопку панели, а затем — **инспектор атрибутов.** Измените **изображение** на **"плюс"**.
1. Добавьте представление **контейнера** из **библиотеки в** представление под панели навигации. Измейте представление контейнера, чтобы занять все оставшееся место в представлении.
1. Установите ограничения для панели навигации и представления контейнера следующим образом.

    - **Панель навигации**
        - Добавить ограничение: высота, значение: 44
        - Add constraint: Leading space to Safe Area, value: 0
        - Добавление ограничения: после пробела в безопасную область, значение: 0
        - Add constraint: Top space to Safe Area, value: 0
    - **Представление контейнера**
        - Add constraint: Leading space to Safe Area, value: 0
        - Добавление ограничения: после пробела в безопасную область, значение: 0
        - Добавление ограничения: верхнее место на панели навигации снизу, значение: 0
        - Добавление ограничения: нижнее пространство в безопасной области, значение: 0

1. Найдите второй контроллер представления, добавленный в storyboard при добавлении представления контейнера. Он подключается к сцене **календаря** с помощью встраиваемой segue. Выберите этот контроллер и используйте **identity Inspector,** чтобы изменить **класс** на **CalendarTableViewController.**
1. Удалите **представление** из **контроллера представления таблицы календаря.**
1. Добавьте представление **таблицы** из **библиотеки в** контроллер **представления таблицы календаря.**
1. Выберите представление таблицы, а затем выберите **инспектор атрибутов.** Установите **для прототипа ячеек** **1**.
1. Перетащите нижний край прототипа ячейки, чтобы предоставить вам большую область для работы.
1. Используйте **библиотеку,** чтобы добавить три **метки в** прототип ячейки.
1. Выберите прототип ячейки, а затем выберите **identity Inspector.** Измените **класс** **на CalendarTableViewCell.**
1. Выберите **"Attributes Inspector"** (Инспектор атрибутов) и за **установите для идентификатора (Identifier)** `EventCell` (Идентификатор).
1. Выбрав **EventCell,** выберите **Connections Inspector** и подключите его, а также метки, добавленные в ячейку `durationLabel` в `organizerLabel` `subjectLabel` storyboard.
1. Установите свойства и ограничения для трех меток следующим образом.

    - **Метка темы**
        - Add constraint: Leading space to Content View Leading Margin, value: 0
        - Добавление ограничения: в окн. место в поле "Точка с представлением содержимого", значение: 0
        - Add constraint: Top space to Content View Top Margin, value: 0
    - **Метка организатора**
        - Шрифт: System 12.0
        - Добавить ограничение: высота, значение: 15
        - Add constraint: Leading space to Content View Leading Margin, value: 0
        - Добавление ограничения: в окн. место в поле "Точка с представлением содержимого", значение: 0
        - Add constraint: Top space to Subject Label Bottom, value: Standard
    - **Метка длительности**
        - Шрифт: System 12.0
        - Цвет: темно-серый цвет
        - Добавить ограничение: высота, значение: 15
        - Add constraint: Leading space to Content View Leading Margin, value: 0
        - Добавление ограничения: в окн. место в поле "Точка с представлением содержимого", значение: 0
        - Add constraint: Top space to Organizer Label Bottom, value: Standard
        - Добавление ограничения: нижнее пространство для нижнего поля представления содержимого, значение: 0

1. Выберите **EventCell,** а затем выберите **инспектор размеров.** Включить **автоматически для** **высоты строки.**

    ![Снимок экрана: контроллеры представления таблиц календаря и календаря](images/calendar-table-storyboard.png)

1. Откройте **CalendarViewController.swift** и замените его содержимое следующим кодом.

    :::code language="swift" source="../demo/GraphTutorial/GraphTutorial/CalendarViewController.swift" id="CalendarViewSnippet":::

1. Запустите приложение, во войти и коснитесь вкладки **"Календарь".** Должен отлиться список событий.

    ![Снимок экрана с таблицей событий](images/calendar-list.png)
