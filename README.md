# XmlExport

XmlExport - универсальная выгрузка данных из баз данных Firebird. Выгрузка осуществляется по набору данных, сформированному SQL-запросом и XML-шаблону. Работа с базой данных Firebird реализуется на основе компонентов FireDac. Для подключения к базе данных используется TFDConnection, для работы с запросами - TFDQuery. Для работы с XML используется [NativeXML](https://github.com/kattunga/NativeXml).

## Принцип работы выгрузки.
По SQL-запросу формируется набор данных. Для каждой записи запускается парсинг XML-шаблона, в процессе которого все значения атрибутов и нодов, соответствующие наименованиям полей запроса, заменяются на значения полей из текущего набора.

## Начало работы.
В качестве FDConnection передается активное подключение к базе данных, с которым, в последствие, работает выгрузка.
```delphi
XmlExport := TXmlExport.Create(FDConnection);
XmlExport.Xml := '<?xml version="1.0"?><somenodes><node>some text</node></somenodes>';
XmlExport.Sql := 'select ID, NAME from CUSTOMERS';
...
XmlExport.Run;
```
## Необязательные параметры
* `XmlExport.Directory := 'C:\';` - корневой каталог, в который будут сохраняться результирующие файлы.
* `XmlExport.GroupingField := 'GENDER';` - поле из запроса, по которому будут группироваться файлы.
* `XmlExport.FilenameField := 'NAME';` - поля из запроса, по которому будут именоваться файлы.
* `XmlExport.DeleteDeclaration := True;` - удаление декларации из файлов.
* `XmlExport.DeleteEmptyNodes := True;` - удаление пустых элементов из файлов.
* `XmlExport.OpenDirectory := True;` - открывать корневой каталог выгрузки после завершения.

### Отображение процесса выгрузки.
Для отображения прогресса выгрузки предусмотрены следующие обработчики:
* `OnProgressValue(AValue: integer)` - передается RecNo текущей записи, по которой выполняется парсинг.
* `OnProgressMax(AMax: integer)` - передается максимальное кол-во записей из набора данных.
* `OnProgressText(AMsg: string)` - передаются информационные сообщения, возникающие в процессе выгрузки.

### Пример.

Имеется таблица CUSTOMERS:

ID  | NAME
----|----------------------
1   | Иванов
2   | Петров

SQL-запрос:
```sql
select ID, NAME from CUSTOMERS
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
