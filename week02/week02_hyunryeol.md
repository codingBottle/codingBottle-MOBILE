## Delegate 패턴을 활용하는 경우를 예를 들어 설명하시오.

### Delegate 패턴이란?
> 객체지향 프로그램에서 하나의 객체가 모든 일을 처리하는 것이 아닌
> 해야하는 일 중에 일부를 다른 객체에게 위임하는 것

### 예시 코드
```swift
// 1. 프로토콜 정의
protocol ButtonDelegate: class {
    func buttonTapped()
}

// 2. 버튼 뷰 정의
struct ButtonView: View {
    weak var delegate: ButtonDelegate?
    
    var body: some View {
        Button("Tap Me") {
            // 3. 버튼 클릭 시 델리게이트 호출
            delegate?.buttonTapped()
        }
    }
}

// 4. 컨텐츠 뷰 정의
struct ContentView: View, ButtonDelegate {
    var body: some View {
        VStack {
            Text("Hello, SwiftUI!")
            ButtonView(delegate: self)
        }
    }
    
    // 5. 델리게이트 메서드 구현
    func buttonTapped() {
        print("Button tapped!")
    }
}
```
<hr>

## 프로토콜이란 무엇인지 설명하시오.
>_프로토콜 (protocol)_ 은 메서드, 프로퍼티, 그리고 특정 작업이나 기능의 부분이 적합한 다른 요구사항의 청사진을 정의합니다. 프로토콜은 요구사항의 구현을 제공하기 위해 클래스, 구조체, 또는 열거형에 의해 채택될 수 있습니다. 프로토콜의 요구사항에 충족하는 모든 타입은 프로토콜에 _준수 (conform)_ 한다고 합니다.

### 프로토콜 구문 (Protocol Syntax)
> 클래스, 구조체, 그리고 열거형과 유사한 방법으로 프로토콜을 정의합니다
```swift
protocol SomeProtocol {
    // protocol definition goes here
}
```

#### 프로토콜 채택방법
> 콤마로 분리하여 여러 프로토콜도 채택할 수 있다
```swift
struct SomeStructure: FirstProtocol{
// structure definition goes here
}
struct SomeStructure: FirstProtocol, AnotherProtocol {
    // structure definition goes here
}
```

### 프로퍼티 요구사항 (Property Requirements)
> 인스턴스 프로퍼티 또는 타입 프로퍼티를 제공하기 위해 모든 준수하는 타입을 요구할 수 있습니다
	프로토콜은 각 프로퍼티가 gettable 인지 gettable과 settable 인지도 지정해줘야 합니다.
	gettable과 settable 프로퍼티는{ get set }으로 작성gettable 프로퍼티는{ get }으로 작성
```swift
protocol SomeProtocol {
    var mustBeSettable: Int { get set }
    var doesNotNeedToBeSettable: Int { get }
}
```

타입 프로퍼티 요구하려면 프로토콜에 정의할 때 `static` 키워드를 접두사로 둡니다.
```swift
protocol AnotherProtocol {
    static var someTypeProperty: Int { get set }
}
```

>Protocol Property의 정의와 사용 예시
```swift
protocol FullyNamed {
    var fullName: String { get }
}//프로토콜 선언
struct Person: FullyNamed {
    var fullName: String
}//struct에서 FullyNamed프로토콜 채택
let john = Person(fullName: "John Appleseed")
// john.fullName is "John Appleseed"
```
<hr>

## String은 왜 subscript로 접근이 안되는지 설명하시오

### Subscript란?
> `subscript`는 Swift에서 특정 컬렉션 내의 요소에 접근하고 수정하는 특별한 메서드입니다. 이 메서드를 사용하면 컬렉션 내의 요소를 다룰 때 인덱스 대신 편리하게 사용할 수 있습니다. `subscript`를 정의하면 해당 컬렉션을 마치 배열처럼 `[ ]` 괄호를 사용하여 요소에 접근할 수 있습니다.

### String에 subscript로 접근이 안되는 이유
>  Swift의 `String`은 유니코드 문자를 처리하는데 최적화되어 있습니다.
>  일부 문자는 하나의 코드 포인트로 표현되고 다른 문자는 여러 개의 코드 포인트로 표현될 수 있기 때문입니다.
- 예시
  - `a`는 1개의 코드 포인트로 표현되지만 `🚀`는 1개의 코드 포인트가 아니라 2개의 코드 포인트로 표현됩니다.

### 문자열 인덱스에 접근하는 방법?
> Swift에서는 String에 subscript를 사용하는 것 대신
> `startIndex`과 `endIndex` 속성을 사용하여 문자열의 시작과 끝을 나타내고, `String.Index`를 사용하여 문자열 내에서 이동하거나 부분 문자열을 추출할 수 있습니다.

- 예시코드
```swift
let s = "12:00:00AM"
let timeIndex = s.index(s.startIndex, offsetBy: 7)
let timeStr = String(s[...timeIndex]) // 12:00:00
let ampmIndex = s.index(s.endIndex, offsetBy: -2)
let ampmStr = String(s[ampmIndex...]) // AM

let timeStr2 = s.prefix(8) // 12:00:00
let ampmStr2 = s.suffix(2) // AM
```
<hr>

## instance 매서드와 class매서드의 차이점을 설명하시오
### instance Method
> 인스턴스 메서드는 `클래스, 구조체, 열거형` 등의 인스턴스(객체)에 속하는 메서드입니다. 인스턴스를 생성한 후에 해당 인스턴스에 대해 호출할 수 있습니다. 이 메서드는 해당 인스턴스의 상태를 조작하거나 해당 인스턴스에 특화된 작업을 수행하는 데 사용됩니다.
- 선언 방법
	인스턴스 메서드는 일반적인 함수와 유사하게 선언됩니다. 다만 해당 메서드가 어떤 클래스 또는 구조체의 인스턴스 메서드 인지를 선언 앞에 표시해야 합니다.
- 예시 코드
```swift
class MyClass {
    var value: Int
    init(value: Int) {
        self.value = value
    }
    // 인스턴스 메서드 선언
    func instanceMethod() {
        // 해당 인스턴스의 상태에 접근 가능
        print("This is an instance method. Value: \(self.value)")
    }
}
// 클래스의 인스턴스 생성 
let myInstance = MyClass(value: 42) 
// 인스턴스 메서드 호출
myInstance.instanceMethod()// 출력: This is an instance method. Value: 42
```
### Class Method
> 클래스 메서드는 클래스 자체에 속하는 메서드로, 클래스의 인스턴스를 생성하지 않아도 클래스 이름을 통해 직접 호출할 수 있습니다. 이 메서드는 클래스 수준의 작업을 수행하는 데 사용됩니다. 인스턴스와 관계 없이 클래스 자체의 동작을 구현할 때 유용합니다.
- 선언 방법
	클래스 메서드는 `class` 또는 `static` 키워드 를 사용하여 선언합니다. 두 키워드 중 하나를 선택하여 사용할 수 있습니다.
- 예시 코드
```swift
class MyClass {
    static var classValue: Int = 0
    
    // 클래스 메서드 선언
    class func classMethod() {
        // 클래스 변수에 접근 가능
        print("This is a class method. Class Value: \(classValue)")
    }
    
    // static 키워드를 사용한 클래스 메서드 선언
    static func anotherClassMethod() {
        print("This is another class method.")
    }
}
// 클래스 메서드 호출 (인스턴스 없이 직접 호출)
MyClass.classMethod() // 출력: This is a class method.
```
