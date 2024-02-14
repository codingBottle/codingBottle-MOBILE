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
- 재시도: 성공 / 30분내외
  - ceil 함수를 고려하지 못하고 스스로 구현하려던 것이 좋지 않았던 것 같다.
- 내 코드

  ```swift
  // 1차 실패 케이스
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


  // 2차 성공 케이스
  import Foundation

  func solution(_ progresses:[Int], _ speeds:[Int]) -> [Int] {
    var lastReleaseDate: Int = 0
    var numOfReleases: [Int] = []

    for i in 0..<progresses.count {
        let progress = Double(progresses[i])
        let speed = Double(speeds[i])
        let day = Int(ceil((100-progress) / speed)) // 각 진행률의 일자 계산

        if day > lastReleaseDate {

            // 다음 완료일(day)이 최근 배포일(lastReleaseDate)보다 오래걸리면, 배포갯수 추가
            lastReleaseDate = day
            numOfReleases.append(1)
        } else {

            // 다음 완료일이 최근 배포일보다 조금 걸리면, 앞 순서가 배포될 때 같이 배포되므로 +1
            numOfReleases[numOfReleases.count - 1] += 1
        }
    }
    return numOfReleases
  }
  ```

<br>

---

### 4번 문제 : [영어 끝말잇기](https://school.programmers.co.kr/learn/courses/30/lessons/12981)

- 풀이실패 여부 및 유형 : 실패/3번
- 풀기까지 걸린 시간 : 60분 초과
- 어떤 알고리즘을 사용했는지 :
- 접근부터 풀이까지의 과정
  1. 제대로 풀이를 시도하지 못하였던 것 같다.
- 재시도 : 타인 코드 참고
  - 풀이를 시작할 때에 모든 경우를 고려하지 못한 것이 문제였던 것 같다.
- 내 코드

  ```swift
  // 1차 실패 케이스
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


  // 참고한 코드
  import Foundation

  func solution(_ n:Int, _ words:[String]) -> [Int] {
    // 규칙
    // 1번부터 번호 순서대로 한 사람씩 차례대로 단어를 말합니다.
    // 마지막 사람이 단어를 말한 다음에는 다시 1번부터 시작합니다.
    // 앞사람이 말한 단어의 마지막 문자로 시작하는 단어를 말해야 합니다.
    // 이전에 등장했던 단어는 사용할 수 없습니다.
    // 한 글자인 단어는 인정되지 않습니다.

    var wordDB: [String] = []
    for i in 0..<words.count{
        var word = words[i]
        // 한 글자 이상인지
        if word.count < 1{
            print(word.count)
            return [i%n + 1, i/n + 1]
        }
        // 말했던 단어인지
        if wordDB.contains(words[i]){
            print(i)
            return [i%n + 1, i/n + 1]
        }
        // 마지막 문자와 첫 문자가 같은지
        if wordDB.count != 0{
            var lastWord = wordDB[wordDB.count - 1]
            if lastWord.removeLast() != word.removeFirst(){
                return [i%n + 1, i/n + 1]
            }
        }
        wordDB.append(words[i])

    }
    return [0,0]
  }
  ```

  <br>
