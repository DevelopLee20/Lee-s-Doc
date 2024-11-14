# 요기요 멘토링 내용 정리

## 개발 방식

### 외부 API Client

> 이니시스는 결제에서 만큼은 절대 피하자...

- 외부 API를 불러와야할 경우 Client를 만들어, 따로 캡슐화해 관리하는 것이 좋다.

- 머신의 프로세스 개수를 세서 worker를 생성 후 병렬로 동시에 실행한다.
- 기존 테스트 코드 도는 속도보다 향상된 속도를 기대할 수 있다.

### poetry를 사용한 파이썬 패키지 관리

```bash
poetry run
poetry shell
poetry install
```

- poetry는 파이썬 패키지 명세 툴이다.
- 명세가 변경되었을 경우 "poetry install"을 통해 설치해야 적용된다.

### 테스트 주도 개발 순서

1. 기능 명세(최대한 작은 단위로 기능을 나눠서 진행)
2. 구현할 기능의 알고리즘 문서화 및 컨펌
3. 테스트 코드 작성
4. 코드 구현
5. PR 작성

### 개발 후 배포시 주의 사항

- 무조건 롤백을 고려하여야 한다.
- 롤백을 위해서 단계별로 작업을 진행해야 한다.
  1. 데이터베이스 테이블 or 콜랙션 수정
  2. 관련 코드 마이그레이션
  3. 알고리즘 수정
  4. 추가해야할 코드 추가

### 시퀀스 다이어그램 Mermaid

```Mermaid
sequenceDiagram
    autonumber
    Client->>Toss: 정기 결제 빌링키 발급 요청
    Toss-->>Client: 빌링 키 발급
    Client->>Server: 빌링키를 전송 (POST /subscription)
    Server->>PortOne: 출금 요청
    PortOne-->>Server: 출금 응답
    Server-->>Client: 정기결제 수행 후 결과 리턴
```

- Mermaid가 자동으로 그림을 그려준다.

### Exception 발생

- 기본적으로 서비스 레이어가 아닌 라우터에서 오류를 발생시키는 것이 재사용성에 좋다.
- 배치 파일 Exception시 전체 배치를 하나의 try except으로 catch를 통해 에러와 로그를 남긴다.
- 실제 배치를 실패했을 때, catch로 로그를 남기고 꼭 re raise를 발생시켜야 함!

## 테스트 코드 작성

- 테스트 코드는 멍청하게 짜야 한다. (너무 멍청하면 안된다.)
- 모킹은 절대적으로 적게 쓰는 것이 좋다.
- 테스트 코드는 완전성을 충족해야 한다.
  - 완전성이란? 코드의 given, when, then으로 테스트에 대한 모든 정보가 눈에 보여야 한다.
- assert 문은 then 문에서만 사용한다. (다른 곳에서는 잘 되었다는 전제로 진행)
- teardown(실행한 dump를 삭제한는 것)은 하지 않는 것이 좋다.
- 코드 일부를 주석처리했을 때 에러가 발생하면, 테스트를 매우 잘 작성한 것이다.

### Xdist 테스트 코드 병렬 처리

```bash
pip install pytest-xdist
```

```bash
pytest . -n auto --asyncio-mode=auto
```

### 테스트 실행시 collection Error

- test 실행시 테스트에 필요한 종속성을 모으는 과정이 존재한다.
- 이때 필요한 종속성이 발견되지 않았을 때, collection Error가 발생한다.

### 테스트 코드 작성시 freeze_time으로 시간 제한 없애기

```python
from freezegun import freeze_time

with freeze_time("2024-03-04"):
  pass
```

- options의 {verify_exp: False}를 설정하면 exp를 무시할 수 있다.

## 개발 용어

### 당직

> 당직은 자는 중에도 연락 또는 전화를 받을 수 있어야 한다. 만약 국제전화로 연락이 왔다면 아주 큰 문제가 발생했다는 것...

### API 타임아웃

- 타임아웃은 API 호출하는 과정에서 기본값이 설정되어 있다 하더라도 명시적으로 적어주는 것이 좋다.
- Connect Timeout: TCP Handshake하는 과정에서 발생한 timeout
  - TCP Handshake 과정에서의 속도를 향상시키기 위해 Connection pool을 사용하기도 한다.
  - Timeout 발생시 retry를 해도 괜찮다.
- Read Timeout: 클라이언트가 데이터를 모두 전송한 후, 서버에서 데이터를 읽는 중 발생한 timeout
  - 데이터가 이미 전달되었기 때문에, retry를 하면 절대 안되는 타임아웃이다. (결제 호출이 2번되어 2번 결제할 수도 있다.)

## Database 및 SQL 관련

> 2024/09 기준 Postgre는 대세가 아니다.

### MongoDB

- MongoDB의 _id는 시간을 기준으로 생성되기 때문에 생성시간을 대신할 수 있다.

#### MongoDB의 INDEX

> INDEX는 자료형으로 정렬된 BTREE 인덱스를 의미.

- BTREE의 유지 비용과 쿼리 조회 시간을 비교해 INDEX로 사용할지 결정
- COLLSCAN은 인덱스 하지 않고 쿼리를 사용 중
- 그러나 INDEXSCAN은 인덱스를 사용하는 쿼리 -> 실무에서 이 방식이 정상이다.
- COMPOUND INDEX: 다중 컬럼에 대한 인덱스를 작성할 수 있음 - 필터링 순서가 중요하다.
- MongoDB는 Explain으로 쿼리 소요 시간을 확인 가능
- MongoDB의 아틀라스(유료버전)은 Search Index(본문의 의미 없는 내용도 인덱싱 해줌)를 지원
- 리버스 인덱싱이란 본문 단어를 쪼개서 인덱싱으로 본문을 가리킴

### 테이블 및 콜랙션 구축

- 기록을 남기는 테이블과 현재 상태를 저장하는 테이블은 꼭 구분 되어야 한다.

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

- 기본적으로 시작, 종료 시간과 성공 및 실패 이유를 로깅함

### Timed Rotating File Handler

[타임드 로테이트 파일 핸들러](https://docs.python.org/3/library/logging.handlers.html#timedrotatingfilehandler)

- 정해진 시간에 슬랙이나 디스코드로 로깅되도록 할 수 있음

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

### 딕셔너리로 데이터를 관리하지 않는 이유

- 딕셔너리는 실행하기 전까지 어떤 데이터를 가진지 모른다.
- 반면에 클래스는 어떤 데이터를 가진지 코드를 보고 바로 확인 가능하다.

### asyncio의 gather 사용

```python
from asyncio import gather

async def function1():
  pass

async def function2():
  pass

await gather(await function1(), function2())
```

- gather는 두 개 이상의 의존성 없는 await 문을 동시에 처리 가능하다.
- latency(지연시간)를 절약할 수 있다.
- asyncio 실행 방식
  - gather는 덱(deque)에서 실행됨
  - 덱이 이벤트 루프에게 실행권한 양도
  - 이벤트 루프는 ready(덱)에 있는 모든 내용을 진행함
  - ready -> await 실행 -> 스케줄(ready로 돌아가는지를 검사)

### __init__.py

- 기본으로 설정해야하는 코드를 작성
- 데이터베이스의 INDEX를 사용할 때에는 이 파일에 인덱싱을 정의해 주어야 함

## 이력서 관련 내용

> 중소기업은 당장의 인력이 필요해서 개발 경력을 중요하게 본다.

### 프로젝트 작업시 중요한 점

1. 개선해야할 점 생각
2. 어떻게 할지 결정 및 구현
3. 결과를 수치로 나타내야 한다.

### 이력서에 기입할 커리어

- 프로젝트 단위로 개발된 내용이 있으면 좋다.
- 만약 상을 수상했다면 프로젝트 단위가 아니더라도 증명 가능

### 백엔드 개발시 이력서에 적으면 좋을 내용들

- CI 경험
- FastAPI 및 프레임워크 개발 경험
- MongoDB 및 DBMS 경험
- INDEX와 쿼리 플랜 경험(특정 쿼리가 얼만큼 오버헤드가 발생하는지 확인)

## 적어놓고 기억이 안나는 내용

- 컴플로언서
- 코드 오너
