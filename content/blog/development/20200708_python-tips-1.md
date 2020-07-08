---
title: '파이썬 팁'
date: 2020-7-1 16:21:13
category: 'development'
draft: true
---

```
import inspect

def add(x, y):
return x + y

print("===============")
print(inspect.getsource(add))
```

```
import sys
from pprint import pprint

pprint(sys.path)

list1 = range(1,3)
list2 = range(4,6)
list3 = range(7,9)
from itertools import product

for i1,i2,i3 in product(list1, list2, list3):
    print(i1+i2+i3)
```

```
import time
import timeit

def run_sleedp(second):
    print(second)
    time.sleep(second)

print(timeit.timeit(lambda :run_sleedp(2), number=2))
```

출처: [Medium - Ten Python development skills - Towards Data Science](https://towardsdatascience.com/ten-python-development-skills-998a52f8f7c0)
