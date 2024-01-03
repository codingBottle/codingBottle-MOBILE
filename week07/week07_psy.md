# 23.11.08 \_ AD 7주차

<br>

## Q : Safearea에 대해서 설명하시오.

### Safearea에 관하여

<br>

### 등장배경 :

- iOS 11이 나오기 이전, iOS7에서 등장한 **Top / Bottom Layout Guide** 라는 것이 존재했다.
  - 이는 **상태바(Status Bar), 내비게이션 바(Navigation Bar), 탭바(Tabbar)** 등에 의해서View가 가려지지 않기 위해서 제공 되던 시스템적인 마진이다.
    <img width="534" alt="1" src="https://github.com/ParkSY0919/ParkSY0919/assets/114901417/2956c17a-2f67-4a0a-8f51-d50ef550e4a3">
- 그러나 iOS 11로 넘어오며 위에 노치가 생겼고 이를 Landscape로 사용했을 때에 아래와 같은 문제점이 나타났다.

  - 따라서 Landscape 일 때 Leading / Trailing에 대한 마진도 필요하게 되었다.

      <img width="434" alt="2" src="https://github.com/ParkSY0919/ParkSY0919/assets/114901417/7b98102e-dfb5-453b-bb03-b2c7cb45dec5">

- 위와 같은 이유로 **Top, Bottom 뿐만 아니라, Leading, Trailing도 시스템 마진이 필요**해졌고, **Top, Bottom, Leading, Trailing 시스템 마진을 모두 가지는 `Safe Area`가 등장하게 된 것이다.**

### Safe Area 란:

- 앞서 말한 것과 같이, 노치 디자인, 홈바, 상태 표시줄 등 특정 UI 요소들에 의해 부분적으로 가려질 수 있는 콘텐츠를 보호하기 위하여 모든 방면으로 시스템 마진을 모두 가진 영역을 Safe Area라 칭한다.
- ViewController를 생성하게 되면 기본적으로 View에 Safe Area가 들어간다.

  - View를 하나 추가하여 Constraints를 모두 0으로 설정하면 아래와 같이 기준이 Safe Area로 잡힌다

      <img width="328" alt="3" src="https://github.com/ParkSY0919/ParkSY0919/assets/114901417/56dd86e4-975b-4595-8a4b-47e989e9456d">

      <img width="608" alt="4" src="https://github.com/ParkSY0919/ParkSY0919/assets/114901417/35c5b274-bc07-465c-a348-210102258b8d">

<aside>
❓ Q: 2번 째 사진의 경우 노치는 Trailing에만 있는데, 왜 Leading에도 마진이 들어갔는가?

A:

**Lancscape일 때 전체 화면에 대한 View의 비율을 맞추기 위해, 자동으로 노치 반대 부분에도 그만큼의 마진이 들어간다.**

</aside>

### Safe Area를 사용하지 않는 방법

- Constraint의 기준을 equalToSuperView()와 같이 모든 방면을 준다면 Safe Area의 제약에 걸리지 않게 된다.
  - 그러나 특별한 경우가 아닌 이상 Safe Area 사용을 권장함을 유의하자.

### Safe Area Custom 가능 여부

- **safeAreaInsets는 읽기 전용 프로퍼티이기에 직접적인 변경은 불가능하나, 아래 코드와 같이additionalSafeAreaInsets 프로퍼티를 사용하여 Safe Area의 크기를 늘리는 등의 커스텀은 가능합니다.**
  ```swift
  override func viewDidAppear(_ animated: Bool) {
     var newSafeArea = UIEdgeInsets()
     // Adjust the safe area to accommodate
     //  the width of the side view.
     if let sideViewWidth = sideView?.bounds.size.width {
        newSafeArea.right += sideViewWidth
     }
     // Adjust the safe area to accommodate
     //  the height of the bottom view.
     if let bottomViewHeight = bottomView?.bounds.size.height {
        newSafeArea.bottom += bottomViewHeight
     }
     // Adjust the safe area insets of the
     //  embedded child view controller.
     let child = self.childViewControllers[0]
     child.additionalSafeAreaInsets = newSafeArea
  }
  ```

<aside>
❓ **Q: 만약에 뷰의 위치나 방향이 변경된다면, safeAreaInsets 값도 자동으로 바뀌는가?

A:
뷰의 위치나 방향이 변경되면, 시스템은 자동으로 해당 뷰의 `safeAreaInsets` 값을 업데이트하기에 자동으로 바뀐다.
따라서 개발자는 `safeAreaInsets` 의 변화된 값을 관찰하고 하여 `safeAreaInsets` 값이 변화될 때마다 호출되는 viewSafeAreaInsetsDidChange() 메소드를 사용하여 이에 맞게 레이아웃 업데이트 로직을 구현해야한다.\*\*

</aside>

➕ 참고:

- [https://velog.io/@ryan-son/Safe-area-정리](https://velog.io/@ryan-son/Safe-area-%EC%A0%95%EB%A6%AC)
- [https://babbab2.tistory.com/134](https://babbab2.tistory.com/134)

---

<br>

## Q : Left Constraint 와 Leading Constraint 의 차이점을 설명하시오.

### Left Constraint와 Right Constraint

- **사용자가 보는 화면상의 왼쪽과 오른쪽 위치 속성으로 이는 절대적이며 `Left`는 어떤 객체의 왼쪽을 뜻하고, `Right`는 어떤 객체의 오른쪽을 뜻한다.**

### **Leading Constraint와 Trailing Constraint**

- 이는 device locale의 영향을 받기에 **언어 및 지역 설정에 따른 읽기 방향에 따라 동작이 달라진다.**
- 읽기 방향이 오른쪽에서 왼쪽인 locale(예: 히브리어, 아랍어)에서는 leading이 오른쪽이 되고 trailing이 왼쪽이 된다.
- 예시

  ```swift
  final class ViewController: UIViewController {

      // MARK:- Views
      private var label: UILabel = {
          let label = UILabel()
          label.text = NSLocalizedString("Hello", comment: "constraint ex1")
          label.translatesAutoresizingMaskIntoConstraints = false
          return label
      }()

      private var button: UIButton = {
          let button = UIButton()
          let pressLocalize = NSLocalizedString("Press", comment: "constraint ex2")
          button.setTitle(pressLocalize, for: .normal)
          button.backgroundColor = .black
          button.translatesAutoresizingMaskIntoConstraints = false
          return button
      }()

      // MARK:- Lifecycle methods
      override func viewDidLoad() {
          super.viewDidLoad()

          // add subviews
          view.addSubview(label)
          view.addSubview(button)

          // define constraints
          NSLayoutConstraint.activate([
              label.leadingAnchor.constraint(equalTo: view.safeAreaLayoutGuide.leadingAnchor, constant: 10),
              label.topAnchor.constraint(equalTo: view.safeAreaLayoutGuide.topAnchor, constant: 10),
              button.leadingAnchor.constraint(equalTo: label.trailingAnchor, constant: 50),
              button.lastBaselineAnchor.constraint(equalTo: label.lastBaselineAnchor)
          ])
      }

  }
  ```

  - 위와 같은 코드를 사용하였을 때 “label은 왼쪽에 붙어 있을 것이고, button은 50 떨어진 위치에 있을 것이다.” 라고 예상할 수 있지만, 아랍권 사람들은 leading이 오른쪽이라서 “label은 오른쪽에 붙어있고 그로부터 50 떨어진 위치에 button이 있을 것이다.” 라고 기대할 것 입니다.
  - 결과는 아래와 같습니다.
    <img width="828" alt="5" src="https://github.com/ParkSY0919/ParkSY0919/assets/114901417/2efa4e24-5570-405c-9ae7-2a541eff14b6">

  - 코드를 아래와 같이 수정하면 그 하단 첨부 사진과 같이 변화합니다.
    ```swift
    NSLayoutConstraint.activate([
                label.leftAnchor.constraint(equalTo: view.safeAreaLayoutGuide.leftAnchor, constant: 10),
                label.topAnchor.constraint(equalTo: view.safeAreaLayoutGuide.topAnchor, constant: 10),
                button.leftAnchor.constraint(equalTo: label.rightAnchor, constant: 50),
                button.lastBaselineAnchor.constraint(equalTo: label.lastBaselineAnchor)
            ])
    ```
      <img width="810" alt="6" src="https://github.com/ParkSY0919/ParkSY0919/assets/114901417/f5d27d41-3b4c-4419-a44b-5e73c7606c43">

➕ 참고:

- [https://sueaty.tistory.com/175](https://sueaty.tistory.com/175)
