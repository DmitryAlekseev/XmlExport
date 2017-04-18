# XmlExport

XmlExport - универсальная выгрузка данных из баз данных Firebird. Выгрузка осуществляется по набору данных, сформированному SQL-запросом и XML-шаблону. Работа с базой данных Firebird реализуется на основе компонентов FireDac. Для подключения к базе данных используется TFDConnection, для работы с запросами - TFDQuery. Для работы с XML используется [NativeXML](https://github.com/kattunga/NativeXml).

### Принцип работы выгрузки.
По SQL-запросу формируется набор данных. Для каждой записи запускается парсинг XML-шаблона, в процессе которого все значения атрибутов и нодов, соответствующие наименованиям полей запроса, заменяются на значения полей из текущего набора.

### Создание объекта.
В качестве FDConnection передается активное подключение к базе данных, с которым, в последствие, работает выгрузка.
```
...
XmlExport := TXmlExport.Create(FDConnection);
...
```
### Установка XML-шаблона.
```
...
XmlExport.Xml := '<?xml version="1.0" ?><somenodes><node>some text</node></somenodes>';
...
```
### Передача SQL-запроса.
```
...
XmlExport.Sql := 'select ID, NAME from CUSTOMERS';
...
```
### Каталог выгрузки.
```
...
XmlExport.Directory := 'C:\';
...
```
### Группировка файлов.
```
...
XmlExport.GroupingField := 'GENDER';
...
```
### Именование файлов.
```
...
XmlExport.FilenameField := 'NAME';
...
```
### Удаление делакрации.
```
...
XmlExport.DeleteDeclaration := True;
...
```
### Удаление пустых элементов.
```
...
XmlExport.DeleteEmptyNodes := True;
...
```
### Открытие каталога выгрузки.
```
...
XmlExport.OpenDirectory := True;
...
```
### Запуск выгрузки.
Класс TXmlExport унаследован от TThread и является Suspended, поэтому, после определения всех параметров, следует запустить поток.
```
...
XmlExport.Run
...
```
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
