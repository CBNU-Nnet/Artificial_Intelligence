# Stanford CS231n - Numpy Tutorial

## Numpy

[Numpy](http://www.numpy.org/)는 파이썬이 계산과학분야에 이용될때 핵심 역할을 하는 라이브러리입니다.
Numpy는 고성능의 다차원 배열 객체와 이를 다룰 도구를 제공합니다. 만약 MATLAB에 익숙한 분이라면 Numpy 학습을 시작하는데 있어
[이 튜토리얼](http://wiki.scipy.org/NumPy_for_Matlab_Users)이 유용할 것입니다.

<a name='numpy-arrays'></a>

### 배열
Numpy 배열은 동일한 자료형을 가지는 값들이 격자판 형태로 있는 것입니다. 각각의 값들은 튜플(이때 튜플은 양의 정수만을 요소값으로 갖습니다.) 형태로 색인 됩니다.
*rank*는 배열이 몇 차원인지를 의미합니다; *shape*는 는 각 차원의 크기를 알려주는 정수들이 모인 튜플입니다.

파이썬의 리스트를 중첩해 Numpy 배열을 초기화 할 수 있고, 대괄호를 통해 각 요소에 접근할 수 있습니다:

~~~python
import numpy as np

a = np.array([1, 2, 3])  # rank가 1인 배열 생성
print type(a)            # 출력 "<type 'numpy.ndarray'>"
print a.shape            # 출력 "(3,)"
print a[0], a[1], a[2]   # 출력 "1 2 3"
a[0] = 5                 # 요소를 변경
print a                  # 출력 "[5, 2, 3]"

b = np.array([[1,2,3],[4,5,6]])   # rank가 2인 배열 생성
print b.shape                     # 출력 "(2, 3)"
print b[0, 0], b[0, 1], b[1, 0]   # 출력 "1 2 4"
~~~

리스트의 중첩이 아니더라도 Numpy는 배열을 만들기 위한 다양한 함수를 제공합니다.

~~~python
import numpy as np

a = np.zeros((2,2))  # 모든 값이 0인 배열 생성
print a              # 출력 "[[ 0.  0.]
                     #       [ 0.  0.]]"

b = np.ones((1,2))   # 모든 값이 1인 배열 생성
print b              # 출력 "[[ 1.  1.]]"

c = np.full((2,2), 7) # 모든 값이 특정 상수인 배열 생성
print c               # 출력 "[[ 7.  7.]
                      #       [ 7.  7.]]"

d = np.eye(2)        # 2x2 단위행렬 생성
print d              # 출력 "[[ 1.  0.]
                     #       [ 0.  1.]]"

e = np.random.random((2,2)) # 임의의 값으로 채워진 배열 생성
print e                     # 임의의 값 출력 "[[ 0.91940167  0.08143941]
                            #                  [ 0.68744134  0.87236687]]"
~~~
배열 생성에 관한 다른 방법들은 [문서](http://docs.scipy.org/doc/numpy/user/basics.creation.html#arrays-creation)를 참조하세요.

<a name='numpy-array-indexing'></a>

### 배열 인덱싱
Numpy는 배열을 인덱싱하는 몇 가지 방법을 제공합니다.

**슬라이싱:**
파이썬 리스트와 유사하게, Numpy 배열도 슬라이싱이 가능합니다. Numpy 배열은 다차원인 경우가 많기에, 각 차원별로 어떻게 슬라이스할건지 명확히 해야 합니다:

~~~python
import numpy as np

# Create the following rank 2 array with shape (3, 4)
# [[ 1  2  3  4]
#  [ 5  6  7  8]
#  [ 9 10 11 12]]
a = np.array([[1,2,3,4], [5,6,7,8], [9,10,11,12]])

# 슬라이싱을 이용하여 첫 두 행과 1열, 2열로 이루어진 부분배열을 만들어 봅시다;
# b는 shape가 (2,2)인 배열이 됩니다:
# [[2 3]
#  [6 7]]
b = a[:2, 1:3]

# 슬라이싱된 배열은 원본 배열과 같은 데이터를 참조합니다, 즉 슬라이싱된 배열을 수정하면
# 원본 배열 역시 수정됩니다.
print a[0, 1]   # 출력 "2"
b[0, 0] = 77    # b[0, 0]은 a[0, 1]과 같은 데이터입니다
print a[0, 1]   # 출력 "77"
~~~

정수를 이용한 인덱싱과 슬라이싱을 혼합하여 사용할 수 있습니다.
하지만 이렇게 할 경우, 기존의 배열보다 낮은 rank의 배열이 얻어집니다.
이는 MATLAB이 배열을 다루는 방식과 차이가 있습니다.

슬라이싱:

~~~python
import numpy as np

# 아래와 같은 요소를 가지는 rank가 2이고 shape가 (3, 4)인 배열 생성
# [[ 1  2  3  4]
#  [ 5  6  7  8]
#  [ 9 10 11 12]]
a = np.array([[1,2,3,4], [5,6,7,8], [9,10,11,12]])

# 배열의 중간 행에 접근하는 두 가지 방법이 있습니다.
# 정수 인덱싱과 슬라이싱을 혼합해서 사용하면 낮은 rank의 배열이 생성되지만,
# 슬라이싱만 사용하면 원본 배열과 동일한 rank의 배열이 생성됩니다.
row_r1 = a[1, :]    # 배열a의 두 번째 행을 rank가 1인 배열로
row_r2 = a[1:2, :]  # 배열a의 두 번째 행을 rank가 2인 배열로
print row_r1, row_r1.shape  # 출력 "[5 6 7 8] (4,)"
print row_r2, row_r2.shape  # 출력 "[[5 6 7 8]] (1, 4)"

# 행이 아닌 열의 경우에도 마찬가지입니다:
col_r1 = a[:, 1]
col_r2 = a[:, 1:2]
print col_r1, col_r1.shape  # 출력 "[ 2  6 10] (3,)"
print col_r2, col_r2.shape  # 출력 "[[ 2]
                            #       [ 6]
                            #       [10]] (3, 1)"
~~~

**정수 배열 인덱싱:**
Numpy 배열을 슬라이싱하면, 결과로 얻어지는 배열은 언제나 원본 배열의 부분 배열입니다.
그러나 정수 배열 인덱싱을 한다면, 원본과 다른 배열을 만들 수 있습니다.
여기에 예시가 있습니다:

~~~python
import numpy as np

a = np.array([[1,2], [3, 4], [5, 6]])

# 정수 배열 인덱싱의 예.
# 반환되는 배열의 shape는 (3,)
print a[[0, 1, 2], [0, 1, 0]]  # 출력 "[1 4 5]"

# 위에서 본 정수 배열 인덱싱 예제는 다음과 동일합니다:
print np.array([a[0, 0], a[1, 1], a[2, 0]])  # 출력 "[1 4 5]"

# 정수 배열 인덱싱을 사용할 때,
# 원본 배열의 같은 요소를 재사용할 수 있습니다:
print a[[0, 0], [1, 1]]  # 출력 "[2 2]"

# 위 예제는 다음과 동일합니다
print np.array([a[0, 1], a[0, 1]])  # 출력 "[2 2]"
~~~

정수 배열 인덱싱을 유용하게 사용하는 방법 중 하나는 행렬의 각 행에서 하나의 요소를 선택하거나 바꾸는 것입니다:

~~~python
import numpy as np

# 요소를 선택할 새로운 배열 생성
a = np.array([[1,2,3], [4,5,6], [7,8,9], [10, 11, 12]])

print a  # 출력 "array([[ 1,  2,  3],
         #             [ 4,  5,  6],
         #             [ 7,  8,  9],
         #             [10, 11, 12]])"

# 인덱스를 저장할 배열 생성
b = np.array([0, 2, 0, 1])


# b에 저장된 인덱스를 이용해 각 행에서 하나의 요소를 선택합니다
print a[np.arange(4), b]  # 출력 "[ 1  6  7 11]"

# b에 저장된 인덱스를 이용해 각 행에서 하나의 요소를 변경합니다
a[np.arange(4), b] += 10

print a  # 출력 "array([[11,  2,  3],
         #             [ 4,  5, 16],
         #             [17,  8,  9],
         #             [10, 21, 12]])
~~~

**불리언 배열 인덱싱:**
불리언 배열 인덱싱을 통해 배열 속 요소를 취사선택할 수 있습니다.
불리언 배열 인덱싱은 특정 조건을 만족하게 하는 요소만 선택하고자 할 때 자주 사용됩니다.
다음은 그 예시입니다:

~~~python
import numpy as np

a = np.array([[1,2], [3, 4], [5, 6]])

bool_idx = (a > 2)  # 2보다 큰 a의 요소를 찾습니다;
                    # 이 코드는 a와 shape가 같고 불리언 자료형을 요소로 하는 numpy 배열을 반환합니다,
                    # bool_idx의 각 요소는 동일한 위치에 있는 a의
                    # 요소가 2보다 큰지를 말해줍니다.

print bool_idx      # 출력 "[[False False]
                    #       [ True  True]
                    #       [ True  True]]"

# 불리언 배열 인덱싱을 통해 bool_idx에서
# 참 값을 가지는 요소로 구성되는
# rank 1인 배열을 구성할 수 있습니다.
print a[bool_idx]  # 출력 "[3 4 5 6]"

# 위에서 한 모든것을 한 문장으로 할 수 있습니다:
print a[a > 2]     # 출력 "[3 4 5 6]"
~~~

튜토리얼을 간결히 하고자 numpy 배열 인덱싱에 관한 많은 내용을 생략했습니다.
조금 더 알고싶다면 [문서](http://docs.scipy.org/doc/numpy/reference/arrays.indexing.html)를 참조하세요.

<a name='numpy-datatypes'></a>

### 자료형
Numpy 배열은 동일한 자료형을 가지는 값들이 격자판 형태로 있는 것입니다.
Numpy에선 배열을 구성하는 데 사용할 수 있는 다양한 숫자 자료형을 제공합니다.
Numpy는 배열이 생성될 때 자료형을 스스로 추측합니다, 그러나 배열을 생성할 때 명시적으로 특정 자료형을 지정할 수도 있습니다. 예시:

~~~python
import numpy as np

x = np.array([1, 2])  # Numpy가 자료형을 추측해서 선택
print x.dtype         # 출력 "int64"

x = np.array([1.0, 2.0])  # Numpy가 자료형을 추측해서 선택
print x.dtype             # 출력 "float64"

x = np.array([1, 2], dtype=np.int64)  # 특정 자료형을 명시적으로 지정
print x.dtype                         # 출력 "int64"
~~~
Numpy 자료형에 관한 자세한 사항은 [문서](http://docs.scipy.org/doc/numpy/reference/arrays.dtypes.html)를 참조하세요.

<a name='numpy-math'></a>

### 배열 연산
기본적인 수학함수는 배열의 각 요소별로 동작하며 연산자를 통해 동작하거나 numpy 함수모듈을 통해 동작합니다:

~~~python
import numpy as np

x = np.array([[1,2],[3,4]], dtype=np.float64)
y = np.array([[5,6],[7,8]], dtype=np.float64)

# 요소별 합; 둘 다 다음의 배열을 만듭니다
# [[ 6.0  8.0]
#  [10.0 12.0]]
print x + y
print np.add(x, y)

# 요소별 차; 둘 다 다음의 배열을 만듭니다
# [[-4.0 -4.0]
#  [-4.0 -4.0]]
print x - y
print np.subtract(x, y)

# 요소별 곱; 둘 다 다음의 배열을 만듭니다
# [[ 5.0 12.0]
#  [21.0 32.0]]
print x * y
print np.multiply(x, y)

# 요소별 나눗셈; 둘 다 다음의 배열을 만듭니다
# [[ 0.2         0.33333333]
#  [ 0.42857143  0.5       ]]
print x / y
print np.divide(x, y)

# 요소별 제곱근; 다음의 배열을 만듭니다
# [[ 1.          1.41421356]
#  [ 1.73205081  2.        ]]
print np.sqrt(x)
~~~

MATLAB과 달리, '*'은 행렬 곱이 아니라 요소별 곱입니다. Numpy에선 벡터의 내적, 벡터와 행렬의 곱, 행렬곱을 위해 '*'대신 'dot'함수를 사용합니다. 'dot'은 Numpy 모듈 함수로서도 배열 객체의 인스턴스 메소드로서도 이용 가능한 합수입니다:

~~~python
import numpy as np

x = np.array([[1,2],[3,4]])
y = np.array([[5,6],[7,8]])

v = np.array([9,10])
w = np.array([11, 12])

# 벡터의 내적; 둘 다 결과는 219
print v.dot(w)
print np.dot(v, w)

# 행렬과 벡터의 곱; 둘 다 결과는 rank 1인 배열 [29 67]
print x.dot(v)
print np.dot(x, v)

# 행렬곱; 둘 다 결과는 rank 2인 배열
# [[19 22]
#  [43 50]]
print x.dot(y)
print np.dot(x, y)
~~~

Numpy는 배열 연산에 유용하게 쓰이는 많은 함수를 제공합니다. 가장 유용한 건 'sum'입니다:

~~~python
import numpy as np

x = np.array([[1,2],[3,4]])

print np.sum(x)  # 모든 요소를 합한 값을 연산; 출력 "10"
print np.sum(x, axis=0)  # 각 열에 대한 합을 연산; 출력 "[4 6]"
print np.sum(x, axis=1)  # 각 행에 대한 합을 연산; 출력 "[3 7]"
~~~
Numpy가 제공하는 모든 수학함수의 목록은 [문서](http://docs.scipy.org/doc/numpy/reference/routines.math.html)를 참조하세요.

배열연산을 하지 않더라도, 종종 배열의 모양을 바꾸거나 데이터를 처리해야 할 때가 있습니다.
가장 간단한 예는 행렬의 주 대각선을 기준으로 대칭되는 요소끼리 뒤바꾸는 것입니다; 이를 전치라고 하며 행렬을 전치하기 위해선, 간단하게 배열 객체의 'T' 속성을 사용하면 됩니다:

~~~python
import numpy as np

x = np.array([[1,2], [3,4]])
print x    # 출력 "[[1 2]
           #          [3 4]]"
print x.T  # 출력 "[[1 3]
           #          [2 4]]"

# rank 1인 배열을 전치할 경우 아무 일도 일어나지 않습니다:
v = np.array([1,2,3])
print v    # 출력 "[1 2 3]"
print v.T  # 출력 "[1 2 3]"
~~~
Numpy는 배열을 다루는 다양한 함수들을 제공합니다; 이러한 함수의 전체 목록은 [문서](http://docs.scipy.org/doc/numpy/reference/routines.array-manipulation.html)를 참조하세요.


<a name='numpy-broadcasting'></a>

### 브로드캐스팅
브로트캐스팅은 Numpy에서 shape가 다른 배열 간에도 산술 연산이 가능하게 하는 메커니즘입니다. 종종 작은 배열과 큰 배열이 있을 때, 큰 배열을 대상으로 작은 배열을 여러 번 연산하고자 할 때가 있습니다. 예를 들어, 행렬의 각 행에 상수 벡터를 더하는 걸 생각해보세요. 이는 다음과 같은 방식으로 처리될 수 있습니다:

~~~python
import numpy as np

# 행렬 x의 각 행에 벡터 v를 더한 뒤,
# 그 결과를 행렬 y에 저장하고자 합니다
x = np.array([[1,2,3], [4,5,6], [7,8,9], [10, 11, 12]])
v = np.array([1, 0, 1])
y = np.empty_like(x)   # x와 동일한 shape를 가지며 비어있는 행렬 생성

# 명시적 반복문을 통해 행렬 x의 각 행에 벡터 v를 더하는 방법
for i in range(4):
    y[i, :] = x[i, :] + v

# 이제 y는 다음과 같습니다
# [[ 2  2  4]
#  [ 5  5  7]
#  [ 8  8 10]
#  [11 11 13]]
print y
~~~

위의 방식대로 하면 됩니다; 그러나 'x'가 매우 큰 행렬이라면, 파이썬의 명시적 반복문을 이용한 위 코드는 매우 느려질 수 있습니다. 벡터 'v'를 행렬 'x'의 각 행에 더하는 것은 'v'를 여러 개 복사해 수직으로 쌓은 행렬 'vv'를 만들고 이 'vv'를 'x'에 더하는것과 동일합니다. 이 과정을 아래의 코드로 구현할 수 있습니다:

~~~python
import numpy as np

# 벡터 v를 행렬 x의 각 행에 더한 뒤,
# 그 결과를 행렬 y에 저장하고자 합니다
x = np.array([[1,2,3], [4,5,6], [7,8,9], [10, 11, 12]])
v = np.array([1, 0, 1])
vv = np.tile(v, (4, 1))  # v의 복사본 4개를 위로 차곡차곡 쌓은 것이 vv
print vv                 # 출력 "[[1 0 1]
                         #       [1 0 1]
                         #       [1 0 1]
                         #       [1 0 1]]"
y = x + vv  # x와 vv의 요소별 합
print y  # 출력 "[[ 2  2  4
         #       [ 5  5  7]
         #       [ 8  8 10]
         #       [11 11 13]]"
~~~

Numpy 브로드캐스팅을 이용한다면 이렇게 v의 복사본을 여러 개 만들지 않아도 동일한 연산을 할 수 있습니다.
아래는 브로드캐스팅을 이용한 예시 코드입니다:

~~~python
import numpy as np

# 벡터 v를 행렬 x의 각 행에 더한 뒤,
# 그 결과를 행렬 y에 저장하고자 합니다
x = np.array([[1,2,3], [4,5,6], [7,8,9], [10, 11, 12]])
v = np.array([1, 0, 1])
y = x + v  # 브로드캐스팅을 이용하여 v를 x의 각 행에 더하기
print y  # 출력 "[[ 2  2  4]
         #       [ 5  5  7]
         #       [ 8  8 10]
         #       [11 11 13]]"
~~~

`x`의 shape가 `(4, 3)`이고 `v`의 shape가 `(3,)`라도 브로드캐스팅으로 인해 `y = x + v`는 문제없이 수행됩니다;
이때 'v'는 'v'의 복사본이 차곡차곡 쌓인 shape `(4, 3)`처럼 간주되어 'x'와 동일한 shape가 되며 이들 간의 요소별 덧셈연산이 y에 저장됩니다.

두 배열의 브로드캐스팅은 아래의 규칙을 따릅니다:

1. 두 배열이 동일한 rank를 가지고 있지 않다면, 낮은 rank의 1차원 배열이 높은 rank 배열의 shape로 간주합니다.
2. 특정 차원에서 두 배열이 동일한 크기를 갖거나, 두 배열 중 하나의 크기가 1이라면 그 두 배열은 특정 차원에서 *compatible*하다고 여겨집니다.
3. 두 행렬이 모든 차원에서 compatible하다면, 브로드캐스팅이 가능합니다.
4. 브로드캐스팅이 이뤄지면, 각 배열 shape의 요소별 최소공배수로 이루어진 shape가 두 배열의 shape로 간주합니다.
5. 차원에 상관없이 크기가 1인 배열과 1보다 큰 배열이 있을 때, 크기가 1인 배열은 자신의 차원 수만큼 복사되어 쌓인 것처럼 간주합니다.

설명이 이해하기 부족하다면 [scipy문서](http://docs.scipy.org/doc/numpy/user/basics.broadcasting.html)나 [scipy위키](http://wiki.scipy.org/EricsBroadcastingDoc)를 참조하세요.

브로드캐스팅을 지원하는 함수를 *universal functions*라고 합니다.
*universal functions* 목록은 [문서](http://docs.scipy.org/doc/numpy/reference/ufuncs.html#available-ufuncs)를 참조하세요.

브로드캐스팅을 응용한 예시들입니다:

~~~python
import numpy as np

# 벡터의 외적을 계산
v = np.array([1,2,3])  # v의 shape는 (3,)
w = np.array([4,5])    # w의 shape는 (2,)
# 외적을 계산하기 위해, 먼저 v를 shape가 (3,1)인 행벡터로 바꿔야 합니다;
# 그다음 이것을 w에 맞춰 브로드캐스팅한뒤 결과물로 shape가 (3,2)인 행렬을 얻습니다,
# 이 행렬은 v와 w 외적의 결과입니다:
# [[ 4  5]
#  [ 8 10]
#  [12 15]]
print np.reshape(v, (3, 1)) * w

# 벡터를 행렬의 각 행에 더하기
x = np.array([[1,2,3], [4,5,6]])
# x는 shape가 (2, 3)이고 v는 shape가 (3,)이므로 이 둘을 브로드캐스팅하면 shape가 (2, 3)인
# 아래와 같은 행렬이 나옵니다:
# [[2 4 6]
#  [5 7 9]]
print x + v

# 벡터를 행렬의 각 행에 더하기
# x는 shape가 (2, 3)이고 w는 shape가 (2,)입니다.
# x의 전치행렬은 shape가 (3,2)이며 이는 w와 브로드캐스팅이 가능하고 결과로 shape가 (3,2)인 행렬이 생깁니다;
# 이 행렬을 전치하면 shape가 (2,3)인 행렬이 나오며
# 이는 행렬 x의 각 열에 벡터 w을 더한 결과와 동일합니다.
# 아래의 행렬입니다:
# [[ 5  6  7]
#  [ 9 10 11]]
print (x.T + w).T
# 다른 방법은 w를 shape가 (2,1)인 열벡터로 변환하는 것입니다;
# 그런 다음 이를 바로 x에 브로드캐스팅해 더하면
# 동일한 결과가 나옵니다.
print x + np.reshape(w, (2, 1))

# 행렬의 스칼라배:
# x 의 shape는 (2, 3)입니다. Numpy는 스칼라를 shape가 ()인 배열로 취급합니다;
# 그렇기에 스칼라 값은 (2,3) shape로 브로드캐스트 될 수 있고,
# 아래와 같은 결과를 만들어 냅니다:
# [[ 2  4  6]
#  [ 8 10 12]]
print x * 2
~~~

브로드캐스팅은 보통 코드를 간결하고 빠르게 해줍니다, 그러니 가능한 많이 사용하세요.

### Numpy Documentation
이 문서는 여러분이 numpy에 대해 알아야할 많은 중요한 사항들을 다루지만 완벽하진 않습니다.
numpy에 관한 더 많은 사항은 [numpy 레퍼런스](http://docs.scipy.org/doc/numpy/reference/)를 참조하세요.
