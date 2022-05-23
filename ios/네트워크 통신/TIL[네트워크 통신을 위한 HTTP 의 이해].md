# Today I Learned.

## 학습내용
# 네트워크 통신을 위한 HTTP 의 이해

# 클라이언트와 서버 
> ![](https://i.imgur.com/DhCFjLH.png)
> 클라이언트는 항상 서버에게 액세스를 요청(request) 하여 통신을 시작한다.
> 
>모든 요청에 대해 서버는 응답(response)하고 이미지, 음악, 비디오, 등과 같은 리소스가 서버에서 클라이언트 로 다운로드된다.

# URL 과 URI
>![](https://i.imgur.com/91BGG51.png)
>
>- **URL(Uniform Resource Locator)**
>   - 자원이 실제로 존재하는 위치를 가르킨다.
>   
>- **URI(Uniform Resource Identifier)**
>   - 자원의 위치뿐만 아니라 자원에 대한 고유 식별자로서 URL을 포함한다.
>
>**URI가 URL 보다 포괄적인 개념이다.**
>
>- **예시**
>https://camp5th.yagom-academy.kr/camp - URL
>https://camp5th.yagom-academy.kr/camp?id=grumpy - URI

# Query String, Path Variable
>## Query String
>Query String이란 서버에게 무엇이 필요한지, 혹은 클라이언트에게 무엇이 필요한지 묻는 문자열을 의미한다.
>```
>https://market-training.yagom-academy.kr/api/products?page_no=1&items_per_page=10
>```
>간단하게 얘기하자면 위의 예시 URL 에서 ? 뒤에있는것을 뜻한다.
>```
>page_no=1&items_per_page=10
>```
>요녀석이 되겠다.
>
>해석하자면 page_no = 1 이고 items_per_page = 10 이다.
>현재 페이지는 1번이고 한페이지당 아이템10 개를 보여주란 뜻이다.
>
>## Path Variable
>Path Variable이란 Query String과 같이 데이터를 넘기는 방법 중의 하나로 경로를 변수처럼 사용하는 것을 의미한다.
>```
>https://market-training.yagom-academy.kr/api/products?page_no=1&items_per_page=10
>```
>위의 예시와 같은 URL 이다.
>
>여기서 path 란 
>```
>/api/products
>```
>요녀석이다.
>

# HTTP(HyperText Transfer Protocol)
>하이퍼 텍스트(HTML) 문서를 교환하기 위해 만들어진 protocol(통신규약)
>
>웹상에서 네트워크로 서버와 통신을 할때 어떠한 형식으로 서로 통신을 하자고 규정해놓은 통신형식, 혹은 통신구조 라고 보면된다!
>
>프론트엔드 서버와 클라이언트간의 통신에 통신에 사용된다.
>
>백엔드와 프론트 엔드 서버간의 통신에도 사용된다.
>
>HTTP는 TCP/IP 기반으로 되어있다.
>
>예를 들면 클라이언트인 웹 브라우저가 HTTP를 통해 서버로 부터 그림이나 정보를 요청하면 서버는 이요청에 응답하여 필요한 정보를 해당 사용자에게 전달하게 된다.
>
>이 정보가 모니터와 같은 출력 장치를 통해 사용자에게 나타나는것이다.
>
>## HTTP 통신방식
>- HTTP 는 기본적으로 요청(request)/응답(response)구조로 되어있다.
>클라이언트가 HTTP request를 서버에 보내면 서버는 HTTP response 를 보내는 구조다.
>클라이언트와 서버의 모든 통신이 요청과 응답으로 이루어 진다.
>
>- HTTP 는 Stateless 다.
>Stateless 란? 말그대로 state(상태)가 없다. 상태를 저장하지 않는 다는뜻이다.
>요청이 오면 그에 응답할뿐 다른 요청들과는 무관하게 각각의 요청에 각각의 응답을 할뿐이다.
>예를 들면 클라이언트가 요청을 보내고 응답을 받은후 다시 요청을 보낼때 전에 보낸 요청과 응답에 대해서는 알지못한다는 뜻이다.
>그래서 여러 요청과응답 의 진행과정이나 데이터가 필요할때는 쿠키나 세션 등등을 이용한다.
>
>---
>## HTTP 메세지 구조
>**Request(요청), Response(응답) 의 구조 예시사진**
>![](https://i.imgur.com/RiglaYT.jpg)
>
>---
>## HTTP Request 의 구조
>![](https://i.imgur.com/PbpHwwe.png)
>위의 사진은 body 가 없는 request 메세지 예시 사진이다.
>
>HTTP request 메세지는 크게 3부분으로 구성된다.
>
>**1. start line(라인)**
>- 응답/요청 여부, 메시지 전송방식, 상태 정보
>
>**2. headers(헤더)**
>- 메시지 본문에 대한 메타 정보
>
>**3. body(바디)**
>- 보내고자 하는 메시지(데이터)
>
>
>**HTTP POST Request 예시**
>```swift
>//start line
>POST /payment-sync HTTP/1.1
>
>//headers
>Accept: application/json
>Accept-Encoding: gzip, deflate
>Connection: keep-alive
>Content-Length: 83
>Content-Type: application/json
>Host: intropython.com
>User-Agent: HTTPie/0.9.3
>
>//body
>{
>    "imp_uid": "imp_1234567890",
>    "merchant_uid": "order_id_8237352",
>    "status": "paid"
>}
>```
>
>---
>### Request start Line
>```swift
>POST /payment-sync HTTP/1.1
>```
>말그대로 HTTP request 의 첫 라인.
>HTTP request의 start line 또한 3부분으로 구성되어있다.
>
>**1. HTTP Method**
>- 해당 request 가 의도한 action을 정의하는 부분
>HTTP Method 에는  GET, POST, PUT, DELETE, OPTIONS 등이있다.
>
>**2. Request target**
>- 해당 request 가 전송되는 목표 uri
>예를들면 /login, 위의 예시에서 보자면 /payment-sync 부분
>
>**3. HTTP Version**
>- 사용되는 HTTP 버전.
>버전에는 1.0, 1.1, 2.0 등이있다. 
>
>---
>### Request Headers
>```swift
>Accept: application/json
>Accept-Encoding: gzip, deflate
>Connection: keep-alive
>Content-Length: 83
>Content-Type: application/json
>Host: intropython.com
>User-Agent: HTTPie/0.9.3
>```
>- 해당 request 에 대한 추가정보 를 담고있는 부분.
>- Key: Value 값으로 되어있다.
>예시 HOST(Key): intropython.com(Value)
>- Headers 도 크게 3부분으로 나뉜다.(general headers, request headers, entity headers)
>
>**자주 사용되는 header 정보들**
>
>**1. HOST**
>- 요청이전송되는 target의 host url
>
>**2. User-Agent**
>- 요청을 보내는 클라이언트에 대한정보
>
>**3. Accept**
>- 해당 요청이 받을수 있는 응답(response)타입.
>
>**4. Connection**
>- 해당 요청이 끝난후에 클라이언트와 서버가 게속해서 네트워크 커넥션을 유지할것인지 아니면 끊을것인지에 대해 지시하는 부분
>
>**5. Content-Type**
>- 해당 요청이 보내는 메세지 body의 타입.
>- 예시: JSON을 보내면 application/json 이다.
>
>**6. Content-Length**
>- 메세지 body의 길이
>
>**7. Cookie**
>- 클라이언트 로컬에 저장되는 Key-Value 쌍의 데이터 파일
>
>**8. Referer**
>- 경유한 웹 사이트에 대한 정보
>
>---
>### Request Body
>```swift
>{
>    "imp_uid": "imp_1234567890",
>    "merchant_uid": "order_id_8237352",
>    "status": "paid"
>}
>```
>해당 request의 실제 메세지/내용
>
>body 가 없는 request 도 많다.
>예를 들면 GET request 는 body 가 없는 경우가 많다.
>
>---
># HTTP Request 메서드, RESTful API
>
>**CRUD 란 Create, Read, Update, Delete 를 의미한다.**
>그리고 HTTP 메서드에서 GET, POST, PUT, DELETE 가 각 CRUD의 역할을 한다.
>
>**HTTP의 메서드의 역할을 잘 구분하여 사용하는 방식을 'RESTful API'라고 한다.**
>
>HTTP 메서드는 클라이언트가 하고싶은 처리를 서버에게 전하는 역할을 한다.
>메서드는 총 8개가 존재한다.
>
>## 1. GET 
>- 서버로 부터 데이터를 취득할때 사용하는 메서드다.
>- Web페이지, 이미지, 영상 등을 취득할때 사용된다.
>- 데이터 생성, 수정, 삭제 없이 받아오기만 할때 사용된다.
>- 데이터를 받아오기만 하기떄문에 request 에 body 를 안보내는 경우가 많다.
>
>```swift
>// 요청 예시
>GET /HTTP/1.1
>Host: foo.com
>
>// 코드 예시
>var urlRequest = URLRequest(url: url)
>     urlRequest.httpMethod = "GET"
>```
>
>## 2. POST
>- 서버에 데이터를 추가, 작성 등
>- 데이터를 생성 및 수정 할때 사용된다.
>- request body 가 대부분 포함되어 보내진다.
>- 서버로 데이터를 전송하고, 요청 유형은 Content-Type 헤더로 나타낸다.
>- 예를 들면 블로그에 새로운 게시글을 올릴때 처럼 새로운 리소스에 데이터를 만들때 사용되며 기존의 데이터를 수정할때도 사용한다.
>
>```swift
>// 요청 예시
>POST / HTTP/1.1
>Host: foo.com
>Content-Type: application/x-www-form-urlencoded
>Content-Length: 13
>
>say=Hi&to=Mom
>
>// 코드 예시
>var urlRequest = URLRequest(url: url)
>     urlRequest.httpMethod = "POST"
>```
>
>## 3. PUT
>- 서버의 데이터를 갱신, 작성 등
>- 요청 페이로드(request body)를 사용해 새로운 리소스를 생성하거나, 대상 리소스를 대체할때 사용한다.
>
>- POST, PUT 둘다 새로운 리소스를 생성할수 있는데 둘의 차이점은 POST 는 기존의 것이있다면 Update 하고, 없다면 새로운 리소스를 생성한다.
>PUT 는 기존의 것이있다면 새로만든 리소스로 대체하고, 기존의 것이 없다면 새로운 리소스를 생성하는 것이다.
>
>```swift
>// 요청 예시
>PUT /new.html HTTP/1.1
>Host: example.com
>Content-type: text/html
>Content-length: 16
>
><p>New File</p>
>
>// 코드 예시
>var urlRequest = URLRequest(url: url)
>     urlRequest.httpMethod = "PUT"
>```
>
>## 4. DELETE
>- 서버의 데이터를 삭제
>- 메소드 이름 그대로 특정 리소스를 제거하는데 사용된다.
>
>```swift
>// 요청 예시
>DELETE /file.html HTTP/1.1
>
>// 코드 예시
>var urlRequest = URLRequest(url: url)
>     urlRequest.httpMethod = "DELETE"
>```
>
>## 5. HEAD
>- 서버 리소스의 헤더(메타 데이터의 취득)
>
>## 6. OPTIONS
>- 리소스가 지원하고 있는 메소드의 취득
>- 예를 들면 URI에서 어떤 메서드를 사용할수있는지 알고싶을때 OPTIONS 요청을 사용해서 확인할수있다.
>```swift
>http -v OPTIONS http://example.org
>
>OPTIONS / HTTP/1.1
>Accept: */*
>Accept-Encoding: gzip, deflate
>Connection: keep-alive
>Content-Length: 0
>Host: example.org
>User-Agent: HTTPie/0.9.3
>
>HTTP/1.1 200 OK
>Allow: OPTIONS, GET, HEAD, POST
>Cache-Control: max-age=604800
>Content-Length: 0
>Date: Mon, 20 Aug 2018 08:37:45 GMT
>Expires: Mon, 27 Aug 2018 08:37:45 GMT
>Server: EOS (vny006/0450)
>```
>
>## 7. PATCH
>- 리소스가 지원하고 있는 메소드의 취득
>
>## 8. CONNECT
>- 프록시 동작의 터널 접속을 변경
>
>
>---
>## HTTP Response 의 구조
>
>![](https://i.imgur.com/srHGVLt.png)
>위의 사진도 body 가 없는 Response 의 예시 사진이다.
>
>Response 도 Request 와 마찬가지로 3부분으로 구성되어 있다.
>**1. Status line**
>**2. Headers**
>**3. Body**
>
>![](https://i.imgur.com/MwMby4j.png)
>
>---
>### Response Status Line
>```swift
>HTTP/1.1 404 Not Found
>```
>Response 의 상태를 간략하게 나타내주는 부분으로 3부분으로 구성되어있다.
>
>**1. HTTP 버전**
>
>**2. Status code**
>- 응답 상태를 나타내는 코드, 숫자로 되어있는 코드
>
>**3. Status text**
>- 응답상태를 간략하게 설명해주는 부분
>예를 들면 NotFound
>
>---
>### Response Headers
>
>```swift
>Connection: close
>Content-Length: 1573
>Content-Type: text/html; charset=UTF-8
>Date: Mon, 20 Aug 2018 07:59:05 GMT
>```
>
>- Request의 Headers 와 동일하다.
>- 하지만 response 에서만 사용되는 header 값들이 있다.
>- 예를들면 User-Agent 대신에 Server 헤더가 사용된다.
>
>---
>### Response Body
>![](https://i.imgur.com/Q4h7mTS.png)
>
>- Request 의 body 와 일반적으로 동일하다.
>- Request 와 마찬가지로 데이터를 전송할 필요가 없을경우 body가 비어있을수 있다.
>
>---
>## HTTP Status Code
>
>**200**
>- 문제없이 잘 실행되었을때 보내는 코드
>
>**301**
>- Moved Permanently
>- 해당 URI 가 다른 주소로 바뀌었을때 보내는 코드.
>```swift
>HTTP/1.1 301 Moved Permanently
>Location: http://www.example.org/index.asp
>```
>
>**400**
>- Bad Request
>- 해당 요청이 잘못된 요청일때 보내는 코드
>- 주로 요청에 포함된 input 값들이 잘못된 값들이 보내졌을때 사용되는 코드
>- 예를 들어 전화번호를 보내야 되는데 text 가 보내졌을때 등등
>
>**401**
>- Unauthorized
>- 유저가 해당 요청을 진행하려면 로그인을 하거나 회원 가입이 필요하다는 것을 나타내려 할때 쓰이는 코드
>
>**403**
>- Forbidden
>- 유저가 해당 요청에 대한 권한이 없다는뜻
>- 예를 들면 유료결제를 한 유저만 볼수있는 데이터를 요청했을때를 예시로 들수 있다.
>
>**404**
>- 요청된 uri가 존재하지 않는다는 뜻.
>
>**500**
>- internal Sercer Error
>- 서버에서 에러가 났을때 사용되는 코드
>- 백엔드 개발자가 싫어하는 코드
>
>---

