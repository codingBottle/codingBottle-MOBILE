# 23.10.25 \_ 문인호, 박현렬 Part

# 문인호 질문

## Q : **옵저버 패턴에 대해 설명하시오.**

---

## A :

### 정의 :

- 이는 소프트웨어 디자인 패턴 중 하나로, 객체 간의 일대다 의존성을 정의하는 패턴이다.
- 한 객체의 상태 변화가 다른 여러 개체에게 자동으로 알려지고, 업데이트 되도록 하는 메세지 전달 메커니즘을 제공한다.
- 구성은 주체와 옵저버로 두 가지 역할로 구성된다. (주체 = Subject / 옵저버 = Observer)
- 주체는 상태가 변할 때마다 등록된 옵저버들에게 알림을 보내는 역할을 합니다.
  - 예시로 : 유튜브 구독자 알림이 있다.
- 옵저버는 주체에서 발생한 변경 사항에 대해 관심이 있는 객체로, 주체로부터 전달받은 알림을 받아 처리하는 역할을 한다.

---

### 장점 :

- **느슨한 결합(Coupling)**: 주체와 옵저버 간의 의존성이 낮아져 유연하고 확장 가능한 시스템 설계가 가ㅂ능하다. 즉, 변경 사항이 생겨도 무난히 처리할 수 있기에 유연한 객체지향 시스템을 구축할 수 있는 것이다. 이는 상호의존성을 최소화 하였기에 가능하다.
  - 느슨한 결합(Loose Coupling) 이란?
    - 두 객체가 상호작용을 하지만, 서로에 대해 잘 모르는 것
    - 인터페이스를 이용하여 객체간 느슨한 결합이 가능하다.
    - 상속을 통한 구현이 아닌 구성(Composition)을 이용해야 한다.
  - 예시
    - [참고](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FcavZRm%2Fbtrp0H233Ko%2FnZRUTTS335c5kuO7QSzkl1%2Fimg.png)
      - Dog, Duck, Cat, Mouse Object 와 같이 전혀 관련이 없는 객체들은 Observer 라는 인터페이스로 묶음으로써 주제(Object)는 Observer들의 정보를 구체적으로 알 필요없이 정보 전달을 할 수 있다.
      - Dog, Duck, Cat, Mouse Object 와 같은 Observer 객체들은 주제(Object)의 정보를 알 필요 없이 구독을 할 수 있다.
        - 위는 객체지향의 다형성을 생각하면 이해에 용이하다.
      - 옵저버 패턴에서는 주제(Subject || Publisher)와 옵저버(Observer)가 느슨하게 결합되어 있는 객체 디자인을 제공.
- **Open / Close 원칙 (개방 폐쇄 원칙)**을 지킬 수 있다.
  - 확장에는 열려있고, 변경에는 닫혀있어야 함 = 개방 폐쇄 원칙
- **이벤트 기반 아키텍처:** 비동기적인 이벤트 처리를 지원하여 병렬적인 작업 흐름 구현이 용이합니다.

---

### 단점 :

- 순서 보장 문제: 여러 개의 옵저버가 동일한 시간에 알림을 받으면 순서가 보장되지 않을 수 있다.
- 성능 저하: 많은 수의 옵저버 등록 시 성능 저하 문제가 발생할 수 있다.

---

### 옵저버 패턴의 단점 해결 방안:

- Reactive Extensions(Rx):
  - RxSwift나 Combine과 같은 리액티브 프로그래밍 라이브러리를 사용하여 데이터 스트림과 관련된 문제를 해결한다.
    - 리액티브 프로그래밍은 데이터 스트림에 대해 선언적으로 작업하고 변환하기 위한 방법론과 도구이다.

---

### 다른 패턴들과 옵저버패턴의 차이점 :

- **참조하는 객체 간 일대다 관계에서만 동작한다는 것.**
- **중재자(Mediator) 패턴과 유사하지만 중재자는 객체 사이에서 통신 및 조정하는 반면, 옵저버 패턴은 한 객체의 상태 변화에 따라 다른 객체들에게 알리기 위해 사용된다.**

---

### 예시 및 참고 :

- 예시

  - Subject

    ```swift
    // 주체 프로토콜
    protocol Subject {
    	var observers: [Observer] { get set }
        func subscribe(observer: Observer)
        func unSubscribe(observer: Observer)
        func notify(message: String)
    }

    // 애플 스토어
    class AppleStore: Subject {
    	var observers: [Observer]

        // 초기화
        init(observers: [Observer]) {
        	self.observers = observers
        }

        // 구독
        func subsribe(observer: Observer) {
        	self.observers.append(observer)
        }

        // 구독 해제
        func unSubscribe(observer: Observer) {
        	if let idx = self.observers.firstIndex(where: { $0.id == observer.id}) {
            	self.observers.remove(at: idx)
            }
        }

        // 알림
        func notify(message: String) {
        	for observer in observers {
            	observer.update(message: message)
            }
        }
    }
    ```

  - Observer

    ```swift
    // 옵저버(Observer) 프로토콜
    protocol Observer {
    	var id: String { get set }
        func update(message: String)
    }

    // 고객
    class Customer: Observer {
    	var id: String

        // 초기화
        init(id: String) {
        	self.id = id
        }

        // 알림
        func update(message: String) {
        	print("\(id) → \(message) 수신")
        }
    }
    ```

  - ViewController

    ```swift
    func test() {
    	let appleStore = AppleStore(observer: [])

        let first = Customer(id: "first")
        let second = Customer(id: "second")
        let third = Customer(id: "third")

        // 고객 → 애플 스토어 구독
        appleStore.subscribe(observer: first)
        appleStore.subscribe(observer: second)
        appleStore.subscribe(observer: third)


        // 애플 스토어 알림
        appleStore.notify(message: "첫번째 알림!!")

        // third 고객이 구독 해제
        appleStore.unSubscribe(observer: third)

        // 애플 스토어 두번째 알림
        appleStore.notify(message: "두번째 알림!!")

    }

    test()
    ```

- 자세한 것은 아래 블로그에 사진과 함께 예시가 설명 되어 있으니 참고.

[[iOS]옵저버 패턴(Observer Pattern)](https://jiseok-zip.tistory.com/entry/iOS옵저버-패턴Observer-Pattern)

# 박현렬 질문

## Q : Delegation와 Notification 방식의 차이점에 대해 설명하시오.

---

## A :

## 1. Delegation

### 정의 :

- Delegation(델리게이션)은 특정 객체가 자신의 일부 기능을 다른 객체에 위임하는 디자인 패턴으로 코드의 복잡성을 줄이고, 재사용성과 유연성을 높일 수 있다.

---

### 로직 **:**

1. **델리게이트 프로토콜 정의 :**
   - 먼저, 델리게이트가 구현해야 할 메서드를 정의하는 프로토콜을 만들어 일반적으로 위임하려는 클래스나 구조체에 정의된다.
     ```swift
     protocol SomeDelegate {
         func didSomethingHappen(with data: Data)
     }
     ```
2. **델리게이트 프로토콜 속성 추가 :**
   - 위임하려는 객체 내부에 델리게이트 속성을 추가. 위에서 정의한 프로토콜 타입을 가지며, 일반적으로 `weak` 키워드를 사용하여 순환 참조 문제를 방지한다.
     ```swift
     class SomeClass {
         weak var delegate: SomeDelegate?
     }
     ```
3. **델리게이트 메서드 호출 :**

   - 필요한 시점에 델리게이트 메서드를 호출하여 작업을 위임한다.

     ```swift
     class SomeClass {
         weak var delegate: SomeDelegate?

         func doSomething() {
             let data = // some data...
             delegate?.didSomethingHappen(with: data)
         }
     }
     ```

4. **델리게이트 설정 및 구현 :**

   - 다른 객체에서 이 클래스를 사용하고 싶다면, 해당 객체에서 `SomeDelegate` 프로토콜을 채택(adopt)하고 필요한 메서드를 구현해야 한다. 이후, 해당 객체를 `SomeClass` 인스턴스의 `delegate` 속성에 할당.

     ```swift
     class AnotherClass: SomeDelegate {
         let someInstance = SomeClass()

         init() {
             someInstance.delegate = self
         }

         func didSomethingHappen(with data: Data) {
             // Handle the event here...
         }
     }
     ```

---

### **장점 :**

- **명확한 인터페이스 :** 델리게이트는 일관된 방식으로 동작하는 명확한 인터페이스를 제공한다.
- **직관적인 코드 :** 델리게이션 패턴은 코드가 직관적이고 이해하기 쉽도록 만든다.
- **결합도 감소 :** 객체 간의 결합도를 줄여 유지 보수와 확장성을 향상시킨다.

---

### **단점 :**

- **메모리 문제 :** 델리게이터와 델리게이트 사이에 강한 연결(strong reference)을 가지므로 메모리 관련 문제가 발생할 수 있다.

---

### **Delegation 단점 해결 방안 :**

- 메모리 문제 해결: 순환 참조(Circular Reference)나 메모리 누수(Retain Cycle)를 방지하기 위해 Weak Reference나 Unowned Reference를 사용하여 참조 카운팅 문제를 완화하거나 해결할 수 있다.

## 2. Notification

### 정의 :

- Notification은 일대다(one-to-many) 통신 패턴으로 NotificationCenter를 사용하여 여러 객체에게 동시에 알릴 수 있다.
- 이벤트 발생 시 관련된 모든 옵저버들에게 알림을 보내므로 복잡성 및 결합도가 증가할 수 있다.

---

### 로직 :

1. **NotificationCenter에 Observer 등록 :**
   - 각 객체는 NotificationCenter에 자신을 Observer로 등록하고, 특정 이벤트(Notification.Name)를 구독한다.
     ```swift
     NotificationCenter.default.addObserver(self,
     selector: #selector(handleEvent), name: .someNotificationName, object: nil)
     ```
2. **Notification Post :**
   - 특정 이벤트가 발생했을 때 해당 이벤트를 NotificationCenter에 post.
   - 이후부터 NotificationCenter는 해당 이벤트를 구독하는 모든 Observer들에게 알림을 전달합니다.
     ```swift
     NotificationCenter.default.post(name: .someNotificationName, object: nil)
     ```
3. **Notification 처리 :**
   - 각 Observer는 알림이 전달되면 해당 메서드(위 예제에서는 `handleEvent`)를 실행하여 필요한 작업을 수행한다.

---

### 장점 :

- **다양한 객체와 손쉽게 데이터 공유 가능**
- **느슨한 결합으로 인해 유지보수와 확장성이 좋음**

---

### 단점 :

- **순차적인 로직 처리가 어려움**
- **낮은 가독성과 디버깅 어려움**
- **메모리 누수 문제 (Observer 해제 안하면)**

---

### Notification 단점 해결 방안 :

- **KVO(Key-Value Observing)나 Bindings 같은 다른 패턴들을 사용하거나 Reactive Programming(RxSwift, Combine 등)을 사용하여 개선할 수 있다.**
  - **하지만 각각의 대안도 그 자체의 장단점이 있으므로 상황과 요구사항에 맞춰 적용해야 한다.**
- **메모리 누수 문제는 deinit에서 removeObserver를 호출하여 observer를 해제함으로써 해결할 수 있다.**

## Delegation과 Notification의 차이점

- **통신 패턴:**
  - **Delegation은 일대일(one-to-one) 통신을 사용합니다. 즉, 하나의 객체가 특정 작업을 다른 하나의 객체에게 위임한다.**
  - **반면에 Notification은 일대다(one-to-many) 통신을 사용한다. 한 객체가 이벤트를 발생시키면, 그 이벤트를 구독하고 있는 모든 객체들에게 알림이 전달된다.**
- **연결 강도:**
  - **Delegation은 보통 강한 연결(Strong Reference)를 가지기에 메모리 관리에 주의해야 한다.**
  - **반면에 Notification은 느슨한 연결(Loose Coupling)을 가진다.**
- **제어권:**
  - **Delegation에서는 제어권이 명확하게 분리되어 있다. 위임하는 측과 받는 측 사이에서 역할 분담이 명확함.**
  - **반면에 Notification에서는 여러 객체가 동일한 이벤트를 수신하므로 제어권이 분산된다.**
- **메모리 관리:**
  - **Delegation에서는 순환 참조 문제로 인한 메모리 누수가 발생할 수 있으며, 이를 해결하기 위해 weak reference 등을 사용해야 한다.**
  - **Notification에서는 Observer 해제를 잊으면 메모리 누수가 발생할 수 있다.**
