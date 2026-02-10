## *Stream*

- [Что такое lambda-выражение?](#1-что-такое-lambda-выражение)
- [Что такое функциональные интерфейсы?](#2-что-такое-функциональные-интерфейсы)
- [Перечислите функциональные интерфейсы из пакета java.util.function.](#3-перечислите-функциональные-интерфейсы-из-пакета-javautilfunction)
- [Что такое функции высшего порядка?](#4-что-такое-функции-высшего-порядка)
- [Какие функциональные интерфейсы из пакета java.util.function поддерживают функции высшего порядка?](#5-какие-функциональные-интерфейсы-из-пакета-javautilfunction-поддерживают-функции-высшего-порядка)
- [Что такое ссылки на методы?](#6-что-такое-ссылки-на-методы)
- [Что такое ссылки на конструкторы?](#7-что-такое-ссылки-на-конструкторы)
- [Расскажите о зоне видимости переменных в lambda - выражениях?](#8-расскажите-о-зоне-видимости-переменных-в-lambda---выражениях)
- [Как быть в ситуации, если внутри lambda - выражения операторы могут выкинуть исключение?](#9-как-быть-в-ситуации-если-внутри-lambda---выражения-операторы-могут-выкинуть-исключение)
- [Что такое Stream API?](#10-что-такое-stream-api)
- [Расскажите, какие шаблоны проектирования используются внутри Stream API? (Builder, Strategy, Decorator, Factory Method, Pipeline).](#11-расскажите-какие-шаблоны-проектирования-используются-внутри-stream-api-builder-strategy-decorator-factory-method-pipeline)
- [Объясните, где они используются в Stream API.](#12-объясните-где-они-используются-в-stream-api)
- [Что такое конвейерные и терминальные операции? ](#13-что-такое-конвейерные-и-терминальные-операции)
- [Перечислите конвейерные (промежуточные) методы Stream API.](#14-перечислите-конвейерные-промежуточные-методы-stream-api)
- [Перечислите терминальные методы Stream API.](#15-перечислите-терминальные-методы-stream-api)
- [Что такое отложенный выполнение lamdba?](#16-что-такое-отложенный-выполнение-lamdba)
- [Что делает метод filter()?](#17-что-делает-метод-filter)
- [Что делает метод map()?](#18-что-делает-метод-map)
- [Что делает метод flatMap()?](#19-что-делает-метод-flatmap)
- [Что делает метод collect?](#20-что-делает-метод-collect)
- [Что делает метод findFirst?](#21-что-делает-метод-findfirst)
- [Что делает метод reduce?](#22-что-делает-метод-reduce)
- [Что делают методы min и max?](#23-что-делают-методы-min-и-max)
- [Что делают методы count, sum, average?](#24-что-делают-методы-count-sum-average)
- [Что делают методы forEach и peek?](#25-что-делают-методы-foreach-и-peek)
- [Что делают методы skip и limit?](#26-что-делают-методы-skip-и-limit)
- [Что делают методы allMatch(), noneMatch() и anyMatch()?](#27-что-делают-методы-allmatch-nonematch-и-anymatch)
- [Что делают методы mapToInt(), flatMapToInt(), mapToObj()?](#28-что-делают-методы-maptoint-flatmaptoint-maptoobj)
- [Что такое числовой поток?](#29-что-такое-числовой-поток)
- [Чем отличается Stream<Integer> от IntStream<int>?](#30-чем-отличается-streaminteger-от-intstreamint)
- [Что делает метод boxed?](#31-что-делает-метод-boxed)
- [Возможно ли прервать выполнение потока по аналогии с break?](#32-возможно-ли-прервать-выполнение-потока-по-аналогии-с-break)
- [Возможно ли пропустить элемент потока по аналогии с continue?](#33-возможно-ли-пропустить-элемент-потока-по-аналогии-с-continue)
- [Что такое Optional?](#34-что-такое-optional)
- [Перечислите методы Optional.](#35-перечислите-методы-optional)
- [В чем разница между методами orElse() и orElseGet()?](#36-в-чем-разница-между-методами-orelse-и-orelseget)
- [Расскажите про фабричные методы List.of, Set.of, Map.of?](#37-расскажите-про-фабричные-методы-listof-setof-mapof)
- [Для чего используется var?](#38-для-чего-используется-var)
- [В каких случаях можно использовать var?	](#39-в-каких-случаях-можно-использовать-var)

---

### 1. Что такое lambda-выражение?
Лямбда-выражение представляет собой блок кода, который можно передать в другое место, поэтому он может быть выполнен позже один или несколько раз. По сути это функция, которая существует, но в данный момент времени не может быть вычислена.
(компактная форма записи анонимной функции, которая используется для реализации единственного абстрактного метода функционального интерфейса)

По существу является анонимным (безымянным) методом, который реализует метод, определенный в функциональном интерфейсе.

```java
(входящие параметры через запятую без указания типа) -> {
    операторы;
    return вычисленное значение;
}
```
Если функция не принимает параметры, то указываем пустые скобки. Если не возвращает, то не указываем return.
### 2. Что такое функциональные интерфейсы?
Функциональным называется интерфейс, который содержит только 1 абстрактный метод. Такой интерфейс описыват классическую математическую функцию.

Основное назначение – использование в лямбда выражениях, method reference или ссылок на конструкторы. Функциональный интерфейс может содержать так же default и static методы. К функциональному интерфейсу можно добавить аннотацию @FunctionalInterface

```java
@FunctionalInterface
interface Calculator {
    int sum(int a, int b);

    default void print() {
        System.out.println("Calculator");
    }

    static void info() {
        System.out.println("Static method");
    }
}
```
### 3. Перечислите функциональные интерфейсы из пакета java.util.function.
Основные интерфейсы

| Интерфейс           | Описание                                                                        | Метод            |
| ------------------- |---------------------------------------------------------------------------------|------------------|
| `Function<T,R>`     | Представляет функцию перехода от объекта типа T к объекту типа R                | `R apply(T t)`            |
| `Predicate<T>`      | Проверяет соблюдение некоторого условия true/false                              | `boolean test(T t)` |
| `Consumer<T>`       | Выполняет некоторые действия над объектом типа Т, при этом ничего не возвращает | `void accept(T t)`     |
| `Supplier<T>`       | Не принимает аргументов, возвращая значение типа Т                              | `T get()`     |
| `UnaryOperator<T>`  | Принимает в качестве параметра объект типа T, выполняет над ними операции и возвращает результат операций в виде объекта типа T (Function)| `T apply(T t)`            |
| `BinaryOperator<T>` | Принимает в качестве параметра два объекта типа T, выполняет над ними бинарную операцию и возвращает ее результат также в виде объекта типа T (BiFunction)Лямба выражение - это объект анонимного класса ?| `T apply(T t1, T t2)`        |

Bi-версии кроме Supplier

| Интерфейс           |
| ------------------- |
| `BiFunction<T,U,R>` |
| `BiPredicate<T,U>`  |
| `BiConsumer<T,U>`   |

Помимо основных есть обёртки Predicate - IntPredicate/LongPredicate/DoublePredicate, Consumer - IntConsumer/LongConsumer/DoubleConsumer ... Всего 6 основных, BI версиями - 3, с обёртками - 44.
### 4. Что такое функции высшего порядка?
Функции высшего порядка - это функции, зависящие от других функций. (которые либо принимают другие функции в качестве аргумента, либо возвращают функцию в качестве результата, либо делают и то, и другое)

```java
y = ln(x) + x;

public static void main(String[] args) {
        // Функция высшего порядка
        Function<Integer, Integer> square = x -> x * x;
        int result = applyFunction(square, 5);
        System.out.println(result); // Результат: 25
    }

    public static <T, R> R applyFunction(Function<T, R> f, T value) {
        return f.apply(value);
    }
```

### 5. Какие функциональные интерфейсы из пакета java.util.function поддерживают функции высшего порядка?
Функциональные интерфейсы, поддерживающие функции высшего порядка:
1.	Function<T, R> — для преобразования данных.
2.	BiFunction<T, U, R> — для работы с двумя аргументами.
3.	Supplier<T> — для генерации значений (в том числе функций).
4.	Consumer<T> — для последовательного выполнения операций (andThen).
5.	Predicate<T> — для объединения условий (and, or).
6.	UnaryOperator<T> — для упрощённого преобразования одного типа.
7.	BinaryOperator<T> — для операций над двумя значениями одного типа.

```java
public static void main(String[] args) {
        Function<Integer, Integer> square = x -> x * x;
        Function<Integer, Integer> increment = x -> x + 1;

        // Композиция функций: (x + 1)^2
        Function<Integer, Integer> composed = square.compose(increment);
        System.out.println(composed.apply(3)); // Результат: 16
    }

BiFunction<Integer, Integer, Integer> add = (a, b) -> a + b;
Function<Integer, Integer> square = x -> x * x;

// Комбинируем BiFunction и Function
Function<Integer, Integer> result = add.andThen(square);
System.out.println(result.apply(2, 3)); // Результат: 25

public static void main(String[] args) {
        Supplier<Function<Integer, Integer>> supplier = () -> x -> x * x;
        Function<Integer, Integer> square = supplier.get();
        System.out.println(square.apply(5)); // Результат: 25
    }

public static void main(String[] args) {
        Consumer<String> print = System.out::println;
        Consumer<String> shout = s -> System.out.println(s.toUpperCase());

        // Выполняем два действия
        Consumer<String> combined = print.andThen(shout);
        combined.accept("hello"); // Результат: hello \n HELLO
    }

public static void main(String[] args) {
        Predicate<Integer> isEven = x -> x % 2 == 0;
        Predicate<Integer> isPositive = x -> x > 0;

        // Проверяем, что число положительное и четное
        Predicate<Integer> isPositiveEven = isEven.and(isPositive);
        System.out.println(isPositiveEven.test(4)); // Результат: true
    }
UnaryOperator<Integer> square = x -> x * x;
UnaryOperator<Integer> increment = x -> x + 1;
// Комбинируем функции
UnaryOperator<Integer> combined = square.andThen(increment);
System.out.println(combined.apply(3)); // Результат: 10
```
### 6. Что такое ссылки на методы?
Ссылка на метод - это компактное лямбда-выражение, которое позволяет передавать ссылки на методы или конструкторы.

Ссылка на метод передается в виде:

    имя_класса::имя_статического_метода ссылка на статический метод ClassName::staticMethod.

для нестатических методов:

    объект_класса::имя_метода на метода конкретного объекта (экземпляра) instance::instanceMethod.
```java
public static void main(String[] args) {
        String message = "Hello, ";
        Consumer<String> printer = System.out::println;
        
        // Вызов метода с использованием ссылки
        printer.accept(message + "world!"); // Результат: Hello, world!
    }
```

    тип_объекта::имя_метода на метод произвольного объекта конкретного типа ClassName::instanceMethod
    название_класса::new ссылка на конструктор ClassName::new.
### 7. Что такое ссылки на конструкторы?
Можно в качестве параметров использовать конструкторы:

    название_класса::new, для дженериков название_класса<T>::new.

При использовании конструкторов методы функциональных интерфейсов должны принимать тот же список параметров, что и конструкторы класса, и должны возвращать объект данного класса.

### 8. Расскажите о зоне видимости переменных в lambda - выражениях?
Лямбда-выражения имеют доступ к переменным в области видимости, в которой их определили. Но доступ возможен только при условии, что переменные являются effective final, то есть либо явно имеют модификатор final, либо не меняют своего значения после инициализации (константы). Если переменной присваивается значение во 2й раз, то лямбда-выражение вызовет ошибку компиляции.

Можно ссылаться на:
+	effective final локальные переменные;
+	статические переменные;
+	доступ к аргументам метода
+	доступ ко всем полям и методам внешнего класса, даже если изменяют их.
Доступ к локальным переменным

```java
public class Main {
    public static void main(String[] args) {
        String localVariable = "Hello"; // Переменная эффективно финальна

        Consumer<String> lambda = s -> System.out.println(localVariable + ", " + s);
        lambda.accept("world!"); // Результат: Hello, world!

        // Если попытаться изменить localVariable, произойдёт ошибка компиляции:
        // localVariable = "Goodbye"; // Ошибка: переменная должна быть эффективно финальной
    }
}
```

Доступ к полям внешнего класса
```java
public class Main {
    private String message = "Hello";

    public void printMessage() {
        Runnable lambda = () -> System.out.println(message);
        lambda.run(); // Результат: Hello

        // Изменение поля допустимо
        message = "Goodbye";
        lambda.run(); // Результат: Goodbye
    }

    public static void main(String[] args) {
        new Main().printMessage();
    }
}
```
Доступ к параметрам метода

```java
public class Main {
    public static void main(String[] args) {
        Main obj = new Main();
        obj.processMessage("Hello");
    }

    public void processMessage(String param) {
        Function<String, String> lambda = s -> param + ", " + s;
        System.out.println(lambda.apply("world!")); // Результат: Hello, world!

        // Нельзя изменить значение param после его использования в лямбда-выражении:
        // param = "Goodbye"; // Ошибка компиляции
    }
}
```

Объявление переменных внутри лямбды
```java
public class Main {
    public static void main(String[] args) {
        Function<Integer, Integer> lambda = x -> {
            int square = x * x; // Локальная переменная внутри лямбды
            return square;
        };

        System.out.println(lambda.apply(5)); // Результат: 25
    }
}
```

Если переменные с одинаковыми именами объявлены в лямбде и в её внешней зоне видимости, лямбда использует локальную переменную.
```java
public static void main(String[] args) {
        String message = "Outer";

        Runnable lambda = () -> {
            String message = "Inner"; // Локальная переменная
            System.out.println(message); // Результат: Inner
        };

        lambda.run();
        System.out.println(message); // Результат: Outer
    }
```

Причина кроется в особенностях работы замыканий в Java:
+	Лямбда-выражения могут быть выполнены позже, чем вызов метода, в котором они определены. Если локальная переменная изменится после передачи её в лямбду, это может привести к несогласованным данным (связанно захватом переменной*).
+	Чтобы избежать таких ошибок, компилятор требует, чтобы локальные переменные, используемые в лямбдах, оставались неизменными после их инициализации.

### 9. Как быть в ситуации, если внутри lambda - выражения операторы могут выкинуть исключение?
Обернуть лямбду в try-catch
```java
public static void main(String[] args) {
        Consumer<String> consumer = s -> {
            try {
                System.out.println(Integer.parseInt(s)); // Может выбросить NumberFormatException
            } catch (NumberFormatException e) {
                System.err.println("Invalid number: " + s);
            }
        };

        consumer.accept("123"); // Результат: 123
        consumer.accept("abc"); // Результат: Invalid number: abc
    }
```

Создать вспомогательный метод
```java
public static void main(String[] args) {
        Consumer<String> consumer = wrapException(s -> System.out.println(Integer.parseInt(s)));

        consumer.accept("123"); // Результат: 123
        consumer.accept("abc"); // Результат: Исключение перехвачено: Invalid number: abc
    }

    private static <T> Consumer<T> wrapException(ConsumerWithException<T> consumer) {
        return t -> {
            try {
                consumer.accept(t);
            } catch (Exception e) {
                System.err.println("Исключение перехвачено: " + e.getMessage());
            }
        };
    }

    @FunctionalInterface
    interface ConsumerWithException<T> {
        void accept(T t) throws Exception;
    }
```

Преобразовать исключение в непроверяемое (UncheckedException)
```java
public static void main(String[] args) {
        Function<String, Integer> parseInt = wrapException(Integer::parseInt);

        System.out.println(parseInt.apply("123")); // Результат: 123
        System.out.println(parseInt.apply("abc")); // Выбросит RuntimeException
    }

    private static <T, R> Function<T, R> wrapException(FunctionWithException<T, R> function) {
        return t -> {
            try {
                return function.apply(t);
            } catch (Exception e) {
                throw new RuntimeException(e);
            }
        };
    }

    @FunctionalInterface
    interface FunctionWithException<T, R> {
        R apply(T t) throws Exception;
    }
```

Написать свои интерфейсы с поддержкой исключений
```java
@FunctionalInterface
public interface ThrowingFunction<T, R> {
    R apply(T t) throws Exception;

    static <T, R> Function<T, R> unchecked(ThrowingFunction<T, R> throwingFunction) {
        return t -> {
            try {
                return throwingFunction.apply(t);
            } catch (Exception e) {
                throw new RuntimeException(e);
            }
        };
    }
}
public static void main(String[] args) {
        Function<String, Integer> parseInt = ThrowingFunction.unchecked(Integer::parseInt);

        System.out.println(parseInt.apply("123")); // Результат: 123
        System.out.println(parseInt.apply("abc")); // Выбросит RuntimeException
    }
```
### 10. Что такое Stream API?
Вся основная функциональность данного API сосредоточена в пакете java.util.stream. Ключевым понятием в Stream API является поток данных. Поток представляет канал передачи данных из источника данных. Причем в качестве источника могут выступать как файлы, так и массивы и коллекции.

Все операции над потоками бывают 2х видов:
+	terminal терминальные или конечные операции возвращают конкретный результат (void или результат определенного типа).
+	intermediate промежуточные или конвейерные операции возвращают трансформированный поток.

Все потоки производят вычисления, в т.ч. в промежуточных операциях, только когда к ним применяется терминальная операция. Т.е. в данном случае применяется отложенное выполнение.

Потоки не могут быть использованы повторно - как только была вызвана конечная операция, поток закрывается.

Одной из отличительных черт Stream API является применение лямбда-выражений, которые позволяют значительно сократить запись выполняемых действий. Т.е. Stream API позволяет взаимодействовать с данными в функциональном стиле, представляя их в качестве конечного потока данных.

Промежуточные методы выполняются только при наличии терминального метода.

### 11. Расскажите, какие шаблоны проектирования используются внутри Stream API? (Builder, Strategy, Decorator, Factory Method,     Pipeline).
*Pipeline (конвейер)*

Промежуточные операции формируют цепочку, а выполнение начинается только при терминальной операцией.
```java
names.stream()
    .filter(...)
    .map(...)
    .collect(...)
```

Особенности:
+	lazy evaluation
+	операции не выполняются сразу
+	строится граф вычислений
+	терминальная операция запускает весь конвейер

*Builder (Строитель)*

Используется для пошагового построения Stream:
```java
Stream<String> stream = Stream.<String>builder()
    .add("one")
    .add("two")
    .build();
```
Так же применяется в Collector (Collector.of(..)).

*Strategy (Стратегия)*

Шаблон Strategy позволяет определять семейство алгоритмов, инкапсулировать их в отдельные классы и делать их взаимозаменяемыми.

Передача поведения через функциональные интерфейсы:
+	Comparator
+	Predicate
+	BinaryOperator
+	Function

```java
.sorted((a, b) -> b - a)
.reduce((a, b) -> a + b)
```
Алгоритм выбирается во время выполнения.

*Decorator (Декоратор)*

Динамически добавляет поведение объекту, оборачивая его в другие объекты.

Каждая промежуточная операция:
+	возвращает новый Stream
+	оборачивает предыдущий
```java
stream
   .filter(...)
   .map(...)
   .distinct(...)
```
Каждый шаг добавляет поведение, не изменяя оригинальный поток.

*Factory Method (Фабричный метод)*

Создание потоков скрыто за фабричными методами:

```java
Stream.of(...)
Arrays.stream(...)
Files.lines(...)
Stream.generate(...)
```
Источник потока абстрагирован от клиента.

| Паттерн        | Роль в Stream API                   |
| -------------- | ----------------------------------- |
| Pipeline       | Промежуточные операции формируют цепочку преобразований, терминальная операция инициирует выполнение.|
| Builder        | Для пошагового создания потоков (например, Stream.builder()) или для конфигурации Collector.|
| Strategy       | Передача поведения в виде функциональных интерфейсов (например, Comparator в sorted или BinaryOperator в reduce).|
| Decorator      | Промежуточные операции (map, filter, sorted) украшают поток, добавляя функциональность.|
| Factory Method | Методы Stream.of(), Arrays.stream(), Files.lines() позволяют создавать потоки из разных источников, абстрагируя процесс их создания.|

### 12. Объясните, где они используются в Stream API.
| Паттерн        | Роль в Stream API                   |
| -------------- | ----------------------------------- |
| Pipeline       | Организация последовательных операций над потоком: filter, map, collect.|
| Builder        | Создание потоков (Stream.builder()), конфигурация сборщиков (Collectors.toMap(), Collectors.groupingBy()).|
| Strategy       | Настройка поведения с помощью Comparator (sorted) или BinaryOperator (reduce).|
| Decorator      | Промежуточные операции (filter, map, sorted, distinct) добавляют функциональность, возвращая новый поток.|
| Factory Method | Методы создания потоков: Stream.of(), Arrays.stream(), Files.lines(), Stream.generate().|

### 13. Что такое конвейерные и терминальные операции?
Все операции над потоками бывают 2х видов:
+	terminal терминальные или конечные операции возвращают конкретный результат -завершают поток, так же инициируют выполнения потока (void или результат определенного типа).
+	intermediate промежуточные или конвейерные операции возвращают трансформированный поток – создают цепочку преобразований, работают лениво.

Все потоки производят вычисления, в т.ч. в промежуточных операциях, только когда к ним применяется терминальная операция. Т.е. в данном случае применяется отложенное выполнение.

### 14. Перечислите конвейерные (промежуточные) методы Stream API.

| Метод | Описание | Пример |
|------|----------|--------|
| filter | Отфильтровывает записи, возвращает только записи, соответствующие условию | `collection.stream().filter("a1"::equals).count()` |
| skip | Позволяет пропустить N первых элементов | `collection.stream().skip(collection.size() - 1).findFirst().orElse("1")` |
| distinct | Возвращает стрим без дубликатов (по equals) | `collection.stream().distinct().collect(Collectors.toList())` |
| map | Преобразует каждый элемент стрима | `collection.stream().map(s -> s + "_1").collect(Collectors.toList())` |
| peek | Возвращает тот же стрим, но применяет функцию к каждому элементу | `collection.stream().map(String::toUpperCase).peek(e -> System.out.print("," + e)).collect(Collectors.toList())` |
| limit | Ограничивает количество элементов | `collection.stream().limit(2).collect(Collectors.toList())` |
| sorted | Сортирует элементы (natural order или Comparator) | `collection.stream().sorted().collect(Collectors.toList())` |
| mapToInt / mapToDouble / mapToLong | Аналог map, но возвращает числовой стрим | `collection.stream().mapToInt(Integer::parseInt).toArray()` |
| flatMap / flatMapToInt / flatMapToDouble / flatMapToLong | Из одного элемента может создать несколько | `collection.stream().flatMap(p -> Arrays.stream(p.split(","))).toArray(String[]::new)` |

### 15. Перечислите терминальные методы Stream API.

| Метод | Описание | Пример |
|------|----------|--------|
| findFirst | Возвращает первый элемент стрима (Optional) | `collection.stream().findFirst().orElse("1")` |
| findAny | Возвращает любой элемент стрима (Optional) | `collection.stream().findAny().orElse("1")` |
| collect | Представляет результат в виде коллекций или других структур | `collection.stream().filter(s -> s.contains("1")).collect(Collectors.toList())` |
| count | Возвращает количество элементов | `collection.stream().filter("a1"::equals).count()` |
| anyMatch | true, если условие выполняется хотя бы для одного элемента | `collection.stream().anyMatch("a1"::equals)` |
| noneMatch | true, если условие не выполняется ни для одного элемента | `collection.stream().noneMatch("a8"::equals)` |
| allMatch | true, если условие выполняется для всех элементов | `collection.stream().allMatch(s -> s.contains("1"))` |
| min | Возвращает минимальный элемент (Comparator) | `collection.stream().min(String::compareTo).get()` |
| max | Возвращает максимальный элемент (Comparator) | `collection.stream().max(String::compareTo).get()` |
| forEach | Применяет действие к каждому элементу (порядок не гарантирован) | `set.stream().forEach(p -> p.append("_1"))` |
| forEachOrdered | Применяет действие с сохранением порядка | `list.stream().forEachOrdered(p -> p.append("_new"))` |
| toArray | Возвращает массив элементов стрима | `collection.stream().map(String::toUpperCase).toArray(String[]::new)` |
| reduce | Агрегирует элементы и возвращает один результат | `collection.stream().reduce((s1, s2) -> s1 + s2).orElse(0)` |

### 16. Что такое отложенный выполнение lamdba?
Свойство отложенного выполнения. Выполнятся будет тогда, когда к нему идёт обращение.

### 17. Что делает метод filter()?
Метод ```filter(Predicate<T>)```(промежуточный-запускается при терминальной операции) используется для фильтрации элементов потока. Он принимает на вход предикат (функцию, возвращающую boolean) и пропускает только те элементы, для которых предикат возвращает true.
```java
List<Integer> evenNumbers = numbers.stream()
    .filter(n -> n % 2 == 0)
    .collect(Collectors.toList());
```

### 18. Что делает метод map()?
Метод ```map(Function<T, R>)``` используется для преобразования (маппинга) элементов потока из одного типа в другой. Он применяет указанную функцию к каждому элементу потока и возвращает новый поток, содержащий результаты этих преобразований.
```java
List<String> uppercasedWords = words.stream()
    .map(String::toUpperCase)
    .collect(Collectors.toList());
```

### 19. Что делает метод flatMap()?
```flatMap``` промежуточная операция. Плоское отображение похоже на map, но может создавать из одного элемента несколько. Т.е. каждый объект будет преобразован в 0, 1 или несколько других объектов, поддерживаемых потоком.

```Function<? super T, ? extends Stream<? extends R>> ```— функция, которая принимает элемент типа T и возвращает поток элементов типа R.

```java
List<String> words = sentences.stream()
    .flatMap(sentence -> Arrays.stream(sentence.split(" ")))
    .collect(Collectors.toList());
```

Отличие flatMap() от map():
1.	map():
      +	Преобразует элемент в другой тип или структуру, но поток остаётся неизменным.
      +	Если элемент преобразуется в поток, результатом будет поток потоков (Stream<Stream<R>>).
2.	flatMap():
      +	Преобразует элемент в поток и "разворачивает" полученные потоки в один поток.
      +	Результатом будет единый поток элементов (Stream<R>).
      
### 20. Что делает метод collect?
collect терминальная операция, преобразует поток в коллекцию.
+ Использует Collector<T, A, R>, где:
  + T — тип элементов в потоке.
  + A — тип промежуточной структуры (аккумулятора).
  + R — тип результата агрегации.
  
 ```java
List<String> collectedList = names.stream()
    .collect(Collectors.toList());
    
Set<String> collectedSet = names.stream()
    .collect(Collectors.toSet());

String result = names.stream()
    .collect(Collectors.joining(", "));

Map<String, Integer> nameLengthMap = names.stream()
    .collect(Collectors.toMap(
        name -> name,            // Ключ: само имя
        name -> name.length()    // Значение: длина имени
    ));

IntSummaryStatistics stats = numbers.stream()
    .collect(Collectors.summarizingInt(Integer::intValue));

System.out.println(stats);
// IntSummaryStatistics{count=5, sum=15, min=1, average=3.000000, max=5}
```
1.	Сбор в коллекции:
      +	Collectors.toList(): сбор в список (List).
      +	Collectors.toSet(): сбор в множество (Set).
      +	Collectors.toMap(): сбор в карту (Map).
2.	Сводные операции:
      +	Collectors.counting(): подсчёт количества элементов.
      +	Collectors.summingInt(): суммирование числовых элементов.
      +	Collectors.averagingDouble(): вычисление среднего значения.
3.	Объединение:
      +	Collectors.joining(): объединение элементов в строку.
4.	Пользовательские Collector:
      +	Можно создавать свои Collector для сложных преобразований.
### 21. Что делает метод findFirst?
```findFirst```  терминальная операция, возвращает первый элемент из стрима в виде обертки ```Optional<T>```.
```java
Optional<String> firstName = names.stream()
    .findFirst();

    // когда требуется вернуть не обёртку
  String firstName = names.stream()
    .findFirst()
    .orElse("No names");
```

### 22. Что делает метод reduce?

```reduce``` позволяет выполнять агрегатные функции на всей коллекцией и возвращать один результат. Т.е. позволяет преобразовать все элементы стрима в один объект. Например, посчитать сумму всех элементов.
1.	Без начального значения:
    + ```Optional<T> reduce(BinaryOperator<T> accumulator)```
      + BinaryOperator<T> — функция, которая объединяет два элемента потока в одно значение.
      + Возвращает Optional<T>: пустой, если поток пуст.
2.	С начальным значением:
    + ```T reduce(T identity, BinaryOperator<T> accumulator)```
      + identity — начальное значение (например, 0 для суммы или 1 для произведения).
      + Возвращает результат типа T.
3.	С функцией преобразования:

+ ```<U> U reduce(U identity, BiFunction<U, ? super T, U> accumulator, BinaryOperator<U> combiner)```
+ Используется в параллельных потоках для промежуточного объединения результатов.

```java
int sum = numbers.stream()
    .reduce(0, Integer::sum); // Эквивалентно (a, b) -> a + b

    System.out.println(sum); // 15
```

Объяснение:
+	0 — начальное значение.
+	```Integer::sum``` — операция, складывающая два элемента.

```java
List<Integer> numbers = List.of(1, 2, 3, 4);

int product = numbers.stream()
    .reduce(1, (a, b) -> a * b);
```
Объяснение:
+	Начальное значение 1.
+	Аккумулятор умножает элементы потока.
```java
Optional<Integer> max = numbers.stream()
    .reduce(Integer::max);

String concatenated = words.stream()
    .reduce("", (a, b) -> a + b);
```
Объяснение:
+	Начальное значение — пустая строка "".
+	Аккумулятор объединяет строки.
```java
int sum = numbers.parallelStream()
    .reduce(0, 
        Integer::sum,  // Аккумулятор
        Integer::sum); // Комбайнер

    System.out.println(sum); // 15
```
Объяснение:
+ Первый параметр (0) — начальное значение.
+ Второй — функция аккумуляции.
+ Третий — функция объединения частичных результатов.

Ассоциативность:
+ Операция должна быть ассоциативной, т.е. (a + b) + c должно быть эквивалентно a + (b + c).

Начальное значение:
+	Если начальное значение указано, поток не может быть пустым (начальное значение возвращается).
+	Если не указано, результат может быть пустым Optional.

Параллельные потоки:
+ В параллельных потоках метод с тремя параметрами обеспечивает корректное объединение.


### 23. Что делают методы min и max?
Нахождение минимального и максимального значения в потоке, в качестве сравнения используется компаратор, возвращает Optional.
```java
Optional<Person> oldest = people.stream()
    .max(Comparator.comparingInt(person -> person.age));
```
### 24. Что делают методы count, sum, average?
Методы ```count```, ```sum```, и ```average``` в Stream API предоставляют простой способ подсчёта количества элементов, вычисления суммы и среднего значения в потоке.
```count()``` подсчитывает количество элементов в потоке
```java
long count = names.stream().count();
```
```sum()``` возвращает сумму всех элементов в потоке. Этот метод доступен только для специализированных потоков: IntStream, LongStream, и DoubleStream.
```java
int total = IntStream.of(1, 2, 3, 4).sum();
```
```average()``` вычисляет среднее значение всех элементов в потоке. Этот метод также доступен только для IntStream, LongStream, и DoubleStream.
```java
OptionalDouble avg = IntStream.of(1, 2, 3, 4).average();
```

### 25. Что делают методы forEach и peek?
| Характеристика | forEach() | peek() |
|---------------|-----------|--------|
| Тип метода | Терминальный | Промежуточный |
| Возвращаемое значение | void | Stream с теми же элементами |
| Цель | Выполнить действие и завершить поток | Выполнить действие без завершения потока |
| Применение | Финальные действия | Отладка / наблюдение |
| Гарантия порядка | В упорядоченных потоках сохраняется (forEachOrdered — гарантированно) | Зависит от потока |

```java
List<String> upperNames = names.stream()
            .peek(s -> System.out.println("Original: " + s)) // Для отладки
            .map(String::toUpperCase)
            .peek(s -> System.out.println("Uppercase: " + s)) // Для отладки
            .collect(Collectors.toList()); // Терминальная операция
			
 // forEach — терминальный метод, завершает поток
        names.stream()
             .map(String::toUpperCase)  // Преобразуем в верхний регистр
             .forEach(System.out::println); // Вывод каждого элемента
```

### 26. Что делают методы skip и limit?
```java
            names.stream()
    .skip(2)
    .forEach(System.out::println);
    
    names.stream()
    .limit(3)
    .forEach(System.out::println);
```

Эти методы часто используются вместе для реализации постраничного вывода данных (пагинации).
```java
names.stream()
    .skip((pageNumber - 1) * pageSize)
    .limit(pageSize)
    .forEach(System.out::println);
```


| Метод | Тип | Что делает |
|------|-----|------------|
| skip | Промежуточный | Пропускает первые *n* элементов потока |
| limit | Промежуточный | Оставляет только первые *n* элементов потока |


| Характеристика | skip() | limit() |
|---------------|--------|---------|
| Назначение | Пропустить заданное количество элементов | Ограничить поток заданным количеством элементов |
| Влияние на поток | Убирает первые *n* элементов | Убирает все элементы после первых *n* |
| Тип метода | Промежуточный | Промежуточный |
| Пустой поток | Возвращает пустой поток | Возвращает пустой поток |

### 27. Что делают методы allMatch(), noneMatch() и anyMatch()?

| Метод | Возвращает true если... | Возвращает false если... |
|------|--------------------------|--------------------------|
| allMatch() | Все элементы удовлетворяют предикату | Хотя бы один элемент не удовлетворяет предикату или поток пуст |
| noneMatch() | Ни один элемент не удовлетворяет предикату | Хотя бы один элемент удовлетворяет предикату |
| anyMatch() | Хотя бы один элемент удовлетворяет предикату | Все элементы не удовлетворяют предикату или поток пуст |

Все три метода останавливают обработку, как только результат может быть определён.
+ allMatch() завершает проверку, если найдёт первый элемент, который не удовлетворяет условию.
+ noneMatch() завершает проверку, если найдёт первый элемент, который удовлетворяет условию.
+ anyMatch() завершает проверку, как только найдёт первый элемент, удовлетворяющий условию.
1.	allMatch(): Используется для проверки, соответствуют ли все элементы потока условию.
2.	noneMatch(): Используется для проверки, что ни один элемент не удовлетворяет условию.
3.	anyMatch(): Используется для проверки, что хотя бы один элемент удовлетворяет условию.

### 28. Что делают методы mapToInt(), flatMapToInt(), mapToObj()?

| Метод | Цель | Тип возвращаемого потока |
|------|------|--------------------------|
| mapToInt() | Преобразует поток объектов в поток примитивов `int` | IntStream |
| flatMapToInt() | Преобразует каждый объект в `IntStream` и объединяет их | IntStream |
| mapToObj() | Преобразует поток примитивов (например, `IntStream`) в поток объектов | Stream<R> |

```java
List<Double> numbers = List.of(1.5, 2.3, 3.8);
IntStream roundedNumbers = numbers.stream()
   .mapToInt(Math::round);
```
Разделение строк на символы

```java
List<String> words = List.of("cat", "dog");
IntStream charStream = words.stream()
    .flatMapToInt(String::chars);
charStream.forEach(ch -> System.out.print((char) ch + " ")); // Вывод: c a t d o g
```

Преобразование чисел в строки
```java
IntStream numbers = IntStream.range(1, 4);
Stream<String> numberStrings = numbers.mapToObj(num -> "Value: " + num);
numberStrings.forEach(System.out::println); 
```
### 29. Что такое числовой поток?
Числовой поток — это специализированный поток, предназначенный для работы с примитивными типами данных, такими как int, long, и double. Числовые потоки предоставляют более производительный способ обработки чисел, так как избегают автоупаковки (boxing) и распаковки (unboxing) примитивов в объекты-оболочки.

В Stream API есть три типа числовых потоков:
1.	IntStream — для работы с примитивами int.
2.	LongStream — для работы с примитивами long.
3.	DoubleStream — для работы с примитивами double.

| Метод | Описание |
|------|----------|
| sum() | Возвращает сумму всех элементов потока |
| average() | Возвращает среднее арифметическое элементов потока (Optional) |
| min() | Возвращает минимальное значение (Optional) |
| max() | Возвращает максимальное значение (Optional) |
| count() | Возвращает количество элементов в потоке |
| reduce() | Производит свёртку элементов потока с использованием заданной операции |
| map() | Преобразует элементы потока, возвращая новый числовой поток |
| filter() | Фильтрует элементы по заданному условию |
| boxed() | Преобразует числовой поток в поток объектов-оболочек (`Stream<Integer>`, `Stream<Long>`, `Stream<Double>`) |
### 30. Чем отличается Stream<Integer> от IntStream<int>?
Поток обёрток и поток примитивов.

| Характеристика | Stream<Integer> | IntStream |
|---------------|-----------------|-----------|
| Тип данных | Поток объектов-оболочек `Integer` | Поток примитивов `int` |
| Производительность | Ниже из-за boxing / unboxing | Выше — работает напрямую с примитивами |
| Память | Требует больше памяти (объекты) | Использует меньше памяти |
| Специализированные методы | Нет числовых методов (`sum()`, `average()` и т.д.) | Есть `sum()`, `min()`, `max()`, `average()` |
| Преобразование | `mapToInt()` | `boxed()` |
| Гибкость | Универсальный, может содержать `null` | `null` не допускается |
### 31. Что делает метод boxed?
Метод boxed() используется для преобразования числового потока примитивов (IntStream, LongStream, или DoubleStream) в поток объектов-оболочек (Stream<Integer>, Stream<Long>, или Stream<Double>).

Метод полезен, если нужно:
1.	Использовать числовой поток в контексте, требующем объектов. Например, при работе с коллекциями (List, Set) или библиотеками, которые требуют Stream<T>, а не специализированного потока примитивов.
2.	Сохранить элементы числового потока в коллекцию. Методы вроде Collectors.toList() работают только с объектами, а не с примитивами.
3.	Использовать методы объектов-оболочек. Например, при использовании методов Integer или Double.
```java
List<Integer> list = intStream.boxed().toList();

	Stream<Integer> boxedStream = intStream.boxed();
	Stream<String> result = boxedStream.map(i -> "Value: " + i);
```
### 32. Возможно ли прервать выполнение потока по аналогии с break?
Java Stream API нету встроенной команды, аналогичной break, для явного завершения обработки элементов потока. Однако, есть несколько способов добиться аналогичного поведения:

```limit(size)```, ```skip()```, ```findFirst()``` или ```findAny()```, ```allMath()``` – ```anyMatch()``` – ```noneMatch()```, ```filter ()```, ```takeWhile()``` – возвращает элементы пока они удовлетворяют условие.

Или выкидывать исключение:
```java
try {
    Stream.of(1, 2, 3, 4, 5)
          .forEach(x -> {
              if (x == 3) throw new RuntimeException("Прерывание потока");
              System.out.println(x);
          });
} catch (RuntimeException e) {
    System.out.println("Поток прерван: " + e.getMessage());
}
```
### 33. Возможно ли пропустить элемент потока по аналогии с continue?
Метод ```filter(Predicate<? super T> predicate)``` позволяет пропускать элементы, которые не удовлетворяют условию. Это фактически эквивалентно использованию ```continue``` в цикле.

```java
Stream.of(1, 2, 3, 4, 5)
      .flatMap(x -> (x % 2 == 0) ? Stream.empty() : Stream.of(x)) // Пропускаем четные числа
      .forEach(System.out::println);
```
### 34. Что такое Optional?
Optional — это контейнерный класс в Java (пакет java.util), введенный в Java 8, который используется для обработки значений, которые могут быть null. Основная цель Optional — минимизировать использование null и избежать типичных ошибок, связанных с ним, таких как NullPointerException.

### 35. Перечислите методы Optional.
| Метод | Что делает |
|------|------------|
| empty() | Возвращает пустой Optional |
| of(T value) | Создаёт Optional с НЕ-null значением (иначе NPE) |
| ofNullable(T value) | Создаёт Optional, допускает null |
| get() | Возвращает значение или кидает NoSuchElementException |
| isPresent() | true, если значение есть |
| isEmpty() | true, если значения нет (Java 11+) |
| ifPresent(Consumer) | Выполняет действие, если значение есть |
| ifPresentOrElse(Consumer, Runnable) | Действие при наличии / отсутствии значения (Java 9+) |
| filter(Predicate) | Фильтрует значение по условию |
| map(Function) | Преобразует значение |
| flatMap(Function) | Преобразует значение, возвращая Optional |
| orElse(T other) | Возвращает значение или дефолт |
| orElseGet(Supplier) | Возвращает значение или результат Supplier |
| orElseThrow() | Кидает исключение, если пусто (Java 10+) |
| orElseThrow(Supplier) | Кидает пользовательское исключение |
| stream() | Преобразует Optional в Stream (0 или 1 элемент, Java 9+) |
### 36. В чем разница между методами orElse() и orElseGet()?
| Характеристика | orElse() | orElseGet() |
|---------------|---------|-------------|
| Аргумент | Готовое значение | Поставщик значения (`Supplier`) |
| Когда вычисляется аргумент | Всегда, независимо от наличия значения | Только если значение отсутствует |
| Поддержка ленивого выполнения | Нет | Да |
| Использование ресурсов | Может быть неэффективным при дорогих вычислениях | Эффективен, так как выполняется по необходимости |

Когда использовать orElse() или orElseGet()
1.	Используйте orElse(), если:
      +	Альтернативное значение уже доступно.
      +	Вычисление альтернативного значения дешевое и не требует дополнительных ресурсов.
2.	Используйте orElseGet(), если:
      +	Альтернативное значение нужно вычислять динамически.
      +	Вычисление альтернативного значения дорогостоящее (например, запрос к базе данных, сложные вычисления).
```java
Optional<String> opt = Optional.of("Hello");
String value = opt.orElse(getDefaultValue());
System.out.println(value); // "Hello"

Optional<String> emptyOpt = Optional.empty();
String defaultValue = emptyOpt.orElse(getDefaultValue());
System.out.println(defaultValue); // "Default"
Метод getDefaultValue():

private static String getDefaultValue() {
    System.out.println("Вычисление значения...");
    return "Default";
}

Optional<String> opt = Optional.of("Hello");
String value = opt.orElseGet(() -> getDefaultValue());
System.out.println(value); // "Hello"

Optional<String> emptyOpt = Optional.empty();
String defaultValue = emptyOpt.orElseGet(() -> getDefaultValue());
```

### 37. Расскажите про фабричные методы List.of, Set.of, Map.of?
Фабричные методы ```List.of```, ```Set.of```, ```Map.of``` (java 9) — это удобный способ быстро создавать неизменяемые коллекции. Они повышают читаемость кода и обеспечивают безопасность данных за счёт неизменяемости. Однако важно учитывать их ограничения, такие как невозможность содержать ```null``` и запрет на дубликаты (в случае ```Set``` и ключей в ```Map```).

### 38. Для чего используется var?
Дает возможность сократить объявления переменных используя ключевое слово ```var```. При обработке ```var```, компилятор просматривает правую часть объявления, так называемый инициализатор и использует его тип для переменной.

```var``` это зарезервированное имя, но не ключевое слово (наряду с ```true```, ```false```, и ```null```), что обеспечивает обратную совестимость программ, использующих его.
```java
var var = 10;
```
var нельзя использовать для:
+	полей класса;
+	параметров методов;
+	переменных без знания (т.е. без инициализатора).
также var:
+	не будет работать, если инициализируется null.
+	не будет работать для нелокальных переменных.
+	нельзя использовать в лямбда-выражениях, т.к. им нужен явный целевой тип.
+	нельзя использовать в случае инициализации массива.
### 39. В каких случаях можно использовать var?
Var может использоваться только в сочетании с данными, т.е. нужно обязательно инициализировать переменную. Компилятору нужно знать тип переменной, а тип можно извлечь только из значения.