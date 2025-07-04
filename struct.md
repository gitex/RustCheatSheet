## Объявление структуры

````rust
struct Person {
    name: String,
    age: u32,
}

fn main() {
    let person = Person {
        name: String::from("Иван"),
        age: 30,
    };

    println!("Имя: {}, Возраст: {}", person.name, person.age);  // Имя: Иван, Возраст: 30

}
````

## Инициализация с помощью функции-конструктора


````rust
impl Person {
    fn new(name: &str, age: u32) -> Self {
        Person {
            name: String::from(name),
            age,
        }
    }
}

fn main() {
    let p = Person::new("Анна", 25);
    println!("{} - {}", p.name, p.age);
}
````

## Методы для структур

````rust
impl Person {
    fn greet(&self) {
        println!("Привет, меня зовут {}!", self.name);
    }
}

fn main() {
    let p = Person::new("Олег", 40);
    p.greet();
}
````

## Tuple-структуры

Структуры без имён полей, только с типами:

````rust
struct Color(u8, u8, u8);

fn main() {
    let black = Color(0, 0, 0);
    println!("Красный: {}", black.0);
}
````

## Unit-структуры

Пустые структуры без полей:

````rust
struct Marker;

fn main() {
    let _m = Marker;
}
````

## Обновление структур

````rust
let person2 = Person {
    age: 35,
    ..person1  // Часть полей будет заимствована из существующей структуры
};
````

