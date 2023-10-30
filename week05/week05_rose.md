# 5주차

## 옵저버 패턴에 대해 설명하시오.

## 정의

- 원하는 작업이 변경되는 것을 관찰하고 있다가 관찰하는 작업이 변경될 때 원하는 작업을 수행하게 하는 형태
    - 값이 변경됨에 따라 피드백에 바로 나와야하는 프로그램에 사용하면 굿

## 구성 요소와 역할

- **Subject**
    - 옵저버를 가지고 있으며 개수 제한 X
    - 옵저버 추가 및 제거하는 인터페이스 제공
    - 상태 변화가 일어날 때 모든 옵저버에게 알림 보냄
- **Concrete Subject**
    - 실제로 관찰되는 객체
    - 상태 변경되면 옵저버에게 알림
- **Observer**
    - Subject의 상태 변화를 감지하기 위한 인터페이스 정의
    - update라는 메서드를 가짐
        - Subject의 상태가 바뀔 때마다 호출됨
- **Concrete Observer**
    - 옵저버 인터페이스를 구현하는 실제 클래스
    - Subject의 상태와 일관성 유지
        - 객체의 상태와 일관성을 유지하기 위해 update 인터페이스 구현

## 왜 사용?

- 두 객체가 서로 느슨하게 연결
    - 두 연결된 객체들 사이의 결합도가 낮음
    - 서로가 서로에 대한 최소한의 정보만을 가지고 소통한다는 것
- 비순차적인 작업
    - 비동기적이거나 비순차적인 작업을 처리해야하는 경우
- 변경 전파
    - 한 개체의 상태 변화를 다른 개체들에게 즉시 알려야하는 경우

### 느슨한 결합

- 두 객체가 상호작용을 하지만, 서로에 대해 잘 모른다는 점
- 인터페이스 이용
- 상속이 아닌 구성 이용

## 언제 사용?

- 한 객체의 변경이 다른 하나 이상의 객체에게 영향을 미칠 때
- 어떤 객체가 다른 객체를 직접 참조하지 않고도 그것들과 상호작용 해야 할 때

## 어떻게 사용?

- NotificationCenter
    - 옵저버들이 해당 메시지를 받아 동작하도록 만드는 방식
- KVO(key Value Observing)
    - 특정 객체의 프로퍼티 값 변화를 관찰해서 반응하는 방식
- Combine
    - 이벤트 처리와 데이터 처리를 위한 선언형 Swift API로서 Observable 스트림과 Subscriber 사이에서 데이터 변화를 전달함

## 장점

- Open / Close 원칙
    - Subject의 코드를 수정하지 않고 새로운 Observer 클래스 추가 가능 / 반대도 가능
- 유연한 객체지향 시스템 구축
    - 상호의존성 최소화 (느슨한 결합이기 때문)

## 단점

- Observer에게 알림이 가는 순서 보장 X
- Observer, Subject의 관계가 잘 정의되지 않으면 원하지 않는 동작이 발생할 수도 있음
- 메모리 누수
    - 옵저버가 제대로 해제되지 않으면 메모리 누수 발생 가능

## 예제

- Subject : 온도 추적, Observer : 온도 변화 출력

```swift
// Observer 프로토콜 정의
protocol Observer {
    func update(_ temp: Int)
}

// Subject 클래스 정의
class Subject {
    private var observers = [Observer]()
    private var temperature: Int = 0 {
        didSet {
            notifyObservers()
        }
    }

    // 옵저버 추가
    func add(observer: Observer) {
        observers.append(observer)
    }

    // 모든 옵저버에게 알림
    func notifyObservers() {
        observers.forEach { $0.update(temperature) }
    }

    // 온도 변경 메서드
    func changeTemperature(to temp: Int) {
        temperature = temp
    }
}

// ConcreteObserver 클래스 정의
class ConcreteObserver: Observer {

   let name: String

   init(name: String) {
       self.name = name
   }

   func update(_ temp: Int) {
       print("\(name) observes temperature change to \(temp)")
   }
}

let subject = Subject()

let observer1 = ConcreteObserver(name: "Observer1")
let observer2 = ConcreteObserver(name: "Observer2")

subject.add(observer: observer1)
subject.add(observer: observer2)

subject.changeTemperature(to: 35)
```

- Subject의 changeTemperature 메서드가 호출될 때마다 등록된 모든 ConcreteObserver 객체들이 업데이트됨
- 각각 자신의 이름과 함께 새로운 온도 출력

# Delegation와 Notification 방식의 차이점에 대해 설명하시오.

## Delegation

**동작 방식**

- 일대일 통신 방식
    - 한 객체가 다른 객체를 대신하여 특정 작업을 수행하거나 결정을 내리는 역할
- Delegate가 될 객체가 반드시 준수해야 하는 메서드들을 정의한 프로토콜이 있음
    - 호출하는 쪽과 호출받는 쪽 사이에 명확한 프로토콜 정의

**역할 분담**

- Delegatee : 자신의 상태 변화를 알릴 필요가 있음
- Delagate : 그 변화에 대응하는 행동 결정

**사용 상황**

- 보통 사용자 인터페이스 이벤트에 대한 처리 등에 많이 사용됨
- 한 객체가 다른 객체의 동작을 제어하거나 수정할 필요가 있을 때 사용됨
    - UITableView는 셀 선택, 스크롤 등의 이벤트를 처리하기 위해 UITableViewDelegate 사용

**주의**

- 너무 많은 기능들을 분산시켜 코드 추적 및 관리가 어려움
- 과도하게 사용될 경우 설계 복잡성 증대

## Notification

**동작 방식**

- 일대다 통신 방식
- NotificationCenter를 통해 한 객체가 이벤트 발생을 알리면, 그 이벤트를 관찰하고 있는 모든 객체들이 알림을 받아 각자 처리
- 옵저버가 NotificationCenter에 등록되고, NotificationCenter는 해당 이벤트 발생 시 모든 옵저버에게 알림

**사용 상황**

- 시스템 상태 변화나 데이터 변경 등 여러 곳에서 관심을 가질 수 있는 사건에 대해 사용됨
    - 다양한 컴포넌트가 동일한 이벤트에 반응해야 하는 경우
    - 데이터 모델의 변화를 UI에 반영하는 경우
    - 시스템 또는 외부 이벤트 응답

**자동 전파**

- NotificationCenter와 Observer 사이에는 명확한 프로토콜 X
- NotificationCenter : 일반적인 이벤트 형식으로 알림 보냄
- Observer : 자신들이 필요로 하는 정보만 추출하여 사용

**주의**

- 네임스페이스 충돌
    - 유니크한 이름으로 Notification 이름 설정

### 공통점

- 객체 간 통신
    - 객체 간의 상호작용을 가능하게 하는 디자인 패턴
- 비동기 처리
    - 비동기적으로 동작하여 호출자가 결과를 기다리지 않아도 되는 구조
- 느슨한 결합
    - 서로 다른 객체나 컴포넌트 사이에 직접적인 의존성을 최소화하여 변경에 유연하고 확장성 있는 설계를 가능하게 함

### 차이점

- 관계 형태
    - Delegation : 일대일
    - Notification : 일대다
- 사용 케이스
    - Delegation : 한 객체가 다른 객체의 행동을 대신 수행하거나 수정할 때 사용됨 / 사용자 인터렉션 등
    - Notification : 한 객체에서 발생한 이벤트를 여러 개의 객체가 알아야 할 경우 사용됨 / 시스템 상태 변화나 데이터 변경 등
- 변경
    - Delegation : 프로토콜 정의하여 메서드들을 명시적으로 호출
    - Notification : 옵저버들에게 브로드캐스트 방식으로 알림