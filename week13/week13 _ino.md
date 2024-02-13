# week13 Algorithm_ino

> 1번 문제 : [](https://school.programmers.co.kr/learn/courses/30/lessons/181939)[영어가 싫어요](https://school.programmers.co.kr/learn/courses/30/lessons/120894)
> 
- 풀이실패 유형 : X
- 풀기까지 걸린 시간 : 20분
- 사용한 알고리즘 : 함수사용
- 접근부터 풀이까지의 과정
    1. 문자열 대치 함수를 사용하면 되겠다고 생각 → 성공
- 성공 코드

```swift
import Foundation

func solution(_ numbers:String) -> Int64 {
    var num = numbers
    num = num.replacingOccurrences(of: "zero", with: "0")
    num = num.replacingOccurrences(of: "one", with: "1")
    num = num.replacingOccurrences(of: "two", with: "2")
    num = num.replacingOccurrences(of: "three", with: "3")
    num = num.replacingOccurrences(of: "four", with: "4")
    num = num.replacingOccurrences(of: "five", with: "5")
    num = num.replacingOccurrences(of: "six", with: "6")
    num = num.replacingOccurrences(of: "seven", with: "7")
    num = num.replacingOccurrences(of: "eight", with: "8")
    num = num.replacingOccurrences(of: "nine", with: "9")
    return Int64(num)!
}
```

> 2번 문제 : [최댓값과 최솟값](https://school.programmers.co.kr/learn/courses/30/lessons/12939)
> 
- 풀이실패 유형 : X
    - 풀기까지 걸린 시간 : 30분
- 사용한 알고리즘 : 형변환
- 접근부터 풀이까지의 과정
    1. 배열로 변경 후 min, max 함수를 이용해서 풀이하면 되겠다고 생각
- 성공 코드
    
    ```swift
    func solution(_ s:String) -> String {
        let stringArray = s.split(separator: " ")
        let intArray = stringArray.compactMap { Int($0) }
        let min = intArray.min()!
        let max = intArray.max()!
        return "\(min) \(max)"
    }
    ```
    

> 3번 문제 : [기능개발](https://school.programmers.co.kr/learn/courses/30/lessons/42586)
> 
- 풀이실패 유형 : X
- 풀기까지 걸린 시간 : 2시간
- 사용한 알고리즘 : 큐 자료구조
- 접근부터 풀이까지의 과정
    1. fifo 형태의 자료구조를 사용하면 되겠다고 생각
    2. queue가empty 할 때까지 반복하며 요소 삭제, progress와 speed 병합
- 성공 코드

```swift
import Foundation

func solution(_ progresses:[Int], _ speeds:[Int]) -> [Int] {
    var prog = progresses
    var speed = speeds
    var answer = [Int]()
    
    while !(prog.isEmpty) {
        let a = prog.prefix { $0 >= 100 }.count
        
        if a > 0 {
            prog.removeFirst(a)
            speed.removeFirst(a)
            answer.append(a)
        }
        
        prog = zip(prog, speed).map { $0 + $1 }
    }
    return answer
}
```

> 4번 문제 : [](https://school.programmers.co.kr/learn/courses/30/lessons/120869)[영어 끝말잇기](https://school.programmers.co.kr/learn/courses/30/lessons/12981)
> 
- 풀이실패 유형 : x
- 풀기까지 걸린 시간 : 2시간
- 사용한 알고리즘 : 완전탐색
- 접근부터 풀이까지의 과정
1. 중복 value가 있을 경우 인덱스를 받아 answer에 삽입후 리턴하는 함수 작성
2. 실패 이후 뒷글자와 첫글자가 같아야 한다는 추가조건 찾음
3. 케이스 추가 이후 성공
- 성공 코드

```swift
import Foundation

func solution(_ n:Int, _ words:[String]) -> [Int] {
    var uniqueArray = [String]()
    var answer = [0, 0]
    var tmp = words.first?.last
    for (idx, i) in words.enumerated() {
    if uniqueArray.contains(i) {
        answer[0] = idx % n + 1
        answer[1] = idx / n + 1 
        return answer
    }
    else if idx != 0 && tmp != i.first {
        answer[0] = idx % n + 1
        answer[1] = idx / n + 1 
        return answer
    }
    else {
        tmp = i.last
        uniqueArray.append(i)
    }
}
    return answer
}
```