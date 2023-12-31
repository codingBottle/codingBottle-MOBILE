# 질문 리스트
> **_hanni님의 질문_**
- mutating 키워드에 대해 설명하시오.
- 탈출 클로저에 대하여 설명하시오.

> **_PSY님의 질문_**
- Optional 이란 무엇인지 설명하시오.
- MVC 구조에 대해 블록 그림을 그리고, 각 역할과 흐름을 설명하시오.

## mutating 키워드에 대해 설명하시오.
구조체(Struct)나 열거형(Enum) 내부의 메서드(Method)가 해당 구조체나 열거형의 속성(Property)을 변경할 수 있도록 허용하는 데 사용.
```
//mutating 예제
struct Point {
    var x: Double
    var y: Double

    // 이 메서드는 구조체의 속성을 변경할 수 있도록 mutating 키워드를 사용합니다.
    mutating func moveBy(x deltaX: Double, y deltaY: Double) {
        x += deltaX
        y += deltaY
    }
}

```
### mutaiting을 사용하는 이유

> - 값 타입의 불변성 유지 
: Swift에서 구조체와 열거형은 값 타입(Value Types)으로, 인스턴스가 복사될 때마다 독립적인 메모리 공간에 저장. 이러한 특성으로 인해 값을 복사하는 방식으로 데이터의 불변성(Immutability)을 유지할 수 있음. 때로는 값을 변경해야 할 필요가 있는데 이때 mutating을 사용하여 값 타입 내의 메서드에서 속성을 변경할 수 있도록 허용.
 - 메서드 내부에서 속성 변경 
: mutating을 사용하면 값 타입 내부에서 해당 값 타입의 속성을 수정할 수 있음. 값 타입은 일반적으로 불변성을 가지므로 속성을 변경하기 위해 mutating 키워드를 사용.
- 복사 대신 직접 수정
: 값 타입의 인스턴스를 변경하려면 일반적으로 인스턴스를 복사하여 수정하는 것이 아니라 직접 수정하는 것이 효율적임. mutating 키워드를 사용하면 인스턴스를 복사하지 않고도 메서드 내에서 인스턴스를 직접 수정할 수 있으므로 성능 상 이점이 있음.
- 코드 가독성 향상
: mutating 키워드를 사용하면 메서드가 해당 인스턴스의 상태를 변경할 수 있음을 명시적으로 나타내므로 코드의 의도를 명확하게 표현할 수 있음. 코드가 다른 개발자나 본인에게 더 명확하게 이해되도록 도와줌.

### 질문
- mutating 키워드와 함께 사용하는 것이 가독성과 코드 유지보수 측면에서 왜 중요한가?
- mutating 키워드는 어떤 상황에서 특히 유용한가?
- 구조체와 열거형에서 mutating의 동작 방식이 다른가?

## 탈출 클로저에 대하여 설명하시오.
클로저가 함수나 메서드의 범위를 벗어나서 나중에 비동기적으로 실행될 때를 가리킴. 클로저가 함수가 반환된 후에 실행되거나 다른 스레드에서 실행될 수 있음을 의미.

> - 범위를 벗어남: 탈출 클로저는 해당 함수나 메서드의 범위를 벗어나서 저장되거나 다른 함수나 메서드로 전달될 수 있음.
- 비동기 작업: 주로 비동기 작업을 수행할 때 사용. 클로저가 비동기 작업이 완료된 후 호출되거나, 나중에 이벤트 핸들러로 사용될 때 탈출 클로저를 사용.
- @escaping : 함수나 메서드 매개변수에 탈출 클로저를 전달하려 @escaping 키워드를 사용하여 명시적으로 표시해야함.

```
// 비동기 작업 탈출 클로저 예제
func performAsyncTask(completion: @escaping () -> Void) {
    DispatchQueue.global().async {
        // 비동기 작업 수행
        // 작업이 완료되면 클로저 호출
        completion()
    }
}
```

### 질문
- 탈출 클로저를 사용할 때 메모리 관리에 주의해야 하는 이유가 무엇인가?
- 탈출 클로저와 비탈출 클로저를 같은 함수나 메서드에서 혼용해서 사용할 수 있는지, 그렇다면 어떤 제약 사항이 있는지 있는가?

## Optional 이란 무엇인지 설명하시오.
값이 존재할 수도 있고 존재하지 않을 수도 있는 데이터 형식. 값이 없는 상황에 대한 명시적인 처리를 가능하게 하며, 안전성을 높이고 예기치 않은 오류를 방지하는 데 도움.

```
//Optional 예제
var userName: String? = "John"

// Optional 바인딩을 사용하여 값이 있는 경우에만 출력
if let name = userName {
    print("사용자 이름: \(name)")
} else {
    print("사용자 이름이 없습니다.")
}

// 값이 없는 경우 기본값 사용
let userAge: Int? = nil
let age = userAge ?? 30 // userAge가 nil인 경우 기본값 30 사용

// 옵셔널 체이닝을 통한 안전한 접근
class Person {
    var name: String?
}

let person: Person? = Person()
let personName = person?.name // person이 nil이 아니라면 name에 접근, 그렇지 않으면 nil

```

> - 값의 존재 여부 표현
: Optional은 값이 있을 때와 없을 때를 나타냅니다. 값이 있으면 해당 값을 갖고 있고, 값이 없으면 "nil"을 갖음.
- ? 연산자를 사용한 선언
: Optional을 사용하려면 변수나 상수의 선언 시에 "?" 연산자를 사용하여 해당 데이터 형식을 Optional로 표시.  ex) var -age: 
Int?은 "age" 변수가 Optional Int 타입임을 나타냄.
- 안전한 접근
: Optional 값을 사용할 때는 값을 가져오기 전에 nil 여부를 확인하고 안전하게 접근할 수 있는 방법을 제공합니다. 이를 위해 "옵셔널 바인딩"이나 "옵셔널 체이닝"과 같은 기능을 사용할 수 있음.
- 예외 처리 방지
: Optional을 사용하면 예기치 않은 값이 없는 상황에서 발생하는 예외를 방지할 수 있음. 값이 없는 경우 명시적으로 처리하도록 유도하므로 코드의 안정성을 향상시킴.
- 기본값 설정
: Optional 변수에 기본값을 설정할 수 있으며, 값이 없을 때 기본값이 사용. 이를 통해 Optional 변수를 사용할 때 항상 초기값이 있도록 할 수 있음.

### 질문
- 옵셔널 바인딩은 어떤 개념이며, Optional 값을 안전하게 추출하기 위해 왜 사용하는가?
- 옵셔널 체이닝은 어떤 원리로 동작하며, 어떤 상황에서 유용하게 사용되는지 예시가 있는가?


## MVC 구조에 대해 블록 그림을 그리고, 각 역할과 흐름을 설명하시오.
MVC (Model-View-Controller)는 소프트웨어 디자인 패턴 중 하나로, 애플리케이션을 세 가지 주요 컴포넌트로 분리하여 구성. 이 세 가지 컴포넌트는 서로 다른 역할을 가지며, 애플리케이션의 구조와 흐름을 정의.
```
                        ┌───────────────┐
                        │  모델 (Model)  │
                        │               │
                        └───────────────┘
                                 ▲
                                 │
                                 │
              ┌──────────────────┼──────────────────────┐
              ▼                  ▼                      ▼
  ┌─────────────────┐   ┌─────────────────┐   ┌─────────────────┐
  │    뷰 (View)    │    │     컨트롤러      │   │    뷰 (View)    │
  │                 │   │   (Controller)  │   │                 │
  └─────────────────┘   └─────────────────┘   └─────────────────┘
              ▲             │
              │             │
              └─────────────┘
                   │
                   ▼
            ┌───────────────┐
            │   사용자 입력    │
            └───────────────┘

```

- Model (모델):

모델은 애플리케이션의 데이터와 비즈니스 로직을 담당.
데이터베이스, 네트워크 요청, 파일 시스템과 같은 데이터 저장소와 상호 작용함.
모델은 데이터의 상태를 추적하고 변경 사항을 통지하기 위해 옵저버 디자인 패턴을 사용할 수 있음.
- View (뷰):

뷰는 사용자 인터페이스(UI)를 나타내며, 사용자에게 정보를 표시하고 사용자 입력을 받는 역할을 함.
뷰는 모델의 데이터를 표시하고 사용자와 상호작용을 통해 이벤트를 생성.
사용자 입력을 받아 컨트롤러에 전달하거나 모델을 업데이트하는 등의 동작을 수행.
- Controller (컨트롤러):

컨트롤러는 모델과 뷰 사이의 중개 역할을 함.
사용자 입력을 처리하고 해당 입력에 대한 적절한 모델 조작을 수행.
모델과 뷰 간의 동기화를 관리하고 데이터의 변화를 뷰에 반영.
MVC 패턴에서 컨트롤러는 모델과 뷰 사이의 결합을 방지하여 유지보수성과 확장성을 향상.

### 질문
- MVC (Model-View-Controller) 구조를 사용하는 이유?
- 모델이 변경될 때 뷰는 어떻게 업데이트되는가?