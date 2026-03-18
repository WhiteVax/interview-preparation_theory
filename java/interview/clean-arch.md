## *Clean arch*

- [Что такое ООД? Какие задачи выполняет ООД?](#1-что-такое-оод-какие-задачи-выполняет-оод)
- [Расскажите о принципах KISS, DRY, YAGNI?](#2-расскажите-о-принципах-kiss-dry-yagni)
- [Что такое SOLID?](#3-что-такое-solid)
- [Для чего используется JavaDoc?](#4-для-чего-используется-javadoc)
- [Как писать JavaDoc? Опишите основные теги @param, @return, @throws.](#5-как-писать-javadoc-опишите-основные-теги-param-return-throws)
- [Расскажите про принципы составления Java класса.](#6-расскажите-про-принципы-составления-java-класса)
- [Что такое SRP?](#7-что-такое-srp)
- [Расскажите о нарушениях принципа SRP.](#8-расскажите-о-нарушениях-принципа-srp)
- [Каковы последствия нарушения SRP?](#9-каковы-последствия-нарушения-srp)
- [Что такое OCP?](#10-что-такое-ocp)
- [Расскажите о нарушениях принципа OCP.](#11-расскажите-о-нарушениях-принципа-ocp)
- [Каковы последствия нарушения OCP?](#12-каковы-последствия-нарушения-ocp)
- [Что такое LSP?](#13-что-такое-lsp)
- [Расскажите о нарушениях принципа LSP.](#14-расскажите-о-нарушениях-принципа-lsp)
- [Каковы последствия нарушения LSP?](#15-каковы-последствия-нарушения-lsp)
- [Что такое ISP?](#16-что-такое-isp)
- [Расскажите о нарушениях принципа ISP.](#17-расскажите-о-нарушениях-принципа-isp)
- [Каковы последствия нарушения ISP?](#18-каковы-последствия-нарушения-isp)
- [Что такое DIP?](#19-что-такое-dip)
- [Расскажите о нарушениях принципа DIP.](#20-расскажите-о-нарушениях-принципа-dip)
- [Каковы последствия нарушения DIP?](#21-каковы-последствия-нарушения-dip)
- [Расскажите, что такое автоматизированное тестирование.](#22-расскажите-что-такое-автоматизированное-тестирование)
- [Как в Java осуществляется автоматизированное тестирование?](#23-как-в-java-осуществляется-автоматизированное-тестирование)
- [Что такое JUnit? Как использовать ее для тестирования?](#24-что-такое-junit-как-использовать-ее-для-тестирования)
- [Что такое функциональное тестирование и чем отличается оно от модульного?](#25-что-такое-функциональное-тестирование-и-чем-отличается-оно-от-модульного)
- [Расскажите про методологию TDD.](#26-расскажите-про-методологию-tdd)
- [Расскажите про методологию BDD.](#27-расскажите-про-методологию-bdd)
- [Что такое тестирование черным, белым, серым ящиком?](#28-что-такое-тестирование-черным-белым-серым-ящиком)
- [Опишите типы тестов: модульное, интеграционное, функциональное, приемочное?](#29-опишите-типы-тестов-модульное-интеграционное-функциональное-приемочное)

---

### 1. Что такое ООД? Какие задачи выполняет ООД?
Объектно-ориентированный дизайн (ООД, OOD — Object-Oriented Design) — это этап проектирования программного обеспечения, на котором система разбивается на объекты, определяются их свойства, методы и связи, чтобы реализовать требования системы с использованием принципов объектно-ориентированного программирования.

ООД отвечает на вопрос — как именно будет устроена система внутри (классы, объекты, их взаимодействие).

Основные задачи ООД

1️. Определение объектов системы
+ Выявляются основные сущности предметной области, которые будут представлены в программе как классы.

Интернет-магазин

+ User
+ Product
+ Order
+ Cart

2️. Определение атрибутов объектов
+ Определяются свойства, которые описывают состояние объекта.

User
- id
- name
- email

3️. Определение методов (поведения)
+ Определяются действия, которые может выполнять объект.

User
+ login()
+ logout()
+ changePassword()

4️. Определение связей между объектами
+ Определяются отношения между классами:

Типы связей:

| Связь       | Описание                                             |
| ----------- | ---------------------------------------------------- |
| Association | объекты взаимодействуют                              |
| Aggregation | объект содержит другие                               |
| Composition | сильная зависимость (часть не существует без целого) |
| Inheritance | наследование                                         |
| Dependency  | временная зависимость                                |


5️. Определение взаимодействия объектов
Определяется, как объекты обмениваются сообщениями и вызывают методы друг друга.
```java
User -> Order.create()
Order -> Payment.pay()
```
6️. Проектирование структуры классов

Создается архитектура системы:
+ классы
+ интерфейсы
+ абстракции
+ зависимости

Часто используются UML-диаграммы классов.

Основные шаги OOD
+ Выявление объектов системы
+ Определение их атрибутов (свойств)
+ Определение методов (поведения)
+ Определение связей между объектами
+ Определение взаимодействия объектов
+ Определение интерфейсов и структуры классов
+ Результат ООД

После этапа ООД появляется:
+ модель классов
+ UML-диаграммы
+ описание взаимодействий
+ архитектура системы

То есть готовая схема, по которой разработчики пишут код.

```java
Система: банк
Классы:

class BankAccount {
    String accountNumber;
    double balance;

    void deposit(double amount);
    void withdraw(double amount);
}
```
Связь:
`Customer 1 ---- * BankAccount`

Коротко

ООД (Object-Oriented Design) — это процесс проектирования программной системы, при котором система моделируется как набор взаимодействующих объектов.

Основные задачи:
+ выявление объектов системы;
+ определение их свойств и методов;
+ определение связей между объектами;
+ определение взаимодействия объектов;
+ построение структуры классов и интерфейсов.
### 2. Расскажите о принципах KISS, DRY, YAGNI?
Принципы KISS, DRY, YAGNI
1. DRY — Don't Repeat Yourself

DRY (Не повторяй себя) — принцип, согласно которому каждая часть знаний или логики в системе должна иметь единственное представление, если код повторяется — его нужно вынести в одно место.

Что нарушает DRY
+ Copy-paste кода
+ Дублирование бизнес-логики
+ Повторяющиеся SQL запросы
+ Повторяющиеся алгоритмы

Пример плохого кода
```java
double price1 = productPrice * 1.2;
double price2 = servicePrice * 1.2;
```
Лучше
```java
double applyTax(double price) {
    return price * 1.2;
}

Использование:
applyTax(productPrice);
applyTax(servicePrice);
```
Преимущества:
+ легче изменять код
+ меньше ошибок
+ лучше поддержка

2. YAGNI — You Aren't Gonna Need It

YAGNI (Вам это не понадобится) — принцип, который говорит:
+ не реализовывайте функциональность, пока она действительно не нужна.

Очень частая ошибка разработчиков — добавлять функциональность "на будущее".

Плохой пример

Разработчик делает:
+ UserService
+ UserRepository
+ UserCache
+ UserAnalytics
+ UserExport
+ UserImport

Хотя пока нужен только:
UserService

Правильно

Добавлять функциональность только когда она реально требуется.

Преимущества:
+ код проще
+ меньше лишних классов
+ быстрее разработка
+ легче поддержка

3. KISS — Keep It Simple, Stupid

KISS — принцип простоты.

Суть:
система должна быть максимально простой.

Нужно избегать:
+ сложных конструкций
+ избыточной архитектуры
+ "умных" но непонятных решений

Плохой пример

Сложная логика:
```java
if ((a && b) || (!a && c && !d))
Лучше
boolean canProcess = isAuthorized && hasPermission;
if (canProcess) {
    process();
}
```
Другой пример

Слишком сложная архитектура:
+ Factory
+ AbstractFactory
+ Builder
+ Strategy
+ Adapter

Для маленького проекта.

Лучше:

+ 1-2 простых класса

Кратко

KISS, DRY и YAGNI — это базовые принципы проектирования кода.

DRY (Don't Repeat Yourself)
— избегать дублирования кода, выносить общую логику в отдельные методы или классы.

YAGNI (You Aren't Gonna Need It)
— не реализовывать функциональность заранее, добавлять только то, что необходимо сейчас.

KISS (Keep It Simple)
— стремиться к максимально простым и понятным решениям.
### 3. Что такое SOLID?
SOLID — это акроним из 5 принципов объектно-ориентированного проектирования, которые помогают создавать гибкий, расширяемый и поддерживаемый код.

Главная цель SOLID — проектировать классы и модули так, чтобы они:
+ легко изменялись
+ были понятными
+ были переиспользуемыми
+ имели слабую связанность (low coupling)

Принципы SOLID
1. S — Single Responsibility Principle (SRP)

Принцип единственной ответственности.
+ Класс должен иметь только одну причину для изменения.
+ То есть класс должен отвечать за одну задачу.

Плохой пример
```java
class UserService {
    void saveUser(User user) {}
    void sendEmail(User user) {}
    void generateReport() {}
}
```
Этот класс:
+ работает с БД
+ отправляет email
+ делает отчёты
+ Слишком много обязанностей.

Хороший пример
```java
class UserService {
    void saveUser(User user) {}
}

class EmailService {
    void sendEmail(User user) {}
}

class ReportService {
    void generateReport() {}
}
```
2. O — Open/Closed Principle (OCP)

Принцип открытости/закрытости.

Классы должны быть:
+ открыты для расширения
+ закрыты для изменения
+ Новую функциональность нужно добавлять через расширение, а не изменяя существующий код.

```java
interface Shape {
    double area();
}
```
Классы:
```java
class Circle implements Shape { }
class Rectangle implements Shape { }
```
Чтобы добавить новую фигуру:
`class Triangle implements Shape { }`

+ старый код менять не нужно.

3. L — Liskov Substitution Principle (LSP)

Принцип подстановки Барбары Лисков.

Объекты подкласса должны полностью заменять объекты родительского класса, не ломая программу.

Если B наследует A, то B должен работать везде, где ожидается A.

Плохой пример
```java
class Bird {
    void fly() {}
}

class Penguin extends Bird {
    void fly() {
        throw new UnsupportedOperationException();
    }
}
```
+ Пингвин не умеет летать → нарушает LSP.

Лучше
```java
class Bird {}

class FlyingBird extends Bird {
    void fly() {}
}

class Penguin extends Bird {}
```
4. I — Interface Segregation Principle (ISP)

Принцип разделения интерфейсов.

+ Лучше иметь несколько маленьких интерфейсов, чем один большой.
+ Клиенты не должны зависеть от методов, которые они не используют.

Плохой пример
```java
interface Worker {
    void work();
    void eat();
}
```
Робот:
```java
class Robot implements Worker {
    void work() {}
    void eat() {} // не нужно
}
```
Хороший пример
```java
interface Workable {
    void work();
}

interface Eatable {
    void eat();
}
````
5. D — Dependency Inversion Principle (DIP)

Принцип инверсии зависимостей.
+ Модули верхнего уровня не должны зависеть от модулей нижнего уровня.
+ Они должны зависеть от абстракций (интерфейсов).

Также:
+ абстракции не должны зависеть от деталей
+ детали должны зависеть от абстракций

Плохой пример
```java
class OrderService {
    private MySQLDatabase database = new MySQLDatabase();
}
```
Жёсткая зависимость.

Хороший пример
```java
interface Database {
    void save();
}
class MySQLDatabase implements Database {}
class OrderService {
    private Database database;

    OrderService(Database database) {
        this.database = database;
    }
}
```
Теперь можно легко заменить:

+ MySQL
+ PostgreSQL
+ MongoDB

Кратко

| Принцип                       | Суть                                          |
| ----------------------------- | --------------------------------------------- |
| **S** — Single Responsibility | класс должен иметь одну ответственность       |
| **O** — Open/Closed           | открыты для расширения, закрыты для изменения |
| **L** — Liskov Substitution   | подклассы должны заменять родительские классы |
| **I** — Interface Segregation | лучше много маленьких интерфейсов             |
| **D** — Dependency Inversion  | зависимость от абстракций, а не реализаций    |

### 4. Для чего используется JavaDoc?
Javadoc — это инструмент из JDK, который используется для автоматической генерации документации в HTML из комментариев исходного кода Java.

Он анализирует специальные комментарии (/** ... */) перед классами, методами и полями и на их основе создаёт API-документацию.

Для чего используется Javadoc

1. Генерация документации API

Из комментариев в коде создаётся HTML-документация, которая описывает:
+ классы
+ методы
+ параметры
+ возвращаемые значения
+ исключения

```java
/**
 * Calculates sum of two numbers
 * @param a first number
 * @param b second number
 * @return sum of numbers
 */
public int sum(int a, int b) {
    return a + b;
}
```
После генерации появится HTML-страница с описанием метода.

2. Документирование кода

Javadoc помогает разработчикам понимать:
+ назначение класса
+ что делает метод
+ какие параметры принимает
+ что возвращает
  
Это особенно важно для публичных API библиотек.

3. Помощь IDE

Многие IDE (IntelliJ IDEA, Eclipse) используют Javadoc для:
+ подсказок при наведении
+ автодополнения
+ быстрого просмотра документации

Когда наводишь на метод — IDE показывает текст из Javadoc.

4. Анализ структуры программы

Javadoc также предоставляет API для создания:
+ Doclet — инструменты для обработки структуры программы
+ Taglet — кастомные теги документации
### 5. Как писать JavaDoc? Опишите основные теги @param, @return, @throws.
Используется специальный формат комментариев.
```java
/**
 * комментарий документации
 */
```
В начале две *, а не одна (/* */ — обычный комментарий).

Javadoc можно писать перед:
+ классами
+ интерфейсами
+ методами
+ конструкторами
+ полями

Комментарий обязательно располагается перед документируемым элементом.

Типы тегов Javadoc

Существует два типа тегов.

1️. Блочные (автономные)

Начинаются с @ и пишутся с новой строки.

Примеры:

+ @param
+ @return
+ @throws
+ @author
+ @version
+ @since
+ @see
+ @deprecated

2️. Встроенные (inline)

Используются внутри текста и заключаются в {}.

Пример:
`{@link ClassName}`

**Основные теги Javadoc**

1. @param

Используется для описания параметров метода.

Формат:
`@param имя_параметра описание`

```java
/**
 * Calculates sum
 * @param a first number
 * @param b second number
 */
public int sum(int a, int b) {
    return a + b;
}
```
2. @return

Описывает возвращаемое значение метода.

Формат:

`@return` описание возвращаемого значения

```java
/**
 * @return сумма двух чисел
 */
public int sum(int a, int b) {
    return a + b;
}
```
Используется только для методов, которые возвращают значение.

3. `@throws / @exception`

Используется для описания исключений, которые может выбросить метод.

Формат:
`@throws ExceptionClass описание`

```java
/**
 * Reads file
 * @param path file path
 * @return file content
 * @throws IOException if file not found
 */
public String readFile(String path) throws IOException {
}
```
@exception — устаревший синоним `@throws`.

```java
/**
 * Класс реализует простую очередь задач.
 * Очередь работает по принципу FIFO.
 *
 * @author Vova
 * @version 1.0
 * @since 1.0
 */
public class PriorityQueue {

    /**
     * Список задач
     */
    private LinkedList<Task> tasks = new LinkedList<>();

    /**
     * Добавляет задачу в очередь.
     *
     * @param task задача для добавления
     */
    public void put(Task task) {
        tasks.add(task);
    }

    /**
     * Получает первую задачу из очереди.
     *
     * @return задача из очереди или null если очередь пуста
     */
    public Task take() {
        return tasks.poll();
    }
}
```
| Тег            | Где используется   | Назначение                            | Пример                                    |
| -------------- | ------------------ | ------------------------------------- | ----------------------------------------- |
| `@author`      | класс, интерфейс   | указывает автора                      | `@author Vlad`                            |
| `@version`     | класс, интерфейс   | версия класса                         | `@version 1.0`                            |
| `@since`       | класс, метод, поле | версия, с которой элемент доступен    | `@since 1.8`                              |
| `@param`       | метод, конструктор | описание параметра                    | `@param name имя пользователя`            |
| `@return`      | метод              | описание возвращаемого значения       | `@return список пользователей`            |
| `@throws`      | метод              | описание выбрасываемого исключения    | `@throws IOException если файл не найден` |
| `@exception`   | метод              | устаревший аналог `@throws`           | `@exception SQLException ошибка БД`       |
| `@see`         | класс, метод, поле | ссылка на другой элемент документации | `@see UserService`                        |
| `@deprecated`  | класс, метод, поле | пометка устаревшего элемента          | `@deprecated use newMethod()`             |
| `{@link}`      | внутри текста      | ссылка на класс или метод             | `{@link User}`                            |
| `{@linkplain}` | внутри текста      | ссылка без форматирования code        | `{@linkplain User}`                       |
| `{@code}`      | внутри текста      | отображает код                        | `{@code List<String>}`                    |
| `{@literal}`   | внутри текста      | вывод текста без обработки HTML       | `{@literal <html>}`                       |
| `{@value}`     | поле               | вывод значения константы              | `{@value MAX_SIZE}`                       |
| `@serial`      | поле               | описание сериализуемого поля          | `@serial`                                 |
| `@serialField` | поле               | описание поля сериализации            | `@serialField name type description`      |
| `@serialData`  | метод              | описание данных сериализации          | `@serialData`                             |
| `@inheritDoc`  | метод              | наследование документации родителя    | `{@inheritDoc}`                           |
| `@implSpec`    | метод              | описание реализации                   | `@implSpec implementation detail`         |
| `@implNote`    | метод              | заметки реализации                    | `@implNote optimized version`             |
| `@apiNote`     | метод, класс       | примечание для пользователей API      | `@apiNote thread-safe`                    |
| `@hidden`      | класс, метод       | скрыть элемент из документации        | `@hidden`                                 |
| `@provides`    | модуль             | описание предоставляемого сервиса     | `@provides Service with Impl`             |
| `@uses`        | модуль             | описание используемого сервиса        | `@uses LoggerService`                     |

Генерация Javadoc документации в Maven выполняется с помощью плагина maven-javadoc-plugin. Этот плагин анализирует Javadoc-комментарии в исходном коде и генерирует HTML-документацию API.

Подключение плагина в Maven

В файл pom.xml добавляется плагин:
```xml
<plugin>
    <groupId>org.apache.maven.plugins</groupId>
    <artifactId>maven-javadoc-plugin</artifactId>
    <version>3.2.0</version>
</plugin>
```
Этот плагин вызывает утилиту javadoc, входящую в JDK, и генерирует документацию по комментариям /** ... */.

Запуск генерации выполняется командой:

`mvn javadoc:javadoc`

или

`mvn site`

После генерации файлы создаются в каталоге:

`target/site/apidocs`

```text
target
 └─ site
     └─ apidocs
         ├─ index.html
         ├─ allclasses.html
         ├─ package-summary.html
```

`target/site/apidocs/index.html`

HTML-документация включает:
+ список пакетов
+ список классов
+ описание классов
+ описание методов
+ параметры (@param)
+ возвращаемые значения (@return)
+ исключения (@throws)
+ ссылки (@see, {@link})
### 6. Расскажите про принципы составления Java класса.
Принципы составления Java-класса — это правила организации структуры класса, которые делают код понятным, читаемым и поддерживаемым. Они включают как синтаксическую структуру, так и рекомендации по оформлению и проектированию.

1. Правильный порядок элементов класса

В Java принято соблюдать определённый порядок объявления элементов внутри класса.

| Порядок | Элемент                            |
| ------- | ---------------------------------- |
| 1       | статические поля (`static fields`) |
| 2       | обычные поля (instance fields)     |
| 3       | конструкторы                       |
| 4       | публичные методы                   |
| 5       | защищённые методы (`protected`)    |
| 6       | приватные методы                   |
| 7       | вложенные классы                   |

```java
public class User {

    private static final int MAX_USERS = 100;

    private String name;
    private int age;

    public User(String name, int age) {
        this.name = name;
        this.age = age;
    }

    public String getName() {
        return name;
    }

    private void validate() {
    }
}
```
2. Инкапсуляция (Encapsulation)

Поля класса обычно делают private, чтобы ограничить доступ к ним.

Доступ осуществляется через геттеры и сеттеры.

```java
public class User {

    private String name;

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }
}
```
+ контроль доступа
+ защита состояния объекта
+ возможность добавлять валидацию

3. Принцип одной ответственности (SRP)

Класс должен выполнять одну логическую задачу.
```java
class UserService {
    void saveUser() {}
    void sendEmail() {}
    void generateReport() {}
}
```
Лучше разделить:
+ `UserService`
+ `EmailService`
+ `ReportService`

4. Минимизация зависимостей

Класс должен иметь минимальное количество зависимостей от других классов.

Лучше использовать:
+ интерфейсы
+ внедрение зависимостей (Dependency Injection)
```java
class OrderService {

    private PaymentService paymentService;

    public OrderService(PaymentService paymentService) {
        this.paymentService = paymentService;
    }
}
```
5. Именование

Названия классов должны:
+ быть существительными
+ отражать назначение
+ использовать CamelCase
```java
User
OrderService
PaymentProcessor
```
6. Использование модификаторов доступа

Java имеет 4 уровня доступа:

| Модификатор | Доступ             |
| ----------- | ------------------ |
| `public`    | везде              |
| `protected` | пакет + наследники |
| `default`   | внутри пакета      |
| `private`   | внутри класса      |

+ поля → `private`
+ методы API → `public`
+ вспомогательные методы → `private`

7. Использование конструкторов

Конструкторы используются для инициализации объекта.
```java
public class User {

    private String name;

    public User(String name) {
        this.name = name;
    }
}
```
Если параметров много (больше 5-8) — используют Builder pattern.

8. Документирование

Классы и методы должны документироваться через Javadoc.
```java
/**
 * Service for working with users
 */
public class UserService {
}
```
9. Следование принципам проектирования

Хороший класс должен следовать принципам:
+ SOLID
+ KISS
+ DRY
+ YAGNI

```java
/**
 * Represents a user of the system.
 */
public class User {

    private String name;
    private int age;

    public User(String name, int age) {
        this.name = name;
        this.age = age;
    }

    public String getName() {
        return name;
    }

    public int getAge() {
        return age;
    }
}
```
Коротко

Основные принципы составления Java-класса:
+ соблюдать порядок элементов класса
+ использовать инкапсуляцию (private поля + getters/setters)
+ класс должен иметь одну ответственность
+ минимизировать зависимости
+ использовать правильные имена классов
+ применять модификаторы доступа
+ использовать конструкторы для инициализации
+ документировать код с помощью Javadoc

### 7. Что такое SRP?
**SRP (принцип единственной ответственности)** гласит:
+ программная сущность (класс, модуль, пакет) должна иметь **только одну причину для изменения**.

Важно: речь не просто про «одну обязанность», а про **одного актора (роль)**, который инициирует изменения.

На проекте есть разные акторы:

* Аналитик (бизнес-логика)
* UX/UI дизайнер (представление)
* DBA (работа с БД)

Если класс изменяется по причинам, связанным с разными ролями — это **нарушение SRP**.

SRP =
**Один класс → одна роль → одна причина для изменения**
### 8. Расскажите о нарушениях принципа SRP.
1. **Слишком широкий функционал**

    * Класс делает сразу несколько вещей (например: бизнес-логика + логирование + сохранение в БД)

2. **Класс создает объекты (new) помимо своей основной задачи**

    * Нарушение разделения ответственности (лучше использовать фабрики / DI)

3. **Несвязанные операции**

    * Методы не связаны общей целью
    * Например:

        * форматирование даты
        * работа с файлами
        * бизнес-логика в одном классе

4. **Зависимость от внешних правил**

    * Например:
        * формат даты
        * правила валидации
        * условия поиска
          * → такие вещи должны выноситься отдельно
### 9. Каковы последствия нарушения SRP?
* Изменения приходится делать **в нескольких местах**
* Повышается риск ошибок
* Усложняется тестирование
* Код становится менее читаемым и поддерживаемым

Пример нарушения SRP

```java
class UserService {

    public void registerUser(String name) {
        // бизнес-логика
        System.out.println("Register user: " + name);

        // сохранение в БД
        saveToDatabase(name);

        // логирование
        log("User registered: " + name);
    }

    private void saveToDatabase(String name) {
        System.out.println("Saving to DB: " + name);
    }

    private void log(String message) {
        System.out.println("LOG: " + message);
    }
}
```

**Проблема:**

* бизнес-логика (Analyst)
* работа с БД (DBA)
* логирование (DevOps)

→ **3 причины для изменения → нарушение SRP**

Исправленный вариант

```java
class UserService {

    private UserRepository repository;
    private Logger logger;

    public UserService(UserRepository repository, Logger logger) {
        this.repository = repository;
        this.logger = logger;
    }

    public void registerUser(String name) {
        System.out.println("Register user: " + name);
        repository.save(name);
        logger.log("User registered: " + name);
    }
}

class UserRepository {
    public void save(String name) {
        System.out.println("Saving to DB: " + name);
    }
}

class Logger {
    public void log(String message) {
        System.out.println("LOG: " + message);
    }
}
```

**Теперь:**

* `UserService` → бизнес-логика
* `UserRepository` → работа с БД
* `Logger` → логирование

→ у каждого класса **одна причина для изменения**

### 10. Что такое OCP?
**OCP (Open Closed Principle принцип открытости/закрытости)** гласит:
программные сущности (классы, модули, функции) должны быть:

* **открыты для расширения**
* **закрыты для изменения**

Это означает:
новое поведение должно добавляться **без изменения существующего кода**.

Почему важно избегать изменений

1. **Риск поломки**

   * Любое изменение может сломать уже работающий функционал
   * Чем больше кодовая база, тем выше риск

2. **Рост системы**

   * Требования постоянно меняются
   * Гораздо безопаснее **добавлять новый код**, чем менять старый


Как достигается OCP

Основной инструмент — **динамический полиморфизм**:

* интерфейсы
* абстрактные классы
* композиция

Идея: поведение задаётся через абстракцию, а не через жёсткую реализацию

Пример нарушения OCP

```java
class Calculator {

    public int calculate(String operation, int a, int b) {
        if (operation.equals("add")) {
            return a + b;
        } else if (operation.equals("sub")) {
            return a - b;
        } else if (operation.equals("mul")) {
            return a * b;
        }
        throw new IllegalArgumentException();
    }
}
```

**Проблема:**

* Чтобы добавить новую операцию → нужно **изменять класс**
* Нарушение OCP

Исправленный вариант (через полиморфизм)

```java
interface Operation {
    int apply(int a, int b);
}

class AddOperation implements Operation {
    public int apply(int a, int b) {
        return a + b;
    }
}

class SubOperation implements Operation {
    public int apply(int a, int b) {
        return a - b;
    }
}

class Calculator {
    public int calculate(Operation operation, int a, int b) {
        return operation.apply(a, b);
    }
}
```

**Теперь:**

* Добавление новой операции = новый класс
* `Calculator` не изменяется → соблюден OCP

### Альтернатива (лямбды)

```java
class Calculator {
    public int calculate(java.util.function.BiFunction<Integer, Integer, Integer> op,
                         int a, int b) {
        return op.apply(a, b);
    }
}
```

Использование:

```java
calculator.calculate((a, b) -> a + b, 5, 3);
calculator.calculate((a, b) -> a - b, 5, 3);
```

OCP =
Не изменяй рабочий код → расширяй через абстракции

### 11. Расскажите о нарушениях принципа OCP.
Примеры нарушений OCP

1. if / switch по типу (классический антипаттерн)

```java
class PaymentService {

    public void pay(String type, double amount) {
        if (type.equals("card")) {
            System.out.println("Pay by card: " + amount);
        } else if (type.equals("paypal")) {
            System.out.println("Pay by PayPal: " + amount);
        } else if (type.equals("crypto")) {
            System.out.println("Pay by crypto: " + amount);
        }
    }
}
```

**Проблема:**

* Добавление нового способа оплаты → изменение метода `pay`
* Нарушение OCP

2. Жёсткая зависимость от конкретных классов (new внутри)

```java
class OrderService {

    public void process() {
        EmailSender sender = new EmailSender(); // жесткая привязка
        sender.send("Order processed");
    }
}

class EmailSender {
    public void send(String msg) {
        System.out.println("Email: " + msg);
    }
}
```

**Проблема:**

* Хотим добавить SMS / Push → нужно менять `OrderService`
* Нет расширяемости

3. Изменение логики при каждом новом кейсе

```java
class DiscountCalculator {

    public double calculate(String customerType, double price) {
        if (customerType.equals("regular")) {
            return price * 0.9;
        } else if (customerType.equals("vip")) {
            return price * 0.8;
        }
        return price;
    }
}
```

**Проблема:**

* Новый тип клиента → изменение метода
* Код постоянно переписывается

4. Смешивание логики разных уровней

```java
class ReportService {

    public void generate(String type) {
        if (type.equals("pdf")) {
            System.out.println("Generate PDF");
        } else if (type.equals("excel")) {
            System.out.println("Generate Excel");
        }

        // сразу же отправка
        System.out.println("Send report via email");
    }
}
```

**Проблема:**

* Генерация + отправка в одном месте
* Расширение форматов → изменение класса

5. Неправильное использование наследования

```java
class Bird {
    public void fly() {
        System.out.println("Flying");
    }
}

class Penguin extends Bird {
    @Override
    public void fly() {
        throw new UnsupportedOperationException(); // ломаем поведение
    }
}
```

**Проблема:**

* Иерархия неустойчива (Penguin не "is-a" Bird в контексте полёта)
* При изменениях базового класса могут ломаться наследники

**Как распознать нарушение**

Если при добавлении новой функциональности ты:

* открываешь старый класс
* добавляешь `if / else`
* или меняешь существующий код

почти наверняка нарушается **OCP**

**Наследование vs интерфейсы**

* Используй наследование, если есть устойчивое отношение **"is-a"**
* Если есть сомнения → лучше **интерфейсы + композиция**
* Помни: наследование тянет за собой **состояние**, что не всегда нужно

Нарушение OCP =
каждая новая фича → изменение старого кода
### 12. Каковы последствия нарушения OCP?
* При добавлении функционала нужно менять существующий код
* Повышается риск багов
* Дублирование изменений в разных местах
* Ухудшается поддерживаемость
### 13. Что такое LSP?
**LSP (Liskov Substitution Principle принцип подстановки Лисков)** гласит:
объекты подклассов должны **без проблем заменять объекты базового класса**, не изменяя корректность программы.

Проще:
если код работает с базовым типом, он должен так же работать с любым его наследником **без дополнительной логики и проверок**.

Связь с OCP

LSP обеспечивает корректную работу **OCP**:

* если наследники ведут себя неправильно → приходится добавлять `if / instanceof`
* это ломает расширяемость

Контракты LSP

Чтобы не нарушать принцип, соблюдаются следующие правила:

1. **Предусловия (Preconditions)**

   * нельзя **усиливать** в подклассе
     (нельзя требовать больше, чем базовый класс)

2. **Постусловия (Postconditions)**

   * нельзя **ослаблять**
     (результат должен быть не хуже ожидаемого)

3. **Инварианты (Invariants)**

   * должны сохраняться
     (состояние объекта остаётся корректным)

LSP =
Наследник должен полностью заменять базовый класс без сюрпризов

### 14. Расскажите о нарушениях принципа LSP.
Примеры нарушений LSP

1. Изменение контракта метода (исключение вместо результата)

```java
class OrderStockValidator {

    public boolean isValid(Order order) {
        for (Item item : order.getItems()) {
            if (!item.isInStock()) {
                return false;
            }
        }
        return true;
    }
}

class OrderStockAndPackValidator extends OrderStockValidator {

    @Override
    public boolean isValid(Order order) {
        for (Item item : order.getItems()) {
            if (!item.isInStock() || !item.isPacked()) {
                throw new IllegalStateException("Order is not valid"); // нельзя
            }
        }
        return true;
    }
}
```

**Проблема:**

* базовый класс возвращает `true/false`
* подкласс бросает исключение
  → нарушен контракт → клиентский код ломается

#### 2. Классическая ошибка с наследованием (Bird / Penguin)

```java
class Bird {
    public void fly() {
        System.out.println("Flying");
    }
}

class Penguin extends Bird {
    @Override
    public void fly() {
        throw new UnsupportedOperationException(); // нельзя
    }
}
```

**Проблема:**

* `Penguin` нельзя корректно подставить вместо `Bird`
* нарушено поведение

3. Усиление предусловий

```java
class Rectangle {
    public void setWidth(int width) {
        this.width = width;
    }
}

class PositiveRectangle extends Rectangle {
    @Override
    public void setWidth(int width) {
        if (width <= 0) {
            throw new IllegalArgumentException(); // нельзя усилили условие
        }
        super.setWidth(width);
    }
}
```

**Проблема:**

* базовый класс принимает любые значения
* подкласс ограничивает → нарушение LSP

4. Проверки типов (instanceof / getClass)

```java
class ShapeService {

    public void draw(Shape shape) {
        if (shape instanceof Circle) {
            System.out.println("Draw circle");
        } else if (shape instanceof Rectangle) {
            System.out.println("Draw rectangle");
        }
    }
}
```

**Проблема:**

* код зависит от конкретных типов
* при добавлении нового класса → нужно менять код
  → нарушение LSP и OCP

#### 5. Проблема с Generics

```java
List<Circle> circles = new ArrayList<>();
// List<Shape> shapes = circles; // нельзя

// если бы было можно:
shapes.add(new Rectangle()); // сломало бы список circles
```

**Суть:**

* `List<Circle>` не является `List<Shape>`
* иначе нарушилась бы типобезопасность

Как распознать нарушение

Если:

* появляется `instanceof`
* меняется ожидаемое поведение метода
* подкласс «ведёт себя странно»
* приходится писать адаптацию под тип

скорее всего нарушен LSP


**Предусловия** (preconditions) - это что должно быть выполнено ДО вызова метода (какие требования к входным данным)

**Постусловия** (postconditions) это что метод гарантирует ПОСЛЕ выполнения (Какой результат обещаешь)

Наследник может:
+ ослаблять предусловия
+ усиливать постусловия

```java
// Нарушение LSP (усиление предусловия)
class Bird {
    void fly(int speed) {
        // можно любое значение
    }
}

class FastBird extends Bird {
    @Override
    void fly(int speed) {
        if (speed < 100) {
            throw new IllegalArgumentException();
        }
    }
}
```

```java
// Ослабление предусловия (нормально)
class Bird {
    void fly(int speed) {
        if (speed <= 0) throw new IllegalArgumentException();
    }
}

class SafeBird extends Bird {
    @Override
    void fly(int speed) {
        if (speed < 0) speed = 0;
    }
}
```

```java
// Ослабление постусловия (нарушение)
class Payment {
    boolean pay() {
        return true; // гарантирует успех
    }
}

class UnstablePayment extends Payment {
    @Override
    boolean pay() {
        return false; // уже не гарантирует
    }
}
```
```java
// Усиление постусловия (нормально)
class Payment {
    boolean pay() {
        return true;
    }
}

class LoggedPayment extends Payment {
    @Override
    boolean pay() {
        log();
        return true;
    }
}
```
Но НЕ может:
+ усиливать предусловия
+ ослаблять постусловия

LSP =
**Наследник должен полностью заменять базовый класс без сюрпризов**

### 15. Каковы последствия нарушения LSP?
* Неожиданное поведение программы
* Ошибки при подстановке подклассов
* Нарушение OCP (появляются `if/switch`)
* Усложнение поддержки кода

### 16. Что такое ISP?
**ISP (Interface Segregation Principle принцип разделения интерфейсов)** гласит:
клиенты не должны зависеть от методов, которые они не используют.

Проще:
лучше иметь несколько **узкоспециализированных интерфейсов**, чем один «толстый».

Основная идея

* интерфейсы должны быть **маленькими и целевыми**
* каждый интерфейс — под конкретный сценарий использования (client-specific)
* класс реализует только то, что ему действительно нужно

ISP =
Не заставляй класс реализовывать то, что ему не нужно
### 17. Расскажите о нарушениях принципа ISP.
Пример нарушения ISP

```java
interface Report {

    void generatePdf();

    void generateExcel();
}

class PdfReport implements Report {

    @Override
    public void generatePdf() {
        System.out.println("Generate PDF");
    }

    @Override
    public void generateExcel() {
        // не нужно
        throw new UnsupportedOperationException();
    }
}
```

**Проблема:**

* класс вынужден реализовывать лишний метод
* появляется `UnsupportedOperationException`
* явное нарушение ISP

Исправленный вариант

```java
interface PdfReport {
    void generatePdf();
}

interface ExcelReport {
    void generateExcel();
}

class PdfReportImpl implements PdfReport {

    @Override
    public void generatePdf() {
        System.out.println("Generate PDF");
    }
}
```

**Теперь:**

* класс реализует только нужный интерфейс
* нет лишних методов → соблюден ISP

Ещё пример нарушения (много обязанностей)

```java
interface Worker {

    void work();

    void eat();

    void sleep();
}

class Robot implements Worker {

    @Override
    public void work() {
        System.out.println("Working");
    }

    @Override
    public void eat() {
        throw new UnsupportedOperationException(); // нельзя
    }

    @Override
    public void sleep() {
        throw new UnsupportedOperationException(); // нельзя
    }
}
```
Исправление

```java
interface Workable {
    void work();
}

interface Eatable {
    void eat();
}

interface Sleepable {
    void sleep();
}

class Robot implements Workable {

    @Override
    public void work() {
        System.out.println("Working");
    }
}
```

Признаки нарушения ISP

* интерфейс содержит **слишком много методов**
* классы реализуют методы «пусто» или через `throw`
* разные клиенты используют **разные части интерфейса**
* интерфейс часто меняется из-за разных причин (нарушение SRP)
### 18. Каковы последствия нарушения ISP?

* Загрязнение кода лишними методами
* Повышенная связность (coupling)
* Сложность тестирования
* Рост количества ошибок (забыли реализовать, сделали заглушку и т.д.)

### 19. Что такое DIP?
**DIP (Dependency Inversion Principle принцип инверсии зависимостей)** гласит:

* **Модули верхнего уровня** не должны зависеть от модулей нижнего уровня
* **Оба должны зависеть от абстракций**
* **Абстракции не должны зависеть от деталей**
* **Детали должны зависеть от абстракций**

Простыми словами

Зависим не от конкретных классов, а от интерфейсов

Уровни модулей

* **Высокий уровень** — бизнес-логика (например: `OrderService`)
* **Низкий уровень** — детали реализации:

   * БД
   * API
   * UI
   * Email / логирование

Чем ближе к вводу/выводу — тем ниже уровень

Что считается зависимостью

* `new` (создание объектов)
* `import` конкретных классов
* использование конкретных типов в:

   * полях
   * параметрах
   * возвращаемых значениях

Пример нарушения DIP

```java
class OrderProcessor {

    public void process(Order order){

        MySQLOrderRepository repository = new MySQLOrderRepository(); // нельзя
        ConfirmationEmailSender mailSender = new ConfirmationEmailSender(); // нельзя

        if (order.isValid() && repository.save(order)) {
            mailSender.sendConfirmationEmail(order);
        }
    }
}
```

**Проблема:**

* бизнес-логика зависит от конкретных реализаций
* невозможно заменить БД или способ отправки без изменения класса

Исправленный вариант

```java 
interface OrderRepository {
    boolean save(Order order);
}

interface MailSender {
    void sendConfirmationEmail(Order order);
}

class OrderProcessor {

    private MailSender mailSender;
    private OrderRepository repository;

    public OrderProcessor(MailSender mailSender, OrderRepository repository) {
        this.mailSender = mailSender;
        this.repository = repository;
    }

    public void process(Order order){
        if (order.isValid() && repository.save(order)) {
            mailSender.sendConfirmationEmail(order);
        }
    }
}
```

**Теперь:**

* `OrderProcessor` зависит от абстракций
* реализации можно менять без изменения кода
  
DIP =
  **Зависим от интерфейсов, а не от реализаций**
### 20. Расскажите о нарушениях принципа DIP.
Частые нарушения DIP

1. new внутри бизнес-логики

```java
class NotificationService {

    public void send() {
        EmailSender sender = new EmailSender(); // нельзя
        sender.send("Hello");
    }
}
```

2. Использование конкретных типов в сигнатурах

```java
class UserService {

    public void saveUser(MySQLUserRepository repo) { // нельзя
        repo.save();
    }
}
```

3. Статические зависимости

```java
class ReportService {

    public void generate() {
        FileUtil.writeToFile("report"); // нельзя - жесткая зависимость
    }
}
```

Как реализуется DIP

* Интерфейсы / абстракции
* Dependency Injection (через конструктор — лучший вариант)
* Полиморфизм

Почему «плодят интерфейсы»

Даже если:

* вы не планируете менять БД (например, PostgreSQL)
* или не планируете менять сервис

 абстракции позволяют:

* изолировать бизнес-логику
* тестировать (mock/stub)
* отложить решения (архитектурная гибкость)

### 21. Каковы последствия нарушения DIP?

* Нельзя заменить реализацию без изменения кода
* Сильная связность (tight coupling)
* Сложность тестирования
* Нарушение OCP
### 22. Расскажите, что такое автоматизированное тестирование.
### Что такое автоматизированное тестирование

**Автоматизированное тестирование** — это процесс проверки программы с помощью **тестов, написанных в виде кода**, которые можно запускать многократно.

**Тест** — это программная проверка ожидаемого поведения системы:

* сравнивает фактический результат с ожидаемым
* автоматически сообщает об ошибке при несовпадении

Противопоставление: ручное тестирование

**Ручное тестирование** — это когда:

* тест выполняет человек
* сам проверяет результат
* сам принимает решение «работает / не работает»

Пример:

* запускаете `main`
* смотрите вывод в консоль
* делаете вывод «всё ок» или «есть ошибка»

Пример автоматизированного теста (Java, JUnit)

```java
import org.junit.jupiter.api.Test;
import static org.junit.jupiter.api.Assertions.*;

class CalculatorTest {

    @Test
    void shouldAddTwoNumbers() {
        Calculator calc = new Calculator();

        int result = calc.add(2, 3);

        assertEquals(5, result); // проверка
    }
}
```

Преимущества автоматизированного тестирования

* **Повторяемость** — тесты можно запускать сколько угодно раз
* **Скорость** — быстрее ручной проверки
* **Надёжность** — исключает человеческий фактор
* **Регрессия** — позволяет проверять, что старый функционал не сломался
* **Интеграция с CI/CD** — автозапуск при каждом изменении кода

Виды автоматизированных тестов

* **Unit-тесты** — проверяют отдельные классы/методы
* **Интеграционные** — проверяют взаимодействие компонентов
* **E2E (end-to-end)** — проверяют систему целиком

Когда использовать

* для проверки бизнес-логики
* при частых изменениях кода
* для предотвращения регрессии

### 23. Как в Java осуществляется автоматизированное тестирование?
В Java автоматизированное тестирование обычно выполняется с помощью фреймворков, например:

* **JUnit** — для написания и запуска тестов
* **Mockito** — для создания заглушек (mock-объектов)

Принцип AAA (Arrange – Act – Assert)

Все тесты строятся по шаблону **AAA**:

1. **Arrange (подготовка)**

   * задаём входные данные
   * настраиваем окружение

2. **Act (действие)**

   * вызываем тестируемый метод

3. **Assert (проверка)**

   * сравниваем фактический результат с ожидаемым

Пример теста (JUnit)

```java
import org.junit.jupiter.api.Test;
import static org.junit.jupiter.api.Assertions.*;

class CalculatorTest {

    @Test
    void add2To1ThenSumIs3() {
        // Arrange
        int a = 2;
        int b = 1;
        int expected = 3;
        Calculator calculator = new Calculator();

        // Act
        int sum = calculator.sum(a, b);

        // Assert
        assertEquals(expected, sum);
    }
}
```
Что происходит при запуске теста

* JUnit находит методы с аннотацией `@Test`
* запускает их автоматически
* если `assert` проходит → тест **успешен**
* если нет → тест **падает (fail)**

Основные элементы теста

* `@Test` — помечает тестовый метод
* `assertEquals`, `assertTrue`, `assertThrows` — проверки
* тесты изолированы друг от друга

Пример с Mockito (моки)

```java
import static org.mockito.Mockito.*;

class UserServiceTest {

    @Test
    void shouldCallRepository() {
        // Arrange
        UserRepository repo = mock(UserRepository.class);
        UserService service = new UserService(repo);

        // Act
        service.saveUser("Alex");

        // Assert
        verify(repo).save("Alex");
    }
}
```

Зачем нужны моки

* изолируют тестируемый класс
* убирают зависимости (БД, сеть и т.д.)
* делают тесты быстрыми и предсказуемыми

### 24. Что такое JUnit? Как использовать ее для тестирования?
**JUnit** — это фреймворк для автоматизированного тестирования в Java,
который используется для проверки отдельных частей программы:

* методов
* классов
* компонентов

Каждый тест — это **отдельный метод в тестовом классе**

Основные возможности JUnit

* автоматический запуск тестов
* проверки через `assert`-методы
* изоляция тестов
* отчёт о результатах (passed / failed)

 Пример простого теста (JUnit 5)

```java 
import org.junit.jupiter.api.Test;
import static org.junit.jupiter.api.Assertions.*;

class CalculatorTest {

    @Test
    void shouldAddNumbers() {
        // Arrange
        Calculator calc = new Calculator();

        // Act
        int result = calc.add(2, 3);

        // Assert
        assertEquals(5, result);
    }
}
```
Основные аннотации

* `@Test` — тестовый метод
* `@BeforeEach` — выполняется перед каждым тестом
* `@AfterEach` — после каждого теста
* `@BeforeAll` / `@AfterAll` — до/после всех тестов

Assertions (проверки)

* `assertEquals(expected, actual)`
* `assertTrue(condition)`
* `assertFalse(condition)`
* `assertThrows(Exception.class, ...)`

Hamcrest (матчеры)

**Hamcrest** — библиотека для более гибких и читаемых проверок.

```java
import static org.hamcrest.MatcherAssert.assertThat;
import static org.hamcrest.Matchers.containsString;

@Test
void withHamcrest() {
    assertThat("job4j", containsString("4"));
}
```

Сравнение:

```java
assertTrue("job4j".contains("4")); // менее читаемо
```

Mockito (моки)

**Mockito** — библиотека для создания **заглушек (mock-объектов)**

```java
import static org.mockito.Mockito.*;

@Test
void testWithMock() {
    UserRepository repo = mock(UserRepository.class);

    UserService service = new UserService(repo);
    service.saveUser("Alex");

    verify(repo).save("Alex");
}
```

Подключение через Maven

JUnit 5:

```xml
<dependency>
    <groupId>org.junit.jupiter</groupId>
    <artifactId>junit-jupiter</artifactId>
    <version>5.10.0</version>
    <scope>test</scope>
</dependency>
```

Hamcrest:

```xml
<dependency>
    <groupId>org.hamcrest</groupId>
    <artifactId>hamcrest</artifactId>
    <version>2.2</version>
    <scope>test</scope>
</dependency>
```

Mockito:

```xml
<dependency>
    <groupId>org.mockito</groupId>
    <artifactId>mockito-core</artifactId>
    <version>5.8.0</version>
    <scope>test</scope>
</dependency>
```

`<scope>test</scope>` означает:

* используется только для тестов
* не попадает в итоговый `.jar`

Как запускаются тесты

* из IDE (IntelliJ IDEA, Eclipse)
* через Maven: `mvn test`
* через CI/CD

### 25. Что такое функциональное тестирование и чем отличается оно от модульного?

**Функциональное тестирование** — это проверка того, что система выполняет свои функции в соответствии с требованиями.

Система рассматривается как **«чёрный ящик»**:

* не важно, как она устроена внутри
* важно, что она принимает и что возвращает

Проверяется:

* бизнес-логика
* сценарии пользователя
* соответствие требованиям

---
Что такое модульное (unit) тестирование

**Модульное тестирование (Unit testing)** — это проверка отдельных частей программы:

* методов
* классов
* небольших компонентов

Тестируется изолированно, часто с использованием mock-объектов

---
Виды тестирования по «ящикам»

* **Белый ящик (White-box)**

   * тестирование с учётом внутренней реализации
   * пример: unit-тесты

* **Чёрный ящик (Black-box)**

   * без знания внутренностей системы
   * пример: функциональные тесты

* **Серый ящик (Gray-box)**

   * частичное знание системы
   * пример: интеграционные / регрессионные тесты

---

Основное отличие

| Критерий           | Unit-тестирование      | Функциональное тестирование  |
| ------------------ | ---------------------- | ---------------------------- |
| Уровень            | Низкий (метод / класс) | Высокий (функционал системы) |
| Подход             | Белый ящик             | Чёрный ящик                  |
| Зависимости        | Изолируются (моки)     | Реальные компоненты          |
| Скорость           | Очень быстро           | Медленнее                    |
| Кто пишет          | Разработчики           | QA / разработчики            |
| Цель               | Проверка логики кода   | Проверка требований          |
| Когда используются | Во время разработки    | После / вместе с разработкой |

---

Примеры

Unit-тест

```java
@Test
void shouldAddNumbers() {
    Calculator calc = new Calculator();
    assertEquals(5, calc.add(2, 3));
}
```

Проверяется один метод

---

#### Функциональный тест

```java
@Test
void shouldCreateOrderSuccessfully() {
    OrderService service = new OrderService(realRepo, realPayment);

    Order order = service.createOrder(user, items);

    assertTrue(order.isCreated());
}
```
Проверяется целый сценарий:

* создание заказа
* работа с БД
* бизнес-логика

Важный момент

* **Функциональные тесты** → показывают, что система работает для пользователя
* **Unit-тесты** → помогают разработчику быстро находить ошибки

Они **не заменяют**, а **дополняют друг друга**

### 26. Расскажите про методологию TDD.

**TDD (разработка через тестирование)** — это методология разработки, при которой:

**сначала пишется тест**, а уже потом код, который этот тест проходит.

Основной цикл TDD (Red → Green → Refactor)

1. **Red (красный)**

   * пишем тест на новую функциональность
   * тест падает (функциональности ещё нет)

2. **Green (зелёный)**

   * пишем минимальный код, чтобы тест прошёл

3. **Refactor (рефакторинг)**

   * улучшаем код, не ломая тесты
   * 
Пример

Шаг 1: пишем тест (Red)

```java 
@Test
void shouldAddNumbers() {
    Calculator calc = new Calculator();
    assertEquals(5, calc.add(2, 3));
}
```

Тест не компилируется / падает — метода ещё нет

Шаг 2: минимальная реализация (Green)

```java
class Calculator {
    int add(int a, int b) {
        return a + b;
    }
}
```

Тест проходит

Шаг 3: рефакторинг (Refactor)

```java 
class Calculator {
    int add(int a, int b) {
        return a + b; // можно улучшать код, не ломая тест
    }
}
```

Основные принципы TDD

* писать **минимальный код**
* тесты — это **спецификация поведения**
* код всегда покрыт тестами
* изменения безопасны благодаря тестам

Преимущества

*  меньше багов
*  безопасный рефакторинг
*  лучшее проектирование (вынуждает думать об API)
*  высокая тестируемость кода

Недостатки

*  больше времени на старте
*  требует опыта
*  не всегда удобно для:

   * UI
   * сложных интеграций

Когда использовать

* бизнес-логика
* сложные алгоритмы
* проекты с высокой критичностью

TDD =
**сначала тест → потом код → потом улучшение**

### 27. Расскажите про методологию BDD.

**BDD (разработка через поведение)** — это методология разработки,
которая является расширением **TDD** и фокусируется на **описании поведения системы с точки зрения бизнеса**.

Главная идея:
объединить разработчиков, тестировщиков и бизнес через **единый понятный язык**.

Основная концепция

В BDD:

* сначала описывается **поведение системы**
* затем пишутся тесты
* потом реализуется код

В отличие от TDD:

* TDD → «как работает код»
* BDD → «что делает система для пользователя»

---

Язык BDD (Given – When – Then)

Сценарии пишутся в виде:

* **Given (Дано)** — начальное состояние
* **When (Когда)** — действие
* **Then (Тогда)** — ожидаемый результат

---

Пример сценария

```gherkin
Feature: Создание заказа

Scenario: Успешное создание заказа
  Given пользователь авторизован
  When пользователь создает заказ
  Then заказ успешно создается
```


Пример user story

```
Как пользователь
Я хочу создать заказ
Чтобы купить товары
```

Пример теста (псевдокод / Java стиль)

```java
@Test
void shouldCreateOrderWhenUserIsAuthorized() {
    // Given
    User user = authorizedUser();

    // When
    Order order = orderService.createOrder(user);

    // Then
    assertTrue(order.isCreated());
}
```

Основные принципы BDD

* описание поведения на **понятном языке**
* фокус на **бизнес-требованиях**
* тесты как **живая документация**
* участие не только разработчиков, но и:

   * аналитиков
   * QA
   * менеджеров


Что отвечает BDD

* что тестировать?
* какие сценарии важны?
* как система должна вести себя?
* как понять причину падения теста?

Отличие BDD от TDD

| Критерий | TDD         | BDD                         |
| -------- | ----------- | --------------------------- |
| Фокус    | Код         | Поведение системы           |
| Уровень  | Unit        | Функциональный / acceptance |
| Язык     | Технический | Понятный бизнесу            |
| Подход   | Test → Code | Behavior → Test → Code      |


Популярные инструменты

* **Cucumber** (Java, Gherkin)
* **JBehave**
* **SpecFlow** (C#)

Преимущества

*  Общее понимание между бизнесом и разработкой
*  Живая документация
*  Фокус на реальных сценариях
*  Повышение качества требований

Недостатки

*  требует времени на описание сценариев
*  нужен процесс и дисциплина
*  сложнее внедрять, чем TDD

BDD =
**описываем поведение системы на языке бизнеса → пишем тесты → реализуем код**

### 28. Что такое тестирование черным, белым, серым ящиком?
Тестирование «чёрным ящиком» (Black-box)

**Определение:**
тестирование без знания внутренней реализации системы.

Система рассматривается как **«чёрный ящик»**:

* известно, что подаём на вход
* известно, что ожидаем на выходе
* **не важно**, как это реализовано внутри

Что проверяется

* корректность функциональности
* соответствие требованиям
* интерфейсы (API, UI)
* производительность и поведение

Типичные ошибки

* отсутствующие функции
* неверная реализация требований
* ошибки взаимодействия (интерфейсы, БД)
* проблемы производительности

Пример

```java
int result = calculator.add(2, 3);
assertEquals(5, result);
```

Нам не важно, как реализован `add()`

Тестирование «белым ящиком» (White-box)

**Определение:**
тестирование с полным знанием внутренней структуры кода.

Мы:

* знаем реализацию
* проверяем конкретные ветки, условия, алгоритмы

Что проверяется

* покрытие кода (if, циклы, ветвления)
* внутренняя логика
* граничные случаи

Пример

```java 
if (a > 0) {
    return a;
} else {
    return -a;
}
```

Тесты:

* `a > 0`
* `a <= 0`

проверяем **все ветки кода**

Тестирование «серым ящиком» (Gray-box)

**Определение:**
комбинация чёрного и белого ящика.

Мы:

* частично знаем внутреннюю реализацию
* используем это знание при тестировании

Пример

* знаем, что используется БД
* тестируем API, но учитываем:

   * индексы
   * кэш
   * структуру данных

Сравнение

| Критерий       | Чёрный ящик       | Белый ящик        | Серый ящик        |
| -------------- | ----------------- | ----------------- | ----------------- |
| Знание кода    | Нет               | Полное            | Частичное         |
| Фокус          | Поведение системы | Внутренняя логика | И то, и другое    |
| Кто использует | QA, аналитики     | Разработчики      | QA + разработчики |
| Уровень        | Функциональный    | Unit              | Интеграционный    |

Когда использовать

* **Чёрный ящик** → проверка требований (функциональные тесты)
* **Белый ящик** → проверка кода (unit-тесты)
* **Серый ящик** → сложные сценарии (интеграция, регрессия)

Коротко
*  Black-box → *что делает система*
*  White-box → *как работает код*
*  Gray-box → *и что, и как*

### 29. Опишите типы тестов: модульное, интеграционное, функциональное, приемочное?.
Модульное тестирование (Unit testing)

**Определение:**
тестирование **отдельного модуля** (метода или класса) в изоляции.

Особенности

* тестируется минимальная единица кода
* зависимости заменяются:

   * **Mock** — проверка поведения
   * **Stub** — возврат нужных данных
* нет работы с:

   * БД
   * сетью
   * файлами

Пример

```java id="unit_ex"
@Test
void shouldValidatePhone() {
    assertTrue(phoneValidator.isValid("+380991234567"));
}
```

Кто пишет

* разработчики

---

Интеграционное тестирование (Integration testing)

**Определение:**
проверка **взаимодействия между модулями** и внешними системами.

Особенности

* тестируется связка компонентов
* используются реальные зависимости:

   * БД
   * API
   * файловая система
* проверяется корректность интеграции

Пример

```java
@Test
void shouldSaveOrderToDatabase() {
    Order order = new Order();
    orderRepository.save(order);

    assertNotNull(orderRepository.findById(order.getId()));
}
```
Кто пишет

* разработчики (иногда QA)

---

Функциональное тестирование (Functional testing)

**Определение:**
проверка функциональности системы по требованиям
(система как **чёрный ящик**).

Особенности

* тестируются пользовательские сценарии
* не важно, как устроена система внутри
* проверяется бизнес-логика

Виды

* **Позитивное тестирование**

   * корректные данные → ожидаемый результат

* **Негативное тестирование**

   * некорректные данные → обработка ошибок

Пример

```java 
@Test
void shouldCreateOrder() {
    Order order = orderService.createOrder(user, items);
    assertTrue(order.isCreated());
}
```

Кто пишет

* QA + разработчики

---

Приемочное тестирование (Acceptance testing)

**Определение:**
проверка, что система **соответствует бизнес-требованиям** и может быть принята заказчиком.

Особенности

* проводится на основе:

   * user stories
   * критериев приемки
* отвечает на вопрос:
   «Готов ли продукт к использованию?»

Пример (BDD стиль)

```gherkin id="acc_ex"
Scenario: Успешное создание заказа
  Given пользователь авторизован
  When пользователь создает заказ
  Then заказ успешно создается
```
Кто выполняет

* QA
* заказчик / бизнес
* иногда автоматизировано

---
Общее сравнение

| Тип теста   | Что проверяет            | Уровень       | Зависимости  |
| ----------- | ------------------------ | ------------- | ------------ |
| Unit        | Логику метода/класса     | Низкий        | Моки / стабы |
| Integration | Связь компонентов        | Средний       | Реальные     |
| Functional  | Поведение системы        | Высокий       | Реальные     |
| Acceptance  | Соответствие требованиям | Очень высокий | Реальные     |

Коротко

* **Unit** → один класс
* **Integration** → связь модулей
* **Functional** → бизнес-функции
* **Acceptance** → готов ли продукт