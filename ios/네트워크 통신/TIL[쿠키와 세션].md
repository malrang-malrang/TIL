# Today I Learned.

## 학습내용
# 쿠키와 세션

## HTTP 의 특징, 쿠키와 세션을 사용하는 이유
>쿠키와 세션을 이해하기위해 먼저 HTTP 의 특징에 대해 알아보자!
>
>HTTTP(Hypertext Transfer Protocol) 는 인터넷상에서 데이터를 주고받기 위해 서버/ 클라이언트 모델을 따르는 통신 규약을 말한다.
>
>HTTP 는 비연결성(Connectionless) 과 비상태성(Stateless) 이라는 특징이 있다.
>
>이는 서버의 자원을 절약 하기위함이다.
>모든 사용자의 요청마다 연결과 해제의 과정을 거치기 때문에 연결 상태가 유지되지 않고, 연결 해제 후에 상태 정보가 저장되지 않는다는것이다.
>
>하지만 이로 인해 사용자를 식별할 수 없어 같은 사용자가 요청을 여러번 하더라도 매번 새로운 사용자로 인식하는 단점이 있다.
>
>하지만 우리가 사용하는 웹사이트를 생각해보면 로그인을 한번 하고나면 그 사이트에서는 다시 로그인할 필요 없이 여러 페이지의 기능들을 이용할수 있고 심지어 브라우저를 종료했다가 나중에 다시 접속 했을때도 로그인 상태를 유지하는것을 볼수 있는데 이를 가능케 하는것이 쿠키와 세션이다.
>
>HTTP의 비연결성과 비상태성을 보완하여 서버가 클라이언트를 식별하게 해주는것이라 할수있다.
>
>![](https://i.imgur.com/3ppjj0Z.png)

## 쿠키(Cookie)
>### 쿠키란?
>- 클라이언트(브라우저 등등) 로컬에 저장되는 키와 값이 들어있는 작은 데이터 파일이다.
>
>- 사용자 인증이 유효한 시간을 명시할수 있으며, 유효시간이 정해지면 브라우저가 종료되어도 인증이 유지된다는 특징이있다.
>
>- 쿠키는 클라이언트의 상태 정보를 로컬에 저장했다가 참조한다.
>
>- 클라이언트에 300개까지 쿠키를 저장할수 있으며 하나의 도메인당 20개의 값만 가질수 있다.
>
>- 하나의 쿠키값은 4KB까지 저장한다.
>
>- Response Header 에  Set-Cookie 속성을 사용하면 클라이언트에 쿠키를 만들 수 있다.
>
>- 쿠키는 사용자가 따로 요청하지 않아도 브라우저가 Request 시에 Request Header 를 넣어서 자동으로 서버에 전송한다.
>
>### 쿠키의 구성 요소
>- 이름: 각각의 쿠키를 구별하는 데 사용되는 이름
>- 값: 쿠키의 이름과 관련된 값
>- 도메인: 쿠키를 전송할 도메인
>- 경로: 쿠키를 전송할 요청 경로
>
>### 쿠키의 동작 방식
>1. 클라이언트가 페이지를 요청
>2. 서버에서 쿠키를 생성
>3. HTTP 헤더에 쿠키를 포함 시켜 응답
>4. 브라우저가 종료되어도 쿠키 만료 기간이 있다면 클라이언트에서 보관하고 있는다.
>5. 같은 요청을 할경우 HTTP 헤더에 쿠키를 함께 보낸다.
>6. 서버에서 쿠키를 읽어 이전 상태 정보를 변경할 필요가 있을때 쿠키를 업데이트 하면 변경된 쿠키를 HTTP 헤더에 포함시켜 응답한다.
>
>### 쿠키의 사용 예시
>- 방문 사이트에서 로그인 시:  아이디와 비밀번호를 저장하시겠습니까?
>- 쇼핑몰의 장바구니 기능
>- 자동 로그인: 팝업 에서 오늘 더이상 이창을 보지 않음 체크
>
## 세션(Session)
>### 세션이란?
>- 세션은 쿠키를 기반 하고 있지만, 사용자 정보 파일을 브라우저에 저장하는 쿠키와 달리 세션은 서버측에서 관리한다.
>
>- 서버에서는 클라이언트를 구분하기 위해 세션 ID 를 부여하며 웹 브라우저가 서버에 접속해서 브라우저를 종료할때까지 인증상태를 유지한다.
>
>- 접속 시간에 제한을 두어 일정 시간 응답이 없다면 정보가 유지되지 않게 설정이 가능하다.
>
>- 사용자에 대한 정보를 서버에 두기 때문에 쿠키보다 보안에 좋지만, 사용자가 많아질수록 서버 메모리를 많이 차지하게 된다.
>
>- 동시접속자 수가 많은 웹사이트의 경우 서버에 과부하를 주게 되므로 성능 저하의 요인이된다.
>
>- 클라이언트가 Request를 보내면 해당 서버의 엔진이 클라이언트에게 유일한 ID 를 부여하는데 이것이 세션이다.
>
>- 쿠키와 세션은 비슷한 역할을 하며 동작원리도 비슷하다.
>  그 이유는 세션도 결국 쿠키를 사용하기 때문이다.
>
>### 세션의 동작 방식
>1. 클라이언트가 서버에 저복 시 세션 ID 를 발급 받는다.
>2. 클라이언트는 세션 ID 에 대해 쿠키를 사용해서 저장하고 가지고 있는다.
>3. 클라이언트는 서버에 요청할때, 이쿠키의 세션 ID 를 같이 서버에 전달해서 요청한다.
>4. 서버는 세션 ID 를 전달 받아서 별다른 작업 없이 세션 ID 로 세션에 있는 클라이언트 정보를 가져와서 사용한다.
>5. 클라이언트 정보를 가지고 서버 요청을 처리하여 클라이언트에게 응답한다.
>
>### 세션의 특징
>- 각 클라이언트에게 고유 ID 를 부여 한다.
>- 세션 ID로 클라이언트를 구분해서 클라이언트의 요구에 맞는 서비스를 제공한다.
>보안면에서 쿠키보다 우수하다.
>사용자가 많아질수록 서버 메모리를 많이 차지하게된다.
>
>### 세션의 사용예시
>- 로그인 같이 보안상 중요한 작업을 수행할때 사용한다.
>
>
>### 쿠키와 세션의 차이점 
>|종류   |Cookie      | Session|
>|------|------------|---------|
>|저장위치 |클라이언트 로컬|서버      |
>|저장형식 |text       |object   |
>|리소스  |클라이언트의 리소스| 서버의 리소스|
>|용량제한 |도메인당 20개, 1쿠키당 4KB|제한 없음|
>|만료시점 |쿠키 저장시 설정(default 브라우저 종료시 만료)|알수 없음
>   
>차이점은 저장위치, 보안, 라이프사이클, 속도 등등이 있다.
>
>**1. 저장위치**
>- 쿠키: 클라이언트 로컬에 저장된다. 서버의 자원을 사용하지 않는다.
>- 세션: 서버에 저장된다. 서버의 자원을 사용한다.
>
>**2. 보안**
>- 쿠키: 쿠키는 로컬에 저장되기 때문에 변질되거나 request 에서 스니핑 당할 우려가 있어 보안에 취약하다.
>- 세션: 세션은 쿠키를 이용해 세션 id 만 저장하고 그것으로 구분해서 서버에서 처리하기 때문에 보안성이 좋다.
>
>**3. 라이프 사이클**
>쿠키와 세션 모두 만료시간이 존재한다.
>
>쿠키와 세션의 차이를 물어볼때 저장위치와 보안에 대해서 말하지만 사실 중요한것은 라이프사이클이다.
>
>- 쿠키: 만료시간이 있지만 파일로 저장되기 때문에 브라우저를 종료해도 게속해서 정보가 남아있을수 있다. 만료기간을 넉넉하게 잡아두면 쿠키살제를 할때까지 유지될수도 있다.
>- 세션: 세션도 만료시간을 정할수 있지만 브라우저가 종료되면 만료시간에 상관없이 삭제된다.
>
>**4. 속도**
>- 쿠키: 쿠키에 정보가 있기 때문에 서버에 요청시 속도가 빠르다.
>- 세션: 세션은 정보가 서버에 있기 때문에 서버 작업이 요구되어 비교적 느리다.
>
>
>### 세션이 보안이 더좋은데 쿠키 왜씀??
>- 세션은 서버의 자원을 사용하기 때문에 무분별하게 만들다보면 서버의 메모리가 감당할수 없어질수있다. 
>- 속도도 느려질수 있기 때문에 쿠키가 유리한 경우가 있다.
>
>### 쿠키,세션 과 캐시는 엄연히 다른놈이다
>- 캐시는 이미지나 css, js 파일등을 브라우저나 서버 앞단에 저장해놓고 사용하는것이다.
>- 한번 캐시에 저장되면 브라우저를 참고하기 때문에 서버에서 변경이 되어도 사용자는 변경되지 않게 보일수 있는데 이런 부분을 캐시를 지워주거나 서버에서 클라이언트로 응답을 보낼때 header 에 캐시 만료시간을 명시하는 방법등을 이용할수 있다.
