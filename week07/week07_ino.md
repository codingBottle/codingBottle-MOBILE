# SafeArea / Left Constraint 와 Leading Costraint 차이

## Safearea에 대해서 설명하시오.

아이폰 X의 노치 등장으로 이전부터 

상태바(Status Bar), 내비게이션 바(Navigation Bar), 탭바(Tabbar) 등에 의해서

View가 가려지지 않기 위해서 제공 되던 시스템적인 마진인

top bottom layout guide를 대체하기 

위해 iOS 11에서 생겨난 새로운 개념.

상단바는 지금도 존재해서 상관 없지 않나 ? → 가로 모드일 경우 layout 문제 발생.

가로 모드로 돌릴경우 비율을 맞추기 위해 trailing , leading 모두 margin이 들어감

위 노치를 무시하고 뷰를 끝까지 올리고 싶다면 ? → superview 이용

Q1. 노치를 무시하고 뷰를 끝까지 올리고 싶으면 어떤 방법을 사용하면 되는가 ?

Q2. top bottom layout guide 대신 SafeArea를 사용해야 하는 이유는 ?

## Left Constraint 와 Leading Constraint 의 차이점을 설명하시오.

좌 → 우 방향으로 글씨를 쓰는 방식의 국가에선 같이 왼쪽 마진이 들어가나,

우 → 좌 방향으로 글씨를 쓰는 국가에선 localizing 할 시 leading constraint가 우측 마진으로 변경됨.