# Today I Learned

# **학습내용**

[로그 확인 사이트 wtfautolayout](https://www.wtfautolayout.com/)

## Debugging Auto Layout
### Types of Errors(오류의 종류들)
- 충족되지 않은 Layout(Unsatisfiable Layout)
- 애매한 Layout(Ambiguous Layout)
- 논리적 오류(Logical Errors)

### 충족되지 않은 Layout(Unsatisfiable Layout)
조건이 모두 충족되지 않았을때.(지금 조건들로 크기와 위치를 특정할수 없을때.)
![](https://i.imgur.com/IYge8ke.png)
빨간거 누르면 무슨 조건이 부족한지 어떤 조건과 충돌이 일어나는지 알수있다.

충족되지 않은 레이아웃을 실행하면 다음과 일이 발생한다.
1. 충돌이 일어난 조건을 알아낸다.
2. 충돌이 일어난 조건들을 충돌이 일어나지 않을때까지 하나씩 제거해본다.
3. 어떤 조건이 잘못되었는지 로그를 남겨 개발자가 알수있게 해준다.

#### Unsatisfiable Layout 오류 가 일어나는 상황
- view 를 코드로 추가해주는 경우에 많이 발생한다.
- 부모 view 의 크기가 너무작을때 발생할수있다.

### 애매한 Layout(Ambiguous Layout)
중복된 조건이 있어 어떤걸 선택해야 할지 모를때(우선도를 조정해야한다.)

#### Ambiguous Layout 오류 가 일어나는 상황
두개이상의 가능성이 있을때.(이렇게 해도되고 저렇게 해도되고)

#### 디버깅 하기위해 사용할 메서드들
**lldb** 의 명령어를 알고있다면 훨씬더 수월하게 디버깅 할수있다.
- hasAmbiguousLayout
- exerciseAmbiguityInLayout
- constraintsAffectingLayoutForAxis
- constraintsAffectingLayoutForOrientation
- _autolayoutTrace


### 논리적 오류(Logical Errors)
사람의 논리적 오류.
생각을 잘못해서 코드, 오토레이아웃 을 잘못 작성,조작 한 경우

## Debugging Tricks and Tips(디버깅을 하기위한 기술과 팁)
Understanding the Logs(로그를 이해해봐라)
로그 읽는거 도움이 많이 된다.

[로그 확인 사이트 wtfautolayout](https://www.wtfautolayout.com/)

잘 모르겠으면 로그 복붙 해서 위의 사이트에 넣으면
현재 조건들이 어떻게 구성되어있는지 출력이 된다.

![](https://i.imgur.com/pHMoVpE.png)

코드에서 브레이크 포인트를 걸어주면 로그에서도 어떤 지점에서 브레이크 했는지 확인할수있다.

![](https://i.imgur.com/sWG18hZ.png)

위의 것들을 참고해서 어떻게 디버깅 할수 있을지 생각해 보아야 한다.

### Adding identifiers to the Logs
로그에 식별자를 넣어 활용해보자.

1. 방법
뷰의 요소중 Accessibility 에서 identifier 에 값을 넣게되면 identifier 에 작성한 값이 로그에 찍히게 된다.
로그를 쉽게 확인할 수 있도록 도와준다.
![](https://i.imgur.com/MdjMB3R.png)

![](https://i.imgur.com/7MIDT1n.png)

2. 방법
뷰의 조건 을 클릭후 identifier 에 값을 넣어 활용할수 있다.
조건에 식별자(identifier) 가 로그에 찍히기 때문에 보기 어려운 로그를 쉽게 확인할 수 있도록 도와준다.

![](https://i.imgur.com/mUBifR4.png)

### Visualizing Views and Constraints
뷰의 엣지와 조건을 눈으로 볼수 있도록 할수있다.

![](https://i.imgur.com/TIdYK5D.png)

`Debug -> View Debugging -> Show Alignment Rectangles`

시뮬레이터에서 뷰의 프레임을 확인할수 있게 된다.
콘솔창의 뷰 디버깅 툴을 사용하여 각각의 뷰에 걸려있는 조건을 확인할수도 있다.
