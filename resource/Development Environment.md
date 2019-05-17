# Windows 10 Python + Opencv 2 + Tensorflow 개발환경 세팅 

예전에 저희는 Anaconda를 설치를 했었습니다. 
이제는 Anaconda를 활용하여, 자신의 운영체제에 Python과 Opencv, Tensorflow 환경을 세팅해보도록 하겠습니다.

먼저, Anaconda Prompt 창을 띄웁니다.
그 후에 먼저, conda 자체를 업데이트 합니다.

> conda update -n base conda

다음에는 설치된 파이썬 패키지를 모두 최신 버전으로 업데이트합니다.

> conda update --all

그 후에 저희 스터디의 개발환경을 통일하기 위해서, 가상환경을 설치합니다.
※ 이때 주의하실 점은, AVX를 지원하지 않는 CPU를 사용하고 있다면, 1.5.0 버전을 설치하셔야합니다.

> conda create --name ai-study python=3.6.5

왜 버전을 3.6.5버전으로 하느냐? 이것이 텐서플로우에서 추천하는 파이썬 버전이기 때문입니다.그 후에 가상환경에서 파이썬 패키지 설치를 진행하기 위해서 아래와 같이 입력합니다.

> conda activate ai-study

자신의 bash prompt창 앞에 (ai-study)라는 문구가 붙어있다면, 가상환경에 성공적으로 진입하신겁니다.

그 후에 Tensorflow를 설치하겠습니다.

```{.bash}
#설치 
#CPU version (자신의 노트북에 외장 그래픽카드가 없을 시)
pip install --ignore-installed --upgrade tensorflow 

#GPU (GPU 버전은 따져야할 환경이 많기에 일단 CPU로 설치하고 추후에 재설명해드리도록 하겠습니다.)
pip install --ignore-installed --upgrade tensorflow-gpu 

#마지막으로 텐서플로우 실행이 제대로 되는지 확인 차원에서의 테스트 코드를 돌리기위해 ipython을 설치합니다.
pip install ipython 
```

그 후에 간단한 테스트를 진행합니다.

```{.bash}
ipython (타이핑 후 코드 입력)

import tensorflow as tf
hello = tf.constant('Hello, TensorFlow!')
sess = tf.Session()
print(sess.run(hello))
```

일단 여기까지가 간략한 개발환경 세팅이며, opencv와 연동 부분이나 상세한 설정부분은 따로 다시 올려서 공지해드리도록 하겠습니다.