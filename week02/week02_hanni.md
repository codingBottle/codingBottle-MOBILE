# Delegate 패턴을 활용하는 경우를 예를 들어 설명하시오.
## **Delegate 패턴이란**

델리게이트(delegate)는 한 객체가 다른 객체로 동작을 위임하거나, 다른 객체의 이벤트에 대한 처리를 위임하는 디자인 패턴이며, 주로 객체 간의 통신 및 이벤트 처리에 사용된다.

델리게이트는 일반적으로 프로토콜(Protocol)을 사용하여 정의하며, 델리게이트를 채택한 객체는 델리게이트 프로토콜에 정의된 메서드를 구현해야 합니다.

예를 들어, UITableView는 데이터 소스와 델리게이트를 사용하여 테이블 뷰의 데이터를 관리하고 사용자 상호작용에 대한 이벤트를 처리한다.

데이터 소스는 테이블 뷰에 표시할 데이터를 제공하고, 델리게이트는 사용자의 선택 등 다양한 이벤트를 처리합니다.

흠 … 내가 이해한 것을 바탕으로 쉽게 정리하자면..

delegate는 `위임` 이라는 뜻을 가지고 있다.

테이블 뷰라는 것이 있으면 테이블을 그리는 뷰 / 테이블의 데이터 이렇게 있다.

그러면 delegate는 데이터와 뷰 사이의 통신과 데이터 관리를 위임하는 것이다.

아직 잘 안 와 닿아 `위임` 의 의미를 알아보았다. 

즉, 테이블 뷰의 데이터 소스에게 데이터를 제공하고, 델리게이트에게 사용자의 터치나 이벤트 처리를 위임한다는 것이다. 

다시 말해, 데이터 소스와 델리게이트는 해당 작업을 대신 수행하도록 위임받은 역할을 한다. 

예를 들어, 데이터 소스는 테이블 뷰에 어떤 데이터를 표시할지 위임받아 결정하고, 델리게이트는 사용자가 셀을 선택할 때 어떤 동작을 실행할지를 위임받아 처리한다.

- code 가 첨부된 설명! (이것을 보면 더 쉽게 이해할 수 있다.)
    
    ```swift
    import UIKit
    
    class MyTableViewController: UIViewController, UITableViewDataSource, UITableViewDelegate {
        
        @IBOutlet weak var tableView: UITableView!
        
        var data = ["항목 1", "항목 2", "항목 3"]
        
        override func viewDidLoad() {
            super.viewDidLoad()
            
            // 테이블 뷰의 데이터 소스와 델리게이트를 현재 뷰 컨트롤러로 설정
            tableView.dataSource = self
            tableView.delegate = self
        }
        
        // UITableViewDataSource 프로토콜 구현
        func tableView(_ tableView: UITableView, numberOfRowsInSection section: Int) -> Int {
            return data.count
        }
        
        func tableView(_ tableView: UITableView, cellForRowAt indexPath: IndexPath) -> UITableViewCell {
            let cell = tableView.dequeueReusableCell(withIdentifier: "cell", for: indexPath)
            cell.textLabel?.text = data[indexPath.row]
            return cell
        }
        
        // UITableViewDelegate 프로토콜 구현
        func tableView(_ tableView: UITableView, didSelectRowAt indexPath: IndexPath) {
            // 특정 항목을 선택했을 때 처리할 로직
            print("선택된 항목: \(data[indexPath.row])")
        }
    }
    ```
    
    위 코드에서 `MyTableViewController`는 `UITableViewDelegate`와 `UITableViewDataSource` 프로토콜을 채택하고 있다. 
    
    이 객체는 데이터 관리 및 테이블 뷰의 동작을 위임받은 역할을 한다.
    
    - `viewDidLoad` 에서 `tableView` 의 데이터 소스와 델리게이트를 현재 뷰 컨트롤러로 설정한다.
    - `numberOfRowsInSection`과 `cellForRowAt` 메서드는 데이터 소스 역할로, 테이블 뷰의 행 수와 각 셀을 어떻게 구성할 지를 정의한다.
    - `didSelectRowAt` 메서드는 델리게이트 역할로, 사용자가 특정 행을 선택했을 때 수행할 로직을 정의한다.

### 궁금한 질문들

- delegate 패턴은 모든 ios 앱을 만들때 기본적으로 사용하는가?
    - delegate 패턴은 모든 iOS 앱을 만들 때 기본적으로 사용되는 것은 아니다.
    - 그러나 UIKit과 같은 iOS 프레임워크에서 자주 사용되며, 특히 인터페이스 구성 요소와의 상호작용에서 많이 활용된다.
        - 인터페이스 구성요소와의 상호작용이란 무엇인가?
            - **인터페이스 구성 요소**
                - iOS 앱에서 화면에 보이는 모든 것, 예를 들면 버튼, 텍스트 필드, 테이블 뷰 등을 가리키며 이러한 요소들은 사용자와 앱 간의 상호작용을 담당한다.
            - **Delegate 패턴과 상호작용**
                - Delegate 패턴은 주로 이러한 인터페이스 구성 요소와 상호작용하기 위해 사용된다.
            - **상호작용 예시**
                - 가령 테이블 뷰에서 사용자가 셀을 탭할 때, 해당 셀을 감지하고 선택한 항목에 대한 작업을 수행하려면 테이블 뷰의 델리게이트(delegate)를 설정하여 이벤트를 처리할 수 있다. 이를 통해 사용자와 앱 간의 상호작용을 더욱 효과적으로 제어할 수 있다.
- delegate는 프로토콜을 사용해 정의하기 때문에 ios 프로젝트를 진행할 때 기본적인 패턴으로 삼고 있나?
    
    Delegate 패턴은 iOS 개발에서 일반적으로 사용되며, 프로토콜을 사용하여 객체 간 통신을 구현한다.
    
- delegate는 객체 간의 통신 및 이벤트 처리에 사용된다고 하는데 mvc, mvvm 패턴에서도delegate가 사용될 수 있나?
    
    MVC 및 MVVM 패턴에서도 delegate 패턴은 여전히 사용될 수 있다. 
    
    주로 컨트롤러나 뷰 모델이 뷰와 모델 사이의 통신을 관리할 때 delegate 패턴을 활용할 수 있다.
    
- delegate에서 객체가 설정이 되었는지 테스트 코드로 확인하는 방법
    
    ```swift
    protocol MyDelegate {
        func doSomething()
    }
    
    class MyObject {
        var delegate: MyDelegate?
    
        func performAction() {
            // delegate가 설정되었는지 확인하고, 설정되어 있을 경우 동작 수행
            delegate?.doSomething()
        }
    }
    
    // 테스트 코드
    class MyDelegateMock: MyDelegate {
        var didCallDoSomething = false
    
        func doSomething() {
            didCallDoSomething = true
        }
    }
    
    let myObject = MyObject()
    let delegateMock = MyDelegateMock()
    
    myObject.delegate = delegateMock
    myObject.performAction()
    
    // delegateMock의 didCallDoSomething 속성이 true인지 확인하여 테스트
    assert(delegateMock.didCallDoSomething)
    ```
    
    위 코드에서 `MyDelegateMock` 클래스는 실제 델리게이트 역할을 대신하며 `didCallDoSomething` 속성을 사용하여 `doSomething` 메서드가 호출되었는지 확인한다.
    

## **delegate의 필요성**

Delegate는 객체 간의 느슨한 결합을 가능하게 하며, 코드 재사용성과 모듈성을 높인다.

또한 이벤트 처리와 행동의 책임을 분리하여 코드를 더 간결하게 만들 수 있다.

델리게이트를 사용하면 다른 객체가 어떤 작업을 수행할지 결정할 수 있으므로 확장성이 좋다.

## **delegate의 장점 / 단점**

- **장점**
    - 느슨한 결합: 객체 간 상호작용이 느슨하게 결합되므로 유지보수가 용이하고 테스트하기 쉽다.
        - 느슨하다는 정의가 프로토콜이 채택을 사용하기 때문에 상속과는 상관이 없는 이런 맥락과 같다고 이해하면 될까?
            
            맞다. 느슨한 결합은 객체 간의 상호 의존성을 최소화하는 것을 의미하며, delegate 패턴은 상속과는 독립적으로 객체 간 통신을 허용한다. 이로써 객체 간 결합도를 낮추고 유지보수 및 테스트가 용이해진다.
            
    - 모듈성: 코드를 더 작은 모듈로 분리하여 개발 및 유지보수가 쉬워진다.
        - 모듈로 개발하는 것이 무엇인가?
            
            모듈로 개발한다는 것은 기능을 작은 단위로 분리하고 각 모듈이 독립적으로 동작할 수 있도록 하는 것을 의미한다. 
            
            예를 들어, 주문 처리와 결제 처리를 별도의 모듈로 분리하는 경우, 주문 처리 모듈과 결제 처리 모듈을 개별적으로 테스트하고 수정할 수 있으며, 코드의 재사용성과 유지보수성이 향상된다.
            
    - 다중 상속 문제 해결: 언어에서 다중 상속을 지원하지 않는 경우에 객체 간 기능 공유가 가능하다.
        - 이건 프로토콜이 상속이 아닌 채택을 하기 때문에 가능한 것 인가?
            
            맞다. Delegate 패턴에서는 클래스가 다른 클래스를 상속할 필요 없이 프로토콜을 채택함으로써 다른 객체의 기능을 확장할 수 있다. 이로써 다중 상속 문제를 우회하고 객체 간 기능 공유가 가능해진다.
            
- **단점**
    - 복잡성: Delegate 패턴을 과도하게 사용하면 코드가 복잡해질 수 있으며, 이해하기 어려울 수 있다.
        - 모듈화가 많아지면 복잡해진다고 이해해도 괜찮은가?
            
            Delegate 패턴을 과도하게 사용하면 코드의 복잡성이 증가할 수 있습니다. 
            
            모듈화가 많아질수록 각 모듈 간의 통신 및 의존성이 증가할 수 있으므로, 신중하게 사용해야 한다. 
            
            이를 관리하기 위해선 코드를 잘 구조화하고 명확한 프로토콜 정의가 필요하다.
            
    - 런타임 오류: 델리게이트 객체가 설정되지 않았을 때 런타임 오류가 발생할 수 있다.
    - 성능 문제: 델리게이트 호출이 많은 경우 성능 저하가 발생할 수 있다.

## **delegate의 대체제**

델리게이트 패턴의 대체제로는 클로저(Closure)나 옵저버 패턴(Observer Pattern) 등이 있다. 

선택은 상황과 요구사항에 따라 다를 수 있지만, 클로저를 사용하면 코드가 더 간결해지고, 옵저버 패턴을 활용하면 다수의 객체가 이벤트를 관찰하고 처리할 수 있다.


# 프로토콜이란 무엇인지 설명하시오.
## protocol이란?

Protocol의 정의는 협약, 통신 규약등인데 쉽게 말해 약속이다. 

프로토콜은 `타입` 으로 Swift의 class나 struct의 행동을 정의하는 역할을 한다. 

쉽게 말하자면 프로토콜은 프로퍼티를 선언하여 값을 직접 정의하고, 메서드를 직접 구현 하는 것이 아닌 어떤 기능을 수행할 때 필요한 `선언` 만 하는 것이다.

그 후 프로토콜을 채택한 곳(class나 struct 등)에서 실제 구현을 하게 되는 것이다.

프로토콜 안에 선언 되어 있는 프로퍼티와 메서드는 required 이다. (필수적이다.) 하지만 필수적으로 들어 있고 싶지 않은 것을 선언하고 싶은 경우에는 optional로 선언 할 수 있다. optional로 선언하게 되면 채택해주는 곳에서 꼭 선언해주지 않아도 에러가 나지 않는다.

- 하지만 optional로 선언된 프로퍼티는 sturct에서 사용할 수 없고, class에서 사용할 수 있다.
    
    **`@objc optional`**로 선언된 프로퍼티는 Objective-C와의 상호 운용성을 위한 것으로, 스위프트에서는 클래스(Class)에서만 사용할 수 있다. 스위프트의 구조체(struct)나 열거형(enum)은 Objective-C와의 호환성을 가지지 않으므로 **`@objc optional`**을 사용할 수 없다.
    
    따라서 **`@objc optional`**을 프로퍼티에 적용하고 구조체에서 사용하는 것은 스위프트에서는 직접적으로 지원되지 않는다. 스위프트에서 구조체 내의 프로퍼티는 일반적으로 옵셔널로 선언하거나 기본값을 제공하는 방식으로 옵셔널 값을 다루게 된다.
    

### 프로토콜은 1급 객체라고 하는데 “1급 객체”라는 것은 어떤 의미인가요?

Swift에선 함수, 클로저, 프로토콜이 1급 객체이다. 1급 객체란 어떠한 객체(object)가 독립적인 하나의 타입으로 사용될 수 있는 것을 말한다.

→ 프로토콜은 독립적인 하나의 타입이다.

예시 - 1)

축구 선수는 포지션에 따라 공격수, 수비수, 미드필더, 골기퍼 이렇게 나뉘고 경기를 같이 진행하게 된다.

이걸 프로그래밍적으로 보면 축구 경기를 할때 필요한 속성(공격수, 수비수, 미드필더, 골기퍼)은 `프로퍼티` 로 경기를 play 하는 행위는 `메서드` 로 만들 수 있게된다.

그럼 프로토콜은 포지션 별로 어떤 선수인지, 혹은 어떤 경기를 하는지 등을 실제로 구현하는 것이 아닌 `프로퍼티` 와 `메서드` 가 필요하고, 해당 기능에 필요한 요구 사항을 `선언해두는 것` 이다.

- code로 보는 예시
    
    ```swift
    // 축구 선수를 나타내는 프로토콜
    protocol SoccerPlayer {
        var name: String { get }
        var age: Int { get }
        var position: String { get }
    
        func play()
    }
    
    // 공격수 타입
    struct Forward: SoccerPlayer {
        var name: String
        var age: Int
        var position: String = "공격수"
        
        func play() {
            print("\(name)은(는) \(position)로 득점을 시도합니다!")
        }
    }
    
    // 수비수 타입
    struct Defender: SoccerPlayer {
        var name: String
        var age: Int
        var position: String = "수비수"
        
        func play() {
            print("\(name)은(는) \(position)로 수비에 잘 대응합니다.")
        }
    }
    
    // 미드필더 타입
    struct Midfielder: SoccerPlayer {
        var name: String
        var age: Int
        var position: String = "미드필더"
        
        func play() {
            print("\(name)은(는) \(position) 역할을 수행하고 패스를 연습합니다.")
        }
    }
    
    // 골키퍼 타입
    struct Goalkeeper: SoccerPlayer {
        var name: String
        var age: Int
        var position: String = "골키퍼"
        
        func play() {
            print("\(name)은(는) \(position)로 골을 막아냅니다.")
        }
    }
    
    // 축구 선수들의 배열
    let players: [SoccerPlayer] = [
        Forward(name: "메시", age: 34),
        Defender(name: "람프라드", age: 42),
        Midfielder(name: "이니에스타", age: 37),
        Goalkeeper(name: "버프폰", age: 43)
    ]
    
    // 모든 축구 선수들의 플레이 액션 실행
    for player in players {
        player.play()
    }
    ```
    

### 프로토콜에서 프로퍼티 선언하기

1. 프로토콜을 채택해서 선언된 프로퍼티를 구현할 경우, 저장 프로퍼티로 구현하든, 연산 프로퍼티로 구현하든 상관없다.
2. 프로토콜에 선언되는 프로퍼티는 항상 var로 선언되어야 한다.
    1. **`let`**으로 선언된 불변 프로퍼티는 값의 읽기만 가능하고 쓰기는 불가능하니 프로토콜을 채택해서 구현이 이루어져야하니 안되지 않을까?
3. {get set} 속성에 따라 달라지는 채택하는 곳의 let / var 속성
    1. `{get}` 으로 선언이 되어 있는 경우 
        1. 채택하는 곳이 저장 프로퍼티의 경우 let / var선언 모두 가능하고, 연산 프로퍼티의 경우 getter(get-only) / getter & setter 선언 모두 가능 하다.
            - code 예시
                
                ```swift
                protocol Band {
                    var piano: String { get }
                }
                
                // 상관없음!
                class ABand: Band {
                    let piano: String = "sodeul"          
                }
                 
                class BBand: Band {
                    var piano: String = "sodeul"          
                }
                
                // getter만 있던지 getter & setter 모두 만들던지 상관없음!
                class ABand: Band {
                    var random: String = ""
                    var piano: String {
                        get {
                            return "sodeul"
                        }
                    }
                }
                 
                class BBand: Band {
                    var random: String = ""
                    var piano: String {
                        get {
                            return "sodeul"
                        }
                        set {
                            self.random = newValue
                        }
                    }
                }
                ```
                
    2. `{get set}` 으로 선언이 되어 있는 경우
        1. 저장 프로퍼티의 경우 var로만 선언 가능하고, 연산 프로퍼티의 경우 getter & setter로 선언해주어야 한다.
            - code 예시
                
                ```swift
                protocol Band {
                    var piano: String { get set }
                }
                
                // 채택하는 곳은 무조건 var로만 선언할 수 있다.
                class ABand: Band {
                    let piano: String = "sodeul"  // error! Type 'ABand' does not conform to protocol 'Band'
                }
                 
                class BBand: Band {
                    var piano: String = "sodeul"          
                }
                
                // getter 만 선언하는 것이 아닌 getter & setter 둘 다 선언을 해주어야한다.
                class ABand: Band {     
                    var random: String = ""
                    var piano: String {      // error! Type 'ABand' does not conform to protocol 'Band'
                        get {
                            return "sodeul"
                        }
                    }
                }
                 
                class BBand: Band {
                    var random: String = ""
                    var piano: String {
                        get {
                            return "sodeul"
                        }
                        set {
                            self.random = newValue
                        }
                    }
                }
                ```
                

### 프로토콜에서 메서드 선언하기

1. 구조체의 경우, mutating이 필요하면 프로토콜 자체에 추가
    1. 프로토콜 메서드 자체에 mutating이 붙어 있는 경우, 구조체의 경우는 반드시 changeName이란 메서드를 선언할 때 mutating을 붙여야 하고, 클래스의 경우엔 mutating 키워드가 필요 없으니 떼고 선언해 주면 된다.
    - code 예시
        
        ```swift
        protocol Renameable {
            mutating func changeName(newName: String)
        }
         
        struct Sodeul: Renameable {
            var name: String = "sodeul"
            
            mutating func changeName(newName: String) {
                self.name = newName
            }
        }
         
        class DeulSo: Renameable {
            var name: String = "sodeul"
            
            func changeName(newName: String) {
                self.name = newName
            }
        }
        ```
        

## **Swift는 프로토콜 지향 프로그래밍 (POP)**

### 객체 지향 프로그래밍 (OOP)

- **클래스 중심**
    - OOP는 주로 클래스(Class)를 중심으로 데이터와 해당 데이터를 처리하는 메서드(함수)를 묶어서 관리한다. 객체(Object)는 클래스의 인스턴스이다.
- **상속**
    - OOP에서 클래스는 상속을 통해 다른 클래스로부터 속성과 메서드를 상속받을 수 있다. 이로 인해 코드 재사용이 가능하며, 부모 클래스와 자식 클래스 간에 계층 구조가 형성된다.
- **캡슐화와 정보 은닉**
    - OOP는 정보 은닉과 캡슐화를 강조한다. 클래스는 내부 상태를 숨기고 외부에서 접근 가능한 인터페이스를 제공한다. 이를 통해 코드의 안정성과 유지 보수성을 높일 수 있다.
- **다형성**
    - OOP에서 다형성은 하위 클래스의 객체를 상위 클래스 타입으로 다룰 수 있는 능력을 의미한다. 이는 메서드 오버라이딩과 연결돼 다양한 객체가 동일한 메서드 호출을 다르게 처리할 수 있게 한다.

### 프로토콜 지향 프로그래밍 (POP)

- **프로토콜 중심**
    - POP는 프로토콜(Protocol)을 중심으로 데이터와 동작을 추상화한다. 프로토콜은 메서드와 프로퍼티 요구사항을 정의하며, 다양한 타입이 이를 채택할 수 있다.
- **상속 대신 프로토콜 채택**
    - 프로토콜을 사용하여 다양한 타입이 공통적으로 사용할 수 있는 인터페이스를 정의하고, 클래스나 구조체는 상속 대신 프로토콜을 채택하여 이 인터페이스를 구현한다.
- **객체의 타입 중립성**
    - POP는 객체의 실제 타입보다는 어떤 프로토콜을 채택했는지에 중점을 둔다. 이로써 타입 간에 더 자유로운 교류가 가능하며, 다양한 객체를 더 쉽게 조합할 수 있다.
- **확장성**
    - 프로토콜은 타입 간에 유연한 협력을 가능하게 하며, 새로운 기능을 추가하기 쉽다. 코드 확장성이 높아 유지 보수가 쉬워진다.
- **데이터 형식 독립성**
    - POP는 클래스뿐만 아니라 구조체, 열거형 등 다양한 데이터 형식에서 프로토콜을 채택할 수 있어 데이터 형식 독립성을 제공한다.

## **그럼 객체지향과 프로토콜지향의 차이점을 알고 계신가요?**

OOP는 클래스 계층 구조와 상속을 강조하며 객체의 내부 상태를 캡슐화한다. 

반면 POP는 프로토콜을 중심으로 타입 간에 더 자유로운 상호 작용을 허용하며, 객체의 타입보다 인터페이스에 중점을 둔다. 

어떤 패러다임을 선택할지는 프로젝트의 요구사항과 설계 목표에 따라 다를 것이며, 때로는 두 가지 패러다임을 혼합하여 사용하기도 한다.

## **그럼 OOP를 사용하는 것보다 POP를 사용하는 것이 유용한 경우는 무엇인가?**

→ 상속이 아닌 채택을 해야하는 경우

## **프로토콜을 사용해야하는 이유**

- 다형성(Polymorphism): 프로토콜은 다양한 타입들이 공통적으로 사용할 수 있는 인터페이스를 정의한다. 이를 통해 다형성을 구현할 수 있어, 같은 프로토콜을 따르는 여러 타입을 일관된 방식으로 다룰 수 있다.
- 코드 재사용과 확장성: 프로토콜은 타입 간 코드 재사용을 촉진하고, 새로운 기능을 쉽게 추가하거나 기존 기능을 수정할 수 있도록 도와준다.
- 테스트 용이성: 프로토콜을 사용하면 Mock Object(모의 객체) 또는 Test Doubles(테스트 대역)을 더 쉽게 작성하고 단위 테스트를 수행할 수 있다.

## **모든 상황에 프로토콜을 사용해야하는가?**

- 모든 상황에 프로토콜을 사용할 필요는 없다.
- 프로토콜은 특정한 상황에서 유용하며, 타입 간 인터페이스를 명확하게 정의하고 관리해야할 때 사용된다.
    - ex. 다형성을 활용하거나 여러 다른 타입이 동일한 동작을 공유해야 할 때 프로토콜을 사용

## **프로토콜을 대체할 수 있는 것**

- 대체 할 수 있는 것은 없음.
- 상황에 따라 추상 클래스나 다른 방식의 상속을 고려할 수 있지만, 이는 프로토콜과는 다른 개념이며, 상황에 따라 적절한 방법을 선택해야 한다.

## **프로토콜은 객체지향의 어떤 의미를 가지고 있는 키워드 인가?**

- 프로토콜은 객체 지향 프로그래밍의 인터페이스 개념과 유사한 역할을 한다.
- 객체 지향 프로그래밍에서 인터페이스는 어떤 객체가 특정한 동작을 수행할 수 있는 능력을 정의하고, 클래스나 구조체가 이를 구현하여 해당 능력을 갖도록 하는데 프로토콜도 이와 유사하게 타입 간의 공통 인터페이스를 정의하고, 다형성을 구현하도록 도와준다.
    - ex.
    
    ```swift
    protocol Animal {
        var name: String { get }
        func makeSound()
    }
    
    struct Dog: Animal {
        let name: String
        func makeSound() {
            print("\(name) barks!")
        }
    }
    
    struct Cat: Animal {
        let name: String
        func makeSound() {
            print("\(name) meows!")
        }
    }
    
    let dog = Dog(name: "Buddy")
    let cat = Cat(name: "Whiskers")
    
    let animals: [Animal] = [dog, cat]
    for animal in animals {
        print("\(animal.name) makes a sound: ", terminator: "")
        animal.makeSound()
    }
    ```

# String은 왜 subscript로 접근이 안되는지 설명하시오.

## Swift의 String이란..

Swift의 문자열과 다른 언어의 문자열 타입의 가장 큰 차이는 Int 기반의 인덱스 참조가 가능한지 여부이다. 

다른 언어에서는 Int값을 통해서 원하는 글자를 마음대로 참조하고 변경할 수 있지만, Swift에서는 Int 참조가 불가능하다.

왜냐하면 String은 Character의 Collection 이다. 여기서 Collection은 Array라고 생각하면 된다.

다른 언어에서도 String은 Character 타입의 배열로 나타내는데 왜 Swift만 다를까? 

답은 Character 타입이 무엇을 가리키는 지에 있습니다.

C++나 Java의 char타입은 고정된 크기를 가진다. 하지만 Swift의 Character는 1개 이상의 Unicode Scalar로 이루어져 있다. 

즉 크기가 가변적이라는 것이다. 

실제로 String은 하나의 값에 다양한 뷰를 제공한다.

[라이노님 블로그 참조](https://jcsoohwancho.github.io/2019-11-19-Swift-String-%ED%9A%A8%EC%9C%A8%EC%A0%81%EC%9C%BC%EB%A1%9C-%EC%93%B0%EA%B8%B0/)

따라서 String은 크기가 가변적이기 때문에 Int형식의 subscript로는 접근이 불가능하고, associated index 타입으로 String.Index를 통해 값을 확인할 수 있다.

## subscript란 무엇인가?

공식문서에 의하면 ..

클래스, 구조체, 그리고 열거형은 콜렉션, 리스트, 또는 시퀀스의 멤버 요소에 접근할 수 있는 단축키이다.

설정과 검색을 위한 별도의 메서드 없이 인덱스로 값을 설정하고 조회하기 위해 서브 스크립트를 사용한다. 

예를 들어 `someArray[index]` 로 `Array` 인스턴스에 요소를 접근하고 `someDictionary[key]` 로 `Dictionary` 인스턴스에 요소를 접근한다.

단일 타입을 위한 여러개 서브 스크립트를 정의할 수 있고 사용할 적절한 서브 스크립트 오버로드는 서브 스크립트에 전달하는 인덱스 값의 타입에 따라 선택된다. 

서브 스크립트는 단일 차원으로 제한되지 않고 사용자 타입에 맞춰 여러개의 입력 파라미터로 서브 스크립트를 정의할 수 있다.

쉽게 말해 Swift에서 `subscript`는 클래스, 구조체, 또는 열거형에서 인스턴스 내부의 요소에 접근하기 위한 특별한 메서드이며, 이 메서드를 사용하면 객체 내의 값을 배열처럼 인덱스로 접근할 수 있다. 

그리고 `subscript`를 사용하면 인스턴스를 다룰 때 편리하고 직관적인 코드를 작성할 수 있다.

### subscript 정의

```swift
subscript(index: Index) -> Element {
    get {
        // index에 해당하는 요소를 반환
    }
    set(newValue) {
        // index에 해당하는 요소에 값을 할당
    }
}
```

여기서 `Index`는 요소에 접근하기 위해 사용하는 인덱스 또는 키의 타입이며, `Element`는 해당 컨테이너의 요소 타입이다.

예를 들어, 배열을 생각해보면 배열은 `subscript`를 사용하여 각 요소에 인덱스로 접근한다.

```swift
struct MyArray<T> {
    private var elements: [T] = []

    // 배열 요소에 접근하기 위한 subscript 정의
    subscript(index: Int) -> T {
        get {
            return elements[index]
        }
        set(newValue) {
            elements[index] = newValue
        }
    }
}

var myArray = MyArray<Int>()
myArray[0] = 42
let value = myArray[0] // 42
```

이렇게 정의된 `subscript`를 사용하면 `MyArray` 인스턴스의 요소에 인덱스로 접근하고 값을 설정할 수 있다.

### subscript 사용 예시

```swift
var numberOfLegs = ["spider": 8, "ant": 6, "cat": 4]
numberOfLegs["bird"] = 2
```

`numberOfLegs` 값은 타입 추론에 의해 `[String: Int]형`을 갖는다. 

`numberOfLegs[”bird”] = 2` 는 사전형 변수 `numberOfLegs` 에 key로 **`bird`**를 , 그 값은 `2`를 넣으려는 서브스크립트 문법이다.

## String에서 subscript로 접근을 가능하게 하려면 어떻게 해야하나요?

- String을 확장해서 서브스크립트를 작성해주면 된다.
    
    ```swift
    extension String {
        subscript(idx: Int) -> String? {
            guard (0..<count).contains(idx) else {
                return nil
            }
            let target = index(startIndex, offsetBy: idx)
            return String(self[target])
        }
    }
    // 접근이 가능해진다.
    let sodeul = "Hello, Sodeul!"
    sodeul[0]           // Optional("H")
    sodeul[100]         // nil
    ```
# instance 메서드와 class 메서드의 차이점을 설명하시오.
## **Methods 메서드란?**

일반적으로 func 으로 정의하는 것들을 함수라고 부른다. 그 중에서도 클래스, 구조체, 열거형 타입 내에서 정의된 함수들을 메서드라고 한다.

**Objective-C → Swift**

- Objective-C에서는 클래스 내에서만 메서드 정의가 가능
- Swift에서는 구조체, 열거형에서도 유연하게 메서드 사용이 가능

## **instance method**

- 특정 클래스, 구조체, 열거형 등의 인스턴스에 속한 메소드를 말합니다.
- 인스턴스 메서드는 "인스턴스"와 연관된 메서드이기 때문에, "인스턴스를 생성해야만" 해당 인스턴스를 통해 메서드에 접근할 수 있으며, 아무런 수식어 없이 func으로 선언된 메서드는 모두 인스턴스 메서드이다.
- `.` 을 통해 접근하여 사용하면된다.
- mutating 키워드를 사용
    - 인스턴스 메소드 내에서는 값 타입의 프로퍼티를 변경할 수 없다.
    - mutating 이라는 키워드를 사용해 값 타입 형의 메소드에서 프로퍼티를 변경할 수 있고, mutating 메소드 안에서 self 프로퍼티를 사용하여 완전히 새로운 인스턴스를 생성할 수 있다.
        - struct code 예시
            
            ```swift
            struct Point {
            	var x = 0.0, y = 0.0
            	func moveBy(x deltaX: Double, y deltaY: Double) {
            		self.x += deltaX    // error
            		self.y += deltaY    // error
            	}
            }
            
            // 값 타입 형의 메소드에서 프로퍼티를 변경하고 싶을 경우
            struct Point {
            	var x = 0.0, y = 0.0
            	mutating func moveBy(x deltaX: Double, y deltaY: Double) {
            		self.x += deltaX    // OK
            		self.y += deltaY    // OK
            	}
            }
            
            var somePoint = Point(x: 1.0, y:1.0)
            somePoint.moveBy(x: 2.0, y: 3.0)
            // "The point is now at (3.0, 4.0)" 출력
            print("The point is now at (\(somePoint.x), \(somePoint.y))")
            
            // self를 통하여 새로운 자신의 Point 인스턴스를 생성해 값을 설정할 수 있다.
            struct Point {
            	var x = 0.0, y = 0.0
            	mutating func moveBy(x deltaX: Double, y deltaY: Double) {
            		self = Point(x: x + deltaX, y: y + deltaY)
            	}
            }
            ```
            

## class method

- Type method로 인스턴스를 생성하지 않고 타입 메서드 자체에서 바로 접근이 가능하다.
- 타입 메서드 내부에서 `self` 는 타입 그 자체를 가리킨다.

### **instance method** 와 **class method** 의 차이점

- `self` Property
    - 인스턴스 메서드 내부에서 `self` 는 인스턴스를 가르킨다.
    - 타입 메서드 내부에서 `self` 는 타입 그 자체를 가리킨다.
- 인스턴스 메서드와 타입 메서드는 **접근(호출) 방식**
    - 인스턴스 메서드를 호출할 때는 인스턴스를 만들어 사용해야한다. "인스턴스를 생성해야만" 해당 인스턴스를 통해 메서드에 접근할 수 있다.
    - 타입 메서드(클래스 메서드)는 인스턴스를 생성하지 않고 타입 메서드 자체에서 바로 접근이 가능하다.