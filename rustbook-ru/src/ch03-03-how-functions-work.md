## Как работают функции

Функция - это ключевые части Rust кода. Вы, конечно, знакомы с самой важной функцией. Это функция `main`, которая является точкой входа в программу. Также вы уже познакомились с ключевым словом `fn` - обозначающим начало объявления функции.

В Rust используется т.н.*"змеиный"* стиль написания функций и переменных: это когда все слова пишутся в нижнем регистре и слова в многословных обозначениях разделяются нижним подчёркиванием. Пример объявления функции:

<span class="filename">Файл: src/main.rs</span>

```rust
fn main() {
    println!("Hello, world!");

    another_function();
}

fn another_function() {
    println!("Another function.");
}
```

Определение функции начинается с `fn` и состоит из группы скобок после имени функции. Фигурные скобки указывают, где начинается и заканчивается тело функции.

Можно вызвать любую определённую функцию, введя её имя со скобками за ним. Так как `another_function` определена в программе, её можно вызвать из функции `main`. Заметьте, что мы определили `another_function` *после* функции `main` в исходном коде; мы также могли бы определить её до. Rust не волнует, где вы определяете свои функции, главное только то, что они где-то определены.

Создадим новый проект с названием *functions* для дальнейшего изучения функций. Поместите пример `another_function` в файл *src/main.rs* и запустите его. Вы должны увидеть следующий вывод:

```text
$ cargo run
   Compiling functions v0.1.0 (file:///projects/functions)
    Finished dev [unoptimized + debuginfo] target(s) in 0.28 secs
     Running `target/debug/functions`
Hello, world!
Another function.
```

Строчки кода выполняются в том порядке, в котором они появляются в функции `main`. Сначала печатается сообщение “Hello, world!”, а затем вызывается `another_function` и она также печатает сообщение.

### Параметры функции

Функции также можно определять с *параметрами*, которые являются специальными переменными и частью сигнатуры функции. Когда у функции есть параметры, то можно предоставлять ей конкретные значения этих параметров. Технически, конкретные параметры называются *аргументами*, но в обычном разговоре люди часто используют слово *параметр* и *аргумент* взаимозаменяемо, для переменной в описании функции или для конкретных значений аргументов при вызове функции.

Переписанная версия функции `another_function` показывает, как параметры выглядят в Rust:

<span class="filename">Файл: src/main.rs</span>

```rust
fn main() {
    another_function(5);
}

fn another_function(x: i32) {
    println!("The value of x is: {}", x);
}
```

Попробуйте запустить программу, вы должны получить следующий вывод:

```text
$ cargo run
   Compiling functions v0.1.0 (file:///projects/functions)
    Finished dev [unoptimized + debuginfo] target(s) in 1.21 secs
     Running `target/debug/functions`
The value of x is: 5
```

Объявление функции `another_function` имеет один параметр с именем`x`. Тип параметра `x` определён как `i32`. Когда значение `5` передаётся в функцию `another_function`, то макрос  `println!` помещает число `5` туда где пара фигурных скобок используется для форматирования строк.

В сигнатуре функции вы *должны* указать тип каждого параметра. Это осознанное решение в дизайне языка Rust: требование аннотации типов в определении функции означает, что компилятору почти никогда не нужно ничего использовать ещё откуда-то из кода для понимания, что вы имеете ввиду.

Когда у функции множество параметров, то они разделяются с помощью запятой, как тут:

<span class="filename">Файл: src/main.rs</span>

```rust
fn main() {
    another_function(5, 6);
}

fn another_function(x: i32, y: i32) {
    println!("The value of x is: {}", x);
    println!("The value of y is: {}", y);
}
```

Данный пример создаёт функцию с двумя параметрами, оба из которых имеют тип `i32`. Функция затем печатает значения обоих параметров. Заметьте, что параметры функции не должны быть все одного типа, просто так получилось в этом примере.

Попробуем запустить этот код. Замените программу из проекта  *functions* в файле *src/main.rs* следующим примером и запустите его через `cargo run`:

```text
$ cargo run
   Compiling functions v0.1.0 (file:///projects/functions)
    Finished dev [unoptimized + debuginfo] target(s) in 0.31 secs
     Running `target/debug/functions`
The value of x is: 5
The value of y is: 6
```

Так как мы вызвали функцию со значением `5` в параметре `x` и число `6` передано в `y`, две строки в функции напечатали эти два значения.

### Тело функции состоит из операторов и выражений

Тело функции состоит из серии операторов, которое опционально может заканчиваться выражением. До этого времени, мы ещё не рассмотрели функции с телом оканчивающимся выражением в конце, а только выражения как часть операторов в теле. По причине того, что Rust является языком на основе выражений, то это различие важно понимать. Другие языки могут не иметь такие различия, поэтому посмотрим что такое операции и выражения, и как различие между ними влияют на тело функций.

На самом деле мы уже использовали операторы и выражения. *Операторы (Statements)* - это инструкции, которые  выполняют действия, но не возвращают значение. *Выражения (Expressions)* вычисляются в результирующее значение, которое возвращается. Рассмотрим примеры.

Создадим переменную и присвоим ей значение с помощью ключевого слова `let`. В листинге 3-1 `let y = 6;` является оператором.

<span class="filename">Файл: src/main.rs</span>

```rust
fn main() {
    let y = 6;
}
```

<span class="caption">Листинг 3-1: Объявление функции <code>main</code> включающей один оператор</span>

Определение функции также является оператором. Весь предыдущий пример тоже является оператором.

Операции не возвращают значений. Тем не менее, нельзя назначить оператор `let` другой переменной, как это пытается сделать следующий коде. Вы получите ошибку:

<span class="filename">Файл: src/main.rs</span>

```rust,ignore,does_not_compile
fn main() {
    let x = (let y = 6);
}
```

Если вы запустите эту программу, то ошибка будет выглядеть так:

```text
$ cargo run
   Compiling functions v0.1.0 (file:///projects/functions)
error: expected expression, found statement (`let`)
 --> src/main.rs:2:14
  |
2 |     let x = (let y = 6);
  |              ^^^
  |
  = note: variable declaration using `let` is a statement
```

Оператор `let y = 6` не возвращает значения, так что нет ничего что можно назначить переменной `x`. Это отличается от некоторых других языков, типа C и Ruby, где выражение присваивания возвращает присваиваемое значение. В таких языках можно писать код `x = y = 6` и обе переменные `x` и `y` будут иметь одинаковое значение `6`; но это не так в Rust.

Выражения обычно во что-то вычисляются и составляют большую часть кода, который вы будете писать в Rust. Рассмотрим простую математическую операцию `5 + 6`, которая является выражением вычисляющим значение `11`. Выражения могут быть частью операторов: в листинге 3-1 число `6` в операторе `let y = 6;` является выражением, которое вычисляется в значение `6`. Вызов функции является выражением. Вызов макроса является выражением. Блок используемый для создания областей видимости `{}` также является выражением, например:

<span class="filename">Файл: src/main.rs</span>

```rust
fn main() {
    let x = 5;

    let y = {
        let x = 3;
        x + 1
    };

    println!("The value of y is: {}", y);
}
```

Это выражение:

```rust,ignore
{
    let x = 3;
    x + 1
}
```

является блоком, который в данном случае вычисляется в число `4`. Его значение назначается переменной `y` как часть оператора `let`. Заметьте, что строка `x + 1` не имеет точки с запятой в конце, что отличается от большинства строк, которые вы видели до сих пор. Выражения не включают завершающих их точки с запятой. Если её добавить в конце выражения, вы превратите его в оператор, который тогда не возвращает значение. Имейте это в виду, по мере дальнейшего изучения функций, возвращаемых значения и выражения.

### Функции возвращающие значения

Функции могут возвращать значения в вызывающий их код. Мы не именуем возвращаемые значения, но мы объявляем их тип после символа (`->`). В Rust, возвращаемое значение функции является синонимом значения последнего выражения в блоке тела функции. Можно выполнить ранний возврат из функции используя ключевое слово `return` и указав значение, но большинство функций явно возвращает последнее выражение в теле. Вот пример функции возвращающей значение:

<span class="filename">Файл: src/main.rs</span>

```rust
fn five() -> i32 {
    5
}

fn main() {
    let x = five();

    println!("The value of x is: {}", x);
}
```

Здесь нет вызовов функций, макросов или даже операторов  `let` в коде функции `five` - есть только одно число `5`. Это является абсолютно корректной функцией в Rust. Заметьте, что возвращаемый тип у данной функции определён как `-> i32`. Попробуйте запустить; вывод будет таким:

```text
$ cargo run
   Compiling functions v0.1.0 (file:///projects/functions)
    Finished dev [unoptimized + debuginfo] target(s) in 0.30 secs
     Running `target/debug/functions`
The value of x is: 5
```

Число `5` в функции `five` является возвращаемым значением функции, вот почему возвращаемым типом является `i32`. Рассмотрим пример более детально. Есть два важных момента: первый строка `let x = five();` показывает, что мы используем значение возвращаемое функцией для инициализации переменной. Так как функция `five` возвращает `5`, то эта строка эквивалентна следующей:

```rust
let x = 5;
```

Второй момент, функция `five` не имеет входных параметров и определяет тип возвращаемого значения, но само тело функции только `5` без точки с запятой, потому что является выражением, значение которого мы хотим вернуть из функции.

Рассмотрим другой пример:

<span class="filename">Файл: src/main.rs</span>

```rust
fn main() {
    let x = plus_one(5);

    println!("The value of x is: {}", x);
}

fn plus_one(x: i32) -> i32 {
    x + 1
}
```

Запуск кода выведет `The value of x is: 6`. Но если поместить точку с запятой в конец строки, включающей `x + 1`, то это изменит её с выражения на оператор и мы получим ошибку.

<span class="filename">Файл: src/main.rs</span>

```rust,ignore,does_not_compile
fn main() {
    let x = plus_one(5);

    println!("The value of x is: {}", x);
}

fn plus_one(x: i32) -> i32 {
    x + 1;
}
```

Компиляция данного кода вызывает следующую ошибку:

```text
error[E0308]: mismatched types
 --> src/main.rs:7:28
  |
7 |   fn plus_one(x: i32) -> i32 {
  |  ____________________________^
8 | |     x + 1;
  | |          - help: consider removing this semicolon
9 | | }
  | |_^ expected i32, found ()
  |
  = note: expected type `i32`
             found type `()`
```

Главное сообщение в ошибке, “mismatched types” раскрывает основную проблему этого кода. Определение функции `plus_one` говорит, что она вернёт тип `i32`, но теперь уже оператор в её теле не вычисляет значение, что отображено символами `()` в описании ошибки, который является пустым кортежем (empty tuple). Поэтому, здесь ничего не возвращается, что противоречит определению функции и приводит к ошибке. В данном выводе Rust  предлагает свою помощь в сообщении, чтобы исправить проблему. Он советует убрать точку с запятой, что должно исправить ошибку.
