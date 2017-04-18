# XmlExport

XmlExport - универсальная выгрузка данных из баз данных Firebird. Выгрузка осуществляется по набору данных, сформированному SQL-запросом и XML-шаблону. Работа с базой данных Firebird реализуется на основе компонентов FireDac. Для подключения к базе данных используется TFDConnection, для работы с запросами - TFDQuery. Для работы с XML используется [NativeXML](https://github.com/kattunga/NativeXml).

## Принцип работы выгрузки.
По SQL-запросу формируется набор данных. Для каждой записи запускается парсинг XML-шаблона, в процессе которого все значения атрибутов и нодов, соответствующие наименованиям полей запроса, заменяются на значения полей из текущего набора.

## Начало работы.
В качестве FDConnection передается активное подключение к базе данных, с которым, в дальнейшем, работает объект класса.
```delphi
...
XmlExport := TXmlExport.Create(FDConnection);
...
```
## Обязательные параметры.
```delphi
...
XmlExport.Xml := '<?xml version="1.0"?><somenodes><node>some text</node></somenodes>';
XmlExport.Sql := 'select FIELD from TABLE'; 
...
```
## Необязательные параметры
* `XmlExport.Directory` - String - корневой каталог, в который будут сохраняться результирующие файлы.
* `XmlExport.GroupingField` - String - поле из запроса, по которому будут группироваться файлы.
* `XmlExport.FilenameField` - String - поля из запроса, по которому будут именоваться файлы.
* `XmlExport.DeleteDeclaration` - Boolean - удаление декларации из файлов.
* `XmlExport.DeleteEmptyNodes` - Boolean - удаление пустых элементов из файлов.
* `XmlExport.OpenDirectory` - Boolean - открывать корневой каталог выгрузки после завершения.

## Обработчики событий.
* `XmlExport.OnActive` - передает статус выгрузки.
* `XmlExport.OnProgressValue` - передает RecNo текущей, обрабатываемой.
* `XmlExport.OnProgressMax` - передает максимальное кол-во записей набора данных.
* `XmlExport.OnProgressText` - передает информационные сообщения, возникающие в процессе выгрузки.

## Запуск.
```delphi
...
XmlExport.Run;
...
```

## Пример.

Имеется таблица CUSTOMERS. Необходимо выгрузить всех клиентов, с группировкой в полу, в качестве имен файлов должны быть имена клиентов.

ID  | GENDER | NAME
----|--------|---------
1   | МУЖ    | Иванов
2   | МУЖ    | Петров
3   | ЖЕН    | Кузьмина


```delphi
...
var
  XmlExport: TXmlExport;
begin
  XmlExport := TXmlExport.Create(Database);
  XmlExport.Xml := '<?xml version="1.0"?><customers id="ID"><name>NAME</name></customers>';
  XmlExport.Sql := 'select ID, GENDER, NAME from CUSTOMERS'; 
  XmlExport.Directory := 'C:\exports';
  XmlExport.GroupingField := 'GENDER';
  XmlExport.FilenameField := 'NAME';
  XmlExport.Run;
end;
...
```
Результатом выгрузки, в данном случае, будут 3 файла, в соответствующих каталогах:

*C:\exports\GENDER_МУЖ\Иванов.xml*
```xml
<?xml version="1.0" ?>
<customers id="1">
  <name>Иванов</name>
</customers>
```
*C:\exports\GENDER_МУЖ\Петров.xml*
```xml
<?xml version="1.0" ?>
<customers id="2">
  <name>Петров</name>
</customers>
```
*C:\exports\GENDER_ЖЕН\Кузьмина.xml*
```xml
<?xml version="1.0" ?>
<customers id="3">
  <name>Кузьмина</name>
</customers>
```

