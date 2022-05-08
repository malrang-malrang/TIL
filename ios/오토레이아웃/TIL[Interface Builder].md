# Today I Learned

# **학습내용**
## Interface Builder 의 도구들

![](https://i.imgur.com/eAxdHcj.png)

1. Update Frame 메뉴
- 잘못 만져서 view 의 위치가 바뀌었을 경우 클릭시 현재 view 가 조건에 걸려있는 위치,크기로 적용된다.
2. Align 메뉴
- 엣지 들을 정렬 시켜줄수 있는 메뉴
- view 여러개를 Top Edges Align 로 묶어주게되면 같은 위치의 높이로 정렬되게 된다.(위치 뿐만아니라 조건에 따라 크기도 결정할수 있음.)(여러개가 관계를 맺게 됨.)
3. 핀(pin) 메뉴 
- view 간의 거리 혹은 사이즈를 정할수있는 메뉴
- Aspect Ratio 
    - view 의 크기를 비율로 정해주고 싶을 경우 사용되는듯하다.
4. reserve issue 메뉴
- 선택한 view와 모든 view 의 설정이 나뉘어져 있음.
- Add Missing Constraints
    - 부족한 조건들을 컴퓨터가 알아서 채워주겠다.(이놈 믿으면 안된다. 절대 절대)
- Reset to Suggested Constraints 
    - 얘도 위랑 비슷한데 쓰지말자.(믿을게 못됨.)
- Clear Constraints
    - view 에 설정된 조건을 모두 삭제 해줌.
5. Embed 메뉴
- 여러개의 view 를 묶어 하나의 틀에 넣어주고 싶은 경우 사용할수 있다.

해당 view 를 선택후 마우스 우클릭 으로 조건을 걸어 줄수 있다.
(옵션 키 누르게 되면 조건들이 다른 기능을 사용할수도 있음)
초보때는 Interface Builder 메뉴 에서 사용하도록 하자.

## Simple Constraints(간단한 샘플)

### Two Different-Width Views
예제1

![](https://i.imgur.com/Vq954wg.png)

위와 같은 뷰를 만들기 위해서는 어떻게 해야하는가?

2view 의 넓이 즉 x축 을 1view 의 2배만큼 만들어야하니 multiplier 의 값을 1 -> 2로 변경 해주면 가능하다.

![](https://i.imgur.com/mD4Qh4P.png)

이때 관계가 형성된 view의 경우 First Item 의 이름과 Second Item 의 이름이 모호하다면 이해하기 어려울수 있으니 이름을 변경 해두면 이해하기 편하다.

Interface Builder 에서 multiplier 를 사용하는 방법
- 2.0, 200%, 2/1, 2:1 와 같은 표현식을 된다.

예제2
![](https://i.imgur.com/jxlQBEE.png)

BlueView 가 최소한 150 포인트의 넓이 (x축) 를 가지고싶다.
RedView 의 넓이가 BlueView 의 두배 였으면 좋겠다.
REdview 넓이 가 BlueView 의 최소값 150을 유지한채 2배가 될수 없다면 최대한 2배 에 가까운 값만큼 늘어나라 라는것을 의미할수있다.

우선도를 정하면 가능할것같다.

먼저 BlueView 의 크기를 150과 크거나 같다 로 설정해준뒤
우선도를 RedView 는 BlueView 의 넓이보다 2배이다 라는 조건보다 높은 우선도를 주도록 설정 해주면된다.

![](https://i.imgur.com/syjZ0sr.png)

![](https://i.imgur.com/xD8nJHy.png)

