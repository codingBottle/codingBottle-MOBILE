### 1번 문제 :  [중복된 문자 제거](https://school.programmers.co.kr/learn/courses/30/lessons/120888)

- 풀이실패 유형 : X
- 풀기까지 걸린 시간 : 약 30분
- 어떤 알고리즘을 사용했는지 : 탐색
- 접근부터 풀이까지의 과정
    1. 빈 문자의 result 생성
    2. 해당 문자가 문장에 포함되어있지 않으면 result에 넣기
- 성공 코드
    
    ```swift
    import Foundation
    
    func solution(_ my_string:String) -> String {
        var result = ""
    
        for char in my_string {
            if !result.contains(char) {
                result.append(char)
            }
        }
    
        return result
    }
    ```
    

### 2번 문제 :  [주사위 게임 3](https://school.programmers.co.kr/learn/courses/30/lessons/181916)

- 풀이실패 유형 : 2. 어떻게 풀어야 할 지 아이디어는 생각했지만 어떤 유형의 알고리즘을 사용해야하는지 모르겠다.
- 풀기까지 걸린 시간 : 약 1시간
- 어떤 알고리즘을 사용했는지 : 탐색
- 접근부터 풀이까지의 과정
    1. 배열로 만들고 set을 이용하여 중복되는 값 없이 만들고 개수를 저장함
    2. 중복되는 값이 하나부터 네개일 때까지의 케이스를 나눠서 리턴
- 실패 코드
    
    ```swift
    import Foundation
    
    func solution(_ a:Int, _ b:Int, _ c:Int, _ d:Int) -> Int {
        let array = [a, b, c, d]
        let set = Set(array)
        let uniqueCount = set.count
        
        switch uniqueCount {
        case 1:
            return 1111 * array[0] 
        case 2:
            return Int(pow(Double(10 * array[0] + array[1]), 2.0))
        case 3:
            return array[0] * array[1]
        case 4:
            return array.min()!
        default:
            return 0
        }
    }
    ```
    
- 성공 코드
    
    ```swift
    import Foundation
    
    func solution(_ a:Int, _ b:Int, _ c:Int, _ d:Int) -> Int {
        let nums = [a, b, c, d]
        let counts = nums.map { num in nums.filter { $0 == num }.count }
        
        if counts.max() == 4 {
            return a * 1111
    
        } else if counts.max() == 3 {
            let p = nums[counts.firstIndex(of: 3)!]
            let q = nums[counts.firstIndex(of: 1)!]
                    return Int(pow(Double(10 * p + q), 2.0))
    
        } else if counts.max() == 2 {
            if counts.min() == 2 {
                return a == b ? (a + c) * abs(a - c) : (a + b) * abs(a - b)
            } else {
                let p = nums[counts.firstIndex(of: 2)!]
                return nums.filter { $0 != p }.reduce(1, *)
            }
        } else {
            return nums.min()!
        }
    }
    ```
    

### 3번 문제 :  [서울에서 김서방 찾기](https://school.programmers.co.kr/learn/courses/30/lessons/12919)

- 풀이실패 유형 : X
- 풀기까지 걸린 시간 : 약 15분
- 어떤 알고리즘을 사용했는지 : 탐색
- 접근부터 풀이까지의 과정
    1. 포함되는 문자의 위치 찾기
    2. 반드시 있기 때문에 강제 언래핑
- 성공 코드
    
    ```swift
    func solution(_ seoul:[String]) -> String {
        return "김서방은 \(seoul.index(of: "Kim")!)에 있다"
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
