# 12주차

### 1번 문제 :  [더 크게 합치기](https://school.programmers.co.kr/learn/courses/30/lessons/181939)

- 풀이실패 유형 : X
- 풀기까지 걸린 시간 : 10분
- 어떤 알고리즘을 사용했는지 : 탐색
- 접근부터 풀이까지의 과정
    1. 문자열로 바꿈
    2. 문자열끼리 더한 값을 숫자로 변환
    3. 두 수 중 큰 값 리턴
- 성공 코드
    
    ```swift
    import Foundation
    
    func solution(_ a:Int, _ b:Int) -> Int {
        let a = String(a)
        let b = String(b)
        
        let ab = Int(a + b)!
        let ba = Int(b + a)!
        
        return max(ab, ba)
    }
    ```
    

### 2번 문제 :  [피자 나눠 먹기(2)](https://school.programmers.co.kr/learn/courses/30/lessons/120815)

- 풀이실패 유형 :
- 풀기까지 걸린 시간 : 약 40분
- 어떤 알고리즘을 사용했는지 : 탐색
- 접근부터 풀이까지의 과정
    1. pizza를 1판을 여섯조각으로 나눈 나머지가 0이 될 때까지 pizza 한판 값 증가
- 성공 코드
    
    ```swift
    import Foundation
    
    func solution(_ n:Int) -> Int {
        var pizza = 1
        
        while (pizza * 6) % n != 0 {
            pizza += 1
        }
        
        return pizza
    }
    ```
    

### 3번 문제 :  [문자열 여러 번 뒤집기](https://school.programmers.co.kr/learn/courses/30/lessons/181913)

- 풀이실패 유형 : X
- 풀기까지 걸린 시간 : 약 1시간
- 어떤 알고리즘을 사용했는지 : 탐색
- 접근부터 풀이까지의 과정
    1. queries의 0번째 인덱스와 1번째 인덱스를 처음과 끝으로 지정
    2. 1씩 증가와 감소하며 문자 위치 바꿈
- 성공 코드
    
    ```swift
    import Foundation
    
    func solution(_ my_string:String, _ queries:[[Int]]) -> String {
        var arr = Array(my_string)
        
        for query in queries {
            var start = query[0]
            var end = query[1]
            
            while start < end {
                arr.swapAt(start, end)
                start += 1
                end -= 1
            }
        }
        
        return String(arr)
    }
    ```
    

### 4번 문제 :  [배열 조각하기](https://school.programmers.co.kr/learn/courses/30/lessons/181893)

- 풀이실패 유형 : X
- 풀기까지 걸린 시간 : 약 50분
- 어떤 알고리즘을 사용했는지 : 탐색
- 접근부터 풀이까지의 과정
    1. 짝수인 query는 현재 query 인덱스 + 1부터 배열의 마지막까지 삭제
    2. 홀수인 query는 배열의 시작부터 query 인덱스 - 1까지 삭제
- 성공 코드
    
    ```swift
    import Foundation
    
    func solution(_ arr:[Int], _ query:[Int]) -> [Int] {
        var arr = Array(arr)
        
        for i in 0..<query.count {
            if (i % 2 == 0) {
                arr.removeSubrange(query[i] + 1..<arr.endIndex)
            } else {
                arr.removeSubrange(arr.startIndex..<query[i])
    
            }
        }
        return arr
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