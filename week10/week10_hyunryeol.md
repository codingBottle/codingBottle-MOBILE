# week10 Algorithm _ HyunRyeol

### 1번 문제 : [암호해독](https://school.programmers.co.kr/learn/courses/30/lessons/120892)
- 풀이실패 유형 : x
- 풀기까지 걸린 시간 : 20분
- 어떤 알고리즘을 사용했는지 : enumerated, stride
- 접근부터 풀이까지의 과정
    1. 빈 문자열 생성 이후 cnt = 1 역시 생성
    2. 이후 cipher 반복문 돌려 code로 나눈 나머지 값이 0인 문자를 문자열에 담고 리턴
    3. 이후 hashmap이나 code의 배수만큼 건너뛰는게 가능하지 않을까? 생각하여 로직 변경
- 성공 코드
    1차 로직
    ```swift
    import Foundation
    func solution(_ cipher:String, _ code:Int) -> String {
        var result = ""
        var count = 1
        for i in cipher{
            if count % code == 0{
                result += String(i)
            }
            count += 1
        }
        return result
    }   
    ```
    2차 로직
    ```swift
    import Foundation
    func solution(_ cipher:String, _ code:Int) -> String {
        let result = cipher.enumerated()
        .filter { ($0.offset + 1) % code == 0 }
        .map { String($0.element) }
        .joined()
    return result
    }   
    ```
    ```swift
    import Foundation
    func solution(_ cipher:String, _ code:Int) -> String {
        let result = stride(from: code - 1, to: cipher.count, by: code)
       .map { index in
           let stringIndex = cipher.index(cipher.startIndex, offsetBy: index)
           return String(cipher[stringIndex])
       }
       .joined()
    return result
    }   
    ```
<br>

### 2번 문제 : [옹알이](https://school.programmers.co.kr/learn/courses/30/lessons/120956)
- 풀이실패 유형 : x
- 풀기까지 걸린 시간 : 20분
- 어떤 알고리즘을 사용했는지 : contains활용 문자열 탐색
- 접근부터 풀이까지의 과정
    1. 사용 가능한 옹알이 문자 배열 word생성, babbling 중 가능한 단어 count 변수 생성,겹치는 글씨 수 카운트 담당 변수 wordcount 생성
    2. babbling 안에서 반복하는 for문{word안에서 반복하는 반복문}이중 반복문 생성
    3. 반복문 안에서 옹알이 단어와 일치하는 단어가 문자열내에 있는지 contains활용해 탐색
    4. 있다면 있는 문자 수 만큼 카운트 증가
    5. wordcount가 i.count와 일치하면 가능한 단어로 보고 count 증가
- 성공 코드
    ```swift
    import Foundation

    func solution(_ babbling:[String]) -> Int {
        let word = ["aya", "ye", "woo", "ma"]
        var count = 0
        for i in babbling {
            var wordCount = 0
            for j in word { 
                if i.contains(j){
                    wordCount += j.count
                }
                if i.count == wordCount{
                    count += 1
                    break
                }
            }
        }
        return count
    }
    ```
<br>

### 3번 문제 : [둘만의암호](https://school.programmers.co.kr/learn/courses/30/lessons/155652)
- 풀이실패 유형 : x
- 풀기까지 걸린 시간 : 30분
- 어떤 알고리즘을 사용했는지 : contains활용 문자열 탐색
- 접근부터 풀이까지의 과정
    1. String s에 첫 글짜에서 index만큼이동하는동안 탐색되는 알파벳을 구하고 해당 알파벳중 String skip에 포함된 문자가 있으면 제외하고 인덱스 길이에 맞춰서 추가로 이동하는 로직을 생각
    2. 근데 기존 알파벳 배열에서 skip에 들어있는 알파벳을 미리 지워놓고 탐색한다면 쉬울 것 같다고 생각
- 성공 코드
    ```swift
    import Foundation

    func solution(_ s:String, _ skip:String, _ index:Int) -> String {
        var list = Array("abcdefghijklmnopqrstuvwxyz")
            
            for item in skip {
                if let idx = list.firstIndex(of: item) {
                    list.remove(at: idx)
                }
            }
            
            var result = ""
            
            for ch in s {
                if let idx = list.firstIndex(of: ch) {
                    let newIndex = (idx + index) % list.count
                    result.append(list[newIndex])
                }
            }
            
            return result

    }
    ```
<br>