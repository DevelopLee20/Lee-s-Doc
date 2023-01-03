# Github 명령어 정리

## Git 초기 설정

### 초기 이름 설정

```bash
git config --global user.name "User Name"
```

- 깃허브 커밋(commit)시 사용될 이름
- 깃허브 유저명으로 넣으면 된다.

### 초기 이메일 설정

```bash
git config --global user.email "UserEmail@gmail.com"
```

- 깃허브 커밋(commit)시 사용될 이메일
- 깃허브 가입시 사용한 이메일을 넣으면 된다.

---

## 코드 백업하기

### 현재 작업공간의 수정하거나 생성한 모든 파일 트래킹하기

```bash
git add -A
```

### 현재 루트 내의 수정하거나 생성한 모든 파일 트래킹하기

```bash
git add .
```

### 트래킹한 파일 커밋하기

```bash
git commit -m "commit message"
```

- 커밋할 때의 메시지를 입력해주면 된다.

### 커밋한 내용 깃허브 레포지토리에 반영하기

```bash
git push
```

---

### 나의 작업공간을 레포지토리와 동일하기 만들기

```bash
git pull
```

---

## 깃허브로 협업하기

### 레포지토리 포크하기

- 포크하고 싶은 레포지토리에 가서 포크를 하고, 나의 레포지토리에 추가시킨다.

### 포크한 레포지토리를 클론(clone) 하기

```bash
git clone "클론한 나의 레포지토리 주소"
```

### 원본 리모트 추가하기

```bash
git remote add upstream "원본 레포지토리 주소"
```

- 이 작업을 마치면, 나의 레포지토리는 origin의 이름을 가지고, 원본 레포지토리는 upstream의 이름을 가진다.

---

## 협업 과정에서 코드 업데이트하기

### 원본 레포지토리(upstream)에서 수정된 내용 내 작업공간에 반영하기

```bash
git fetch upstream
```

### 만약 반영된 내용이 있다면

```bash
git merge fetch         # 또는
git merge fetch/main    # 또는
git merge fetch/master  # 을 입력한다.
```

### 모든 작업을 마친 후 Pull Request(PR) 보내기

- 협업 중 모든 작업을 마쳤다면, github에 들어가서 Pull Request를 만들어 원본 레포지토리에 수정을 요청한다.

---

## 발생한 에러 해결

### 작업공간 권한 문제(safe.directory)

#### 오류 코드

```bash
fatal: detected dubious ownership in reqository at "C:/"
```

#### 해결 코드
```bash
git config --global --add safe.directory C:/
```

- 저장소를 안전한 공간이라고 추가해주면 해결된다.
- 오류 코드 하단에 제안 사항에 해결 방안이 나와있기도 하다.

---
