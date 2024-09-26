# 요기요 멘토링 내용 정리

## 개발 방식

### 외부 API Client

> 이니시스는 결제에서 만큼은 절대 피하자...

- 외부 API를 불러와야할 경우 Client를 만들어, 따로 캡슐화해 관리하는 것이 좋다.

### 테스트 코드

1. 테스트 코드는 멍청하게 짜야 한다. (너무 멍청하면 안된다.)
2. 모킹은 절대적으로 적게 쓰는 것이 좋다.

- 테스트 코드는 완전성을 충족해야 한다.
  - 완전성이란? 코드의 given, when, then으로 테스트에 대한 모든 정보가 눈에 보여야 한다.
- assert 문은 then 문에서만 사용한다. (다른 곳에서는 잘 되었다는 전제로 진행)
- teardown(실행한 dump를 삭제한는 것)은 하지 않는 것이 좋다.

### Xdist 테스트 코드 병렬 처리

```bash
pip install pytest-xdist
```

- 머신의 프로세스 개수를 세서 worker를 생성 후 병렬로 동시에 실행한다.
- 기존 테스트 코드 도는 속도보다 향상된 속도를 기대할 수 있다.

### API 타임아웃

- 타임아웃은 API 호출하는 과정에서 기본값이 설정되어 있다 하더라도 명시적으로 적어주는 것이 좋다.
- Connect Timeout: TCP Handshake하는 과정에서 발생한 timeout
  - TCP Handshake 과정에서의 속도를 향상시키기 위해 Connection pool을 사용하기도 한다.
  - Timeout 발생시 retry를 해도 괜찮다.
- Read Timeout: 클라이언트가 데이터를 모두 전송한 후, 서버에서 데이터를 읽는 중 발생한 timeout
  - 데이터가 이미 전달되었기 때문에, retry를 하면 절대 안되는 타임아웃이다. (결제 호출이 2번되어 2번 결제할 수도 있다.)

## 개발 용어

### 당직

> 당직은 자는 중에도 연락 또는 전화를 받을 수 있어야 한다. 만약 국제전화로 연락이 왔다면 아주 큰 문제가 발생했다는 것...

## SQL 관련

> 2024/09 기준 Postgre는 대세가 아니다.

## 리눅스 명령어 관련

### python3 실행파일 위치 찾는 방법

```bash
which python3
```

## 터미널 관련

### Oh my zshell

- <https://ohmyz.sh/>
- git을 사용할 때 도움을 받을 수 있는 터미널 테마
- 명령어 실행시간이 나오도록 커스텀 가능

## Github 관련

### Github action

- 조건부(push 시 등등)에 스크립트를 작동하도록 할 수 있다.

### Github stash

- 브랜치 작업 중 잠깐 저장하고 싶은 변경사항이 있다면 보관할 수 있다.
- stash 로 저장
- stash pop 으로 저장한 내용 불러오기
- JetBrain 사에서는 "쉘브" 라는 동일한 기능을 지원한다.

## logging 관련

> logging은 print보다 오버헤드가 적다. 그 이유는 logging은 file에 write 하고, print는 terminal에 write 하기 때문이다. 또한 terminal에 write 하는 작업이 시간도 오래 걸린다.

## PyCharm 관련

### 맥 기준 단축키 모음

```text
command + 7     # 구조 확인
```

### configuration

- configuration? 작업 디렉토리, 환경변수 정보 등등 관리 해준다.

> configuration을 수정해도 config 관련 내용이 수정되지 않을 때, configuration template를 수정하도록 하자. 왜냐하면 지속적으로 configuration은 업데이트/삭제를 반복하기 때문에 수정한 내용이 삭제될 수 있기 때문이다.

### Column select

- column select를 사용하고 드래그 후 사용하면, 모든 라인을 동시에 수정 가능하다.

## pre-commit 관련

> 현업에서 rebase 충돌시 pre-commit 이 시간을 지체시키는 문제가 있다.

### Black

- 파일 포메터, 협업시 코드 스타일을 하나로 만들어준다.
- PEP8을 준수함
- Python Enhancement Proposals란? 파이썬으로 논쟁하는 커뮤니티, 의견이 맞으면 파이썬의 다음 버전으로 업데이트 됨

### mypy

- 코드의 타입 힌팅(hint)을 관리한다.
- strict = True 옵션이란? list 내부 value도 비교한다.
- 정적으로 문제를 찾아 해결할 수 있다.

## Python 코드 관련

### 튜플 Tuple

```python
tup1 = tuple(10)
tup2 = (10, )
print(tup1 == tup2) # True
```

- 튜플은 ()로 선언할 때 , 를 사용해야 한다. 이유는 그냥 괄호랑 헷갈리기 때문이다.

### 타입 힌팅 Type hinting ... 사용법

```python
def function(numbers: list[str, ...]) -> list[str, ...]:
    return numbers
```

- ...를 활용해 입력되는 list와 출력되는 list의 요소 제한을 없앴다.

### collection 이란?

- list, tuple, set 을 의미한다.

### a == b == c

```python
a = False
b = False
c = False

print(a == b == c) # True
```

- 위의 연산자는 a, b, c가 모두 같은지 비교한다.

> a, b가 우선 연산되어 True 반환, True == False로 False가 나올 것이라고 예상했겠지만...

### True, False와 값 비교

```python
# 일반적인 방법
assert a is True
assert b == 10
```

- 이렇게 하는 이유는 파이썬은 "==" 연산자를 오버라이딩 할 수 있기 때문이다.

> 이상하게 오버라이딩 하면 "a는 무조건 False 반환" 이런식으로 가능하기 때문이다.
