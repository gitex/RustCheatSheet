# Rust Cheet Sheet

## Установка и проект



## Основы синтаксиса

```rust
fn main() {
    println!("Hello, world!");
}
```

- `//` — однострочный комментарий
- `/* */` — многострочный комментарий

## Переменные и типы

```rust
let x = 5;           // неизменяемая переменная
let mut y = 10;      // изменяемая переменная
const PI: f64 = 3.14; // константа

let a: i32 = 42;
let b: f64 = 3.14;
let c: bool = true;
let d: char = '🦀';
let s: &str = "строка";
```

## Управляющие конструкции

```rust
if x > 0 {
    println!("Положительное");
} else {
    println!("Ноль или отрицательное");
}

for i in 0..5 {
    println!("{}", i);
}

while x < 10 {
    x += 1;
}

loop {
    break;
}
```

## Match

```rust
let dir = Direction::North;

match dir {
    Direction::North => println!("Север"),
    Direction::South => println!("Юг"),
    _ => println!("Другое направление"),
}
```

## Вектора, строки, кортежи

```rust
let mut v = vec![1, 2, 3];
v.push(4);

let s = String::from("hello");
let slice = &s[0..2];

let tup = (1, "hi", true);
let (x, y, z) = tup;
```

## Обработка ошибок

```rust
fn divide(a: f64, b: f64) -> Result<f64, String> {
    if b == 0.0 {
        Err("Деление на ноль".to_string())
    } else {
        Ok(a / b)
    }
}

let result = divide(4.0, 2.0);
match result {
    Ok(val) => println!("Результат: {}", val),
    Err(e) => println!("Ошибка: {}", e),
}
```

## Ownership, Borrowing, Lifetimes

```rust
fn print_length(s: &String) {
    println!("{}", s.len());
}

let s = String::from("hello");
print_length(&s);  // заимствование (borrow)
```

## Тесты

```rust
#[cfg(test)]
mod tests {
    use super::*;

    #[test]
    fn test_add() {
        assert_eq!(add(2, 3), 5);
    }
}
```

## Traits (Черты)

Traits — это интерфейсы в стиле Rust.

```rust
trait Greet {
    fn greet(&self) -> String;
}

struct Person {
    name: String,
}

impl Greet for Person {
    fn greet(&self) -> String {
        format!("Привет, {}!", self.name)
    }
}
```

## Modules и use

Модули позволяют организовать код по файлам и папкам.

```rust
// src/main.rs
mod utils;

fn main() {
    utils::hello();
}

// src/utils.rs
pub fn hello() {
    println!("Привет из модуля!");
}
```

- `mod` — объявляет модуль
- `pub` — делает функцию/структуру доступной снаружи
- `use` — импорт:

```rust
use crate::utils::hello;
```

## Async / Await (асинхронность)

```rust
use tokio::time::{sleep, Duration};

#[tokio::main]
async fn main() {
    say_hello().await;
}

async fn say_hello() {
    sleep(Duration::from_secs(1)).await;
    println!("Привет, async!");
}
```

Для async-кода часто используется tokio или async-std

В Cargo.toml:

```toml
[dependencies]
tokio = { version = "1", features = ["full"] }
```

# Итераторы

```rust
let numbers = vec![1, 2, 3, 4, 5];
let doubled: Vec<i32> = numbers.iter().map(|x| x * 2).collect();
```

## Методы итераторов
###  `.next()`

Возвращает следующий элемент или None, если элементы закончились

### `.collect()`

Преобразует итератор в коллекцию (Vec, HashMap, и т.д.)

### `.map()`

Преобразует каждый элемент

```rust
let doubled: Vec<i32> = vec![1, 2, 3].iter().map(|x| x * 2).collect();
```

### `.filter()`

Пропускает элементы, не проходящие условие

```rust
let even: Vec<i32> = vec![1,2,3,4].into_iter().filter(|x| x % 2 == 0).collect();
```

### `.enumerate()`

Оборачивает элементы в (index, item)

```rust
for (i, val) in vec.iter().enumerate() {
    println!("{}: {}", i, val);
}
```

### `.fold()`

Сводит элементы к одному значению

```rust
let sum = vec![1,2,3].iter().fold(0, |acc, x| acc + x);
```

### `.find()`

Находит первый подходящий элемент

```rust
let found = vec![1, 2, 3].iter().find(|&&x| x > 1); // Some(&2)
```

### `.all(), .any()`

Проверяют все/хотя бы один элемент

```rust
vec![1,2,3].iter().all(|x| *x > 0); // true
vec![1,2,3].iter().any(|x| *x == 2); // true
```

### `.take(n)`

Берёт первые `n` элементов

```rust
let first_two: Vec<_> = vec![1,2,3].into_iter().take(2).collect();
```

### `.skip(n)`

Пропускает первые `n` элементов

```rust
let rest: Vec<_> = vec![1,2,3].into_iter().skip(1).collect(); // [2, 3]
```

### `.zip(other)`

Объединяет два итератора

```rust
let a = [1, 2, 3];
let b = ["a", "b", "c"];
let zipped: Vec<_> = a.iter().zip(b.iter()).collect();
```

### `.chain(other)`

Соединяет два итератора

```rust
let joined: Vec<_> = [1,2].iter().chain([3,4].iter()).collect();
```


### `.flatten()`

«Разворачивает» вложенные итераторы

```rust
let nested = vec![vec![1,2], vec![3,4]];
let flat: Vec<_> = nested.into_iter().flatten().collect(); // [1,2,3,4]
```

### `.flat_map()`

Как `map` + `flatten`

```rust
let nums = vec![1, 2];
let result: Vec<_> = nums.into_iter().flat_map(|x| vec![x, x * 10]).collect(); // [1,10,2,20]
```

# Option и Result

## Option<T>

```rust
let maybe_number: Option<i32> = Some(10);

match maybe_number {
    Some(n) => println!("Число: {}", n),
    None => println!("Нет значения"),
}
```

## Result<T, E>

```rust
fn safe_div(a: i32, b: i32) -> Result<i32, &'static str> {
    if b == 0 {
        Err("Нельзя делить на ноль")
    } else {
        Ok(a / b)
    }
}
```

# Pattern Matching

```rust
let (x, y) = (10, 20);

match (x, y) {
    (0, _) => println!("x == 0"),
    (_, 0) => println!("y == 0"),
    _ => println!("Оба не нули"),
}
```

# Smart Pointers (Box, Rc, RefCell)

| Указатель              | Назначение                                | Изменяемость  | Поддержка потоков  | Когда использовать                         | Проверка времени            |
| ---------------------- | ----------------------------------------- | ------------- | ------------------ | ------------------------------------------ | --------------------------- |
| `Box<T>`               | Выделение в куче                          | ❌ (без `mut`) | ✅ (если `T: Send`) | Рекурсивные типы, перенос тяжёлых структур | Компиляция                  |
| `Rc<T>`                | Подсчёт ссылок (однопоточный)             | ❌             | 🚫                 | Разделение доступа без изменения           | Компиляция                  |
| `RefCell<T>`           | Внутренняя изменяемость                   | ✅             | 🚫                 | Изменение внутри `&self` в одном потоке    | **Выполнение**              |
| `Rc<RefCell<T>>`       | Разделение + изменяемость (однопоточный)  | ✅             | 🚫                 | Общий доступ с возможностью изменения      | Выполнение                  |
| `Arc<T>`               | Подсчёт ссылок (многопоточный)            | ❌             | ✅                  | Разделение между потоками                  | Компиляция                  |
| `Arc<Mutex<T>>`        | Разделение + изменяемость (многопоточный) | ✅             | ✅                  | Общие данные с безопасным изменением       | **Блокировка + выполнение** |
| `Cell<T>`              | Копируемые типы с изменяемостью           | ✅ (`Copy`)    | 🚫                 | Изменение примитивов в `&self`             | Выполнение                  |
| `Mutex<T>`             | Защита от одновременного доступа          | ✅             | ✅                  | Синхронизация данных между потоками        | Выполнение                  |
| `Ref<T>` / `RefMut<T>` | Возвращаются из `RefCell<T>`              | ✅ / ✅         | 🚫                 | Управление временем жизни ссылок           | Выполнение                  |
| `Weak<T>`              | Слабая ссылка на `Rc`/`Arc`               | ❌             | Зависит от обёртки | Избегать циклических ссылок                | Компиляция                  |

## `Box<T>`

```rust
let b = Box::new(5); // перемещает значение 5 в кучу (heap)
println!("b = {}", b); // автоматически разыменовывается — печатает 5

```
📌 Что происходит:

- `Box::new(5)` создаёт указатель на 5 в куче
- `b` владеет этим значением
- `Box` освобождает память при выходе из области видимости

Полезно в рекурсиях:
```rust
enum List {
    Cons(i32, Box<List>),
    Nil,
}
```

## `Rc<T>` (Reference Counter)

Reference Counter (Ref Count) — это механизм, который подсчитывает количество владельцев одного объекта в памяти. Как только последняя ссылка удаляется — объект освобождается автоматически.

Структура `Rc` примерно такая:
```rust
struct RcBox<T> {
    strong: Cell<usize>, // счётчик "сильных" ссылок
    value: T,            // значение
}
```
Пример:
```rust
let a = Rc::new(String::from("hello"));  // Создаётся `RcBox { strong: 1, value: "hello" }`
let b = Rc::clone(&a);  // `clone()` увеличивает `strong` до `2`
```

📦 Под капотом:
- Создаётся `RcBox { strong: 1, value: "hello" }`
- `clone()` увеличивает `strong` до `2`
- При `drop(b)` → `strong -= 1`
- Когда `strong == 0`, вызывается `drop(value)` и память освобождается

> Ссылка называется "сильной" потому, что сильная ссылка, в отличие от слабой - увеличивает счётчик.


```rust
use std::rc::Rc;

let a = Rc::new(String::from("hello")); // создаём строку в куче с подсчётом ссылок
let b = Rc::clone(&a); // увеличиваем счётчик ссылок (оба a и b владеют строкой)

println!("a = {}, b = {}", a, b);
println!("Rc count = {}", Rc::strong_count(&a)); // 2

```

📌 Что происходит:

- `a` создаёт объект с 1 владельцем
- `Rc::clone` делает поверхностное копирование — счётчик = 2
- Строка удалится, когда все владельцы выйдут из области видимости

> Полезно при построении графов или деревьев с разделяемыми узлами.

## `RefCell<T>` — Внутренняя изменяемость

```rust
use std::cell::RefCell;

let x = RefCell::new(5);        // создаём обёртку с возможностью изменять
*x.borrow_mut() += 1;           // получаем мутабельную ссылку и изменяем значение

println!("x = {}", x.borrow()); // borrow() даёт неизменяемую ссылку: 6
```

📌 Что происходит:
- `borrow_mut()` даёт временный доступ на изменение
- После этого `borrow()` можно получить только если нет активных `borrow_mut()`
- Проверки происходят **во время выполнения**, не компиляции

## `Rc<RefCell<T>>` — совместный доступ с изменяемостью

````rust
use std::rc::Rc;
use std::cell::RefCell;

let data = Rc::new(RefCell::new(42));     // оборачиваем значение в RefCell внутри Rc
let a = Rc::clone(&data);                 // увеличиваем счётчик Rc
*a.borrow_mut() += 1;                     // изменяем внутреннее значение

println!("data = {}", data.borrow());     // 43
````

📌 Что происходит:
- `Rc` управляет владением, `RefCell` даёт возможность временного изменяемого доступа
- Можно создать сколько угодно `Rc::clone`, но данные внутри — только через `borrow()` / `borrow_mut()`
- Полезно в структурах с несколькими владельцами, которые **хотят менять содержимое** (очень популярен в tree/graph структурах)


## `Arc<T>` — многопоточное разделение

```rust
use std::sync::Arc;

let shared = Arc::new(5);        // создаёт объект в куче с атомарным счётчиком ссылок
let t1 = Arc::clone(&shared);    // клонируем владение для другого потока

println!("t1 = {}, shared = {}", t1, shared); // оба видят одно и то же
```

📌 Что происходит:

- `Arc` (atomic Rc) безопасен для использования между потоками
- Внутреннее значение должно быть иммутабельным или защищено (`Mutex`), если нужно изменять

## Mutex<T> + Arc<T> — изменяемость в многопоточном коде

```rust
use std::sync::{Arc, Mutex};
use std::thread;

let counter = Arc::new(Mutex::new(0));      // оборачиваем значение в Mutex и Arc
let c = Arc::clone(&counter);               // клонируем владение

let handle = thread::spawn(move || {
    let mut num = c.lock().unwrap();        // получаем эксклюзивный доступ
    *num += 1;                              // изменяем значение (0 → 1)
});

handle.join().unwrap();
println!("counter = {:?}", counter.lock().unwrap()); // 1
```

📌 Что происходит:

- `Mutex::new(0)` создаёт значение, защищённое от одновременного доступа
- `.lock()` даёт доступ и блокирует другие потоки
- `Arc` делит владение между потоками
- После выхода потока из области видимости, `Mutex` разблокируется

## Cell<T> — Copy-типы с внутренней изменяемостью

```rust
use std::cell::Cell;

let c = Cell::new(10); // инициализация Cell с Copy-типом
c.set(20);             // перезапись значения
let x = c.get();       // получение текущего значения (20)

println!("x = {}", x);
```
📌 Что происходит:
- Работает только с типами, реализующими `Copy`
- Можно менять значение даже внутри `&self` (например, в `impl`)

## Как выбрать нужный Smart Pointer?

| Требуется                   | Используй                   |
| --------------------------- | --------------------------- |
| Хранить в куче              | `Box<T>`                    |
| Разделить между владельцами | `Rc<T>` или `Arc<T>`        |
| Внутренне изменять          | `RefCell<T>` или `Mutex<T>` |
| Потокобезопасность          | `Arc<T>` + `Mutex<T>`       |




# Macros

```rust
macro_rules! say_hello {
    () => {
        println!("Привет из макроса!");
    };
}

fn main() {
    say_hello!();
}
```

# Лайфтаймы (Lifetimes)

```rust
fn longest<'a>(a: &'a str, b: &'a str) -> &'a str {
    if a.len() > b.len() { a } else { b }
}
```

- `'a` — **параметр времени жизни**
- `&'a str` — строковая ссылка, живущая не меньше `'a`
- Возвращаемая ссылка также должна жить хотя бы `'a`

🧠 Это значит: результат живёт столько же, сколько самый короткий из входов.

## `'static` лайфтайм

```rust
static NAME: &str = "Rustacean"; // живёт всю программу

fn get_name() -> &'static str {
    "Forever" // строковый литерал живёт в статической памяти
}
```

## Когда лайфтаймы нужны?

### Функции с несколькими ссылками

```rust
fn combine<'a>(a: &'a str, b: &'a str) -> String {
    format!("{}{}", a, b)
}
```

### Структуры с полями-ссылками

Структура не может жить дольше, чем её ссылки.

```rust
struct Book<'a> {
    title: &'a str,
    author: &'a str,
}
```

Основные Smart Pointer’ы в Rust
Указатель	Назначение	Смена владельца	Многопоточность
Box<T>	Куча. Хранит один объект	✅ move	🚫
Rc<T>	Подсчёт ссылок (однопоточный)	✅ clone()	🚫
Arc<T>	Подсчёт ссылок (многопоточный)	✅ clone()	✅
RefCell<T>	Внутренняя изменяемость (однопоточная)	✅ через borrow()	🚫
Mutex<T>	Потокобезопасная внутренняя изменяемость	✅ через lock()	✅

