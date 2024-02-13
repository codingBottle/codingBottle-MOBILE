# week13 Algorithm \_ 박신영

### 1번 문제 : [영어가 싫어요 ](https://school.programmers.co.kr/learn/courses/30/lessons/120894)

- 풀이실패 유형 : X
- 풀기까지 걸린 시간 : 15분 내외
- 어떤 알고리즘을 사용했는지 : 문자열 처리 및 딕셔너리 검색
- 접근부터 풀이까지의 과정
  1. keys 와 values 값 생성하고, [zip](<https://developer.apple.com/documentation/swift/zip(_:_:)>) 함수와 [uniqueKeysWithValues](<https://developer.apple.com/documentation/swift/dictionary/init(uniquekeyswithvalues:)/>) 메서드 사용하여 Dictionary로 변환.
  2. 이후 numbers for문 돌려, world에 한글자씩 더하고 진행중 단어가 dic의 key 값에 해당되면 value 값을 result에 더해준다.
  3. 이후 Int64로 형변환
- 내 코드

  ```swift
  import Foundation

  func solution(_ numbers:String) -> Int64 {
    let keys = ["zero", "one", "two", "three", "four", "five", "six", "seven","eight", "nine"]
    let values = [0, 1, 2, 3, 4, 5, 6, 7, 8, 9]
    let dic = Dictionary(uniqueKeysWithValues: zip(keys, values))

    var world = ""
    var result = ""

    for i in numbers {
        world += String(i)
        if dic.keys.contains(world) {
            guard let value = dic[world] else { continue }
            result += String(value)
            world = ""
        }
    }
    return Int64(result) ?? 0
  }
  ```

<br>

---

### 2번 문제 : [최댓값과 최솟값](https://school.programmers.co.kr/learn/courses/30/lessons/12939)

- 풀이실패 유형 : X
- 풀기까지 걸린 시간 : 10분 내외
- 어떤 알고리즘을 사용했는지 : 문자열 변환 및 min, max
- 접근부터 풀이까지의 과정
  1. split 메서드를 이용하여 공백으로 구분하여 원소를 추가고 고차함수 map 을 이용하여 Int로 변환
  2. min, max 메서드를 이용하여 최소, 최대 값들을 구해 반환
- 내 코드

  ```swift
  import Foundation

  func solution(_ s:String) -> String {
    let t = s.split(separator: " ").map {Int($0)!}
    return "\(t.min()!) \(t.max()!)"
  }
  ```

<br>

---

### 3번 문제 : [기능개발](https://school.programmers.co.kr/learn/courses/30/lessons/42586)

- 풀이실패 여부 및 유형 : 실패/4번
- 풀기까지 걸린 시간 : 40 내외
- 어떤 알고리즘을 사용했는지 :
- 접근부터 풀이까지의 과정
  1. 런타임에러 발생
- 내 코드

  ```swift
  import Foundation

  func solution(_ progresses:[Int], _ speeds:[Int]) -> [Int] {
    let arr = progresses.map({100 - $0})
    var newArr = zip(arr, speeds).map {
        let div = $0.0 / $0.1
        return ($0.0 % $0.1) > 0 ? Int(div) + 1 : Int(div)
    }
    var result = [Int]()

    while newArr.count != 0 {
        for i in 0..<newArr.count {
            if newArr[i] == 0 {
                continue
            } else {
                newArr[i] -= 1
            }
        }
        if newArr[0] == 0 {
            var cnt = 0
            while cnt < newArr.count && newArr[cnt] == 0 {
                cnt += 1
            }
            result.append(cnt)
            newArr.removeFirst(cnt)
        }
    }
    return result
  }
  ```

<br>

---

### 4번 문제 : [영어 끝말잇기](https://school.programmers.co.kr/learn/courses/30/lessons/12981)

- 풀이실패 여부 및 유형 : 실패/3번
- 풀기까지 걸린 시간 : 60분 초과
- 어떤 알고리즘을 사용했는지 :
- 접근부터 풀이까지의 과정
  1. 제대로 접근하지 못하였던 것 같다.
- 내 코드

  ```swift
  import Foundation

  func solution(_ n:Int, _ words:[String]) -> [Int] {
    var words = words
    // 반복된 단어로 탈락하지 않은 경우
    if words.count == Set(words).count {
        var num = 0
        var flag = true
        var failCount = 0
        for i in 1..<words.count {
            num += 1
            if num >= n { num = 1 }
            let word1 = words[i-1].map({String($0)})
            let word2 = words[i].map({String($0)})
            // 끝말잇기 실패
            if word1.last != word2.first {
                flag.toggle()
                print(num)
                failCount = i + 1
                break
            }
        }
        let wrongPersonNum = failCount%n
        var a = failCount / n
        var b = 0
        if wrongPersonNum > 0 {
            b += 1
        }
        var cnt = 0

        if failCount/n >= wrongPersonNum {
            cnt += 1
        }
        return flag == true ? [0, 0] : [wrongPersonNum, a+b]
    }

    // 반복된 단어로 탈락한 경우
    else {
        var num = 0
        var arr = [words[0]]
        var failCount = 0
        for i in 1..<words.count {
            num += 1
            if num / n != 0 {
                num = 0
            }
            if arr.contains(words[i]) {
                failCount = i + 1
                break
            } else {
                arr.append(words[i])
            }
        }
        let wrongPersonNum = failCount%n
        var cnt = 0

        if failCount/n >= wrongPersonNum {
            cnt += 1
        }
        return [wrongPersonNum, words.count/n]
    }
  }
  ```

  <br>
