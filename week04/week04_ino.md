# mutating & @escaping & optional & MVC

# mutating

특정 메소드 내에서 구조체 또는 열거형의 프로퍼티를 수정해야 하는 경우, 해당 메소드의 동작을 변경하도록 하는 것

- 실패 예시

```swift
struct Person {
    let name: String
    var age: Int

    init(name: String, age: Int) {
        self.name = name.uppercased()
        self.age = age
    }

    func changeAge() {
        age = 10       // Cannot assign to property: 'self' is immutable
    }
}
```

- 성공 예시

```swift
struct Person {
    let name: String
    var age: Int

    init(name: String, age: Int) {
        self.name = name.uppercased()
        self.age = age
    }

    mutating func changeAge() {
        age = 10
    }
}

var man = Person(name: "Song", age: 24)
man.changeAge()
print(man.age)
```

에러 발생 이유는? 

swift의 구조체는 값타입임 → 값타입의 인스턴스 프로퍼티 변경시 원본값 변경이 아닌 인스턴스를 복사하여 변경하기에 프로퍼티를 변경해줘도 소용없음 why ? → 원본값은 그대로.

mutating 키워드를 붙여 메서드를 사용할 경우 메서드가 종료될 때 원본 구조체 인스턴스 → 프로퍼티가 변경된 구조체 인스턴스로 대체함.

1. mutating 키워드를 사용하지 않을 경우 에러가 발생하는 이유는 ?
2. mutating 키워드의 동작 원리는 ?

# @escaping

Escaping 클로저는 클로저가 함수의 인자로 전달됐을 때, 함수의 실행이 종료된 후 실행되는 클로저 입니다. Non-Escaping 클로저는 이와 반대로 함수의 실행이 종료되기 전에 실행되는 클로저 입니다.

```swift
func runClosure(closure: () -> Void) {
 closure()
}
```

클로저가 실행되는 순서를 보면

1. 클로저가 `runClosure()` 함수의 `closure` 인자로 전달됨
2. 함수 안에서 `closure()` 가 실행됨
3. `runClosure()` 함수가 값을 반환하고 종료됨

이렇게 클로저가 함수가 종료되기 전에 실행되기 때문에 `closure`는 Non-Escaping 클로저 입니다.

```swift
class ViewModel {
    var completionhandler: (() -> Void)? = nil

    func fetchData(completion: @escaping () -> Void) {
        completionhandler = completion
    }
}
```

클로저가 실행되는 순서를 보면

1. 클로저가 `fetchData()` 함수의 `completion` 인자로 전달됨
2. 클로저 `completion`이 `completionhandler` 변수에 저장됨
3. `fetchData()` 함수가 값을 반환하고 종료됨
4. 클로저 `completion`은 아직 실행되지 않음

`completion`은 함수의 실행이 종료되기 전에 실행되지 않기 때문에 escaping 클로저, 다시말해 함수 밖(escaping)에서 실행되는 클로저 입니다.

escaping 클로저가 사용되는 흔한 예로는 비동기로 실행되는 HTTP Request CompletionHandler이 있습니다.

```swift
func makeRequest(_ completion: @escaping (Result<(Data, URLResponse), Error>) -> Void) {
  URLSession.shared.dataTask(with: URL(string: "[http://jusung.github.io/](http://jusung.github.io/)")!) { data, response, error in
    if let error = error {
      completion(.failure(error))
    } else if let data = data, let response = response {
      completion(.success((data, response)))
    }
  }
}
```

`makeRequest()` 함수에서 사용되는 `completion` 클로저는 함수 실행 중에 즉시 실행되지 않고, URL 요청이 끝난 후 비동기로 실행 됩니다. 이 경우에도 `completion`의 타입에 `@escaping` 을 붙여서 escaping 클로저라는 것을 명시해줘야 합니다.

보통 클로저가 다른 변수에 저장되어 나중에 실행되거나 비동기로 실행될 때 escaping 클로저가 사용됩니다.

escaping 클로저에는 반드시 escaping 일 필요 x , 반대는 반드시 필요함.

but 파라미터로 받을 클로저가 있을 수도 , 없을 수도 있으니

클로저를 옵셔널로 선언하면?

Closure is already escaping in optional type argument Remove “@escaping” 오류 발생

fix 를 할 경우 → @escaping 이 사라짐

****파라미터 타입이 Optional Closure일 경우,  더이상 Closure 파라미터 타입이 아닌 Optional 파라미터 타입이다.****

옵셔널을 붙이는 순간 **() -> ()라는 함수 타입을 제네릭 함수의 Type Parameter로 받은 "옵셔널 타입"이 되는것.**

따라서 기본적으로 파라미터로 받는 ‘클로저’는 함수 흐름을 탈출하지 못한다는 명제에 위반하지 않는것.

why ? → ((() → ())? 타입은 더 이상 클로저가 아님. 

클로저 파라미터 타입에 @escaping을 명시하면 escaping 클로저와 non-escaping 클로저를 모두 사용할 수 있는데, 그러면 그냥 모든 클로저 파라미터 타입에 @escaping 를 붙여서 사용하면 이유는 ?

→ 컴파일러의 퍼포먼스와 최적화

non-escaping 클로저는 컴파일러가 클로저의 실행이 언제 종료되는지 알기 때문에, 때에 따라 클로저에서 사용하는 특정 객체에 대한 retain, release 등의 처리를 생략해 객체의 라이프싸이클(life-cycle)을 효율적으로 관리할 수 있습니다.

반면 esacping 클로저는 함수 밖에서 실행되기 때문에 클로저가 함수 밖에서도 적절히 실행되는 것을 보장하기 위해, 클로저에서 사용하는 객체에 대한 추가적인 참조싸이클(reference cycles) 관리 등을 해줘야 합니다. 이 부분이 컴파일러의 퍼포먼스와 최적화에 영향을 끼치기 때문에 Swift에서는 필요할 때만 escaping 클로저를 사용하도록 구분해 두었습니다.

1. 굳이 escaping 클로저와 non-escaping 클로저로 나누어서 처리하는 이유는 무엇인가?
2. @escaping 키워드를 사용하지 않고 optional을 통해 파라미터를 받으면 함수 외부에서 escaping 키워드 없이 실행이 가능한데 가능한 이유는 무엇인가?

# optional

A type that represents either a wrapped value or the absence of a value.

있을수도, 없을 수도 있는 value 의 안전한 처리를 위해 사용

사용 방법 → 선언 시에 ? 를 붙여 사용

```swift
var hello : String? = "Hello"
```

1. 옵셔널 바인딩
    
    if let , if var 구문과 함께 사용
    
    value 값의 유무에 따라 분기처리
    
    ```swift
    var hello : String? = nil
    if let hello = print("Hello")
    ```
    
    값이 존재할 경우만 다음 행 진행, 없을 경우 아무것도 나오지 않음
    
2. 옵셔널 체이닝
    
    옵셔널 체이닝은 하위 property에 optional 값이 있는지 연속적으로 확인하면서, 중간에하나라도 nil이 발견된다면 nil이 반환되는 형식
    
    ```swift
    class Person {
        var residence: Residence?
    //residence라는 변수가 Residence 클래스를 상속 
    // but Optional기호 ?를 같이 있음
     //밑에서 Person타입의 인스턴스가 만들어지면 residence변수의 초기값은 nil이 됨
    }
    class Residence {
        var numberOfRooms = 1
    }
    
    let inho = Person()
    //여기서 Person타입의 인스턴스가 만들어짐
    // inho의 프로퍼티로 residence가 있겠죠?
    // 하지만 residence변수는 Residence클래스를 
    //옵셔널 타입으로 상속받고 있기 때문에 
    // residence에는 값이 있을수도, 또는 없을수도 있음
    // 옵셔널 타입은 따로 초기화를 하지 않으면 nil로 초기화
    // -> inho 의 residence의 값은 nil이겠네요!
    
            
    if let roomCount = inho.residence?.numberOfRooms {
        print("inho's residence has \(roomCount) room(s).")
    } else {
       print("Unable to retrieve the number of rooms.")
    }
    ```
    
    옵셔널 체이닝이 쓰여진 곳
    
    ```swift
    let roomCount = inho.residence?.numberOfRooms
    ```
    
    프로퍼티를 통해 계속 체인처럼 이어져 있음
    
    inho 의 residence가 nil이 아니라면, 다음으로 넘어가
    
    residence의 numberOfRooms를 또 확인.
    
    여기서는 inho의 residence가 nil이기 때문에 (위의 주석 설명)
    
    else문을 수행 위 코드가 if문의 조건을 수행하게 하려면,
    
    inho.residence = Residence()
    
    라는 코드를 추가해주면 된다.
    
    그러면 nil이 였던 residence의 값이 더이상 nil이 아니게 되고,
    
    inho**'s residence has 1 room(s).**
    
    라는 결과를 주겠네요 :)
    
    그러면 여기서 궁금한 점
    
    ```swift
    let roomCount = inho.residence?.numberOfRooms
    ```
    
    "이부분에서 왜 residence뒤에 ?표시가 붙었지?"
    
    이유는 **residence가 nil을 반환할 수도 있고 아닐 수도 있기 때문**
    
    **하위 프로퍼티에 Optional 값이 있는지 연속적으로 확인하면서중간에 하나라도 nil이 발견된다면 nil이 반환하는 것이 옵셔널 체이닝 방식**
    
    1. 옵셔널 체이닝을 사용하는 이유는?
    2. 옵셔널 체이닝이 사용될 경우, 옵셔널 체이닝이 사용된 값의 타입은 어떻게 지정되는가?
    
    MVC 패턴
    
    ```mermaid
    graph TD
      Model --> View 
    	Controller --> Model
    	View --> User --> Controller
    ```
    
    ### **1.1. Model**
    
    > 데이터(data) 가공을 책임지는 컴포넌트
    > 
    
    모델은 어플리케이션의 정보, 데이터를 나타냅니다. 데이타베이스, 초기화 된 상수나 값, 변수 등을 뜻합니다. 비즈니스 로직을 처리한 후 모델의 변경 사항을 컨트롤러와 뷰에 전달합니다.
    
    모델은 다음과 같은 규칙을 가지고 있습니다.
    
    - 사용자가 편집하길 원하는 모든 데이터를 가지고 있어야 합니다.
    - 뷰나 컨트롤러에 대해서 어떤 정보도 알지 말아야 합니다.
    - 변경이 일어나면, 변경 통지에 대한 처리 방법을 구현해야만 합니다.
    
    ### **1.2. View**
    
    > 사용자에게 보여지는 부분, 즉 유저 인터페이스(User interface)를 의미합니다.
    > 
    
    MVC 패턴은 여러 개의 뷰가 존재할 수 있으며, 모델에게 질의하여 데이터를 전달받습니다. 뷰는 받은 데이터를 화면에 표시해주는 역할을 가지고 있습니다. 모델에게 전달받은 데이터를 별도로 저장하지 않아야 합니다. 사용자가 화면에 표시된 내용을 변경하게 되면 모델에게 전달하여 모델을 변경해야 합니다.
    
    뷰는 다음과 같은 규칙을 가지고 있습니다.
    
    - 모델이 가지고 있는 정보를 따로 저장해서는 안됩니다.
    - 모델이나 컨트롤러와 같이 다른 구성 요소들을 몰라야 됩니다.
    - 변경이 일어나면 변경통지에 대한 처리방법을 구현해야만 합니다.
    
    ### **1.3. Controller**
    
    > 모델과 뷰 사이를 이어주는 브릿지(bridge) 역할을 의미합니다.
    > 
    
    모델이나 뷰는 서로의 존재를 모르고 있습니다. 변경 사항을 외부로 알리고 수신하는 방법만 있습니다. 컨트롤러는 이를 중재하기 위한 컴포넌트입니다. 모델과 뷰에 대해 알고 있으며 모델이나 뷰로부터 변경 내용을 통지 받으면 이를 각 구성 요소에게 통지합니다. 사용자가 어플리케이션을 조작하여 발생하는 변경 이벤트들을 처리하는 역할을 수행합니다.
    
    컨트롤러는 다음과 같은 규칙을 가지고 있습니다.
    
    - 모델이나 뷰에 대해서 알고 있어야 합니다.
    - 모델이나 뷰의 변경을 모니터링 해야 합니다.
    
    ## **2. 왜 MVC pattern을 이용해야 하는가?**
    
    > 유지보수의 편리성
    > 
    
    최초 설계를 꼼꼼하게 진행한 시스템이라도 유지 보수가 발생하기 시작하면 각 기능간의 결합도(coupling)가 높아집니다. 이는 최초 설계 이념을 정했던 사람들의 부재 혹은 비즈니스 요건 변경으로 인해 필연적으로 발생하는 것 같습니다. 결합도가 높아진 시스템은 유지보수 작업 시 다른 비즈니스 로직에 영향을 미치게 되므로 사소한 코드의 변경이 의도치 않은 버그를 유발할 수 있습니다.
    
    디자인 패턴이란 개발하는 과정에서 마주치는 문제들을 해결하기 위한 방법들입니다. 선배 개발자들은 이런 문제점을 해결하기 위해 UI 시스템을 위한 책임을 기준으로 3개의 핵심 컴포넌트 모델, 뷰, 컨트롤러라는 책임을 나누었습니다. 각 컴포넌트가 자신의 수행 결과를 다른 컴포넌트에게 전달하는 프로그래밍 방식으로 결합도를 낮췄습니다. 시스템 유지보수 시에도 특정 컴포넌트만 수정하면 되기 때문에 보다 쉬운 시스템 변경이 가능합니다.
    
    - 화면의 변경은 뷰를 수정하여 반영합니다.
    - 데이터나 비즈니스 요건이 변경은 모델을 수정하여 반영합니다.
    - 뷰와 모델 변경에 따른 컨트롤러를 수정합니다.
    
    ## **3. MVC Pattern의 한계**
    
    세상에 완벽이라는 단어는 없습니다. MVC 패턴에도 한계가 존재합니다. 복잡한 대규모 프로그램의 경우 다수의 뷰와 모델이 컨트롤러를 통해 연결되기 때문에 컨트롤러가 불필요하게 커지는 현상이 발생합니다. 복잡한 화면을 구성하는 경우에도 동일한 현상이 발생하는데 이를 **`'Massive-View-Controller'`** 라고 합니다. 이런 문제점을 보완하기 위해 다양한 패턴이 파생되었습니다.
    
    - MVP 패턴
    - MVVM 패턴
    - Flux
    - Redux
    - RxMVVM
    1. MVC 패턴의 한계는 무엇인가?
    2. 그것을 보완하기 위한 디자인 패턴에는 어떤 것이 있는가?