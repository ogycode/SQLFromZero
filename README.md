[![Logo](https://raw.githubusercontent.com/verloka/SQLFromZero/master/merch/logo.jpg)](https://github.com/verloka/CPPFromZero)

# SQL from Zero
**I study the technology and this is my "note-book"**

## <a name="top"></a>Содержание

* [Создание БД](#create)
  * [Системные базы данных](#create1)
  * [Ключевые слова](#create2)
  * [Пример запроса](#create3)
* [Основы](#base)
  * [Удаление и создание БД](#base1)
  * [Таблицы](#base2)
  * [Удаление таблицы](#base3)
  * [Переименование таблицы](#base4)
  * [Первичный ключ](#base5)
  * [Атрибут IDENTITY](#base6)
  * [Атрибут UNIQUE](#base7)
  * [Атрибут DEFAULT](#base8)
  * [Атрибут CHECK](#base9)
  * [Оператор CONSTRAINT](#base10)
  * [Внешние ключи](#base11)
  * [ON DELETE и ON UPDATE](#base12)
  * [Изменение существующей таблицы](#base13)
  * [Команда Go](#base14)
* [Типы данных](#datatype)
  * [Числовые](#datatype1)
  * [Дата и время](#datatype2)
  * [Строковые](#datatype3)
  * [Бинарные типы](#datatype4)
  * [Остальное](#datatype5)
* [Работа с данными](#job)
  * [INSERT](#job1)
  * [SELECT](#job2)
  * [DISTINCT](#job3)
  * [SELECT INTO](#job4)
  * [ORDER BY](#job5)
  * [TOP](#job6)
  * [OFFSET и FETCH](#job7)
  * [WHERE](#job8)
  * [IS NULL](#job9)
  * [IN](#job10)
  * [BETWEEN](#job11)
  * [LIKE](#job12)
  * [UPDATE](#job13)
  * [DELETE](#job14)
* [Группировка](#group)
  * [Агрегатные функции](#group1)
  * [GROUP BY](#group2)
  * [HAVING](#group3)
* [Подзапросы](#req)
  * [Коррелирующие запросы](#reqKorel)
  * [Оператор ALL](#reqAny)
  * [Оператор SOME и ANY](#reqSomeAny)
  * [Оператор EXISTS](#reqExisrs)
* [Соединение таблиц](#join)
  * [Оператор INNER JOIN](#joinInner)
  * [Оператор OUTER JOIN](#joinOuter)
  * [Оператор UNION](#joinUnion)
  * [Оператор EXCEPT](#joinExcept)
  * [Оператор INTERSECT](#joinIntersect)
* [Встроенные функции](#func)
  * [Функции для работы со строками](#funcStr)
  * [Функции для работы с числами](#funcInt)
  * [Функции по работе с датами и временем](#funcDt)
  * [Преобразование данных](#funcCvt)
    * [График преобразований](#funcChart)
  * [Функции CASE и IIF](#funcIF)
  * [Функции NEWID, ISNULL и COALESCE](#funcOther)

## <a name="create"></a>[⇈](#top) Создание БД
### <a name="create1"></a>[⇈](#top) *Системные базы данных, которые нужны для работы SQL Server'a:*
 - **master** - главная БД, без нее не будет работать сервер
 - **model** - шаблон для создания других БД
 - **msdb** - хранит информацию о планировщике и бэкапах
 - **tempdb** - хранилище для воеменных обьектов, пересоздается при каждом запуске сервера

### <a name="create2"></a>[⇈](#top) *Ключевые слова при создании БД:*
 - **Logical Name** - имя файла БД
 - **File Type** - тип файлов БД, ROWS Data, LOG
 - **Filegroup** - группа файлов, используется для разбиения БД на части
 - **Initial Size** - размер БД при старте
 - **Autogrowth/Maxsize** - размер, в мегабайтах или процентах, на который будет увеличиватся БД
 - **Path** - путь, где будут хранится БД
 - **File Name** - непосредственно имя БД
 - **Column Name** - имя столбца
 - **Data Type** - тип данных в столбце
 - **Allow Nulls** - может ли значение в ячейке == NULL

Перед созданием необходимо создать столбец с ID и устaновить для него *Primary Key*.

### <a name="create3"></a>[⇈](#top) Пример запроса к БД:
    SELECT * FROM [Имя БД].[Схема].[Имя таблицы]

*Пример:*

    SELECT * FROM bdName.dbo.Users

Этот запрос выведет все записи из таблици Users (Выбрать ВСЕ из БД). Если необходимо выполнить несколько запросов к одной таблице:

    USE [Имя БД]
    SELECT * FROM [Имя таблицы]

*Пример:*

    USE bdName
    SELECT * FROM Users


## <a name="base"></a>[⇈](#top) Основы

### <a name="base1"></a>[⇈](#top) Удаление и создание БД
 Для создания БД необходимо выполнить команду

 `CREATE DATABASE BD_NAME`, где BD_NAME - это имя базы данных.

Для прикрипления уже существующих БД к серверу необходимо выполнить

    CREATE DATABASE BD_NAME
    ON PRIMARY(FILENAME='path')
    FOR ATTACH;

 Для удаления БД необходимо выполнить `DROP DATABASE BD_NAME`

### <a name="base2"></a>[⇈](#top) Таблицы
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

### <a name="base3"></a>[⇈](#top) Удаление таблицы
    DROP TABLE TABLE_NAME

### <a name="base4"></a>[⇈](#top) Переименование таблицы

    USE BD_NAME;
    EXEC sp_rename 'OLD_NAME', 'NEW_NAME';

`sp_rename` - инструкция которая позволяет переименовывать таблицы, столбцы.

### <a name="base5"></a>[⇈](#top) Первичный ключ

Для персонализации записей в БД необходимо использовать какой-нибудь идентификатор. Задается он ключевым словом `PRIMARY KEY`. Использовать его можно несколькими способами:

#1

    CREATE TABLE table_Name
    {
        Id INT PRIMARY KEY,
        Number INT
    };

#2

    CREATE TABLE table_Name
    {
        Id INT,
        Number INT,
        PRIMARY KEY(Id)
    };

Также, первичный ключ может быть составным, например, если необходимо использовать сразу несколько столбцов как идентифицирующие.

    CREATE TABLE table_Name
    {
        Id INT,
        Number INT,
        Price MONEY,
        PRIMARY KEY(Id, Number)
    }

### <a name="base6"></a>[⇈](#top) Атрибут IDENTITY

Атрибут IDENTITY указывает, что столбец при добавлении новой будет инкремироватся на указаное число. Можно применять только к числовым значениям.

`IDENTITY(seed, increment)`, где seed - место, с которого будет инкремирование, increment - число на которое будет происходить инкремирование. Например:

    CREATE TABLE table_Name
    {
        Id INT PRIMARY KEY IDENTITY(1, 1),
        Number INT
    };
инкремироватся будет столбец Id на еденицу при добавлении новой записи.

### <a name="base7"></a>[⇈](#top) Атрибут UNIQUE
Этот атрибут указывает, что поле может иметь только уникальное значение.

    CREATE TABLE table_Name
    {
        Id INT PRIMARY KEY,
        Emain VARCHAR(32) UNIQUE,
        Phone VARCHAR(32) UNIQUE
    };
эта запись определяет, что добавлять записи в таблицу с одинаковыми Email и Phone нельзя! Т.е. это сделать невозможно если уже есть запись с таким же значеним Email или Phone ранее.

Также есть и вторая форма записи подобно `PRIMARY KEY`:

    CREATE TABLE table_Name
    {
        Id INT PRIMARY KEY,
        Emain VARCHAR(32),
        Phone VARCHAR(32),
        UNIQUE(Email, Phone)
    }

### <a name="base8"></a>[⇈](#top) Атрибут DEFAULT

Этот атрибут указывает какое значение будет записано по умолччанию в ячейку. Пример использования:

    CREATE TABLE table_Name
    {
        Id INT PRIMARY KEY,
        Number INT DEFAULT 44
    };
исходя из этой записи в поле Number будет записано значение 44, по-умолчанию.

### <a name="base9"></a>[⇈](#top) Атрибут CHECK

Атрибут указывает ограничения для добавляемых значений. Пример использования:

    CREATE TABLE table_Name
    {
        Id INT PRIMARY KEY,
        Age INT CHECK(Age > 0 AND Age < 100),
        Emain VARCHAR(32) UNIQUE CHECK(Email != ''),
        Phone VARCHAR(32) UNIQUE CHECK(Phone != '')
    };

Више описано, что в поле Age может быть записано только значения от нуля и до ста, а также, что поля Email и Phone не могут равнятся пустой строке. Такую запись можно записать иначе:

    CREATE TABLE table_Name
    {
        Id INT PRIMARY KEY,
        Age INT,
        Emain VARCHAR(32),
        Phone VARCHAR(32),
        CHECK(Age > 0 AND Age < 100 AND (Email != '') AND (Phone != '')),
        UNIQUE(Email, Phone)
    };

Можно использовать ключевые слова AND - и, OR - или.

### <a name="base10"></a>[⇈](#top) Оператор CONSTRAINT

Этот оператор устанавливает именя для ограничений. Т.е. можно устоновить имя ограничения для `DEFAULT` или `CHECK` для того, что бы потом можно было уего удалить без проблем. Как обычно, два примера записи:

#1

    CREATE TABLE table_Name
    {
        Id INT CONSTRAINT PK_Id PRIMARY KEY,
        Age INT CONSTRAINT CK_Age CHECK(Age > 0 AND Age < 100),
        Number INT CONSTRAINT DF_Number DEFAULT 44,
        Emain VARCHAR(32) CONSTRAINT UQ_Email UNIQUE CHECK(Email != '')
    }

#2

    CREATE TABLE table_Name
    {
        Id INT,
        Age INT,
        Number INT,
        Emain VARCHAR(32),
        CONSTRAINT PK_Id PRIMARY KEY,
        CONSTRAINT CK_Age CHECK(Age > 0 AND Age < 100),
        CONSTRAINT DF_Number DEFAULT 44,
        CONSTRAINT UQ_Email UNIQUE CHECK(Email != '')
    }

При задании имени ограничению принято использовать следуюущие префиксы:
 - *PK_* - для PRIMARY KEY
 - *FK_* - для FOREIGN KEY
 - *CK_* - для CHECK
 - *UQ_* - для UNIQUE
 - *DF_* - для DEFAULT

### <a name="base11"></a>[⇈](#top) Внешние ключи

Внешне ключи используются для связи таблиц между собой. Обычно, внешний ключ указывает на Id другой таблицы но это не обязательно, он может указывать на другой столбец с уникальным значением.

Имеем две таблицы `Customer` и `Order`, пусть одна таблица ссылается на другую.

    CREATE TABLE Customer
    {
        Id INT PRIMARY KEY,
        ...
    };

    CREATE TABLE Order
    {
        Id INT PRIMARY KEY,
        ...
        CustomerId INT REFERENCES Customer(Id)
    };

В примере выше, написано, что поле `CustomerId` ссылается на поле `Id` в таблице `Customer`. В данном случае, таблица `Customer` - главная таблица, а `Order` - зависимая. CustomerId - это внешний ключ. Также можно записать и подругому:

    CREATE TABLE Order
    {
        Id INT PTIMARY KEY,
        CustomerId INT,
        ...
        FOREIGN KEY(CustomerId)  REFERENCES Customer(Id)
    };

или так

    CREATE TABLE Order
    {
        Id INT PTIMARY KEY,
        CustomerId INT,
        ...
        CONSTRAINT FK_CustomerId KEY(CustomerId)  REFERENCES Customer(Id)
    };
в этом случае, задается имя для внешнего ключа.

### <a name="base12"></a>[⇈](#top) ON DELETE и ON UPDATE

Данные ключевые слова позволят задать действия при изменении или удалении записи из главной таблицы. Для этого указываются следующие опции:
 - CASCADE: автоматически удаляет или изменяет строки.
 - NO ACTION: фактически какие-либо действия отсутствуют.
 - SET NULL: устанавливает для столбца внешнего ключа значение NULL.
 - SET DEFAULT: устанавливает для столбца внешнего ключа значение по умолчанию.

 При удалении строки из главной таблицы мы не сможем ее удалить если на нее ссылается внешний ключи из какой-нибудь зависимой таблицы. Если необходимо удалить все записи из зависимых таблиц при удалении из главной используют `CASCADE`:

    CREATE TABLE Order
    {
        Id INT PTIMARY KEY,
        CustomerId INT,
        ...
        FOREIGN KEY(CustomerId)  REFERENCES Customer(Id) ON DELETE CASCADE
    };

Аналогично работает и `ON UPDATE CASCADE` т.е. при изменении занчения в главной таблице изменения произойдут в зависимой.

Установка NULL при удалении:

    CREATE TABLE Order
    {
        Id INT PTIMARY KEY,
        CustomerId INT,
        ...
        FOREIGN KEY(CustomerId)  REFERENCES Customer(Id) ON DELETE SET NULL
    };

Установка DEFULT при удалении:

    CREATE TABLE Order
    {
        Id INT PTIMARY KEY,
        CustomerId INT DEFAULT -1,
        ...
        FOREIGN KEY(CustomerId)  REFERENCES Customer(Id) ON DELETE SET DEFAULT
    };

Если в качестве значения по-умолчанию не установлено значение то применится значение NULL, в случае выше, при удалении записи из главной таблицы, значение `CustomerId` установится в -1.

### <a name="base13"></a>[⇈](#top) Изменение существующей таблицы

**Добавление нового столбца:**

    ALTER TABLE table_Name
    ADD column_Name INT NOT NULL DEFAULT -1;

**Удаление столбца:**

    ALTER TABLE table_Name
    DROP COLUMN column_Name;

**Изменение типа столбца:**

    ALTER TABLE table_Name
    ALTER COLUMN column_Name varchar(32);

**Добавление ограничения CHECK:**

    ALTER TABLE table_Name
    ADD CHECK(column_Name > 21);

При выполнении команды выше, сервер сначала проверит уже добавленные данные на соответствие этому ограничению и если хотя бы одно не пройдет проерку вернет ошибку и ограничение не добавит. Для добавления ограничения без проверку существующих записей используется выражение `WITH NOCHECK`:

    ALTER TABLE table_Name WITH NOCHECK
    ADD CHECK(column_Name > 21);

**Добавление внешнего ключа**

Пусть ключ `CustomerId` ссылается на столбец `Id` из таблицы `Customer`:

    ALTER TABLE Order
    ADD FOREIGN KEY(CustomerId) REFERENCES Customer(Id);

**Добавление первичного ключа:**

Возможно Вы забыли, при создании таблицы, указать первичный ключ, добавть его можно следующим действием:

    ALTER TABLE table_Name
    ADD PRIMARY KEY (Id);

**Добавление ограничений с именами:**

    ALTER TABLE Orders
    ADD CONSTRAINT PK_OrderId PRIMARY KEY (Id),
        CONSTRAINT FK_CustomerId FOREIGN KEY(CustomerId) REFERENCES Customer(Id);

**Удаление ограничений:**

    ALTER TABLE Orders
    DROP PK_OrderId;

### <a name="base14"></a>[⇈](#top) Команда `Go`

Команда `Go` служит указателем того, что сначала нужно дождатся окончания операций определенных перед ним. Пример:

    CREATE DATABASE DB_NAME;
    GO

    USE DB_NAME;

    CREATE TABLE table_Name
    {
        Id INT PRIMARY KEY
    };

Из текста выше следует, что перед созданием таблицы `table_Name` необходимо дождатся создания базы данных `DB_NAME`. Это и делает команда `Go`.

## <a name="datatype"></a>[⇈](#top) Типы данных

### <a name="datatype1"></a>[⇈](#top) Числовые:
 - *BIT* - хранит значение 0 или 1 (`1` байт).
 - *TINYINT* - хранит числа от 0 до 255 (`1` байт).
 - *SMALLINT* - хранит числа от –32 768 до 32 767 (`2` байта).
 - *INT* - хранит числа от –2 147 483 648 до 2 147 483 647 (`4` байта).
 - *BIGINT* - хранит очень большие числа от -9 223 372 036 854 775 808 до 9 223 372 036 854 775 807 (`8` байт).
 - *DECIMAL* - хранит числа c фиксированной точностью (`5` - `17` байт). Данный тип может принимать два параметра precision и scale: `DECIMAL(precision, scale)`. Параметр `precision` представляет максимальное количество цифр, которые может хранить число. Это значение должно находиться в диапазоне от 1 до 38. По умолчанию оно равно 18.Параметр `scale` представляет максимальное количество цифр, которые может содержать число после запятой. Это значение должно находиться в диапазоне от 0 до значения параметра `precision`. По умолчанию оно равно 0.
 - *NUMERIC* - данный тип аналогичен типу DECIMAL.
 - *SMALLMONEY* - хранит дробные значения от -214 748.3648 до 214 748.3647 (`4` байта). Предназначено для хранения денежных величин. Эквивалентен типу DECIMAL(10,4).
 - *MONEY* - хранит дробные значения от -922 337 203 685 477.5808 до 922 337 203 685 477.5807 (`8` байт). Представляет денежные величины. Эквивалентен типу DECIMAL(19,4).
 - *FLOAT* - хранит числа от –1.79E+308 до 1.79E+308 (`4` - `8` байт). Может иметь форму опредеения в виде `FLOAT(n)`, где `n` представляет число бит, которые используются для хранения десятичной части числа (мантиссы). По умолчанию n = 53.
 - *REAL* - хранит числа от –340E+38 to 3.40E+38 (`4` байта). Эквивалентен типу FLOAT(24).

### <a name="datatype2"></a>[⇈](#top) Дата и время:
  - *DATE* - хранит даты от 0001-01-01 (1 января 0001 года) до 9999-12-31 (31 декабря 9999 года) (`3` байта).
 - *TIME* - хранит время в диапазоне от 00:00:00.0000000 до 23:59:59.9999999 (`3` - `5` байт). Может иметь форму `TIME(n)`, где `n` представляет количество цифр от 0 до 7 в дробной части секунд.
 - *DATETIME* - хранит даты и время от 01/01/1753 до 31/12/9999 (`8` байт).
 - *DATETIME2* - хранит даты и время в диапазоне от 01/01/0001 00:00:00.0000000 до 31/12/9999 23:59:59.9999999 (`6` - `8` байт). Может иметь форму `DATETIME2(n)`, где `n` представляет количество цифр от 0 до 7 в дробной части секунд.
 - *SMALLDATETIME* - хранит даты и время в диапазоне от 01/01/1900 до 06/06/2079 (`4` байта).
 - *DATETIMEOFFSET* - хранит даты и время в диапазоне от 0001-01-01 до 9999-12-31. Сохраняет детальную информацию о времени с точностью до 100 наносекунд (`10` байт).

### <a name="datatype3"></a>[⇈](#top) Строковые:
  - *CHAR* хранит строку длиной от 1 до 8 000 символов. На каждый символ выделяет по `1` байту. Не подходит для многих языков, так как хранит символы не в кодировке Unicode. Количество символов, которое может хранить столбец, передается в скобках. Например, для столбца с типом `CHAR(10)` будет выделено `10` байт. И если мы сохраним в столбце строку менее 10 символов, то она будет дополнена пробелами.
 - *VARCHAR* - хранит строку. На каждый символ выделяется `1` байт. Можно указать конкретную длину для столбца - от 1 до 8 000 символов, например, `VARCHAR(10)`. Если строка должна иметь больше 8000 символов, то задается размер MAX, а на хранение строки может выделяться до 2 Гб: `VARCHAR(MAX)`. Не подходит для многих языков, так как хранит символы не в кодировке Unicode. В отличие от типа `CHAR` если в столбец с типом `VARCHAR(10)` будет сохранена строка в 5 символов, то в столце будет сохранено именно пять символов.
 - *NCHAR* - хранит строку в кодировке Unicode длиной от 1 до 4 000 символов. На каждый символ выделяется `2` байта.
 - *NVARCHAR* - хранит строку в кодировке Unicode. На каждый символ выделяется `2` байта. Можно задать конкретный размер от 1 до 4 000 символов. Если строка должна иметь больше 4000 символов, то задается размер MAX, а на хранение строки может выделяться до 2 Гб.

### <a name="datatype4"></a>[⇈](#top) Бинарные типы:
  - *BINARY* - хранит бинарные данные в виде последовательности от 1 до 8 000 байт.
 - *VARBINARY* хранит бинарные данные в виде последовательности от 1 до 8 000 байт, либо до 2^31–1 байт при использовании значения MAX (`VARBINARY(MAX)`).

### <a name="datatype5"></a>[⇈](#top) Остальное:
  - *UNIQUEIDENTIFIER* - уникальный идентификатор GUID (по сути строка с уникальным значением) (`16` байт).
 - *TIMESTAMP* - некоторое число, которое хранит номер версии строки в таблице (`8` - байт).
 - *CURSOR* - представляет набор строк.
 - *HIERARCHYID* - представляет позицию в иерархии.
 - *SQL_VARIANT* - может хранить данные любого другого типа данных T-SQL.
 - *XML* хранит документы XML или фрагменты документов XML (до `2` Гб).
 - *TABLE* - представляет определение таблицы.
 - *GEOGRAPHY* - хранит географические данные, такие как широта и долгота.
 - *GEOMETRY* - хранит координаты местонахождения на плоскости.

 ## <a name="job"></a>[⇈](#top) Работа с данными

### <a name="job1"></a>[⇈](#top) INSERT

INSERT служит для добавление данных к уже существующим таблицам. Пример использование:

#1 (без указания конкретных столбцов)

    INSERT table_Name VALUES(value, 'value', ... , 'value');

#2 (с указанием столбцов, если указать не все столбы то данные в этих столбцах будут равны либо значению по-умолчанию либо NULL)

    INSERT INTO table_Name(rowName1, rowName2, ... , rowNameN)
    VALUES (value, 'value', ... , 'value');

#3 (добавть несколько строк в одном запроссе)

    INSERT INTO table_Name(rowName1, rowName2, ... , rowNameN)
    VALUES
          (value, 'value', ... , 'value'),
          (value, 'value', ... , 'value'),
          (value, 'value', ... , 'value');

#4 (добавтьт строку со значениями по-умолчанию, все столбы должны обладать значениями по-умолчанию, в этом случае)

    INSERT INTO table_Name DEFAULT VALUES;


### <a name="job2"></a>[⇈](#top) SELECT

SELECT - используется для получения данных. Пример использования:

#1 (получить все строки из таблицы)

    SELECT * FROM table_Name;

#2 (получить талблицу с конкретными столбцами)

    SELECT rowName1, rowName2, ... , rowNameN FROM table_Name;

#3.1 (составные столбцы - значение из нескольких столбцов в одной ячейке, пример ниже покажет таблицу с одним столбцом в котором будут значения из двух столбцов)

    SELECT rownName1 + rowName2 FROM table_Name;

#3.2 (столбцы можно не только компоновать как строковые значения, можно еще совершать алгебраические операции *, /, +, -. С числовыми типами данных)

    SELECT rownName1 * rowName2 FROM table_Name;

#3.3 (недостатком предыдущих примеров является то, что столбцы в таком вызове не получат имени, необходимо использовать оператор `AS`)

    SELECT
    column1 + '(' + column2 + ')' AS column_Name1,
    column3 * + column4 AS 'column Name2'
    FROM table_Name;

### <a name="job3"></a>[⇈](#top) DISTINCT

Оператор DISTINCT используется для получения уникальных значений, например в таблице имеется несколько разных и несколько одинаковых моделей телефонов, этот оператор вернет все модели, убрав дубликаты

    SELECT DISTINCT column_Name FROM table_Name;

### <a name="job4"></a>[⇈](#top) SELECT INTO

Сочетание этих опреаторов служит для добавления данных из одной таблицы в другую. Примеры испоьзования:

#1 (в данном случае таблица не должна существовать, создается новая)

    SELECT column_Name1 + column_Name2 AS 'new column name'
    INTO new_table
    FROM old_table;

#2 (добавление данных из существующей таблицы в существующую, добавим данные из table_Name1 в table_Name2)

    INSERT INTO table_Name1
    SELECT
          column_Name1 + column_Name2,
          column_Name3 * column_Name4
    FROM table_Name2;

### <a name="job5"></a>[⇈](#top) ORDER BY

ORDER BY - сортировка. Пример испольщования:

#1 (отсортирует данные из таблицы `table_Name` по-возрастанию столбца `column_Name`)

    SELECT * FROM table_Name
    ORDER BY column_Name;

#2 (такойже как и предыдущий, явно указать, что сортировать нужно по-возрастанию `ASC`)

    SELECT * FROM table_Name
    ORDER BY column_Name ASC;

#3 (сортрока по-убыванию, использовать ключевое слово `DESC`)

    SELECT * FROM table_Name
    ORDER BY column_Name DESC;

#4 (несколько аргументов сортировки, если будут одинаковые поля `column_Name1` то они отсортируются по полю `column_Name2`)

    SELECT * FROM table_Name
    ORDER BY column_Name1 DESC, column_Name2 DESC;

#5 (сложный аргумент сортировки)

    SELECT * FROM table_Name
    ORDER BY column_Name1 * column_Name2 DESC;

### <a name="job6"></a>[⇈](#top) TOP

Оператор TOP позволяет выбрать определенное количество строк из таблицы. Пример использования:

#1 (выбрать 5 первых строк из `table_Name`, вместо * можно указать любые столбцы которые нужно получить)

    SELECT TOP 5 * from table_Name;

#2 (выбрать количество строк указав процентное значение с помощью оператора `PERCENT`)

    SELECT TOP 50 PERCENT * from table_Name;

### <a name="job7"></a>[⇈](#top) OFFSET и FETCH

Оператор TOP позволяет выбрать значения только от начала тоблицы, для выбора набора данных из любого места таблицы применяют оператор OFFSET. Важно! Этот оператор применяется только на отсортированные данные (ORDER BY). Пример испольщования:

#1 (в примере будут выбраны все строки начиная с третей, т.е. пропускаются две строки)

    SELECT * FROM table_Name
    ORDER BY Id OFFSET 2 ROWS;

#2 (в примере будут выбраны только две строки начиная с третей)

    SELECT * FROM table_Name
    ORDER BY Id OFFSET 2 ROWS FETCH NEXT 2 ROWS ONLY;

Пример выше может использоватся для реализации навигации по страницам, т.е. определенное количество элементов на странице.

### <a name="job8"></a>[⇈](#top) WHERE

WHERE используется для фильтрации.

    WHERE выражение

Операции сравнения:

  - **=** - сравнение на равенство
  - **<>** - сравнение на неравенство
  - **<** - меньше чем
  - **>** - больше чем
  - **!<** - не меньше чем
  - **!>** - не больше чем
  - **<=** - меньше чем или равно
  - **>=** - больше чем или равно

Пример использования (выбрать все записи в таблице у которых Id больше 20):

    SELECT * FROM table_Name
    WHERE Id > 20;

**Логические операторы:**

  - `AND` - и
  - `OR` - или
  - `NOT` - отрицание

Примеры:

#1 (с использованием AND - выбрать все записи Id котрых больше `8` и меньше `16`)

    SELECT * FROM table_Name
    WHERE Id > 8 AND Id < 16;

#2 (с использованием OR - выбрать записи с Id > `8` или именем которое равняется `name`)

    SELECT * FROM table_Name
    WHERE Id > 8 OR name = 'name';

#3 (с использованием NOT - выбрать все записи Id, которых не больше `20`)

    SELECT * FROM table_Name
    WHERE NOT Id > 20;

OR и AND можно использовать совместно ***НО*** AND имеет более высокий приоритет, как умножение в алгебре по сравнению с сложением.

### <a name="job9"></a>[⇈](#top) IS NULL

IS NULL используется для проверки поля на равенство с NULL. Примеры:

#1 (выбрать все записи, где `column_Name` равняется NULL)

    SELECT * FROM table_Name
    WHERE column_Name IS NULL;

#2 (выбрать все записи, где `column_Name` *не* равняется NULL)

    SELECT * FROM table_Name
    WHERE column_Name IS NOT NULL;

### <a name="job10"></a>[⇈](#top) IN

IN - оператор который применяется для фильтрации данных, позволяет указать набор данных который должен присутствовать. Например выберем только тех пользователей имена, которых равны либо Jane либо Maxin либо Anne:

    SELECT * from Users
    WHERE UserName IN('Jane', 'Maxin', 'Anne');

Можно, например, использовать оператор NOT, тогда буду выбраны пользователи имена, которых *не* равны указаным:

    SELECT * from Users
    WHERE UserName NOT IN('Jane', 'Maxin', 'Anne');

### <a name="job11"></a>[⇈](#top) BETWEEN

Этот оператор позволяет выбрать данные которые соответсвуют указаному диапазону. Пример:

    SELECT * from table_Name
    WHERE Price BETWEEN 100 AND 200;

В примере выше будут выбраны данные стоимость которых лежит в диапазоне 100 - 200, начало диапазона указывается после слова BETWEEN, а конец после слова AND.

### <a name="job12"></a>[⇈](#top) LIKE

Оператор LIKE принимает шаблон строки, котрому должно соответсвовать значения в поле или выражении.

Шаблоны:
  - **%** - соответствует любой подстроке, которая может иметь любое количество символов, при этом подстрока может и не содержать ни одного символа
  - **_** - соответствует любому одиночному символу
  - **[ ]** - соответствует одному символу, который указан в квадратных скобках
  - **[ - ]** - соответствует одному символу из определенного диапазона
  - **[ ^ ]** - соответствует одному символу, который не указан после символа ^

Примеры использования (например, выбор телефонов по маркам):

#1 (будут выбраны 'Galaxy Ace 2' или 'Galaxy S7')

    SELECT * FROM Phones
    WHERE name LIKE 'Galaxy%';

#2 (будут выбраны 'Galaxy S7' или 'Galaxy S8')

    SELECT * FROM Phones
    WHERE name LIKE 'Galaxy S_';

#3 (будут выбраны 'iPhone 7' или 'iPhone 8')

    SELECT * FROM Phones
    WHERE name LIKE 'iPhone [78]';

#4 (будут выбраны 'iPhone 6' или 'iPhone 7' или 'iPhone 8')

    SELECT * FROM Phones
    WHERE name LIKE 'iPhone [6-8]';

#5 (будут выбраны 'iPhone 6' или 'iPhone 8' *но* не будут выбраны 'iPhone 7' и 'iPhone 7S')

    SELECT * FROM Phones
    WHERE name LIKE 'iPhone [^7]%';

#6 (будут выбраны 'iPhone 7' или 'iPhone 8' *но* не будут выбраны 'iPhone 4' и 'iPhone 5S' и т.д.)

    SELECT * FROM Phones
    WHERE name LIKE 'iPhone [^1-6]%';

### <a name="job13"></a>[⇈](#top) UPDATE

UPDATE применяется для обновления (изменения) значений в полях, всех или выбраных с помощью WHERE.

Пример простого использования (в данном примере будут поднята стоимость всех телефонов на 100 и увеличено их количество на 5):

    UPDATE Phones
    SET Price = Price + 100, Count = Count + 5;

В этом примере применим фильтр для изменения определенных записей (пусть в таблице есть телефоны марки 'Nokia' заменим это название на 'Nokia Inc.')

    UPDATE Phones
    SET PhoneName = 'Nokia Inc.'
    WHERE PhoneName = 'Nokia';

А в этом примере используем сложный запрос, заменим 'Apple' на 'Apple Inc.' в первых двух записях:

    UPDATE Phones
    SET PhoneName = 'Apple Inc.'
    FROM (SELECT TOP 2 * FROM Phones
          WHERE PhoneName = 'Apple') AS Selected
    WHERE Phones.Id = Selected.Id;

В примере выше, делается выборка ~SELECT TOP 2 * FROM Phones~ которой с помощью оператора AS дается псевдоним Selected.

### <a name="job14"></a>[⇈](#top) DELETE

Удаление записей из таблицы.

Удалить все записи из таблицы (табица будет пуста):

    DELETE table_Name

Удалить конкретные записи, например удалить запись, Id, которой равно 9 (можно использовать более сложные условия):

    DELETE table_Name
    WHERE Id = 9;

Например удалим первых два пользователя с именем 'Jane':

    DELETE Users
    FROM (SELECT TOP 2 * FROM Users
          WHERE UserName = 'Jane') AS Selected
    WHERE Users.Id = Selected.Id;

## <a name="group"></a>[⇈](#top) Группировка
### <a name="group1"></a>[⇈](#top) Агрегатные функции

 - **AVG** - находит среднее значение
 - **SUM** - находит сумму значений
 - **MIN** - находит наименьшее значение
 - **MAX** - находит наибольшее значение
 - **COUNT** - находит количество строк в запросе

 Пример использования AVG, SUM, MIN, MAX (в данном примере будет вернута средняя цена для телефонов в таблице Phones. Также можно использовать фильтры и тому подобное):

    SELECT AVG(Price) FROM Phones;

Пример использования COUNT:

#1 (вернет количество всех строк в таблице):

    SELECT COUNT(*) FROM table_Name;

#2 (вернет количество строк в которых значение в `column_Name` не равно NULL)

    SELECT COUNT(column_Name) FROM table_Name;

По умолчанию, агрегатные функции не игнорируют дубликаты, т.е. функция COUNT посчитате все строки, даже с дубликатами т.е. по-умолчанию запрос выглядит так:

    SELECT COUNT(ALL column_Name) FROM table_Name;

Для того, что бы функция пропускала дубликаты необходимо указать оператор `DISTINCT` (пример вернут количество строк, пропуская дубликаты):

    SELECT COUNT(DISTINCT column_Name) FROM table_Name;

### <a name="group2"></a>[⇈](#top) GROUP BY

GROUP BY применяется для групировки данных. Пример использования:

    SELECT column1, column2, COUNT(*)
    FROM table_Name
    WHERE column1 > 15
    GROUP BY column1, column2
    ORDER BY column1;

Пример выше группирует данные по таблице `table_Name`, после `SELECT` пишутся столбцы которые необходимо выбрать, далее после `FROM` пишем таблцу откуда будут взяты эти столбцы, далее идет фильтраия данных (`WHERE`), а потом пишется `GROUP BY` и указываются столбы, которые были указаны после `SELECT` за исключением агригатных функций.

### <a name="group3"></a>[⇈](#top) HAVING

Эквивалентно `WHERE` за исключением того, что WHERE сортирует строки, а `HAVING` сортирует группы.

    SELECT PhoneName, PhonePrice, COUNT(*)
    FROM Phones
    WHERE PhonePrice > 15
    GROUP BY PhoneName, colPhonePriceumn2
    HAVING COUNT(*) > 5
    ORDER BY PhonePrice;

Пример выше делает то же, что и пример в [GROUP BY](#group2) только еще и сортирует группы т.е. выведит только те группы в которых количество телефонов с одинаковым именем больше 5, т.е. `COUNT(*) > 5` считает количество строк в исходной таблице с одинаковым значением PhoneName.

## <a name="req"></a>[⇈](#top) Подзапросы
Подзапросы - запросы которые выполняются внутри других запросов. Пример подзапроса:

    SELECT
       Name,
       Age,
       (SELECT FirstName FROM NameTable WHERE ID=UserInfo.UserID)
    FROM UserInfo

Подзапросы можно использовать не только в `SELECT` но и в других местах, таких как `WHERE`, `INSERT`, `DELETE`, `UPDATE`:

    SELECT * FROM Abonents
    WHERE Age > (SELECT AVG(Age) FROM Abonents)

Пример выше выберет все записи из таблицы Abonents у которых поле Age выше среднего.

### <a name="reqKorel"></a>[⇈](#top) Коррелирующие подзапросы

Собственно коррелирующие подзапросы - это запросы которые выполняются для каждой строки в запросе, т.е. подзапрос в примере выше не зависит от строк которые возвращает "главный" запрос.

Пример коррелирующего подзапроса:

    SELECT
       CellPhoneModel,
       CellPhoneManufacturer,
       (SELECT Price FROM Prices WHERE Prices.CellPhoneModel=CellPhones.CellPhoneModel)
    FROM CellPhones

В примере выше к таблице со (результат запроса) добавляется еще один столбец со стоимостью, занчение которго зависит от названия телефона (в данном случае стоит думать, что имена уникальны).
### <a name="reqAny"></a>[⇈](#top) Оператор ALL

При использовании оператора ALL в операция сравнения все значения для возвращающего значения должны быть `True`. Пример:

    SELECT * FROM CellPhones
    WHERE Price > ALL(SELECT Price FROM CellPhones WHERE Manufacturer='Apple')

Пример выше выберет только только те телефоны стоимость которых выше чем стоимость всех телефонов фирмы Apple но этот запрос можно написать иначе без использования оператора `ALL`:

    SELECT * FROM CellPhones
    WHERE Price > (SELECT MAX(Price) FROM CellPhones WHERE Manufacturer='Apple')

### <a name="reqSomeAny"></a>[⇈](#top) Оператор SOME и ANY

Эти операторы работают одинаково и какой выбрать не имеет значения. Оператор `ANY` означает, что при сравнении должен соответсвовать хотябы одному. Пример:

    SELECT * FROM CellPhones
    WHERE ScreenSize = ANY(SELECT * FROM CellPhones WHERE Manufacturer='Apple')

Запрос выше вернет телефоны с размерами экранов такими как у телефонов фирмы Apple.

### <a name="reqExisrs"></a>[⇈](#top) Оператор EXISTS

Оператор EXISTS возвращает `True` если подзапрос возвращает хотябы одну строку. Пример:

    SELECT * FROM Abonents
    WHERE EXISTS(SELECT * FROM Orders WHERE Orders.AbonentID=Abonents.ID)

В этом примере, запрос вернет всех абонентов для которых есть запись в таблице с заказами т.е. тех которые совершали заказы, например в онлайн-магазине.

## <a name="join"></a>[⇈](#top) Соединение таблиц

Соеденять таблицы можно неявно, т.е. таким запросом:

    SELECT * FROM Abonents, Numbers

Этот запрос выедет перекрестное соединение, каждой строчке из `Abonents` будут выведены все строчки из `Numbers` (если в обеих таблицах по три строки будут выведены 9 строк). Мы получим все строки из обеих таблиц, если необходимо получить строки только зависящие друг от друга делаем так:

    SELECT * FROM Abonents, Numbers
    WHERE Numbers.AbonentID=Abonents.ID

Результатом этого запроса будет таблица с абонентами и номерами которые им принадлежат. Таким образом можно соединять не только две таблици.
Пример с использованием `псевдонимов`:

    SELECT A.FirstName, N.PrimaryNumber, AI.*
    FROM  Abonents as A, Numbers as N, AbonentsInfo as AI
    WHERE A.ID=N.AbonentID AND A.ID=AI.AbonentID

В запросе выше в таблицу будут добавлены: столбец FirstName из Abonents, PrimaryNumber из Numbers и все столбцы из AbonentsInfo.

### <a name="joinInner"></a>[⇈](#top) Оператор INNER JOIN

Оператор INNER JOIN используется для явного соединения таблиц. Пример использование:

    SELECT Abonents.FirstName, Abonents.Address, Numbers.Numbers
    FROM  Abonents
    INNER JOIN Numbers ON Numbers.AbonentID=Abonents.ID

Оператор пишется после ключевого слова `FROM`, наличие `INNER` необязательно, после JOIN идет название таблицы которую нужно присоеденить, а после ON указываем условие по которому соединять таблицы. Также как и с неявным соединением можно соединять больее двух таблиц.

    SELECT A.FirstName, N.PrimaryNumber, AI.Address
    FROM Abonents as A
    JOIN Numbers as N ON Numbers.AbonentID=Abonents.ID
    JOIN AbonentsInfo as AI ON AbonentsInfo.AbonentID=Abonents.ID

Благодаря явному соединению столбцы присоединенных таблиц можно использовать в фильтрации:

    SELECT Abonents.FirstName, Abonents.Address, Numbers.Numbers
    FROM  Abonents
    JOIN Numbers ON Numbers.AbonentID=Abonents.ID
    WHERE Number.IsPrimary=1

Пример выше выберет только те номера, которые являются главными.

### <a name="joinOuter"></a>[⇈](#top) Оператор OUTER JOIN

Оператор OUTER JOIN используется для внешнего соединения таблиц, слово OUTER заменяется LEFT, RIGHT или FULL.

 * LEFT - берет все строки из `левой` таблицы
 * RIGHT - берет все строки из `правой` таблицы
 * FULL - берет `все` строки из двух таблиц

Пример:

    SELECT Name, Address, Number
    FROM Abonents LEFT JOIN AbonentsInfo ON Abonents.ID=AbonentsInfo.AbonentID

В примере выше:
 * `Abonents` - левая таблицам
 * `AbonentsInfo` - правая таблица

Может показатся, что LEFT JOIN соответсвует INNER JOIN но это `не так`, INNER JOIN добавляет строки только если они соответствуют условию, напрример, две таблицы (Абоненты, Тел. Номера), если в таблице нет номера для абонента то строка с этим абонентом не будет добавлена к результату, а с использованием LEFT JOIN будут добавлены все абоненты, даже если для них не нашлось номера в таблице номеров, т.е. сначала добавляются все строки из `левой` таблицы, а потом по условию к ним добавляются данные из `правой` таблицы.

|Name|Address|Number|
|---|---|---|
| Том | США | 00278 |
| Джон | Голандия | `NULL` |

В таблице выше для Джона не нашлось номера телефона. RIGHT JOIN добавляет все строки из `правой`, а потом по условию с `левой`:

|Name|Address|Number|
|---|---|---|
| Генри | Россия | 00273 |
| `NULL` | `NULL` | 00321 |

### <a name="joinUnion"></a>[⇈](#top) Оператор UNION

Оператор UNION используется для соединения строк (JOIN соединяет столбцы). Т.е. UNION соединяет однотипные надоры данных. Например есть две таблицы в которые есть два одинаковых поля (столбцы FirstName, LastName):

    SELECT FirstName, LastName
    FROM Abonents
    UNION SELECT FirstName, LastName FROM AbonentsAfrica

В итоге мы получим таблицу с именами абонентов из двух таблиц. Для верного соединения количество столбцов должно быть равно и типы этих столбцов должны соответсвувать дург другу.

Повторяющиеся данные будут добавлены один рас. Для добавления всех данных необходимо использовать оператор `ALL`:

    SELECT FirstName, LastName
    FROM Abonents
    UNION ALL SELECT FirstName, LastName FROM AbonentsAfrica

Со столбцами можно производить арифметические операции, например в первой таблице (FirstName, SecondName,LastName, Balance), а во второй (Name, Balance):

    SELECT  FirstName + ' ' + SecondName + ' ' + LastName, Balance
    FROM Abonents
    UNION SELECT Name, Balance FROM AbonentsAfrica

### <a name="joinExcept"></a>[⇈](#top) Оператор EXCEPT

Оператор EXCEPT используется для нахождения разности выборок данных, т.е. вернет те строки которые есть в первой выборке, а во второй их нет. Пример:

    SELECT Name FROM Abonents
    EXCEPT SELECT Name from AbonentsAfrica

### <a name="joinIntersect"></a>[⇈](#top) Оператор INTERSECT

Оператор INTERSECT используется для нахождения пересичения выборок данных, т.е. возвращает строки которые есть в первой и второй выборке данных. Пример:

    SELECT Name FROM Abonents
    INTERSECT SELECT Name from AbonentsAfrica

## <a name="func"></a>[⇈](#top) Встроенные функции

### <a name="funcStr"></a>[⇈](#top) Функции для работы со строками

`LEN`: возвращает количество символов в строке.

    SELECT LEN('Hello')  -- 5

`LTRIM`: удаляет начальные пробелы из строки.

`RTRIM`: удаляет конечные пробелы из строки.

    SELECT LTRIM('  Hello')     --Hello
    SELECT RTRIM('Hello    ')   --Hello

`CHARINDEX`: возвращает индекс, по которому находится первое вхождение подстроки в строке.

    SELECT CHARINDEX('World', 'Hello World!!!') -- 7

`LEFT`: вырезает с начала строки определенное количество символов.

`RIGHT`: вырезает с конца строки определенное количество символов.

    SELECT LEFT('Hello', 3)     -- Hel
    SELECT RIGHT('Hello', 3)    -- llo

`SUBSTRING`: вырезает из строки подстроку определенной длиной, начиная с определенного индекса.

    SELECT SUBSTRING('Vadim Verloka', 7, 7)    -- Verloka

`REPLACE`: заменяет одну подстроку другой в рамках строки.

    SELECT REPLACE('Vadim ???', '???', 'Verloka')   -- Vadim Verloka

`REVERSE`: переворачивает строку наоборот.

    SELECT REVERSE('123456789')   -- 987654321

`CONCAT`: объединяет две строки в одну. В качестве параметра принимает от 2-х и более строк.

    SELECT CONCAT('Vadim', ' ', 'Verloka')  -- Vadim Verloka

`LOWER`: переводит строку в нижний регистр.

`UPPER`: переводит строку в верхний регистр.

    SELECT LOWER('Verloka')     -- verloka
    SELECT UPPER('Vadim')       -- VADIM

`SPACE`: возвращает строку, которая содержит определенное количество пробелов

    SELECT SPACE(5)   -- "     "

### <a name="funcInt"></a>[⇈](#top) Функции для работы с числами

`ROUND`: округляет число. Первый аргумент - то, что нужно округлить, а второй до какой цифры округлять. Если второй аргумент положительный то это указыает до какой цифры *после запятой* округлять, а если отрицательный то до какой цифры *до запятой* округлять.

    SELECT ROUND(1737.1737, 2)   -- 1737.1700
    SELECT ROUND(1737.1737, -2)  -- 1700.0000

`ISNUMERIC`: определяет, является ли значение числом.

    SELECT ISNUMERIC(1737.1737)         -- 1
    SELECT ISNUMERIC('1737.1737')       -- 1
    SELECT ISNUMERIC('Verloka')         -- 0
    SELECT ISNUMERIC('13-04-1867')      -- 0

`ABS`: возвращает абсолютное значение числа.

    SELECT ABS(-123)    -- 123

`CEILING`: возвращает наименьшее целое число, которое больше или равно текущему значению.

`FLOOR`: возвращает наибольшее целое число, которое меньше или равно текущему значению.

    SELECT CEILING(-123.45)     -- -123
    SELECT CEILING(123.45)      -- 124
    SELECT FLOOR(-123.45)       -- -124
    SELECT FLOOR(123.45)        -- 123

`SQUARE`: возводит число в квадрат.

`SQRT`: получает квадратный корень числа.

    SELECT SQUARE(4)        -- 16
    SELECT SQRT(16)         -- 4

`RAND`: генерирует случайное число с плавающей точкой в диапазоне от 0 до 1.

    SELECT RAND()       -- 0.3424234232151221

`COS`: возвращает косинус угла, выраженного в радианах.

`SIN`: возвращает синус угла, выраженного в радианах.

`TAN`: возвращает тангенс угла, выраженного в радианах

    SELECT COS(1.0472)  -- 0.5 - 60 градусов
    SELECT SIN(1.5708)  -- 1   - 90 градусов
    SELECT TAN(0.7854)  -- 1   - 45 градусов

### <a name="funcDt"></a>[⇈](#top) Функции по работе с датами и временем

`GETDATE`: возвращает текущую локальную дату и время (обьект datetime).

`SYSDATETIME`: возвращает текущую локальную дату и время (обьект datetime2).

    SELECT GETDATE()        -- 2019-01-22 20:29:55.830
    SELECT SYSDATETIME()    -- 2019-01-22 20:29:55.7446744

`GETUTCDATE`: возвращает текущую локальную дату и время по гринвичу (UTC/GMT) (обьект datetime).

`SYSUTCDATETIME`: возвращает текущую локальную дату и время по гринвичу (UTC/GMT)  (обьект datetime2).

    SELECT GETUTCDATE()         -- 2019-01-22 17:29:55.830
    SELECT SYSUTCDATETIME()     -- 2019-01-22 17:29:55.5202777

`SYSDATETIMEOFFSET`: возвращает объект, который содержит дату и время относительно GMT

    SELECT SYSDATETIMEOFFSET()      -- 2017-07-28 21:02:22.7446744 +03:00

`DAY`: возвращает день даты, который передается в качестве параметра.

`MONTH`: возвращает месяц даты.

`YEAR`: возвращает год из даты.

    SELECT DAY(GETDATE())       -- 22
    SELECT MONTH(GETDATE())     -- 1
    SELECT YEAR(GETDATE())      -- 2019

`DATENAME`: возвращает часть даты в виде строки. Параметр выбора части даты передается в качестве первого параметра, а сама дата передается в качестве второго параметра.

    SELECT DATENAME(month, GETDATE())       -- January

Для определения части даты можно использовать следующие параметры (в скобках указаны их сокращенные версии):
 * year (yy, yyyy): год
 * quarter (qq, q): квартал
 * month (mm, m): месяц
 * dayofyear (dy, y): день года
 * day (dd, d): день месяца
 * week (wk, ww): неделя
 * weekday (dw): день недели
 * hour (hh): час
 * minute (mi, n): минута
 * second (ss, s): секунда
 * millisecond (ms): миллисекунда
 * microsecond (mcs): микросекунда
 * nanosecond (ns): наносекунда
 * tzoffset (tz): смешение в минутах относительно гринвича.

`DATEPART`: возвращает часть даты в виде числа. Параметр выбора части даты передается в качестве первого параметра (используются те же параметры, что и для DATENAME), а сама дата передается в качестве второго параметра.

    SELECT DATEPART(month, GETDATE())       -- 1

`DATEADD`: возвращает дату, которая является результатом сложения числа к определенному компоненту даты.

    SELECT DATEADD(month, 2, '2000-1-14')       -- 2000-03-14 00:00:00.000
    SELECT DATEADD(day, 5, '2000-1-14')         -- 2017-01-19 00:00:00.000
    SELECT DATEADD(day, -5, '2000-1-14')        -- 2017-01-09 00:00:00.000

`DATEDIFF`: возвращает разницу между двумя датами. Первый параметр - компонент даты, который указывает, в каких единицах стоит измерять разницу. Второй и третий параметры - сравниваемые даты.

    SELECT DATEDIFF(year, '2017-7-28', '2018-9-28')     -- разница 1 год
    SELECT DATEDIFF(month, '2017-7-28', '2018-9-28')    -- разница 14 месяцев
    SELECT DATEDIFF(day, '2017-7-28', '2018-9-28')      -- разница 427 дней

`TODATETIMEOFFSET`: возвращает значение datetimeoffset, которое является результатом сложения временного смещения с другим объектом datetimeoffset.

`SWITCHOFFSET`: возвращает значение datetimeoffset, которое является результатом сложения временного смещения с объектом datetime2.

    SELECT TODATETIMEOFFSET('2017-7-28 01:10:22', '+03:00')
    SELECT SWITCHOFFSET(SYSDATETIMEOFFSET(), '+02:30')

`EOMONTH`: возвращает дату последнего дня для месяца, который используется в переданной в качестве параметра дате. В качестве необязательного второго параметра можно передавать количество месяцев, которые необходимо прибавить к дате. Тогда последний день месяца будет вычисляться для новой даты.

    SELECT EOMONTH('2019-02-05')    -- 2019-02-28
    SELECT EOMONTH('2019-02-05', 3) -- 2019-05-31

`DATEFROMPARTS`: по году, месяцу и дню создает дату.

    SELECT DATEFROMPARTS(2019, 1, 22)       -- 2019-01-22

`ISDATE`: проверяет, является ли выражение датой.

    SELECT ISDATE('2019-01-22')     -- 1
    SELECT ISDATE('2019-22-01')     -- 0
    SELECT ISDATE('22-01-2019')     -- 0
    SELECT ISDATE('Verloka')        -- 0

### <a name="funcCvt"></a>[⇈](#top) Преобразование данных

В тех случаях, когда необходимо выполнить преобразования от типов с высшим приоритетом к типам с низшим приоритетом, то надо выполнять явное приведение типов. Для этого в T-SQL определены две функции: `CONVERT` и `CAST`.

Функция CAST преобразует выражение одного типа к другому. Пример:

    CAST(AbonentName AS nvarchar)

Большую часть преобразований охватывает функция CAST. Если же необходимо какое-то дополнительное форматирование, то можно использовать функцию CONVERT. Третим параметром этой функции необходимо передать стиль преобразования. Пример:

    CONVERT(datetime, GETDATE(), 108)     -- 20:47:12 - time format hh:mm:ss

При использовании функций CAST и CONVERT SQL Server выбрасывает исключение, если данные нельзя привести к определенному типу. Чтобы избежать генерации исключения можно использовать функцию `TRY_CONVERT`. Ее использование аналогично функции CONVERT за тем исключением, что если выражение не удается преобразовать к нужному типу, то функция возвращает `NULL`.

    SELECT TRY_CONVERT(int, 'Verloka')      -- NULL
    SELECT TRY_CONVERT(int, '66')       -- 66

`STR`: преобразует число в строку. Второй параметр указывает на длину строки, а третий - сколько знаков в дробной части числа надо оставлять.

`CHAR`: преобразует числовой код ASCII в символ. Нередко используется для тех ситуаций, когда необходим символ, который нельзя ввести с клавиатуры.

`ASCII`: преобразует символ в числовой код ASCII.

`NCHAR`: преобразует числовой код UNICODE в символ.

`UNICODE`: преобразует символ в числовой код UNICODE.

    SELECT STR(123.4567, 6,2)   -- 123.46
    SELECT CHAR(219)            --  Ы
    SELECT ASCII('Ы')           -- 219
    SELECT NCHAR(1067)          -- Ы
    SELECT UNICODE('Ы')         -- 1067

#### <a name="funcChart"></a>[⇈](#top) График преобразований

[![chart](https://raw.githubusercontent.com/verloka/SQLFromZero/master/merch/chart.jpg)](https://github.com/verloka/CPPFromZero)

### <a name="funcIF"></a>[⇈](#top) Функции CASE и IIF

Функция `CASE` проверяет значение некоторого выражение, и в зависимости от результата проверки может возвращать тот или иной результат. Пример использования:

    SELECT AbonentName, AbonentBalance,
        CASE AbonentBalance
            WHEN 0 THEN 'Баланс исчерпан'
            WHEN 1 THEN 'На балансе мало'
            WHEN 2 THEN 'На балансе нормально'
            ELSE 'На балансе много'
        END AS AbonentBalanceString
    FROM Abonents

вторая форма записи:

    SELECT AbonentName,
        CASE
            WHEN ServiceCost > 50000 THEN 'Дорого!'
            WHEN ServiceCost BETWEEN 40000 AND 50000 THEN 'Нормально'
            WHEN ServiceCost BETWEEN 30000 AND 40000 THEN 'Дёшево'
            ELSE 'Бесплатно'
        END AS ServiceCostString
    FROM Abonents

Функция `IIF` в зависимости от результата условного выражения возвращает одно из двух значений. Общая форма функции выглядит следующим образом:

    IIF(условие, значение_1, значение_2)

пример использования:

    SELECT
      AbonentName,
      AbonentNumber,
      IIF(AbonentBalance > 3000, 'На балансе достаточно средств.', 'Не достаточно средств.')
    FROM Abonents

### <a name="funcOther"></a>[⇈](#top) Функции NEWID, ISNULL и COALESCE

`NEWID()` - используется для генерации уникаьного значения (обьект UNIQUEIDENTIFIER).

`ISNULL` - проверяет значение некоторого выражения. Если оно равно NULL, то функция возвращает значение, которое передается в качестве второго параметра. Пример:

    SELECT AbonentName,
        ISNULL(AbonentNumber, 'Номер не указан') AS AbonentNumber,
        ISNULL(AbonentMail, 'Почта не указана') AS AbonentMail
    FROM Abonents

`COALESCE` - принимает список значений и возвращает первое из них, которое не равно NULL:

    SELECT AbonentName,
        COALESCE(AbonentNumber, AbonentMail, 'не указано') AS Contacts
    FROM Abonents
