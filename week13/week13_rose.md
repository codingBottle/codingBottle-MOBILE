# 13주차

### 1번 문제 :  [영어가 싫어요](https://school.programmers.co.kr/learn/courses/30/lessons/120894)

- 풀이실패 유형 : X
- 풀기까지 걸린 시간 : 20분
- 어떤 알고리즘을 사용했는지 : 탐색
- 접근부터 풀이까지의 과정
    1. 영어로 된 숫자를 영어로 대체함
    2. 64비트 정수형으로 변환하여 반환
- 성공 코드
    
    ```swift
    import Foundation
    
    func solution(_ numbers:String) -> Int64 {
        var arr: [String: String] = ["zero": "0","one": "1","two": "2","three": "3","four": "4","five": "5","six": "6","seven": "7","eight": "8","nine": "9"]
        var answer = numbers
    
        for i in arr {
            answer = answer.replacingOccurrences(of: i.key, with: i.value)
        }
    
        return Int64(answer)!
    }
    ```
    

### 2번 문제 :  [최댓값과 최솟값](https://school.programmers.co.kr/learn/courses/30/lessons/12939)

- 풀이실패 유형 :
- 풀기까지 걸린 시간 : 20분
- 어떤 알고리즘을 사용했는지 : 탐색
- 접근부터 풀이까지의 과정
    1. 공백 간격으로 끊은 뒤, 정수로 변환한 numbers 선언
    2. 최댓값과 최솟값을 max와 min으로 선언
    3. max, min 출력
- 성공 코드
    
    ```swift
    import Foundation
    
    func solution(_ s:String) -> String {
        let numbers = s.split(separator: " ").map { Int(String($0))! }
        let max = numbers.max()!
        let min = numbers.min()!
        
        return "\(min) \(max)"
    }
    ```
    

### 3번 문제 :  [기능개발](https://school.programmers.co.kr/learn/courses/30/lessons/42586)

- 풀이실패 유형 : O, 문제 이해가 안됨..
- 풀기까지 걸린 시간 : 약 1시간
- 어떤 알고리즘을 사용했는지 : 탐색
- 접근부터 풀이까지의 과정
    1. 
- 성공 코드
    
    ```swift
    import Foundation
    
    func solution(_ progresses:[Int], _ speeds:[Int]) -> [Int] {
        var task = 0
        var day = 0
        
        for i in 0..<progresses.count {
            task = 100 - progresses[i]
    
        }
        
        return []
    }
    ```
    

### 4번 문제 :  [영어 끝말잇기](https://school.programmers.co.kr/learn/courses/30/lessons/12981)

- 풀이실패 유형 : X
- 풀기까지 걸린 시간 : 50분
- 어떤 알고리즘을 사용했는지 : 탐색
- 접근부터 풀이까지의 과정
    1. 단어 배열 선언
    2. 단어 길이가 1 미만일 때 & 전 단어의 마지막 글자와 현재 단어의 첫 글자가 다를 때 & 이미 있는 단어일 때
    3. 순서와 차례 반환
    4. 끝말잇기 성공하면 [0, 0]으로 반환
- 성공 코드
    
    ```swift
    import Foundation
    
    func solution(_ n:Int, _ words:[String]) -> [Int] {
        var arr: [String] = []
        
        for i in 0..<words.count {
            let word = words[i]
            
            if word.count < 1 || (i > 0 && arr[i - 1].last != word.first) || arr.contains(word) {
                return [i % n + 1, i / n + 1]
            }
            
            arr.append(words[i])  
        }
        
        return [0,0]
    }
    ```
    

---

## 모바일 세션

### 1번 문제 :

- 성공 코드
    
    ```swift
    
    ```
    

### 2번 문제 :

- 성공 코드
    
    ```swift
    
    ```