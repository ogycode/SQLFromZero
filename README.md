[![Logo](https://raw.githubusercontent.com/ogycode/SQLFromZero/master/merch/logo.jpg)](https://github.com/ogycode/CPPFromZero)

# SQL from Zero
**I study the technology and this is my "note-book"**

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

## <a name="create"></a>Создание БД
### <a name="create1"></a>*Системные базы данных, которые нужны для работы SQL Server'a:*
 - **master** - главная БД, без нее не будет работать сервер
 - **model** - шаблон для создания других БД
 - **msdb** - хранит информацию о планировщике и бэкапах
 - **tempdb** - хранилище для воеменных обьектов, пересоздается при каждом запуске сервера

### <a name="create2"></a>*Ключевые слова при создании БД:*
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

### <a name="create3"></a>Пример запроса к БД:
    SELECT * FROM [Имя БД].[Схема].[Имя таблицы]

*Пример:*

    SELECT * FROM bdName.dbo.Users

Этот запрос выведет все записи из таблици Users (Выбрать ВСЕ из БД). Если необходимо выполнить несколько запросов к одной таблице:

    USE [Имя БД] 
    SELECT * FROM [Имя таблицы]

*Пример:*
 
    USE bdName
    SELECT * FROM Users


## <a name="base"></a>Основы

### <a name="base1"></a>Удаление и создание БД
 Для создания БД необходимо выполнить команду

 `CREATE DATABASE BD_NAME`, где BD_NAME - это имя базы данных.

Для прикрипления уже существующих БД к серверу необходимо выполнить 

    CREATE DATABASE BD_NAME
    ON PRIMARY(FILENAME='path')
    FOR ATTACH;

 Для удаления БД необходимо выполнить `DROP DATABASE BD_NAME`

### <a name="base2"></a>Таблицы
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

### <a name="base3"></a>Удаление таблицы
    DROP TABLE TABLE_NAME

### <a name="base4"></a>Переименование таблицы

    USE BD_NAME;
    EXEC sp_rename 'OLD_NAME', 'NEW_NAME';

`sp_rename` - инструкция которая позволяет переименовывать таблицы, столбцы.

### <a name="base5"></a>Первичный ключ

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

### <a name="base6"></a>Атрибут IDENTITY

Атрибут IDENTITY указывает, что столбец при добавлении новой будет инкремироватся на указаное число. Можно применять только к числовым значениям.

`IDENTITY(seed, increment)`, где seed - место, с которого будет инкремирование, increment - число на которое будет происходить инкремирование. Например:

    CREATE TABLE table_Name
    {
        Id INT PRIMARY KEY IDENTITY(1, 1),
        Number INT
    };
инкремироватся будет столбец Id на еденицу при добавлении новой записи.

### <a name="base7"></a>Атрибут UNIQUE
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

### <a name="base8"></a>Атрибут DEFAULT

Этот атрибут указывает какое значение будет записано по умолччанию в ячейку. Пример использования:

    CREATE TABLE table_Name
    {
        Id INT PRIMARY KEY,
        Number INT DEFAULT 44
    };
исходя из этой записи в поле Number будет записано значение 44, по-умолчанию.

### <a name="base9"></a>Атрибут CHECK

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

### <a name="base10"></a>Оператор CONSTRAINT

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

### <a name="base11"></a>Внешние ключи

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

### <a name="base12"></a>ON DELETE и ON UPDATE

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

### <a name="base13"></a>Изменение существующей таблицы

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

### <a name="base14"></a>Команда `Go`

Команда `Go` служит указателем того, что сначала нужно дождатся окончания операций определенных перед ним. Пример:

    CREATE DATABASE DB_NAME;
    GO

    USE DB_NAME;

    CREATE TABLE table_Name
    {
        Id INT PRIMARY KEY
    };

Из текста выше следует, что перед созданием таблицы `table_Name` необходимо дождатся создания базы данных `DB_NAME`. Это и делает команда `Go`.

## <a name="datatype"></a>Типы данных

### <a name="datatype1"></a>Числовые:
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

### <a name="datatype2"></a>Дата и время:
  - *DATE* - хранит даты от 0001-01-01 (1 января 0001 года) до 9999-12-31 (31 декабря 9999 года) (`3` байта).
 - *TIME* - хранит время в диапазоне от 00:00:00.0000000 до 23:59:59.9999999 (`3` - `5` байт). Может иметь форму `TIME(n)`, где `n` представляет количество цифр от 0 до 7 в дробной части секунд.
 - *DATETIME* - хранит даты и время от 01/01/1753 до 31/12/9999 (`8` байт).
 - *DATETIME2* - хранит даты и время в диапазоне от 01/01/0001 00:00:00.0000000 до 31/12/9999 23:59:59.9999999 (`6` - `8` байт). Может иметь форму `DATETIME2(n)`, где `n` представляет количество цифр от 0 до 7 в дробной части секунд.
 - *SMALLDATETIME* - хранит даты и время в диапазоне от 01/01/1900 до 06/06/2079 (`4` байта).
 - *DATETIMEOFFSET* - хранит даты и время в диапазоне от 0001-01-01 до 9999-12-31. Сохраняет детальную информацию о времени с точностью до 100 наносекунд (`10` байт).

### <a name="datatype3"></a>Строковые:
  - *CHAR* хранит строку длиной от 1 до 8 000 символов. На каждый символ выделяет по `1` байту. Не подходит для многих языков, так как хранит символы не в кодировке Unicode. Количество символов, которое может хранить столбец, передается в скобках. Например, для столбца с типом `CHAR(10)` будет выделено `10` байт. И если мы сохраним в столбце строку менее 10 символов, то она будет дополнена пробелами.
 - *VARCHAR* - хранит строку. На каждый символ выделяется `1` байт. Можно указать конкретную длину для столбца - от 1 до 8 000 символов, например, `VARCHAR(10)`. Если строка должна иметь больше 8000 символов, то задается размер MAX, а на хранение строки может выделяться до 2 Гб: `VARCHAR(MAX)`. Не подходит для многих языков, так как хранит символы не в кодировке Unicode. В отличие от типа `CHAR` если в столбец с типом `VARCHAR(10)` будет сохранена строка в 5 символов, то в столце будет сохранено именно пять символов.
 - *NCHAR* - хранит строку в кодировке Unicode длиной от 1 до 4 000 символов. На каждый символ выделяется `2` байта.
 - *NVARCHAR* - хранит строку в кодировке Unicode. На каждый символ выделяется `2` байта. Можно задать конкретный размер от 1 до 4 000 символов. Если строка должна иметь больше 4000 символов, то задается размер MAX, а на хранение строки может выделяться до 2 Гб.

### <a name="datatype4"></a>Бинарные типы:
  - *BINARY* - хранит бинарные данные в виде последовательности от 1 до 8 000 байт.
 - *VARBINARY* хранит бинарные данные в виде последовательности от 1 до 8 000 байт, либо до 2^31–1 байт при использовании значения MAX (`VARBINARY(MAX)`).

### <a name="datatype5"></a>Остальное:
  - *UNIQUEIDENTIFIER* - уникальный идентификатор GUID (по сути строка с уникальным значением) (`16` байт).
 - *TIMESTAMP* - некоторое число, которое хранит номер версии строки в таблице (`8` - байт).
 - *CURSOR* - представляет набор строк.
 - *HIERARCHYID* - представляет позицию в иерархии.
 - *SQL_VARIANT* - может хранить данные любого другого типа данных T-SQL.
 - *XML* хранит документы XML или фрагменты документов XML (до `2` Гб).
 - *TABLE* - представляет определение таблицы.
 - *GEOGRAPHY* - хранит географические данные, такие как широта и долгота.
 - *GEOMETRY* - хранит координаты местонахождения на плоскости.

 ## <a name="job"></a>Работа с данными

### <a name="job1"></a>INSERT

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


### <a name="job2"></a>SELECT

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

### <a name="job3"></a>DISTINCT

Оператор DISTINCT используется для получения уникальных значений, например в таблице имеется несколько разных и несколько одинаковых моделей телефонов, этот оператор вернет все модели, убрав дубликаты

    SELECT DISTINCT column_Name FROM table_Name;

### <a name="job4"></a>SELECT INTO

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

### <a name="job5"></a>ORDER BY

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

### <a name="job6"></a>TOP

Оператор TOP позволяет выбрать определенное количество строк из таблицы. Пример использования:

#1 (выбрать 5 первых строк из `table_Name`, вместо * можно указать любые столбцы которые нужно получить)

    SELECT TOP 5 * from table_Name;

#2 (выбрать количество строк указав процентное значение с помощью оператора `PERCENT`)

    SELECT TOP 50 PERCENT * from table_Name;

### <a name="job7"></a>OFFSET и FETCH

Оператор TOP позволяет выбрать значения только от начала тоблицы, для выбора набора данных из любого места таблицы применяют оператор OFFSET. Важно! Этот оператор применяется только на отсортированные данные (ORDER BY). Пример испольщования:

#1 (в примере будут выбраны все строки начиная с третей, т.е. пропускаются две строки)

    SELECT * FROM table_Name
    ORDER BY Id OFFSET 2 ROWS;

#2 (в примере будут выбраны только две строки начиная с третей)

    SELECT * FROM table_Name
    ORDER BY Id OFFSET 2 ROWS FETCH NEXT 2 ROWS ONLY;

Пример выше может использоватся для реализации навигации по страницам, т.е. определенное количество элементов на странице.