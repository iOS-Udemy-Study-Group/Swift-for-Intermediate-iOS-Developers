# Swift-for-Intermediate-iOS-Developers


<br>



## 스터디 목차





## 스터디 방식

- Udemy 강의를 구매 후 개별 인증을 해야 스터디 참여 가능
- 각자 학습한 내용을 공용 study git repository에 공유하고 토의하면서 내용 보완
  - 개인적으로 공부한 내용을 개인폴더에 기록 (fork 후, 개인 스터디 내용 기록 했다가 공용 repository에 PR)
    - 각자 의무감을 갖고 스터디하기 위함
  - 매주 다수결로 스터디 날짜 지정 후, 2시간씩 스터디 진행
- 스터디 기록 정리 방식에 대한 좋은 의견 자유롭게 공유
- 스터디 중에 자유롭게 이해한 내용을 얘기할 수 있으며, 이해가 되지 않은 내용이 있다면 언제든지 공유





### 1주차 스터디 

- 1/28(토), 오후 2시 ~ 4시 (완료)
- Section 2: Swift Collections ~ Section 3: Functions
- 👩🏻‍💻 [applebuddy](https://github.com/applebuddy) | [AppleCEO](https://github.com/appleceo) | [Jae-eun](https://github.com/jae-eun) 

### 2주차 스터디 

- 2/4(토), 오후 9시 ~ 11시 (완료)
- Section 4: Enumerations ~ Section 5: Properties, Lazy Stored Properties
- 👩🏻‍💻 [applebuddy](https://github.com/applebuddy) | [AppleCEO](https://github.com/appleceo) | [Jae-eun](https://github.com/jae-eun) | [Lim-YongKwan](https://github.com/lim-yongkwan)

### 3주차 스터디 

- 2/12(일), 오전 10시 ~ 12시 (완료)
- Section 5: Computed Properties ~ Section 5: Error Handling, Example 1, Throwing Errors
- 👩🏻‍💻 [applebuddy](https://github.com/applebuddy) | [AppleCEO](https://github.com/appleceo) | [Jae-eun](https://github.com/jae-eun) | [Lim-YongKwan](https://github.com/lim-yongkwan)

### 4주차 스터디 

- 2/19(일), 오전 10시 ~ 12시
- Section 5: Error Handling, Example 2, Throwing Errors ~ Section 9: Async and Await
- 👩🏻‍💻 [applebuddy](https://github.com/applebuddy) | [AppleCEO](https://github.com/appleceo) | [Jae-eun](https://github.com/jae-eun) | [Lim-YongKwan](https://github.com/lim-yongkwan)

### 5주차 스터디

- 3/5(일), 오전 9시 ~ 11시
- Section 9: Async and Await ~ Section 12: Handling Error for Async Architecture, Using Result Type
- 👩🏻‍💻 [applebuddy](https://github.com/applebuddy) | [AppleCEO](https://github.com/appleceo) | [Jae-eun](https://github.com/jae-eun)

<br>



## Section 2: Swift Collections

### Sequence Protocol

- swift로 선언되는 Array는 기본적으로 Sequence 프로토콜을 준수하고 있습니다.

- Sequence 프로토콜은 IteratorProtocol opaque type을 반환하는 makeIterator()를 구현해야 합니다.
  - makeIterator()는 IteratorProtocol을 준수하는 타입을 반환할 수 있는데, IteratorProtocol을 준수하기 위해서는 next() 메서드를 정의해주어야 합니다.

~~~swift
let names = ["Alex", "John", "Mary"]
// Sequence프로토콜을 준수하는 경우, makeIterator()를 통해 iterator를 반환할 수 있습니다.
var nameIterator = names.makeIterator()
// iterator는 next()를 통해 다음 Element를 반환할 수 있습니다.
while let name = nameIterator.next() {
  print(name)
}

// Sequence 프로토콜을 채택한 배열은 for in loop를 사용 가능합니다.
for name in names {
 print(name)
}
~~~

~~~swift
/*
// IteratorProtocol의 정의 형태, next()를 구현해주어야 한다.
protocol IteratorProtocol {
  associatedtype Element
  
  mutating func next() -> Element?
}
*/
~~~



### Sequence Protocol을 채택한 struct 사용 예시

~~~swift
// Sequence 프로토콜을 준수하는 Countdown
// makeIterator()를 구현해야한다. IteratorProtocol을 준수하는 CountdownIterator를 반환하고 있다.
struct Countdown: Sequence {
  let start: Int
  func makeIterator() -> some IteratorProtocol {
    return CountdownIterator(self)
  }
}

// IteratorProtocol은 next()를 구현해주어야 한다.
struct CountdownIterator: IteratorProtocol {
  typealias Element = Int
  let countdown: Countdown
  var currentValue = 0
  
  init(_ countdown: Countdown) {
    self.countdown = countdown
    self.currentValue = countdown.start
  }
  
  mutating func next() -> Element? {
    if currentValue > 0 {
      let value = currentValue
      currentValue -= 1
      return value
    } else {
      return nil
    }
  }
}

// Countdown의 Element를 for in loop로 순회 (
let countdown = Countdown(start: 10)
for count in countdown {
  print(count)
}
~~~



### Filter

- 특정 조건을 충족하는 Element만 filtering하고 싶을 때 사용

~~~swift
let names = ["Apple", "Banana", "Car", "Elephant"]
let finalNames = names.filter { name in
  // 이름 길이가 5 이상인 것만 필터링 할래
  return name.count >= 5
}

print(finalNames)

struct Movie {
  let title: String
  let genre: String
}

let movies = [Movie(title: "A", genre: "asb"),
              Movie(title: "fireMan", genre: "hero"),
              Movie(title: "C", genre: "zesb")]
let heroMovies = movies.filter { movie in
  // hero genre만 filtering
  return movie.genre == "hero"
}
~~~



### ForEach, Enumeration

- 각각 Collection의 Element를 전체 순회하거나, (offset, element) 형태로 순회할 때 사용

~~~swift
// MARK: - ForEach

let movies = [Movie(title: "AS", genre: "asb"),
  Movie(title: "fireMan", genre: "hero"),
  Movie(title: "C", genre: "zesb")]
movies.forEach {
  print($0)
}

// MARK: - Enumeration

movies.enumerated().forEach {
  print("offset : \($0.offset), element : \($0.element)")
}
~~~



### Lazy Sequence

- 많은 Element Set 을 처리하는데, 최종 결과값은 그 중 극 소수의 Element가 되는 경우, Lazy Sequence를 활용하면 불필요한 연산을 줄일 수 있다.

~~~swift
// MARK: Lazy Sequence

let indexes = 1..<5000
let images = indexes.lazy.filter { index -> Bool in
  print("[Filter]")
  // lazy sequence를 사용하지 않는 일반적인 경우, filter가 5000가량 가량 실행된다.
  // lazy sequence를 사용하면, 연산량이 대폭 줄어든다.
  return index % 2 == 0
}.map { index -> String in
  print("[Map]")
  return "index_\(index)"
}

// lazy itertion + suffix를 한 결과물을 상수에 할당했을때 lazy 동작이 됨
let lastThreeImages = images.prefix(3)

// 근데 우리는 마지막 세개 요소만 필요한데,,, 너무 불필요한 작업이 많은 것 같다.
for image in lastThreeImages {
  print(image)
}

// 단순 print문에 suffix를 사용하는건 lazy 동작이 되지 않았음
print(images.suffix(3))
~~~



### Reduce

- Collection의 Element를 특정 연산으로 하나로 합치고 싶을때 사용할 수 있다. inout parameter를 사용하지 않는 reduce(_:), inout parameter를 사용하는  reduce(into:) 가 있다.

~~~swift
// MARK: - reduce

struct Item {
  let name: String
  let price: Double
}

struct Cart {
  private(set) var items: [Item] = []
  mutating func addItem(_ item: Item) {
    items.append(item)
  }
  
  var total: Double {
    items.reduce(0) { (value, item) -> Double in
      value + item.price
    }
  }
}

var cart = Cart()
cart.addItem(Item(name: "Milk", price: 4.50))
cart.addItem(Item(name: "Bread", price: 2.50))
cart.addItem(Item(name: "Eggs", price: 12.50))
print(cart.total)

let totalExample = [1, 2, 3, 4, 5].reduce(0, +)
print(totalExample)

let ratings = [4, 0.5, 4.23, 3.64, 7.3, 0.1, 8.6]
// 기본 reduce로는 dictionary 등의 최종 결과값을 처리히기 위해 클로져 내부에 지역변수를 만들어서 결과물을 누적시켜야하는 번거로움이 있다.
// 이 경우에는 복사본을 만들 필요가 없는 reduce(into:)를 대신 사용할 수 있다.
let results = ratings.reduce(into: [:]) { (results: inout [String: Int], rating: Double) in
  switch rating {
  case 1..<4: results["very bad", default: 0] += 1
  case 5..<7: results["medium", default: 0] += 1
  case 7..<9: results["good", default: 0] += 1
  default: break
  }
}
print(results)
~~~



### Zip

- 두 개의 Collection을 각 Element 순서에 맞게 쌍을 이루는 튜플 형태로 반환하여 처리할 수 있다.

~~~swift
// MARK: - Zip
// 각각의 Collection의 동일 index의 Element를 짝지어 튜플형태로 처리하고 싶을때 zip을 사용 가능
let students = ["Alex", "Mary", "John", "Steven"]
let grades = [3.4, 2.8, 3.8, 4]
let pairs = zip(students, grades)

for pair in pairs {
  print(pair)
}

/*
// == output ==
("Alex", 3.4)
("Mary", 2.8)
("John", 3.8)
("Steven", 4.0)
*/
~~~



## Section 3: Functions

### Nested function, 중첩 함수

- 함수 내에 또다른 함수를 정의하여 함수내에서 사용할 수 있다. 함수 내의 코드를 함수로 분리해서 사용하고 싶을때 사용 가능
  - 하지만, 적시에 사용하지 않으면 오히려 코드의 가독성이 떨어지게 될 수 있다.

~~~swift
// MARK: - Nested Functions

func makeApp() {
  // 함수안에 함수가 정의되어 사용 가능
  func design() {}
  func develop() {}
  func qa() {}
  func distribution() {}
  
  design()
  develop()
  qa()
  distribution()
}

makeApp()
~~~



### Closure

- Closure는 1급객체이자 익명함수입니다. 함수, 콜벡함수 등으로 사용될 수 있습니다.
  - Closure는 할당받을 수 있고, 인자로 전달도 되며, 반환도 가능합니다.

~~~swift
// 하나의 인자를 받아 그 값을 출력하는 함수의 역할을 하는 closure
let hello: (String) -> () = { name in
  print("hello, \(name)!")
}

// 두 개의 인자를 받아 곱을 반환하는 함수의 역할을 하는 closure
let pow: (Int, Int) -> Int = {
  $0 * $1
}

// closure 사용 예시
hello("John Doe")
let result = pow(5, 3)
print(result)
/* == output ==
 hello, John Doe!
 15
*/
~~~



- Closure 내에서 사용되는 변수, 상수가 메모리가 해제 되지 않고, 외부로 전달이 되는 경우, escaping closure임을 명시해주어야 한다.
  - escaping keyword를 명시하지 않으면 Escaping closure captures non-escaping parameter 'completion' 컴파일 에러 발생
  - escaping closure 방식 대신,  async await 방식으로 개선해서 사용할 수도 있다.

~~~swift
// escaping keyword 명시 안하면, Escaping closure captures non-escaping parameter 'completion' compile error 발생
// 외부에서 parameter를 접근하는 경우, escaping closure가 되어야 한다.
func getPosts(completion: @escaping ([String]) -> ()) {
  var posts: [String] = []
  DispatchQueue.main.asyncAfter(deadline: .now() + 2) {
    posts = ["Hello World", "Introduction to Closures"]
    completion(posts)
  }
}

getPosts { posts in
  // 약 2초 뒤, ["Hello World", "Introduction to Closures"] 를 받게 됨
  print(posts)
}
~~~





## Section 4: Enumerations

### 객체를 Struct 대신 Enum으로 선언하면 얻을 수 있는 이점

- Teacher, Student 등 다양한 케이스로 정의가 되어야 할때 enum은 case정의를 통해 타입 분기를 할때 switch문을 활용하여 깔끔하게 처리할 수 있다.
  - 다양한 케이스 객체를 하나의 타입에 담아서 switch 문과 사용할때 default 분기 없이 보다 깔끔한 케이스 분기를 정의할 수 있게 된다.
  - 또한 타입 내의 다양한 객체를 처리할때 타입캐스팅을 할 필요가 줄어든다.
- 타입 하나만으로 케이스를 나누어 다양한 객체를 분기하여 다룰 수 있다. (enum의 다형성 활용)
  - 강타입 언어인 swift 정책에 어긋나는 Any타입 사용을 줄일 수 있다.
- 타입 하나에 대한 케이스가 추후 확장 되어도, 손쉽게 케이스를 확장하여 정의할 수 있다. (enum의 확장성)



### ⚠ enum이 항상 장점만 있는 것은 아니다. 

- 나누어지는 각각의 케이스가 공통적인 멤버를 다수 갖고 있을때, class는 서브클래싱을 통해 공통 멤버를 정의하고, 중복 정의를 줄일 수 있다.

- 반면, enum은 각각 케이스의 중복 멤버를 하나하나 정의해주어야 한다.

- 공통멤버가 추후 변동되지 않고 고정됨이 보장된다면, 서브클래싱이 가능한 class가 더 좋은 방법이 될 수도 있다.

  - 물론, 공통 멤버가 일정하지 않고, 언제든 바뀔 수 있다면, 더욱 쉽게 케이스 멤버 변형이 가능한 enum을 사용하는 것이 더 좋을 수 있다.
  - 특정 타입이 생기거나, 공통멤버가 바뀌는 경우에 대한 수정 소요가 enum을 사용할때 clsas subclassing 방식에 비해 용이하기 때문이다.

  

### enum + failable initializer를 활용하여 rawValue에 따른 세부 타입 설정 정의하기

~~~swift
import UIKit

enum ImageType: String {
  case jpg
  case bmp
  case png
  
  // jpg, jpeg 모두 jpg 타입으로 보고 싶다면? init?(rawValue: String) {} 생성자를 활용해볼 수 있다.
  init?(rawValue: String) {
    // 생성자에 들어간 rawValue가 jpg, jpeg일 경우에는 .jpg 타입으로 설정되도록 했다. 잘못된 rawValue가 들어가면 nil을 반환하는 failable initializer이다.
    switch rawValue.lowercased() {
    case "jpg", "jpeg": self = .jpg
    case "bmp": self = .bmp
    case "png": self = .png
    default: return nil
    }
  }
}
~~~



## Section 5: Properties

### Lazy stored properties

- 문제 5초 후에 돌려주는 프로퍼티를 만들고 호출하면 매 문제를 받으려면 5초를 기다려야 한다. 이미 있는 문제인데도 5초를 기다리는 것이 불필요할 때 lazy 를 붙이고 가장 뒤에 () 를 붙엿 연산 프로퍼티를 만들 수 있다. 이렇게 되면 두번째 문제를 받을 때 5초 기다리지 않고 바로 받을 수 있다.

- lazy 프로퍼티는 접근하는 시점에 초기화되는 특성이 있다.

### computed properties

- 연산 프로퍼티라고도 불리운다.

- 시작 시간 프로퍼티와 끝난 시간 프로퍼티를 따로 불러와서 빼줘서 걸린 시간을 계산할 수 있다.

- 아래와 같이 연산 프로퍼티를 선언하여 시작 시간과 끝난 시간의 차이를 바로 가져올 수 있다.

~~~swift
struct Workout {
  let startTime: Date
  let endTime: Date

  var timeElapsed: TimeInterval {
    endTime.timeIntervalSince(startTime)
  }
}
~~~

### propertyObservers

- didSet { } 등을 사용해 프로퍼티가 변경되는 것을 탐지하여 어떤 동작을 수행할 수 있다.

- 초기화 시에는 didSet 이 호출되지 않는데 그 때는 defer 를 사용해 우회적으로 didSet을 호출할 수 있다.

- 아래 코드에서는 초기화부터 url이 바뀔 때마다 인코딩을 해줘서 url 프로퍼티가 항상 url 인코딩된 것을 보장한다.
~~~swift
struct Website {
  init(url: String) {
    defer { self.url = url }
    
    self.url = url
  }
  
  var url: String {
    didSet {
      url = url.addingPercentEncoding(withAllowedCharacters: .urlHostAllowed) ?? url
    }
}
~~~


## Section 9: Async and Await

### 동기 코드에서 await 키워드와 함께 비동기 동작 실행하기

~~~swift
// MARK: 45. Performing Asynchronous Action from Synchronous Code
// task를 사용하면 블럭 내에서 await 예약어를 사용한 비동기 동작을 수행하여 비동기적으로 결과를 기다릴 수 있다.
Button {
	// iOS 13+
	Task {
	  // .. do something
	}
	// iOS 15+
	task {
	  // refresh the news
	  await newsSourceListViewModel.getSources()
	}
} label: {
	Text("Test Button")
}
~~~



## Section 10: Protocol Oriented Design



### Protocol의 associatedtype과 generic type 사용방법, where 절을 활용한 타입 제약설정 하기

~~~swift
import Foundation

// MARK: 연관타입(associatedtype), 제네릭(generic), where 절 associatedtype 제약 활용하기
// Parser protocol은 Input, Output 두개의 연관타입(associatedtype)을 갖습니다.
// Parser를 채택하는 곳에서 입맛에 따라 Input, Output 타입을 typealias로 지정하여 사용 가능합니다.
protocol Parser {
  associatedtype Input
  associatedtype Output
  
  func parse(input: Input) -> Output
}

class NewParser: Parser {
  // NewParser 클래스에서의 Input 타입은 String, Output 타입은 [String: String]으로 간주가 됩니다.
  typealias Input = String
  typealias Output = [String: String]
  
  func parse(input: Input) -> Output {
    return [input: input]
  }
}

// 제네릭 타입을 사용해서 Parser protocol의 연관타입에 대한 제약을 줄 수 있습니다.
// 가령 아래 메서드의 경우, parser는 Parser protocol을 준수해야 하며, inputs의 elements는 NewParser의 Input타입인 경우에만 사용이 가능합니다.
func runParse<P: Parser>(parser: P, inputs: [P.Input]) where P.Input == NewParser.Input {
  inputs.forEach {
    let result = parser.parse(input: $0)
    print(result)
  }
}

func test() {
  let newParser = NewParser()
  runParse(parser: newParser, inputs: ["input", "input2"])
}

test()
// - Output
/*
["input": "input"]
["input2": "input2"]
*/
~~~



### Protocol Composition, 프로토콜 구성

~~~swift
import Foundation

// MARK: Protocol Composition (프로토콜 구성)

struct Lecture {
  let title: String
  let time: Int
}

// Student, VerifiedStudent protocol은 각기 다른 멤버, 메서드가 정의되어 있습니다.
protocol Student {
  var lectures: [Lecture] { get set }
  mutating func enroll(_ lecture: Lecture)
}

protocol VerifiedStudent {
  func verify() -> Bool
}

// 아래 extension 기능은 VerifiedStudent, Student 둘 다 준수하는 경우에 사용 가능하다.
// protocol composition
extension VerifiedStudent where Self : Student {
  /// protocol에 mutating func으로 지정되어 있어도, extension부에서 일반 func으로 정의하여 사용할 수 있다.
  func enroll(_ lecture: Lecture) {
    if verify() {
      print("Verified and Enrolled")
    }
  }
  
  func verify() -> Bool {
    return true
  }
}

// VerifiedStudent, Student 프로토콜을 동시에 준수하는 InternationalStudent는 위에 구현한 extension 기능을 사용 가능하다.
// protocol에서 지정한 멤버, 메서드를 별도로 구현하지 않으면 extension에서 구현한 default 기능이 적용된다.
// Student, VerifiedStudent를 준수하는 아래 구조체는 별도 구현하지 않은 enroll, verify 메서드가 extension에서 정의한 default 구현으로 적용된다.
struct InternationalStudent: Student, VerifiedStudent {
  var lectures: [Lecture] = []
}

let student = InternationalStudent()
print(student.verify())
student.enroll(Lecture(title: "myLecture", time: 3))
// - Output
/*
 true
 Verified and Enrolled
*/
~~~





### 제네릭타입을 사용한 객체 vs 제네릭타입을 제거 후, 프토토콜 타입만 적용한 객체의 차이

~~~swift
// AirlineTicket 프로토콜을 준수하는 제네릭 타입을 갖는 클래스
class CheckoutServiceWithGeneric<Ticket: AirlineTicket> {
  var tickets: [Ticket]
  
  init(tickets: [Ticket]) {
    self.tickets = tickets
  }
  // AirlineTicket을 준수하는 여러개의 타입을 addTicket에서 취급할 수 없다..
  // AirlineTicket을 채택한 EconomyTicket을 기준으로 초기화가 되었으면, EconomyTicket타입만 취급 가능...
  func addTicket(_ ticket: Ticket) {
    self.tickets.append(ticket)
  }
}

// -> 현실 세계에선 economy, business, first ticket 모두 취급을 할 수 있어야 하는 경우가 있지만, 위 객체는 그게 안됨 😢
~~~

~~~swift
// 제네릭 타입을 제거하고, 대신 protocol 타입을 적용했다.
class CheckoutServiceWithoutGeneric {
  var tickets: [AirlineTicket]
  
  init(tickets: [AirlineTicket]) {
    self.tickets = tickets
  }
  // AirlineTicket을 준수하는 여러개의 타입을 addTicket에서 취급할 수 있게 되었다.
  func addTicket(_ ticket: AirlineTicket) {
    self.tickets.append(ticket)
  }
  // AirlineTicket을 채택한 다양한 타입을 취급 가능
  func processTickets() {
    tickets.forEach { print($0) }
  }
}
~~~

