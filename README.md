# XmlExport

XmlExport - универсальная выгрузка данных из баз данных Firebird. Выгрузка осуществляется по набору данных, сформированному SQL-запросом и XML-шаблону. Работа с базой данных Firebird реализуется на основе компонентов FireDac. Для подключения к базе данных используется TFDConnection, для работы с запросами - TFDQuery. Для работы с XML используется [NativeXML](https://github.com/kattunga/NativeXml).

## Принцип работы выгрузки.
По SQL-запросу формируется набор данных. Для каждой записи запускается парсинг XML-шаблона, в процессе которого все значения атрибутов и нодов, соответствующие наименованиям полей запроса, заменяются на значения полей из текущего набора.

## Начало работы.
В качестве FDConnection передается активное подключение к базе данных, с которым, в последствие, работает класс.
```delphi
...
XmlExport := TXmlExport.Create(FDConnection);
```
## Обязательные параметры.
```delphi
...
XmlExport.Xml := '<?xml version="1.0"?><somenodes><node>some text</node></somenodes>'; // xml-шаблон
XmlExport.Sql := 'select FIELD from TABLE'; // sql-запрос
...
```

## Необязательные параметры
* `XmlExport.Directory` - String - корневой каталог, в который будут сохраняться результирующие файлы.
* `XmlExport.GroupingField` - String - поле из запроса, по которому будут группироваться файлы.
* `XmlExport.FilenameField` - String - поля из запроса, по которому будут именоваться файлы.
* `XmlExport.DeleteDeclaration` - Boolean - удаление декларации из файлов.
* `XmlExport.DeleteEmptyNodes` - Boolean - удаление пустых элементов из файлов.
* `XmlExport.OpenDirectory` - Boolean - открывать корневой каталог выгрузки после завершения.

### Отображение процесса выгрузки.
Для отображения прогресса выгрузки предусмотрены следующие обработчики:
* `OnProgressValue(AValue: integer)` - передается RecNo текущей записи, по которой выполняется парсинг.
* `OnProgressMax(AMax: integer)` - передается максимальное кол-во записей из набора данных.
* `OnProgressText(AMsg: string)` - передаются информационные сообщения, возникающие в процессе выгрузки.

### Пример.

Имеется таблица CUSTOMERS:

ID  | GENER | NAME
----|-------|---------
1   | МУЖ   | Иванов
2   | МУЖ   | Петров
3   | ЖЕН   | Кузьмина

SQL-запрос:
```sql
select ID, GENDER, NAME from CUSTOMERS
```

XML-шаблон, для такого запроса, будет следующего вида:
```xml
<?xml version="1.0" ?>
<customers id="ID">
  <name>NAME</name>
</customers>
```

Результатом выгрузки, в данном случае, будет 2 файла:

**1.xml**
```xml
<?xml version="1.0" ?>
<customers id="1">
  <name>Иванов</name>
</customers>
```
**2.xml**
```xml
<?xml version="1.0" ?>
<customers id="2">
  <name>Петров</name>
</customers>
```
