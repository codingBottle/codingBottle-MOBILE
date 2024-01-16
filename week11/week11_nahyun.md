# week11 Algorithm _ nahyun

### 1번 문제 : [중복된 문자 제거](https://school.programmers.co.kr/learn/courses/30/lessons/120888)
- 풀이실패 유형 : X (풀이성공시 X, 실패시에는 해당 유형 번호 작성하고 기입된 내용에 맞게 자신이 어디까지 진행하였는지 작성.)
- 풀기까지 걸린 시간 : 30분
- 어떤 알고리즘을 사용했는지 : 필터링
- 접근부터 풀이까지의 과정
1. 빈 문자열 result 생성
2. 입력받은 my_string의 문자가 result에 포함되어 있지 않으면 result에 추가해서 결과적으로 중복 없는 문자만 출력
- 성공 코드
 ```swift
    import Foundation
    
    func solution(_ my_string:String) -> String {
      var result = ""
      for char in my_string{
          if !result.contains(char) {
            result.append(char)
        }
    }
    return result
}
  ```

### 2번 문제 : [주사위 게임 3](https://school.programmers.co.kr/learn/courses/30/lessons/181916)
- 풀이실패 유형 : X
- 풀기까지 걸린 시간 : 2-3시간 
- 어떤 알고리즘을 사용했는지 : swith 조건 분기
- 접근부터 풀이까지의 과정 
1. 주사위의 숫자를 배열 dice에 저장. Set로 중복을 제거한 후, uniqueDice 상수에 저장
2. counts라는 딕셔너리를 선언. 주사위 숫자를 키, 그 숫자가 주사위에서 몇 번 나왔는지를 값으로 저장
3. dice 배열을 순회, 각 숫자가 몇 번 등장하는지 counts 딕셔너리에 저장
4. 중복을 제거한 주사위 숫자의 개수(uniqueDice.count)에 따라 점수를 계산
5. switch문에서 주사위의 숫자 종류의 개수(uniqueDice.count)를 기준으로 각각의 경우를 선택
- 성공 코드
    ```swift
    import Foundation
    func solution(_ a: Int, _ b: Int, _ c: Int, _ d: Int) -> Int {
      let dice = [a, b, c, d]
      let uniqueDice = Set(dice) //중복 제거 후 저장
      var counts = [Int: Int]() // 키: 주사위 숫자, 값: 주사위가 몇 번 나왔는지
    
    for num in dice {
        counts[num, default: 0] += 1
    }
    
    switch uniqueDice.count {
    case 1:
        // 모든 주사위의 숫자가 같을 때
        return 1111 * a
    case 2:
        // 두 개의 서로 다른 숫자가 나올 때
        if counts.values.contains(3) {
            // 세 개의 주사위의 숫자가 같은 경우
            let p = counts.first { $1 == 3 }!.key
            let q = counts.first { $1 == 1 }!.key
            return (10 * p + q) * (10 * p + q)
        } else {
            // 두 개씩 동일한 숫자가 나올 때
            let (p, q) = (counts.keys.min()!, counts.keys.max()!)
            return (p + q) * abs(p - q)
        }
    case 3:
        // 두 개의 주사위에서 나온 숫자가 같고 나머지 두 개의 주사위에서 나온 숫자가 다른 경우
        let p = counts.first { $1 == 2 }!.key
        let qr = dice.filter { $0 != p }
        return qr[0] * qr[1]
    default:
        // 모든 주사위의 숫자가 다를 때
        return dice.min()!
    }
}
    ```

### 3번 문제 : [서울에서 김서방 찾기](https://school.programmers.co.kr/learn/courses/30/lessons/12919)
- 풀이실패 유형 : X
- 풀기까지 걸린 시간 : 10분
- 어떤 알고리즘을 사용했는지 : 배열 인덱스 조회
- 접근부터 풀이까지의 과정
1. firstIndex를 사용하여 배열에서 Kim이 첫 번째로 등장하는 위치를 찾는다
2. 값을 return
- 성공 코드
    ```swift
    import Foundation

    func solution(_ seoul:[String]) -> String {
      return "김서방은 \(seoul.firstIndex(of: "Kim")!)에 있다"
  }
    ```
