---
title: '파이썬팁-product 와 timeit'
date: 2020-7-09 19:00:00
category: '파이썬'
draft: true
---

## 쉽게 다중 반복문을 만들 수 있도록 도와주는 product

product 에 대한 python 공식 문서를 찾아보니 아래와 같은 정의를 찾을 수 있었다.

> Functions creating iterators for efficient looping
> (효과적인 루핑(반복)을 하기 위해 이터레이터를 만드는 함수)

```
list1 = range(1,3)
list2 = range(4,6)
list3 = range(7,9)
from itertools import product

for i1 in list1:
    for i2 in list2:
        for i3 in list3:
            print(i1+i2+i3)
```

위와 같은 3중 중첩 for 문을 아래와 같이 간단하게 표현할 수 있다.

```
for i1,i2,i3 in product(list1, list2, list3):
    print(i1+i2+i3)
```

## 메소드의 러닝타임을 측정할 수 있는 timeit

time 으로도 메소드의 러닝타임을 측정할 수 있지만 번거롭게 앞과 뒤에 붙이고 빼줘야 하는데, 간단하게 메소드 자체의 러닝 타임을 측정할 수 있는 함수가 있었다. 람다로 실행시키고 반복 횟수를 정해주면 끝!

```
import time
import timeit

def run_sleedp(second):
    print(second)
    time.sleep(second)

print(timeit.timeit(lambda :run_sleedp(2), number=2))
```

## 참고자료

- [Medium - Ten Python development skills - Towards Data Science](https://towardsdatascience.com/ten-python-development-skills-998a52f8f7c0)
- [9.7. itertools — Functions creating iterators for efficient looping — Python 2.7.18 documentation](https://docs.python.org/2/library/itertools.html)o
