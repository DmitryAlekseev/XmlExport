# uXmlExport

Класс наследуется от TThread.

Шаблонная выгрузка из БД Firebird в Xml. В исходный xml-шаблон проставляются переменные, которые равны именам полей из запроса. В процессе парсинга, переменные подменяются значениями полей запроса. На каждую строку запроса создается отдельный xml-файл.

#### Создание объекта класса.
    XmlExport = TXmlExport.Create(Database); // где Database объект TFDConnection
    XmlExport.Xml = Xml; // Xml-шаблон выгрузки, принимает String;
    XmlExport.Sql = Sql; // Sql-запрос, по которому будет формироваться набор данных для выгрузки
    
#### Запуск выгрузки
Так как класс унаследован от TThread и предствляет собой Suspend
    XmlExport.Run // запускает поток

#### Обработчик события. Смена состояния выгрузки (запущена/остановлена)
    XmlExport.OnActive = ProcedureOnActive; // где ProcedureOnActive - процедура вида ProcedureOnActive(Active: Boolean), в параметр Active передается True, когда выгрузка запущена и False - когда остановлена.
    
#### Обработчик события. Текущий RecNo на котором выполняется парсинг.
    XmlExport.OnProgressValue = ProcedureOnProgressValue; // где ProcedureOnProgressValue - процедура вида ProcedureOnProgressMax(Value: integer), в параметр Value передается максимальное число записей в наборе данных
    
#### Обработчик события. Максимальное кол-во записей в наборе данных
    XmlExport.OnProgressMax = ProcedureOnProgressMax; // где ProcedureOnProgressMax - процедура вида ProcedureOnProgressValue(Max: integer), в параметр Max передается текущий RecNo строки из набора данных, участвующей в парсинге
    
#### Обработчик события. Передача текстовых сообщений прогресса выгрузки
    XmlExport.OnProgressText := ProcedureOnProgressText; // где ProcedureOnProgressText - процедура вида ProcedureOnProgressValue(Msg: string), в параметр Msg передается текстовое сообщение из внутреннего цикла выгрузки


