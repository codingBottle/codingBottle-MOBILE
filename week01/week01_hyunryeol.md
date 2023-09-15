# CodingBottle - Mobile
## struct, class, enum의 차이?
- 공통점
  - struct, class, enum은 모두 값을 저장하거나 그룹화 하는데 사용합니다.
- 차이점
  - 
    ||Struct|Class|Enum|
    |------|---|---|---|
    |상속가능|<center>x</center>|<center>o</center>|<center>x</center>|
    |<center>타입</center>|<center>값 복사</center>|<center>주소 복사</center>|<center>값 복사</center>|

        구조체는 데이터를 담는 용도로 사용되며 값 복사에 의해 전달됩니다.
        클래스는 복잡한 객체와 상속을 다루며 참조에 의해 전달됩니다.
        열거형은 관련된 값들을 그룹화하고 패턴 매칭을 통해 다양한 상황을 처리하는 데 사용됩니다.
    - 차이점 예시 코드
        ~~~swift
        // Struct 예시
        struct Point {
            var x: Int
            var y: Int
        }

        var point1 = Point(x: 10, y: 20)
        var point2 = point1 // 값 복사
        point2.x = 30

        print(point1) // 출력: Point(x: 10, y: 20)
        print(point2) // 출력: Point(x: 30, y: 20)


        // Class 예시
        class Person {
            var name: String
            init(name: String) {
                self.name = name
            }
        }

        var person1 = Person(name: "Alice")
        var person2 = person1 // 참조 복사
        person2.name = "Bob"

        print(person1.name) // 출력: Bob
        print(person2.name) // 출력: Bob


        // Enum 예시
        enum Day {
            case sunday
            case monday
            case tuesday
        }

        var today = Day.monday
        var tomorrow = today // 값 복사

        tomorrow = .tuesday

        print(today)     // 출력: monday
        print(tomorrow)  // 출력: tuesday

        ~~~
<hr>

## class의 성능을 향상 시킬수 있는 방법들을 나열해보시오.
    Dispatch에 대한 이해
    - Static Dispatch
        컴파일 시점에 호출 할 함수를 결정합니다.
        컴파일 시점이란 앱을 동작시키기 전에 개발자가 작성한 코드들이 컴퓨터가 읽을 수 있게 만들어서 앱 실행을 준비하는 과정입니다.
        앱 실행 전, 어떤 상황에서 이 함수가 호출될 것이라는 결정이 나기 때문에 성능이 좋습니다.
    - Dynamic Dispatch
        런타임 시점에 호출 할 함수를 결정합니다.
        컴파일이 끝난 후 앱을 실행하는 동안 일어나는 과정(런타임) 속에서 호출할 함수를 결정하게 되는데,
        하위 클래스가 상위 클래스의 메서드를 호출할 때 해당 메모리 배열을 참조하여 호출하는 함수를 결정하는 과정을 런타임에 결정하고 성능은 떨어지게 됩니다.

    Dynamic => Static으로 변경하는 것 만으로 유의미한 성능 향상
    Class대신 Struct를 사용하는게 가장 좋은 방법이라고 생각
  ### Class 자체 성능 향상 방법
   - 상속이나 오버라이딩이 필요없는 클래스나 메서드, 프로퍼티에 final 선언
     - final로 선언시 하위 클래스에서 오버라이딩 할 수 없고 상속도 불가능해 Static Dispatch 방식으로 동작
   - 파일 내에서만 접근시 private 선언
     - private 선언 시 오버라이드 하는 곳이 없다면 final로 추론, class에 private를 붙이면 내부 프로퍼티, 메소드 에 private가 붙은 것으로 동작
<hr>

## class 메서드와 static 메서드의 차이점을 설명하시오
    static func, class func 모두 타입 메소드이기 때문에 인스턴스를 생성하지 않고 타입에 접근해 함수를 호출할 수 있습니다.
    class func는 오버라이딩을 허용하지만 static func는 오버라이딩을 허용하지 않습니다.
<hr>

## defer란 무엇인지 설명하시오.
    defer란, 현재 코드 블록을 나가기 전에 꼭 실행해야 되는 코드를 작성하여
    코드가 블록을 어떻게 빠져 나가든 꼭 마무리해야 되는 작업을 할 수 있게 도와줍니다.
<hr>

## defer가 호출되는 순서는 어떻게 되고, defer가 호출되지 않는 경우를 설명하시오.
    코드블록 순서로 호출되며, 같은 코드블럭일 경우 역순으로 호출됩니다. defer를 읽지 않고 return을 하는 경우 defer가 호출되지 않습니다.