# 질문 리스트
- 옵저버 패턴에 대해 설명하시오.
- Delegation와 Notification 방식의 차이점에 대해 설명하시오.

## 옵저버 패턴에 대해 설명하시오.
스위프트에서 옵저버 패턴은 객체의 상태 변화를 다른 객체들에게 알리는 디자인 패턴. 이 패턴은 한 객체(Subject)가 변경될 때마다 그 변화를 관찰하는 다른 모든 객체(Observer)들에게 자동으로 알려주는 방식으로 작동함.

```
//옵저버 패턴 예제
protocol Observer: AnyObject {
    func update() // 관찰 대상이 변경될 때 호출되는 메소드를 정의.
}

// Subject라는 클래스를 정의합니다. 이 클래스는 옵저버들을 관리하는 역할.
class Subject {
    private var observers: [Observer] = [] // 옵저버들을 저장하는 배열

    // 새로운 옵저버를 추가하는 메소드
    func attach(observer: Observer) {
        observers.append(observer)
    }

    // 기존의 옵저버를 제거하는 메소드
    func detach(observer: Observer) {
        observers.removeAll { $0 === observer }
    }

    // 모든 옵저버들에게 변화를 알리는 메소드
    func notify() {
        for observer in observers {
            observer.update()
        }
    }
}

// 실제로 'Observer' 프로토콜을 구현하는 구체적인 클래스
class ConcreteObserver: Observer {

  private let name: String // 각 인스턴스별 고유 이름

  init(name: String) { 
      self.name = name  // 초기화 시 이름 설정
  }

  // 상태 변화시 출력문으로 업데이트 사실 표시
  func update() {
      print("\(name) received an update.")
  }
}

let subject = Subject() // Subject 인스턴스를 생성

// "Observer A"라는 이름을 가진 ConcreteObserver 인스턴스를 생성
let observerA = ConcreteObserver(name: "Observer A") 

// "Observer B"라는 이름을 가진 ConcreteObserver 인스턴스를 생성
let observerB = ConcreteObserver(name: "Observer B") 


subject.attach(observer: observerA) 
// Observer A를 subject에 붙여(attach) 관찰자로 등록
subject.attach(observer: observerB) 
// Observer B도 마찬가지로 붙여(attach) 관찰자로 등록

subject.notify() // 모든 옵저버들에게 변화를 알림. 이 경우, Observer A와 Observer B 모두 update 메소드가 호출되어 콘솔에 출력.

subject.detach(observer: observerA) // Observer A를 제거(detach)하여 더 이상 변화를 감지하지 않도록 합니다.

subject.notify()  // 다시 모든 옵저버들에게 변화를 알림. 이번엔 Observer A가 제거되었으므로, 오직 Observer B만 update 메소드가 호출되어 콘솔에 출력.

```
### 옵저버 패턴을 사용하는 이유

> -느슨한 결합 (Loose Coupling): 옵저버 패턴은 관찰 대상(Subject)과 관찰자(Observer) 사이에 느슨한 결합을 제공. 이는 한 객체의 변경이 다른 객체에 직접적인 영향을 미치지 않도록 하며, 코드의 유연성과 확장성을 향상시킴. 
즉, 한 객체의 변화를 다른 객체가 알아야 하지만, 그 변화가 어떻게 전달되는지나 어떤 시점에서 전달되는지에 대해서는 서로 독립적으로 유지할 수 있음.
- 상태 변화 공유: 상태를 가진 주체가 있고 그 상태의 변화를 여러 개체들이 알아야 하는 경우에 사용. 주체의 상태가 변경되면 모든 등록된 옵저버들에게 알림을 보내어 해당 상태의 변경 사항을 공유.
예를 들어, UI 업데이트나 데이터 바인딩에서 자주 볼 수 있는 시나리오. UI 요소 (예: 텍스트 필드, 슬라이더 등)의 값이 변경되면 연결된 다른 UI 요소나 데이터 모델도 해당 변경사항을 반영해야 할 때 이 패턴은 매우 유용함.
- 비동기 프로그래밍: 예를 들어 네트워크 요청 후 응답이 오면 그 결과값으로 여러 작업들 (UI 업데이트, 로컬 저장소 업데이트 등) 을 해야하는 경우 등.


### 질문
- 느슨한 결합이 무엇인가?
: 느슨한 결합은 하나의 컴포넌트의 변경이 다른 컴포넌트들의 변경을 요구하는 위험을 줄이는 것을 목적으로 하는 시스템에서  컴포넌트 간의 내부 의존성을 줄이는 것을 추구하는 디자인 목표. 느슨한 결합은 시스템을 더욱 유지 할 수 있도록 만들고, 전체 프레임워크를 더욱 안정적으로 만들고 시스템의 유연성을 증가하게 하려는 의도를 가진 포괄적인 개념임.
- 옵저버 패턴을 대체할 수 있는 다른 패턴이 있는가?
: Publish-Subscribe (Pub-Sub) Pattern은 옵저버 패턴과 매우 유사하지만, 중간에 메시지 브로커나 이벤트 채널이 존재하는 것이 특징. 발행자(Publisher)가 직접 구독자(Subscriber)에게 메시지를 보내는 대신, 메시지 브로커를 통해 메시지가 전달됨. 이로 인해 발행자와 구독자 사이의 결합도가 더욱 낮아짐. Pub-Sub 패턴은 옵저버 패턴의 확장된 형태라고 볼 수 있으며, 좀 더 복잡한 시나리오에서 사용될 수 있음.


## Delegation와 Notification 방식의 차이점에 대해 설명하시오.
Delegation과 Notification은 모두 애플리케이션 내에서 객체 간의 통신을 위한 디자인 패턴입니다. 두 방식 모두 정보를 전달하는 목적은 같지만, 사용되는 상황과 방식에는 몇 가지 차이점이 있음. 

> - Delegation 
일대일(one-to-one) 통신 패턴으로 하나의 객체(델리게이트)가 다른 객체의 이벤트에 응답하도록 설계되어 있음.
Delegate는 특정 이벤트가 발생했을 때 어떻게 처리해야 할지를 정의. 예를 들어, UITableView의 경우 스크롤, 선택 등의 이벤트가 발생하면UITableViewDelegate에 정의된 메소드를 호출하여 처리합니다.
Delegation 이 사용되는 경우는 대체로 '행동'을 위임받아 처리하는 상황.
- Notification
일대다(one-to-many) 통신 패턴으로 한 객체가 변경 사항을 알릴 때 여러 객체들에게 동시에 알릴 수 있음.
NotificationCenter를 사용하여 이벤트를 발생시키고, 해당 이벤트에 대해 관심을 가진 여러 객체들(옵저버)가 그것을 수신하여 반응할 수 있음.
Notification이 주로 사용되는 경우는 '상태 변화'를 다수의 관찰자들에게 알려주어야 하는 상황.

### 차이점
Notification과 다르게 Delegate의 경우에는 수신자가 발신자의 정보를 알고 있어야 함.

SomeDelegate에서 SomeDelegate을 가진 객체는 SomeDelegate protocol의 메서드로 이벤트가 발생했을 때 처리해줄 수 있는 메서드를 요구함으로써 SomeDelegate을 가진 객체가 이벤트를 처리할 수 있도록 해줌.

SomeDelegate protocol을 제작할 때 메서드의 틀을 미리 잡아놓아야하며, SomeDelegate이라는 변수를 이벤트 처리 담당 객체가 들고 있어야 한다. 하지만 Notification의 경우에는 이 과정이 무의미하다. 단순히 어떤 값의 변화를 포착하여 그에 맞는 이벤트를 발생시켜주면 됨.

Delegation은 보다 구체적인 행동 제어와 연관성 있는 작업처리가 필요한 경우 유용하며, Notification은 넓고 복잡한 범위에서 정보 공유와 상태 변경 등록 및 처리가 필요한 경우 유용함.

Delegate을 사용하여 설계하지 않고 Notification만 가지고 설계를 했다면 다른 사람이 코드를 유지보수할 때 이 Notification을 누가 Subscribe하고 있는지 알기가 어려움. 반면 Delegate으로 설계를 했다면 해당 Delegate의 이름, 프로토콜의 메서드 등을 통해 어떤 과정에 의해 이벤트 처리 로직이 돌아가는지 파악하기 한결 수월함.


### 질문
- Delegation과 Notification 패턴 중 어떤 것을 선택할지 결정하는 기준은 무엇인가?
: Delegation은 한 객체에서 발생한 특정 이벤트를 다른 한 개체만 처리해야 할 경우 혹은 구체적인 행동 제어와 연관된 작업 처리가 필요한 경우.
Notification은 한 객체에서 발생한 특정 이벤트를 여러 개체에서 알아야 하는 경우 혹은 넓고 복잡한 범위에서 정보 공유와 상태 변경 등록 및 처리가 필요한 경우.

- 실제 애플리케이션 개발에서 Delegation과 Notification 패턴은 어떻게 함께 사용될 수 있는가?
:실제 애플리케이션에서는 Delegation과 Notification 패턴을 함께 사용하여 코드의 유연성과 확장성을 높일 수 있음. 예를 들어, 특정 UI 요소(예: 텍스트 필드)의 이벤트 처리를 위해 Delegation 패턴을 사용하고, 동시에 여러 객체에게 해당 UI 요소의 상태 변화를 알려주기 위해 Notification 패턴을 사용할 수 있습니다. 이런 방식으로 두 패턴을 적절히 조합하면 복잡한 프로그램 구조를 효율적으로 관리할 수 있음.