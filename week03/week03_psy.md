# 23.09.27 \_ 윤성은, 김나현 Part

# Position : Attack

## 윤성은

---

> **_Extension에 대해 설명하시오._**

- Extension 정의
  - 기존 클래스, 구조체, 열거형 타입에 새로운 Property, Method, Initializer 등 새로운 기능을 추가하는 것이다.
  - 원본 타입(소스 코드)에 접근하지 못하는 타입들도 확장해서 사용할 수 있다.
    - 따라서 소스 코드를 수정하지 않고도 타입의 기능을 확장할 수 있다.
  - extension이란 키워드를 사용하여 확장한다
- Extension의 기능
  - **계산된 인스턴스 속성 및 계산된 타입 속성**
  - **인스턴스 메서드와 타입 메서드**
  - **초기화 구문**
  - **서브 스크립트**
  - **중첩 타입**
  - **특정 프로토콜을 준수할 수 있도록 만듭니다**
- 예시
  - 예시 1
    ```swift
    let point: CGPoint = .init(x: 10, y: 20)
    // 원하는 출력결과 : x: 10, y: 20
    //위와 같은 point가 있을 때 이를 위와 같이 출력하려면 아래와 같이 항상 print 함수를 구현해야함.
    // print("x: \(point.x), y: \(point.y)")

    이 때 extension을 활용한다면!!!

    extension CGPoint {
        func printPoint() {
            print("x: \(self.x), y: \(self.y)")
        }
    }

    print.printPoint() 함수를 사용가능케한다.
    ```
  - 예시 2
    ```swift
    extension Int {
        var isEven: Bool {
            return self % 2 == 0
        }

        func squared() -> Int {
            return self * self
        }
    }

    let number = 8

    if number.isEven {
        print("\(number) is even.")
    } else {
        print("\(number) is odd.")
    }

    print("\(number) squared is \(number.squared()).")

    /*
    - Int에 대한 extension으로 isEven라는 계산된 속성과 squared()라는 메소드를 추가했다.
    - 이렇게 하면 모든 Int 값에 대해 이들 새롭게 정의한 메소드와 속성을 사용할 수 있다.
    */
    ```
- 확장에 프로퍼티 추가하기
  - 오직 `연산 프로퍼티`만 추가O / `저장 프로퍼티` 추가X
      <aside>
      ❓ 왜 저장 프로퍼티는 안돼?
      **: 저장 인스턴스 속성은 해당 타입의 원본 정의 내부에서만 선언할 수 있기에 확장 영역에서는 불가하다.**
      
      </aside>

- 확장에 메서드 추가하기
  - 인스턴스 메서드, 타입 메서드 모두 추가 가능 O
  - 예시
    ```swift
    // 타입 메서드
    extension Int {
        static func printZero() {
            print(0)
        }
    }

    Int.printZero()             // 0

    // 인스턴스 메서드
    extension Int {
        func printDouble() {
            print(self * 2)
        }
    }

    let num = 100
    num.printDouble()           //200
    ```
    <aside>
    ❓ **그렇다면 Extension으로 이미 정의된 메서드나 속성의 동작을 변경할 수 있나?
    : Extension에서는 해당 타입에 대한 새로운 기능만 제공하기에 이미 정의된 메서드, 속성 또는 서브 스크립트의 동작 자체를 변경하는 것은 불가능하다.**
    
    </aside>

- 확장에 생성자 추가
  - Class에서 사용
    - 기존 타입에 새로운 initializer 추가 가능하다.
      - init은 안되고, convenience init 메서드 사용된다. 그러나 최종적으로 Designated Initializer를 호출해야하는데 Designated Initializer는 extension에서 사용 불가하다.
    - 단, deinit은 extension에서는 안된다.(원본 class에서만 가능)
  - Struct에서 사용
    - 구조체에 extension을 사용하여 init을 해준다면, 원본은 유지되기에 Memberwise Initializer를 보존하며 생성자를 추가할 수 있다.

<aside>
❓ **특정 타입만 확장 가능하게 하려면 어떻게 해야해?
: 아래와 같이 where을 == 를 이용하여 타입 옆에 사용한다면 제한 조건을 걸 수 있다.**

</aside>

```swift
extension Array where Element == Int {
    func printAll() {
        for item in self { print(item) }
    }
}

let intNums: [Int] = [1, 2, 3, 4]
let doubleNums: [Double] = [1.0, 2.0, 3.0, 4.0]

intNums.printAll()
doubleNums.printAll()
/* error! Referencing instance method 'printAll()'
 on 'Array' requires the types 'Double' and 'Int' be equivalent */
```

<aside>
❓ **여러 개의 Extension이 같은 타입에 대해 중복해서 사용될 수 있어?**
: **Swift 컴파일러는 각각 독립적인 확장으로 처리하기에 여러 개의 Extension을 같은 타입에 대해 중복해서 사용할 수 있다.**

</aside>

---

> **_Extension 내부에서 함수를 override할 수 있는지 설명하시오._**

- 확장에서는 기존에 정의된 메서드를 오버라이드(override)할 수 없다.
- 메서드를 오버라이드 할 수 있는 것은, 클래스의 서브클래스에서만 부모 클래스의 메서드를 오버라이드 할 수 있다.
  - 상속을 통해 부모 클래스의 속성과 메서드를 상속받은 서브클래스가 부모 클래스의 메서드를 자신만의 구현으로 대체할 수 있는 것을 의미한다.
- 확장은 상속이 아니기에, 상속에서만 가능한 override는 불가능하다.
  - 이는 Swift 언어 설계 원칙 중 하나인 '안전성(Safety)' 때문이다.

<aside>
❓ **확장과 상속의 차이점은 무엇인가요?**

: **확장은 기존 타입에 새로운 기능을 추가하는 것이고, 상속은 기존 클래스를 기반으로 하위 클래스를 만들어 부모 클래스의 속성과 메서드를 상속받는 것이다.
: 확장은 이미 존재하는 타입에 추가적인 기능을 제공하고자 할 때 사용되며, 상속은 타입 계층 구조와 다형성을 구현하기 위해 사용된다.**

</aside>

---

## 김나현

---

> **_Closure에 대하여 설명하시오._**

- **클로저의 정의**
  - 클로저(Closure)는 스위프트에서 일급 객체로 취급되는 이름 없는 함수 또는 "블록"이다.
  - 클로저는 특정 문맥에서 작업을 수행하는 코드 블록을 캡슐화하며, 변수와 상수에 할당될 수 있고, 인자로 전달되거나 반환될 수 있다.
- **클로저의 특징**
  1. Capture Value : 클로저는 자신이 정의된 문맥의 상수와 변수를 "캡처"할 수 있다.
     1. 이는 클로저가 자신이 생성된 범위 밖에서도 이 값들에 접근하고 조작할 수 있다는 것을 의미한다.
  2. Reference Type : 클로저는 참조 타입이기에 다른 변수나 상수에 할당하면 원래의 클로저와 동일한 인스턴스를 가리키게 된다.
     1. == 이는 Class와 동일
  3. Function as a Special Case of Closure : 스위프트에서 모든 함수는 사실 이름 있는 클로저이다.
     1. 예시로 설명 (자세한 것은 아래 `Closure와 함수와의 관계에 대해 설명하시오.` 질문에서 더 다룰예정.)

        ```swift
        let multiplyClosure = { (num1: Int, num2: Int) -> Int in
            return num1 * num2
        }

        let result = multiplyClosure(4, 2) // 결과 : 8

        /*
        위 코드와 같이 multiplyClosure라는 이름의 클로저가 정의되어 있을 때,
        이 클로저는 두 개의 Int 파라미터를 받아서 그 곱셈 결과를 Int 타입으로 반환한다
        이는 함수와 같다고 볼 수 있다.
        */
        ```
    <aside>
    ❓ **그래서 클로저를 왜 쓰는데?
    :**
    
    1. **비동기 실행: 클로저는 종종 비동기 작업에서 결과를 처리하는 데 사용된다. 예를 들어, 네트워크 요청이 완료되면 특정 코드 블록(클로저)을 실행하도록 스케줄링 할 수 있다.**
    2. **캡처링: 클로저는 자신이 정의된 범위의 변수와 상수를 캡처할 수 있다. 이는 해당 변수와 상수가 범위를 벗어나더라도 해당 값을 계속 사용할 수 있다는 것을 의미.**
    3. **고차 함수: Swift에서 함수는 일급 객체이기에 함수에 함수(또는 클로저)를 매개변수로 전달하거나 반환값으로 제공할 수 있다.**
    4. **익명함수: 클로저는 이름이 없으므로 임시적인 작업에 유용하다.**
    5. **문법 간소화: Swift의 문법은 클로저를 간결하게 표현할 수 있도록 지원한다.**
    </aside>


---

> **_Closure와 함수와의 관계에 대해 설명하시오._**

- **간단 요약으로 `모든 함수 = 이름있는 클로저`이다.**
- **함수 / 클로저 정의**
  - 함수(Function)
    - 함수는 이름을 가진 코드 블록으로, 특정 작업을 수행하거나 계산 결과를 반환하는데 사용된다.
    - 예시
      ```swift
      func addTwoNumbers(a: Int, b: Int) -> Int {
          return a + b
      }
      ```
  - 클로저(Closure)
    - 클로저는 이름 없는 코드 블록으로 변수나 상수에 할당되거나 다른 함수에 인자로 전달될 수 있다.
    - 클로저가 정의된 범위의 변수와 상수를 캡처할 수 있는 기능도 제공한다.
    - 예시
      ```swift
      let addTwoNumbers = { (a: Int, b: Int) -> Int in
          return a + b
      }
      ```
- **함수 / 클로저 차이**
  - 위의 예시들을 보면 `addTwoNumbers`라는 동일한 작업을 하는 함수와 클로저가 있다.
  - 첫 번째 예제에서 `addTwoNumbers`는 일반적인 함수이며, 두 번째 예제에서 `addTwoNumbers`는 클로저로 선언되었다.
  - 이 때에 함수와 클로저 사이의 주요 차이 중 하나는 그 문맥에서 캡처된 값을 유지할 수 있다는 것인데, 아래 예시와 함께 보자.
    ```swift
    func makeIncrementer(incrementAmount: Int) -> () -> Int {
        var total = 0
        let incrementer: () -> Int = {
            total += incrementAmount
            return total
        }
        return incrementer
    }

    let incrementByTwo = makeIncrementer(incrementAmount: 2)
    print(incrementByTwo()) // 출력: 2
    print(incrementByTwo()) // 출력: 4

    /*
    위 코드에서 makeIncrementer 함수는 클로저 incrementer를 반환하며,
    이 클로저는 total과 incrementAmount 두 변수를 캡처한다.
    이 때, incrementByTwo라는 상수에 할당된 클로저는 makeIncrementer 함수의 실행이 끝나서
    그 범위(scope)를 벗어났음에도 여전히 캡처한 변수들(total, incrementAmount)에 접근하고
    수정할 수 있다.
    */
    ```
    - 위 코드에서 볼 수 있듯, 함수와 클로저 사이의 주요 차이 중 하나인 '값 캡쳐' 기능은 클로져가 특정 문맥(context)에서 필요한 정보를 저장하고 재사용하는 강력한 도구로써 역할을 한다.
      <aside>
      ❓ **함수와 클로저 사이의 주요 차이 중 하나는 그 문맥에서 캡처된 값을 유지할 수 있다는 것인데, 여기서 `문맥에서 캡처된 값을 유지할 수 있다` 가 무슨 뜻이야?**
      : 이는 클로저가 자신이 생성된 범위의 변수와 상수를 "캡처"하고, 그 값들을 나중에 사용할 수 있다는 것을 의미한다.
      즉, 위 예시로 보면 incrementer 클로저는 자신이 생성된 범위, {} 코드블록을 벗어나더라도 해당 값을 밖에서 유지한다.
      
      </aside>

- **Closure** VS **Function 질문 폭탄**
    <aside>
    ❓ **클로저를 사용하는 이유는 무엇인가?**
    : 함수보다 구문적으로 간결하거나 특정 문맥에서 변수를 캡처해야 할 때에도 자주 사용되며, 비동기 작업, 콜백 처리, 데이터 정렬 및 필터링 등과 같은 상황에 사용된다.
    
    </aside>
    
    <aside>
    ❓ **클로저와 함수 중 어떤 것이 성능상 더 우수한가?**
    : 일반적으로 성능 면에서 큰 차이는 없다. 따라서 가독성과 코드 구조에 초점을 두고 선택하자.
    
    </aside>
    
    <aside>
    ❓ **함수와 클로저 중 어떤 것을 선택해야 할까?**
    : 이름 있는 함수는 재사용 가능한 코드 조각을 정의할 때 유용하며, 여러 번 호출되거나 다른 곳에서 참조될 수 있다.
    반면에 한 번만 사용되거나 간단한 작업을 위해서는 클로저가 더 적합하다.
    
    </aside>


---
