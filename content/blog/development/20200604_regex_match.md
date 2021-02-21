---
title: 'python 정규식(regex) 초보자가 `김*한 == 김진한` 판별하기'
date: 2020-6-4 09:54:00
category: '정규식'
draft: true
---

### 서론

정규식의 길은 항상 멀고도 험했습니다. 간단한 것 같은데 검색을 해도 원하는 정보를 찾기가 쉽지가 않던지요. 그리고 설명이 되어 있는 사이트의 언어는 뭐가 그리 난해한지!

기본부터 해보자해서 유투브 강의를 찾아들었습니다. 귀에 쏙쏙 들어오는 엄청나게 쉬운 강의를 발견! 강의를 잘 들어보면서 정규식의 기초를 닦을 수 있었습니다. 내가 알고 있었던 정규식은 무엇이었던가. **정규식 초보자는 아래 강의를 꼭 들어보세요!**

<iframe id="ytplayer" type="text/html" width="640" height="360"
  src="https://www.youtube.com/embed/Gg0tlbrxJSc?autoplay=1"
  frameborder="0"></iframe>

### 본론으로 들어가서

위의 강의는 문장에 대한 패턴을 모두 찾습니다. 제가 필요한 건 한 단어에 대해서 맞느냐 틀리냐만 판별을 하는 것이어서 추가 검색을 조금 더 해서 정보를 찾았습니다.

결론부터 이야기하자면 김\*한 을 김.한으로 바꿔서 김진한과 맞는 패턴인지 매칭하면 됩니다. `match` 함수로 `true(object 반환)`, `false(null 반환)` 를 반환하도록 하시면 됩니다.

```python
# PYTHON 용
>>> import re
>>> re.match("김.한", "김진한")
<re.Match object; span=(0, 3), match='김진한'>
>>> re.match("김.한", "김민한")
<re.Match object; span=(0, 3), match='김민한'>
>>> re.match("김.한", "김진민")
>>>

## 좀 더 친절하게
name_check = re.match("김.한", "김진한")
if name_check:
    print("김*한이 김진한이 맞구나!!")
```

## Django 에서 regex 로 검색해보기

regex의 세계를 알고보니 새로운 세상이 열린것 같습니다. django를 기본 백엔드로 사용하고 있는데, djnago object filter 를 regex 로 할 수 있었더군요.

```python
User.objects.filter(name__regex="김.한")
```

이렇게 하면 이름이 김진한, 김명환, 김문환 이러신 분들 모두 소환됩니다.

## 참고자료

- [정규표현식의 개념과 패턴 사용법 총정리 · Wireframe](https://soooprmx.com/archives/7718) - 초보자시면 유투브 강의 듣고 한번 훑어보시면 조금 더 이해가 될 것 같네요.
- [RegExr: Learn, Build, & Test RegEx](https://regexr.com/) - regex 테스트 해보기 좋은 사이트, 위의 유투브 강의에서 사용됩니다.
