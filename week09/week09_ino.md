# StackView 장단점, Frame - Bounds 차이점

장점 - 

1. 코드를 간결하게 작성하기 쉬움
2. 새로운 뷰를 추가해야 할 경우 유연하게 대처하기 쉬움
3. 자동으로 뷰를 배치하므로 대응이 편함.

단점 - 

1. 특정 뷰의 위치 및 크기의 세밀한 조작이 제한됨
2. 내부적으로 많은 제약 조건을 사용하므로 레이아웃이 복잡해질경우 성능 이슈가 발생할 수 있음.

Frame - Bouds 간 공통점으로는 CGRect(origin, size) 를 갖는다는 점.

차이점은 

“frame - The frame rectangle, which describes the view’s location and size in its superview’s coordinate system” 

SuperView 의 좌표시스템 내에서 View 의 위치와 크기를 나타내는 개념.

즉, 한단계 상위뷰의 좌상단에서 떨어진만큼의 origin에서부터 사각형을 그림.

Bounds?

The bounds rectangle, which describes the view’s location and size in its own coordinate system.

View 의 위치와 크기를 자신만의 좌표시스템 내에서 나타냄.

Bounds.origin 의 x, y 를 바꾸게 되면 내부의 contents가 -x, -y 만큼 이동함

why? bounds를 변경하는 것은 해당 위치에서 view를 다시 그리는 의미가 됨. bounds는 상위뷰와 관련 x, 따라서 내부의 뷰가 움직이는 것처럼 보이는 것임.