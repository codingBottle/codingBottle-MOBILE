# week11 Algorithm \_ 박신영

### 1번 문제 : [중복된 문자 제거](https://school.programmers.co.kr/learn/courses/30/lessons/120888)

- 풀이실패 유형 : X
- 풀기까지 걸린 시간 : 5분 내외
- 어떤 알고리즘을 사용했는지 : 완전탐색
- 접근부터 풀이까지의 과정
  1. Character 타입 빈 배열 result 생성.
  2. 이후 my_string 반복문 돌려 result에 있는 글자인지 확인하여 없는 글자라면 추가.
  3. 종료.
- 내 코드

  ```swift
  import Foundation

  func solution(_ my_string:String) -> String {
      var result = [Character]()

      for i in my_string {
          if !result.contains(i) {
              result.append(i)
          }
      }
      return String(result)
    }
  ```

- 고차함수 활용 풀이 코드

  ```swift
  import Foundation

  func solution(_ my_string:String) -> String {
    var set = Set<Character>()
    return my_string.filter{set.insert($0).inserted}
  }
  ```

  - String 타입인 my_string에 filter 함수를 사용하였으니 반환값은 string이며, 빈 Set에 insert($0) 을 사용하여 추가를 시도하고, Set에 없는 글자라서 추가가 됐다면 inserted 를 통해 true가 반환되니 해당 글자는 true로 인식되어 필터링 된 my_string에 추가된다.
  - 이후 필터링을 마친 my_string을 리턴.
  - 종료.

<br>

---

### 2번 문제 : [주사위 게임 3](https://school.programmers.co.kr/learn/courses/30/lessons/181916)

- 풀이실패 유형 : X
- 풀기까지 걸린 시간 : 20분 내외
- 어떤 알고리즘을 사용했는지 : 조건문
- 접근부터 풀이까지의 과정

  1. 아래 5개의 case에 맞게 5개 상황으로 나눠 case 1, case 5는 Set() 사용하여 처리.
  2. 2개, 2개 같은 값이 나온 case 3는 count 변수를 사용하여 같은 주사위 값을 지닌 두 값을 n 배열에 추가하여 case에 맞는 공식을 활용하여 점수 리턴.
  3. 3개의 주사위 값이 같은 case 2는 n1, n2 변수를 활용하여 count가 3인 값은 n1에 남은 값은 n2에 넣어 case에 맞는 공식을 활용하여 점수 리턴.
  4. 2개의 주사위가 같고, 남은 2개의 주사위는 서로 다른 값인 case 4는 count를 활용하여 count 값이 2가 아니면서, arr[i]의 값이 0이 아닌 값을 n 배열에 추가하고 이후 case에 맞는 공식을 활용하여 점수 리턴.
  5. 종료

  - 각 case 별 점수
    1. 네 주사위에서 나온 숫자가 모두 p로 같다면 1111 × p점을 얻습니다.
    2. 세 주사위에서 나온 숫자가 p로 같고 나머지 다른 주사위에서 나온 숫자가 q(p ≠ q)라면 (10 × p + q)2 점을 얻습니다.
    3. 주사위가 두 개씩 같은 값이 나오고, 나온 숫자를 각각 p, q(p ≠ q)라고 한다면 (p + q) × |p - q|점을 얻습니다.
    4. 어느 두 주사위에서 나온 숫자가 p로 같고 나머지 두 주사위에서 나온 숫자가 각각 p와 다른 q, r(q ≠ r)이라면 q × r점을 얻습니다.
    5. 네 주사위에 적힌 숫자가 모두 다르다면 나온 숫자 중 가장 작은 숫자 만큼의 점수를 얻습니다.

- 내 코드

  ```swift
    import Foundation

    func solution(_ a:Int, _ b:Int, _ c:Int, _ d:Int) -> Int {
        let s = [a, b, c, d]
        var arr = Array(repeating: 0, count: 7)

        //case 1
        if Set(s).count == 1 {
            return a*1111
        }
        //case 5
        else if Set(s).count == 4 {
            return s.min()!
        }

        for i in s {arr[i] += 1}
        var count = 0
        for i in arr {
            if i >= 2 {
                if i == 3 {
                    count += 3
                } else {
                    count += 1
                }
            }
        }

        var n = [Int]()
        var n1 = 0
        var n2 = 0

        //case 3
        if count == 2 {

            for i in 0..<arr.count {
                if arr[i] != 0 {
                    n.append(i)
                }
            }
            return (n[0]+n[1]) * (abs(n[0]-n[1]))
        }

        //case 2
        else if count % 3 == 0 {

            for i in 0..<arr.count {
                if arr[i] == 3 {
                    n1 = i
                }
                if arr[i] == 1{
                    n2 = i
                }
            }
            return Int(pow((10.0*Double(n1)+Double(n2)), 2))
        }

        //case 4
        else {
            for i in 0..<arr.count {
                if arr[i] > 0 && arr[i] != 2 {
                    n.append(i)
                }
            }
            return n[0]*n[1]
        }
    }
  ```

<br>

---

### 3번 문제 : [서울에서 김서방 찾기](https://school.programmers.co.kr/learn/courses/30/lessons/12919)

- 풀이실패 유형 : X
- 풀기까지 걸린 시간 : 5분 내외
- 어떤 알고리즘을 사용했는지 : 배열의 firstIndex(of:) 메서드 사용
- 접근부터 풀이까지의 과정

  1. "Kim"이 등장하는 firstIndex 값을 반환하고 이를 문자열 보간 법을 활용하여 return 에 넣는다.
  2. 종료

- 내 코드

  ```swift
  func solution(_ seoul:[String]) -> String {
    return "김서방은 \(seoul.firstIndex(of: "Kim") ?? 0)에 있다"
  }
  ```

  <br>

---

### 2024.01.18 \_ 모바일 세션 중 풀이한 것

### 1번 문제 : [외계어 사전](https://school.programmers.co.kr/learn/courses/30/lessons/120869)

- 내 코드

  ```swift
  import Foundation
  func solution(_ spell: [String], _ dic: [String]) -> Int {
    let spellSet = Set(spell.joined())
    for word in dic {
        if spellSet == Set(word) {
            return 1
        }
    }
    return 2
  }
  ```

  - Set은 순서를 고려하지 않는 데이터 구조이기에, 원소의 순서나 중복 여부는 고려하지 않고, 원소의 존재 여부만을 판단하기에 위와 같은 풀이가 가능하다.

  <br>
