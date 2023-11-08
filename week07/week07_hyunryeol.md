## Safearea에 대해서 설명하시오.
    ios 11 이후 기존 디바이스 간섭을 없에는 guideline 방식에 무리가 생겨 도입된 방법
    아이폰에 노치가 생기면서 가로 방향으로 화면을 보일 때 노치에 걸리는 문제점 때문
```swift
let label = UILabel()
label.translatesAutoresizingMaskIntoConstraints = false
label.text = "Hello, Safe Area!"
view.addSubview(label)

NSLayoutConstraint.activate([
    label.topAnchor.constraint(equalTo: view.safeAreaLayoutGuide.topAnchor, constant: 20),
    label.leadingAnchor.constraint(equalTo: view.safeAreaLayoutGuide.leadingAnchor, constant: 20),
    label.trailingAnchor.constraint(equalTo: view.safeAreaLayoutGuide.trailingAnchor, constant: -20)
])

```
## Left Constraint 와 Leading Constraint 의 차이점을 설명하시오.
    contsraint = > 제약 조건
1. ### left
   뷰의 가장 왼쪽 부분에서 부터 제약 조건을 두는것
2. ### Leading
   뷰의 시작 위치부터 제약 조건을 두는 것
 텍스트를 읽는 언어에서 시작 가장자리가 되지만, 오른쪽에서 왼쪽으로 텍스트를 읽는 언어(예: 아랍어, 히브리어)에서는 오른쪽 가장자리를 가리킵니다.
 `다국어` 지원을 고려할 때 
 Leading 과 Trailing을 잘 고려해야함

## *추가 공부내용
1. ### Safe Area가 없는 이전 버전의 iOS에서는 어떻게 화면 가장자리를 피하나요?
    iOS 11 이전에는 상태 표시줄, 내비게이션 바, 툴바, 탭 바 등의 시스템 제공 뷰의 레이아웃 가이드를 사용하여 화면 가장자리를 피했습니다. 이렇게 하면 시스템 제공 뷰와 겹치지 않도록 콘텐츠를 배치할 수 있습니다.
2. ### Auto Layout에서 Priority란 무엇인가요?
    Priority는 레이아웃 제약의 중요도를 나타내는 값으로, 1부터 1000까지의 값을 가집니다. 더 높은 우선순위를 가진 제약이 더 먼저 충족되며, 같은 우선순위를 가진 제약들은 가능한 동등하게 충족됩니다.
3. ### Constraint를 코드로 어떻게 설정하나요?
    NSLayoutConstraint 클래스를 사용하여 제약을 생성하고, 이를 뷰의 addConstraint(_:) 메서드를 통해 적용할 수 있습니다. 또는 뷰의 NSLayoutConstraint.activate(_:) 메서드를 사용하여 여러 제약을 한 번에 활성화할 수 있습니다. 또한, iOS 9 이후에는 NSLayoutAnchor를 사용하여 더 간결하게 제약을 설정할 수 있습니다.