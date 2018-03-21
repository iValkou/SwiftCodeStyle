# Руководство по стилю кода Swift

Данное руководства является переводом [The Official raywenderlich.com Swift Style Guide](https://github.com/raywenderlich/swift-style-guide/) со значительными изменениями и адаптацией под собственные задачи.

Для Swift 4

## Содержание

* [Именование](#Именование)
    * [Язык](#Язык)
    * [Методы](#Методы)
    * [Префиксы классов](#Префиксы-классов)
    * [Делегаты](#Делегаты)
    * [Типонезависимый контекст](#Типонезависимый-контекст)
    * [Дженерики](#Дженерики)
* [Организация кода](#Организация-кода)
    * [Соответствие протоколу](#Соответствие-протоколу)
    * [Неиспользуемый код](#Неиспользуемый-код)
    * [Минимальный импорт](#Минимальный-импорт)
* [Отступы](#Отступы)
* [Комментарии](#Комментарии)
* [Классы и структуры](#Классы-и-структуры)
    * [Что использовать?](#Что-использовать)
    * [Пример определения](#Пример-определения)
    * [Использование Self](#Использование-self)
    * [Вычисляемые свойства](#Вычисляемые-свойства)
    * [Final](#final)
* [Объявление функций](#Объявление-функций)
* [Замыкания](#Замыкания)
* [Типы](#Типы)
    * [Константы](#Константы)
* [Статические методы и свойства типа](#Статические-методы-и-свойства-типа)
* [Опциональные типы](#Опциональные-типы)
* [Ленивая инициализация](#Ленивая-инициализация)
* [Приведение типов](#Приведение-типов)
    * [Аннотация типа для пустых массивов и словарей](#Аннотация-типа-для-пустых-массивов-и-словарей)
* [Синтаксический сахар](#Синтаксический-сахар)
* [Функции vs Методы](#Функции-vs-Методы)
* [Управление памятью](#Управление-памятью)
    * [Продление времени жизни объекта](#Продление-времени-жизни-объекта)
* [Управление доступом](#Управление-доступом)
* [Циклы](#Циклы)
* [Условные операторы](#Условные-операторы)
    * [Провал Guards](#Провал-guards)
* [Точка с запятой](#Точка-с-запятой)
* [Круглые скобки](#Круглые-скобки)
* [Экспорт в Objective-C](#Экспорт-в-objective-c)
* [Ссылки](#Ссылки)

## Основы

- Стремитесь скомпилировать свой **код без предупреждений** компилятора. Это правило информирует о многих решениях стиля, таких как использование типов ```#selector``` вместо строковых литералов.
- **Ясность в применении**. Дизайн программных интерфейсов должен быть ясным, лаконичным и понятным в контексте.
- **Ясность важнее краткости**. Хотя код Swift может быть компактным, нецелесообразно использовать наименьший возможный код с наименьшим количеством символов в ущерб ясности.


## Именование

Описательное и последовательное именование упрощает чтение и понимание программного кода. Используйте соглашения об именах Swift, описанные в [API Design Guidelines](https://swift.org/documentation/api-design-guidelines/). Вот некоторые положения:

- используйте ```camelСase``` (не *snake_case*)
- используйте ```UpperCamelCase``` в именах типов и протоколов, для всего остального используйте ```lowerCamelCase```
- включайте все необходимые слова, опускайте ненужные слова
- используйте имена на основе ролей, а не типов
- компенсируйте недостаточную информацию типа параметра в его имени
- стремитесь использовать грамматическое положение слов в именах
- начинайте имена фабрик со слова ```make```
- именуйте методы, учитывая их побочные эффекты
	- когда операция описывается глаголом, используйте суффиксы -```ed``` и ```-ing``` для не мутирующих аналогов методов: ```x.sort()``` и  ```z = x.sorted()```
	- когда операция описывается существительным, используйте существительное для метода без мутации и добавляйте префикс ```form```, чтобы назвать его мутирующую копию: ```x = y.union(z)``` и	```y.formUnion(z)```
	- методы, возвращающие ```Bool```, должны читаться как утверждения
	- протоколы, описывающие роль, должны читаться как существительные
	- протоколы, описывающие возможность, должны заканчиваться на ```-able``` или ```-ible```

- избегайте невразумительных терминов
- вообще избегайте сокращений 
- избегайте аббревиатур
- используйте прецедент для имён
- предпочитайте методы и свойства глобальным функциям
- давайте одинаковые базовые имена методам, которые имеют одинаковое значение
- избегайте перегрузок возвращаемого типа
- выбирайте хорошие имена параметров, чтобы они служили в качестве документации
- именуйте параметры замыканий и кортежей (tuples)
- используйте преимущества параметров по умолчанию

### Язык

Используйте американский английский, чтобы соответствовать языку Apple API.

### Методы

Критическим является однозначное описание действия методов. Используйте максимально простую форму именования метода.

1. Напишите имя метода без параметров. ***Пример***: вам нужно назвать метод ```addTarget```
2. Напишите имя метода с метками аргументов. ***Пример***: вам нужно назвать метод ```addTarget(_:action:)```
3. Напишите полное имя метода с метками и типами аргументов. ***Пример***: вам нужно назвать метод ```addTarget(_: Any?, action: Selector?)```.

### Префиксы классов

Типы Swift автоматически оборачиваются в пространство имен модуля, который их содержит, и вы не должны добавлять префикс класса, такой как MF. Если сталкиваются два имени из разных модулей, вы можете устранить неоднозначность, предварительно указав имя типа с именем модуля. Когда есть редкая вероятность путаницы, указывается только имя модуля.

```swift
import SomeModule

let myClass = SomeModule.UsefulClass()
```

**Примечание**. Допускается добавление префиксов к классам, экспортируемым в Objective-C код, для единообразия с используемым там API.

### Делегаты

При создании методов делегирования первый параметр должен быть источником делегата. (UIKit содержит многочисленные примеры.)

#### Рекомендуется

```swift
func namePickerView(_ namePickerView: NamePickerView, didSelectName name: String)
func namePickerViewShouldReload(_ namePickerView: NamePickerView) -> Bool
```

#### Не рекомендуется

```swift
func didSelectName(namePicker: NamePickerViewController, name: String)
func namePickerShouldReload() -> Bool
```

### Типонезависимый контекст

Чтобы писать более короткий и понятный код, используйте контекст компилятора.

#### Рекомендуется

```swift
let selector = #selector(viewDidLoad)
view.backgroundColor = .red
let toView = context.view(forKey: .to)
let view = UIView(frame: .zero)
```

#### Не рекомендуется

```swift
let selector = #selector(ViewController.viewDidLoad)
view.backgroundColor = UIColor.red
let toView = context.view(forKey: UITransitionContextViewKey.to)
let view = UIView(frame: CGRect.zero)
```

### Дженерики

Параметры дженериков (generics) должны быть описаны, используя ```UpperCamelCase```. Если имя типа не имеет значимых отношений или роли, используйте традиционную одиночную прописную букву, такую как ```T```, ```U``` или ```V```.

#### Рекомендуется

```swift
struct Stack<Element> { ... }
func write<Target: OutputStream>(to target: inout Target)
func swap<T>(_ a: inout T, _ b: inout T)
```

#### Не рекомендуется

```swift
struct Stack<T> { ... }
func write<target: OutputStream>(to target: inout target)
func swap<Thing>(_ a: inout Thing, _ b: inout Thing)
```

## Организация кода

Используйте расширения (extensions), чтобы организовать свой код в логические блоки.  Каждое расширение должно быть помечено с помощью ```// MARK: - комментарий```. Это способствует хорошей организации, кроме того эти отметки видны в выпадающем списке символов в Xcode. 

### Соответствие протоколу

При объявленном соответствии протоколу в модели желательно добавить отдельное расширение для методов протокола. Это удерживает связанные с протоколом методы вместе и может упростить добавление протокола к классу.

#### Рекомендуется

```swift
class MyViewController: UIViewController {
    // class stuff here
}

// MARK: - UITableViewDataSource
extension MyViewController: UITableViewDataSource {
    // table view data source methods
}

// MARK: - UIScrollViewDelegate
extension MyViewController: UIScrollViewDelegate {
    // scroll view delegate methods
}
```

#### Не рекомендуется

```swift
class MyViewController: UIViewController, UITableViewDataSource, UIScrollViewDelegate {
    // all methods
}
```

Поскольку компилятор не позволяет повторно объявить соответствие протоколу в производном классе, не всегда требуется повторять группы расширений базового класса. Особенно если производный класс является конечным (```final```) и переопределяет небольшое число методов. Сохранение группы расширений остаётся на усмотрение автора.

### Неиспользуемый код

Неиспользуемый (мертвый) код, включая код шаблона Xcode и комментарии к ним, должен быть удалён.

Методы, реализация которых просто вызывает суперкласс, также должны быть удалены. Сюда входят любые пустые / неиспользуемые методы ```UIApplicationDelegate```.

#### Рекомендуется

```swift
override func tableView(_ tableView: UITableView, numberOfRowsInSection section: Int) -> Int {
    return Database.contacts.count
}
```

#### Не рекомендуется

```swift
override func didReceiveMemoryWarning() {
    super.didReceiveMemoryWarning()
    // Dispose of any resources that can be recreated.
}

override func numberOfSections(in tableView: UITableView) -> Int {
    // #warning Incomplete implementation, return the number of sections
    return 1
}

override func tableView(_ tableView: UITableView, numberOfRowsInSection section: Int) -> Int {
    // #warning Incomplete implementation, return the number of rows
    return Database.contacts.count
}
```


### Минимальный импорт

Держите импорт минимальным. Например, не импортируйте UIKit, если достаточно импортировать Foundation.

## Отступы

- Отступ использует 4 пробела, не табуляцию.
- Скобки методов и другие фигурные скобки (```if``` / ```else``` / ```switch``` / ```while``` и т. д.) всегда открываются в той же строке, что и оператор, но закрываются на новой строке.

#### Рекомендуется

```swift
if user.isHappy {
    // Do something
} else {
    // Do something else
}
```

#### Не рекомендуется

```swift
if user.isHappy
{
  // Do something
}
else {
  // Do something else
}
```

- Должна быть ровно одна пустая линия между методами, помогающая визуальной ясности и организации. Пустые строки внутри методов должны разделять функциональность, но наличие слишком большого количества разделов в методе часто означает, что вы должны реорганизовать эту функциональность в нескольких методах.
- У двоеточия всегда нет пробела слева и есть один пробел справа. Исключением является тернарный оператор ```? : ```, пустой словарь ```[:]``` и синтаксис ```#selector``` для неназванных параметров ```(_ :)```.

#### Рекомендуется

```swift
class TestDatabase: Database {
    var data: [String: CGFloat] = ["A": 1.2, "B": 3.2]
}
```

#### Не рекомендуется

```swift
class TestDatabase : Database {
    var data :[String:CGFloat] = ["A" : 1.2, "B":3.2]
}
```

- Длинные строки должны быть ограничены примерно 70 символами. Жёсткий предел намеренно не указан.
- Избегайте пробелов на концах строк.
- Добавьте один символ новой строки в конце каждого файла.

## Комментарии

Когда они понадобятся, используйте комментарии, чтобы объяснить, ***почему*** какой-то фрагмент кода что-то делает. Комментарии должны обновляться или быть удалены.

Избегайте блочных комментариев в одной строке с кодом, поскольку код должен быть как можно более самодокументированным. 

*Исключение: комментарии, которые используются для создания документации.*

## Классы и структуры

### Что использовать?

Помните, что **структуры** имеют [семантику значений](https://developer.apple.com/library/content/documentation/Swift/Conceptual/Swift_Programming_Language/ClassesAndStructures.html#//apple_ref/doc/uid/TP40014097-CH13-XID_144). Используйте структуры для вещей, которые не имеют идентичности. Массив, содержащий [a, b, c], действительно тот же, что и другой массив, содержащий [a, b, c], и они полностью взаимозаменяемы. Неважно, используете ли вы первый массив или второй, потому что они представляют собой то же самое. Вот почему массивы - это структуры.

**Классы** имеют [ссылочную семантику](https://developer.apple.com/library/mac/documentation/Swift/Conceptual/Swift_Programming_Language/ClassesAndStructures.html#//apple_ref/doc/uid/TP40014097-CH13-XID_145). Используйте классы для вещей, которые имеют идентичность или определённый жизненный цикл. Человека нужно моделировать как класс, потому что два человека различаются между собой. Если у двух людей одно и то же имя и дата рождения, это не значит, что они одно и то же. Но дата рождения человека была бы структурой, потому что дата 3 марта 1950 года такая же, как и любой другой объект даты 3 марта 1950 года. Сама дата не имеет идентичности.

Иногда модели должны быть структурами, но вынуждены соответствовать ```AnyObject``` или исторически уже смоделированны как классы (```NSDate```, ```NSSet```). Постарайтесь как можно ближе следовать этим рекомендациям.

### Пример определения

Вот пример хорошо продуманного определения класса:

```swift
class Circle: Shape {
    var x: Int, y: Int
    var radius: Double
    var diameter: Double {
        get {
            return radius * 2
        }
        set {
            radius = newValue / 2
        }
    }

    init(x: Int, y: Int, radius: Double) {
        self.x = x
        self.y = y
        self.radius = radius
    }
    
    convenience init(x: Int, y: Int, diameter: Double) {
        self.init(x: x, y: y, radius: diameter / 2)
    }

    override func area() -> Double {
        return Double.pi * radius * radius
    }
}

extension Circle: CustomStringConvertible {
    var description: String {
        return "center = \(centerString) area = \(area())"
    }
    private var centerString: String {
        return "(\(x),\(y))"
    }
}
```


В приведенном выше примере демонстрируются следующие стили:

- Указание типов свойств, переменных, констант, объявление аргументов и других операторов с пробелом после двоеточия, но не раньше, например ```x: Int``` и ```Circle: Shape```.
- Определение нескольких переменных и структур в одной строке, если они имеют общую цель/контекст.
- Отступы для определений геттера/сеттера и наблюдателей свойств (property observers).
- Отсутствие явных модификаторов доступа ```internal```, так они присутствую неявно по умолчанию. Аналогичным образом не следует повторять модификатор доступа при переопределении метода.
- Дополнительная функциональность организована в расширениях.
- Частные детали реализации, такие как ```centerString```, скрыты внутри расширения модификатором доступа ```private```.

### Использование Self

Для краткости избегайте использования ```self```, так как Swift не требует этого в обращениях к свойствам объекта или при вызове его методов.

Используйте ```self``` только тогда, когда этого требует компилятор (в ```@escaping``` замыканиях или в инициализаторах свойств из аргументов для устранения неоднозначности). Другими словами, если код компилируется без ```self```, то опустите его.

### Вычисляемые свойства

Если вычисляемое свойство (сomputed property) доступно только для чтения, опустите блок ```get```. Он требуется только тогда, когда предоставляется блок ```set```.

#### Рекомендуется

```swift
var diameter: Double {
    return radius * 2
}
```

#### Не рекомендуется

```swift
var diameter: Double {
    get {
        return radius * 2
    }
}
```

### Final

Иногда стоит использовать ```final```, чтобы уточнить ваши намерения. В приведенном ниже примере ```Box``` имеет особую цель и не предназначен для наследования. Пометка его как ```final``` проясняет это.

```swift
// Используйте Box для преобразования любого общего типа в ссылочный.
final class Box<T> {
    let value: T
        init(_ value: T) {
        self.value = value
    }
}
```

## Объявление функций

Сохраняйте короткие объявления функций на одной строке, включая открывающую скобку:

```swift
func reticulateSplines(spline: [Double]) -> Bool {
    // reticulate code goes here
}
```

Для функций с длинными сигнатурами добавьте переносы строк в соответствующих местах и добавьте дополнительный отступ в последующих строках:

```swift
func reticulateSplines(spline: [Double], adjustmentFactor: Double,
    translateConstant: Int, comment: String) -> Bool {
    // reticulate code goes here
}
```

## Замыкания

Используйте синтаксис закрывающего замыкания только в том случае, если в конце списка аргументов имеется только одно выражение замыкания. Дайте описательные имена параметрам замыкания.

#### Рекомендуется

```swift
UIView.animate(withDuration: 1.0) {
    self.myView.alpha = 0
}

UIView.animate(withDuration: 1.0, animations: {
    self.myView.alpha = 0
}, completion: { finished in
    self.myView.removeFromSuperview()
})
```

#### Не рекомендуется

```swift
UIView.animate(withDuration: 1.0, animations: {
    self.myView.alpha = 0
})

UIView.animate(withDuration: 1.0, animations: {
    self.myView.alpha = 0
}) { f in
    self.myView.removeFromSuperview()
}
```

Используйте неявное возвращение в замыканиях с ясным контекстом:

```swift
attendeeList.sort { a, b in
    a > b
}
```

Цепочки методов с использованием закрывающих замыканий должны быть четкими и легко читаемыми в контексте. Решения о расположении, переносе строк, использовании именных или анонимных аргументов оставляются на усмотрение автора. Примеры:

```swift
let value = numbers.map { $0 * 2 }.filter { $0 % 3 == 0 }.index(of: 90)

let value = numbers
    .map {$0 * 2}
    .filter {$0 > 50}
    .map {$0 + 10}
```

## Типы

Всегда используйте родные типы Swift, когда они доступны. Swift связан с Objective-C, поэтому вы можете использовать полный набор методов по мере необходимости.

#### Рекомендуется

```swift
let width = 120.0                                    // Double
let widthString = (width as NSNumber).stringValue    // String
```

#### Не рекомендуется

```swift
let width: NSNumber = 120.0                          // NSNumber
let widthString: NSString = width.stringValue        // NSString
```

В коде Sprite Kit, UIKit и Core Graphics используйте ```CGFloat```, если он делает код более кратким, избегая слишком большого количества преобразований.

### Константы

Константы определяются ключевым словом ```let```, а переменные с ключевым словом ```var```. Всегда используйте ```let``` вместо ```var```, если значение переменной не изменится.

***Совет***. Хорошей практикой является определение всего, используя ```let``` и только тогда изменять его на ```var```, когда компилятор жалуется.

Вместо глобальных констант объявляйте статические свойства типа, используя ```static let```. Пример:

#### Рекомендуется
```swift
enum Math {
    static let e = 2.718281828459045235360287
    static let root2 = 1.41421356237309504880168872
}

let hypotenuse = side * Math.root2
```

**Примечание**. Преимущество использования ```enum``` без ```case``` состоит в том, что не может быть случайно создан экземпляр такого перечисления, и оно работает как чистое пространство имен.

#### Не рекомендуется

```swift
let e = 2.718281828459045235360287  // загрязняет глобальное пространство имён
let root2 = 1.41421356237309504880168872

let hypotenuse = side * root2 // что такое root2?
```

## Статические методы и свойства типа

Статические методы и свойства типа работают аналогично глобальным функциям и глобальным переменным и должны использоваться экономно. Они полезны, когда функциональность привязана к определенному типу или когда требуется совместимость Objective-C.

## Опциональные типы

Объявляйте переменные и возвращаемые типы функций как опциональные ```?``` там, где допустимо значение ```nil```.

Используйте неявно развернутые типы, объявленные с помощью ```!``` только для переменных, о которых вы точно знаете, что они будут инициализированы до их использования, например, ```subviews```, которые будут установлены во ```viewDidLoad```.

При доступе к опциональному значению (одному или многим) используйте опциональную цепочку:

```swift
self.textContainer?.textLabel?.setNeedsDisplay()
```

Когда удобнее один раз развернуть и выполнить несколько операций со значением, используйте опциональную привязку (optional binding):

```swift
if let textContainer = self.textContainer {
    // do many things with textContainer
}
```

Избегайте такие имена для опциональных переменных, как ```optionalString``` или ```optionalView```, поскольку их опциональность уже указана в объявлении типа.

Для опциональной привязки при необходимости затените исходное имя переменной в локальной области видимости.

#### Рекомендуется

```swift
var subview: UIView?
var volume: Double?

// later on...
if let subview = subview, let volume = volume {
    // do something with unwrapped subview and volume
}
```

#### Не рекомендуется

```swift
var optionalSubview: UIView?
var volume: Double?

if let unwrappedSubview = optionalSubview {
    if let realVolume = volume {
        // do something with unwrappedSubview and realVolume
    }
}
```

## Ленивая инициализация

Рассмотрите возможность использования ленивой инициализации для более тонкого контроля за временем жизни объекта. Это особенно удобно для ```UIViewController```, который лениво загружает представления. Вы можете использовать замыкание, которое вызывается на месте ```{} ()``` или вызов приватной фабрики. Пример:

```swift
lazy var locationManager: CLLocationManager = self.makeLocationManager()

private func makeLocationManager() -> CLLocationManager {
    let manager = CLLocationManager()
    manager.desiredAccuracy = kCLLocationAccuracyBest
    manager.delegate = self
    manager.requestAlwaysAuthorization()
    return manager
}
```

#### Примечения:

- ```[unowned self]``` использовать не требуется. Циклической ссылки не создаётся.
- У ```CLLocationManager``` есть побочный эффект появления всплывающего окна для запроса разрешения у пользователя, поэтому здесь есть смысл в точном контроле времени инициализации.

## Приведение типов

Предпочитайте компактный код, пусть компилятор сам выводит тип для констант или переменных отдельных экземпляров. Приведение типов также подходит для небольших (непустых) массивов и словарей. При необходимости укажите конкретный тип, например ```CGFloat``` или ```Int16```.

#### Рекомендуется

```swift
let message = "Click the button"
let currentBounds = computeViewBounds()
var names = ["Mic", "Sam", "Christine"]
let maximumWidth: CGFloat = 106.5
```

#### Не рекомендуется

```swift
let message: String = "Click the button"
let currentBounds: CGRect = computeViewBounds()
let names = [String]()
```

### Аннотация типа для пустых массивов и словарей

Используйте аннотацию типа для пустых массивов и словарей.

#### Рекомендуется

```swift
var names: [String] = []
var lookup: [String: Int] = [:]
```

#### Не рекомендуется

```swift
var names = [String]()
var lookup = [String: Int]()
```

**Примечание**. Здесь видно, почему так важен выбор описательных имён.

## Синтаксический сахар

Предпочитайте краткие версии объявлений типа полному синтаксису с дженериками.

#### Рекомендуется

```swift
var deviceModels: [String]
var employees: [Int: String]
var faxNumber: Int?
```

#### Не рекомендуется

```swift
var deviceModels: Array<String>
var employees: Dictionary<Int, String>
var faxNumber: Optional<Int>
```

## Функции vs Методы

Свободные функции, которые не привязаны к классу или типу, должны использоваться экономно. Предпочитайте использовать метод вместо свободной функции всегда, когда это возможно. Это помогает читаемости и понятности кода.

Свободные функции допускаются, если они не связаны с каким-либо конкретным типом или экземпляром.

#### Рекомендуется

```swift
let sorted = items.mergeSorted()
rocket.launch()
```

#### Не рекомендуется

```swift
let sorted = mergeSort(items)
launch(&rocket)
```

#### Исключения

```swift
let tuples = zip(a, b)  // выглядит естественно как свободная функция
let value = max(x, y, z)  // аналогично
```

## Управление памятью

Код не должен создавать ссылочные циклы. Проанализируйте граф объектов и предотвратите ```strong``` циклы использованием ```weak``` и ```unowned``` ссылок. Кроме того, используйте типы значений (```struct```, ```enum```) для предотвращения циклов.

### Продление времени жизни объекта
 
Продлевайте время жизни объекта, используя идиому ```[weak self]``` и ```guard let strongSelf = self else { return }```.

```[weak self]``` предпочтительнее, чем ```[unowned self]```, где не сразу очевидно, что ```self``` используется в замыкании. Явное продление срока жизни предпочтительнее, чем опциональная  развёртка.

#### Рекомендуется

```swift
resource.request().onComplete { [weak self] response in
    guard let strongSelf = self else {
        return
    }
    let model = strongSelf.updateModel(response)
    strongSelf.updateUI(model)
}
```

#### Не рекомендуется

```swift
// может упасть, если self будет отпущен до того, как выполнится запрос
resource.request().onComplete { [unowned self] response in
    let model = self.updateModel(response)
    self.updateUI(model)
}
```

#### Не рекомендуется

```swift
// деаллокация может случиться между обновлением модели и обновлением UI
resource.request().onComplete { [weak self] response in
    let model = self?.updateModel(response)
    self?.updateUI(model)
}
```

## Управление доступом

Использование ```private``` и ```fileprivate``` надлежащим образом добавляет ясности и поддерживает инкапсуляцую. По возможности используйте ```private``` вместо ```fileprivate```. Использование расширений может потребовать использования ```fileprivate```.

Используйте ```open```, ```public``` и ```internal``` только когда вам требуется полная спецификация контроля доступа.

Используйте спецификатор управления доступом в качестве ведущего спецификатора свойств. Единственное, что должно идти до контроля доступа, это спецификатор ```static``` или такие атрибуты, как ``@objc``, ``@IBAction``, ``@IBOutlet`` и ``@discardableResult``.

#### Рекомендуется

```swift
private let message = "Great Scott!"

class TimeMachine {  
    fileprivate dynamic lazy var fluxCapacitor = FluxCapacitor()
}
```

#### Не рекомендуется

```swift
fileprivate let message = "Great Scott!"

class TimeMachine {  
    lazy dynamic fileprivate var fluxCapacitor = FluxCapacitor()
}
```

## Циклы

Для циклов предпочитайте ```for-in``` вместо ```while-condition-increment```.

#### Рекомендуется

```swift
for _ in 0..<3 {
    print("Hello three times")
}

for (index, person) in attendeeList.enumerated() {
    print("\(person) is at position #\(index)")
}

for index in stride(from: 0, to: items.count, by: 2) {
    print(index)
}

for index in (0...3).reversed() {
    print(index)
}
```

#### Не рекомендуется

```swift
var i = 0
while i < 3 {
    print("Hello three times")
    i += 1
}


var i = 0
while i < attendeeList.count {
    let person = attendeeList[i]
    print("\(person) is at position #\(i)")
    i += 1
}
```

## Условные операторы

Избегайте вложенности условных операторов. Использовать ```return``` несколько раз нормально. Оператор ```guard``` создан специально для этого.

#### Рекомендуется

```swift
func computeFFT(context: Context?, inputData: InputData?) throws -> Frequencies {

    guard let context = context else {
        throw FFTError.noContext
    }
    guard let inputData = inputData else {
        throw FFTError.noInputData
    }

    // use context and input to compute the frequencies
    return frequencies
}
```

#### Не рекомендуется

```swift
func computeFFT(context: Context?, inputData: InputData?) throws -> Frequencies {

    if let context = context {
        if let inputData = inputData {
            // use context and input to compute the frequencies

            return frequencies
        } else {
            throw FFTError.noInputData
        }
    } else {
        throw FFTError.noContext
    }
}
```

Когда разворачиваются несколько опциональных переменных с ```guard``` или ```if```, минимизируйте вложенность, используя составную версию. Пример:

#### Рекомендуется

```swift
guard let number1 = number1,
        let number2 = number2,
        let number3 = number3 else {
        fatalError("impossible")
}
// do something with numbers
```

#### Не рекомендуется

```swift
if let number1 = number1 {
    if let number2 = number2 {
        if let number3 = number3 {
            // do something with numbers
        } else {
            fatalError("impossible")
        }
    } else {
        fatalError("impossible")
    }
} else {
    fatalError("impossible")
}
```

### Провал Guards

Операторы ```guard``` обязаны каким-то образом выйти. Как правило, это должен быть простой однострочный оператор, такой как ```return```, ```throw```, ```break```, ```continue``` и ```fatalError()```. Следует избегать больших блоков кода. Если для нескольких точек выхода требуется код очистки, подумайте об использовании блока ```defer```, чтобы избежать дублирования.

## Точка с запятой

Swift не требует точки с запятой после каждого выражения в вашем коде. Они необходимы только в том случае, если вы хотите объединить несколько операторов в одной строке.

Не записывайте несколько операторов в одну строку, разделённые точкой с запятой.

#### Рекомендуется

```swift
let swift = "not a scripting language"
```

#### Не рекомендуется

```swift
let swift = "not a scripting language";
```

## Круглые скобки

Скобки вокруг условий не требуются и их следует опустить.

#### Рекомендуется

```swift
if name == "Hello" {
    print("World")
}
```

#### Не рекомендуется

```swift
if (name == "Hello") {
    print("World")
}
```

В длинных выражениях необязательные круглые скобки иногда могут сделать код более читаемым.

#### Рекомендуется

```swift
let playerMark = (player == current ? "X" : "O")
```

## Экспорт в Objective-C

- Добавляйте ```@objc``` атрибут к Swift классам, их методам и протоколам, которые будут использоваться в Objective-C коде.
- Всегда добавляйте ```@objc``` атрибут к методам, обрабатываемым динамически, например, при использовании ```#selector```.
- ```@objc``` типы должны иметь спецификатор доступа ```open``` или ```public``` и наследоваться от ```NSObject```.
- Используйте ```@objc(methodName:)``` для назначения экспортируемых имён методов согласно правилам именования Objective-C.
- Добавляйте префиксы к экспортируемым классам для единообразия с Objective-C API.

## Ссылки

* [The Official raywenderlich.com Swift Style Guide.](https://github.com/raywenderlich/swift-style-guide/)
* [The Swift API Design Guidelines](https://swift.org/documentation/api-design-guidelines/)
* [The Swift Programming Language](https://developer.apple.com/library/prerelease/ios/documentation/swift/conceptual/swift_programming_language/index.html)
* [Using Swift with Cocoa and Objective-C](https://developer.apple.com/library/prerelease/ios/documentation/Swift/Conceptual/BuildingCocoaApps/index.html)
* [Swift Standard Library Reference](https://developer.apple.com/library/prerelease/ios/documentation/General/Reference/SwiftStandardLibraryReference/index.html)