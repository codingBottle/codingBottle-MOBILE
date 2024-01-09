# week10 Algorithm_ino

> 1번 문제 : [암호해독](https://school.programmers.co.kr/learn/courses/30/lessons/120892)
> 
- 풀이실패 유형 : X
- 풀기까지 걸린 시간 : 10분
- 사용한 알고리즘 : 완전탐색
- 접근부터 풀이까지의 과정
    1. index+1의 값이 해당 문자열의 차례임을 인지
    2. brute-force로 idx+1의 순서에 해당 값을 answer에 추가함
- 성공 코드

```swift
import Foundation

func solution(_ cipher:String, _ code:Int) -> String {
    var answer = ""
    for (idx, i) in cipher.enumerated() {
        if (idx+1) % code == 0 {
            answer.append(String(i))
        } 
    }
    return answer
}
```

> 2번 문제 : [옹알이](https://school.programmers.co.kr/learn/courses/30/lessons/120956)
> 
- 풀이실패 유형 : X
- 풀기까지 걸린 시간 : 60분
- 사용한 알고리즘 : 완전탐색
- 접근부터 풀이까지의 과정
    1. babbling을 탐색하며 내부의 값과 filter의 값을 비교
    2. 일치할 경우 해당 filter의 값을 temp배열에 추가
    3. filter 배열과 비교가 끝난 이후 temp string의 길이와 타겟 string의 길이가 같을 경우 answer 값을 증감 연산
- 성공 코드

```swift
import Foundation

func solution(_ babbling:[String]) -> Int {
    let filter = ["aya", "ye", "woo", "ma"]
    var count = 0
    for i in babbling {
        var tmp = ""
        for j in filter {
            if i.range(of: j) != nil {
            tmp += j
        }
        }
        if tmp.count == i.count {
            count += 1
        }
    }
    return count
}
```

> 3번 문제 : [둘만의 암호](https://school.programmers.co.kr/learn/courses/30/lessons/155652)
> 
- 풀이실패 유형 : X
- 풀기까지 걸린 시간 : 120분
- 사용한 알고리즘 : 완전탐색
- 접근부터 풀이까지의 과정
    1. z → a 변한다는 점에서 ascii code 이용 풀이임을 추측
    2. a, z 를 선언 및 초기화, skip 배열을 ascii 코드화하여 초기화(mapping)
    3. s 를 탐색하며 index와 실 증가 횟수를 저장하는 변수 indexcnt의 값이 같아질 때까지 ascii를 증감연산( skipascii 배열에 해당하는 문자일 경우 indexcnt의 값을 증가시키지 아니함 )
    4.  answer에 추가한뒤 탐색이 끝나면 이를 반환
- 성공 코드

```swift
import Foundation

func solution(_ s:String, _ skip:String, _ index:Int) -> String {
    let aAscii = Int(UnicodeScalar("a").value)
    let zAscii = Int(UnicodeScalar("z").value)
    var answer : String = ""
    let skipAscii = skip.map { Int(UnicodeScalar(String($0))!.value) }

    for i in s {
        var indexcnt = 0
        var ascii = Int(UnicodeScalar(String(i))!.value)
        
        while indexcnt < index {
            
            ascii += 1
                        
            if ascii > zAscii {
                ascii = aAscii
            }
            
            if skipAscii.contains(ascii) {
                continue
            }
            else {
                indexcnt += 1
            }
        }
        answer.append(String(UnicodeScalar(ascii)!))
    }
    return answer
}
```

> 1번 문제 : [원하는 문자열 찾기](https://school.programmers.co.kr/learn/courses/30/lessons/181878)
> 

풀이실패 유형 : X

- 풀기까지 걸린 시간 : 10분
- 사용한 알고리즘 : 완전탐색
- 접근부터 풀이까지의 과정
    1. 대소문자를 구분하지 않는다는 구문을 보고 문자열을 전부 소or대문자로 변경 후 처리 가능함을 유추
    2. 소문자로 변경 이후 포함할 경우 1을 , 그렇지 않을 경우 0 을 리턴
- 성공 코드

```swift
import Foundation

func solution(_ myString:String, _ pat:String) -> Int {
    var a = myString.lowercased()
    var b = pat.lowercased()
    if a.contains(b) {
        return 1
    }
    else {
    return 0
    }
}
```

> 2번 문제 : [ad 제거하기](https://school.programmers.co.kr/learn/courses/30/lessons/181870)
> 

풀이실패 유형 : X

- 풀기까지 걸린 시간 : 10분
- 사용한 알고리즘 : 완전탐색
- 접근부터 풀이까지의 과정
    1. 배열을 루프하며 ad를 가진 문자열을 정답 배열에 삽입하면 되겠다고생각함
- 성공 코드

```swift
import Foundation

func solution(_ strArr:[String]) -> [String] {
    var answer = [String]()
    for i in strArr {
        if i.contains("ad") {
            continue
        }
        else {
            answer.append(i)
        }
    }
    return answer
}
```
