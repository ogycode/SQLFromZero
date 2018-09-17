[![Logo](https://raw.githubusercontent.com/ogycode/SQLFromZero/master/merch/logo.jpg)](https://github.com/ogycode/CPPFromZero)

# SQL from Zero
**I study the technology and this is my "note-book"**

1. [Создание БД](#create)
2. [Основы](#base)

## <a name="create"></a>Создание БД
### *Системные базы данных, которые нужны для работы SQL Server'a:*
 - **master** - главная БД, без нее не будет работать сервер
 - **model** - шаблон для создания других БД
 - **msdb** - хранит информацию о планировщике и бэкапах
 - **tempdb** - хранилище для воеменных обьектов, пересоздается при каждом запуске сервера

 ### *Ключевые слова при создании БД:*
 - **Logical Name** - имя файла БД
 - **File Type** - тип файлов БД, ROWS Data, LOG
 - **Filegroup** - группа файлов, используется для разбиения БД на части
 - **Initial Size** - размер БД при старте
 - **Autogrowth/Maxsize** - размер, в мегабайтах или процентах, на который будет увеличиватся БД
 - **Path** - путь, где будут хранится БД
 - **File Name** - непосредственно имя БД

### *Ключевые слова при создание таблицы:*
 - **Column Name** - имя столбца
 - **Data Type** - тип данных в столбце
 - **Allow Nulls** - может ли значение в ячейке == NULL

Перед созданием необходимо создать столбец с ID и устaновить для него *Primary Key*.

### Пример запроса к БД:
    SELECT * FROM [Имя БД].[Схема].[Имя таблицы]

*Пример:*

    SELECT * FROM bdName.dbo.Users

Этот запрос выведет все записи из таблици Users (Выбрать ВСЕ из БД). Если необходимо выполнить несколько запросов к одной таблице:

    USE [Имя БД] 
    SELECT * FROM [Имя таблицы]`

*Пример:*
 
    USE bdName
    SELECT * FROM Users


 ## <a name="base"></a>Основы

### Удаление и создание БД
 Для создания БД необходимо выполнить команду

 `CREATE DATABASE BD_NAME`, где BD_NAME - это имя базы данных.

Для прикрипления уже существующих БД к серверу необходимо выполнить 

    CREATE DATABASE BD_NAME
    ON PRIMARY(FILENAME='path')
    FOR ATTACH;

 Для удаления БД необходимо выполнить `DROP DATABASE BD_NAME`

 ### Таблицы

 Для добавления таблицы необходимо выполнить следующий запрос:

    USE DB_NAME
    CREATE TABLE TABLE_NAME
    (
        
        Id INT,
        FirstName VARCHAR(64),
        SecondName VARCHAR(64),
        ....
        Age INT
    );
где TABLE_NAME - имя таблицы, (Id, FirstNameб SecondNameб Age) - имена колонок, (INT, VARCHAR(64)) - типы данных. `USE DB_NAME` - указывает, что создавать таблицу необходимо именно в БД с именем DB_NAME
