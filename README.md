# XmlExport

XmlExport - универсальная выгрузка данных из баз данных Firebird. Выгрузка осуществляется по набору данных, сформированному SQL-запросом и XML-шаблону. Работа с базой данных Firebird реализуется на основе компонентов FireDac. Для подключения к базе данных используется TFDConnection, для работы с запросами - TFDQuery. Для работы с XML используется [NativeXML](https://github.com/kattunga/NativeXml).

#### Создание объекта.
В качестве FDConnection передается активное подключение к базе данных, с которым, в последствие, работает выгрузка.
```
...
XmlExport = TXmlExport.Create(FDConnection);
...
```
#### Установка XML-шаблона.
В качестве обработчика XML выступает [NativeXML](https://github.com/kattunga/NativeXml). Вся работа с шаблном реализована внутри класса TXmlExport и объекту требуется только передать содержимое шаблона.
```
...
XmlExport.Xml = '<?xml version="1.0" ?><somenodes><node>some text</node></somenodes>';
...
```
#### Передача SQL-запроса.
Работа с запросами к базе данных реализована через TFDQuery и происходит внутри класса TXmlExport, требуется только передать запрос.
```
...
XmlExport.Sql = 'select ID, NAME from CUSTOMERS';
...
```
#### Запуск выгрузки.
Класс TXmlExport унаследован от TThread и является Suspended, поэтому, после определения всех параметров, следует запустить поток.
```
...
XmlExport.Run
...
```



