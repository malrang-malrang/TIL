# Today I Learned

# 학습내용

## Software Architecture
영어단어로써의 아키텍처는 건축학 이라는 뜻이다.
건축으로 치면 건물의 평면도에 해당한다고 할수있다.
“소프트웨어아키텍처는 소프트웨어 요소와 이들 요소의 외부 속성 그리고 이들 사이 의 관계를 구성하는 시스템의 구조이다.
간단히 말하자면 소프트웨어 의 구조 라고 할수있을것 같다.

- 포토샵 으로 예를 들자면 도형그리기, 색칠하기, 저장하기, 글씨쓰기 등 다양한 기능들이 존재한다. 
- 이 수많은 기능 하나하나의 부품들을 어떻게 연결시켜 관계를 맺었는지 각각의 요소들간의 관계를 표현하는것 이라 할수있다.

소프트웨어를 구성하고있는 작은 기능 하나하나를 모듈 이라 하며
모듈을 기능별로 묶어 놓은 집합을 컴포넌트 라고 한다.

그리고 모듈, 컴포넌트 이 전체를 라이브러리라 한다.

소프트웨어 개발을 할때 이 모듈들을 어떻게 분할하고 배치할지 파일메뉴에는 어떤 기능을 넣을지 편집에는 어떤 기능을 넣을것인지 결정할때 참고하는것이 Software Architecture 이고 건물로 따지면 설계도 평면도에 해당하는 것이다.

소프트웨어 아키텍처 설계의 기본 원리에는 모듈화, 추상화, 단계전 분해, 은닉화 가 있다.
- **모듈화**: 소프트웨어 성능 향상 및 유지보수 등이 용이하도록 시스템의 기능을 모듈 단위로 나누는것.
- **추상화**: 전체적이고 포괄적인 개념을 설계한 후에 구체화시켜 나가는것.
- **단계전 분해**: 상위 개념부터 하위 개념으로 구체화 시키는 분할 기법 하향식 설계 전략.
- **은닉화**: 모듈 내부에 정보와 자료들을 숨겨서 다른 모듈이 접근하거나 수정 하도록 하는 기법.

## 디자인 패턴
디자인 패턴은 건축으로치면 공법에 해당하는 것으로 소프트웨어의 개발 방법을 공식화 한 것이다. 소수의 뛰어난 엔지니어가 해결한 문제를 다수의 엔지니어들이 처리 할 수 있도록 한 규칙이면서, 구현자들 간의 커뮤니케이션의 효율성을 높이는 기법이다.

## MVC (Model-View-Controller)
MVC란 Model View Controller의 약자로 에플리케이션을 세가지의 역할로 구분한 개발 방법론이다. 아래의 그림처럼 사용자가 Controller를 조작하면 Controller는 Model을 통해서 데이터를 가져오고 그 정보를 바탕으로 시각적인 표현을 담당하는 View를 제어해서 사용자에게 전달하게 된다. 

![MVC](https://user-images.githubusercontent.com/88717147/154671312-6ab82f4d-9752-4b2f-8d4f-e0b19e162fa4.png)

객체가 Model, View, Controller 중 한가지 역할을 담당하게 한다.
MVC패턴은 객체가 App에서 수행하는 역할을 정의할뿐만 아니라 객체가 서로 통신하는 방법을 정의 하게된다.

MVC유형의 객체 모음을 레이어 라고 하는 경우가 있다(Model-Layer)

MVC패턴을 채택할경우 재사용 가능성이 높고 인터페이스의 역할이 정의되며
MVC디자인을 가진 프로그램은 다른 프로그램보다 더 쉽게 확장할수 있다.

많은 Cocoa기술과 Architecture는 MVC 기반으로 만들어져있다.

### Model
- Model 객체는 특정한 데이터를 캡슐화하고 해당 데이터를 조작하고 처리하는 부분을 담당한다. 
- 예를 들어 게임의 캐릭터 또는 주소록의 연락처 를 담당한다. 
- Model 은 여러개가 존재할수 있다.
- 인터넷 과의 통신 되는 Model 은 View를 통해 생성되거나 업데이트 될수있다. Model 개체가 변경되면(네트워크 연결을 통해 새 데이터가 수신됨)Controller 객체에 알리고 적절한 View 객체를 업데이트 하여 보여주게된다.

### View
- View 는 사용자가 볼수 있는 객체이다.
- View 는 사용자의 작업에 응답할수있다.(App의 경우 터치,클릭 등)
- View 의 주목적은 Model 객체에서 데이터를 표시하고 해당 데이터를 편집할수 있도록 하는것이다.
- MVC 디자인 에서 View 객체는 일반적으로 Model 객체와 분리 된다.
- 통신되는 과정은 사용자가 view를 통해 입력을 하면 Controller 를 통해 변경사항을 Model 이 받게 되고 Model 은 변경사항을 Controller 에게 다시 전달해주며 Controller 는 Model에게 받은 업데이트된 정보를 View에게 전달해준다. 

### Controller
- Controller 는 하나 이상의 View객체와 하나 이상의 Model 객체 사이에서 중개자 역활을 하게된다. 
- Controller 객체는 View 객체가 Model 객체의 변경사항을 전달해줄수 있도록 하며, 반대로 View를 통해 입력,변경된것을 Model에게 전달해 데이터를 변경수정할수 있도록 한다.
- 통신과정은 Controller 객체는 View 객체에서 수행된 사용자 작업을 해석하고 새로운 데이터 또는 변경된 데이터를 Model 객체에 전달한다. Model 객체에서 새로운 데이터, 변경된 데이터를 Controller 에게 전달하여 View 객체에 전달하여 표시할수 있도록 해주게 된다.
- Controller 객체는 유형에 따라 재사용 가능하거나 불가능할수 있다.

### 역할 결합
- 객체가 Controller 와 View의 역할을 모두 수행할수있도록 할수 있다.
- 이경우엔 View-Controller 라고 한다.
- 같은 방식으로 Model과 Controller 를 결합해 Model-Controller 라고 하는 객체로 만들수도 있다.

#### Model-Controller
- Model-Controller 는 주로 Model-Layer 에 관련된 Controller 이다.
- Model 을 가지고있는 Controller 라고 이해하자.
- Model 을 관리하고 View 객체와 통신하는것을 목적으로 둔다.
- Model 에서 사용되던 메서드 들을 Model-Controller 에서 구현하게 된다.

#### View-Controller
- View-Controller 는 주로 View-Layer 와 관련된 Controller 이다.
- View 를 가지고 있는 Controller 라고 이해하자.
- View 를 관리하고 Model 과 통신하는것을 목적으로 둔다.
- View 에 표시되는 데이터와 관련된 메서드는 일반적으로 View-Controller 에서 구현된다.