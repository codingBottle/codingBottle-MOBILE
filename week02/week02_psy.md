# 23.09.20 \_ week03

# Defencer : 문인호, 박현렬

## Delegate 패턴을 활용하는 경우를 예를 들어 설명하시오.

- Delegate 란?
  ![스크린샷 2023-09-19 오전 11.26.57.png](23%2009%2020%20_%20week03%205e086c669c3341628302c47615c572da/%25E1%2584%2589%25E1%2585%25B3%25E1%2584%258F%25E1%2585%25B3%25E1%2584%2585%25E1%2585%25B5%25E1%2586%25AB%25E1%2584%2589%25E1%2585%25A3%25E1%2586%25BA_2023-09-19_%25E1%2584%258B%25E1%2585%25A9%25E1%2584%258C%25E1%2585%25A5%25E1%2586%25AB_11.26.57.png)
  - 이는 사전적 정의로 위임하다, 대표 등이 있는데 ‘대신 해주는 것’ 이라고 생각하면 조금 더 이해에 용이할 것이다.
  - 쉽게 말하여 A 객체가 해야할 일을 B 객체가 도맡아 하는 것이다.
    - 예시로 각종 뷰(tableView, collectionView …)의 구성요소를 탭하였을 때 어떠한 행동을 보일지에 대한 책임을 ViewController에게 Delegate를 이용하여 위임하는 것이다.
- Delegate를 왜 사용하는가?
  - 주요한 이유는 객체 간 불필요한 직접적 접촉을 최소화함과 더불어 모듈화, 캡슐화를 유지하기 위함 등 다양한 객체간 통신을 용이하게 만들기 위함이다.
  1. 객체 간의 느슨한 결합
     1. delegate 패턴을 사용한다면 객체가 다른 객체의 내부를 직접적으로 알 필요 없이 통신할 수 있기에 객체간 결합이 줄어 변경과 확장이 있을 때에 용이합니다.
        - ❓ : 객체가 다른 객체의 내부를 직접 알게됐을 때의 단점이 뭔데?
          1️⃣ 결합도 증가 = 하나의 객체를 수정한다면 다른 객체에 영향을 줄 가능성이 높아진다. 이는 곧 유지 보수가 필요할 때에 난관으로 작용할 수 있다.
          2️⃣ 재사용 어려움 = 레이아웃 혹은 같은 타입 값을 공유하는 뷰가 있을 때에 A 객체에서 값을 변경한 것이 원본 객체에 영향을 준다면, 원본 객체를 이용하는 또 다른 객체는 B 역시도 이에 영향을 받아 재사용하기에 어려움이 있다.
  2. 모듈화
     1. 특정 작업을 수행하는 객체는 해당 작업을 수행하는 delegate 객체에 위임하는 작업들을 통하여 각각 독립적인 모듈로 구성할 수 있다.
        - ❓ : 모듈화가 뭔데?
          ➡️ 큰 시스템을 작은 부분으로 분리하는 프로그래밍 방법론이라 정의되지만 이렇게만 이야기하면 제대로 이해되지 않으니 아래의 예시로 부연설명을 하겠다.
          EX) 레고 한 테마를 구상할 때에 집, 사람, 주변 요소(나무, 자동차) 등이 위 테마를 이룬다. 또한 사람의 구성요소는 머리, 팔, 다리, 몸 등의 구성요소로 이루어지고, 팔은 손, 팔목, 어깨 등이 모여 팔을 구성하는 것이다. 위와 같이 하나의 구성을 최대한 작은 부분으로 나누어 같은 것을 바라보는 이들끼리 모아둔 것을 모듈이라 칭하며 위의 과정을 적용한 것을 ‘모듈화’ 라 칭한다. (`참고1`에 자세히 정리되어 있다)
  3. 캡슐화
     1. delegate 패턴을 사용하여 객체 간 통신을 구현하면 객체의 내부 상태를 외부로 노출시키지 않고도 해당 상태를 조작할 수 있다. 이는 객체의 데이터를 직접 노출하지 않고, 메서드를 통하여 데이터를 읽고 쓰는 방식으로 접근한다.
        - ❓ : 캡슐화(Encapsulation)가 뭔데?
          ➡️ 이는 객체 지향 프로그래밍(OOP: Object-Oriented Programming)의 핵심 원칙 중 하나로, 데이터와 그 데이터를 조작하는 함수를 하나의 '객체'라는 단위로 묶는 것을 뜻한다.
          캡슐화를 적용한다면 이를 통하여 객체 내부의 상세 구현 내용을 숨기고, 외부에서는 이에 접근할 때에 해당 객체가 제공하는 메서드만을 통하여 접근하도록 할 수 있다. 이는 객체의 상태 변경을 보다 안전하게 관리하기에 용이하다.
- Delegate 패턴을 언제 사용하는가?
  - 사용자 인터랙션 처리: iOS에서 UITableView 또는 UICollectionView와 같은 UI 요소들이 사용자 인터랙션에 대응하기 위해 Delegate 패턴을 자주 사용한다.
    - 예를 들어, UITableView의 경우 UITableViewDelegate를 구현하여 테이블 뷰에서 유저가 하나의 셀을 선택했을 때에, 선택된 셀에 대한 반응을 처리할 수 있습니다. 이는 위 정의에서 언급한 것과 동일한 내용이다.
  - 커스텀 데이터 전달: 한 객체가 다른 객체로 정보를 전달해야 하는 경우 Delegate 패턴을 사용할 수 있다.
    - 예를 들어, modal 방식으로 화면 전환 후 데이터를 원래 화면으로 전달할 때 delegate를 사용합니다. 이는 아래 `참고2`에 잘 정리되어있다.
  - 복잡한 작업 분리: 한 클래스나 구조체가 너무 많은 역할을 담당하고 있거나 복잡해질 때, 관련 기능들을 delegate로 분리하여 코드의 가독성과 유지 보수성을 높일 수 있다.

⚠️ 참고

- 1번 : [https://hh963103.medium.com/swift-애플리케이션을-위한-상향식-개발-part-1-모듈화-1c1af4bff268](https://hh963103.medium.com/swift-%EC%95%A0%ED%94%8C%EB%A6%AC%EC%BC%80%EC%9D%B4%EC%85%98%EC%9D%84-%EC%9C%84%ED%95%9C-%EC%83%81%ED%96%A5%EC%8B%9D-%EA%B0%9C%EB%B0%9C-part-1-%EB%AA%A8%EB%93%88%ED%99%94-1c1af4bff268)
- 2번 : [https://zeddios.tistory.com/8](https://zeddios.tistory.com/8)

---

## 프로토콜이란 무엇인지 설명하시오.

- 프로토콜이란?
  - 특정 역할을 하기 위한 메서드, 프로퍼티, 기타 요구사항 등의 청사진
    - 쉽게 이야기하여 서로간의 ‘약속’이라고 생각하면 이해에 용이하다.
      - EX) ‘선생님’ 과 같은 프로토콜이 있을 때에 해당 프로토콜은
        과목명, 담당 클래스, 가르치다(), 출석부르다() 등 을 갖고 있을 것이다.
        이 때에 국어, 영어 선생님이 각각 생성되었고 이들은 ‘선생님’ 이라는 신분을 갖고 있기에 ‘선생님’ 이라는 프로토콜을 준수 해야만 한다.
    - 위의 예시에서 [과목명, 담당 클래스, 가르치다(), 출석부르다() 등]과 같은 청사진을 만든 것이다.
- 프로토콜의 사용
  - 구조체, 클래스, 열거형은 프로토콜을 채택해서 특정 기능을 실행하기 위한 프로토콜의 요구사항을 실제로 구현할 수 있다.
  - 프로토콜은 정의를 하고 제시를 할 뿐 스스로 기능을 구현은 하지 않는다. (조건만을 정의)
    EX)
    - ‘선생님’ 프로토콜
      ```swift
      protocol Teacher {
          var subject: String { get }
          var classInCharge: String { get }

          func teach()
          func callAttendance()
      }
      ```
    - ‘영어선생님’ 클래스 \_ ‘선생님’ 프로토콜 채택
      ```swift
      class EnglishTeacher: Teacher {
          var subject: String = "English"
          var classInCharge: String

          init(classInCharge: String) {
              self.classInCharge = classInCharge
          }

          func teach() {
              print("\(subject) lesson starts.")
          }

          func callAttendance() {
              print("Calling attendance for \(classInCharge) class.")
          }
      }
      ```
      - 위 영어선생님 클래스에서는 `Teacher` 프로토콜을 채택하고 있으며, `subject`는 영어로 고정되어 있고, 담당하는 클래스(`classInCharge`)는 인스턴스 생성 시 초기화된다.
        또한 `teach()`와 `callAttendance()` 메서드도 각각 수업 시작과 출석 부르기를 출력하도록 구현되어있다.
  - 위 예시처럼 프로토콜은 특정 역할을 하기 위한 메서드와 속성의 청사진 역할을 담당한다.
  - 이러한 청사진에 따라 실제 타입들이 구현됨으로써 코드의 응용성과 재사용성 등이 크게 향상된다.
    - **❓ : 위의 예시도 그렇고 프로토콜에서의 프로퍼티는 항상 var로 정의하여야 한다고 하는데 이유는 무엇일까?**
      ➡️ 이는 위에서도 언급하였듯 프로토콜이 해당 프로퍼티의 구체적인 구현 방식을 제한하지 않기 때문이다.
      Swift의 프로토콜은 해당 타입이 가져야 할 속성과 메서드를 명시하는 것을 끝으로, 이를 준수하는 타입이 어떻게 이들을 구현할지에 대해서는 규정하지 않는다.
      따라서 프로토콜에서 선언된 `var` 키워드는 "이 타입에는 이런 이름과 타입의 '속성'이 있어야 한다"라고 명시하는 것일 뿐, 실제 값이 변수인지 상수인지에 대해서는 결정하지 않기에 모든 경우를 고려하여 유연하게 사용하기 위하여 `var` 키워드를 사용한다.
      **+@** ➡️ \*\*\*\*프로토콜에서 `{ get }` 요구사항을 갖는 프로퍼티는 클래스나 구조체 등에서 `let` 상수 또는 `var` 변수, 혹은 읽기 전용 계산 속성(read-only computed property)으로 만족시킬 수 있다. 하지만 `{ get set }` 요구사항을 갖는 프로퍼티는 반드시 가변 속성(mutable property), 즉 `var` 변수나 읽고 쓸 수 있는 계산 속성(read-write computed property)으로 만족시켜야 하기에 추후 유연한 대처를 위해 var로 선언
    - **❓ : 프로토콜에서 메서드 정의시 {}를 적지 않는 이유는 뭘까?**
      ➡️ 이는 위 `var` 선언의 이유와 동일하다. 프로토콜의 본질적인 목적으로 프로토콜은 '어떤 일'을 해야 한다는 것만 명시하고, 그 '일'이 어떻게 이루어져야 하는지는 각각의 타입에게 맡기기 때문이다.
    - **❓ : 아래와 같은 코드가 있을 때에 어째서 부모 클래스를 프로토콜보다 앞선 순서로 적어야 할까?**
      ```swift
      // Person: 부모 클래스
      class Person {
          var name: String
          var age: Int

          init(name: String, age: Int) {
              self.name = name
              self.age = age
          }
      }

      // EnglishTeacher 클래스 _ Person, Teacher
      class EnglishTeacher: Person, Teacher {
          var subject: String = "English"
          var classInCharge: String

          init(name: String, age: Int, classInCharge: String) {
              self.classInCharge = classInCharge
              super.init(name: name, age: age)
          }

          func teach() {
              print("\(subject) lesson starts.")
          }

          func callAttendance() {
              print("Calling attendance for \(classInCharge) class.")
          }
      }
      ```
      ➡️ 이는 Swift 클래스는 단일 상속만을 지원하는 특성으로 한 클래스가 상속받을 수 있는 부모 클래스는 오직 하나인 것을 뜻한다.
      반면 프로토콜은 여러 개를 동시에 채택 가능하기에 클래스 정의 시 상속과 프로토콜 채택을 함께 명시해야 한다면, 부모 클래스를 먼저 나열하는 것이 언어 설계상 설정이다. 이는 문법적 혼동방지와 코드 가독성 향상에도 도움을 준다.

⚠️ 참고

1번 : [https://medium.com/@jgj455/오늘의-swift-상식-protocol-f18c82571dad](https://medium.com/@jgj455/%EC%98%A4%EB%8A%98%EC%9D%98-swift-%EC%83%81%EC%8B%9D-protocol-f18c82571dad)

---

## String은 왜 subscript로 접근이 안되는지 설명하시오.

- Swift의 `String` 타입은 문자열을 나타내는 타입이지만, `subscript`를 통한 인덱스로 접근이 불가하다 이유는 아래와 같다.
  - 문자열의 불변성(Immutability): Swift에서의 `String`은 불변(immutable)한 값 타입이다.
  - 이는 한 번 생성된 문자열은 수정할 수 없음을 의미하기에, `subscript`를 통해 특정 인덱스에 접근하여 값을 변경하는 것은 불가능하다.
  - Swift의 문자열은 Unicode characters로 구성되는데 하나의 문자를 나타내기 위해서는 여러 개의 유니코드 스칼라(scalar)가 조합될 수 있다. 즉, 크기가 가변적인 것이다.
    - 위와 같은 내용이 무엇을 뜻하느냐면 일부 문자나 기호들은 단일 유니코드 스칼라로 표현하기 어렵거나 불가능할 때가 있다. 다음은 위에 해당하는 경우인데 여러 개의 유니코드 스칼라를 조합하여 하나의 문자를 만들어야 하는 때이다. "👨‍👩‍👧‍👦"와 같은 이모지 시퀀스는 여러 개의 유니코드 스칼라로 구성되며 하나의 가족을 나타내기에 이모지와 조합된 문자열도 유효한 문자열로 취급한다.
    - 이러한 경우에는 단순히 인덱스로 접근하기 어렵고, 복잡한 처리 과정이 필요하기에 `subscript` 로 곧 바로 접근하기에 어려운 것으로 정의한다.
  - 따라서, Swift에서 `String` 타입은 값 자체를 변경할 수 없으므로 `subscript`로 직접 접근하여 수정하는 것은 지원되지 않는다. 그렇기에 새로운 문자열을 생성하거나 기존 문자열을 조작하는 메서드와 속성들을 사용하여 작업해야 합니다**.**
    - 다음은 String은 왜 subscript로 접근이 가능하도록 하는 예이다.
      ```swift
      let str = "Hello, World!"

      let firstCharacter = str[str.startIndex]
      print(firstCharacter)  // "H"

      let thirdCharacter = str[str.index(str.startIndex, offsetBy: 2)]
      print(thirdCharacter)  // "l"
      ```
      - 이의 첫 번째 예시는 문자열의 첫 번째 문자에 접근하고 있으나, 2번째는 \*\*\*\*`str[startIndex]`, `str[index(_:offsetBy:)]`와 같은 문법을 사용하여 원하는 위치의 문자에 접근할 수 있도록 하였다.
      - 그러나 위의 코드 중 주의해야 할 것은 위 코드에서 언급한 것처럼, 이러한 방식으로 가져온 인덱스를 통해 직접 값을 수정하거나 변경할 수 없다는 것이다.
      - 대신 다른 방법을 사용하여 새로운 문자열을 생성하거나 기존 문자열을 조작해야 합니다. 그렇기에 문자열 내부의 각 문자에 접근하거나 값을 읽는 등 다른 방식으로 활용할 수 있다.
      - 즉, `subscript`를 통한 인덱스 접근이 가능한 이유는 Swift에서 제공하는 일반적인 컬렉션 타입(`Collection`)으로서의 동작과 기능 덕분이다.
  - ❓: 그럼에도 나는 String은 왜 subscript로 접근이 가능케 하고 싶은데 어떻게 해야해?
    1️⃣ : \*\*\*\*정수 인덱스를 사용할 때 : `String`의 정수 인덱스를 사용하여 문자에 접근할 때에 문자열 내의 유효한 위치를 나타낸다.
    ```swift
    let str = "Hello, World!"

    let firstCharacter = str[str.startIndex]
    print(firstCharacter)  // "H"

    let thirdCharacter = str[str.index(str.startIndex, offsetBy: 2)]
    print(thirdCharacter)  // "l"
    ```
    2️⃣ : 문자열 인덱스 사용할 때 :  `String.Index` 타입을 사용하여 문자열 내의 특정 위치에 대한 인덱스를 얻고, 해당 인덱스를 통해 문자에 접근할 수 있다.
    ```swift
    let str = "Hello, World!"

    let index = str.index(str.startIndex, offsetBy: 7)
    let character = str[index]
    print(character)  // "W"
    ```
    3️⃣ : Range(범위) 사용할 때 : `subscript`를 통해 범위(Range)를 지정하여 부분 문자열(substring)을 가져올 수 있다.
    ```swift
    let str = "Hello, World!"

    let range = str.index(str.startIndex, offsetBy: 7)...str.index(str.endIndex, offsetBy: -2)
    let substring = str[range]
    print(substring)  // "World"
    ```
    4️⃣ : ClosedRange(폐구간), PartialRangeFrom(시작부터 끝까지), PartialRangeThrough(시작부터 끝까지 포함) 등 다양한 범위 연산자와 함께 subscript로 접근할 수도 있다.
    ```swift
    // Corrected code
    let indexStart = str.index(str.startIndex, offsetBy: 3)
    let indexEnd = str.index(str.startIndex, offsetBy: 7)
    let closedRangeSubstring1 = str[indexStart...indexEnd]

    let indexFrom = str.index(str.startIndex, offsetBy: 8)
    let partialRangeFromSubstring2 = str[indexFrom...]

    let indexThrough = str.index(str.startIndex, offsetBy: 4) // the end is exclusive in through range
    let partialRangeThroughSubstring3 = str[...indexThrough]
    ```

---

## instance 메서드와 class 메서드의 차이점을 설명하시오.

- **인스턴스 메서드 (Instance Method)**
  ```swift
  struct ExampleStruct{
    var count: Int = 0
    mutating func countUp(){ // 구조체의 인스턴스 메서드
          self.count += 1
    }

  var exampleStruct = ExampleStruct()
  exampleStruct.countUp()
  ```
  - Swift에서 인스턴스 메서드(instance method)는 특정 클래스, 구조체, 또는 열거형의 인스턴스에 속한 함수를 의미한다.
  - 인스턴스 메서드는 해당 타입의 새로운 인스턴스를 생성한 후에만 호출할 수 있습니다. 이는 반드시 실체화를 하여야만 생성된 해당 인스턴스를 통하여 호출할 수 있다는 뜻이다.
  - 객체 지향 프로그래밍에서 매우 일반적이며, 각각의 인스턴스에 속하는 데이터나 상태를 읽거나 수정하는데 사용됩니다.
  - 위의 예시 코드에서는 `exampleStruct` 라는 새로운`ExampleStruct()` 객체를 생성해주었기 때문에 `countUp()` 이라는 인스턴스 메서드를 사용할 수 있었던 것이다.
- \***\*타입 메서드 (Type Method)\*\***

  ```swift
  class SuperClass{
      class func printName(){
          print("this is \(self)")
      }

      static func printName2(){
          print("this is \(self)")
      }
  }

  class SubClass: SuperClass {
      override class func printName(){   // >>> 오버라이드 가능
          print("this is \(self)")
      }

      override static func printName2(){ // >>> 오버라이드 불가 컴파일 에러
          print("this is \(self)")
      }

  }

  SuperClass.printName()  // 인스턴스가 아니라 타입 자체에 접근하여 메서드를 호출한다.
  SubClass.printName()
  ```

  - 타입 메서드는 특정 인스턴스에 연결되지 않고, 그 타입 자체에 연결된 메서드를 말한다.
    이는 클래스, 구조체, 또는 열거형 등에서 사용된다.
  - Swift에서는 `static` 또는 `class` 키워드를 사용하여 타입 메서드를 정의하며, static 메서드는 모든 타입(클래스, 구조체, 열거형)에서 사용 가능하며, 상속이 불가능하며 즉, 하위 클래스에서 재정의할 수 없다.
    class 메서드는 클래스에서만 사용 가능하며 하위 클래스에서 재정의(오버라이딩)할 수 있다.

- instance 메서드와 class 메서드의 차이점
  1. **호출 방식**
     1. **인스턴스 메서드: 특정 타입의 인스턴스를 생성한 후, 해당 인스턴스를 통해 호출됩니다. 즉, 객체의 상태나 속성을 변경하거나 해당 객체에 대한 작업을 수행합니다.**
     2. **클래스 메서드: 타입 자체에 직접 접근하여 호출됩니다. 인스턴스 없이도 호출할 수 있으며, 주로 타입 자체와 관련된 작업을 수행합니다.**
  2. **메모리 공간**
     1. **인스턴스 메서드 : 각각의 인스턴스마다 별도의 상태와 속성을 가지며, 이를 통해 해당 객체의 상태 및 속성에 접근할 수 있습니다**
     2. **클래스 메서드: 타입 자체에 연결되어있으며, 모든 인스턴스에서 공유됩니다**
  3. s**elf 키워드 사용**
     1. **인스턴스 메서드: 'self' 키워드를 사용하여 현재 작업 중인 인스턴스 그 자신을 참조할 수 있습니다**
     2. **클래스 메서드 : 'self' 키워드를 사용하여 현재 작업 중인 클래스의 타입을 참조할 수 있습니다.**

---
