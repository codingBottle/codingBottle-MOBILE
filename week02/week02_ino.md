# Delegate & Protocol

## Delegate & Protocol

객체 지향 프로그래밍에서 델리게이트(delegate)는 한 객체가 특정 책임이나 작업을 다른 객체로 위임할 수 있는 디자인 패턴입니다. 이는 객체들이 서로 통신하고 협력하며, 느슨한 결합을 가능하게 하고 코드 모듈성을 향상시키는 방법을 제공합니다.

델리게이트는 종종 한 객체가 다른 객체에게 특정 작업을 요청하거나 통지할 필요가 있을 때 사용됩니다. 이때 객체는 구체적인 구현 세부 정보를 알 필요 없이 작업을 위임할 수 있습니다. 이로써 관심사 분리(separation of concerzns)와 더 나은 캡슐화를 촉진합니다.

한편, 프로토콜(protocol)은 클래스, 구조체 또는 열거형이 프로토콜을 준수하도록 정의한 메서드와 속성의 집합입니다. 프로토콜은 행동을 정의하고 애플리케이션의 다른 부분 간에 계약을 만드는 청사진 역할을 합니다.

프로토콜은 여러 클래스나 구조체가 공유 기능을 사용하고 공유 계약을 준수할 수 있도록 공통 인터페이스를 정의하는 방법을 제공합니다. 이로써 코드 재사용과 모듈성이 증가하며, 다른 구현을 교체하거나 기능을 확장하는 것이 더 쉬워집니다.

델리게이트와 프로토콜은 주로 Swift와 Objective-C 등을 사용하는 iOS 및 macOS 개발에서 많이 사용됩니다. 이들은 위임(delegate), 타겟-액션(target-action) 등 다양한 디자인 패턴을 구현하는 데 중요한 역할을 합니다.

델리게이트와 프로토콜을 사용하면 모듈화 가능한 코드를 작성하고 코드 구조를 개선하며 애플리케이션의 유연성과 확장성을 향상시킬 수 있습니다. 이를 통해 관심사를 분리하고 객체 간 협력을 용이하게 하며, 유지 보수 가능하고 확장 가능한 코드베이스를 촉진합니다.

요약하면, 델리게이트와 프로토콜은 객체 지향 프로그래밍에서 효과적인 통신, 코드 구성 및 유연성을 가능하게 하는 강력한 도구입니다. 이들은 iOS 및 macOS 개발과 관련하여 중요한 개념이며, 이를 숙달하면 애플리케이션의 품질과 효율성을 크게 향상시킬 수 있습니다.

**Protocol**

<aside>
💡 Protocol (프로토콜)
”*A protocol defines a blueprint of methods, properties, and other requirements that suit a particular task or piece of functionality. The protocol can then be adopted by a class, structure, or enumeration to provide an actual implementation of those requirements.”*

</aside>

- **공식 문서를 번역해보자면! 프로토콜이란 어떤 기능에 적합한 특정 메소드, 프로퍼티 및 기타 요구 사항의 청사진(Bluprint)을 의미해요!**
- **프로토콜은 클래스, 구조체, 열거형에 의해 채택되며, 프로토콜에 정의된 요구사항의 실제 구현을 제공합니다.**

# **프로토콜의 사용**

- **구조체, 클래스, 열거형**은 **프로토콜을 채택**해서 특정 기능을 실행하기 위한 프로토콜의 요구사항을 실제로 구현할 수 있다.
- 프로토콜은 **정의를 하고 제시를 할 뿐** 스스로 **기능을 구현하지는 않는다.** (**조건만 정의**)
- **하나의 타입으로 사용**되기 때문에 아래와 같이 타입 사용이 허용되는 모든 곳에 프로토콜을 사용할 수 있다.

> - 함수, 메소드, 이니셜라이저의 파라미터 타입 혹은 리턴 타입
> 
> - *상수, 변수, 프로퍼티의 타입*
> - *배열, 딕셔너리의 원소타입*
- 기본형태

```
protocol 프로토콜이름 {
 // 프로토콜 정의
}
```

- 구조체, 클래스, 열거형 등에서 프로토콜을 채택하려면 타입 이름 뒤에 콜론“**:”**을 붙여준 후 채택할 프로토콜 이름을 쉼표“**,”**로 구분하여 명시해준다. (**SubClass의 경우 SuperClass를 가장 앞에 명시한다.**)

```
struct SomeStruct: AProtocol, AnotherProtocol {
 // 구조체 정의
}// 상속받는 클래스의 프로토콜 채택
class SomeClass: SuperClass, AProtocol, AnotherProtocol {
 // 클래스 정의
}
```

# **Property Requirments**

: 프로토콜에서는 프로퍼티가 저장프로퍼티인지 연산프로퍼티인지 명시하지 않고, **이름과 타입 그리고 gettable, settable한지 명시**한다. (프로퍼티는 **항상 var로 선언**해야 한다.)

```swift
protocol Student {
  var height: Double { get set }
  var name: String { get }
  static var schoolNumber: Int { get set }
}
```

- 해당 프로토콜은 학생의 키, 이름, 학번을 정의하였고, 이를 체택하는 타입은 해당 프로퍼티를 구현해줘야한다.

```swift
class Aiden: Student {
    var roundingHeight: Double = 0.0
    var height: Double {
        get {
            return roundingHeight
        }
        set {
            roundingHeight = 183.0
        }
    }
    var name: String = "Aiden"
    static var schoolNumber: Int = 20112330
}

let aiden = Aiden()
print(aiden.height, aiden.name, Aiden.schoolNumber)
// 0.0 Aiden 20112330
aiden.height = 183.0
print(aiden.height, aiden.name, Aiden.schoolNumber)
// 183.0 Aiden 20112330
```

- 프로퍼티는 저장 프로퍼티나 연산 프로퍼티 둘다 사용해서 구현할 수 있다.

# **Method Requirements**

: 프로토콜에서는 인스턴스 메소드와 타입 메소드를 정의할 수 있다. 하지만 **메소드 파라미터의 기본 값은 프로토콜 안에서 사용할 수 없다.**

- 메소드를 정의할 때 **함수명과 반환값을 지정**할 수 있고, **{}는 적지 않는다.**
- **mutating 키워드를 사용해 인스턴스에서 변경 가능하다는 것을 표시할 수 있다. (값 타입에서만 사용 가능)**

```swift
protocol Person {
  static func breathing()
  func sleeping(time: Int) -> Bool
  mutating func running()
}
```

```swift
struct Aiden: Person {
    var heartRate = 100
    static func breathing() {
        print("숨을 쉽니다")
    }
    
    func sleeping(time: Int) -> Bool {
        if time >= 23 {
            return true
        } else {
            return false
        }
        
    }
    
    mutating func running() {
        heartRate += 20
    }
}

print(Aiden.breathing())
// 숨을 쉽니다.
var aiden = Aiden()
print(aiden.sleeping(time: 23))
// true
print(aiden.heartRate)
// 100
aiden.running()
print(aiden.heartRate)
// 120
```

# I**nitializer Requirements**

: 프로토콜에서는 이니셜라이저도 정의할 수 있다.

- **실패가능한 이니셜라이저도 선언할 수 있다.**

```swift
protocol SomeProtocol {
  init(someParameter: Int)
}
```

- 프로토콜에서 특정 이니셜라이저가 필요하다고 명시했기 때문에 구현할때 해당 이니셜라이저에 **required** 키워드를 붙여줘야 한다.

```swift
class SomeClass: SomeProtocol {
  required init(someParameter: Int) {
    // 구현부
  }
}
```

- 특정 프로토콜의 required 이니셜라이저를 구현하고, SuperClass의 이니셜라이저를 SubClass에 상속하는 경우 **SubClass의 이니셜라이저 앞에 required 키워드와 override 키워드를 붙여줘야 한다.**

```swift
protocol SomeProtocol {
  init()
}class SomeSuperClass {
  init() {
    // 구현부
  }
}class SomeSubClass: SomeSuperClass, SomeProtocol {
  required override init() {
    // 구현부
  }
}
```

# **Delegation(위임)**

: Delegation이란 클래스나 구조체의 **인스턴스에 특정 행위에 대한 책임을 넘기는** 디자인 패턴 중 하나이다.

- Delegation된 기능을 제공할 수 있도록 **Delegation된 책임을 캡슐화하는 프로토콜을 정의하는것으로 구현**
- Delegation은 특정 액션에 응답하거나 해당 소스의 기본 타입을 알 필요 없이 외부 소스에서 데이터를 검색하는 데 사용할 수 있다.

> 아래의 두 코드는 Delegation의 예시로 내부 코드는 자세하게 볼 필요없다. (주석이 달린 부분만 봐도 된다.)
> 
- 첫번째 코드는 RandomNumberGenerator 프로토콜을 채택하는 LinearCongruentialGenerator 클래스(난수를 생성하는 클래스)와 Dice라는 타입을 정의해놓은 코드이다. (완벽한 코드 구현을 위해 정의한 것으로 그냥 넘어가도 된다.)

```swift
protocol RandomNumberGenerator {
    func random() -> Double
}

class LinearCongruentialGenerator: RandomNumberGenerator {
    var lastRandom = 42.0
    let m = 139968.0
    let a = 3877.0
    let c = 29573.0
    func random() -> Double {
        lastRandom = ((lastRandom * a + c).truncatingRemainder(dividingBy:m))
        return lastRandom / m
    }
}

class Dice {
    let sides: Int
    let generator: RandomNumberGenerator
    init(sides: Int, generator: RandomNumberGenerator) {
        self.sides = sides
        self.generator = generator
    }
    func roll() -> Int {
        return Int(generator.random() * Double(sides)) + 1
    }
}
```

- Delegation에 대한 설명은 아래 코드부터
- 프로토콜에 **AnyObject**를 추가하면 **클래스에서만 해당 프로토콜을 채택**할 수 있다.(**Class-Only Protocol**이 된다.)

```swift
protocol DiceGame {
    var dice: Dice { get }
    func play()
}

/// DiceGame의 책임을 DiceGameDelegate를 따르는 인스턴스에 위임
protocol DiceGameDelegate: AnyObject { // AnyObject로 선언하면 클래스만 이 프로토콜을 채택할 수 있는 속성이 된다.
    func gameDidStart(_ game: DiceGame)
    func game(_ game: DiceGame, didStartNewTurnWithDiceRoll diceRoll: Int)
    func gameDidEnd(_ game: DiceGame)
}
```

- DiceGameDelegate 프로토콜을 사용하여 **DiceGame의 진행 상황을 추적 할 수 있다.**
- 아래 코드는 DiceGame 프로토콜을 채택하며, DiceGameDelegate에게 진행상황을 알리는 코드이다.

```swift
class SnakesAndLadders: DiceGame { // 프로토콜의 조건에 맞추기위해 dice라는 프로퍼티는 gettable하게 구현되어 있고, play()메소드가 구현되어 있다.
    let finalSquare = 25
    let dice = Dice(sides: 6, generator: LinearCongruentialGenerator())
    var square = 0
    var board: [Int]
    init() {
        board = Array(repeating: 0, count: finalSquare + 1)
        board[03] = +08; board[06] = +11; board[09] = +09; board[10] = +02
        board[14] = -10; board[19] = -11; board[22] = -02; board[24] = -08
    }
    // 강한 참조 순환을 막기위해 약한 참조를 함
    weak var delegate: DiceGameDelegate? // 게임 진행에 반드시 필요한 것은 아니기 때문에 옵셔널로 정의
    /// 게임의 전체 로직이 들어있는 메소드
    func play() { // 바로 위에 정의한 delegate가 DiceGameDelegation 옵셔널타입이므로 play() 메소드는 delegate의 메소드를 호출할때마다 옵셔널 체이닝을 한다.
        square = 0
        /// DiceGameDelegation의 게임 진행상황을 tracking하는 메소드(게임 시작)
        delegate?.gameDidStart(self)
        gameLoop: while square != finalSquare {
            let diceRoll = dice.roll()
            /// DiceGameDelegation의 게임 진행상황을 tracking하는 메소드(게임 진행)
            delegate?.game(self, didStartNewTurnWithDiceRoll: diceRoll)
            switch square + diceRoll {
            case finalSquare:
                break gameLoop
            case let newSquare where newSquare > finalSquare:
                continue gameLoop
            default:
                square += diceRoll
                square += board[square]
            }
        }
        /// DiceGameDelegation의 게임 진행상황을 tracking하는 메소드(게임 종료)
        delegate?.gameDidEnd(self)
    }
}
```

• 아래 코드는 DiceGameDelegate 프로토콜을 채택한 DiceGameTracker클래스에 대한 코드이다.

```swift
class DiceGameTracker: DiceGameDelegate { // 프로토콜의 조건에 맞추기위해 3개의 메소드가 구현되어 있다.
    var numberOfTurns = 0
    func gameDidStart(_ game: DiceGame) {
        numberOfTurns = 0
        if game is SnakesAndLadders { // SnakesAndLadders 클래스의 인스턴스가 매개변수로 들어오면 실행
            print("Started a new game of Snakes and Ladders")
        }
        print("The game is using a \(game.dice.sides)-sided dice")
    }
    func game(_ game: DiceGame, didStartNewTurnWithDiceRoll diceRoll: Int) {
        numberOfTurns += 1
        print("Rolled a \(diceRoll)")
    }
    func gameDidEnd(_ game: DiceGame) {
        print("The game lasted for \(numberOfTurns) turns")
    }
}
```

- 실행 결과

```
lettracker =DiceGameTracker()
letgame =SnakesAndLadders()
game.delegate =tracker
game.play()// Started a new game of Snakes and Ladders
// The game is using a 6-sided dice
// Rolled a 3
// Rolled a 5
// Rolled a 4
// Rolled a 5
// The game lasted for 4 turns
```

# **Extension**

: **타입에 기능을 추가하기 위해 사용하는 문법**으로 기존의 타입에 **새로운 프로토콜을 채택하기 위해 Extension을 사용할 수도 있다.**

- 상속 역시 기존 타입에서 확장할 수 있지만, 새로운 클래스를 만들면서 기능을 확장한다는 차이점이 있다.
- Extensions은 그냥 기능을 덛붙힌다고 보면 된다. (**수평적 확장**)
- 기능들을 모음으로써 가독성을 높힐 수 있다.
- **프로퍼티는 연산프로퍼티만 사용할 수 있다.**
- 기본 형태

```
extension SomeType {
 // 구현부
}
```

- 기본 형태 (새로운 프로토콜 채택)

```
extension SomeType: SomeProtocol {
 // 구현부
}
```

- 예시

```
extension String {
 var length: Int {
  var string: NSString = NSString(string: self)
  return sting.length
 }
}
```

## **Extension에 조건 추가**

: **where**를 사용하여 특정 조건을 만족시킬때만 기능을 확장시키거나 프로토콜을 채택하도록 제한할 수 있다.

- 기본 형태 (해당 코드는 배열의 원소가 Int 타입일때만 Extension 됨)

```
extension Array: SomeProtocol where Element: Int {
 // 구현부
}
```

## **Extension을 사용한 프로토콜 채택 선언**

: 타입이 이미 프로토콜의 모든 요구사항을 만족하지만 아직 해당 프로토콜을 채택한다고 명시하지 않은 경우 형식에 빈 Extension을 사용해서 프로토콜을 채택하도록 할 수 있다.

- 예시

```
struct Hamster {
  var name: String
  var textualDescription: String {
    return "A hamster named \(name)"
  }
}extension Hamster: TextRepresentable {}
```

# **프로토콜 상속(Protocol Inheritance)**

: 클래스 상속과 같이 프로토콜도 상속할 수 있다. 마찬가지로 “,”로 구분한다.

```swift
protocol Movable {
    func go(to destination: String)
}

protocol OnBoardable {
    var numberOfPassangers: Int { get }
}

protocol Vehicle: Movable, OnBoardable { }
// typealias Vehicle = Movable & OnBoardable  <- 위와 같은 효과
struct Car: Vehicle {
    func go(to destination: String) {
        print("\(destination)(으)로 갑니다")
    }
    
    var numberOfPassangers: Int = 4
}

var car = Car(numberOfPassangers: 9)
car.go(to: "집") // 집(으)로 갑니다
print(car.numberOfPassangers) // 9
```

# **프로토콜 합성(Protocol Composition)**

: 동시에 여러 프로토콜을 따르는 타입을 선언할 수 있다.

- 예시

```
protocol Named {
  var name: String { get }
}protocol Aged {
  var age: Int { get }
}struct Person: Named, Aged {
  var name: String = "Aiden"
  var age: Int = 27
}
```

# **프로토콜 타입 확인**

: 일반적인 타입 확인과 마찬가지로 is, as를 사용한다.

- **is :** 앞에있는 타입이 뒤에있는 프로토콜을 채택하고 있는지 확인 (반환타입 Bool)
- **as? :** 앞에있는 타입이 뒤에있는 프로토콜을 채택하고 있는 경우 해당 타입을 프로토콜 타입으로 다운케스트, 그렇지 않은 경우는 nil 반환
- **as! :** 앞에있는 타입을 뒤에있는 프로토콜 타입으로 다운캐스트 실패시 런타임 에러 발생

# **Optional Protocol Requirements**

: 프로토콜을 선언하면서 필수 구현이 아닌 선택적 구현 조건을 정의할 수 있다.

## **@objc를 사용한 방법**

- **Objective-C를 사용한 방법**이다.
- **@objc** 키워드를 프로토콜 앞에 붙이고, 메소드나 프로퍼티에는 **@objc와 optional**키워드를 붙인다.
    - 따라서 채택한 곳에서 선언 했는지 안 했는지 알 수 없기 때문에 아래와 같은 경우 [Person.name](http://Person.name) → String? 의 자료형 가짐
- **@objc 프로토콜은 클래스만 채택할 수 있다.** (구조체와 열거형은 채택 불가)
- 따라서 자동으로 AnyObject가 채택된다고 볼 수 있음
- 예시

```swift
@objc protocol Person {
  @objc optional var name: String {get}
  @objc optional func speak()
}

class Aiden: Person {
  func notChoice() {
    print("name, speak()를 사용하지 않았습니다.")
  }
}

let aiden = Aiden()
aiden.notChoice() // name, speak()를 사용하지 않았습니다.
```

- Person을 채택하면서 구현은 하나도 하지 않는 클래스를 선언할 수는 있지만 권장하지 않는다.

> @objc의 단점
> 
> 
> *: @objc의 단점은 사용자가 정의한 타입을 사용할 수 없다는 것이다.*
> 

```
struct CustomType {
  var property: String = "특수타입"
}@objc protocol Person {
@objc optional var name: CustomType {get}
@objc optional func speak()
}
```

- 이를 해결하기 위한 방법은 사용자가 정의한 타입이 **NSObject를 상속**받아야 하는데 이 방법은 클래스만 가능하다.

```swift
// NSObject를 상속받아야 해서 클래스만 가능
class CustomType: NSObject {
    var property: String = "특수타입"
}

@objc protocol Person {
    @objc optional var name: CustomType {get}
    @objc optional func speak()
}

class Aiden: Person {
    func notChoice() {
        print("name, speak()를 사용하지 않았습니다.")
    }
}

let aiden = Aiden()
aiden.notChoice()
```

## **Extension을 활용한 방법**

: @objc의 단점을 해결하는 또다른 방법으로, 프로토콜을 정의한 후 구현하고 싶은 기능만 확장해서 이를 채택하게 하는 방법이있다.

- 해당 프로토콜을 채택하는 타입들이 기능들을 따로 구현하지 않고, **기능이 구현된 상태로 그대로 받길 원할 때**도 사용

```swift
struct CustomType {
    var name: String
}

protocol Person {
    var specialPeople: CustomType {get}
    func speak()
    func sleep()
}

extension Person {
    func speak() {
        print(specialPeople.name+"이 말합니다.")
    }
}

class Aiden: Person { // name과 sleep() 메소드만 구현하면 된다.
    var specialPeople: CustomType = CustomType(property: "Aiden")
    func sleep() {
        print(specialPeople.name+"잡니다.zzz")
    }
}

let aiden = Aiden()
aiden.speak() // Aiden이 말합니다.
aiden.sleep() // Aiden잡니다.zzz
```

- 이 방법을 사용하면 사용자정의 타입을 사용할 수 있다.
- 이미 Person의 speak() 메소드가 구현되었으므로 Person을 채택하는 Aiden 클래스는 **specialPeaple과 sleep()만 구현하면 된다.**
- speak()메소드가 기능이 없게 놔두고 싶다면 Extension을 사용하고 메소드를 **빈 구현체로 구현**하면된다.

> @objc를 사용한 방법과 Extension을 활용한 방법은 각각의 장단점이 있는것 같다. 먼저 Extension을 활용한 방법에서 염려되었던 부분은
> 
> 
> ***1. Extension하고 메소드를 빈 구현체로 구현하면 기능은 없지만 그 메소드는 남아있다는 점.***
> 
> ***2. return타입을 가지는 경우 반드시 return값을 정해줘야 하기에 메모리 낭비가 생길 수 있다는 점.***
> 
> *이 두가지가 마음에 걸렸다.*
> 
> *@objc를 사용한 방법에서 염려되었던 부분은*
> 
> ***1. class만 채택할 수 있다는 점.***
> 
> ***2. 사용자정의 타입도 클래스로만 정의 가능하다는 점.***
> 
> *만약 프로토콜을 채택하는 타입이 클래스고 사용자 정의 타입이 없거나 클래스로 정의할 타입이라면 @objc를 사용하는것이 좋은 것 같고, 그 외에는 Extension을 활용한 방법이 좋은 것 같다.(필자의 생각)*
> 

두 가지 질문

1. Swift의 String 타입은 BidirectionalCollection 프로토콜을 채택하여, +- 연산을 미지원하여 인덱스의 n 만큼 떨어진 글자의 참조를 위해선 O(n)의 시간복잡도가 필요한데, 이의 성능을 높일 수 있는 방법은 무엇이 있겠는가?
2. 왜 static 메소드는 인스턴스 멤버(인스턴스 변수, 인스턴스 메소드)를 사용할 수 없는가?