# Week13 Algorithm Problems List _ Ryeol

### 1번 문제 : [영어가 싫어요](https://school.programmers.co.kr/learn/courses/30/lessons/120894) 
- 풀이실패 유형 : x
- 풀기까지 걸린 시간 : 10분
- 어떤 알고리즘을 사용했는지 : 탐색
- 접근부터 풀이까지의 과정
    1. key value 배열 생성
    2. numbers안에 있는 내용을 키밸류 값으로 변경해서 출력
- 성공 코드
    ```swift
    import Foundation

    func solution(_ numbers:String) -> Int64 {
        var numbersArray: [String: String] = ["zero": "0","one": "1","two": "2","three": "3","four": "4","five": "5","six": "6","seven": "7","eight": "8","nine": "9"]
        var result = numbers

        for i in numbersArray {
            result = result.replacingOccurrences(of: i.key, with: i.value)
        }

        return Int64(result)!
    }
    ```
    <br>
### 2번 문제 : [최댓값과 최솟값](https://school.programmers.co.kr/learn/courses/30/lessons/12939) 
- 풀이실패 유형 : x
- 풀기까지 걸린 시간 : 5분
- 어떤 알고리즘을 사용했는지 : 정렬
- 접근부터 풀이까지의 과정
    1. 공백으로 나눠서 정렬할 값을 array에 저장
    2. 저장된 값중 처음값과 마지막 값을 순서대로 출력
- 성공 코드
    ```swift
    func solution(_ s:String) -> String {
        var array = s.split(separator: " ").map{Int($0)!}.sorted(by: <)
        return "\(array.first!) \(array.last!)"
    }  
    ```
    <br>

### 3번 문제 : [기능 개발](https://school.programmers.co.kr/learn/courses/30/lessons/42586) 
- 풀이실패 유형 : x
- 풀기까지 걸린 시간 : 15분
- 어떤 알고리즘을 사용했는지 : 반올림
- 접근부터 풀이까지의 과정
    1. 배포 날짜랑 결과를 저장할 변수 생성
    2. 남은 작업량을 스피드로 나누고 반올림한 day에 저장
    3. 만약 릴리즈데이보다 계산된 데이가 높으면 늦게 작업한다고 판단 day최신화
- 성공 코드
    ```swift
    import Foundation

    func solution(_ progresses:[Int], _ speeds:[Int]) -> [Int] {
        var result: [Int] = []
        var releaseDay = 0
        for i in (0..<progresses.count) {
            let day = Int(ceil((Double(100 - progresses[i])) / Double(speeds[i])))
            if day > releaseDay {
                releaseDay = day
                result.append(1)
            } else {
                result[result.count-1] += 1
            }
        }
        
        return result
    }
    ```
    <br>
### 4번 문제 : [영어 끝말잇기](https://school.programmers.co.kr/learn/courses/30/lessons/12981)
- 풀이실패 유형 : 시간초과
- 풀기까지 걸린 시간 : 25분
- 어떤 알고리즘을 사용했는지 : 탐색
- 접근부터 풀이까지의 과정
    1. 'wordSet'이 비어있지 않고, 현재 단어가 'wordSet'에 이미 존재하거나 이전 단어의 마지막 글자와 현재 단어의 첫 글자가 다르다면, 규칙을 위반한 사람의 번호와 차례를 계산하여 반환
    2. 기존에 set이 아닌 그냥 배열로 풀었을때 시간 복잡도가 높아서 시간 초과 set으로 탐색과정 한번 줄이기
- 성공 코드
    ```swift
    import Foundation

    func solution(_ n:Int, _ words:[String]) -> [Int] {
        var wordSet = Set<String>()
        var lastChar = words[0].first!

        for (index, word) in words.enumerated() {
            if !wordSet.isEmpty && (wordSet.contains(word) || lastChar != word.first) {
                return [(index % n) + 1, index / n + 1]
            }
            wordSet.insert(word)
            lastChar = word.last!
        }
        return [0, 0]
    }

    ```
    <br>