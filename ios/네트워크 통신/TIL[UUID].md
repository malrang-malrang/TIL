# Today I Learned.

## 학습내용
# UUID(Universally Unique IDentifier) ????
>![](https://i.imgur.com/SwIFCYL.png)
>
>## Declaration
>```swift
>struct UUID
>```
>**범용 고유 식별자**
>
>네트워크 상에서 고유성이 보장되는 id를 만들기 위한 표준 규약이다.
>
>네트워크 환경에서 유일성이 보장되는 id 라고 이해하자.
>
>UUID 는 128비트의 숫자이며 32자리의 16진수로 표현된다.
>8-4-4-4-12 글자 마다 하이픈을 삽입해 5개의 그룹으로 구분한다.
>
>```swift
>// 예시
>550e8400-e29b-41d4-a716-446655440000
>```
>340,282,366,920,938,463,463,374,607,431,768,211,456 개의 사용가능한 UUID 값이 존재한다;;
>
>### 만들어지는 방식
>
>**RFC 4122 문서를 기반으로 생성한다.**
>
>RFC(Request for Comments)란?
>- 비평을 기다리는 문서라는 의미
>   - 컴퓨터 네트워크 공학 등에서 인터넷 기술에 적용 가능한 새로운 연구, 혁신, 기법 등을 아우르는 메모를 나타낸다고한다.
>- RFC편집자는 매 RFC 문서에 일련 번호를 부여한다.
>   - 일련 번호를 부여받고 출판되면 RFC 는 폐지되거나 수정되지 않는다.
>- RFC문서가 수정이 필요할 경우
>   - 수정된 문서를 다른 RFC 문서로 다시 출판해야한다.
>   - 그렇기 때문에 일부 RFC 는 이전버전의 RFC 를 개선한 문서이며 RFC 는 인터넷 표준의 역사를 나타내기도한다.(옛날꺼 그대로 남아있다는뜻)
>   
>## swift 의 UUID 
>![](https://i.imgur.com/vcJCJTd.png)
>
>공식문서를 보니 RFC4122 문서를 기반으로 만들어지며 4버전 방식으로 생성한다는것을 알수 있다.
>
>4번전 방식은 random bytes 즉 랜덤값으로 만든다.
>
>### 버전 종류
>- **1버전**
>   - 시간 정보를 기반으로 UUID 생성.
>- **3버전**
>   - MD5 hashing을 사용해서 생성.
>- **4버전**
>   - 랜덤 값을 이용해서 UUID를 생성.
>- **5버전**
>   - SHA-1 hashing 을 사용해서 생성.
>

## 참고문서
[developer.apple.com](https://developer.apple.com/documentation/foundation/uuid)
[위키!](https://ko.wikipedia.org/wiki/RFC)
