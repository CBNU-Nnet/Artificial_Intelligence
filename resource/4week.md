# Dive into Linear Regression

우리는 이제 딥러닝과 머신러닝에 대한 개념이 간략하게라도 잡혀있는 상태이다.
예전에 말했던 이야기 중에 **선형 회귀(Linear Regression)** 에 대해서 기억이 나는가?

오늘은 선형 회귀와 Tensorflow의 기본 변수형들을 집중적으로 파헤친 후에 선형회귀 코드를 리뷰하는 식으로 넘어가고자 한다. 

## 선형 분석(Regression Analysis)

먼저 2주차에 **선형 모델** 에 대해서 배웠던 것을 생각하자. 그리고 선형 분석을 알아보려고 한다.

선형 분석은영국의 유전학자 Francis Galton이 유전의 법칙을 연구하다 나온 것에 기인하게 된다. 연구의 내용은 **부모와 자녀의 키 사이의 관계** 였는데 연구결과로, 아버지와 어머니의 키의 평균을 조사하여 표로 나타낸 결과 자녀의 키는 엄청 크거나 작은 것이 아닌 그 세대의 평균으로 돌아가려는 경향이 있다는 것을 발견하였다.

Galton은 이를 **회귀 분석(Regression Analysis)** 라고 명칭하였다.

그렇다면? 머신러닝에서는 어떻게 회귀 분석 모델(= 선형 모델)을 적용하는 것일까?

우리는 이전에 수 많은 예시를 들어서 설명했다. 어느정도 집 값에 대한 내용이 있을 때 이런 평수일때는 집 값은 어떠할지? 3개월 내 채무이행할지? 안할지에 대한 내용들이 전부 선형 모델에 속한다.

일단, **선형 모델은 데이터의 분포가 하나의 선안에 표현될 수 있을 때 최적의 모델을 찾는다.**

그리고 **우리가 학습한 데이터를 통계로 어떤 임의의 점이 평면 상에 그려졌을 때 최적의 선형 모델(선)을 찾는 것이 목표이다.**

[그림1](https://t1.daumcdn.net/cfile/tistory/2277FC3859149DF40A)

이는 우리가 중고등학교때부터 배웠던 일차 함수의 그래프와 일치한다고 볼 수 있다.

[수식1](https://latex.codecogs.com/gif.latex?f%5Cleft%28%20x%20%5Cright%29%20%5Cquad%20%3D%5Cquad%20Ax%5Cquad%20&plus;%5Cquad%20b)

당연하게도 A는 선의 기울기를 나타내며, b는 선의 모양 축의 시작이라고 볼 수가 있다.

[그림2](https://t1.daumcdn.net/cfile/tistory/25047347589EC0592F)

이를 머신러닝에서 적용하는 수식으로 살펴보면 아래와 같다.

[수식2](https://latex.codecogs.com/gif.latex?H%5Cleft%28%20x%20%5Cright%29%20%5Cquad%20%3D%5Cquad%20Wx%5Cquad%20&plus;%5Cquad%20b)

여기서 H를 우리는 앞으로 **가설(Hypothesis)** 을 의미한다 할 것이며, W는 **가중치(Weight)** 라고 부르며, b는 **편향(bias)** 라고 말할 것이다.

편향(bias)말고도, 분산(variance)이라는 용어도 자주 보게 될텐데 
일단 아래와 같이 생각하고 추후에 얘기를 다시 하도록 하자.

> 예측값들과 정답이 대체로 멀리 떨어져 있으며, 결과의 편향(bias)가 높다고 말하고,
예측값들이 자기들끼리 대체로 멀리 흩어져있으면, 결과의 분산(variance)가 높다고 말한다.

이래도 잘 모르겠으면 아래 그림을 보자.

[그림3](https://lh3.googleusercontent.com/rq_iMVSuIK1K4ykF9RQnF05hH6xxWm3lmNPWuQ3hfK9r4-3GBIuCxCW3L7QH53M3EIwbVWOcaRiRLDc0AIJ-0uq8-qzavpSWPceQ1lchq-ZPF16l3KLst24-x6MbGYFqQbEJmEI3gEc)

다시 돌아와, 그림1의 가설은 H(w) = 1*x + 0으로 나타낼 수가 있다.
여기서 그려진 선이 **각 데이터의 분포와의 차이를 계산하여 가장 적은 것이 이 모델에 적합한 선** 이라는 것을 알 수가 있다. 그래서 이를 **비용함수(Cost Function)** 이라고 부른다.

이 비용함수를 통해서 우리가 세운 가설과 실제 나타내는 값이 얼마나 다른가를 가늠해볼 수가 있다.

비용함수는 아래와 같이 나타낸다.

[수식3](https://latex.codecogs.com/gif.latex?Cost%28W%2Cb%29%5Cquad%20%3D%5Cquad%20%5Cfrac%20%7B%201%20%7D%7B%20m%20%7D%20%5Csum%20_%7B%20i%3D1%20%7D%5E%7B%20m%20%7D%7B%20%28H%28%7B%20x%20%7D%5E%7B%20%28i%29%20%7D%29-y%5E%7B%20%28i%29%20%7D%29%5E%7B%202%20%7D%20%7D)


그렇다면? 이 비용함수의 식은 어떻게 도출됐을까??
예전에 내가 아주 단순하게 설명했던 방식이 기억날 것이다.

다 우리가 중고등학교때 했던 내용이다. L2 Distance(= 유클리드 Distnce)라고도 불리는 수식이 있다. 이 수식은 아래와 같다.

[그림5](https://wikimedia.org/api/rest_v1/media/math/render/svg/dc0281a964ec758cca02ab9ef91a7f54ac00d4b7)

어디서 많이 보던 수식아닌가? 바로 두 점 사이의 최소 거리를 구하는 방식이다.
여기서 루트만 벗기면 비용함수와 매우 흡사한 모습이란걸 알 수가 있다.

그렇다면 좀 더 깊게 들어가서 직관적으로 설명하기 위해 예시를 들어보겠다.

아래의 초록색 직선과 빨간색 직선 중 어느 모델이 더 예측을 잘할까?

[그림6](https://t1.daumcdn.net/cfile/tistory/996DB14C5B6BDA382C)


잘 모르겠다면, 아래의 문제는 어떠한가?

[그림7](https://t1.daumcdn.net/cfile/tistory/99379E395B6BDA3932)

많은 사람들이 왼쪽이 더 예측하기 쉽다한다 왜냐? 모든 점들이 직선 상에 존재하기 때문이다.

자, 이제 수치적으로 접근해보자. 

**우리가 예측하기 위해 만든 모델인 H(x) = Wx + b직선과 실제 데이터를 찍어놓은 점들의 y값의 차이를 error라 한다.**

**즉, 아래와 같이 점과 직선사이의 수직거리가 있어야 'error가 있다'라고 말할 수 있다.**

[그림8](https://t1.daumcdn.net/cfile/tistory/99E045425B6BDA3A2A)

다시 위의 그림의 왼쪽 초록색 직선을 보면 **에러가 전혀 없는 것** 을 확인할 수가 있다.그렇다면, 왼쪽 직선이 에러가 없으므로, **왼쪽 직선 모델이 더 낫다고 판단** 을 할 수가 있다.

이제 좀 더 수학적으로 계속 파고들어가보자.
error이외에 **squre error** 라는 것이 있다.

1. Error : 실제 데이터의 y값과 예측 직선모델의 y값의 차이
2. Squre Error : 실제 데이터의 y값과 예측 직선모델의 y값이 차이를 제곱해서 넓이로 보는 것.

[그림9](https://t1.daumcdn.net/cfile/tistory/99577C475B6BDA3A0C)

**Error를 제곱해서 넓이로 보는 이유가 무엇일까?**

1. 우리 눈으로 **직관적으로 볼 수가 있다.**
2. 수학적으로 볼 때, 에러가 조금이라도 있으면 **값이 증폭** 되어 큰값과 작은값의 비교를 수월하게 할 수가 있다.
3. 딥러닝 등의 알고리즘인 경사 하강법(Gradient Descent)와 오차 역전파(Backpropagation)개념에서 계산이 **용이하게 편미분** 된다.

다시 처음 문제로 돌아가 둘 중 어떤 모델이 더 나은 모델일까?

**선형회귀에서 어떤 모델이 나은지 확인하려면 Square Error 측면에서 확인해야한다.**

[그림10](https://t1.daumcdn.net/cfile/tistory/996A6D475B6BDA3B09)

Square Error를 구하고, 그것을 평균을 낸 Mean Square Error를 보면 왼쪽이 더 작다. 그러므로 왼쪽의 예측직선 모델이 예측을 잘함을 알 수가 있다.

[그림11](https://t1.daumcdn.net/cfile/tistory/9982BC335B6BDA3C24)


## Mean Sqaure Error와 Cost Function의 관계
이제 다시 본론으로 돌아와, 비용함수를 보자. 


아래와 같은 문제가 있다고 가정하자. 
어떤 문제인가? 관찰된 값들이 있을때(정답이 주어진 데이터가 존재), 가장 적합한 선(가장 적합한 선형모델)을 그으려면 어떻게 해야될까?

[그림12](https://t1.daumcdn.net/cfile/tistory/992744355B6BDA3D31)


위와 같은 경우 바로 **Linear Regression** 방식을 사용하는 것이다.
여기서 사용될 개념이 **최소 평균 제곱근 오차(Minimum Mean Square Error(MMSE))** 이다.

1. Error = H(x) - y : **예측값[H(X)](직선모델) - 실제값y(실제 데이터의 y값)**
2. Square Error = Error를 제곱한 값 = **(H(x) - y)^2**
3. Mean Square Error = Square Error를 다 더해서 n으로 나누어 평균낸 값 = **오차함수**

[수식3](https://latex.codecogs.com/gif.latex?Cost%28W%2Cb%29%5Cquad%20%3D%5Cquad%20%5Cfrac%20%7B%201%20%7D%7B%20m%20%7D%20%5Csum%20_%7B%20i%3D1%20%7D%5E%7B%20m%20%7D%7B%20%28H%28%7B%20x%20%7D%5E%7B%20%28i%29%20%7D%29-y%5E%7B%20%28i%29%20%7D%29%5E%7B%202%20%7D%20%7D)

이 개념(Mean Square Error : MSE)를 이용하여, **Best한 선형 모델을 그을 것이다.**

이 과정에서 사용되는 것이 **경사하강법(Gradient Descent)** 이다.
그리고 이 알고리즘을 사용하기 위해, 알아야할 개념인 **Cost Function이 Mean Square Error와 같은 것** 을 위에서 확인 할 수가 있다.
그리고 이 Cost Function을 **최소** 로 만드는 개념이 **Minimum Mean Square Error** 일 것이다. 

이제 어떻게? 비용함수를 최소로 만드는지 배워보자!

## 경사하강법(Gradient Descent)

정답이 주어진 데이터가 있을 때, 우리는 최적의 선형모델을 만들고 그 모델의 비용 함수를 최소로 만드는 최적의 직선을 찾아야한다. 그 비용을 최소로 하는 직선을 구하는 과정을 **학습(Train)** 이라하며, 학습에 사용되는 알고리즘이 **경사하강법** 이다.



