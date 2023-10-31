# Mobile_AD(attacker/defensor)

- 23.09.13

  ## struct와 class와 enum의 차이를 설명하시오.

  - **class / struct, enum 의 차이점**
    - class
      - 클래스는 `call by reference` 즉, `참조에 의한 호출`로 이는 함수에 변수를 전달할 때, 실제 변수의 메모리 주소가 전달되어 함수 내에서 해당 변수의 값을 수정할 수 있음을 의미한다.
        - 이는 클래스 인스턴스가 메모리에서 한 번만 생성되고, 해당 인스턴스에 대한 참조(주소)만 복사하기에 가능한 것입니다.
      - 상속과 재정의를 통한 계층적 관계가 가능한 것이 클래스와 구조체의 가장큰 차이점 이다.
      - 타입 캐스팅(Type Casting): 클래스는 실행 시 컴파일러가 클래스 인스턴스의 타입을 미리 파악하고 검사할 수 있다.
      - 소멸화 구문(deinit): 클래스는 인스턴스가 소멸되기 직전에 처리해야할 구문들을 미리 등록할 수 있다.
    - struct, enum
      - 구조체와 열거형은 `call by value` 즉, `값에 의한 호출`로 각각의 함수나 메서드는 실제 변수의 메모리 주소를 받는 것이 아닌 변수의 값 자체의 복사본을 받아 새로운 변수를 생성하여 사용한다.
      - 따라서 이는 인스턴스를 새로 생성할 때마다 새로운 메모리 공간이 할당되기에, 새로 생성된 복사본 인스턴스들은 독립적인 성격으로 원본 인스턴스에 영향을 주지 않는다.
  - **struct와 enum의 차이점**
    | | struct | enum |
    | ------------------------------------------------------- | ---------- | ---- |
    | 1. 초기화함수(Initializer) | O(class o) | X |
    | 2. 저장 프로퍼티(Stored Property) | O(class o) | X |
    | 3. 멤버와이즈 초기화(memberwise init) \_ 자동 생성 init | O(class x) | X |
  - 이해하기 쉽도록 내 방식대로 정리한 정의
    - **타입 (Type)**
      - 데이터와 기능을 용도에 맞게 묶어 구조화 해놓은 것이다.
      - 메모리에 생성될 타입의 인스턴스가 수행할 수 있는 기능과 동작을 상세하게 표현한 것.
      - 종류는 `클래스, 구조체, 열거형`이 있고, 어떠한 객체의 `설계도`라고 생각하면 이해에 용이하다.
        - 일반적으로 "객체"라는 용어는 주로 객체 지향 프로그래밍(OOP)에서 사용되며, 클래스의 인스턴스를 가리킨다. 따라서 구조체와 열거형은 “값” 혹은 “구조체(열거형)의 인스턴스” 라 칭한다.
      - ex) ‘냉장고’ 라고 정의된 것, 우리집에 실제로 있는 냉장고
        - 위의 예시 상황에서 ‘냉장고’ 라는 것은 타입이고, 실제로 집에 있는 냉장고는 냉장고라는 설계도에 속한 인스턴스인 것이다.
    - **속성 = 프로퍼티(Property)**
      - 타입 내부에 저장된 값 또는 값을 제공하는 요소 즉, 상수와 변수로 선언된 속성들을 프로퍼티라 칭한다.
      - 종류
        - 저장, 연산, 타입
    - **메소드(Method)**
      - 타입 안에 선언된 함수로 `???.()` 와 같이 . 을 이용하여 호출하는 함수를 뜻한다.
    - **객체 == 인스턴스 둘은 같을까?!** - 정확히 얘기 하자면 타입을 적용하여 선언된 상수와 변수를 인스턴스라 칭하는데, 이 때 클래스 타입으로 적용된 상수와 변수 인스턴스를 객체라 칭한다.
      ⚠️ 참고
  - [https://jayb-log.tistory.com/216](https://jayb-log.tistory.com/216)
  - [https://babbab2.tistory.com/118](https://babbab2.tistory.com/118)

  ***

  ## class의 성능을 향상 시킬수 있는 방법들을 나열해보시오.

  1. **구조체(Struct) 또는 열거형(Enum) 사용:** 클래스보다 구조체나 열거형을 사용하는 것이 성능상 이점을 가질 수 있습니다. 구조체는 값 형식이므로 복사본이 전달되고 참조 카운트와 같은 추가 오버헤드가 없습니다. 따라서 클래스보다 가벼운 객체를 다루는데 유용합니다.
  2. **불필요한 클래스 인스턴스 생성 피하기:** 객체를 생성하고 해제하는 과정에는 오버헤드가 발생합니다. 불필요한 객체 인스턴스 생성을 피하고, 필요한 경우에만 인스턴스를 생성하도록 최적화합니다.
  3. **자료구조 최적화:** 자료구조의 선택과 사용을 최적화하여 검색, 삽입, 삭제 등의 연산을 빠르게 수행할 수 있도록 합니다. 예를 들어, 배열(Array) 대신에 성능이 뛰어난 Set 또는 Dictionary를 사용할 수 있습니다.
  4. **Lazy 프로퍼티 사용:** 프로퍼티를 처음 접근할 때까지 초기화하지 않고 지연 초기화를 사용하여 성능을 향상시킬 수 있습니다. 이는 초기화 과정에서 오버헤드가 큰 작업을 지연시키는 데 유용합니다.
  5. **스레딩과 병렬처리:** 병렬처리를 사용하여 멀티코어 프로세서에서 작업을 분산시키고 성능을 향상시킬 수 있습니다. Grand Central Dispatch(GCD) 또는 OperationQueue를 사용하여 병렬 처리를 구현할 수 있습니다.
  6. **메모리 관리:** 메모리 누수를 방지하고 불필요한 메모리 사용을 최소화하는 것이 중요합니다. ARC(Automatic Reference Counting)를 사용하여 메모리 관리를 자동화하고, 강한 순환 참조를 피하기 위해 약한 참조와 비소유 참조를 사용할 수 있습니다.
  7. **프로파일링 및 최적화:** 앱의 성능을 분석하고 병목 현상을 식별하기 위해 프로파일링 도구를 사용합니다. 이를 통해 성능을 저하시키는 부분을 찾아내고 최적화 작업을 수행할 수 있습니다.
  8. **캐싱:** 빈번하게 사용되는 데이터나 계산 결과를 캐싱하여 불필요한 연산을 피하고 성능을 향상시킬 수 있습니다.
  9. **올바른 자료구조 선택:** 특정 작업에 적합한 자료구조를 선택하는 것이 중요합니다. 성능을 고려하여 배열, 리스트, 해시 테이블, 힙 등을 선택합니다.
  10. **알고리즘 최적화:** 알고리즘을 개선하거나 더 효율적인 알고리즘을 선택하여 성능을 향상시킬 수 있습니다.

  ***

  ## Struct 가 무엇이고 어떻게 사용하는지 설명하시오.

  - struct가 무엇인가?
    - 구조체는 인스턴스의 값(프로퍼티)을 저장하는 것과 메소드를 제공하고 이를 캡슐화할 수 있도록 Swift가 제공하는 타입 중 하나이다.
      - 값을 저장하는 것 = call by value
    - 형식
      ```jsx
      struct [구조체 이름] {
          [프로퍼티와 메서드들]
      }
      ```
    - 구조체 이름은 대문자 카멜케이스를 사용하며, 프로퍼티와 메소드는 소문자 카멜케이스를 사용한다.
    - 상속이 불가능하다.
  - struct 사용을 권장 하는 경우 \_ by apple
    애플은 다음 조건 중 하나 이상에 해당한다면 구조체를 사용하는 것을 권장
    1. 연관된 간단한 값의 집합을 캡슐화하는 것만이 목적일 때
    2. 캡슐화한 값을 참조하는 것보다 복사하는 것이 합당할때
    3. 구조체에 저장된 프로퍼티가 value 타입이며 참조하는 것보다 복사하는 것이 합당할 때
    4. 다른 타입으로부터 상속받거나 자신을 상속할 필요가 없을 때
  - struct를 사용하는 일반적인 경우

    - 간단한 데이터 모델
      - 간단한 데이터를 그룹화하고 관리하기 위해 사용됩니다.
        - ex) Point(x, y)나 사각형(Rectangle) 등의 데이터 구조를 표현
    - 불변 데이터
      - 데이터의 불변성이 필요한 경우, 구조체를 사용하여 변경을 방지하고 안전성을 확보할 수 있습니다.
        - ex) 휴대폰의 위치 gps설정
          - 어떠한 앱 내부에서의 gps 동의가 아닌, 휴대폰 자체의 gps 설정은 앱과 달리 독립적이어야 하기에.

  - +@ 번외 : 구조체를 초기화 하는 여러 방법들

    ### ❗️초기화(\***\*Initializers\*\***)란?

    ***

    - **구조체 / 열거형 / 클래스의 인스턴스를 생성하는 것이 초기화로, 초기화의 역할은 모든 프로퍼티를 기본값으로 초기화 하는 것**
    - **인스턴스 내 기본값이 존재하지 않는 프로퍼티가 있을 경우, 초기화에 실패하고 인스턴스는 생성되지 않는다**
    - 따라서 만약, **초기화가 끝나는 시점에 모든 프로퍼티가 기본 값을 가지고 있지 않다면** 그것은 **초기화 실패로, 즉 인스턴스가 생성되지 않음**

    ### ❗️구조체를 초기화 하는 방법들

    ***

    1. \***\*선언과 동시에 프로퍼티에 기본값 넣어주기\*\***

       ```swift
       struct Human {
           let name: String = "Sodeul"
           let age: Int = 28
       }
       - name과 age 상수 선언과 동시에 직접 값을 넣어준다.
       ```

    2. \***\*프로퍼티의 타입을 옵셔널(Optional) 타입으로 설정하기\*\***

    ```swift
    struct Human {
        let name: String?
        let age: Int?
    }
    - 상수들을 옵셔널 타입으로 선언한다.
    - 이때에 옵셔널로 선언된 변수 혹은 상수는 자동으로 nil로 초기화 된다.
    ```

    3. \***\*init 함수에서 값을 설정해주기\*\***

    ```swift
    struct Human {
        let name: String
        let age: Int

        init(name: String) {
            self.name = name
            self.age = 28
        }
    }
    - init 함수(생성자)를 이용하여 초기화를 진행한다.
    - 이 때에 가장 중요한 점은 init(초기화) 함수가 종료되기 전까진 모든 프로퍼티가
      기본 값을 가져야 한다.
    ```

    4. **Memberwise Initializers(중요)**

    - 이는 구조체에서만 제공하는 것으로, 구조체가 자동으로 제공하는 생성자로 파라미터를 통해 모든 프로퍼티를 초기화 가능하도록 한다.

    ```swift
    struct Human {
    	let name: String
    	let age: Int
    }

    Human.init(name: , age: )

    - 위와 같이 인스턴스를 생성하기 위해 init을 할 때 무조건 name과 age를 설정할 수
      있도록 프로퍼티를 모두 초기화할 수 있는 Initializers를 자동으로 제공하는 것이다.
    ```

    ⭐️ **Memberwise Initializers의 성질**

    1.  memberwise initializers로 제공되는 생성자의 파라미터는, 프로퍼티가 선언된 순서와 갯수에 맞추어 생성된다.

        1. 이때 파라미터의 이름은 프로퍼티의 이름으로 설정된다

        ```swift
        struct Human {
            let name: String
            let age: Int
        }


        Human.init(name: "Sodeul", age: 28)
        Human.init(age: 28, name: "Sodeul")          //error!!!

        /*
        - 위와 같이 프로퍼티로 선언된 순서인 name -> age 순서를 지키지 않은 init은
        에러가 발생한다.
        */
        ```

    2.  프로퍼티가 let으로 선언 되어 있고, 이미 초기화가 되어있는 상태면 생성자 목록에서 제외된다

        ```swift
        struct Human {
            let name: String = "Sodeul"
            let age: Int
        }

        Human.init(age: 28)

        /*
        - 위 name 프로퍼티와 같이 let 상수로 선언되고, 값이 할당되어 이미 초기화
        되어있는 상태라면 init 목록에서 제외된다.
        */
        ```

    3.  \***\*프로퍼티가 var로 선언 되어 있고, 이미 초기화가 되어있는 상태면 해당 프로퍼티를 초기화 하는 생성자와, 제외된 생성자 두 개를 제공한다\*\***

        ```swift
        struct Human {
            var name: String = "Sodeul"
        }

        Human.init()                    // name이 제외된 생성자
        Human.init(name: "Sodeul")      // name을 초기화 시킬 수 있는 생성자
        ```

    4.  \***\*🚨중요🚨 생성자를 "직접" 생성한 경우, memberwise initializers는 더이상 제공되지 않는다\*\***

        ```swift
        struct Human {
            var name: String = "Sodeul"

                init(name: String) {
                        self.name = name
                }
        }

        Human.init(name: )

        /*
        - memberwise initializers는 구조체 안에 init 함수를 직접 구현하지 않은
        경우에만 제공되기에 구조체 안에 init 함수를 직접 선언해주면 이 땐 더 이상
        제공되지 않는다.
        */
        ```

        - **init 함수도 구현하고 싶고, memberwise initializers도 사용하고 싶다?!**

          ```swift
          struct Human {
              var name: String
              var alias: String
          }

          extension Human {
              init(name: String) {
                  self.name = name
                  self.alias = name
              }
          }

          Human.init(name: "Sodeul")
          Human.init(name: "Sodeul", alias: "Deulso")
          ```

    5.  \***\*🚨중요🚨 프로퍼티 중 하나라도 private일 경우, memberwise initializers는 제공되지 않는다.\*\***

             ```swift
             struct Human {
                 private var name: String = "Sodeul"
             		var alias: String
             }

             Human.init        -> 제공되지 않음.

             /*
             - 선언된 프로퍼티 중 하나라도 private 접근 제어자를 가질 경우,
             이 때도 더이상 memberwise initializers를 제공하지 않음!
             */
             ```

        ⚠️ 참고

  - [https://jayb-log.tistory.com/216](https://velog.io/@hayeon/struct%EA%B0%80-%EB%AC%B4%EC%97%87%EC%9D%B4%EA%B3%A0-%EC%96%B4%EB%96%BB%EA%B2%8C-%EC%82%AC%EC%9A%A9%ED%95%98%EB%8A%94%EC%A7%80-%EC%84%A4%EB%AA%85%ED%95%98%EC%8B%9C%EC%98%A4)
  - [https://babbab2.tistory.com/167](https://babbab2.tistory.com/167)

  ***

  ## class 메서드와 static 메서드의 차이점을 설명하시오.

  - **클래스 타입과의 관계**

    - static 메서드는 **클래스와 무관**하며, 재정의되지 않고 상속되지 않으므로 **항상 동일한 동작**을 합니다.
      - 그러나 클래스에서는 static 메서드와 class 메서드가 동일한 개념이기 때문에 static 메서드를 class 메서드로, class 메서드를 static 메서드로 오버라이드 할 수 있다. 즉, 상속이 가능하다.
    - 클래스 메서드는 **클래스에 속하며 상속 및 재정의가 가능**하고, **서브클래스에서 다르게 동작할 수** 있습니다.

  - 구조체 타입과의 관계
    - struct, enum 내에서 static 메서드는 선언이 가능하나, class 메서드는 불가능하다.

  ***

  ## `defer`란 무엇인지 설명하시오.

  - `defer`란? - `defer`의 사전적 의미로는 `미루다, 연기하다` 라는 뜻으로 정의 되어있는데, 이는 이는 보통 함수 안에서 작성되며, `defer`가 작성된 위치와 상관 없이 해당 코드블럭 종료 직전에 실행되는 구문으로 `defer`에 포함된 코드를 함수의 맨 마지막에 실행하도록 하는 클로저 입니다.
    <br></br>
  - `defer`는 왜, 언제 사용? - 함수를 종료하기 직전에 정리해야 하는 변수나 상수를 처리하는 용도.
    <br></br>
  - `defer` 블록 내에서 변수와 상수를 사용할 수 있나요?\*\*
    - 네, `defer` 블록이 현재 스코프 내에서 실행되기 때문에 `defer` 블록 내에서 현재 스코프의 변수와 상수에 접근할 수 있습니다. - 다만, `defer` 블록 내에서 변수나 상수를 수정하는 것은 불가능합니다.`defer` 블록은 주로 정리 작업을 수행하거나 값을 출력하는 데 사용됩니다.
      <br></br>
  - **설정 초기화 및 정리에서의 `defer` 활용**
    - 객체나 클래스의 초기화 과정 또는 설정 초기화 및 정리에 사용될 수 있습니다. **`init`** 메서드나 클래스 초기화 시 `defer`를 사용하여 설정 초기화 및 정리 코드를 작성할 수 있습니다.
      - 이점
        1. **가독성 향상**: **`init`** 메서드 내부에 설정 초기화 및 정리 코드를 직접 작성하는 대신 `defer`를 사용하면 초기화 과정과 관련된 코드 블록을 명확하게 분리할 수 있습니다. 이렇게 하면 초기화 코드와 정리 코드가 분리되어 가독성이 향상됩니다.
        2. **중복 코드 제거**: 초기화 및 정리 코드를 여러 곳에서 사용해야 할 때 **`defer`**를 사용하면 중복 코드를 제거할 수 있습니다. 예를 들어, 여러 **`init`** 메서드에서 동일한 정리 코드를 사용해야 할 경우, **`defer`**를 사용하여 중복을 제거할 수 있습니다.
        3. **안전성 향상**: **`defer`**를 사용하면 초기화 중 발생할 수 있는 예외 상황이나 에러에 대한 대비가 강화됩니다. 설정 초기화 및 정리 코드가 분리되어 있기 때문에 정리 코드는 항상 실행되므로 자원 누수를 방지하고 안전한 초기화를 보장합니다.
        4. **유지 보수성 향상**: 코드를 유지 관리할 때 초기화 및 정리 코드를 수정해야 할 경우, 해당 코드를 한 곳에서 수정하면 됩니다. 이는 코드 변경에 대한 유지 보수성을 향상시킵니다.
        5. **확장성**: 클래스에 새로운 초기화 로직이 추가될 때, **`defer`** 블록을 추가하기만 하면 됩니다. 이렇게 하면 초기화 및 정리 코드를 쉽게 확장할 수 있습니다.

  ***

  ## `defer`가 호출되는 순서는 어떻게 되고, `defer`가 호출되지 않는 경우를 설명하시오.

  - `defer`는 해당하는 코드블럭의 가장 마지막에 실행되며, 호출되지 않는 경우로는 `defer`에 해당하는 코드로 오기 이전 해당 코드블럭이 종료된다면 실행되지 않습니다.

    ```swift
    func testDefer() {
        print("Check #1")

        return;
        defer { print("defer #1") }

        print("Check #2")
    }

    testDefer()

    /*
    - 위와 같이 defer 코드가 실행되기 이전 return으로 함수 코드블럭을 탈출하게 된다면
    defer는 호출되지 않습니다.
    */
    ```

  - 하나의 함수에서 여러 번 `defer` 호출이 가능한가?

    ```swift
    func testDefer() {
        defer { print("defer #1") }
        defer { print("defer #2") }
        defer { print("defer #3") }
    }

    testDefer()

    /*
    - 가능하다. 이의 실행 순서는 가장 마지막에 실행된 defer부터 역순으로 진행된다.
    즉, defer #3 부터 #1순서로 출력된다.
    - 먼저 읽은 defer부터 stack에 쌓인다고 생각하자.
    따라서 가장 마지막으로 읽은 defer가 1번으로 실행, 가장 처음으로 읽은 defer가 마지막에 실행
    */
    ```

  - `defer`는 중첩으로 사용이 가능할까?

    ```swift
    func testDefer() {
        defer {
            defer {
                defer {
                    print("defer #3")
                }
                print("defer #2")
            }
            print("defer #1")
        }
    }

    /*
    - 이 역시도 가능하며, 실행 순서는 가장 바깥쪽 defer부터 실행된다.
    따라서 defer #1 부터 ~ #3 순서로 출력된다.
    */
    ```

    ⚠️ 참고

  - [https://babbab2.tistory.com/80](https://babbab2.tistory.com/80)

  ***