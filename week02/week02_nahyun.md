# 질문 리스트
> **_ino님의 질문_**
- String은 왜 subscript로 접근이 안되는지 설명하시오.
- instance 메서드와 class 메서드의 차이점을 설명하시오.

> _**hyunryeol님의 질문**_
- Delegate 패턴을 활용하는 경우를 예를 들어 설명하시오.
- 프로토콜이란 무엇인지 설명하시오.

## String은 왜 subscript로 접근이 안되는지 설명하시오.

subscript : 클래스, 구조체, 열거형과 같은 사용자 정의 타입 내에서 인덱스나 키를 사용하여 값을 읽거나 쓸 수 있게 하는 특별한 멤버. 
배열, 딕셔너리, 문자열과 같은 내장 데이터 타입에서 자주 사용되는 개념.
subscript를 사용하면 해당 타입을 사용하는 코드가 더 직관적이고 편리하게 데이터를 접근하고 수정할 수 있음.

```
subscript(index: Int) -> ElementType {
    get {
        // 해당 인덱스에 대한 값을 반환
    }
    set(newValue) {
        // 해당 인덱스에 값을 설정
    }
}

```

#### 그렇다면 왜 subscript로 String에 접근할 수 없는가?

**유니코드를 지원하기 때문**에 문자열 내에는 각 문자의 코드 포인트가 순차적으로 저장되며, 이는 모든 문자가 고정 크기가 아니라 다양한 길이를 가질 수 있음을 의미.
각 문자에 대한 접근을 간단하게 '[index]'로 하게 되면 모든 경우를 다루기 어려움. 

그러므로 문자열 인덱스를 사용해야함.

```
//String 문자열의 특정 위치에 있는 문자에 접근하는 예시
let str = "Hello, Swift!"

// 문자열 인덱스를 사용하여 접근
let index = str.index(str.startIndex, offsetBy: 7)
let char = str[index] // 이제 char에는 "S"가 저장됩니다.

```

> - startIndex: 문자열의 시작 위치를 가리키는 인덱스. 문자열의 첫 번째 문자를 나타냄.
- endIndex: 문자열의 끝을 나타내는 인덱스. 문자열의 마지막 다음 위치를 가리킴.
- offsetBy(_:): offsetBy 메서드를 사용하여 현재 인덱스에서 상대적인 위치로 이동할 수 있음. 이 메서드는 정수를 받아 해당 만큼 인덱스를 이동.

## instance 메서드와 class 메서드의 차이점을 설명하시오.
인스턴스 메서드와 클래스 메서드는 호출 방식에서 차이가 있음.

> #### instance 메서드
- 클래스, 구조체, 또는 열거형의 인스턴스에 속하는 메서드.
- 해당 타입의 인스턴스를 생성한 후에만 호출할 수 있음.
- 인스턴스 메서드 내에서는 인스턴스의 속성에 접근할 수 있으며, 인스턴스를 변경하거나 다양한 작업을 수행하는 데 사용.

> #### Class 메서드
- 클래스 메서드는 클래스 자체에 속하는 메서드로, 특정 인스턴스와는 독립적으로 작동.
- 인스턴스를 생성하지 않고도 클래스 메서드를 직접 호출할 수 있음.
- 주로 클래스와 관련된 유틸리티 메서드나 공유 동작을 수행하는 데 사용.

### 질문 
instance메서드와 class메서드로 둘 다 구현이 가능한 경우 어떤 것을 사용하는 것이 좋은가?

## Delegate 패턴을 활용하는 경우를 예를 들어 설명하시오.
#### Delegate 
Protocol을 이용해 권한을 위임하고 일을 처리하는 방식의 디자인 패턴으로 클래스나 구조체의 인스턴스에 특정 행위에 대한 책임을 다른 타입의 인스턴스에게 넘기는 방식.
```
class Me {
    weak var delegate: Brother?

    func 올때메로나() {
        delegate?.buyIcecream()
    }
}

class Brother {
    let me = Me()

		init() {
        me.delegate = self
    }

    func buyIcecream() {
        print("메로나를 샀다")
    }
}

```
Me는 아이스크림을 먹고 싶다는 이벤트를 받았지만 내가 사러 가기 싫기 때문에 다른 객체, Brother에게 아이스크림을 사는 행위를 위임함. Me가 이벤트를 받아 올때메로나를 실행시켜 이벤트를 처리하면, 실질적으로는 델리게이트인 Brother가 대신 일을 처리해줌. 
```
protocol MeDelegate: AnyObject {
    func buyIcecream()
}

class Me {
    weak var delegate: Brother?

    func 올때메로나() {
        delegate?.buyIcecream()
    }
}

class Brother: MeDelegate {
    let me = Me()

		init() {
        me.delegate = self
    }

    func buyIcecream() {
        print("메로나를 샀다")
    }
}

class Father: MeDelegate {
    let me = Me()

		init() {
        me.delegate = self
    }

    func buyIcecream() {
        print("메로나를 샀다")
    }
}

```
MeProtocol을 만들어놓으면 Me는 MeProtocol을 채택한 누구에게든 아이스크림을 사는 행위를 위임할 수 있음.
남동생뿐만 아니라 아빠에게도 아이스크림을 사오라고 시킬 수 있음.

Delegate 패턴은 객체 간의 효과적인 통신, 재사용성, 코드의 단순화, 이벤트 처리 및 플러그인 아키텍처 등 다양한 이점을 제공하며, 이로 인해 코드의 모듈화와 확장성을 향상시키는 데 도움을 줌.

## 프로토콜이란 무엇인지 설명하시오.

#### Protocol
타입으로 classsk struct의 행동(func)을 정의하는 역할. (다른 언어에서는 인터페이스, 추상 클래스와 유사)
행동을 정의할 뿐 구현은 하지 않음.
> - 추상화(Abstraction)
: 프로토콜은 특정 동작이나 인터페이스에 대한 추상화를 제공. 즉, 어떤 타입이 특정한 메서드나 프로퍼티를 구현해야 한다는 명세를 정의.
- 다형성(Polymorphism)
: 프로토콜을 채택한 다양한 타입들은 동일한 프로토콜을 준수하므로, 다형성을 구현할 수 있음. 이를 통해 코드의 재사용성과 유연성을 높일 수 있음.
- 코드 재사용과 확장성
: 프로토콜을 사용하면 동일한 인터페이스를 가진 여러 타입에서 같은 동작을 수행할 수 있으므로 코드를 재사용할 수 있음. 또한 새로운 타입이 프로토콜을 채택함으로써 기존 코드를 확장할 수 있음.
- 프로토콜 지향 프로그래밍(Protocol-Oriented Programming, POP)
: Swift에서 프로토콜은 객체 지향 프로그래밍뿐만 아니라 프로토콜 지향 프로그래밍의 핵심 개념. 프로토콜을 통해 타입을 추상화하고 프로토콜 확장을 사용하여 기본 구현을 제공하는 방식으로 코드를 작성하는 POP는 Swift의 강력한 기능 중 하나.

```
// 약속
/// **delegate
/// **able, **ing,

// 프로토콜로 약속
protocol Naming {
    // 우리는 이런 변수를 가지고 있을겁니다 라고 약속
    var name : String { get set }
    // 우리는 이런 메소드를 가지고 있을겁니다 라고 약속
    func getName() -> String
}

//
struct Friend : Naming{
    
    var name: String
    
    func getName() -> String {
        return "내 친구: " + self.name
    }
}

var myFriend = Friend(name: "쩡대리")

myFriend.getName()
```
### 질문
프로토콜을 사용하지 않고 비슷하게 구현할 수 있는 방법이 있는가?
