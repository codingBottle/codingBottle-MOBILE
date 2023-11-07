# 질문 리스트
- Safearea에 대해서 설명하시오.
- Left Constraint 와 Leading Constraint 의 차이점을 설명하시오.

## SafeArea에 대해서 설명하시오.
iOS 11부터 도입된 개념. 
뷰의 콘텐츠가 화면의 특정 부분에 의해 가려지지 않도록 보장하는 영역을 의미.
Safearea가 생기기 전에는 Top / Bottom Layout Guide가 존재했음.
상태바, 네비게이션바, 탭바 등에 의해 View가 가려지지 않기 위해 제공되던 것.
-> 콘텐츠가 안전하게 표시될 수 있는 영역

iPhone X 이상의 모델에서는 노치, 홈 인디케이터 등의 UI 요소 때문에 화면의 일부 영역이 가려질 수 있는데 이런 경우, 중요한 콘텐츠가 해당 요소에 의해 가려지는 것을 방지하기 위해 Safe Area를 사용.

시스템에 의해 가려질 수 있는 부분의 margin을 자체적으로 가지는 것이 SafeArea.

Safe Area는 Interface Builder에서 설정할 수도 있고, 코드로 접근 가능. 코드로 접근하는 경우 UIView의 safeAreaLayoutGuide 속성을 사용.

### 질문
- Safe Area와 Auto Layout은 어떤 관계가 있나요?
: Safe Area는 Auto Layout의 일부로서 도입됨. Safe Area는 뷰의 레이아웃 가이드 중 하나로, Auto Layout 제약 조건을 생성할 때 참조점으로 사용.

- Safe Area 밖에 콘텐츠를 배치하려면 어떻게 해야 하나요?
:뷰의 제약 조건을 Safe Area가 아닌 Superview에 연결해야 함.

## Left Constraint 와 Leading Constraint 의 차이점을 설명하시오.

Left Right은 화면상에서 왼쪽과 오른쪽 위치 속성이고 Reading, Trailing은 reading direction의 시작과 끝을 나타내는 위치 속성.

reading direction: 우리가 글을 읽을 때처럼 읽는 방향을 말함 (한글 왼쪽->오른쪽). 
reading direction의 디폴트 값도 사용자 설정 언어에 영향을 받음.

Left Constraint는  항상 화면의 왼쪽 가장자리를 기준으로 뷰의 위치를 결정. 이는 언어 및 지역 설정에 관계없이 항상 동일하게 적용됨.

Leading Constraint는 현재 환경의 텍스트 방향에 따라 달라짐. 대부분의 언어(영어, 한국어 등)에서는 텍스트가 왼쪽에서 오른쪽으로 표시되므로, Leading Constraint는 왼쪽 가장자리를 기준으로 함. 
하지만 일부 언어(아랍어, 히브리어 등)에서는 텍스트가 오른쪽에서 왼쪽으로 표시되므로, 이 경우 Leading Constraint는 오른쪽 가장자리를 기준으로 함.

앱이 다양한 언어와 지역에서 사용되도록 설계된 경우, Leading Constraint와 Trailing Constraint를 사용하여 뷰의 위치를 결정하는 것이 좋음.

### 질문
- 언제 Left Constraint를 사용하고, 언제 Leading Constraint를 사용해야 하나요?
: 다양한 언어와 지역을 지원하는 앱에서 Leading Constraint가 사용. 이는 대부분의 언어(영어, 한국어 등)에서 텍스트가 왼쪽에서 오른쪽으로 읽히는 반면, 아랍어나 히브리어 등 일부 언어에서는 오른쪽에서 왼쪽으로 읽히기 때문. Leading Constraint는 이러한 텍스트 방향에 따라 동적으로 변경되므로, 사용자의 언어 설정에 따라 UI가 자동으로 조정됨.
Left Constraint는 항상 화면의 왼쪽 가장자리를 기준으로 뷰의 위치를 결정. 따라서, 앱이 한 가지 언어 또는 문화만 지원하도록 설계된 경우에는 Left Constraint를 사용할 수 있음.

- Left Constraint와 Leading Constraint 중 어느 것이 더 권장되나요?
:애플 공식 문서에서도 Leading Constraint를 사용하는 것을 권장하고 있고, 앱의 접근성을 높이고 더 많은 사용자를 대상으로 하려면 Leading Constraint와 Trailing Constraint를 사용하는 것이 좋음.
