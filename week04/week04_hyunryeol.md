# mutating 키워드에 대해 설명하시오.
> Swift에서 `mutating` 키워드는 주로 구조체(struct)나 열거형(enum) 내부 메서드에서 사용됩니다. 
> Swift의 구조체와 열거형은 값 타입(value type)이기 때문에, 
> 기본적으로 인스턴스 메서드 내에서 자신의 속성을 수정할 수 없습니다. 하지만 `mutating` 키워드를 사용하면, 그 메서드 내에서 자신의 속성을 수정하는 것이 가능해집니다.

```swift
struct Counter {
    var count = 0
    mutating func increment() { // => mutating func
        count += 1
    }
}
```
위 예시에서 `increment()` 함수는 `mutating`으로 선언되었으므로, 이 함수 내부에서는 구조체의 속성인 `count` 값을 변경할 수 있습니다.

# 탈출 클로저에 대하여 설명하시오.
> Swift에서 함수 또는 메서드의 인자로 전달되는 클로저는 기본적으로 non-escaping입니다. 이는 해당 함수 또는 메서드의 실행 동안만 실행되며, 외부에 "탈출"하지 않음을 의미합니다.
> 반면에 `@escaping` 속성을 가진 클로저는 함수 또는 메서드 외부에서도 실행될 수 있습니다. 예를 들어, 비동기 작업이나 디스패치 큐 등에 전달되어 나중에 호출될 필요가 있는 경우 escaping 속성이 필요합니다.

```swift
func asyncTask(completion: @escaping () -> Void) {
    DispatchQueue.main.async {
        completion()
    }
}
```
주요 사용 사례 중 하나는 비동기 작업입니다. 예를 들어, 네트워크 요청을 보내고 나중에 결과를 받아 처리해야 하는 경우에 탈출 클로저를 사용하게 됩니다.

위 코드에서 completion 클로저는 asyncTask 함수가 반환한 이후에 비동기적으로 호출됩니다.
탈출클로저와 관련해서 주의할 점은 self 참조입니다.
self를 참조하면 `메모리 누수(memory leak)`나 `strong reference cycle`을 일으킬 위험이 있습니다.

Swift는 이러한 문제를 방지하기 위해 `약한 참조(weak reference)`와 `비소유(unowned reference)`라는 개념을 제공합니다.

약한 참조는 `[weak self]` 구문으로 선언할 수 있으며, 이렇게 선언된 self는 Optional 형태가 됩니다. 따라서 해당 객체가 메모리에서 해제된 경우 nil이 되므로 안전합니다.

비소유 참조는 `[unowned self]` 구문으로 선언할 수 있으며, 이렇게 선언된 self는 Non-Optional 형태입니다. 따라서 해당 객체가 반드시 존재함을 보장해야 합니다. 만약 비소유참조 대상이 이미 메모리에서 해제된 상황에서 접근한다면 런타임 오류를 발생시킵니다.

따라서 탈출클로저 내부에서 `self`를 사용할 때에는 주의해서 사용해야 하며 필요에 따라 `weak`나 `unowned` 키워드를 활용하는 것이 중요합니다.

# Optional 이란 무엇인지 설명하시오.
> Optional은 `nil` 값을 가질 수 있는 변수나 상수를 선언하는 데 사용됩니다.

## 선언 방법
```swift
var optionalInteger: Int? = nil
```

### Forced Unwrapping
옵셔널 변수 뒤에 느낌표(!)를 붙여 강제로 언래핑할 수 있습니다.
하지만 만약 옵셔널 값이 `nil`인 경우 실행 시간 오류가 발생 하므로 주의해서 사용해야 합니다.
```swift
var optionalInteger: Int? = 100
let unwrappedInteger: Int = optionalInteger!
```

### 옵셔널 바인딩(Optional Binding)
if-let 구문 등을 활용하여 안전하게 언래핑할 수 있는 방법입니다.
```swift
var optionalInteger: Int? = 100
if let unwrappedInteger = optionalInteger {
    print(unwrappedInteger)
}
```

### Nil-coalescing Operator
옵셔널 값이 nil일 경우 기본값(default value)를 제공하는 연산자입니다
```swift
var optionalString: String? 
let value = optionalString ?? "default string"
```
### nil?
> `nil` 은 `null` 이랑 같지만 다름
> Objective-C에서의 nil은 존재하지 않는 `객체에 대한 포인터`이고,
> Swift에서의 nil은 포인터가 아니라, 단지 `특정 타입에 대한 값의 부재`를 보여주는 것이다.
# MVC 구조에 대해 블록 그림을 그리고, 각 역할과 흐름을 설명하시오.
> 애플리케이션의 로직을 Model, View, Controller 세 부분으로 분리합니다. 이렇게 하는 이유는 각 부분의 역할을 명확하게 구분하고 코드의 재사용성과 유지보수성을 높이기 위함

#### **Model**
애플리케이션의 정보(data)와 그 데이터를 처리하는 로직(business logic)을 관리합니다. 일반적으로 데이터베이스 CRUD(Create, Read, Update, Delete) 작업 등을 담당합니다.
    
#### **View**
사용자에게 보여지는 UI(User Interface)를 관리합니다. 사용자가 보거나 상호작용하는 모든 요소(버튼, 텍스트 필드 등)를 포함합니다.
    
#### **Controller**
Model과 View 사이의 인터페이스 역할을 합니다. 사용자가 View에서 행한 동작(예: 버튼 클릭 등)에 대한 응답으로 Model에 데이터 변경을 요청하거나 Model에서 데이터를 가져와 View를 업데이트하는 등의 작업을 수행합니다.

### 동작 흐름
1. 사용자가 View에서 어떤 동작(예: 버튼 클릭 등)을 수행하면 해당 정보가 Controller에 전달됩니다.
2. Controller는 받은 정보를 바탕으로 필요한 데이터 처리(Model 변경 요청 등)를 진행하고 그 결과를 받아옵니다.
3. Controller는 받아온 결과값으로 View를 업데이트하여 최신 상태로 유지시킵니다.
