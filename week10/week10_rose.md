# 10주차

### 1번 문제 :  [암호해독](https://school.programmers.co.kr/learn/courses/30/lessons/120892)

- 풀이실패 유형 : X
- 풀기까지 걸린 시간 : 30분
- 어떤 알고리즘을 사용했는지 : 탐색
- 접근부터 풀이까지의 과정
    1. cipher를 배열로 만들고 빈 배열인 result 생성
    2. 코드 값의 -1 위치부터 cipher 길이까지 code값 간격으로 위치해있는 배열의 요소를 result에 추가
- 성공 코드
    
    ```swift
    import Foundation
    
    func solution(_ cipher:String, _ code:Int) -> String {
        var array = Array(cipher)
        var result = ""
    
        for i in stride(from: code - 1, to: cipher.count, by: code) {
            result.append(array[i])
        }
    
        return result
    }
    
    ```
    

### 2번 문제 : [옹알이1](https://school.programmers.co.kr/learn/courses/30/lessons/120956)

- 풀이실패 유형 : X
- 풀기까지 걸린 시간 : 약 4시간…
- 어떤 알고리즘을 사용했는지 : 탐색
- 접근부터 풀이까지의 과정
    1. 빈 배열 arr과 카운트 세는 count 생성
    2. bab에 원본 데이터 i를 넣고 변경된 데이터 저장
    3. 해당하는 문자가 있으면 각 숫자로 치환하여 빈 배열에 추가
    4. bab이 숫자로만 이루어져있을 때 1 증가시킴
- 성공 코드
    
    ```swift
    import Foundation
    
    func solution(_ babbling:[String]) -> Int {
        var arr = ""
        var count = 0
    
        for i in babbling{
            var bab = i
            bab = bab.replacingOccurrences(of: "aya", with: "1")
                        .replacingOccurrences(of: "ye", with: "2")
                        .replacingOccurrences(of: "woo", with: "3")
                        .replacingOccurrences(of: "ma", with: "4")
            arr.append(bab)
    
            if bab.range(of: "^[0-9]*$", options: .regularExpression) != nil {
                count += 1
            }
        }
    
        return count
    }
    
    ```
    

### 3번 문제 : [둘만의 암호](https://school.programmers.co.kr/learn/courses/30/lessons/155652)

- 풀이실패 유형 : X
- 풀기까지 걸린 시간 : 약 60분
- 어떤 알고리즘을 사용했는지 : 탐색
- 접근부터 풀이까지의 과정
    1. 알파벳 배열에서 skip 문자를 필터링한 arr 생성 및 빈 배열인 result 생성
    2. s 배열에서 char 문자 찾는 반복문
    3. arr에서 char와 일치하는 첫번째 문자의 위치 찾는 charIndex
    4. charIndex와 index 값을 더해 위치 찾는 targetIndex
    5. arr에서 targetIndex 위치의 문자 찾아서 result에 저장
- 성공 코드
    
    ```swift
    import Foundation
    
    func solution(_ s:String, _ skip:String, _ index:Int) -> String {
        let arr = "abcdefghijklmnopqrstuvwxyz".map { String($0) }.filter { !skip.contains($0) }
        var result = ""
        
        for char in s {
            if let charIndex = arr.firstIndex(of: String(char)) {
                let targetIndex = (charIndex + index) % arr.count
                let element = arr[targetIndex]
                result.append(element)
            }
        }
    
        return result
    }
    ```

---

## 모바일 세션

### 1번 문제 :  [원하는 문자열 찾기](https://school.programmers.co.kr/learn/courses/30/lessons/181878)

- 성공 코드
    
    ```swift
    import Foundation
    
    func solution(_ myString:String, _ pat:String) -> Int {
        let myString = myString.lowercased()
        let pat = pat.lowercased()
    
        if myString.contains(pat) {
            return 1
        } else { return 0 }
    }
    ```
    

### 2번 문제 : [ad 제거하기](https://school.programmers.co.kr/learn/courses/30/lessons/181870)

- 성공 코드
    
    ```swift
    import Foundation
    
    func solution(_ strArr:[String]) -> [String] {
        return strArr.filter { !$0.contains("ad") }
    }
    ```
