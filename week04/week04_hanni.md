# mutating 키워드에 대해 설명하시오.
**`mutating`** 키워드는 Swift에서 구조체(Structures)나 열거형(Enumerations) 내부에서 함수(Methods)에서 해당 구조체나 열거형의 속성(프로퍼티)을 변경할 때 사용한다. 

이 키워드를 사용하면 해당 함수가 구조체나 열거형의 내부 값을 수정할 수 있도록 허용된다. 

→ 복사된 것(내부의 값을 변경한 그 값)이 원래 값에 영향을 주게 된다. 

왜냐하면 Swift에서는 기본적으로 구조체와 열거형이 상수로 선언되면 내부의 프로퍼티도 변경할 수 없게끔 설계되어 있기 때문이다.

구조체 내부에서 프로퍼티를 변경하는 함수를 작성할 때 **`mutating`** 키워드를 사용해야 한다.

- 사용 예시
    
    ```swift
    struct Point {
        var x = 0
        var y = 0
    		// 이 메서드는 mutating을 사용해 원래 값에 영향을 준다.
        mutating func moveX(newX: Int) {
            x = newX
        }
    }
    
    struct Point {
        var x = 0
        var y = 0
    
        func moveX(newX: Int) {
            // 이 메서드는 mutating이 없기 때문에 새로운 복사된 구조체에서만 값을 변경한다.
            x = newX // 에러! 구조체의 프로퍼티를 변경할 수 없음
        }
    }
    ```
    

## class에서는 mutating을 사용하지 않나요?

클래스의 인스턴스를 가리키는 참조는 메모리에서 해당 인스턴스를 가리키고 있기 때문에 mutating을 사용할 필요가 없다.

하지만 구조체와 열거형은 값 타입이므로 내부의 값이 변경되면 해당 값이 복사된 것이므로 원래 값에 영향을 주기 위해서는 **`mutating`** 키워드를 사용해야 한다.

# 탈출 클로저에 대하여 설명하시오.
[탈출 클로저(escaping closure)](https://docs.swift.org/swift-book/documentation/the-swift-programming-language/closures#Escaping-Closures)란, 클로저가 함수의 외부로 전달되어 저장될 때 클로저가 함수의 실행이 끝난 후에도 여전히 호출될 수 있는 클로저를 의미한다. 

일반적으로 클로저는 해당 함수의 스코프 내에서 실행될 것으로 예상된다. 하지만 탈출 클로저는 함수가 반환된 후에도 여전히 호출될 수 있으므로 해당 함수의 스코프를 벗어날 수 있다.

탈출 클로저 예시

- 비동기 작업에서 완료 핸들러로 사용되는 클로저
    - 비동기 함수가 시작된 후에도 완료 클로저가 호출될 수 있어야 한다. 이런 경우의 클로저는 함수의 스코프를 벗어나서 저장되어야 한다.

코드 예시 (**`fetchData`** 함수의 실행이 끝난 후에도 **`completion`** 클로저가 호출될 수 있다.)

```swift
func performAsyncTask(completion: @escaping (Int) -> Void) {
    // 비동기 작업 시뮬레이션 (백그라운드 스레드에서 실행)
    DispatchQueue.global().async {
        // 작업이 완료된 후에 결과를 클로저로 전달
        let result = 42 // 비동기 작업 결과 (예시로 42를 반환)
        completion(result) // 탈출 클로저 호출
    }
}

// 비동기 작업을 시작하고 작업이 완료된 후에 결과를 출력
performAsyncTask { result in
    print("작업이 완료되었습니다. 결과는 \(result)입니다.")
}

print("비동기 작업을 시작합니다.")
```

## 탈출 클로저를 사용하는 때는?

클로저가 함수의 스코프를 벗어나서 호출될 필요가 있는 상황에서 사용된다.

1. **비동기 작업과 콜백 처리:** 비동기 함수는 작업이 완료된 후에 결과를 전달하거나, 에러가 발생했을 때 에러를 처리할 때 탈출 클로저를 사용한다. 비동기 작업이 완료되면 해당 클로저를 호출하여 결과를 반환하거나 에러를 처리할 수 있다.
2. **프로퍼티 옵저버:** 객체의 프로퍼티 값이 변경될 때 특정 동작을 수행하고자 할 때 프로퍼티 옵저버를 사용할 수 있다. 프로퍼티 옵저버에서 클로저를 사용할 때 탈출 클로저를 활용한다. 프로퍼티 값이 변경될 때마다 클로저가 호출되어 원하는 작업을 수행할 수 있다.

## 탈출 클로저가 가장 필요한 때는?

주로 비동기 작업을 할 때이다.  비동기 작업은 대부분 다른 스레드에서 실행되고, 작업이 완료되면 클로저가 호출된다. 

이때 클로저가 함수의 스코프를 벗어나서 호출되어야 하므로 탈출 클로저를 사용한다. 

또한, 콜백 처리나 델리게이션 패턴과 같은 경우에도 탈출 클로저를 사용하여 함수 외부에서 클로저를 저장하고 호출할 수 있다.

따라서, 비동기 작업을 수행하거나 외부에서 함수 내부의 클로저를 호출해야 하는 경우에는 탈출 클로저를 사용한다. 이렇게 하면 비동기 작업의 결과나 다른 객체의 상태 변화 등을 적절히 처리할 수 있다.

## 함수의 [스코프](https://blog.codingcat.kr/139)?

스코프는 어디에서 특정 변수, 함수 또는 클로저에 접근할 수 있는 지 그 범위를 결정해준다.

스코프 내에 변수, 함수, 또는 클로저가 없는 경우 대상에 접근할 수 없다.

## `[self` 를 참조하는 탈출 클로저](https://bbiguduk.gitbook.io/swift/language-guide-1/closures#escaping-closures)

### 특징

- 이스케이프 클로저에 `self` 캡처는 강한 참조 사이클이 생기기 쉽다. (참조 사이클 : [자동 참조 카운팅 (Automatic Reference Counting)](notion://www.notion.so/swift/language-guide-1/automatic-reference-counting))
- 일반적으로 클로저는 클로저 내부에서 변수를 사용하여 암시적으로 변수를 캡처한다. 하지만 self를 참조하는 탈출 클로저인 경우에는 명시적이어야 한다.
    
    → `self` 를 캡처하려면 사용할 때 명시적으로 `self` 를 작성하거나 클로저의 캡처 리스트에 `self` 를 포함한다.
    

```swift
func someFunctionWithNonescapingClosure(closure: () -> Void) {
    closure()
}

class SomeClass {
    var x = 10
    func doSomething() {
        someFunctionWithEscapingClosure { self.x = 100 }    // self 명시적으로 사용
        someFunctionWithNonescapingClosure { x = 200 }
    }
}

let instance = SomeClass()
instance.doSomething()
print(instance.x)
// Prints "200"

completionHandlers.first?()
print(instance.x)
// Prints "100"
```

# Optional이란 무엇인지 설명하시오.
## **옵셔널 (Optional)**

Swift가 가지고 있는 가장 큰 특징이다.

Optional → '선택적인'

즉, 값이 있을 수도 있고 없을 수도 있는 것을 나타냅니다.

### 값이 없는 경우를 나타낼 때에는 `nil`을 사용한다.

- 문자열
    - 값이 있으면 `"가나다"`가 된다. 값이 없다면 `""`일까? `""`도 엄연히 값이 있는 문자열이며 정확히는 '값이 없다'가 아니고 '빈 값'이다.
    - 값이 없는 문자열은 바로 `nil`이다.
- 정수형
    - 값이 있으면 `123`과 같은 값이 값이 없다면 `0`일까? 마찬가지로 `0`은 `0`이라는 숫자 '값'이다.
    - 값이 없는 정수는 `nil`이다.
- 빈 배열, 빈 딕셔너리
    - '값이 없는'것이 아니라 '비어 있을' 뿐이다.
    - 배열과 딕셔너리의 경우에도 '없는 값'은 `nil`이다.

그렇다고 해서 모든 변수에 `nil`을 넣을 수 있는 것은 아니다. 

예로, 우리가 위에서 정의한 `name`이라는 변수에 `nil`을 넣으려 하면 에러가 발생한다.

```swift
var name: String = "전수열"
name = nil // 컴파일 에러!
```

값이 있을 수도 있고 없을 수도 있는 변수를 정의할 때에는 타입 어노테이션에 `?`를 붙여야 한다. 

옵셔널에 초깃값을 지정하지 않으면 기본값은 `nil`이다.

```swift
var email: String?
print(email) // nil

email = "devxoul@gmail.com"
print(email) // Optional("devxoul@gmail.com")
```

옵셔널로 정의한 변수는 옵셔널이 아닌 변수와는 다르다. 아래와 같은 코드는 사용할 수 없다.

```swift
let optionalEmail: String? = "devxoul@gmail.com"
let requiredEmail: String = optionalEmail // 컴파일 에러!
```

### 옵셔널 개념

```
        ,-- 어떤 값 (String, Int, ...)
Optional
        `-- nil
```

# MVC 구조에 대해 블록 그림을 그리고, 각 역할과 흐름을 설명하시오.

```swift
----------------         ----------------        ----------------
|    Model       |      |    View        |      |   Controller   |
| - Data         |<---->| - UI Elements  |<---->| - User Input   |
| - Business     |      | - Display Logic|      | - Data Logic   |
|   Logic        |      |   (What to     |      |   (How to      |
|                |      |   display)     |      |   handle data) |
----------------         ----------------        ----------------
```

## Model
- **역할:** 데이터와 데이터 조작을 책임진다.
- **구성 요소:** 앱에서 사용하는 데이터, 데이터 조작 로직 및 비즈니스 규칙이 포함된다.
- **흐름:** Controller에서 데이터 조작을 요청하면 Model이 이를 수행하고, 변경된 데이터를 Controller에 알리고, 필요할 경우 View에 변경을 알린다.

## View
- **역할:** 사용자 인터페이스를 표시하고 사용자의 입력을 받아 Controller에 전달한다.
- **구성 요소:** 화면에 나타나는 UI 요소들로 구성된다. (예: 버튼, 레이블, 이미지 등)
- **흐름:** Model의 데이터를 표시하고, 사용자 입력을 Controller로 전달하여 Model에 반영하도록 요청한다.

## Controller
- **역할:** Model과 View 간의 중개자 역할을 하며, 사용자 입력에 대한 로직을 처리하고 Model과 View를 업데이트한다.
- **구성 요소:** 사용자 입력 처리 로직, Model의 데이터 요청 및 업데이트 로직, View의 표시 요청 등이 포함된다.
- **흐름:** 사용자 입력이 발생하면 Controller가 이를 받아들이고, 필요한 데이터 조작을 Model에 요청하여 데이터를 업데이트한 후, 변경된 데이터를 View에 반영하도록 요청한다.


## 다른 패턴들과 MVC를 비교했을 때, 각각의 장단점

**MVC (Model-View-Controller)**

- **장점:**
    - 분리된 역할로 유지보수가 쉽고, 확장성이 높다.
    - 코드의 재사용성이 높아지며, 각 부분을 독립적으로 테스트할 수 있다.
- **단점:**
    - 복잡한 앱에서는 모델과 뷰 간의 의존성이나 컨트롤러의 복잡성이 증가할 수 있다.
    - 대규모 앱에서 모델과 뷰 간의 동기화와 업데이트 관리가 복잡해질 수 있다.

**MVVM (Model-View-ViewModel)**

- **장점:**
    - 뷰와 로직을 느슨하게 결합시켜 유닛 테스트 및 UI 테스트가 쉬워진다.
    - 바인딩을 통해 데이터의 양방향 흐름을 지원하므로 코드 양을 줄일 수 있다.
- **단점:**
    - 초보자가 이해하고 구현하기 어려울 수 있다.
    - 데이터 바인딩의 오버헤드로 성능에 영향을 미칠 수 있다.

**VIPER (View-Interactor-Presenter-Entity-Routing)**

- **장점:**
    - 역할이 더 분리되어 각 컴포넌트의 책임이 명확해진다.
    - 모듈성과 확장성이 높아 대규모 앱에서 유용하다.
- **단점:**
    - 구현이 복잡해질 수 있고, 초보자에게는 이해하기 어려울 수 있다.
    - 작은 프로젝트나 간단한 앱에서는 오버 엔지니어링일 수 있다.

## **선택 기준**

- **프로젝트 규모**
    - 작은 프로젝트 → 간단한 구조인 MVC
    - 대규모 프로젝트 → MVVM이나 VIPER와 같은 더 분리된 구조
- **팀의 경험**
    - 팀원들이 특정 패턴에 더 익숙하다면 해당 패턴을 선택하는 것이 합리적일 수 있다.
- **유지보수성과 확장성**
    - 장기적인 유지보수성과 확장성을 고려하여 각 패턴의 장단점을 비교하여 선택할 필요가 있다.