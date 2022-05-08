# Today I Learned

# **학습내용**
## stack-view
스택뷰 를 이해하기 위해 알고있어야 하는 개념들
- compression Resistance (줄어들지 않으려고 버티는 힘)
- Content hugging (늘어나지 않으려고 버티는 힘)

스택뷰의 개념들
- axis
    - 가로(vertical), 세로(Horizontal)
- distribution
    - 스택뷰 내부의 요소들을 어떻게 분배할것인지 설정할수 있다.
- alignment
    - 위치에 대한 설정(정렬)
- spacing
    - 사이의 간격

### Stack-View Properties
#### distribution: UIStackView.Distribution
스택뷰 내부의 요소들을 어떻게 분배할것인지 설정할수 있다.

1. .fill
 ![](https://i.imgur.com/VH8G93F.png)

내부의 요소 뷰들을 최대한 늘릴것이다.
어떤애가 얼만큼 늘어날것인지 고민이 필요하다.

2. .fillEqually
![](https://i.imgur.com/sEK0g4y.png)

내부 요소 뷰들을 모두 같은 사이즈로 설정

3. .fillProportionally
![](https://i.imgur.com/YU36O2r.png)

자신의 콘텐츠 사이즈의 비율대로 채워주겠다.

4. .equalSpacing
![](https://i.imgur.com/lJ9Pu3E.png)

fill 과 비슷하게 동작함.
fill 에서 spacing 은 동일하게 가져가겠다.

5. .equalCentering
![](https://i.imgur.com/ld7MGCU.png)

각자 뷰의 중앙지점의 거리가 같다 라는뜻.
뷰가 4개라면 각 뷰의 중앙지점이 4개이며 각 중앙지점끼리의 거리가 모두 같게된다.


#### alignment: UIStackView.Alignment
위치에 대한 설정

1. .fill
![](https://i.imgur.com/mcm4H35.png)


현재 예시는 가로 StackView 이다.
axis 의 반대방향으로 꽉채우겠다 라는뜻.

2. .leading 
![](https://i.imgur.com/I3IPczq.png)

세로 스택뷰에서 설정하게되면 왼쪽(leading) 에 정렬되게 된다.

3. .top
![](https://i.imgur.com/DaPdATS.png)

가로 스택뷰일 경우 모든 뷰들이 top 에 맞춰 정렬 하게됨.

4. .firstBaseline
![](https://i.imgur.com/urEbVBW.png)

 텍스트가 들어있는 뷰 들이 있을경우
 첫째줄의 기준으로 위치를 정렬해준다.
 (예시 이미지를 보는게 이해가 빠르다.)

5. .center
![](https://i.imgur.com/49wS1Pt.png)

말그대로 중앙 정렬이다.

6. .traling
![](https://i.imgur.com/uSA8jOz.png)

leading 의 반대 
세로 스택에서 오른쪽으로 정렬해준다

7. .bottom
![](https://i.imgur.com/zsBxaEG.png)

가로 스택뷰에서 아래 bottom 에 맞게 정렬해준다.

8. .lastBaseline
![](https://i.imgur.com/Tfng5l7.png)

뷰의 콘텐츠 텍스트의 맨아랫줄을 기준으로 정렬해준다.

