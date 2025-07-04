## Объявление enum

````rust
enum Direction {
    Up,
    Down,
    Left,
    Right,
}

fn move_avatar(dir: Direction) {
    match dir {
        Direction::Up => println!("Идём вверх"),
        Direction::Down => println!("Идём вниз"),
        Direction::Left => println!("Идём влево"),
        Direction::Right => println!("Идём вправо"),
    }
}

fn main() {
    let d = Direction::Left;
    move_avatar(d);  // Идём влево
}
````

## Варианты с данными (enum с ассоциированными значениями)

````rust
enum Message {
    Quit,
    Move { x: i32, y: i32 },
    Write(String),
    ChangeColor(i32, i32, i32),
}

fn process(msg: Message) {
    match msg {
        Message::Quit => println!("Выход"),
        Message::Move { x, y } => println!("Двигаемся на {}, {}", x, y),
        Message::Write(text) => println!("Пишем: {}", text),
        Message::ChangeColor(r, g, b) => println!("Цвет изменён на {}, {}, {}", r, g, b),
    }
}
````

## Пример с опциональным значением

Rust использует `Option<T>` — enum для представления значения, которое может быть или `Some(value)`, или `None`.

````rust
fn main() {
    let x: Option<i32> = Some(10);

    match x {
        Some(v) => println!("Значение: {}", v),
        None => println!("Нет значения"),
    }
}
````

## Enum с методами

````rust
enum Status {
    Success,
    Error(String),
}

impl Status {
    fn is_ok(&self) -> bool {
        matches!(self, Status::Success)
    }
}

fn main() {
    let s = Status::Error(String::from("Что-то пошло не так"));
    println!("{}", s.is_ok());  // false
}
````

## Result<T, E> — для обработки ошибок

`Result<T, E>` — это enum, который используется для 
представления успешного (`Ok`) или ошибочного (`Err`) результата.

````rust
enum Result<T, E> {
    Ok(T),
    Err(E),
}
````
где:
- `T` — тип успешного значения
- `E` — тип ошибки

### Пример использования

````rust
fn divide(a: f64, b: f64) -> Result<f64, String> {
    if b == 0.0 {
        Err(String::from("Деление на ноль"))
    } else {
        Ok(a / b)
    }
}

fn main() {
    match divide(10.0, 2.0) {
        Ok(result) => println!("Результат: {}", result),
        Err(e) => println!("Ошибка: {}", e),
    }
}
````

### Методология ? — упрощённая обработка ошибок

````rust
fn parse_and_divide(a: &str, b: &str) -> Result<f64, String> {
    let num_a: f64 = a.parse().map_err(|_| "Неверный a")?;
    let num_b: f64 = b.parse().map_err(|_| "Неверный b")?;
    divide(num_a, num_b)
}
````
> `?` автоматически разворачивает `Result` или возвращает `Err` из функции.

### Методы Result

| Метод         | Назначение                                |
| ------------- | ----------------------------------------- |
| `.unwrap()`   | Получить значение или паникнуть           |
| `.expect()`   | То же, но с сообщением                    |
| `.is_ok()`    | Возвращает `true`, если `Ok`              |
| `.is_err()`   | Возвращает `true`, если `Err`             |
| `.map()`      | Преобразует `Ok`, оставляя `Err` как есть |
| `.and_then()` | "цепочка" действий, если `Ok`             |

## Generics

### Объявление функции с дженериком

````rust
fn print_twice<T: std::fmt::Debug>(value: T) {
    println!("{:?} {:?}", value, value);
}
````
- `T` — параметр типа.
- `T: Debug` — ограничение: тип должен реализовать трейт `Debug`.

### Объявление структуры с дженериком

````rust
struct Pair<T> {
    a: T,
    b: T,
}

impl<T> Pair<T> {
    fn new(a: T, b: T) -> Self {
        Pair { a, b }
    }
}
````

## Where и ограничения по traits

### Зачем нужен where

Когда ты используешь много ограничений на типы (`trait bounds`), сигнатура функции может стать трудно читаемой:
````rust
// Без where — всё в одной строке
fn print_and_compare<T: Debug + PartialOrd, U: Debug + PartialOrd>(a: T, b: U) {
    println!("{:?} и {:?}", a, b);
}
````

### ✅ Как использовать where

````rust
fn print_and_compare<T, U>(a: T, b: U)
where
    T: std::fmt::Debug + PartialOrd,
    U: std::fmt::Debug + PartialOrd,
{
    println!("{:?} и {:?}", a, b);

    if a.partial_cmp(&b).is_some() {
        println!("Можно сравнить!");
    } else {
        println!("Сравнение невозможно");
    }
}

````


