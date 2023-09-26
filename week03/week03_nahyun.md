# 질문 리스트
> **_나의 질문_**
- Closure에 대하여 설명하시오.
- Closure와 함수와의 관계에 대해 설명하시오.

> **_seongeun님의 질문_**
- Extension에 대해 설명하시오.
- Extension 내부에서 함수를 override할 수 있는지 설명하시오.

## Closure에 대하여 설명하시오. 

클로저(Closure)는 코드 블록으로 전달인자, 변수, 상수 등으로 저장, 전달이 가능.
함수도 클로저에 포함. 이름이 있는 클로저는 함수, 없는 함수는 클로저라고 함. (익명 함수)
다른 곳으로 코드 블록을 전달하고 싶을 때 사용. 참조 타입. 

int a = 10; 이라는 코드가 있다고 가정.
메모리의 특정 위치(주소)에 a가 있겠고, 그 주소에 10을 집어 넣음.
변수와 마찬가지로 함수에도 주소가 있음.
let add = {(x:int, y:int) -> Int in return(x+y)};
클로저는 참조 타입이므로 저 함수가 저장돼있는 주소가 add에 저장. 

```
// 기본 클로저 
{ (매개변수 목록) -> 반환타입 in
    실행 코드
}
```
```
// 클로저 예시
// sum이라는 상수에 클로저를 할당
let sum: (Int, Int) -> Int = { (a: Int, b: Int) in
    return a + b
}

let sumResult: Int = sum(1, 2)
print(sumResult) // 3
```
> - 독립성: 클로저는 독립적으로 존재할 수 있으며, 이름이 없거나 이름이 있어도 필요한 곳에서 직접 호출할 수 있음.
- 캡처(Capture): 클로저는 자신이 정의된 범위 내의 변수와 상수를 캡처할 수 있습니다. 이는 클로저가 외부 범위에서 정의된 변수나 상수의 값을 저장하고 나중에 사용할 수 있게 해줌.
- 일급 객체(First-Class Object): 클로저는 Swift에서 일급 객체로 취급됩니다. 이는 클로저를 변수에 할당하거나 함수의 매개변수로 전달하거나 함수의 반환 값으로 사용할 수 있다는 의미.
- 1급 객체 함수: 함수는 어디는 갈 수 있는 권리가 있음. 다른 함수의 매개변수로 넘어갈 수 있음.함수가 함수를 리턴 가능, 상수, 변수에 할당 가능. 
>

#### Trailing Closure : **함수의 마지막 파라미터가 클로저**일 때, 이를 파라미터 값 형식이 아닌 함수 뒤에 붙여 작성하는 문법이며, **Argument Label은 생략** , 파라미터가 클로저 하나일 경우엔 호출 구문도 생략 가능

```
//클로저 하나만 파라미터로 받는 함수
func doSomething(closure: () -> ()) {
    closure()
}

//호출 Inline Closure
doSomething(closure: { () -> () in
    print("Hello!")
})

//Trailing Closure
doSomething () { () -> () in
    print("Hello!")
}

//호출 구문 생략
doSomething { () -> () in
    print("Hello!")
}
```
```
//파라미터가 여러 개인 함수
func fetchData(success: () -> (), fail: () -> ()) {
    //do something...
}

//Inline Closure
fetchData(success: { () -> () in
    print("Success!")
}, fail: { () -> () in
    print("Fail!")
})

//Trailng Closure
fetchData(success:  { () -> () in
    print("Success!")
}) { () -> () in
    print("Fail!")
} //마지막 파라미터의 클로저는 함수 뒤에 붙여 쓸 수 있음.
 
```
#### 클로저를 좀 더 보기 편하게 문법을 단순하게 변형하는 경량 문법 
파라미터 형식과 리턴 형식을 생략
```
func doSomething(closure: (Int, Int, Int) -> Int) {
    closure(1, 2, 3)
}
//Incline Closure
doSomething(closure: { (a: Int, b: Int, c: Int) -> Int in
    return a + b + c
})
//파라미터 형식, 리턴 형식 생략
doSomething(closure: { (a, b, c) in
    return a + b + c
})
```
Parameter Name은 Shortand Argument Names으로 대체하고, 이 경우 Parameter Name과 in 키워드를 삭제
Shortand Argument Names: $와 index를 이용해 Parameter에 순서대로 접근
```
doSomething(closure: { (a, b, c) in
    return a + b + c
})
//Shortand Argument Names 사용
doSomething(closure: {  
    return $0 + $1 + $2
})
```


## Closure와 함수와의 관계에 대해 설명하시오.
클로저는 unnamed closure로 이름이 없는 함수, 함수는 Named Closure. 
함수도 클로저에 포함. 이름이 있는 클로저는 함수, 없는 함수는 클로저라고 함. (익명 함수)

- 클로저와 함수 모두 코드 블록으로 동작하며, 코드를 재사용하고 구조화하기 위해 사용.
```
// Parameter와 Return Type이 둘 다 없는 클로저
//상수에 클로저를 대입한 경우
let closure = { () -> () in
    print("Closure")
}


```
Return type이 있으나 없으나 생략 가능.
함수에서는 안되는 Parameter 도 생략 가능.
클로저는 함수의 일부분 또는 독립적인 코드 블록으로 사용되므로 코드의 가독성과 유지 보수성을 높이는 데 도움이 됨.
함수는 일반적으로 의미 있는 작업을 수행하는 명명된 코드 블록으로 사용되며, 코드를 구조화하고 재사용 가능하게 만들어줌.

## Extension에 대해 설명하시오.
기존 타입을 확장하며, 존재하는 클래스, 구조체, 열거형, 프로토콜 타입에 새롭게 기능적인 부분을 추가 가능. 요구사항을 구현하는 데도 사용할 수 있음.
특정 타입의 기능 및 준수하는 프로토콜별 구현부를 분리해서 보다 코드 가독성을 높일 수 있음.

> - 계산 프로퍼티, 계산타입 프로퍼티 추가 기능
- 인스턴스 메서드, 타입 메서드의 정의
- 새로운 생성자의 제공
- subscripts 접근 방식 정의
- 중첩타입의 정의 및 사용
- 특정 프로토콜을 준수하는 현존 타입 정의

```
// Int 타입에 제곱을 수행하는 메서드를 추가하는 Extension
extension Int {
    func squared() -> Int {
        return self * self
    }
}

// Int 타입의 숫자에 제곱 메서드를 사용
let number = 5
let squaredNumber = number.squared()
print(squaredNumber) // 출력: 25

```

## Extension 내부에서 함수를 override할 수 있는지 설명하시오.

기존 타입을 확장하여 새로운 멤버(메서드, 계산된 속성 등)를 추가하는 용도로 설계되었고, 이러한 목적으로 사용되기 때문에 Extension 내에서 함수를 override하는 것은 지원하지 않음.
>- 일관성 유지: Swift의 Extension은 기존 타입의 동작을 변경하지 않고, 기존 타입과의 일관성을 유지하기 위한 목적으로 설계되었습니다. Extension을 사용하여 타입을 확장하면 해당 타입의 기존 인스턴스들은 변경 없이 그대로 사용할 수 있어야 합니다.

- 복잡성 제어: Extension 내에서 함수를 override하게 된다면, 해당 Extension이 적용된 코드의 동작이 복잡하고 예측하기 어려워질 수 있습니다. Extension은 보통 간단하고 명확한 기능을 추가하기 위한 용도로 사용됩니다.

- 타입 구조와 오류 방지: Extension 내에서 함수를 override할 수 있다면 코드 구조가 복잡해지고 오류가 발생하기 쉬워질 수 있습니다. Swift는 타입 시스템을 엄격하게 관리하여 타입 안정성을 보장하고 예상치 못한 동작을 방지합니다.

대신에 클래스를 상속(subclassing)하고 서브클래스 내에서 함수를 override하여 원래 타입의 동작을 변경하는 것이 권장되는 방법. 

### 질문
extension을 사용하면 타입의 기능을 확장할 수 있지만, API의 오버로드(동일한 이름을 가진 다양한 시그니처의 메서드 또는 함수 정의)를 작성하기가 어려워질 수 있음. (extension을 통해 동일한 이름의 메서드를 다시 정의할 수 없기 때문) 이 때 해결방법은 무엇이 있는가?