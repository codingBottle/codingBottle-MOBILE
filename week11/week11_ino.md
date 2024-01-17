# week11 Algorithm_ino

> 1번 문제 : [중복된 문자 제거](https://school.programmers.co.kr/learn/courses/30/lessons/120888)
> 
- 풀이실패 유형 : X
- 풀기까지 걸린 시간 : 3분
- 사용한 알고리즘 : 완전탐색
- 접근부터 풀이까지의 과정
    1. 이미있는 문자일 경우 continue, 아닐 경우 추가
- 성공 코드

```swift
import Foundation

func solution(_ my_string:String) -> String {
    var answer = ""
    for i in my_string {
        if answer.contains(i) {
            continue
        }
        else {
            answer.append(i)
        }
        
    }
    return answer
}
```

> 2번 문제 : [주사위](https://school.programmers.co.kr/learn/courses/30/lessons/181916)
> 
- 풀이실패 유형 : O
    - 풀기까지 걸린 시간 : x
- 사용한 알고리즘 : 완전탐색
- 접근부터 풀이까지의 과정
    1. dictionary 형태로 구현한 뒤 key: val 별로 값을 저장 후 카운트한뒤 이를 이용한 풀이 시도
    2. 실패

> 3번 문제 : [서울에서 김서방 찾기](https://school.programmers.co.kr/learn/courses/30/lessons/12919)
> 
- 풀이실패 유형 : X
- 풀기까지 걸린 시간 : 3분
- 사용한 알고리즘 : 완전탐색
- 접근부터 풀이까지의 과정
    1. 이미있는 문자일 경우 continue, 아닐 경우 추가
- 성공 코드

```swift
func solution(_ seoul:[String]) -> String {
    var answer = 0
    for (idx, i) in seoul.enumerated() {
        if i == "Kim" {
            answer = idx
        }
    }
    return "김서방은 \(answer)에 있다"
}
```