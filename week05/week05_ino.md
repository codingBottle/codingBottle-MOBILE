# Observer / Delegate, Notification

## Observer Pattern

> **객체 지향 프로그래밍에서 사용하는 디자인 패턴 중 하나로, 객체의 상태 변화를 다른 객체에게 효과적으로 알리는 방법을 제공하며 "Publish-Subscribe" 또는 "Subscriber-Publisher" 패턴으로도 알려져 있다.**
> 

옵저버 패턴이란 ? Publish(발행) → Subscribe(구독) 의 형태로 Publish Object의 상태가 바뀌면 이를 구독하는 객체들에게 알림이 가고 자동으로 내용이 갱신되는 방식, 1:N 의 관계를 가지고 있다.

Publisher - Subject 라고도 표현하며 상태가 변경될 수 있는 객체이다. Observer 들이 Subject를 ‘구독’ 하고, Subject의 상태가 변경될 때마다 Subscriber(Observer) 들에게 알림을 보냄.

Subscriber - Observer 패턴의 주체가 되는 존재, Observer 라고도 하며 Subject의 상태 변화를 Observing 한다. Subject의 변화 이벤트에 따라 특정 동작 수행함.

Youtube에서 유저가 새 동영상을 올릴 경우 그 사용자를 팔로우하는 사람들에게 알림이 가는 것이 그 예시임.

이 예시를 코드로 살펴보자.

Subject

```swift
protocol Following {
		var observers: [Observer] { get set }
    func subscribe(observer: Observer)
    func unSubscribe(observer: Observer)
    func notify(message: String) 
}

class User: Subject {
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

Observer

```swift
// Observer Protocol
protocol Observer {
	var id: String { get set }
    func update(message: String)
}

// 팔로워
class follower: Observer {
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

VC

```swift
func test() {
	let userA = User(observer: [])
    
    let first = follower(id: "first")
    let second = follower(id: "second")
    let third = follower(id: "third")
    
    // 팔로워 → userA 구독
    userA.subscribe(observer: first)
    userA.subscribe(observer: second)
    userA.subscribe(observer: third)
    
    
    // userA 의 알림
    userA.notify(message: "첫번째 알림!!")
    
    // third follower가 구독 해제
    userA.unSubscribe(observer: third)
    
    // userA 의 두번째 알림
    userA.notify(message: "두번째 알림!!")
    
}

test()
```

Swift에서 사용하는 NotificationCenter나 KVO(KeyValue Observing이 옵저버 패턴의 예시라고 한다.

Observer 패턴의 장단점

<aside>
💡 장점

</aside>

1. 데이터를 갱신하지 않아도 새로운 데이터를 알아서 받아옴
2. Observer 간 서로를 알지 못해 느슨한 결합이 됨
3. 구독/취소가 가능하여 유연한 객체지향 시스템 구축을 통해 상호의존성 최소화 가능

<aside>
💡 단점

</aside>

1. Observer에게 알림 순서를 보장할 수 없음
2. Observer의 수가 많아질 경우 관리가 힘들어 질 수 있음
3. 큐 형태로 데이터 배분 처리가 돼 데이터 분배에 문제가 생기면 커질 수 있다.
4. Thread-Unsafe
5. Oberserver 제거를 제때 하지 않을 경우 메모리 누수가 일어남

## Delegate ,Notification 의 차이

### Synchronous:

Delegate: 동기적으로 호출된다. 메소드 호출이 완료될 때까지 다른 작업을 기다린다. 응답 시간이 빠르며 순서대로 코드를 실행 가능하다.

Notification: 비동기적으로 작동한다. 따라서 여러 Observer이 동시에 알림을 받아 처리 가능하나, 작업 순서의 보장이 어렵다.

### 메모리 관리:

Delegate: 

delegate 속성의 객체가 delegate로 설정된 객체를 강하게 참조 하고, 반대로 delegate로 설정된 객체가 delegate 속성을 갖는 객체를 강하게 참조할 경우 순환 참조가 발생하여 메모리 누수가 발생하므로 약한 참조로 선언해야 함.

Notification: 

notificationCenter의 observer들은 notificationCenter에 의해 직접 관리되지 x 따라서 필요하지 않을 때 명시적으로 등록 해제를 해야 한다. 그렇지 않을 경우 메모리 누수가 발생한다.

### 통신 대상

Delegate: 

1:1 통신으로 하나의 객체가 한 개의 delegate만을 갖는다.

Notification: 

1:N 통신으로 하나의 subject가 여러 개의 observer에 메시지를 보낼 수 있다.

### 사용처

Delegate:

한 개체에서 다른 개체로 직접적 컨트롤이 필요한 경우, 유사한 상호작용 패턴이 있는 경우 사용한다.

사용자 인터페이스 이벤트 처리에 사용되며, 컴파일 타임에 타입 체크가 가능하다. 

Notification: 

여러 개체가 동일 정보를 동시에 받아야 하거나, 시스템 수준의 전역 이벤트를 처리해야 할 경우 사용한다.

시스템 이벤트 감지 및 여러 컴포넌트 사이의 복잡한 데이터 흐름 관리에 사용, 런타임 시 동적으로 구독 및 발송되므로 컴파일 타임에 타입 체크가 어렵다.

### 디버깅

Delegate:

코드 실행의 흐름을 따라가기 비교적 쉬워 delegate 메소드가 호출되는 시점, 위치를 명확히 파악 가능해 디버깅에 용이하다.

Notification: 

여러곳에서 동시에 처리가능해 디버깅이 어렵다.