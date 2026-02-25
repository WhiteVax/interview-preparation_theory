## *SQL-JDBC*

- [Что такое SQL?]()
- [Что такое DML и DDL?]()
- [Что такое первичный ключ?]()
- [Что такое внешний ключ?]()
- [Какие виды связей между таблицами существуют и как они организуются?]()
- [Опишите как вставить, удалить, обновить данные в(из) таблицу(ы).]()
- [Что такое нормализация БД?]()
- [Что такое денормализация БД? Для чего она нужна?]()
- [Что такое кластерный и некластерный индексы?]()
- [Какие типы соединений (join) таблиц существуют? В чем их разница?]()
- [Что такое SQL курсор?]()
- [Опишите шаги по созданию и использованию курсора.]()
- [Что такое транзакция?]()
- [Что такое триггер? Какие типы триггеров Вы знаете?]()
- [В чем разница между where и having?]()
- [Что такое подзапрос (sub-query)?]()
- [Что такое union?]()
- [Что такое group by?]()
- [Что такое хранимые процедуры?]()
- [Что такое view (Представление)?]()
- [Что такое JDBC?]()
- [Что нужно для работы с той или иной БД?]()
- [Как зарегистрировать драйвер?]()
- [Как получить Connection?]()
- [Что такое Statement, PreparedStatement? В чем разница между ними?]()
- [Что такое ResultSet?]()
- [В чем разница между методами execute, executeUpdate, executeQuery?]()
- [Можно ли использовать возвращаемое значение execute() для проверки, что что-то обновилось?]()
- [Как получить при вставке сгенерированные ключи? Как это сделать на чистом sql?]()
- [Для чего используется конструкция try-with-resources?]()

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
### 12. Опишите шаги по созданию и использованию курсора.
### 13. Что такое транзакция?
### 14. Что такое триггер? Какие типы триггеров Вы знаете?
### 15. В чем разница между where и having?
### 16. Что такое подзапрос (sub-query)?
### 17. Что такое union?
### 18. Что такое group by?
### 19. Что такое хранимые процедуры?
### 20. Что такое view (Представление)?
### 21. Что такое JDBC?
### 22. Что нужно для работы с той или иной БД?
### 23. Как зарегистрировать драйвер?
### 24. Как получить Connection?
### 25. Что такое Statement, PreparedStatement? В чем разница между ними?
### 26. Что такое ResultSet?
### 27. В чем разница между методами execute, executeUpdate, executeQuery?
### 28. Можно ли использовать возвращаемое значение execute() для проверки, что что-то обновилось?
### 29. Как получить при вставке сгенерированные ключи? Как это сделать на чистом sql?
### 30. Для чего используется конструкция try-with-resources?

