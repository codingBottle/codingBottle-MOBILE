# week11 Algorithm _ nahyun

### 1번 문제 : [더 크게 합치기](https://school.programmers.co.kr/learn/courses/30/lessons/181939)
- 풀이실패 유형 : X
- 풀기까지 걸린 시간 : 10분
- 어떤 알고리즘을 사용했는지 : 문자열로 변환후 정수 변환
- 접근부터 풀이까지의 과정
1. 두 수를 문자열로 변환하여 이어붙이고 정수로 변환
2. ab가 ba보다 크거나 같으면 ab 반환, 그렇지 않으면 ba 반환
- 성공 코드
 ```swift
   func solution(_ a: Int, _ b: Int) -> Int {
    let ab = Int("\(a)\(b)")!
    let ba = Int("\(b)\(a)")!
    
    if ab > ba || ab == ba {
        return ab
    } else {
        return ba
    }
}

  ```

### 2번 문제 : [피자 나눠 먹기(2)](https://school.programmers.co.kr/learn/courses/30/lessons/120815)
- 풀이실패 유형 : X
- 풀기까지 걸린 시간 : 10분 
- 어떤 알고리즘을 사용했는지 : while문 나머지 연산
- 접근부터 풀이까지의 과정 
1. 피자 개수(x)를 1로 저장
2. while문 x는 1씩 증가하며 6x를 n으로 나눠서 나머지가 0일 때 x값 반환
- 성공 코드
    ```swift
    import Foundation
    func solution(_ n:Int) -> Int {
      var x = 1
      while true {
        if (6 * x) % n == 0 {
          return x
        }
        x += 1
    }
    return 0
}
    ```

### 3번 문제 : [문자열 여러 번 뒤집기](https://school.programmers.co.kr/learn/courses/30/lessons/181913)
- 풀이실패 유형 : X
- 풀기까지 걸린 시간 : 30분
- 어떤 알고리즘을 사용했는지 : 투포인터
- 접근부터 풀이까지의 과정
1. 문자열을 배열로 변환
2. queries 배열에 있는 쿼리 순회
3. left는 오른쪽으로, right는 왼쪽으로 이동하면서 서로의 위치에 있는 문자를 교환하는데 left가 right보다 작거나 같을 때까지 반복
4. 모든 쿼리 처리 후 문자열로 변환하여 반환

- 성공 코드
    ```swift
    import Foundation
    func solution(_ my_string: String, _ queries: [[Int]]) -> String {
    var resultString = Array(my_string)
    
    for query in queries {
        let start = query[0] //시작 인덱스
        let end = query[1] //끝 인덱스
        
        var left = start
        var right = end
        
        while left < right {
            // 범위 내의 문자를 서로 교환
            let temp = resultString[left]
            resultString[left] = resultString[right]
            resultString[right] = temp
            
            // 인덱스 업데이트
            left += 1
            right -= 1
        }
    }
    
    return String(resultString)
}


### 4번 문제 : [배열 조각하기](https://school.programmers.co.kr/learn/courses/30/lessons/181893)
- 풀이실패 유형 : O
- 풀기까지 걸린 시간 : 
- 어떤 알고리즘을 사용했는지 : 
- 접근부터 풀이까지의 과정

- 성공 코드
    ```swift
    func solution(_ arr: [Int], _ queries: [Int]) -> [Int] {
    var resultArr = arr

    for (index, query) in queries.enumerated() {
        if query < resultArr.count {
            if index % 2 == 0 {
                resultArr = Array(resultArr[(query+1)...])
            } else {
                resultArr = Array(resultArr[..<query])
            }
        } else {
            return []
        }
    }
    
    return resultArr
}


    ```
