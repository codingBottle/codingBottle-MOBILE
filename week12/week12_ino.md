# week12 Algorithm_ino

> 1번 문제 : [더 크게 합치기](https://school.programmers.co.kr/learn/courses/30/lessons/181939)
> 
- 풀이실패 유형 : X
- 풀기까지 걸린 시간 : 3분
- 사용한 알고리즘 : 형변환
- 접근부터 풀이까지의 과정
    1. string으로 변환후 더하기연산 
    2. 다시 int로 변환 후 삼항 연산자로 리턴
- 성공 코드

```swift
import Foundation

func solution(_ a:Int, _ b:Int) -> Int {
    return Int(String(a)+String(b))! > Int(String(b)+String(a))! ? Int(String(a)+String(b))! : Int(String(b)+String(a))! 
}
```

> 2번 문제 : [피자 나눠 먹기 (2)](https://school.programmers.co.kr/learn/courses/30/lessons/120815)
> 
- 풀이실패 유형 : X
    - 풀기까지 걸린 시간 : 15분
- 사용한 알고리즘 : 완전탐색
- 접근부터 풀이까지의 과정
    1. while문으로 반복하여 기준달성시 return
    2. 오류 발생
    3. 그냥 for-loop를 이용하여 기준 달성시 break후 return
- 성공 코드
    
```swift
import Foundation
    
func solution(_ n:Int) -> Int {
    var answer = 0
    for i in 1...100 {
        if ( i * 6 ) % n == 0 {
            answer = i
            break
        }
        else {
            continue
        }
    }
    return answer
}
```
    

> 3번 문제 : [문자열 여러 번 뒤집기](https://school.programmers.co.kr/learn/courses/30/lessons/181913)
> 
- 풀이실패 유형 : X
- 풀기까지 걸린 시간 : 20분
- 사용한 알고리즘 : 완전탐색, 형변환
- 접근부터 풀이까지의 과정
    1. 문자열을 char형 배열로 형변환
    2. queries를 순환하며 해당 범위만큼 문자열 대체
- 성공 코드

```swift
import Foundation

func solution(_ my_string:String, _ queries:[[Int]]) -> String {
    var chars = Array(my_string)
    for i in queries {
    let rev = chars[i[0]...i[1]].reversed()
    chars.replaceSubrange(i[0]...i[1], with: rev)
    }
    return String(chars)
}
```

> 4번 문제 : [](https://school.programmers.co.kr/learn/courses/30/lessons/120869)[배열 조각하기](https://school.programmers.co.kr/learn/courses/30/lessons/181893)
> 
- 풀이실패 유형 : x
- 풀기까지 걸린 시간 : 20분
- 사용한 알고리즘 : 완전탐색
- 접근부터 풀이까지의 과정
1. 반복하여 index를 받은 뒤 removesubrange를 통해 제거 시도
2. 문자열 가공 방법을 찾던 중 prefix, suffix 발견
3. 더 낫겠다는 생각이 들어 변경
- 성공 코드

```swift
import Foundation

func solution(_ arr:[Int], _ query:[Int]) -> [Int] {
    var temp = arr
    for (idx,i) in query.enumerated() {
        if idx % 2 == 0 {
            temp = Array(temp.prefix(i+1))
        }
        else {
            temp = Array(temp.suffix(temp.count-i))
        }
    }
    return temp
}
```

> 1번 문제 : [](https://school.programmers.co.kr/learn/courses/30/lessons/120869)
> 
- 풀이실패 유형 : x
- 풀기까지 걸린 시간 : 40분
- 사용한 알고리즘 : 완전탐색
- 접근부터 풀이까지의 과정
1. 전체 사람 수 - 잃어버린 사람의 수를 한 뒤 잃어버린 사람을 순회하며 -1, 1, +1의 경우를 체크함
2. 실패 이후 정렬, 중복을 미리 제거해주기 등으로 실패케이스를 추가하며 디벨롭
- 성공 코드

```swift
import Foundation

func solution(_ n:Int, _ lost:[Int], _ reserve:[Int]) -> Int {
    var answer = n - lost.count
    var lost = lost.sorted(by:<)
    var reserve = reserve.sorted(by:<)
    for i in lost {
        if reserve.contains(i) {
            answer += 1
            reserve.removeAll(where: { $0 == i })
            lost.removeAll(where: { $0 == i })
            continue
        }
    }
    for j in lost {
        if reserve.contains(j-1) {
            answer += 1
            reserve.removeAll(where: { $0 == j-1 })
            continue
        }
        else if reserve.contains(j+1) {
            answer += 1
            reserve.removeAll(where: { $0 == j+1 })
            continue
        }
    }
    return answer
}
```
