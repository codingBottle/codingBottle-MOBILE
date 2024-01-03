### 1번 문제 : [암호해독](https://school.programmers.co.kr/learn/courses/30/lessons/120892)
- 풀이실패 유형 : X
- 풀기까지 걸린 시간 : 30분
- 어떤 알고리즘을 사용했는지 : Stride 순회
- 접근부터 풀이까지의 과정
1. stride 함수를 사용해 code간격으로 순회할 수 있게 설정.
2. 반복문으로 문자열을 순회하고 현재 인덱스에 해당하는 문자를 선택하고 선택된 문자는 새로운 문자열 result에 추가하고 반환.

- 성공 코드
    
    ```swift
    import Foundation
    func solution(_ cipher: String, _ code: Int) -> String {
    var result = ""

    for i in stride(from: code - 1, to: cipher.count, by: code) {
        let index = cipher.index(cipher.startIndex, offsetBy: i)
        result.append(cipher[index])
        }

    return result
    }

    
    ```
    
### 2번 문제 : [옹알이(1)](https://school.programmers.co.kr/learn/courses/30/lessons/155652)
- 풀이실패 유형 : 2
어떻게 풀어야 할 지 아이디어는 생각했지만 어떤 유형의 알고리즘을 사용해야하는지 모르겠다.
- 풀기까지 걸린 시간 : 1시간 
- 어떤 알고리즘을 사용했는지 : 필터링
- 접근부터 풀이까지의 과정 
1. 가능한 발음들 배열 생성
2. 배열을 순회하면 필터링하고 저장
3. 개수 반환

테스트 1만 통과
입력값 〉	["aya", "yee", "u", "maa", "wyeoo"]
기댓값 〉	1
실행 결과 〉	테스트를 통과하였습니다.

- 성공 코드    
    ```swift
    import Foundation
    
    func solution(_ babbling: [String]) -> Int {
    let possibleWords = ["aya", "ye", "woo", "ma"]

    let filteredBabbling = babbling.filter { word in
        return possibleWords.contains { word.contains($0) && word.count == $0.count }
    }

    return filteredBabbling.count
    }
    ```

### 3번 문제 : [둘만의 암호](https://school.programmers.co.kr/learn/courses/30/lessons/155652)
- 풀이실패 유형 : X
- 풀기까지 걸린 시간 : 1시간
- 어떤 알고리즘을 사용했는지 : 문자열 처리, 배열 인덱싱
- 접근부터 풀이까지의 과정
1. a-z까지의 알파벳 배열 생성
2. 문자열 s의 문자들 순회
3. currentIndex를 1씩 증가시키며 index만큼 반복문 순회 
4. currentIndex가 가리키는 알파벳이 skip 문자열에 포함되는지 확인
5. 최종 알파벳 문자열 추가
6. 계속 반복
7. 최종 결과 반환

- 성공 코드
   ```swift
   import Foundation
   
   func solution(_ s:String, _ skip:String, _ index:Int) -> String {
    let alphabets = Array("abcdefghijklmnopqrstuvwxyz")
    let skipSet = Set(skip)
    var result = ""

    for char in s {
        var currentIndex = alphabets.firstIndex(of: char)!
        for _ in 0..<index {
            repeat {
                currentIndex = (currentIndex + 1) % 26
            } while skipSet.contains(alphabets[currentIndex])
        }
        result.append(alphabets[currentIndex])
    }

    return result
    }

   ```
