# week12 Algorithm _ nahyun

### 1번 문제 : [영어가 싫어요](https://school.programmers.co.kr/learn/courses/30/lessons/120894)
- 풀이실패 유형 : X 
- 풀기까지 걸린 시간 : 20분
- 어떤 알고리즘을 사용했는지 : 배열인덱싱
- 접근부터 풀이까지의 과정
1. 배열에 영어 숫자를 저장
2. numberStrings 배열에 포함되어 있는지 확인하고 있다면 result에 추가
- 성공 코드
 ```swift
    func solution(_ numbers: String) -> Int64? {
    let numberStrings = ["zero", "one", "two", "three", "four", "five", "six", "seven", "eight", "nine"]
    var result: Int64 = 0
    var currentNumber = ""
    
    for char in numbers {
        currentNumber.append(char)
        if let index = numberStrings.firstIndex(of: currentNumber) {
            result = result * 10 + Int64(index)
            currentNumber = ""
        }
    }
    
    return result
}
  ```

### 2번 문제 : [최댓값과 최솟값](https://school.programmers.co.kr/learn/courses/30/lessons/12939)
- 풀이실패 유형 : X
- 풀기까지 걸린 시간 : 20분 
- 어떤 알고리즘을 사용했는지 : max, min
- 접근부터 풀이까지의 과정 
1. 문자열 s를 공백 기준으로 나누고 compactMap으로 정수 배열로 변환
2. numbers 배열이 비어있는지 확인
3. 최댓값과 최솟값을 max, min으로 찾고 문자열로 변환하여 리턴
- 성공 코드
    ```swift
    func solution(_ s: String) -> String {
    let numbers = s.split(separator: " ").compactMap { Int($0) }
    
    guard !numbers.isEmpty else {
        return ""
    }
  
    let minNumber = numbers.min()!
    let maxNumber = numbers.max()!
    
    return "\(minNumber) \(maxNumber)"
} ```

### 3번 문제 : [기능개발](https://school.programmers.co.kr/learn/courses/30/lessons/42586)
- 풀이실패 유형 : X
- 풀기까지 걸린 시간 : 30분
- 어떤 알고리즘을 사용했는지 : 반복문
- 접근부터 풀이까지의 과정
1. 작업 진행률과 작업 속도 배열 선언, 작업 개수 배열 초기화
2. 모든 작업이 배포될 때까지 반복하여 개수를 계산
3. 진행률 + 속도로 진행률 업데이트
4. 진행률이 100 이상인 경우 제거



- 성공 코드
    ```swift
    func solution(_ progresses: [Int], _ speeds: [Int]) -> [Int] {
    var progresses = progresses
    var speeds = speeds
    var num: [Int] = []
    
    while !progresses.isEmpty {
        for i in 0..<progresses.count {
            progresses[i] += speeds[i]
        }
        
        var count = 0
        while !progresses.isEmpty && progresses[0] >= 100 {
            progresses.removeFirst()
            speeds.removeFirst()
            count += 1
        }

        if count > 0 {
            num.append(count)
        }
    }
    
    return num
}

    
    ```

### 4번 문제 : [영어 끝말잇기](https://school.programmers.co.kr/learn/courses/30/lessons/12981)
- 풀이실패 유형 : X
- 풀기까지 걸린 시간 : 40분 
- 어떤 알고리즘을 사용했는지 : 반복문
- 접근부터 풀이까지의 과정 
1. 이미 사용된 단어들을 저장하기 위한 빈 배열 usedWords
2. enumerated를 사용해서 단어와 인덱스를 순서대로 가져옴
3. 이미 사용된 단어나 이전 단어의 마지막 글자와 현재 단어의 첫글자가 다른 경우를 찾음
4. 규칙 위반시 말한 사람 번호와 차례 계산하여 리턴
5. 위반하지 않은 단어 usedWords에 추가
6. 규칙 위반자가 없을시 [0,0] 리턴

- 성공 코드
  ```swift
    func solution(_ n:Int, _ words:[String]) -> [Int] {
    var usedWords = [String]()
    
    for (index, word) in words.enumerated() {
        if index > 0 && (usedWords.contains(word) || Array(words[index - 1]).last! != Array(word).first!) {
            return [(index % n) + 1, (index / n) + 1]
        }
        usedWords.append(word)
    }
    
    return [0, 0]
}

  ```
