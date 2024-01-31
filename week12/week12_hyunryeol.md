# week12 Algorithm _ HyunRyeol
### 1번 문제 : [더 크게 합치기](https://school.programmers.co.kr/learn/courses/30/lessons/181939) 
- 풀이실패 유형 : x
- 풀기까지 걸린 시간 : 5분
- 어떤 알고리즘을 사용했는지 : 삼항연산
- 접근부터 풀이까지의 과정
    1. a,b를 문자열로 합치고 int로 형변환
    2. ab와 ba의 크기를 비교
- 성공 코드
    ```swift
    import Foundation

    func solution(_ a:Int, _ b:Int) -> Int {
        return Int("\(a)\(b)")! >= Int("\(b)\(a)")! ? Int("\(a)\(b)")! : Int("\(b)\(a)")!
    }
    ```
<br>

### 2번 문제 : [피자 나눠 먹기 (2)](https://school.programmers.co.kr/learn/courses/30/lessons/120815) 
- 풀이실패 유형 : x
- 풀기까지 걸린 시간 : 5분
- 어떤 알고리즘을 사용했는지 : while
- 접근부터 풀이까지의 과정
    1. 6조각 단위로 증가하는 반복문
    2. i를 n으로 나눴을 때 0이 되는 i찾기
    3. i를 6으로 나눠서 판수 구하기
- 성공 코드
    ```swift
    import Foundation

    func solution(_ n:Int) -> Int {
        var i = 6
        while i % n != 0{
            i += 6
        }
        return i/6
    }
    ```
<br>

### 3번 문제 : [문자열 여러 번 뒤집기](https://school.programmers.co.kr/learn/courses/30/lessons/181913) 
- 풀이실패 유형 : x
- 풀기까지 걸린 시간 : 10분
- 어떤 알고리즘을 사용했는지 : reversed
- 접근부터 풀이까지의 과정
    1. queries를 순환하는 반복문
    2. 0번째값과 1번째 값을 reversed
    3. reversed한 값을 기존 배열 인덱스에 저장
- 성공 코드
    ```swift
    import Foundation

    func solution(_ my_string:String, _ queries:[[Int]]) -> String {
        var str = Array(my_string)
        
        queries.forEach { query in
            let start = query[0]
            let end = query[1]
            let reversed = Array(str[start...end].reversed())
            
            for i in 0 ..< reversed.count {
                str[start + i] = reversed[i]
            }
        }
        
        return String(str)
    }
    ```
<br>

### 4번 문제 : [배열 조각하기](https://school.programmers.co.kr/learn/courses/30/lessons/181893)
- 풀이실패 유형 : x
- 풀기까지 걸린 시간 : 10분
- 어떤 알고리즘을 사용했는지 : prefix, dropFirst
- 접근부터 풀이까지의 과정
    1. query길이만큼 반복문 생성
    2. i가 짝수번째 인덱스면 prefix
    3. 홀수 번째 인덱스면 dropFirst
- 성공 코드
    ```swift
    import Foundation

    func solution(_ arr:[Int], _ query:[Int]) -> [Int] {
        var result = arr
        
        for i in 0..<query.count {
            if i % 2 == 0 {
                result = Array(result.prefix(through: query[i]))
            } else {
                result = Array(result.dropFirst(query[i]))
            }
        }

        return result
    }
    ```
<br>
