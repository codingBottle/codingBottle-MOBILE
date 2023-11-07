# 옵저버 패턴에 대해 설명하시오

> 관찰 중인 객체에서 발생하는 이벤트를 여러 다른 객체에 알리는 메커니즘을 정의할 수 있는 디자인 패턴입니다

 Swift에서 `NotificationCenter라는` 기능을 사용하여 옵저버 패턴을 구현할 수 있습니다. `NotificationCenter는` 앱 내부에서 발생하는 특정 이벤트나 정보를 방송하고, 그것을 수신하는 Observer들에게 전달합니다.
 
	Swift에서 옵저버 패턴을 구현할 때 NotificationCenter를 사용하는 것이 일반적인 방법이지만, 이외에도 KVO(Key-Value Observing), Property Observers, Combine Framework 등 다양한 방법을 활용할 수 있습니다.
---
#### 구성요소
1. **Subject (주체)**: Subject는 상태를 가지고 있는 객체입니다. 이 상태가 변하면, 그 변화를 Observer에게 알립니다. Subject는 Observer를 등록하거나 제거하는 메서드와, 상태 변경을 알리는 메서드를 제공합니다.
    
2. **Observer (관찰자)**: Observer는 Subject의 상태 변화를 관찰하는 객체입니다. Subject의 상태가 바뀔 때마다 업데이트되며, 이 업데이트 동작은 일반적으로 콜백 함수나 메서드 형식으로 구현됩니다.
    

	이 두 요소 간의 관계는 다음과 같습니다:
	
	- 하나의 `Subject`는 여러 `Observer`를 가질 수 있습니다.
	- 각 Observer는 자신이 관심 있는 특정 Subject에 대한 `참조`를 유지합니다.
	- Subject의 상태가 변경되면, 모든 등록된 Observer에게 그 변경 사항을 알립니다.
---
### 장점, 단점

- #### 장점
	1. **Loose Coupling**: 옵저버 패턴은 Subject와 Observer 사이의 결합도를 낮춥니다. Observer는 특정 이벤트가 발생했을 때만 알림을 받으며, 그 외에는 서로 독립적으로 동작합니다. 이렇게 함으로써 코드의 유연성과 재사용성이 향상됩니다.
	    
	2. **Broadcast Communication**: Subject는 상태 변경을 모든 등록된 Observer에게 알리므로, 한 번의 이벤트 발생으로 여러 객체에 영향을 줄 수 있습니다.
	    
	3. **Dynamic Relationships**: 실행 시간 중에 Observer를 추가하거나 제거할 수 있으므로, 동적인 관계를 구현할 수 있습니다.
    

- #### 단점
	1. **Unexpected Updates**: Subject의 상태가 변경될 때마다 모든 Observer가 업데이트되므로, 예상치 못한 업데이트가 발생할 가능성이 있습니다. 이는 성능 저하를 초래할 수도 있습니다.
	    
	2. **Memory Leaks and Dangling References**: 순환 참조나 미제거 옵저버 등으로 인해 메모리 누수나 참조 문제가 발생할 수 있습니다. 따라서 메모리 관리와 참조 제거에 주의해야 합니다.
	    
	3. **Debugging Difficulty**: 옵저버 패턴은 코드 흐름을 따라가기 어려울 수 있는 비동기적인 동작 방식을 가지고 있다보니 디버깅이 어려울 수 있습니다.
---
## Delegates와 Notification 방식의 차이점에 대해 설명하시오.

### Delegates?
객체 간의 통신을 위한 방법 중 하나로, 하나의 객체가 다른 객체에 특정 작업을 위임하는 것을 의미합니다. Delegate는 프로토콜을 통해 정의됩니다. 이 프로토콜은 Delegate가 수행해야 하는 메서드를 선언하며, Delegate 객체는 이 메서드를 구현하여 작업을 수행합니다.

### Notification?
NotificationCenter를 통해 객체 간의 통신을 수행하는 방법입니다. 이 방식은 특정 객체가 이벤트를 발생시킬 때, 이에 대해 관심을 가지고 있는 모든 객체에게 알림을 보내는 일대다 통신 방식입니다.

NotificationCenter에는 Observer를 등록하여 특정 이벤트에 대한 알림을 받을 수 있습니다. 이벤트가 발생하면 NotificationCenter는 해당 이벤트에 대해 등록된 모든 Observer에게 알림을 보냅니다.

---
### 차이점
1. #### **Communication Style (통신 방식)**:
	- **Delegates**
		- Delegate 패턴은 `일대일 통신`을 지원합니다. 하나의 객체가 다른 객체에게 특정 이벤트나 작업을 위임하고 결과를 돌려받을 수 있습니다. Delegate는 프로토콜을 통해 상호작용하므로 컴파일 타임에 타입 안전성을 보장할 수 있습니다.
	- **Notification**
		- Notification 방식은 `일대다 통신`을 지원합니다. 하나의 객체가 이벤트 발생 시 Notification Center를 통해 등록된 다른 여러 객체에게 알림을 보낼 수 있습니다. 객체 간의 직접적인 의존성을 줄일 수 있습니다.
2. #### **Coupling (결합도)**:
	- **Delegates**
		- Delegate 패턴은 높은 결합도를 가지고 있습니다. Delegate 객체와 해당 객체 사이에 `직접적인 관계`가 있어야 합니다. Delegate 객체를 교체하거나 다른 객체와의 관계를 변경할 때 수정이 필요합니다.
	- **Notification**
		- Notification 방식은 낮은 결합도를 가지고 있습니다. Subject와 Observer 사이에 직접적인 관계가 없으며, 객체들은 `Notification Center`를 통해 통신합니다. 이로 인해 객체 간의 관계 변경이 더 유연하고 동적으로 이루어질 수 있습니다.
3. #### **Event Handling (이벤트 처리)**:
	- **Delegates**
		- Delegate 패턴은 이벤트 처리를 위해 `메서드를 호출`하는 방식입니다. Delegate 객체는 Delegate 프로토콜을 준수하고 해당 메서드를 구현하여 이벤트를 처리합니다.
	- **Notification**
		- Notification 방식은 이벤트에 대한` Notification 객체`를 생성하여 Notification Center에게 전달합니다. 등록된 Observer는 Notification을 수신하기 위해 Notification Center를 통해 Notification을 구독하고 처리합니다.
