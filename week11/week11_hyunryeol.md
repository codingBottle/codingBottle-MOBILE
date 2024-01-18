# week11 Algorithm _ HyunRyeol

### 1번 문제 : [중복된 문자 제거](https://school.programmers.co.kr/learn/courses/30/lessons/120888) 
- 풀이실패 유형 : x
- 풀기까지 걸린 시간 : 5분
- 어떤 알고리즘을 사용했는지 : contain
- 접근부터 풀이까지의 과정
    1. 빈 문바열 result 생성
    2. mystring에 index 번째 값을 순환 하는 반복문 작성
    3. index 번째 값이 result 안에 들어 있는지 contains로 탐색 없다면 result에 추가
- 성공 코드
    ```swift
    import Foundation

    func solution(_ my_string:String) -> String {
        var result = ""
        for index in my_string{
            if !result.contains(index){
                result += String(index)
            }
        }
        return result
    }
    ```
<br>

### 2번 문제 : [주사위 게임 3](https://school.programmers.co.kr/learn/courses/30/lessons/181916)
- 풀이실패 유형 : x
- 풀기까지 걸린 시간 : 15분
- 어떤 알고리즘을 사용했는지 : sort
- 접근부터 풀이까지의 과정
    1. dice에 들어온값을 sort로 정렬
    2. n 번째 값과 비교하는 if 문 작성 (switch 잘쓰면 더 좋을 것 같다)
    3. 각 계산식으로 result추론
- 성공 코드
    ```swift
    import Foundation

    func solution(_ a:Int, _ b:Int, _ c:Int, _ d:Int) -> Int {
        var dice = [a, b, c, d]
        dice.sort()

        var result = 0

        if dice[0] == dice[3] {
            result = 1111 * dice[3]
        } else if dice[0] == dice[2] || dice[1] == dice[3] {
            result = Int(pow(Double(10 * dice[1] + (dice[0] + dice[3] - dice[1])), 2.0))
        } else if dice[0] == dice[1] && dice[2] == dice[3] {
            result = (dice[0] + dice[3]) * abs(dice[3] - dice[0])
        } else if dice[0] == dice[1] {
            result = dice[2] * dice[3]
        } else if dice[1] == dice[2] {
            result = dice[0] * dice[3]
        } else if dice[2] == dice[3] {
            result = dice[0] * dice[1]
        } else {
            result = dice[0]
        }

        return result
    }
    ```
<br>

### 3번 문제 : [서울에서 김서방 찾기](https://school.programmers.co.kr/learn/courses/30/lessons/12919) 
- 풀이실패 유형 : x
- 풀기까지 걸린 시간 : 5분
- 어떤 알고리즘을 사용했는지 : index
- 접근부터 풀이까지의 과정
    1. firstIndex 로 kim index 위치 찾고 문자열에 담아 return
- 성공 코드
    ```swift
    func solution(_ seoul:[String]) -> String {
    return "김서방은 \(seoul.firstIndex(of: "Kim")!)에 있다"
    }   
    ```
<br>

----

### 1번 문제 : [외계어 사전](https://school.programmers.co.kr/learn/courses/30/lessons/120869)
- 풀이실패 유형 : x
- 성공 코드
    ```swift
    import Foundation

    func solution(_ spell: [String], _ dic: [String]) -> Int {   
        return dic.map { String($0.sorted()) }.contains(spell.sorted().joined()) ? 1 : 2 
    }
    ```
<br>