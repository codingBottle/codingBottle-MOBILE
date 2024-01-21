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
- 풀이실패 유형 : 2. 어떻게 풀어야 할 지 아이디어는 생각했지만 어떤 유형의 알고리즘을 사용해야하는지 모르겠다.
    - 풀기까지 걸린 시간 : 1시간
- 사용한 알고리즘 : 완전탐색
- 접근부터 풀이까지의 과정
    1. dictionary 형태로 구현한 뒤 key: val 별로 값을 저장 후 카운트한뒤 이를 이용한 풀이 시도
    2. 실패
- 성공 코드
    
    ```swift
    import Foundation
    
     func solution(_ a:Int, _ b:Int, _ c:Int, _ d:Int) -> Int {
         let nums = [a, b, c, d]
         let counts = nums.map { num in nums.filter { $0 == num }.count }
    
         if counts.max() == 4 {
             return a * 1111
    
         } else if counts.max() == 3 {
             let p = nums[counts.firstIndex(of: 3)!]
             let q = nums[counts.firstIndex(of: 1)!]
                     return Int(pow(Double(10 * p + q), 2.0))
    
         } else if counts.max() == 2 {
             if counts.min() == 2 {
                 return a == b ? (a + c) * abs(a - c) : (a + b) * abs(a - b)
             } else {
                 let p = nums[counts.firstIndex(of: 2)!]
                 return nums.filter { $0 != p }.reduce(1, *)
             }
         } else {
             return nums.min()!
         }
     }
    ```
    

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

> 1번 문제 : [외계어 사전](https://school.programmers.co.kr/learn/courses/30/lessons/120869)
> 
- 풀이실패 유형 : x
- 성공 코드
- 사용한 알고리즘 : 완전탐색
- 접근부터 풀이까지의 과정
1. 탐색하며 a를 증가시켜 answer과 값이 같을 경우 1 return, 다를 경우 2 return

```swift
func solution(_ spell:[String], _ dic:[String]) -> Int {
    let a = spell.count
    for i in dic {
        var answer = 0
        for j in spell {
            if i.contains(j) {
                answer += 1
            }
        }
        if a == answer {
        return 1
            }
    }
        return 2
}
```
