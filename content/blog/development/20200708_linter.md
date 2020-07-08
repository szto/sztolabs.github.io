---
title: '두말 없는 python django 코드포멧터'
date: 2020-7-1 16:21:13
category: 'development'
draft: true
---

함께 개발을 하다보면 코드의 형식이 달라지기 쉽습니다. 몇 가지 방법을 찾아보다 현재까지 가장 좋은 방법론을 공유합니다.

- [black](https://github.com/psf/black): 최근 파이썬 커뮤니티에서 가장 널리 쓰이고 있는 코드 포멧터라고 하네요.
- [flake8](https://flake8.pycqa.org/en/latest/): lint 검사기로 에러 발생하기 쉬운 표시에 flag 를 달아주고 명령어 실행 시 포멧이 잘못된 케이스를 에러로 발생시킨다.
- [isort](https://github.com/timothycrosley/isort): import 스타일 체커입니다.
- [mypy](http://mypy-lang.org/): static 타입을 체크해 주는 라이브러리입니다.
- [pytset](): 기본 테스트 라이브러리를 pytest 로 적용
- [coverage](): 테스트 시 커버리지 사용 (코드를 얼마나 테스트가 커버했는가 확인)

저는 `Makefile`을 만들어서 git push를 하기 전에 검사를 하도록 하고 있습니다.

```
(프로젝트 root 에 있는 makefile)
lint:
	isort
	black . --config myproject.toml
	flake8
	# mypy . # mypy 는 추후 적용

lint-test:
	isort --check-only
	flake8
	# mypy . # mypy 는 추후 적용

test: lint-test
	pytest --cov-report term-missing --cov

# make push r=feature/something
push: test
	git push origin "$r"

test-only:
	pytest --cov-report term-missing --cov
```

위와 같이 make 파일을 만들고

1. 코드를 레포에 푸쉬할 때 `make push r=feature/repo(레포명)`

   - 위와 같이 명령어를 입력하면 린트를 테스트하고, 유니테스트를 모두 통과한 뒤에야 git에 push를 합니다. 이 테스트가 통과하지 않는다면 push 자체를 하지 않습니다.
   - 만약 린트 쪽에 에러가 났다면 `make lint`로 에러를 수정할 수 있습니다. black하고 isort는 코드 변경을 강제합니다. `flake8` 은 문법에 대한 에러만 처리하므로 직접 수정을 해 주셔야 합니다.
   - 이렇게 함으로써 개발팀 모두의 코드포멧을 맞출 수 있습니다.
   - 개발 중간에 적용하려니 flake8 에러가 엄청 나서 수정하느라 고생. ㅜㅜ
   - 프로젝트 시작부터 이렇게 설정을 해 놓고 시작하면 좋을것 같네요.

2. `make lint`: `black`과 `isort`가 코드포멧팅과 import 포멧팅을 해주고, `flake8`검사를 진행합니다. 이 때 유의할 점은 vscode 등을 사용해서 `black`으로 코드 포멧팅을 하고 있다면, line 수를 맞춰주셔야 합니다. 아니면 계속 전체가 바뀌는 대참극이 벌어집니다.

3. `make lint-test`: 이 명령어는 사용을 안해도 됩니다. test 시 lint 를 check 합니다. 통과가 안 된다면 테스트는 진행되지 않습니다.

4. `make test`: lint를 검사하고 통과하면 pytest를 실행시킵니다 coverage report 도 함께 만들어줍니다. cicd 파이프라인에서 build 할 때 이 명령어를 실행시키도록 하면 좋을것 같네요.

5. `make push`: `r=feature/repo`를 추가로 git에 push 할 때에도 lint 검사 완료, unit test 완료를 해야 push 가 가능합니다. cicd 파이프라인에서 체크가 되지 않은 코드를 build 하느라 쓰는 리소스를 최대한 줄일 수 있습니다.

6. `make test-onlly`: 린트는 필요 없다. 테스트만 하겠다 할 때는 이 명령어를 실행시키면 됩니다.

makefile 에 대한 설명은 이제 마치겠습니다.

위와 같이 코드포메팅을 할 경우 추가로 설정을 해 줘야 하는 부분들이 있습니다.
