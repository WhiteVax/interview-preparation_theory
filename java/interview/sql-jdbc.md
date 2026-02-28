## *SQL-JDBC*

- [Что такое SQL?](#1-что-такое-sql)
- [Что такое DML и DDL?](#2-что-такое-dml-и-ddl)
- [Что такое первичный ключ?](#3-что-такое-первичный-ключ)
- [Что такое внешний ключ?](#4-что-такое-внешний-ключ)
- [Какие виды связей между таблицами существуют и как они организуются?](#5-какие-виды-связей-между-таблицами-существуют-и-как-они-организуются)
- [Опишите как вставить, удалить, обновить данные в(из) таблицу(ы).](#6-опишите-как-вставить-удалить-обновить-данные-виз-таблицуы)
- [Что такое нормализация БД?](#7-что-такое-нормализация-бд)
- [Что такое денормализация БД? Для чего она нужна?](#8-что-такое-денормализация-бд-для-чего-она-нужна)
- [Что такое кластерный и некластерный индексы?](#9-что-такое-кластерный-и-некластерный-индексы)
- [Какие типы соединений (join) таблиц существуют? В чем их разница?](#10-какие-типы-соединений-join-таблиц-существуют-в-чем-их-разница)
- [Что такое SQL курсор?](#11-что-такое-sql-курсор)
- [Опишите шаги по созданию и использованию курсора.](#12-опишите-шаги-по-созданию-и-использованию-курсора)
- [Что такое транзакция?](#13-что-такое-транзакция)
- [Что такое триггер? Какие типы триггеров Вы знаете?](#14-что-такое-триггер-какие-типы-триггеров-вы-знаете)
- [В чем разница между where и having?](#15-в-чем-разница-между-where-и-having)
- [Что такое подзапрос (sub-query)?](#16-что-такое-подзапрос-sub-query)
- [Что такое union?](#17-что-такое-union)
- [Что такое group by?](#18-что-такое-group-by)
- [Что такое хранимые процедуры?](#19-что-такое-хранимые-процедуры)
- [Что такое view (Представление)?](#20-что-такое-view-представление)
- [Что такое JDBC?](#21-что-такое-jdbc)
- [Что нужно для работы с той или иной БД?](#22-что-нужно-для-работы-с-той-или-иной-бд)
- [Как зарегистрировать драйвер?](#23-как-зарегистрировать-драйвер)
- [Как получить Connection?](#24-как-получить-connection)
- [Что такое Statement, PreparedStatement? В чем разница между ними?](#25-что-такое-statement-preparedstatement-в-чем-разница-между-ними)
- [Что такое ResultSet?](#26-что-такое-resultset)
- [В чем разница между методами execute, executeUpdate, executeQuery?](#27-в-чем-разница-между-методами-execute-executeupdate-executequery)
- [Можно ли использовать возвращаемое значение execute() для проверки, что что-то обновилось?](#28-можно-ли-использовать-возвращаемое-значение-execute-для-проверки-что-что-то-обновилось)
- [Как получить при вставке сгенерированные ключи? Как это сделать на чистом sql?](#29-как-получить-при-вставке-сгенерированные-ключи-как-это-сделать-на-чистом-sql)
- [Для чего используется конструкция try-with-resources?](#30-для-чего-используется-конструкция-try-with-resources)

---

### 1. Что такое SQL?
   SQL (Structured Query Language) — декларативный язык для работы с реляционными БД. Он используется для создания, модификации, извлечения и удаления данных из баз данных
   
Подмножества:
   +	DDL — структура (CREATE, ALTER, DROP)
   +	DML — данные (INSERT, UPDATE, DELETE, SELECT)
   +	DCL — права (GRANT, REVOKE)
   +	TCL — транзакции (COMMIT, ROLLBACK)

Особенности:
   +	декларативный (описываете что, а не как)
   +	есть диалекты: PostgreSQL, MySQL, Oracle, SQL Server

### 2. Что такое DML и DDL?
DDL (Data Definition Language)

DDL — это подмножество SQL, которое используется для создания и изменения структуры объектов базы данных.

Объекты:
+ таблицы
+ индексы
+ представления (`VIEW`)
+ схемы
+ последовательности (SEQUENCE)
+ ограничения (constraints)

Основные команды:
+ `CREATE`
+ `ALTER`
+ `DROP`
+ `TRUNCATE`
+ `RENAME`
```sql
CREATE TABLE employees (
    id INT PRIMARY KEY,
    name VARCHAR(100),
    salary DECIMAL(10,2)
);

ALTER TABLE employees ADD email VARCHAR(100);

DROP TABLE employees;
```
Нюансы DDL

*Автокоммит*

В большинстве СУБД (Oracle, MySQL, SQL Server):
+ DDL выполняет implicit commit

В PostgreSQL:
+ DDL можно откатить, если он внутри транзакции
```sql
BEGIN;
CREATE TABLE test(id INT);
ROLLBACK;  -- В PostgreSQL таблица не создастся
```

*Блокировки*
 
DDL часто ставит exclusive lock

Таблица может быть недоступна другим сессиям

      TRUNCATE

Это *DDL*, а не DML

Особенности:
+ очень быстрый
+ очищает таблицу полностью
+ обычно нельзя откатить (зависит от СУБД)
+ сбрасывает AUTO_INCREMENT

DML (Data Manipulation Language)

DML — используется для работы с данными внутри таблиц.

Основные команды:
+ `INSERT`
+ `UPDATE`
+ `DELETE`
+ `SELECT`
+ `MERGE` (в некоторых СУБД)

Примеры DML
```sql
INSERT
INSERT INTO employees (id, name, salary)
VALUES (1, 'John Doe', 50000);
UPDATE
UPDATE employees
SET salary = 60000
WHERE id = 1;
DELETE
DELETE FROM employees
WHERE id = 1;
SELECT
SELECT * FROM employees WHERE salary > 40000;
```
Нюансы DML

1. Работает в транзакциях

Можно откатить:
```sql
BEGIN;
UPDATE employees SET salary = 0;
ROLLBACK;  -- изменения отменены
```
2. Вызывает триггеры

DML-операции могут активировать:

+ `BEFORE INSERT`
+ `AFTER UPDATE`
+ `BEFORE DELETE`
+ DDL — обычно не вызывает триггеры.

  3. Блокировки строк


     INSERT/UPDATE/DELETE блокируют строки

Это лучше для конкурентности, чем блокировка всей таблицы

4. Нюанс


      UPDATE employees SET salary = 0;

Без `WHERE` → обновит все строки.

| Характеристика | DDL                 | DML                    |
| -------------- | ------------------- | ---------------------- |
| Что изменяет   | Структуру           | Данные                 |
| Транзакции     | Часто автокоммит    | Можно rollback         |
| Блокировки     | Таблица/объект      | Обычно строки          |
| Триггеры       | Нет (обычно)        | Да                     |
| Примеры        | CREATE, ALTER, DROP | INSERT, UPDATE, DELETE |

|          | DELETE   | TRUNCATE   |
| -------- | -------- | ---------- |
| Тип      | DML      | DDL        |
| WHERE    | Да       | Нет        |
| Скорость | Медленно | Быстро     |
| Rollback | Да       | Обычно нет |
| Сброс ID | Нет      | Да         |

```java
// DML
PreparedStatement ps =
    conn.prepareStatement("UPDATE employees SET salary=? WHERE id=?");
ps.setInt(1, 70000);
ps.setInt(2, 1);
ps.executeUpdate();

// DDL
Statement st = conn.createStatement();
st.execute("CREATE TABLE test(id INT)");
```

### 3. Что такое первичный ключ?
`Primary Key` (PK) — это столбец или набор столбцов, который однозначно идентифицирует каждую запись в таблице.

Свойства:
+ уникальность (UNIQUE)
+ не допускает NULL
+ в таблице может быть только один PK
+ может состоять из одного или нескольких столбцов (composite key / составной ключ)
```sql
PostgreSQL
CREATE TABLE employees (
    id SERIAL PRIMARY KEY,
    name VARCHAR(100)
);
```
*Композитный* (составной) первичный ключ

Используется, когда уникальность определяется комбинацией полей.
```sql
CREATE TABLE order_items (
    order_id INT,
    product_id INT,
    quantity INT,
    PRIMARY KEY (order_id, product_id)
);
```
+ порядок столбцов влияет на использование индекса
+ нельзя вставить дубликат комбинации

Нюансы
1. PK автоматически создаёт индекс

Обычно кластерный (SQL Server, InnoDB в MySQL)

Ускоряет:
+ поиск `JOIN`
+ связи через Foreign Key

2. PK нельзя быть NULL

Ошибка:
```sql
INSERT INTO employees (id, name)
VALUES (NULL, 'John'); -- ошибка
(Если не автоинкремент)
```
3. В таблице только один PK

Но он может включать несколько колонок.

Можно иметь много `UNIQUE`, но PK — один.

4. Изменение PK — плохая практика

Технически возможно:
```sql
UPDATE employees SET id = 10 WHERE id = 1;
```
Но:

+ может нарушить внешние ключи
+ дорого по производительности
+ PK должен быть стабильным

Поэтому PK:
+ не меняют
+ часто используют surrogate key (id)

5. Натуральный vs суррогатный ключ

Natural key
+ email
+ passport_number

Surrogate key
+ авто-число (id)

На практике чаще используют:


		id BIGSERIAL PRIMARY KEY

Причины:
+ быстрее
+ стабильнее
+ меньше проблем при изменениях

6. PK и `Foreign Key`

PK часто используется в связях:
```sql
CREATE TABLE orders (
    id INT PRIMARY KEY,
    user_id INT,
    FOREIGN KEY (user_id) REFERENCES users(id)
);
```
7. Автогенерация ключей
   MySQL


    AUTO_INCREMENT


PostgreSQL


    SERIAL


или современно:


    GENERATED ALWAYS AS IDENTITY


Частые ошибки

Дубликат PK
```sql
INSERT INTO employees VALUES (1, 'John');
INSERT INTO employees VALUES (1, 'Mike');
-- ERROR: duplicate key value
```
Когда используют составной PK

Подходит для:
+ таблиц связей (many-to-many)
+ логических уникальных комбинаций

Но в enterprise-проектах часто делают:
```sql
id BIGSERIAL PRIMARY KEY,
UNIQUE(order_id, product_id)
```
### 4. Что такое внешний ключ?
`Foreign Key` (FK внешний ключ) — это столбец или набор столбцов в дочерней таблице, который ссылается на Primary Key (или UNIQUE) в родительской таблице.

Он обеспечивает референциальную целостность:

нельзя добавить значение, которого нет в родительской таблице.
```sql
CREATE TABLE departments (
    dept_id INT PRIMARY KEY,
    name VARCHAR(100)
);

CREATE TABLE employees (
    emp_id INT PRIMARY KEY,
    name VARCHAR(100),
    dept_id INT,
    FOREIGN KEY (dept_id) REFERENCES departments(dept_id)
);
```
Теперь:
+ нельзя вставить сотрудника с несуществующим dept_id
+ нельзя удалить отдел, если на него есть ссылки (по умолчанию)

Нюансы
1. FK может ссылаться на `PRIMARY KEY` или `UNIQUE`
```sql
CREATE TABLE users (
    id INT PRIMARY KEY,
    email VARCHAR(100) UNIQUE
);

CREATE TABLE orders (
    id INT PRIMARY KEY,
    user_email VARCHAR(100),
    FOREIGN KEY (user_email) REFERENCES users(email)
);
```
2. FK может быть `NULL`

Если не указано `NOT NULL`:
```sql
INSERT INTO employees (emp_id, name, dept_id)
VALUES (1, 'John', NULL); -- допустимо
```
Это означает: связь отсутствует.

3. *Действия при DELETE / UPDATE*

`ON DELETE`

| Опция                | Что происходит                   |
| -------------------- | -------------------------------- |
| RESTRICT / NO ACTION | Запретить удаление родителя      |
| CASCADE              | Удалить дочерние записи          |
| SET NULL             | Обнулить FK                      |
| SET DEFAULT          | Установить значение по умолчанию |


Пример:
```sql
FOREIGN KEY (dept_id)
REFERENCES departments(dept_id)
ON DELETE CASCADE
```
Если удалить отдел → удалятся сотрудники.
```sql
ON UPDATE
FOREIGN KEY (dept_id)
REFERENCES departments(dept_id)
ON UPDATE CASCADE
```
Если изменится dept_id → обновится в дочерней таблице.

(PK редко меняют.)

4. Индексы и производительность

В MySQL (InnoDB) индекс создаётся автоматически

В PostgreSQL — нет, индекс нужно создать вручную:
```sql
CREATE INDEX idx_emp_dept ON employees(dept_id);
```
Иначе:
+ медленные JOIN
+ медленные DELETE/UPDATE в родительской таблице

5. Проверка целостности

Ошибка при нарушении:
```sql
INSERT INTO employees VALUES (1, 'John', 999);
-- ERROR: foreign key violation
```
6. Составной внешний ключ
```sql
CREATE TABLE orders (
    order_id INT,
    product_id INT,
    PRIMARY KEY (order_id, product_id)
);

CREATE TABLE shipments (
    order_id INT,
    product_id INT,
    FOREIGN KEY (order_id, product_id)
        REFERENCES orders(order_id, product_id)
);
```
7. Циклические FK

Возможны, но:
+ усложняют вставку/удаление

иногда требуют:

    DEFERRABLE INITIALLY DEFERRED
(PostgreSQL)

8. Когда не используют FK

В high-load системах иногда избегают:
+ для скорости
+ проверку делают на уровне приложения

Но в большинстве проектов FK — обязательная практика.

Поведение по умолчанию

Если не указано:

		FOREIGN KEY (...) REFERENCES ...

То:

+ ON DELETE → RESTRICT / NO ACTION
+ ON UPDATE → NO ACTION

Пример
```sql
CREATE TABLE departments (
    dept_id INT PRIMARY KEY
);

CREATE TABLE employees (
    emp_id INT PRIMARY KEY,
    dept_id INT NOT NULL,
    FOREIGN KEY (dept_id)
        REFERENCES departments(dept_id)
        ON DELETE CASCADE
        ON UPDATE CASCADE
);
```
### 5. Какие виды связей между таблицами существуют и как они организуются?
Виды связей между таблицами

В реляционных БД существует 3 основных типа связей:

+ Один-к-одному (1:1)
+ Один-ко-многим (1:N)
+ Многие-ко-многим (M:N)

Связи реализуются с помощью:
+ Primary Key (PK)
+ Foreign Key (FK)
+ ограничений без явного определения PK и составных ключей (UNIQUE, NOT NULL)

1. Связь Один-к-одному (1:1)

Каждой записи в таблице A соответствует одна запись в таблице B.

В дочерней таблице:

FK
```sql
UNIQUE (или FK = PK)

Пример
CREATE TABLE users (
    id INT PRIMARY KEY,
    name VARCHAR(100)
);

CREATE TABLE user_profiles (
    user_id INT PRIMARY KEY,
    passport VARCHAR(50),
    FOREIGN KEY (user_id) REFERENCES users(id)
);
```
Или:
```sql
CREATE TABLE user_profiles (
    id INT PRIMARY KEY,
    user_id INT UNIQUE,
    FOREIGN KEY (user_id) REFERENCES users(id)
);
```
+ Когда используется
+ разделение большой таблицы (вертикальное масштабирование)
+ безопасность (например, персональные данные)
+ опциональные данные

Нюанс: используется редко.

2. Связь Один-ко-многим (1:N)

Самый распространённый тип.

Одна запись в родительской таблице соответствует множеству записей в дочерней.

FK в таблице "многие".
```sql
Пример
CREATE TABLE departments (
    dept_id INT PRIMARY KEY,
    name VARCHAR(100)
);

CREATE TABLE employees (
    emp_id INT PRIMARY KEY,
    name VARCHAR(100),
    dept_id INT,
    FOREIGN KEY (dept_id) REFERENCES departments(dept_id)
);
```

+ FK можно сделать NOT NULL (обязательная связь)
+ желательно индексировать FK (особенно в PostgreSQL)
+ используется в большинстве бизнес-моделей

3. Связь Многие-ко-многим (M:N)

Одна запись в A связана с многими в B и наоборот.


Через промежуточную таблицу (junction / link table).

```sql
CREATE TABLE employees (
    emp_id INT PRIMARY KEY
);

CREATE TABLE projects (
    proj_id INT PRIMARY KEY
);

CREATE TABLE employees_projects (
    emp_id INT,
    proj_id INT,
    PRIMARY KEY (emp_id, proj_id),
    FOREIGN KEY (emp_id) REFERENCES employees(emp_id),
    FOREIGN KEY (proj_id) REFERENCES projects(proj_id)
);
```
Вариант с отдельным PK

Если есть дополнительные поля:
```sql
CREATE TABLE employees_projects (
    id SERIAL PRIMARY KEY,
    emp_id INT,
    proj_id INT,
    assigned_date DATE,
    FOREIGN KEY (emp_id) REFERENCES employees(emp_id),
    FOREIGN KEY (proj_id) REFERENCES projects(proj_id),
    UNIQUE (emp_id, proj_id)
);
```
*Важные нюансы*
1. FK обеспечивает целостность

+ нельзя создать "висящую" ссылку
+ предотвращает логические ошибки

2. Производительность

Минусы связей:

JOIN может быть дорогим

особенно при: `M:N`

больших таблицах

Поэтому:

+ индексы на FK — обязательно
+ иногда применяют денормализацию

3. Обязательная vs необязательная связь

dept_id INT NOT NULL

— обязательная

dept_id INT

— необязательная (может быть NULL)

4. Каскадные действия
+ FOREIGN KEY (dept_id)
+ REFERENCES departments(dept_id)
+ ON DELETE CASCADE

Часто спрашивают:

+ CASCADE
+ SET NULL
+ RESTRICT

5. Логическая vs физическая связь

В high-load системах иногда:

FK не создают

целостность проверяет приложение

6. *Как определить тип связи*

| Сценарий               | Тип |
| ---------------------- | --- |
| Пользователь — профиль | 1:1 |
| Отдел — сотрудники     | 1:N |
| Студенты — курсы       | M:N |


Коротко

Связи между таблицами бывают:
+ 1:1 — FK + UNIQUE
+ 1:N — FK в дочерней таблице
+ M:N — промежуточная таблица с двумя FK

Они обеспечивают референциальную целостность и позволяют нормализовать данные.


### 6. Опишите как вставить, удалить, обновить данные в(из) таблицу(ы).
1. INSERT — вставка данных

Добавляет новые строки в таблицу.

Вставка одной строки
```sql
INSERT INTO employees (id, name)
VALUES (1, 'John');
Вставка нескольких строк
INSERT INTO employees (id, name)
VALUES 
    (2, 'Jane'),
    (3, 'Mike');
```
INSERT без указания столбцов (опасно при изменении структуры)
```sql
INSERT INTO employees
VALUES (4, 'Alex');
Вставка из SELECT
INSERT INTO archive_employees (id, name)
SELECT id, name
FROM employees
WHERE active = false;
```
Получение сгенерированного ID

PostgreSQL
```sql
INSERT INTO employees (name)
VALUES ('John')
RETURNING id;
```
MySQL
```sql
SELECT LAST_INSERT_ID();
```
Нюансы INSERT

+ Проверяются:
+ PK / UNIQUE
+ Foreign Key
+ NOT NULL
+ Может вызвать BEFORE / AFTER INSERT trigger
+ При массовых вставках лучше использовать batch

2. UPDATE — обновление данных

Изменяет существующие записи.

```sql
UPDATE employees
SET name = 'John Updated'
WHERE id = 1;
Обновление нескольких столбцов
UPDATE employees
SET name = 'Mike',
    salary = 60000
WHERE id = 2;
```
Важно (частая ошибка)

	UPDATE employees SET salary = 0;

Без WHERE → обновит все строки.
```sql
UPDATE с подзапросом
UPDATE employees
SET salary = salary * 1.1
WHERE dept_id IN (
    SELECT dept_id FROM departments WHERE name = 'IT'
);
UPDATE с JOIN
```
PostgreSQL / SQL Server
```sql
UPDATE employees e
SET salary = salary * 1.1
FROM departments d
WHERE e.dept_id = d.dept_id
AND d.name = 'IT';
```
MySQL
```sql
UPDATE employees e
JOIN departments d ON e.dept_id = d.dept_id
SET e.salary = e.salary * 1.1
WHERE d.name = 'IT';
```
3. DELETE — удаление данных
   Удаление по условию
```sql
DELETE FROM employees
WHERE id = 1;
```
Удаление всех данных

	DELETE FROM employees;

(структура остаётся)
```sql
DELETE с подзапросом
DELETE FROM employees
WHERE dept_id IN (
    SELECT dept_id FROM departments WHERE name = 'HR'
);
```
DELETE с JOIN

MySQL
```sql
DELETE e
FROM employees e
JOIN departments d ON e.dept_id = d.dept_id
WHERE d.name = 'HR';
```
|                      | DELETE   | TRUNCATE   |
| -------------------- | -------- | ---------- |
| Тип                  | DML      | DDL        |
| WHERE                | Да       | Нет        |
| Скорость             | Медленно | Быстро     |
| Rollback             | Да       | Обычно нет |
| Сброс автоинкремента | Нет      | Да         |

5. Каскадные операции

Если есть FK:

+ FOREIGN KEY (dept_id)
+ REFERENCES departments(dept_id)
+ ON DELETE CASCADE

Удаление родителя → удаляются дочерние записи.

6. Транзакции
```sql
BEGIN;

UPDATE accounts SET balance = balance - 100 WHERE id = 1;
UPDATE accounts SET balance = balance + 100 WHERE id = 2;

COMMIT;
```
При ошибке:
ROLLBACK;

7. Работа через JDBC
```java
INSERT
PreparedStatement ps =
    conn.prepareStatement("INSERT INTO employees(name) VALUES(?)");
ps.setString(1, "John");
ps.executeUpdate();
UPDATE / DELETE
PreparedStatement ps =
    conn.prepareStatement("UPDATE employees SET name=? WHERE id=?");
ps.setString(1, "Mike");
ps.setInt(2, 1);

int rows = ps.executeUpdate();
```
rows — количество изменённых записей.

8. Безопасность

Всегда использовать PreparedStatement:

Плохо: `"DELETE FROM users WHERE id=" + userInput`

Хорошо: `WHERE id=?`

Защита от SQL Injection.

9. Производительность и практические советы

UPDATE/DELETE без индекса → full table scan

Массовые операции:
+ batch

разбивать на части

DELETE больших объёмов → лучше батчами

Частые UPDATE → подумать о индексах

Коротко
+ INSERT — добавляет данные (возможен RETURNING)
+ UPDATE — изменяет данные (осторожно с WHERE)
+ DELETE — удаляет данные (может каскадировать)
+ Все операции работают в транзакциях и могут быть откатаны
+ Для нескольких таблиц используются JOIN или подзапросы
+ В Java — через PreparedStatement и executeUpdate()

### 7. Что такое нормализация БД?
Нормализация — это процесс организации структуры базы данных для:
+ уменьшения избыточности данных
+ устранения аномалий (вставки, обновления, удаления)
+ обеспечения логической целостности

Какие проблемы решает нормализация (аномалии)
1. Аномалия обновления

Если данные дублируются — их нужно менять в нескольких местах.

Плохо:

| emp_id | name | dept_name |
| ------ | ---- | --------- |
| 1      | John | IT        |
| 2      | Mike | IT        |


Переименование отдела → нужно обновлять все строки.

2. Аномалия вставки

Нельзя добавить отдел без сотрудника.

3. Аномалия удаления

Удалили последнего сотрудника → потеряли информацию об отделе.

*Нормальные формы (NF)*

`1NF` — Первая нормальная форма

Требования:
+ атомарные значения (нет списков, массивов)
+ нет повторяющихся групп
+ есть первичный ключ

Плохо:

| id | phones   |
| -- | -------- |
| 1  | 123, 456 |


Хорошо:

| id | phone |
| -- | ----- |
| 1  | 123   |
| 1  | 456   |

`2NF` — Вторая нормальная форма

Требования:
+ таблица в 1NF
+ нет частичной зависимости от составного PK
+ Актуально только при composite key.

Плохо:
PK = (order_id, product_id)

| order_id | product_id | product_name |

product_name зависит только от product_id → выносим в отдельную таблицу.

`3NF` — Третья нормальная форма

Требования:
+ таблица в 2NF
+ нет транзитивной зависимости

Если:
+ A → B
+ B → C
+ то C не должно храниться вместе с A.

Пример

Плохо:
| emp_id | dept_id | dept_name |

dept_name зависит от dept_id, а не от emp_id.

Решение:
```sql
employees
emp_id | dept_id

departments
dept_id | dept_name
BCNF (Boyce-Codd Normal Form)
```
Условие:
+ Каждый детерминант должен быть кандидатом в ключ.
+ Используется редко, когда есть сложные зависимости.

4NF и 5NF
+ Связаны с:
+ многозначными зависимостями
+ сложными M:N:M отношениями
+ В практике почти не встречаются.

Пример нормализации до 3NF

Ненормализовано

| emp_id | name | dept_name | dept_location |

После нормализации

employees

emp_id | name | dept_id

departments

dept_id | dept_name | dept_location

Нюансы
1. Обычно достаточно 3NF

   Большинство OLTP-систем проектируют в 3NF.

2. Нормализация vs производительность

Минусы:
+ больше таблиц
+ больше JOIN
+ сложнее запросы

Поэтому иногда применяют денормализацию.

3. Нормализация ≠ всегда лучше

В high-load системах:
+ иногда хранят дубли
+ для ускорения чтения

4. Связь с типами связей

Нормализация приводит к:

+ 1:N через FK
+ M:N через промежуточные таблицы

| Нормальная форма | Основное требование                                                | Что устраняет                                             | Когда актуальна                    | Пример нарушения                                                |
| ---------------- | ------------------------------------------------------------------ | --------------------------------------------------------- | ---------------------------------- | --------------------------------------------------------------- |
| **1NF**          | Атомарные значения, нет повторяющихся групп                        | Множественные значения в одном поле                       | Всегда, базовый уровень            | `phones = "123,456"`                                            |
| **2NF**          | 1NF + нет частичной зависимости от составного PK                   | Зависимость части данных только от части составного ключа | Только при composite PK            | `product_name` зависит только от `product_id`                   |
| **3NF**          | 2NF + нет транзитивных зависимостей                                | Зависимость через промежуточное поле                      | Самая используемая форма           | `emp_id → dept_id → dept_name`                                  |
| **BCNF**         | Каждый детерминант — кандидатный ключ                              | Аномалии при сложных функциональных зависимостях          | Редко, при сложных бизнес-правилах | Поле определяет другое, но не является ключом                   |
| **4NF**          | Нет многозначных зависимостей                                      | Независимые множества значений                            | Очень редко                        | У сотрудника независимые множества: навыки и языки              |
| **5NF**          | Нет избыточности из-за разбиения на соединения (join dependencies) | Сложные M:N:M зависимости                                 | Почти не используется на практике  | Данные можно корректно восстановить только через несколько JOIN |

### 8. Что такое денормализация БД? Для чего она нужна?
Денормализация — это намеренное добавление избыточных данных в базу для повышения производительности чтения и упрощения запросов.
Это обратный процесс нормализации: данные дублируются или объединяются, чтобы уменьшить количество JOIN и вычислений.

1. Ускорение SELECT
+ меньше JOIN
+ меньше подзапросов
+ меньше агрегатов

2. Снижение нагрузки на БД

Особенно важно:
+ при высоком количестве чтений
+ в high-load системах

3. OLAP и аналитика
+ чтение >> запись
+ данные редко изменяются

4. Кэширование вычислений

Чтобы не считать каждый раз:
+ суммы
+ количество
+ рейтинги
+ статусы

Пример денормализации

Нормализованная модель
```text
orders
| id |
order_items
| order_id | price | qty |
```
Сумма заказа:
```sql
SELECT SUM(price * qty)
FROM order_items
WHERE order_id = 1;
```

Денормализованная
```text
orders
| id | total_amount |
```
Теперь:
```sql
SELECT total_amount FROM orders WHERE id = 1;
```
Но при изменении order_items нужно обновлять total_amount.

Основные способы денормализации
+ Дублирование данных
  dept_name в employees

Хранение агрегатов
+ total_amount
+ items_count

Объединение таблиц
вместо JOIN

Материализованные представления
+ Отказ от Foreign Key ради скорости (иногда в high-load)

| Проблема                 | Причина                              |
| ------------------------ | ------------------------------------ |
| Несогласованность данных | дублирование                         |
| Сложные UPDATE           | нужно менять в нескольких местах     |
| Аномалии                 | как при ненормализованных структурах |
| Сложность логики         | триггеры / сервисный код             |


2. Как поддерживать согласованность
+ триггеры
+ хранимые процедуры
+ бизнес-логика в приложении
+ batch-обновления
+ event-driven подход

3. Когда применять

Использовать, если:
+ много чтений, мало записей
    + JOIN слишком дорогие
+ есть проблемы с производительностью
+ есть профилирование (не «на всякий случай»)

| Тип системы       | Подход         |
| ----------------- | -------------- |
| OLTP (транзакции) | Нормализация   |
| OLAP (аналитика)  | Денормализация |


Примеры
+ users + orders_count
+ posts + comments_count
+ products + rating_avg
+ логические флаги: is_active, last_login

### 9. Что такое кластерный и некластерный индексы?

Индекс — это структура данных (обычно B-tree), которая ускоряет поиск строк в таблице.

Кластерный индекс (Clustered Index)

Кластерный индекс определяет физический порядок хранения данных в таблице.

То есть: строки таблицы хранятся на диске в порядке значений этого индекса.

Особенности
+ Только один кластерный индекс на таблицу
+ Листовые узлы индекса содержат сами данные

Очень быстрый для:
+ поиска по диапазону (BETWEEN, <, >)
+ сортировки (ORDER BY) 

`MIN`, `MAX`

Пример (SQL Server)
```sql
CREATE CLUSTERED INDEX idx_emp_id ON employees(id);
```
Часто создаётся автоматически:

+ SQL Server — PK по умолчанию clustered
+ MySQL (InnoDB) — таблица всегда хранится как clustered по PK
  Если PK нет (InnoDB):
+ используется первый UNIQUE NOT NULL
+ иначе создаётся скрытый ключ

Минусы:
+ медленные INSERT/UPDATE, если ключ:
+ случайный (UUID)
+ часто меняется
+ возможна fragmentation

Некластерный индекс (Non-Clustered Index)

Некластерный индекс — это отдельная структура, которая содержит:
+ ключ индекса
+ указатель на строку

Данные в таблице не меняют физический порядок.

Структура

В листовых узлах хранится:
+ В SQL Server → pointer (RID)
+ В MySQL InnoDB → Primary Key

Пример
```sql
CREATE INDEX idx_name ON employees(name);
```
**Можно создать много некластерных индексов.**

**Главное различие**
|                    | Clustered    | Non-Clustered |
| ------------------ | ------------ | ------------- |
| Физический порядок | Да           | Нет           |
| Количество         | 1            | Много         |
| Листовой уровень   | Данные       | Указатели     |
| Скорость range     | Очень быстро | Медленнее     |
| Скорость INSERT    | Медленнее    | Быстрее       |

Composite (составные) индексы
```sql
CREATE INDEX idx_name_dept ON employees(dept_id, name);
```
Правило leftmost prefix

Индекс работает для:
+ (dept_id)
+ (dept_id, name)

Но не работает для:
+ (name) отдельно


**Когда выбирать кластерный индекс**

Лучше использовать для:
+ Primary Key
+ Часто используемых диапазонов
+ Сортировок
+ Auto-increment колонок

Плохо подходит:
+ UUID
+ часто обновляемые поля

Покрывающий индекс

Если все нужные столбцы есть в индексе:
```sql
CREATE INDEX idx_cover ON employees(dept_id, name);
```
Запрос:
```sql
SELECT name FROM employees WHERE dept_id = 10;
```
→ таблица не читается (Index Only Scan)

**Особенности в разных СУБД**
+ SQL Server
    + PK clustered по умолчанию

можно явно выбрать clustered/non-clustered

+ MySQL (InnoDB) всегда clustered по PK
    + secondary indexes хранят PK

+ PostgreSQL нет «настоящего» clustered индекса
    + команда CLUSTER просто физически переупорядочивает таблицу (однократно)

**Минусы индексов**
+ замедляют INSERT/UPDATE/DELETE
+ занимают место
+ требуют обслуживания

**Правило**: Индексы ускоряют чтение, но замедляют запись.

Кластерный индекс определяет физический порядок хранения данных и может быть только один на таблицу. Некластерный индекс — отдельная структура с указателями на строки, их может быть несколько. Кластерный быстрее для диапазонов, а некластерные используются для ускорения поиска по различным полям.
### 10. Какие типы соединений (join) таблиц существуют? В чем их разница?
JOIN используется для объединения строк из нескольких таблиц на основе условия.

Пусть есть таблицы:
```text
employees
| emp_id | name | dept_id |

departments
| dept_id | dept_name |
```
1. INNER JOIN

Возвращает только совпадающие строки в обеих таблицах.
```sql
SELECT e.name, d.dept_name
FROM employees e
INNER JOIN departments d
ON e.dept_id = d.dept_id;
```
Логика:

+ Пересечение (A ∩ B)

Когда используется

+ когда нужны только связанные данные

2. LEFT JOIN (LEFT OUTER JOIN)

Возвращает:
+ все строки из левой таблицы
+ совпадения из правой
+ если совпадения нет → NULL
```sql
SELECT e.name, d.dept_name
FROM employees e
LEFT JOIN departments d
ON e.dept_id = d.dept_id;
```
Логика:
+ A + совпадения из B

Частый кейс

Найти сотрудников без отдела:
```sql
SELECT e.*
FROM employees e
LEFT JOIN departments d
ON e.dept_id = d.dept_id
WHERE d.dept_id IS NULL;
```
(на собеседовании)

3. RIGHT JOIN

То же самое, но наоборот:

все строки из правой таблицы
```sql
SELECT e.name, d.dept_name
FROM employees e
RIGHT JOIN departments d
ON e.dept_id = d.dept_id;
```
На практике используется редко — обычно переписывают через LEFT JOIN.

4. FULL JOIN (FULL OUTER JOIN)

Возвращает:

+ все строки из обеих таблиц
+ NULL там, где нет совпадений
```sql
SELECT e.name, d.dept_name
FROM employees e
FULL JOIN departments d
ON e.dept_id = d.dept_id;
```
MySQL не поддерживает FULL JOIN

Эмуляция:
```sql
SELECT ...
FROM A LEFT JOIN B
UNION
SELECT ...
FROM A RIGHT JOIN B;
```
5. CROSS JOIN

Картезианское произведение:

+ каждая строка A × каждая строка B
```sql
SELECT *
FROM employees
CROSS JOIN departments;
```
```text
Если:
100 сотрудников
10 отделов
→ результат = 1000 строк
```
Часто возникает случайно, если забыть условие JOIN.

6. SELF JOIN

Таблица соединяется сама с собой.

Пример: сотрудник → менеджер
```sql
SELECT e.name AS employee,
       m.name AS manager
FROM employees e
LEFT JOIN employees m
ON e.manager_id = m.emp_id;
Дополнительные виды соединений
Equi Join
```
Соединение по равенству (самое частое):
```sql
ON e.dept_id = d.dept_id
Non-Equi Join
SELECT *
FROM employees e
JOIN salary_grades s
ON e.salary BETWEEN s.min_salary AND s.max_salary;
USING vs ON
USING
```
Если одинаковые имена столбцов:
```sql
SELECT *
FROM employees
JOIN departments USING (dept_id);
ON
```
Более универсальный:

	ON e.dept_id = d.dept_id

ON используется чаще.

Производительность (важно)

+ INNER JOIN обычно быстрее

OUTER JOIN:
+ возвращает больше строк
+ сложнее для оптимизатора

Индексы на:
+ PK
+ FK

сильно ускоряют JOIN

Порядок выполнения (частая ловушка)

При LEFT JOIN:
```sql
LEFT JOIN ...
WHERE right.col = value
```
→ превращается в INNER JOIN

Правильно:
```sql
LEFT JOIN ...
ON right.col = value
```
| JOIN  | Что возвращает             |
| ----- | -------------------------- |
| INNER | Только совпадения          |
| LEFT  | Всё из левой + совпадения  |
| RIGHT | Всё из правой + совпадения |
| FULL  | Всё из обеих               |
| CROSS | Все комбинации             |
| SELF  | Таблица сама с собой       |

### 11. Что такое SQL курсор?
Курсор (Cursor) — это объект базы данных, позволяющий построчно читать результат запроса, не загружая всю выборку в память сразу.

Используется для:
+ обработки больших наборов данных;
+ пошагового чтения результата;
+ процедур и функций;
+ streaming-обработки.

В PostgreSQL курсор — это указатель на результат SELECT.

Когда применяют курсоры в PostgreSQL
+ Используются, если:
+ результат запроса очень большой;
+ нужно читать данные частями;
+ требуется логика обработки строк по одной;
+ нужно избежать загрузки всей выборки в память клиента.

В PostgreSQL курсоры чаще применяются для streaming, а не для бизнес-логики.

Жизненный цикл курсора
+ DECLARE — объявление курсора
+ OPEN — открытие (выполнение запроса)
+ FETCH — получение строк
+ CLOSE — закрытие курсора

DEALLOCATE в PostgreSQL обычно не нужен — ресурсы освобождаются автоматически.

Пример курсора в PostgreSQL
```sql
BEGIN;

DECLARE emp_cursor CURSOR FOR
SELECT emp_id, name
FROM employees;

FETCH NEXT FROM emp_cursor;

FETCH NEXT FROM emp_cursor;

CLOSE emp_cursor;

COMMIT;
```
Курсоры в PL/pgSQL (функции)

Частый вариант использования:
```sql
DO $$
DECLARE
    rec RECORD;
    cur CURSOR FOR
        SELECT emp_id, name FROM employees;
BEGIN
    OPEN cur;

    LOOP
        FETCH cur INTO rec;
        EXIT WHEN NOT FOUND;

        RAISE NOTICE '%', rec.name;
    END LOOP;

    CLOSE cur;
END $$;
```

PostgreSQL активно использует курсоры для:
+ pagination без OFFSET
+ streaming результатов
+ работы JDBC (fetchSize)
+ server-side cursors

Например, JDBC-драйвер PostgreSQL автоматически создаёт курсор при:

	statement.setFetchSize(100);

Типы курсоров в PostgreSQL
+ По прокрутке
  NO SCROLL — только вперёд (быстрее)
  SCROLL — можно двигаться вперёд и назад
  DECLARE cur SCROLL CURSOR FOR SELECT * FROM employees;

+ По времени жизни

WITH HOLD

Курсор остаётся доступным после COMMIT.

DECLARE cur CURSOR WITH HOLD FOR SELECT * FROM employees;

WITHOUT HOLD (по умолчанию)

Закрывается после COMMIT.

Курсоры:
+ работают строка-за-строкой (RBAR);
+ медленнее set-based операций;
+ удерживают ресурсы соединения;
+ могут держать транзакцию открытой.

Что использовать вместо курсоров

В PostgreSQL обычно применяют:
+ UPDATE ... FROM
+ INSERT ... SELECT
+ CTE (WITH)
+ оконные функции
+ агрегаты
+ RETURNING

Пример без курсора

Плохо — обработка строк по одной

Хорошо:
```sql
UPDATE employees
SET salary = salary * 1.1
WHERE dept_id = 10;
```
### 12. Опишите шаги по созданию и использованию курсора.
Курсоры в PostgreSQL (PL/pgSQL)

1. Что такое курсор
   Курсор — это объект базы данных, позволяющий обрабатывать результат SELECT-запроса построчно.
   Используется, когда нужно работать с данными по одной строке (итеративная обработка).

2. Шаги работы с курсором в PostgreSQL

+ DECLARE — объявление курсора
+ OPEN — открытие курсора (выполнение SELECT)
+ FETCH — извлечение строк
+ CLOSE — закрытие курсора

В PostgreSQL освобождение ресурсов происходит автоматически при завершении блока или транзакции.

3. Основной синтаксис (PL/pgSQL)
```sql
DO $$
DECLARE
    emp_name TEXT;
    cur CURSOR FOR SELECT name FROM employees;
BEGIN
    OPEN cur;

    LOOP
        FETCH cur INTO emp_name;
        EXIT WHEN NOT FOUND;

        RAISE NOTICE 'Employee: %', emp_name;
    END LOOP;

    CLOSE cur;
END $$;
```
4. Проверка окончания курсора
   + В PostgreSQL используется:
     + NOT FOUND — аналог @@FETCH_STATUS в SQL Server.
   + Если строк больше нет — переменная NOT FOUND становится TRUE.

5. Типы курсоров
- READ ONLY (по умолчанию)
- FOR UPDATE — позволяет изменять выбранные строки

Пример:
```sql
DECLARE cur CURSOR FOR
SELECT * FROM employees FOR UPDATE;
```
6. Использование в транзакциях
   + Курсор живёт внутри транзакции.
   + После COMMIT или ROLLBACK курсор автоматически закрывается.

7. Нюансы работы
- Курсоры полезны при сложной логике обработки.
- Для обычных операций лучше использовать set-based запросы.
- Изменения данных во время FETCH зависят от типа курсора и уровня изоляции.

8. Альтернатива — FOR LOOP (рекомендуемый способ)
```sql
DO $$
DECLARE
    rec RECORD;
BEGIN
    FOR rec IN SELECT name FROM employees LOOP
        RAISE NOTICE 'Employee: %', rec.name;
    END LOOP;
END $$;
```
Этот способ проще и используется чаще, чем явные курсоры.
### 13. Что такое транзакция?
Транзакция — это логическая единица работы с БД, состоящая из одной или нескольких операций, которая выполняется полностью или не выполняется вообще.

Свойства ACID
1. Atomicity (Атомарность)

Все операции выполняются целиком или откатываются.
```sql
BEGIN;
UPDATE accounts SET balance = balance - 100 WHERE id = 1;
UPDATE accounts SET balance = balance + 100 WHERE id = 2;
COMMIT;
```
Если второй UPDATE упал → выполняется ROLLBACK.

2. Consistency (Согласованность)

После транзакции БД остаётся в корректном состоянии:

соблюдаются ограничения (PK, FK, CHECK)

нет «битых» данных

3. Isolation (Изоляция)

Параллельные транзакции не должны мешать друг другу.

Уровни изоляции определяют степень «видимости» изменений.

4. Durability (Долговечность)

После COMMIT данные не потеряются даже при сбое (журналы транзакций).

Основные команды
+ BEGIN TRANSACTION;  -- или BEGIN
+ COMMIT;             -- зафиксировать
+ ROLLBACK;           -- откат

Проблемы

| Проблема            | Описание                                |
| ------------------- | --------------------------------------- |
| Dirty Read          | чтение незакоммиченных данных           |
| Non-repeatable Read | повторный SELECT даёт разные результаты |
| Phantom Read        | появляются новые строки                 |

| Уровень          | Dirty | Non-repeatable | Phantom |
| ---------------- | ----- | -------------- | ------- |
| READ UNCOMMITTED | ✔     | ✔              | ✔       |
| READ COMMITTED   | ✖     | ✔              | ✔       |
| REPEATABLE READ  | ✖     | ✖              | ✔       |
| SERIALIZABLE     | ✖     | ✖              | ✖       |

```sql
SET TRANSACTION ISOLATION LEVEL REPEATABLE READ;
BEGIN;
SELECT * FROM employees WHERE dept_id = 10;
```
	SAVEPOINT (частичный откат)
Полезно, если нужно откатить часть операций:
```sql
BEGIN;
UPDATE employees SET salary = salary * 1.1;
SAVEPOINT sp1;
DELETE FROM employees WHERE salary > 100000;
ROLLBACK TO sp1;
COMMIT;
```
Автокоммит

Во многих СУБД:
+ каждый запрос = отдельная транзакция

В PostgreSQL / MySQL:
+ SET autocommit = 0;

Пример с ошибкой
```sql
BEGIN;

UPDATE accounts SET balance = balance - 100 WHERE id = 1;

-- ошибка (например, id=2 не существует)
UPDATE accounts SET balance = balance + 100 WHERE id = 2;

ROLLBACK;
```

Транзакции в JDBC (для Java)

По умолчанию — autocommit = true
```java
Connection conn = dataSource.getConnection();
conn.setAutoCommit(false);

try {
    PreparedStatement ps1 = conn.prepareStatement(
        "UPDATE accounts SET balance = balance - 100 WHERE id = ?");
    ps1.setInt(1, 1);
    ps1.executeUpdate();

    PreparedStatement ps2 = conn.prepareStatement(
        "UPDATE accounts SET balance = balance + 100 WHERE id = ?");
    ps2.setInt(1, 2);
    ps2.executeUpdate();

    conn.commit();
} catch (Exception e) {
    conn.rollback();
}
```
Нюансы
1. Долгие транзакции — плохо
+ держат блокировки
+ ухудшают производительность

2. COMMIT снимает блокировки

ROLLBACK тоже освобождает ресурсы.

3. Deadlock (взаимная блокировка)

Если две транзакции ждут друг друга — СУБД принудительно откатит одну.

4. READ COMMITTED — самый популярный уровень

Используется по умолчанию в:
+ PostgreSQL
+ Oracle
+ SQL Server
### 14. Что такое триггер? Какие типы триггеров Вы знаете?
Trigger (триггер) — объект БД, который автоматически выполняет функцию при событии:
+ INSERT
+ UPDATE
+ DELETE
+ TRUNCATE
+ DDL-события
+ системные события

В PostgreSQL триггер НЕ содержит код напрямую.
Он вызывает функцию.
Это главное отличие от SQL Server.

1. DML-триггеры

Срабатывают при изменении данных.

Типы по времени выполнения

| Тип        | Когда выполняется          |
| ---------- | -------------------------- |
| BEFORE     | До операции                |
| AFTER      | После операции             |
| INSTEAD OF | Вместо операции (для VIEW) |


Пример `AFTER INSERT` (PostgreSQL)

Шаг 1 — функция триггера
```sql
CREATE OR REPLACE FUNCTION trg_after_insert_fn()
RETURNS TRIGGER
LANGUAGE plpgsql
AS $$
BEGIN
    INSERT INTO audit_log(action, emp_id)
    VALUES ('INSERT', NEW.emp_id);

    RETURN NEW;
END;
$$;
```
Шаг 2 — создание триггера
```sql
CREATE TRIGGER trg_after_insert
AFTER INSERT ON employees
FOR EACH ROW
EXECUTE FUNCTION trg_after_insert_fn();
```

Главное отличие от SQL Server

| SQL Server                   | PostgreSQL                 |
| ---------------------------- | -------------------------- |
| inserted / deleted таблицы   | NEW / OLD записи           |
| код внутри trigger           | trigger → вызывает функцию |
| statement-level по умолчанию | row-level чаще             |

2. BEFORE Trigger (валидация)

Аналог Oracle `:NEW`.
```sql
CREATE OR REPLACE FUNCTION trg_salary_check()
RETURNS TRIGGER
LANGUAGE plpgsql
AS $$
BEGIN
    IF NEW.salary < 0 THEN
        RAISE EXCEPTION 'Salary cannot be negative';
    END IF;

    RETURN NEW;
END;
$$;
CREATE TRIGGER trg_salary_check
BEFORE INSERT OR UPDATE ON employees
FOR EACH ROW
EXECUTE FUNCTION trg_salary_check();
```
3. INSTEAD OF Trigger (для VIEW)

PostgreSQL поддерживает INSTEAD OF.
```sql
CREATE OR REPLACE FUNCTION trg_view_insert_fn()
RETURNS TRIGGER
LANGUAGE plpgsql
AS $$
BEGIN
    INSERT INTO employees(name, dept_id)
    VALUES (NEW.name, NEW.dept_id);

    RETURN NEW;
END;
$$;
CREATE TRIGGER trg_view_insert
INSTEAD OF INSERT ON employee_view
FOR EACH ROW
EXECUTE FUNCTION trg_view_insert_fn();
```
4. DDL-триггеры (Event Triggers)

В PostgreSQL они называются:

`EVENT TRIGGER`
```sql
CREATE OR REPLACE FUNCTION ddl_logger()
RETURNS event_trigger
LANGUAGE plpgsql
AS $$
BEGIN
    RAISE NOTICE 'DDL command executed';
END;
$$;
CREATE EVENT TRIGGER trg_ddl
ON ddl_command_end
EXECUTE FUNCTION ddl_logger();
```
DDL события
+ ddl_command_start
+ ddl_command_end
+ sql_drop
+ table_rewrite

5. Доступ к данным внутри триггера
   
PostgreSQL аналоги

| Oracle | SQL Server | PostgreSQL |
| ------ | ---------- | ---------- |
| :NEW   | inserted   | NEW        |
| :OLD   | deleted    | OLD        |
```sql
UPDATE пример
IF NEW.salary <> OLD.salary THEN
    INSERT INTO audit_log VALUES (...);
END IF;
```
6. Row vs Statement Trigger (важно)
   + Row-level
   + FOR EACH ROW

Вызывается для каждой строки.

+ Statement-level
+ FOR EACH STATEMENT

Вызывается 1 раз на запрос.

7. Триггеры работают в транзакции

Если внутри:

+ RAISE EXCEPTION;
+ откатится:
+ trigger

INSERT / UPDATE / DELETE

8. Рекурсивные триггеры

Если trigger изменяет ту же таблицу:

UPDATE employees ...
→ возможна рекурсия.

Проверяют так:

	IF TG_OP = 'UPDATE' THEN

или используют guard-условия.

9. Когда используют

+ аудит изменений
+ логирование
+ денормализация
+ автоматические вычисления
+ контроль данных

Когда НЕ использовать

+ бизнес-логика
+ интеграции
+ массовые batch операции

Лучше:
+ constraints
+ application layer
+ stored procedures

PostgreSQL имеет:

Transition Tables (аналог inserted/deleted)
```sql
CREATE TRIGGER trg_bulk
AFTER UPDATE ON employees
REFERENCING NEW TABLE AS new_table
FOR EACH STATEMENT
EXECUTE FUNCTION fn_bulk();
```
можно получить все изменённые строки сразу.
### 15. В чем разница между where и having?

| WHERE                                  | HAVING                                         |
| -------------------------------------- | ---------------------------------------------- |
| Фильтрует **строки**                   | Фильтрует **группы**                           |
| Выполняется **до GROUP BY**            | Выполняется **после GROUP BY**                 |
| Нельзя использовать агрегатные функции | Можно использовать агрегаты                    |
| Работает быстрее (фильтрация раньше)   | Медленнее (работает по результату группировки) |

Порядок выполнения SQL-запроса (логический)
+ `FROM`
+ `JOIN`
+ `WHERE`
+ `GROUP BY`
+ `HAVING`
+ `SELECT`
+ `ORDER BY`

```SQL
SELECT dept_id, COUNT(*) AS cnt
FROM employees
WHERE salary > 50000       -- фильтр строк
GROUP BY dept_id
HAVING COUNT(*) > 10;      -- фильтр групп
```

+ Сначала отбираются сотрудники с зарплатой > 50000
+ Потом группировка по dept_id
+ Потом остаются только группы с количеством > 10

Почему агрегаты нельзя в `WHERE`
```sql
Неправильно:
-- Ошибка
SELECT dept_id
FROM employees
WHERE COUNT(*) > 10
GROUP BY dept_id;
Причина: на этапе WHERE ещё нет групп и агрегатов.
```

Неправильно:
```SQL
-- Ошибка
SELECT dept_id
FROM employees
WHERE COUNT(*) > 10
GROUP BY dept_id;
```

Правильный запрос по производительности
```SQL
SELECT dept_id, COUNT(*)
FROM employees
WHERE status = 'ACTIVE'
GROUP BY dept_id
HAVING COUNT(*) > 10;
```

+ `WHERE` уменьшает объём данных до группировки
+ меньше памяти и вычислений


HAVING используют - когда нужно фильтровать по:
+ COUNT()
+ SUM()
+ AVG()
+ MIN()/MAX()
+ любым агрегатным условиям
```sql
SELECT dept_id, SUM(salary) AS total
FROM employees
GROUP BY dept_id
HAVING SUM(salary) > 100000;
```

Можно ли использовать `HAVING` без `GROUP BY`?

Да, но это считается плохой практикой, если можно использовать `WHERE`.
### 16. Что такое подзапрос (sub-query)?
Подзапрос — это SQL-запрос, вложенный внутрь другого запроса.

Он используется для получения промежуточных данных, которые применяются во внешнем запросе.

Подзапрос может находиться в:
+ WHERE
+ SELECT
+ FROM
+ HAVING
+ INSERT / UPDATE / DELETE

Виды подзапросов

1. Скалярный (возвращает одно значение)

Используется как обычное значение.
```sql
SELECT name
FROM employees
WHERE salary > (
    SELECT AVG(salary) FROM employees
);
```
Если подзапрос вернёт больше одной строки → ошибка.

2. Множественный (возвращает набор значений)

Используется с `IN`, `ANY`, `ALL`.
```sql
SELECT name
FROM employees
WHERE dept_id IN (
    SELECT dept_id FROM departments WHERE location = 'Kyiv'
);
```
3. Табличный (в FROM)
```sql
SELECT dept_id, avg_salary
FROM (
    SELECT dept_id, AVG(salary) AS avg_salary
    FROM employees
    GROUP BY dept_id
) t
WHERE avg_salary > 50000;
```
Это называется derived table.

**Коррелированные и некоррелированные**

Некоррелированный (независимый)

Выполняется один раз:
```sql
SELECT name
FROM employees
WHERE salary > (
    SELECT AVG(salary) FROM employees
);
```
Коррелированный

Зависит от внешнего запроса и выполняется для каждой строки.

Пример :
```sql
SELECT name
FROM employees e
WHERE salary > (
    SELECT AVG(salary)
    FROM employees
    WHERE dept_id = e.dept_id
);
```
Минус: может работать медленно.

EXISTS vs IN (частый вопрос)

EXISTS

Проверяет наличие строк (останавливается при первой найденной).
```sql
SELECT name
FROM employees e
WHERE EXISTS (
    SELECT 1
    FROM departments d
    WHERE d.dept_id = e.dept_id
);
```
Лучше для:
+ больших таблиц
+ коррелированных подзапросов

`IN`

Сравнивает со списком значений.
```sql
SELECT name
FROM employees
WHERE dept_id IN (
    SELECT dept_id FROM departments
);
```
Может работать медленнее на больших наборах.

`ANY` и `ALL`
```sql
-- больше хотя бы одного
WHERE salary > ANY (SELECT salary FROM employees WHERE dept_id = 10)

-- больше всех
WHERE salary > ALL (SELECT salary FROM employees WHERE dept_id = 10)
```

Подзапрос в `SELECT`
```sql
SELECT name,
       (SELECT AVG(salary) FROM employees) AS avg_salary
FROM employees;
```
Скалярный подзапрос.

Когда лучше использовать `JOIN` вместо подзапроса

Часто быстрее:
```sql
-- Подзапрос
SELECT name
FROM employees
WHERE dept_id IN (SELECT dept_id FROM departments WHERE location = 'Kyiv');

-- JOIN (обычно быстрее)
SELECT e.name
FROM employees e
JOIN departments d ON e.dept_id = d.dept_id
WHERE d.location = 'K';
```
Важные нюансы

Подзапросы могут быть вложенными (глубина ограничена, например ~32 в SQL Server)

Коррелированные подзапросы могут сильно снижать производительность

`EXISTS` обычно быстрее `IN` при больших данных

Скалярный подзапрос должен возвращать ровно одно значение

В некоторых случаях оптимизатор преобразует подзапрос в `JOIN`
### 17. Что такое union?
`UNION` — это оператор SQL, который объединяет результаты двух или более SELECT-запросов в один результирующий набор.

Основные требования:
+ Одинаковое количество столбцов
+ Совместимые типы данных
+ Порядок столбцов должен совпадать

| Оператор  | Поведение                                |
| --------- | ---------------------------------------- |
| `UNION`     | Удаляет дубликаты                        |
| `UNION ALL` | Сохраняет все строки (включая дубликаты) |

```SQL
SELECT name FROM employees
UNION
SELECT name FROM managers;
```

Если нужны все строки:
```sql
SELECT name FROM employees
UNION ALL
SELECT name FROM managers;
```
`UNION` медленнее, потому что выполняет сортировку или хеширование для удаления дубликатов.

Имена столбцов в результате берутся из первого SELECT:
```sql
SELECT name AS person_name FROM employees
UNION
SELECT name FROM managers;
```

`ORDER BY` - применяется ко всему объединённому результату, а не к отдельным SELECT:
```sql
SELECT name FROM employees
UNION
SELECT name FROM managers
ORDER BY name;
```

Когда использовать `UNION`
+ объединение данных из разных таблиц
+ формирование отчётов
+ объединение исторических и текущих данных
+ эмуляция OR-логики между источниками

Связанные операторы

| Оператор                        | Описание            |
| ------------------------------- | ------------------- |
| INTERSECT                       | Только общие строки |
| EXCEPT (SQL Server, PostgreSQL) | Разность множеств   |
| MINUS (Oracle)                  | Аналог EXCEPT       |

```sql
SELECT name FROM employees
INTERSECT
SELECT name FROM managers;
```

1. `UNION` ≠ `JOIN`

+ `UNION` — объединяет строки
+ `JOIN` — объединяет столбцы

2. Производительность

Если дубликаты не важны — всегда использовать:

`UNION ALL`

Это стандартная рекомендация для production.

Если типы различаются, СУБД попытается привести их:
```sql
SELECT 1
UNION
SELECT '1';
```
### 18. Что такое group by?
`GROUP BY` — это SQL операция, которая группирует строки по значениям одного или нескольких столбцов. После группировки можно применять агрегатные функции к каждой группе:
+ `SUM()` — сумма
+ `COUNT()` — количество
+ `AVG()` — среднее
+ `MIN() / MAX()` — минимальное / максимальное

```sql
SELECT column1, column2, AGG_FUNC(column3)
FROM table
WHERE condition
GROUP BY column1, column2
HAVING condition
ORDER BY column1;
```
Основные нюансы

Все неагрегированные столбцы должны быть в `GROUP BY`

Неправильно:
```sql
SELECT dept_id, name, SUM(salary)
FROM employees
GROUP BY dept_id;
-- Ошибка: name не в GROUP BY
```
Правильно:
```sql
SELECT dept_id, name, SUM(salary)
FROM employees
GROUP BY dept_id, name;
```
Порядок выполнения SQL
+ FROM / JOIN
+ WHERE
+ GROUP BY
+ HAVING
+ SELECT
+ ORDER BY

Это объясняет, почему агрегаты нельзя использовать в `WHERE`, а только в `HAVING`.

Дополнительные возможности

| Функция       | Описание                                                |
| ------------- | ------------------------------------------------------- |
| `WITH ROLLUP`   | Добавляет строки с промежуточными и итоговыми суммами   |
| `WITH CUBE`     | Добавляет все возможные комбинации агрегатных сумм      |
| `GROUPING SETS` | Позволяет указать несколько группировок в одном запросе |

```sql
SELECT dept_id, job_id, SUM(salary)
FROM employees
GROUP BY dept_id, job_id WITH ROLLUP;
```
`HAVING`

Используется для фильтрации групп после агрегирования:
```sql
SELECT dept_id, COUNT(*) AS emp_count
FROM employees
GROUP BY dept_id
HAVING COUNT(*) > 10;
```

`ORDER BY` после группировки
```sql
SELECT dept_id, SUM(salary) AS total_salary
FROM employees
GROUP BY dept_id
ORDER BY total_salary DESC;
```

Полный запрос
```sql
SELECT dept_id, job_id, COUNT(*) AS emp_count, SUM(salary) AS total_salary
FROM employees
WHERE status = 'ACTIVE'
GROUP BY dept_id, job_id WITH ROLLUP
HAVING SUM(salary) > 50000
ORDER BY total_salary DESC;
```
### 19. Что такое хранимые процедуры?
Хранимая процедура — это заранее скомпилированный блок SQL-кода и логики (условия, циклы), хранящийся в базе данных, который можно вызывать многократно.

Используется для:
+ автоматизации повторяющихся операций
+ инкапсуляции бизнес-логики на уровне БД
+ повышения безопасности и производительности

Основные характеристики
+ Компилируются и хранятся в БД
+ Могут принимать входные, выходные и входно-выходные параметры
+ Могут выполнять транзакции (`COMMIT/ROLLBACK`)
+ Можно возвращать значения через `OUTPUT`-параметры или `RETURN`

Преимущества
+ Производительность — компилируются заранее, уменьшают нагрузку на сеть
+ Безопасность — предотвращают SQL-инъекции при правильном использовании параметров
+ Модульность — логика централизована в БД
+ Повторное использование — один код для нескольких приложений

Минусы
+ Сложнее отлаживать, чем обычные SQL-запросы
+ Vendor-lock: синтаксис зависит от СУБД
+ Могут усложнять архитектуру, если логика слишком сложная

Синтаксис (T-SQL / SQL Server)
```sql
CREATE PROCEDURE GetEmployee
    @id INT
AS
BEGIN
    SELECT * 
    FROM employees 
    WHERE id = @id;
END;

-- Вызов процедуры
EXEC GetEmployee @id = 1;
Процедуры с OUTPUT-параметрами
CREATE PROCEDURE GetEmployeeSalary
    @id INT,
    @salary DECIMAL(10,2) OUTPUT
AS
BEGIN
    SELECT @salary = salary
    FROM employees
    WHERE id = @id;
END;

-- Вызов
DECLARE @emp_salary DECIMAL(10,2);
EXEC GetEmployeeSalary @id = 1, @salary = @emp_salary OUTPUT;
PRINT @emp_salary;

-- Процедуры с транзакцией
CREATE PROCEDURE UpdateSalary
    @id INT,
    @increment DECIMAL(10,2)
AS
BEGIN
    BEGIN TRANSACTION;
    UPDATE employees
    SET salary = salary + @increment
    WHERE id = @id;

    IF @@ERROR <> 0
        ROLLBACK TRANSACTION;
    ELSE
        COMMIT TRANSACTION;
END;
```

| СУБД       | Язык / примечание                         |
| ---------- | ----------------------------------------- |
| Oracle     | PL/SQL: `CREATE OR REPLACE PROCEDURE`     |
| PostgreSQL | pgSQL: `CREATE FUNCTION ... RETURNS void` |
| MySQL      | `CREATE PROCEDURE` (CALL для вызова)      |

Когда использовать
+ Много повторяющихся операций
+ Инкапсуляция бизнес-правил
+ Сложные агрегаты или логика с условными операторами
+ Фильтрация и валидация на уровне БД
### 20. Что такое view (Представление)?
`View` — это виртуальная таблица, основанная на результате `SELECT`-запроса.
+ Не хранит данные физически (обычные `view`)
+ Данные вычисляются на лету при обращении
+ Materialized `view` — хранит результат физически, обновляется периодически

Основные характеристики
+ Упрощает сложные запросы (инкапсулирует `JOIN` и агрегаты)
+ Повышает безопасность: пользователи могут видеть только определённые столбцы/строки
+ Можно использовать в `SELECT`, иногда в `INSERT/UPDATE/DELETE` (updatable `view`)

| Тип                    | Особенности                                                  |
| ---------------------- | ------------------------------------------------------------ |
| Простое (Simple View)  | На одной таблице, без агрегатов, можно обновлять             |
| Сложное (Complex View) | `JOIN`, агрегаты, подзапросы; обычно readonly                  |
| Materialized View      | Хранит данные физически; быстрее для чтения, требует refresh |

Updatable Views
+ Можно выполнять `INSERT`, `UPDATE`, `DELETE` только если `view` простое
+ Ограничения:
    + Без агрегатов, DISTINCT, GROUP BY
    + Без подзапросов в SELECT
+ WITH CHECK OPTION — проверяет вставляемые строки на соответствие условию view

Пример:
```sql
CREATE VIEW active_employees AS
SELECT * 
FROM employees
WHERE status = 'ACTIVE'
WITH CHECK OPTION;
```
Любая вставка через active_employees должна иметь status = 'ACTIVE'

Пример простого view
```sql
CREATE VIEW high_salary_employees AS
SELECT id, name, salary
FROM employees
WHERE salary > 50000;
```
Использование:
```sql
SELECT * FROM high_salary_employees;
Materialized view (Oracle / PostgreSQL)
CREATE MATERIALIZED VIEW mv_high_salary AS
SELECT id, name, salary
FROM employees
WHERE salary > 50000
WITH DATA;

-- обновление
REFRESH MATERIALIZED VIEW mv_high_salary;
```

Быстрое чтение, но нужно управлять актуальностью данных

Важные нюансы
+ Простое view можно использовать в JOIN, WHERE, ORDER BY
+ Complex view часто readonly
+ Materialized view экономит ресурсы на тяжелых запросах (OLAP)
+ Ограничения обновления зависят от СУБД

### 21. Что такое JDBC?
JDBC (Java Database Connectivity) — стандартный API в Java для взаимодействия с реляционными базами данных.
+ Позволяет выполнять SQL-запросы (SELECT, INSERT, UPDATE, DELETE)
+ Получать и обрабатывать результаты
+ Управлять транзакциями
+ Используется в Java SE и Java EE

| Компонент         | Описание                                           |
| ----------------- | -------------------------------------------------- |
| Driver            | Драйвер БД (от вендора), реализует интерфейсы JDBC |
| Connection        | Соединение с базой данных                          |
| Statement         | Выполнение SQL-запросов                            |
| PreparedStatement | Предкомпилированный запрос с параметрами           |
| CallableStatement | Для вызова хранимых процедур                       |
| ResultSet         | Результат SELECT-запроса                           |

+ PreparedStatement — предотвращает SQL-инъекции
+ Batch updates — для массовых вставок/обновлений
+ Auto-commit — по умолчанию true, для транзакций лучше false
+ Закрывать ресурсы (Connection, Statement, ResultSet) → try-with-resources
+ CallableStatement — вызов хранимых процедур с параметрами IN/OUT

```Java
import java.sql.*;

public class JdbcExample {
    public static void main(String[] args) {
        String url = "jdbc:mysql://localhost:3306/testdb";
        String user = "root";
        String pass = "password";

        String query = "SELECT id, name FROM employees WHERE salary > ?";

        try (Connection conn = DriverManager.getConnection(url, user, pass);
             PreparedStatement ps = conn.prepareStatement(query)) {

            ps.setDouble(1, 50000);
            try (ResultSet rs = ps.executeQuery()) {
                while (rs.next()) {
                    int id = rs.getInt("id");
                    String name = rs.getString("name");
                    System.out.println(id + " - " + name);
                }
            }
        } catch (SQLException e) {
            e.printStackTrace();
        }
    }
}
```

Регистрация драйвера (не всегда нужна в новых JDBC):

	Class.forName("com.mysql.cj.jdbc.Driver");

Transaction management:
```java
conn.setAutoCommit(false);
// execute statements
conn.commit(); // или conn.rollback();
```
PreparedStatement эффективнее, чем Statement, для повторного выполнения запросов
### 22. Что нужно для работы с той или иной БД?
1. JDBC-драйвер

+ Это JAR-файл, предоставляемый вендором СУБД.
+ Отвечает за низкоуровневое взаимодействие Java ↔ БД.

Примеры:
+ MySQL: mysql-connector-java-X.X.X.jar
+ PostgreSQL: postgresql-X.X.X.jar
+ Oracle: ojdbc8.jar

2. URL подключения

Формат:
`jdbc:<vendor>://<host>:<port>/<database>?<опции>`

Примеры:
+ MySQL: jdbc:mysql://localhost:3306/testdb?useSSL=false&serverTimezone=UTC
+ PostgreSQL: jdbc:postgresql://localhost:5432/testdb
+ Oracle: jdbc:oracle:thin:@localhost:1521:orcl

3. Учетные данные

+ Username и password для доступа к БД.
+ В production — хранить через конфигурацию, environment variables или secret manager.

4. ClassPath / зависимость

Драйвер нужно добавить в проект:
+ Для Maven: dependency в pom.xml
+ Для Gradle: implementation 'mysql:mysql-connector-java:8.1.0'
+ Либо вручную добавить JAR в ClassPath.

5. Импорты в коде Java
```java
import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.PreparedStatement;
import java.sql.ResultSet;
import java.sql.SQLException;
```

6. Регистрация драйвера (опционально в новых версиях JDBC)

   
    Class.forName("com.mysql.cj.jdbc.Driver");  // для MySQL

В JDBC 4.0+ драйверы автоматически регистрируются через SPI.
```java
Пример подключения к MySQL через JDBC
import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.SQLException;

public class DbConnectionExample {
    public static void main(String[] args) {
        String url = "jdbc:mysql://localhost:3306/testdb?useSSL=false&serverTimezone=UTC";
        String user = "root";
        String password = "password";

        try (Connection conn = DriverManager.getConnection(url, user, password)) {
            if (conn != null) {
                System.out.println("Connection successful!");
            }
        } catch (SQLException e) {
            e.printStackTrace();
        }
    }
}
```
Нюансы:
+ `useSSL=false` отключает SSL (только для тестирования)
+ `serverTimezone=UTC` решает проблему с часовым поясом в MySQL
+ Рекомендуется использовать `try-with-resources`, чтобы закрыть соединение автоматически

Пример подключения к PostgreSQL
```java
String url = "jdbc:postgresql://localhost:5432/testdb";
String user = "postgres";
String password = "secret";

try (Connection conn = DriverManager.getConnection(url, user, password)) {
    System.out.println("PostgreSQL connected!");
} catch (SQLException e) {
    e.printStackTrace();
}
```
Best practices
+ Использовать `DataSource` / connection pool (HikariCP, Apache DBCP) для production
+ Не хранить пароли в коде — использовать конфигурационные файлы или переменные среды
+ Для Spring Boot: прописывать в `application.properties` или `application.yml`
+ Всегда закрывать соединение (`try-with-resources`)
### 23. Как зарегистрировать драйвер?
Начиная с JDBC 4.0 (Java 6+), драйвер регистрируется автоматически.

Как это работает:
+ Достаточно добавить JDBC-драйвер (JAR) в classpath
+ При запуске DriverManager автоматически находит драйвер через Service Provider (ServiceLoader)
+ Вызов Class.forName() не требуется

Пример:
```java
String url = "jdbc:postgresql://localhost:5432/test";
Connection conn = DriverManager.getConnection(url, "user", "password");
```
Если драйвер есть в classpath — всё будет работать.

Старый способ (JDBC < 4.0)
+ Ранее драйвер нужно было загрузить вручную:

  Class.forName("com.mysql.cj.jdbc.Driver");

Это:
+ загружало класс драйвера
+ в статическом блоке драйвер регистрировался в DriverManager

Сейчас этот способ устарел, но иногда используется:
+ в старых проектах

| БД         | Класс драйвера                                 |
| ---------- | ---------------------------------------------- |
| MySQL      | `com.mysql.cj.jdbc.Driver`                     |
| PostgreSQL | `org.postgresql.Driver`                        |
| Oracle     | `oracle.jdbc.OracleDriver`                     |
| SQL Server | `com.microsoft.sqlserver.jdbc.SQLServerDriver` |

```Java
import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.SQLException;

public class Main {
    public static void main(String[] args) {
        String url = "jdbc:mysql://localhost:3306/test";
        String user = "root";
        String password = "1234";

        try (Connection conn = DriverManager.getConnection(url, user, password)) {
            System.out.println("Connected!");
        } catch (SQLException e) {
            e.printStackTrace();
        }
    }
}
```

1. Если драйвер не найден


	java.sql.SQLException: No suitable driver

Причина:
+ драйвер не добавлен в classpath
+ неправильный URL

2. В Maven / Gradle

Maven:
```xml
<dependency>
    <groupId>org.postgresql</groupId>
    <artifactId>postgresql</artifactId>
    <version>42.7.0</version>
</dependency>
```
3. В Spring Boot
+ Регистрация полностью автоматическая — нужно только dependency.
### 24. Как получить Connection?
В JDBC соединение с базой данных получают через:
DriverManager (простой способ)
DataSource (способ для production)

1. Через DriverManager (базовый способ)
```java
import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.SQLException;

public class Main {
    public static void main(String[] args) {
        String url = "jdbc:mysql://localhost:3306/mydb?useSSL=false&serverTimezone=UTC";
        String user = "user";
        String password = "pass";

        try (Connection conn = DriverManager.getConnection(url, user, password)) {
            System.out.println("Connection established");
        } catch (SQLException e) {
            e.printStackTrace();
        }
    }
}
```
| БД         | URL                                     |
| ---------- | --------------------------------------- |
| MySQL      | `jdbc:mysql://localhost:3306/mydb`      |
| PostgreSQL | `jdbc:postgresql://localhost:5432/mydb` |
| Oracle     | `jdbc:oracle:thin:@localhost:1521:xe`   |

2. Пример с HikariCP:
```java
import com.zaxxer.hikari.HikariConfig;
import com.zaxxer.hikari.HikariDataSource;
import java.sql.Connection;

HikariConfig config = new HikariConfig();
config.setJdbcUrl("jdbc:postgresql://localhost:5432/mydb");
config.setUsername("user");
config.setPassword("pass");

HikariDataSource ds = new HikariDataSource(config);

try (Connection conn = ds.getConnection()) {
    System.out.println("Connection from pool");
}
```

3. Hibernate

hibernate.cfg.xml
```xml
<hibernate-configuration>
    <session-factory>

        <property name="hibernate.connection.driver_class">
            org.postgresql.Driver
        </property>

        <property name="hibernate.connection.url">
            jdbc:postgresql://localhost:5432/mydb
        </property>

        <property name="hibernate.connection.username">user</property>
        <property name="hibernate.connection.password">pass</property>

        <property name="hibernate.dialect">
            org.hibernate.dialect.PostgreSQLDialect
        </property>

        <property name="hibernate.hbm2ddl.auto">update</property>

    </session-factory>
</hibernate-configuration>
```

```java
SessionFactory factory =
        new Configuration().configure().buildSessionFactory();

Session session = factory.openSession();

Transaction tx = session.beginTransaction();

User user = session.get(User.class, 1L);

tx.commit();
session.close();
```

```text
Session
   ↓
Hibernate
   ↓
DataSource
   ↓
Connection Pool
   ↓
JDBC Connection
```

4. Spring Boot
```yaml
spring:
  datasource:
    url: jdbc:postgresql://localhost:5432/mydb
    username: user
    password: pass
    driver-class-name: org.postgresql.Driver

  jpa:
    hibernate:
      ddl-auto: update
    show-sql: true
```
+ создаёт DataSource
+ подключает пул HikariCP
+ настраивает Hibernate
+ управляет транзакциями
+ закрывает Connection

Spring создаёт бин: DataSource

```text
Controller
   ↓
Service (@Transactional)
   ↓
Hibernate Session
   ↓
HikariCP Pool
   ↓
JDBC Connection
   ↓
Database
```
### 25. Что такое Statement, PreparedStatement? В чем разница между ними?
Оба интерфейса используются в JDBC для выполнения SQL-запросов, но отличаются по безопасности, производительности и способу работы с параметрами.

Statement

Statement — выполняет статический SQL, который формируется как строка.

Пример
```java
Statement stmt = conn.createStatement();
ResultSet rs = stmt.executeQuery("SELECT * FROM employees WHERE id = 1");
```
Минусы

+ SQL-инъекции

Если подставлять пользовательские данные:
```java
String id = request.getParameter("id");
Statement stmt = conn.createStatement();
ResultSet rs = stmt.executeQuery(
    "SELECT * FROM users WHERE id = " + id
);
```
Пользователь может передать:
1 OR 1=1
+ → утечка данных.
+ Низкая производительность
+ SQL компилируется БД каждый раз

PreparedStatement

PreparedStatement — предварительно компилируемый SQL с параметрами ?.

Пример
```java
PreparedStatement ps =
        conn.prepareStatement("SELECT * FROM employees WHERE id = ?");

ps.setInt(1, 1);
ResultSet rs = ps.executeQuery();
```
**Преимущества PreparedStatement**

1. Защита от SQL-инъекций
   Параметры передаются отдельно, БД не воспринимает их как SQL.

2. Производительность
+ SQL компилируется один раз
+ повторные вызовы быстрее
```java
PreparedStatement ps =
    conn.prepareStatement("INSERT INTO users(name) VALUES (?)");

for (String name : names) {
    ps.setString(1, name);
    ps.executeUpdate();
}
```
3. Удобная типизация

Методы:
+ setInt()
+ setString()
+ setLong()
+ setDate()
+ setObject()

|                    | Statement          | PreparedStatement     |
| ------------------ | ------------------ | --------------------- |
| Параметры          | Конкатенация строк | `?` + setXXX()        |
| SQL-инъекции       | Уязвим             | Защищён               |
| Компиляция         | Каждый раз         | Один раз              |
| Производительность | Ниже               | Выше (при повторении) |
| Наследование       | Базовый интерфейс  | Расширяет Statement   |

PreparedStatement хорошо подходит для массовых вставок:
```sql
PreparedStatement ps =
    conn.prepareStatement("INSERT INTO users(name) VALUES (?)");

for (String name : names) {
    ps.setString(1, name);
    ps.addBatch();
}
ps.executeBatch();
```
Преимущественно используется PreparedStatement.
### 26. Что такое ResultSet?
ResultSet — это объект JDBC, который содержит результат выполнения SQL-запроса (обычно SELECT) и позволяет построчно читать данные.

Он работает как курсор:
+ указывает на текущую строку
+ перемещается по результату
+ позволяет получать значения столбцов

Базовый пример
```java
Statement stmt = conn.createStatement();
ResultSet rs = stmt.executeQuery("SELECT id, name FROM employees");

while (rs.next()) {
    int id = rs.getInt("id");
    String name = rs.getString("name");
    System.out.println(id + " " + name);
}
```
Как работает `next()`
+ изначально курсор до первой строки
+ `next()` → переходит на следующую строку
+ возвращает false, если строки закончились

Способы получения данных

+ По имени столбца:
  `String name = rs.getString("name");`

+ По индексу (начинается с 1):
  `String name = rs.getString(2);`

| Метод       | Тип    |
| ----------- | ------ |
| `getInt()`    | int    |
| `getLong() `  | long   |
| `getString()` | String |
| `getDouble()` | double |
| `getDate()`   | Date   |
| `getObject()` | Object |

Проверка `NULL`
```java
int salary = rs.getInt("salary");
if (rs.wasNull()) {
    System.out.println("Salary is NULL");
}
```
Типы `ResultSet`

При создании:
```java
Statement stmt = conn.createStatement(
        ResultSet.TYPE_SCROLL_INSENSITIVE,
        ResultSet.CONCUR_READ_ONLY
);
```
| Тип                     | Описание                                    |
| ----------------------- | ------------------------------------------- |
| TYPE_FORWARD_ONLY       | только вперёд (по умолчанию)                |
| TYPE_SCROLL_INSENSITIVE | можно назад/вперёд, изменения в БД не видны |
| TYPE_SCROLL_SENSITIVE   | изменения в БД могут быть видны             |

Методы навигации:
+ `rs.previous();`
+ `rs.first();`
+ `rs.last();`
+ `rs.absolute(5);`
+ `rs.beforeFirst();`

Обновляемый ResultSet

Если разрешено:
```java
Statement stmt = conn.createStatement(
        ResultSet.TYPE_SCROLL_INSENSITIVE,
        ResultSet.CONCUR_UPDATABLE
);

ResultSet rs = stmt.executeQuery("SELECT id, name FROM employees");

rs.next();
rs.updateString("name", "New Name");
rs.updateRow();
```
Используется редко — обычно обновляют через `UPDATE`.

`ResultSetMetaData`

Позволяет узнать структуру результата:
```java
ResultSetMetaData meta = rs.getMetaData();

int columnCount = meta.getColumnCount();
for (int i = 1; i <= columnCount; i++) {
    System.out.println(meta.getColumnName(i));
}
```
Полезно для универсальных обработчиков.

Закрытие ресурсов

`ResultSet` связан с `Statement` и `Connection`.

Лучше использовать `try-with-resources`:
```java
String sql = "SELECT * FROM employees";

try (Connection conn = DriverManager.getConnection(url, user, pass);
     PreparedStatement ps = conn.prepareStatement(sql);
     ResultSet rs = ps.executeQuery()) {

    while (rs.next()) {
        System.out.println(rs.getString("name"));
    }
}
```
Закрытие происходит автоматически в порядке:
`ResultSet → Statement → Connection`

Нюансы
+ Не хранит все данные в памяти — строки могут подгружаться по мере чтения
+ После закрытия Statement → ResultSet становится недействительным
+ Нельзя использовать executeQuery() для INSERT/UPDATE
+ Для больших данных можно настраивать fetch size:
+ ps.setFetchSize(100);

### 27. В чем разница между методами execute, executeUpdate, executeQuery?
Эти методы используются для выполнения SQL в JDBC, но предназначены для разных типов запросов.

1. executeQuery()
+ Используется только для SELECT.
+ Возвращает: ResultSet
```java
Statement stmt = conn.createStatement();
ResultSet rs = stmt.executeQuery("SELECT * FROM employees");

while (rs.next()) {
    System.out.println(rs.getString("name"));
}
```
Если вызвать для INSERT, UPDATE, DELETE → будет SQLException

2. executeUpdate()

Используется для:
+ INSERT
+ UPDATE
+ DELETE

+ DDL (CREATE, DROP, ALTER)

Возвращает: int — количество затронутых строк
```java
Statement stmt = conn.createStatement();

int rows = stmt.executeUpdate(
        "UPDATE employees SET name = 'New' WHERE id = 1"
);

System.out.println("Updated rows: " + rows);
```
Нюансы

+ Для INSERT/UPDATE/DELETE → число строк
+ Для DDL → обычно 0

3. execute()

Универсальный метод — используется, когда неизвестно, что вернёт запрос.
+ Возвращает: boolean
    + true → есть ResultSet
    + false → есть update count
```java
Statement stmt = conn.createStatement();

boolean hasResult = stmt.execute("SELECT * FROM employees");

if (hasResult) {
    ResultSet rs = stmt.getResultSet();
} else {
    int count = stmt.getUpdateCount();
}
```
Работа с несколькими результатами

Некоторые запросы (например, хранимые процедуры) могут возвращать:
+ несколько ResultSet
+ несколько update count
```java
boolean hasResult = stmt.execute(sql);

while (true) {
    if (hasResult) {
        ResultSet rs = stmt.getResultSet();
        // обработка
    } else {
        int count = stmt.getUpdateCount();
        if (count == -1) break;
    }
    hasResult = stmt.getMoreResults();
}
```
| Метод           | Для чего                 | Возвращает                  |
| --------------- | ------------------------ | --------------------------- |
| executeQuery()  | SELECT                   | ResultSet                   |
| executeUpdate() | INSERT/UPDATE/DELETE/DDL | int (кол-во строк)          |
| execute()       | любой SQL                | boolean (есть ли ResultSet) |

Когда что использовать
+ SELECT → executeQuery()
+ INSERT/UPDATE/DELETE → executeUpdate()
+ Хранимые процедуры / неизвестный результат → execute()
+ В 90% случаев:
    + SELECT → executeQuery
    + остальное → executeUpdate

### 28. Можно ли использовать возвращаемое значение execute() для проверки, что что-то обновилось?

Нет, boolean, который возвращает execute(), не показывает, были ли обновлены строки.
Он показывает только тип результата.

Что возвращает `execute()`

		boolean result = stmt.execute(sql);

| Значение | Что означает                                                   |
| -------- | -------------------------------------------------------------- |
| `true`     | первый результат — `ResultSet` (обычно `SELECT`)                 |
| `false`    | результат — `update` count или ничего (`INSERT/UPDATE/DELETE`/DDL) |

`false` не означает, что строки были изменены.

```JAVA
Statement stmt = conn.createStatement();

boolean hasResultSet =
        stmt.execute("UPDATE employees SET name = 'New' WHERE id = 1");

if (!hasResultSet) {
    int updatedRows = stmt.getUpdateCount();
    if (updatedRows > 0) {
        System.out.println("Rows updated: " + updatedRows);
    } else {
        System.out.println("No rows updated");
    }
}
```

1. Возможные значения `getUpdateCount()`

   | Значение | Описание                               |
   | -------- | -------------------------------------- |
   | > 0      | строки обновлены                       |
   | 0        | запрос выполнен, но строки не изменены |
   | -1       | больше нет результатов                 |


2. Для DDL

CREATE TABLE ...

Обычно:
`getUpdateCount()` → 0

3. Для batch-операций

Если используется batch:
`int[] counts = stmt.executeBatch();`

Массив содержит количество изменённых строк для каждой операции.
### 29. Как получить при вставке сгенерированные ключи? Как это сделать на чистом sql?
Нужно создать PreparedStatement с флагом:

    Statement.RETURN_GENERATED_KEYS

```java
String sql = "INSERT INTO employees(name) VALUES (?)";

PreparedStatement ps = conn.prepareStatement(
        sql,
        Statement.RETURN_GENERATED_KEYS
);

ps.setString(1, "John");
ps.executeUpdate();

ResultSet keys = ps.getGeneratedKeys();

if (keys.next()) {
    int id = keys.getInt(1);
    System.out.println("Generated id: " + id);
}
```

+ БД генерирует ID
+ JDBC запрашивает его после выполнения
+ getGeneratedKeys() возвращает ResultSet

Работает только для авто-генерируемых колонок
+ AUTO_INCREMENT
+ IDENTITY
+ SERIAL

| СУБД       | Способ                    |
| ---------- | ------------------------- |
| PostgreSQL | RETURNING                 |
| MySQL      | LAST_INSERT_ID()          |
| SQL Server | OUTPUT INSERTED.id        |
| Oracle     | RETURNING INTO / sequence |

### 30. Для чего используется конструкция try-with-resources?
try-with-resources — это синтаксис Java (с версии 7), который позволяет автоматически закрывать ресурсы, реализующие интерфейс AutoCloseable (например, JDBC Connection, Statement, ResultSet).

Таким образом, даже при возникновении исключения ресурсы будут закрыты — утечки соединений или файлов исключены.
```java
Синтаксис
try (ResourceType r = new ResourceType()) {
    // Работа с ресурсом
} catch (ExceptionType e) {
    // Обработка ошибок
}
```
JDBC-пример

```java
String sql = "SELECT * FROM employees";

try (Connection conn = DriverManager.getConnection(url, user, pass);
     PreparedStatement ps = conn.prepareStatement(sql);
     ResultSet rs = ps.executeQuery()) {

    while (rs.next()) {
        System.out.println(rs.getString("name"));
    }

} catch (SQLException e) {
    e.printStackTrace();
}
// conn, ps, rs закрыты автоматически
```
Нюансы
+ Несколько ресурсов
```java
try (Connection conn = ...;
     Statement stmt = ...;
     ResultSet rs = ...) {
    ...
}
```
+ Все ресурсы будут закрыты в обратном порядке их объявления.

*Suppressing exceptions*

Если внутри блока try возникает исключение, а при закрытии ресурсов — другое, второе подавляется, чтобы основное исключение не потерялось.

Преимущество перед `finally`
+ Раньше приходилось писать:
```java
Connection conn = null;
try {
    conn = DriverManager.getConnection(url, user, pass);
    // Работа
} finally {
    if (conn != null) conn.close();
}
```
`try-with-resources` кратче и безопаснее

Почему важно в JDBC
+ Connection, Statement и ResultSet — ресурсоёмкие
+ Если не закрыть → утечка соединений, зависание пула, ошибки
+ try-with-resources гарантирует корректное закрытие даже при SQLException