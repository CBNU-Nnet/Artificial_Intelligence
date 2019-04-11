# Stanford CS231n - Python Tutorial


이 튜토리얼은 Justin Johnson에 의해 공헌되었다.
우리는 이 코스에서 모든 과제를 위해 파이썬 프로그래밍 언어를 사용할 것이다.

파이썬은 그 스스로도 굉장한 범용 프로그래밍 언어지만, 몇가지 유명한 라이브러리(numpy, scipy, matplotlib)의 도움을 받으면 과학적 계산(scientific computing)을 위한 강력한 환경을 제공한다.


만약 당신이 Matlab에 대한 지식이 있다면, 우리는 여기를 추천한다. [numpy for Matlab users
](wiki.scipy.org/NumPy_for_Matlab_Users).

당신은 Volodymyr Kuleshov와 Isaac Caswell에 의해 쓰여진 [IPython notebook](https://github.com/kuleshov/cs228-material/blob/master/tutorials/python/cs228-python-tutorial.ipynb)을 참고할 수 있다.


## 파이썬 Python

파이썬은 고 수준의 동적 타입 프로그래밍 언어이다. 파이썬 코드는 자주 의사코드(pseudocode)와 같다고 사람들은 말한다. 이것은 몇줄의 코드로 아이디어를 표현할 수 있으며, 매우 읽기 쉽다. 예를 들어, 파이썬으로 quicksort 알고리즘을 표현하면 다음과 같이 간단하게 표현할 수 있다.

```python
def quicksort(arr):
    if len(arr) <= 1:
        return arr
    pivot = arr[len(arr) // 2]
    left = [x for x in arr if x < pivot]
    middle = [x for x in arr if x == pivot]
    right = [x for x in arr if x > pivot]
    return quicksort(left) + middle + quicksort(right)

print(quicksort([3,6,8,10,1,2,1]))
# Prints "[1, 1, 2, 3, 6, 8, 10]"

```

이것을 C로 구현을 해보자면
```c
#include <stdio.h>
#include <stdlib.h>
#include <malloc/malloc.h>

int *arr;
int len;

void swap(int arr[], int lIdx, int rIdx){
  int temp = arr[lIdx];
  arr[lIdx] = arr[rIdx];
  arr[rIdx] = temp;
}

int Partition(int arr[], int left, int right){
  int pivot = arr[left];
  int low = left+1;
  int high = right;

  while(low <= high){
    while(pivot > arr[low] && low <= right)
    low++;

    while(pivot < arr[high] && high >= (left)+1)
    high--;

    if(low <= high){
      swap(arr, low, high);
    }
  }

  swap(arr, left, high);
  return high;
}

void QuickSort(int arr[], int left, int right){
  if(left <= right){
  int pivot = Partition(arr, left, right);
  QuickSort(arr, left, pivot-1);
  QuickSort(arr, pivot+1, right);
  }
}
  // ... 중략 ... 
```

이런식으로 많이 복잡해진다. 
위와 같이 파이썬은 간단하게 짤 수 있다는 강력한 장점이 있다.

## 파이썬 버젼

파이썬은 2.7과 3.X의 다른 버젼이 현재 존재함. 3.0 버젼부터 이전 버젼과 호환되지 않는 변화가 도입되었다. 그래서 2.7에서 작성된 코드는 3.X 에서 작동하지 않고, 그 역(vice versa)도 마찬가지이다. 이 클래스에서 모든 코드는 파이썬 3.5를 사용하였다. 


너는 `python --version` 파이썬 작동중에 옆에 명령어로 파이썬 버젼을 체크할 수 있다.


## 기본 자료 타입(Basic data types)

대부분의 언어처럼, 파이썬도 정수(integers), 부동소수점(floats), 불리언(booleans, 참 거짓)을 포함하는 기본 타입의 숫자가 있고, 문자열(strings)이 있다. 이들 자료 타입들은 다른 프로그래밍 언어와 유사하다.

### 숫자형(Numbers): 정수(Integers)와 부동소수점(floats)는 너가 다른 언어에서 기대하는 것과 같게 작동한다.

```python
x = 3
print(type(x)) # Prints "<type 'int'>"  int  타입을 표시함
print(x)       # Prints "3"  3이 표시됨
print(x + 1)  # Addition; prints "4"   더해져서 4가 표시됨
print(x - 1)  # Subtraction; prints "2"   빼져서 2가 표시됨
print(x * 2)   # Multiplication; prints "6"   곱해져서 6이 표시됨
print(x ** 2)  # Exponentiation; prints "9"   제곱되어 9가 표시됨
x += 1
print(x)  # Prints "4"   할당연산자 x = x+1 과 같음 4가 표시됨
x *= 2
print(x)  # Prints "8"   할당연산자 x= x*2 와 같음 8이 표시됨
y = 2.5
print(type(y)) # Prints "<type 'float'>"    float 타입이  표시됨
print(y, y + 1, y * 2, y ** 2) # Print  "2.5 3.5 5.0 6.25"가 표시됨
```

다른 언어와 달리, 파이썬은 **단항 증가(x++)이나 감소(x--)연산자**를 갖지 않고 있다. 


파이썬은 또한 long 형태의 정수와 복소수 형태를 갖음. 더 자세한 내용은 [다음 문서](https://docs.python.org/3.5/library/stdtypes.html#numeric-types-int-float-complex)를 참조하면 된다.


### 불리언(Booleans): 파이썬은 Boolean 논리를 위한 일반적인 연산자를 모두 갖지만, (&&, ||, 기타 등등) 과 같은 기호보다는 영어 단어를 사용한다.

```python
t = True
f = False
print(type(t)) # Prints "<type 'bool'>"
print(t and f) # Logical AND; prints "False"
print(t or f)  # Logical OR; prints "True"
print(not t)   # Logical NOT; prints "False"
print(t != f)  # Logical XOR; prints "True" 
```

### 문자열(Strings): 파이썬은 문자열을 위한 굉장한 지원이 있다.

```python
hello = 'hello'   #  따옴표를 사용해 문자열 상수를 만들 수 있음
world = "world"   #  쌍따옴표를 사용해도 됨
print(hello)       # Prints "hello"
print(len(hello))  # 문자열의 길이 5가 표시됨
hw = hello + ' ' + world  #  문자열 결합은 '+' 연산자로 가능
print(hw)  # prints "hello world"   공백이 결합되어 표시됨
hw12 = '%s %s %d' % (hello, world, 12)  # sprintf  스타일의 문자열 형식화 뒤에 인자가 딕셔너리형태로 들어간다고 생각하면 됨
print(hw12)  # prints "hello world 12"
```

문자열 객체는 유용한 메소드를 갖고 있다. 예를 들면,

```python
s = "hello"
print(s.capitalize())  #  문자열의 첫글자를 대문자로 함 "Hello"가 표시됨
print(s.upper())       #  문자열을 전부 대문자료 바굼. "HELLO"가 표시됨
print(s.rjust(7))      #  오른쪽부터 문자의 길이를 맞춤 문자 길이를  넘으면 공백을 더해줌  "  hello"  가 표시됨
print(s.center(7))     #  공백을 더해 문자열을 가운데정렬 해줌   "  hello "   가 표시됨
print(s.replace('l', '(ell)'))  # 첫번째 인자를 모두 찾아서 두번째 인자로 바꿔줌.
                               #  "he(ell)(ell)o"   가 표시됨
print('  world '.strip())  # strip 메소드는 앞뒤에 따라오는 공백을 제거해줌  "world" 가 표시됨
```

파이썬의 모든 문자열 메소드를 보고싶다면, [다음 문서](https://docs.python.org/3.5/library/stdtypes.html#string-methods)에서 찾을 수 있다. 


## 컨테이너(Containers)

파이썬은 몇가지 내장 컨테이너 타입을 포함하고 있다. 
1. 리스트(lists)
2. 딕셔너리(dictionaries)
3. 집합(sets)
4. 튜플(tuples)


### 리스트(Lists)

리스트는 배열과 동일하다. 
그러나 변경가능하고, 다른 타입의 요소를 포함할 수 있다.

```python
xs = [3, 1, 2]   # 리스트는 []를 통해 만들어짐
print(xs, xs[2])  # "[3, 1, 2] 2"가 표시됨
print(xs[-1])     #  음수는 리스트의 끝에서부터 찾게됨 "2" 가 표시됨
xs[2] = 'foo'    #  리스트는 다른 타입의 요소를 가질 수 있음
print(xs)         #  "[3, 1, 'foo']" 가 표시됨
xs.append('bar') # append 메소드는 리스트의  끝에 새로운 요소를 추가함
print(xs)         # Prints "[3, 1, 'foo', 'bar']" 가 표시됨
x = xs.pop()     #  pop 메소드는 가장 마지막 요소를 제거하고 난 리스트를 반환함
print(x, xs)      # Prints "bar [3, 1, 'foo']"
```

리스트에 대한 더 자세한 내용은 [다음](https://docs.python.org/3.5/tutorial/datastructures.html#more-on-lists)을 참조할 수 있다.


슬라이싱(Slicing): 한번에 한 리스트 요소에 접근하는 것보다, 파이썬은 서브리스트에 접근할 수 있는 문법을 제공하는데 이것을 슬라이싱(slicing)이라한다.

```python
nums = range(5)    # range 는 내장함수로서 정수의 리스트를 생성함
print(nums)         # Prints "[0, 1, 2, 3, 4]"
print(nums[2:4])    # 인덱스 2부터 4'미만' 까지를 찾아줌 "[2, 3]" 가 표시됨
print(nums[2:])     # : 뒤에 아무것도 없으면 끝 인덱스까지 표시함 "[2, 3, 4]"
print(nums[:2])     # 처음에 아무것도 없으면 처음 인덱스0 부터  2 '미만' 까지 표시함 "[0, 1]"
print(nums[:])      #  전체 리스트를 "복사" 하게 됨 ["0, 1, 2, 3, 4]"
print(nums[:-1])    # 음수 인덱스를 사용할 수 있음 ["0, 1, 2, 3]"
nums[2:4] = [8, 9] # 슬라이싱을 사용하여 새로운 서브리스트를 그 위치에 할당할 수 있음
print(nums)         # Prints "[0, 1, 8, 9, 4]"
```
우리는 슬라이싱을 numpy 배열에서 다시 볼 것이다.


### 반복문(Loops): 다음과 같이 리스트의 요소를 반복시킬 수 있다.

```python
animals = ['cat', 'dog', 'monkey']
for animal in animals:
    print(animal)
# 각 줄에 "cat", "dog", "monkey", 가 표시됨 print문장을 3번 쓰는 것과 동일함. animals 리스트에서 요소 하나하나씩을 꺼내게 됨
```

만약 너가 반복문 내에서 각 요소가 아닌 각 요소의 인덱스에 접근하고 싶다면 내장함수인 `enumerate`를 사용하면 된다.

```python
animals = ['cat', 'dog', 'monkey']
for idx, animal in enumerate(animals):
    print('#%d: %s' % (idx + 1, animal))
# 각줄에  "#1: cat", "#2: dog", "#3: monkey" 가 표시됨. 문자열 형식화로 인덱스와  요소를 같이 표시하고, for 문에서  enumerate가 인덱스와 요소를 튜플로 반환하기 때문에 두개의 변수로 각각 할당하게 할 수 있음 
```

### 리스트 컴프리헨션(List comprehensions): 프로그래밍 할 때, 우리는 하나의 자료 타입을 다른 것으로 바꾸고 싶어한다. 다음과 같은 간단한 예제를 통해, 주어진 숫자 리스트를 그 숫자의 제곱 리스트으로 바꾸는 것을 생각해보자.

아래의 예시를 보자.

```python
nums = [0, 1, 2, 3, 4]
squares = []
for x in nums:
    squares.append(x ** 2)
print(squares)   # Prints [0, 1, 4, 9, 16]
```

이것을 리스트 컴프리헨션으로 표현하면 다음과 같이 더 간단해진다.

```python
nums = [0, 1, 2, 3, 4]
squares = [x ** 2 for x in nums]
print(squares)   # Prints [0, 1, 4, 9, 16]
```

리스트 컴프리헨션은 조건을 포함할 수도 있다.

```python
nums = [0, 1, 2, 3, 4]
even_squares = [x ** 2 for x in nums if x % 2 == 0]
print(even_squares)  # Prints "[0, 4, 16]"
```

### 사전형(Dictionaries)
딕셔너리는 (키, 값) 쌍으로 저장한다. Java에서 Map 이나 Javascript에서 객체와 유사하다. 다음과 같이 사용할 수 있다.

```python
d = {'cat': 'cute', 'dog': 'furry'}  # 딕셔너리은 {} 형태로 만들어지고, 키와 값의 쌍은 ':' 로 구분되며, 각 키들은 ','로 구분된다.
print(d['cat'])       # 딕셔너리의 인덱스로 키를 넣으면 값을 반환한다. 그래서 키 cat의 값인 "cute" 가 반환된다.
print('cat' in d)    #  in 연산자로 딕셔너리내에 키 값을 검색할 수 있으며, cat 키가 있는지를 검사했고, "True" 을 반환한다.
d['fish'] = 'wet'    # 새로운 키와 값을 할당할 수도 있다. 이는 딕셔너리형태 내의 특수 메소드인 __setitem__를 호출한 결과와 같다.
print(d['fish'])      # 키 fish의 값 "wet" 를 반환한다.
# print d['monkey']  # KeyError: 'monkey'  가 d의 키 값으로 존재하지 않기 때문에 keyerror 가 나온다.
print(d.get('monkey', 'N/A'))  # 기본값과 함께 키를 찾을 수 있다. 없기 때문에 기본값 "N/A" 이 반환된다.
print(d.get('fish', 'N/A'))   #  fish 키는 딕셔너리내에 존재하기 때문에 기본값이 아니라 값 "wet" 을 반환한다.
del d['fish']        # 딕셔너리의 키값을 del 명령어로 삭제할 수 있다.
print(d.get('fish', 'N/A')) # "fish가 더이상 딕셔너리의 키로 존재하지 않기 때문에 기본값  "N/A" 를 반환한다.
```

딕셔너리에 대한 더 자세한 내용은 [다음의 문서](https://docs.python.org/3.5/library/stdtypes.html#dict)를 참조할 수 있다.


### 반복문(Loops): 딕셔너리에서 반복문은 키를 반복한다.

```python
d = {'person': 2, 'cat': 4, 'spider': 8}
for animal in d: 
    legs = d[animal]
    print('A %s has %d legs' % (animal, legs))
# 반복문에서 딕셔너리의 키를 반복하고, legs에 딕셔너리의 값을 넣어 문자열 형식화를 사용해 다음이 표시됨 "A person has 2 legs", "A spider has 8 legs", "A cat has 4 legs"
```

딕셔너리의 키와 값을 한번에 접근하고 싶다면 `iteritems` 메소드를 사용한다.

```python
d = {'person': 2, 'cat': 4, 'spider': 8}
for animal, legs in d.iteritems():
    print('A %s has %d legs' % (animal, legs))
#  반복문에서  iteritems 메소드가 키와 값의 튜플을 반환함으로 animal, legs에 다중할당 할 수 있고, 이를 문자열 형식화를 사용해 다음이 표시됨 "A person has 2 legs", "A spider has 8 legs", "A cat has 4 legs"
```

딕셔너리 컴프리헨션(Dictionary comprehensions): 리스트 컴프리헨션과 유사하다. 그러나 쉽게 딕셔너리를 구성할 수 있다.

```python
nums = [0, 1, 2, 3, 4]
even_num_to_square = {x: x ** 2 for x in nums if x % 2 == 0} 
print(even_num_to_square)  # Prints "{0: 0, 2: 4, 4: 16}" 컴프리헨션의 표현(expresion)이 딕셔너리의 형태임
```

### 집합(Sets)

집합은 중복되지 않은(distinct) 요소의 순서 없는(unordered) 모임이다. 

```python
animals = {'cat', 'dog'}
print('cat' in animals)   #   집합내에 cat 요소를 검색하고, 존재하기 때문에 "True" 를 표시함
print('fish' in animals)  # fish는 없기 때문에 "False" 를 표시함
animals.add('fish')      #  add 메소드로 요소를 추가할 수 있음
print('fish' in animals)  # Prints "True"
print(len(animals))      # 집합의 길이 요소의 갯수 "3" 을 표시함
animals.add('cat')       # 이미 집합에 존재하는 요소일 경우에는 추가되지 않음
print(len(animals))       # Prints "3"
animals.remove('cat')    # remove 메소드로 집합내의 요소를 제거할 수 있음
print(len(animals))       # 요소의 갯수가 "2" 가 됨
```

집합에 대한 더 자세한 내용은 다음을 [참조](https://docs.python.org/3.5/library/stdtypes.html#dict)할 수 있다.


### 반복문(Loops): 리스트의 반복문과 문법은 같다. 그러나 집합은 순서가 없기 때문에 접근하는 요소의 순서대로 반복된다.

```python
animals = {'cat', 'dog', 'fish'}
for idx, animal in enumerate(animals):
    print('#%d: %s' % (idx + 1, animal))
# enumerate로 인덱스와 요소의 튜플을 반환하려 했지만, fish가 1번으로 나타난다. "#1: fish", "#2: dog", "#3: cat"
```

### 집합 컴프리헨션(Set comprehensions): 리스트나 딕셔너리와 같다.

```python
from math import sqrt
nums = {int(sqrt(x)) for x in range(30)}
print(nums)  # Prints "set([0, 1, 2, 3, 4, 5])"
```

### 튜플(Tuples)
튜플은 변경할수 없는(immutable) 순서있는 리스트이다. 튜플은 리스트와 많은 점이 유사하지만, 튜플은 변경되지 않기 때문에 딕셔너리의 키로 사용할 수도 있다는 점과, 집합의 요소가 될 수 있다는 것이다. 리스트는 불가능하다.

```python
d = {(x, x + 1): x for x in range(10)}  # 튜플은 () 로[사실은 ,로] 만들어지고, expression에서 딕셔너리의 키로 사용되었다.
t = (5, 6)       # 튜플은 ()로 사실은 , 로 만들어짐
print(type(t))    # Prints "<type 'tuple'>"
print(d[t])       #  딕셔너리의 키 5,6을 검색하니 값으로 "5"  가 나타남
print(d[(1, 2)])  # 딕셔너리의 키로 1,2를 검색하니 값으로  "1" 이 나타남
```

튜플에 대한 더 자세한 내용은 [다음 문서](https://docs.python.org/3.5/tutorial/datastructures.html#tuples-and-sequences)를 참조할 수 있다.


### 함수(Functions)

파이썬의 함수는 def 키워드를 사용해 정의된다.

```python
def sign(x):
    if x > 0:
        return 'positive'
    elif x < 0:
        return 'negative'
    else:
        return 'zero'

for x in [-1, 0, 1]:
    print(sign(x))
# Prints "negative", "zero", "positive"
```

우리는 함수의 인자의 기본값을 주어 정의할 수도 있다.

```python
def hello(name, loud=False):
    if loud:
        print('HELLO, %s!' % name.upper())
    else:
        print('Hello, %s' % name)

hello('Bob') #  loud는 기본값으로 False가 들어가서 함수가 작동한다. "Hello, Bob"
hello('Fred', loud=True)  # 함수의 loud 인자로 True를 넣어주었기 때문에 "HELLO, FRED!" 가 표시된다.
```

함수에 대한 더 자세한 설명은 [다음 문서](https://docs.python.org/3.5/tutorial/controlflow.html#defining-functions)를 참조할 수 있다.


## 클래스(Classes)
파이썬에서 클래스를 정의하는 문법은 단순하다.

```python
class Greeter(object):

    # Constructor
    def __init__(self, name):
        self.name = name  # 클래스의 함수를 메소드라 부르고 메소드의 첫번째 인자는 언제나 인스턴트를 참조한다.

    # Instance method
    def greet(self, loud=False):
        if loud:
            print('HELLO, %s!' % self.name.upper())
        else:
            print ('Hello, %s' % self.name)

g = Greeter('Fred')  # g는 Greeter 클래스의 인스턴트이다.
g.greet()            # g의 인스턴트 메소드 greet를 실행하여 "Hello, Fred" 가 표시된다.
g.greet(loud=True)   # g의 인스턴트 메소드 greet를 실행하여  "HELLO, FRED!" 가 표시된다.
```

파이썬 클래스의 더 자세한 내용은 [다음 문서](https://docs.python.org/3.5/tutorial/classes.html)를 참조할 수 있다.
