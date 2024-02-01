# week12 Algorithm \_ 박신영

### 1번 문제 : [더 크게 합치기](hhttps://school.programmers.co.kr/learn/courses/30/lessons/181939)

- 풀이실패 유형 : X
- 풀기까지 걸린 시간 : 5분 내외
- 어떤 알고리즘을 사용했는지 : 삼항연산자
- 접근부터 풀이까지의 과정
  1. a+b, b+a 순서의 숫자 상수 2개 ab, ba 선언한다.
  2. 삼항연산자 사용하여 더 큰 것이 출력되도록 구현한다.
- 내 코드

  ```swift
  import Foundation

  func solution(_ a:Int, _ b:Int) -> Int {
      var ab = String(a)+String(b)
      var ba = String(b)+String(a)
      return ab > ba ? Int(ab)! : Int(ba)!
  }
  ```

<br>

---

### 2번 문제 : [피자 나눠 먹기 (2)](https://school.programmers.co.kr/learn/courses/30/lessons/120815)

- 풀이실패 유형 : X
- 풀기까지 걸린 시간 : 30분 내외
- 어떤 알고리즘을 사용했는지 : 최대공약수, 최소공배수
- 접근부터 풀이까지의 과정
  1. 최대공약수 함수를 활용하여 최소공배수 return a를 구한다.
  2. 그렇게 구한 return a를 분모에 두고, 분자는 n \* 6을 진행하여 최소공배수를 구한다.
  3. 이후 최소공배수 / 6 이 return 되도록 구현한다.
- 내 코드

  ```swift
  import Foundation

  // 최대공약수를 구하는 함수
  func gcd(_ a: Int, _ b: Int) -> Int {
      if b == 0 {
          return a
      } else {
          return gcd(b, a % b)
      }
  }

  // 최소공배수를 구하는 함수
  func lcm(_ a: Int, _ b: Int) -> Int {
      return a * b / gcd(a, b)
  }

  func solution(_ n:Int) -> Int {
      return lcm(n, 6) / 6
  }
  ```

<br>

---

### 3번 문제 : [문자열 여러 번 뒤집기](https://school.programmers.co.kr/learn/courses/30/lessons/181913)

- 풀이실패 유형 : X
- 풀기까지 걸린 시간 : 25분 내외
- 어떤 알고리즘을 사용했는지 : 반복문 및 Array의 replaceSubrange 함수 사용
- 접근부터 풀이까지의 과정
  1. my_string 값을 변수로 사용하기 위해 따로 선언.
  2. queries를 반복문 사용하여 돌려 뒤집어야 하는 인덱스 값들 a, b에 나눠 선언
  3. reverseArray 함수를 만들어 입력 받은 인덱스 범위에 해당된 문자열 파트를 뒤집어 반환한다.
     - 여기서 사용한 replaceSubrange 함수는 첫 번째 파라미터에 범위를 넣고, 두 번째 파라미터에는 해당 범위를 대체할 배열을 추가함으로써 범위에 해당되는 부분의 값을 바꿀 수 있다.
  4. 반복문을 돌린 queries가 전부 실행되면 끝으로 my_string 반환
- 내 코드

  ```swift
  func solution(_ my_string:String, _ queries:[[Int]]) -> String {
    var my_string = my_string
    for i in queries {
        let (a, b) = (i[0], i[1])
        my_string = reverseArray(string: my_string, from: a, to: b)
    }

    return my_string
  }

  //reverse 적용한 값 반환하는 함수
  func reverseArray(string: String, from: Int, to: Int) -> String {
    var result = string.map({String($0)})
    let arr = result[from...to].reversed()
    result.replaceSubrange(from...to, with: arr)
    return String(result.joined(separator: ""))
  }
  ```

<br>

---

### 4번 문제 : [배열 조각하기](https://school.programmers.co.kr/learn/courses/30/lessons/181893)

- 풀이실패 유형 : X
- 풀기까지 걸린 시간 : 10분 내외
- 어떤 알고리즘을 사용했는지 : Array slicing
- 접근부터 풀이까지의 과정
  1. 입력받는 상수 arr을 변수로 변환 및 cnt 변수 선언
  2. query 반복문 돌리며 cnt += 1을 통해 해당 순번이 홀수인지 짝수인지 분간할 수 있도록 하고, query의 원소들의 값을 이용하여 arr의 값을 조건에 맞게 재구성
  3. 반복문이 끝난다면 arr 반환
- 내 코드

  ```swift
  import Foundation

  func solution(_ arr:[Int], _ query:[Int]) -> [Int] {
    var arr = arr
    var cnt = 0
    for i in query {
        if cnt % 2 == 0 {
            arr = Array(arr[0...i])
        } else {
            arr = Array(arr[i...(arr.count-1)])
        }
        cnt += 1
    }

    return arr
  }
  ```

  <br>

---

### 2024.02.01 \_ 모바일 세션 중 풀이한 것

### 1번 문제 : [체육복](https://school.programmers.co.kr/learn/courses/30/lessons/42862#)

- 내 코드

  ```swift
  import Foundation

  func solution(_ n:Int, _ lost:[Int], _ reserve:[Int]) -> Int {
    var arr = Array(repeating: 1, count: n+1) // index는 0부터 시작이기에 +1

    lost.forEach { arr[$0] = 0 }
    reserve.forEach { arr[$0] += 1 }

    for i in 0..<arr.count {
        if arr[i] == 2 {
            if i > 0 && arr[i-1] == 0 {
                arr[i] -= 1
                arr[i-1] += 1
            } else if i < arr.count - 1 && arr[i+1] == 0 {
                arr[i] -= 1
                arr[i+1] += 1
            }
        }
    }

    return arr.filter{($0 >= 1)}.count-1
  }
  ```

- 추가 풀이코드

  ```swift
  import Foundation

  func solution(_ n: Int, _ lost: [Int], _ reserve: [Int]) -> Int {
    var lostArr = Set(lost).subtracting(reserve).sorted()
    var reserveArr = Set(reserve).subtracting(lost).sorted()

    for reserve in reserveArr {
        if lostArr.contains(reserve - 1) {
            lostArr = lostArr.filter{($0 != (reserve-1))}
            continue
        } else if lostArr.contains(reserve + 1) {
            lostArr = lostArr.filter{($0 != (reserve+1))}
            continue
        }
    }
    return (n - lostArr.count)
  }
  ```

  <br>
