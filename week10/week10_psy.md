# week10 Algorithm \_ OOO

### 1번 문제 : [암호해독](https://school.programmers.co.kr/learn/courses/30/lessons/120892)

- 풀이실패 유형 : X (풀이성공시 X, 실패시에는 해당 유형 번호 작성하고 기입된 내용에 맞게 자신이 어디까지 진행하였는지 작성.)
- 풀기까지 걸린 시간 : 10분 내외
- 어떤 알고리즘을 사용했는지 : 완전탐색
- 접근부터 풀이까지의 과정
  1. 빈 문자열 생성 이후 cnt = 1 역시 생성
  2. 이후 cipher 반복문 돌려 code로 나눈 나머지 값이 0인 문자를 문자열에 담고 리턴. 종료
- 내 코드
  ```swift
  import Foundation

  func solution(_ cipher:String, _ code:Int) -> String {
  var result = ""
  var cnt = 1
  for i in cipher {
      if cnt % code == 0 {
          result+=String(i)
      }
      cnt += 1
  }
  return result
  }
  ```
- 참고하면 좋을 코드
      ```swift
      func solution(_ cipher: String, _ code: Int) -> String {
      (0..<cipher.count).filter { $0 % code == code - 1 }.map {
          String(Array(cipher)[$0])
      }.joined(separator: "")
      }
      ```
  <br>

---

### 2번 문제 : [옹알이](https://school.programmers.co.kr/learn/courses/30/lessons/120956)

- 풀이실패 유형 : X
- 풀기까지 걸린 시간 : 40분 내외
- 어떤 알고리즘을 사용했는지 : 완전탐색
- 접근부터 풀이까지의 과정
  1. todo 배열로 발음 할 수 있는 것들로 채운다.
  2. babbling 배열의 한 단어씩 빈 word에 넣으며 단어를 만들고, 만들어진 단어가 todo 배열에 들어있다면 발음할 수 있는 것이기에 word를 초기화하고 cnt를 1씩 더한다.
  3. babbling의 한 단어가 반복문이 종료 됐을 때 word가 빈문자열이고 cnt가 0보다 크다면 result에 1을 더한다.
  4. 이후 return result 종료.
- 내 코드

      ```swift
      import Foundation

      func solution(_ babbling:[String]) -> Int {
          var result = 0
          var word = ""
          var cnt = 0

          let todo = ["aya", "ye", "woo", "ma"]

          for i in babbling {
              for j in i {
                  word += String(j)
                  if todo.contains(word) {
                      word = ""
                      cnt += 1
                  }
              }
              if word.count == 0 && cnt > 0 {
                  result += 1
              }
              word = ""
          }
          return result
      }
      ```

  <br>

---

### 3번 문제 : [둘만의 암호](https://school.programmers.co.kr/learn/courses/30/lessons/155652)

- 풀이실패 유형 : X
- 풀기까지 걸린 시간 : 60분 +@
- 어떤 알고리즘을 사용했는지 : 완전탐색
- 접근부터 풀이까지의 과정
  1. skip에 해당되지 않는 알파벳만을 담은 alphabets 배열 생성
  2. 주어진 s를 고차함수 map을 이용하여 첫 글자가 예시와 같이 'a' 일 때 현재 알파벳에서 a 가 위치한 인덱스 값은 beforeIndex에 대입.
  3. 이후 beforeIndex와 예시와 같이 5개 이후 글자로 대체해야하는 주어진 'index'를 더하고 skip에 포함되지 않는 알파벳으로 구성된 alphabets 배열의 원소 개수로 나눈 값을 afterIndex에 넣는다. <br>
  - 이와 같이 사용하면 index를 더한 글자가 z를 넘어설 때도 대처할 수 있는 순환성을 보장한다.
  4. 이후 result에 담긴 글자들을 joined 함수를 이용하여 문자열로 합쳐 리턴. 종료
- 내 코드

      ```swift
      import Foundation

      func solution(_ s:String, _ skip:String, _ index:Int) -> String {
      let alphabets = Array("abcdefghijklmnopqrstuvwxyz").filter { !skip.contains($0)}

      let result = s.map { (char: Character) -> String in
          let beforeIndex = alphabets.firstIndex(of: char)!
          let afterIndex = (beforeIndex + index) % alphabets.count
          return String(alphabets[afterIndex])
      }

      return result.joined(separator: "")
      }
      ```

  <br>
