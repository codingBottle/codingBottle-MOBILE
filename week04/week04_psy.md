# 23.10.04 \_ 박신영, 김하은 Part

## 박신영 질문

---

> Optional 이란 무엇인지 설명하시오.

- Optional은 변수가 값을 가질 수도, 아니면 전혀 값이 없을 수도 있음을 표현하는 타입입니다.
  - 즉, Optional은 두 가지 가능성을 모두 포함하고 있는 것으로 어떤 값이 반드시 존재해야 하는 것이 아니라면 Optional로 선언할 수 있습니다.
- Optional 사용 사례
  - 변수가 초기화되지 않은 상태를 나타낼 때
  - 함수가 실패하거나 적절한 값을 반환할 수 없는 경우를 표현할 때
  - Example)
    ```swift
    let possibleNumber = "123"
    let convertedNumber = Int(possibleNumber) // convertedNumber is of type "Int?", or "optional Int"
    ```
    - 위 코드에서 `Int(possibleNumber)`의 반환 타입은 `Int?`입니다.
    - 이는 "Optional Int"라고 읽으며, 정수값을 가질 수도 있고(nil), 아니면 전혀 값이 없을(nil) 수도 있다는 것을 의미합니다.
- Unwrapping 방법
  1. Forced Unwrapping (!)

     ```swift
     if convertedNumber != nil {
         print("convertedNumber has an integer value of \(convertedNumber!).")
     }
     ```

     - Forced unwrapping(`!`)은 강제적으로 값을 추출하지만, 만약 값이 `nil`인 경우 런타임 에러를 발생시키므로 주의해서 사용해야 합니다.

  2. Optional Binding (`if let` 또는 `guard let`)

     ```swift
     if let actualNumber = convertedNumber {
         print("The number is \(actualNumber).")
     } else {
         print("The conversion did not yield a number.")
     }
     ```

     - optional binding(`if let`, `guard let`)은 안전하게 optional 값을 처리할 수 있는 방법으로 권장됩니다.
- 옵셔널 체이닝(Optional Chaining)
  - 옵셔널 체이닝(Optional Chaining)은 옵셔널 값이 `nil`인지 확인하고, `nil`이 아닌 경우에만 프로퍼티, 메소드, 서브스크립션 등을 호출하는 방법입니다. 이를 통해 여러 단계의 옵셔널 프로퍼티를 안전하게 접근할 수 있습니다.
  - 옵셔널 체이닝은 `?` 기호를 사용하여 수행됩니다. 만약 옵셔널 값이 `nil`인 경우, 체인의 나머지 부분은 무시되고 전체 표현식의 값은 `nil`이 됩니다.
  - Example)
    ```swift
    class Person {
        var residence: Residence?
    }

    class Residence {
        var numberOfRooms = 1
    }

    // 여기서 Person 클래스가 주거지를 가지고 있는지 확인하려면 일반적으로는 옵셔널 바인딩을 사용한다.
    ```
    ```swift
    if let residence = person.residence {
        print("The person has \(residence.numberOfRooms) room(s).")
    } else {
        print("The person has no residence.")
    }

    //위 코드에서 옵셔널 체이닝을 사용하면 아래와 같이 간단해진다.

    if let roomCount = person.residence?.numberOfRooms {
        print("The person has \(roomCount) room(s).")
    } else {
        print("The person has no residence.")
    }
    ```
    - 위 코드에서 `person.residence?.numberOfRooms`는 '만약 `person.residence`가 존재한다면 그것의 `numberOfRooms`에 접근'하는 것을 의미합니다. 만약 `person.residence`가 nil인 경우 전체 표현식도 nil로 평가되며, 그렇지 않으면 해당 숫자값으로 평가됩니다.
    - 따라서 이 코드는 "주거지가 있는 사람"과 "주거지가 없는 사람" 모두에 대해 안전하게 동작할 수 있도록 합니다.

> MVC 구조에 대해 블록 그림을 그리고, 각 역할과 흐름을 설명하시오.

- MVC란?
  - MVC(Model-View-Controller)는 소프트웨어 디자인 패턴 중 하나로, 애플리케이션의 로직을 세 가지 역할로 분리하여 관리하는 방법입니다.

```swift
[ User ]
      ▲
      | (Interacts)
      ▼
[ Controller ]
  ▲     ▼
  |     | (Updates and Instructs)
  |     |
[ Model ]---(Notifies)--->[ View ]
  ^                          |
  | (Updates)                |
  └--------------------------┘
```

![MVC 그림.png](23%2010%2004%20_%20%E1%84%87%E1%85%A1%E1%86%A8%E1%84%89%E1%85%B5%E1%86%AB%E1%84%8B%E1%85%A7%E1%86%BC,%20%E1%84%80%E1%85%B5%E1%86%B7%E1%84%92%E1%85%A1%E1%84%8B%E1%85%B3%E1%86%AB%20Part%20343e0ba5e279493fb50775021e3d1431/%25E1%2584%2589%25E1%2585%25B3%25E1%2584%258F%25E1%2585%25B3%25E1%2584%2585%25E1%2585%25B5%25E1%2586%25AB%25E1%2584%2589%25E1%2585%25A3%25E1%2586%25BA_2023-10-03_%25E1%2584%258B%25E1%2585%25A9%25E1%2584%2592%25E1%2585%25AE_11.35.34.png)

- MVC를 구성하는 구성요소와 역할
  - **Model: Model은 데이터와 비즈니스 로직을 처리하는 부분입니다. 예를 들어, 사용자 데이터, 상품 데이터 등의 실제 정보와 이러한 정보를 처리하기 위한 메소드(예: 데이터 검증, 계산 등)가 포함됩니다.**
  - **View: View는 사용자에게 보여지는 UI(User Interface) 부분입니다. 사용자가 보고 상호작용하는 모든 UI 요소들(버튼, 텍스트 필드, 레이블 등)을 담당합니다.Controller: Controller는 Model과 View 사이의 중재자 역할을 합니다.**
  - **Controller는 사용자의 입력을 받아 Model에 반영하고, Model에서 발생한 변경사항을 다시 View에 반영합니다.**
- Example)
  > 1. 사용자가 앱에서 "새로운 할 일 추가" 버튼(이것은 View에 속함)을 클릭합니다.
  > 2. 버튼 클릭 이벤트는 Controller에 전달됩니다.
  > 3. Controller는 이 입력을 받아서 적절한 Model (여기서는 "할 일" 목록) 에 새로운 할 일 아이템 추가를 지시합니다.
  > 4. Model은 새로운 할 일 아이템을 자신의 데이터 구조체나 배열 등에 추가하고, 그 결과 (성공 또는 실패 등)를 Controller에게 알립니다.
  > 5. Controller는 이 정보를 받아서 View (여기서는 "할 일" 목록 화면) 에게 새로운 할 일 아이템 추가 결과를 반영하도록 지시합니다.
  > 6. 마지막으로 View에서 해당 변경사항 (새롭게 추가된 "할 일") 을 화면에 표시합니다.
  위 과정처럼 MVC 패턴에서 각 구성 요소들은 각각 독립적으로 유지되며 서로 소통하도록 설계됩니다. 이렇게 함으로써 코드 재사용성과 유지보수성이 증가하고 개발 과정에서 각 부분별 책임과 역할이 명확해집니다.
  >
- MVC와 더불어 다른 디자인 패턴들 장단점
  > \*\*MVC (Model-View-Controller)
        ◦ 장점: Model, View, Controller의 역할이 명확하게 분리되어 있어서 설계가 직관적이고 이해하기 쉽습니다. 또한, 유지보수와 테스트가 용이합니다.
        ◦ 단점: View와 Controller가 밀접하게 연결되어 있기 때문에, 복잡한 애플리케이션에서는 Controller가 과도하게 커질 수 있습니다(마치 "Massive View Controller"라는 말이 나올 정도로). 이로 인해 코드의 가독성과 재사용성이 떨어질 수 있습니다.
  MVVM (Model-View-ViewModel)
  ◦ 장점: ViewModel을 도입함으로써 View와 Model 사이의 의존성을 줄일 수 있으며, 이를 통해 UI 로직을 분리하고 테스트를 더 쉽게 할 수 있습니다.
  ◦ 단점: 설계가 복잡하며 구현 난이도가 상대적으로 높아 초보 개발자들에게는 접근하기 어려울 수 있습니다. 또한 ViewModel 내에 비즈니스 로직이 포함될 가능성이 있는데 이 경우 ViewModel 자체가 복잡해져서 유지보수하기 어려울 수 있습니다.
  MVP (Model-View-Presenter)
  ◦ 장점: Presenter를 도입하여 View와 Model 사이의 중재 역할을 명확히 하므로 MVC보다 각 구성 요소간의 결합도(Coupling)를 낮출 수 있으며 테스트 용이성을 보장합니다.
  ◦ 단점: Presenter와 View 사이에 대량의 데이터 교환 작업들 때문에 코드량 증가 및 복잡도 상승 문제 발생 가능합니다.\*\*
  >

<aside>
❓ Q : **Controller가 너무 많은 역할을 담당하는 것 같아요. 이 문제를 어떻게 해결할 수 있을까요?
A : 컨트롤러의 책임을 여러 객체로 분산시키는 것이 유용할 수 있습니다. 예를 들어, 복잡한 데이터 변환 로직은 별도의 Service 객체에 위임하거나, 특정 UI 이벤트 처리 로직은 다른 Helper 객체에 위임할 수 있습니다.**

</aside>

<aside>
❓ Q : **View와 Controller의 분리는 어떻게 이루어져야 할까요?
A : View와 Controller의 분리는 애플리케이션의 유지보수성과 확장성을 향상시키는 데 중요합니다. 일반적으로, View는 사용자 인터페이스와 관련된 로직만 처리하고, Controller는 사용자 입력과 비즈니스 로직 처리를 담당합니다. 이들 사이의 결합도를 낮추기 위해, Controller가 View를 직접 참조하는 것보다는 인터페이스나 Observer 패턴 등을 통해 상호작용하는 것이 좋습니다.**

</aside>

## 김하은 질문

---

> mutating 키워드에 대해 설명하시오.

- Swift 내에서의 값 타입의 속성을 변경하려면 ‘mutating’ 키워드를 사용해야 합니다.
- 이는 값 타입에 해당하는 구조체, 열거형 내부의 메소드에서 인스턴스 자체를 수정할 수 있게 합니다.
- Class의 경우 참조 타입이기에 메소드 내에서 자유롭게 속성들을 변경가능하기에 ‘mutating’이 따로 필요하지 않습니다.
- Example)
  ```swift
  struct Point {
      var x: Int
      var y: Int

      mutating func moveBy(x deltaX: Int, y deltaY: Int) {
          x += deltaX
          y += deltaY
      }
  }

  var point = Point(x: 0, y: 0)
  point.moveBy(x: 5, y: 5)
  print(point) // Prints "Point(x: 5, y: 5)"
  ```
  - 위 `Point`라는 이름의 구조체에 `x`와 `y`라는 두 개의 정수 변수가 포함되어 있고, `moveBy(x:y:)`라는 메소드도 포함되어 있습니다.
  - 해당 메소드 앞에 'mutating' 키워드가 붙어 있는데, 이는 이 메소드가 해당 구조체의 인스턴스 자신을 수정한다는 것을 뜻합니다. 즉, 'x'와 'y' 값을 바꾼다는 것 입니다. 그렇기에 `point.moveBy(x: 5, y: 5)`라고 호출하면 원래 (0,0)이었던 점이 (5,5)로 바뀌는 것 입니다.
  - 또한 var가 아닌 let을 사용하였다면 컴파일러 에러가 발생한다. let은 상수 이기에.

<aside>
❓ **Q** : **클래스에서는 왜 `mutating` 키워드가 필요 없을까?
A : 클래스는 참조 타입이기에 인스턴스 자체를 변경할 수 있습니다. 따라서 메소드 내에서 속성을 변경하는 것이 기본적으로 가능하므로 `mutating` 키워드가 필요 없습니다.**

</aside>

<aside>
❓ **Q : `let`으로 선언된 구조체 인스턴스에 대해 왜 `mutating` 메소드를 호출할 수 없을까?
A : Swift에서 `let`은 상수 선언에 사용되며, 이것은 한 번 할당된 후에 변경할 수 없다는 것을 의미합니다. 따라서 상수로 선언된 구조체 인스턴스의 경우, 그 속성값은 수정될 수 없으므로 `mutating` 메소드를 호출할 수 없습니다.**

</aside>

<aside>
❓ Q : **함수 내부의 클로저(Closure) 안에서 mutating 함수를 호출하려고 하면 어떻게 되나?
A : 일반적으로 클로저 내부에서 self 를 capture 해서 사용하기 때문에 컴파일 에러가 발생합니다. 이 문제를 해결하기 위해서는 클로저 앞에 `[self] in`, `[weak self] in`, `[unowned self] in` 등 적절한 capture list 를 추가해야 합니다.**

</aside>

---

> 탈출 클로저에 대하여 설명하시오.

- 클로저는 일반적인 함수와 달리, 선언된 컨텍스트에서 캡처한 변수나 상수에 접근하고 저장할 수 있습니다.
- 탈출 클로저(Escaping Closure)란, 함수가 종료된 후에 호출되거나 외부의 변수에 저장되어 나중에 사용될 수 있는 클로저를 말합니다.
  - Swift에서는 이러한 탈출 클로저를 위해 `@escaping` 속성을 제공합니다.
    <aside>
    ❓ Q : `@escaping` 속성이 뭐야?
    A :
    
    - **`@escaping`은 함수의 파라미터로 전달되는 클로저가 함수의 실행이 끝난 후에 호출될 수 있음을 나타내는 속성입니다.**
    - **함수나 메소드를 호출하면, 그 안에서 사용되는 모든 파라미터들은 기본적으로 함수가 종료될 때 함께 사라집니다. 이것이 "비탈출(non-escaping)" 클로저의 기본 동작 방식입니다.**
    - **그러나 어떤 경우에는 함수가 반환된 후에도 클로저를 계속 유지하거나 나중에 실행해야 할 필요가 있습니다. 예를 들어, 비동기 작업을 처리하는 경우나 완료 핸들러(completion handler)를 사용하는 경우 등이 있습니다.**
    - **이런 상황에서는 `@escaping` 키워드를 사용하여 클로저가 함수 범위를 "탈출(escape)"한다는 것을 명시해야 합니다. 이렇게 하면 해당 클로저는 함수 외부에서 참조될 수 있고, 필요한 시점에 호출될 수 있습니다.**
    </aside>

- Example)
  ```swift
  var completionHandlers: [() -> Void] = []

  func someFunctionWithEscapingClosure(completionHandler: @escaping () -> Void) {
      completionHandlers.append(completionHandler)
  }
  ```
  - 위 코드에서 `someFunctionWithEscapingClosure` 함수는 파라미터로 `completionHandler`라는 탈출 클로저를 받습니다. 이 클로저는 함수가 종료된 후에도 호출될 수 있으며, 외부의 `completionHandlers` 배열에 저장됩니다.
  - 아래는 이를 활용하는 예시입니다.
  ```swift
  func functionThatCallsCompletion() {
      someFunctionWithEscapingClosure {
          print("Completion handler has been called.")
      }

      // Execute all the completion handlers
      for completionHandler in completionHandlers {
          completionHandler()
      }
  }

  functionThatCallsCompletion()
  // Prints "Completion handler has been called."
  ```
  - `functionThatCallsCompletion()`을 호출하면 "Completion handler has been called."라는 메시지가 출력됩니다. 여기서 중요한 점은, `someFunctionWithEscapingClosure` 내부에서 추가된 `completionHandler`가 나중에 별도로 실행되었다는 것입니다. 이렇게 나중에 실행될 가능성이 있는 경우 탈출클로져(`@escaping`) 표기를 해주어야 합니다.
  ***
  - 탈출클로져(`@escaping`)의 주요 용도 중 하나는 비동기 작업을 다룰 때 입니다. 비동기 작업(예: 네트워크 요청)이 완료되면 실행되어야 하는 코드 블록을 정의하는데 사용할 수 있습니다.

<aside>
❓ Q : **왜 클로저가 기본적으로 비탈출(non-escaping)인가?
A : Swift에서 메모리 관리와 성능 최적화에 도움을 주기위해 모든 함수 파라미터는 기본적으로 비탈출입니다. 클로저가 함수 범위를 탈출하지 않으면, 컴파일러는 메모리 할당과 해제를 더 효율적으로 관리할 수 있습니다.**

</aside>

<aside>
❓ Q : **`@escaping`과 `@autoclosure`는 어떻게 다른가?
A : `@escaping`은 클로저가 함수의 실행이 끝난 후에 호출될 수 있음을 나타내며, `@autoclosure`는 인자 없이 호출되는 자동 생성 클로저를 만드는 것입니다. 이 두 속성은 서로 다른 목적을 가지고 있으며, 필요에 따라 함께 사용될 수도 있습니다.**

</aside>
