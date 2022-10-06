# 안드로이드 스튜디오 앱에 광고 넣기

## 1. Layout 설정

```txt
1. 광고의 위치를 먼저 설정한다.
layout 설정
```

## 2. 함수 추가

```txt
1. build gradle project -> allprojects -> 에
google()과 jcenter()가 있는지 파악
2. 만약 없으면 추가한다.
```

## 3. 코드 작성

```txt
1. build gradle app -> 에 implemetation 코드 추가
- implementation 코드는 사이트에서 주어진다.
2. Sync now 클릭
```

## 4. 메타데이터 추가 ( 애드몹 아이디 필요 )

```txt
1. AndroidManifest.xml -> meta-data를 application 사이에 추가한다.
- 이 코드는 사이트에서 주어진다.
2. Admob 아이디 입력
```

## 5. 코드 추가

```txt
1. MainActivity -> 에 import로 MoblieAds 코드를 추가한다.
- 이 코드 또한 주어진다.
```

## 6. 함수 작성

```txt
1. MainActivity -> private 함수를 하나 작성
```

## 7. Override 작성

```txt
1. Override 함수 안에 findViewById를 작성한다.
2. AdRequest 함수도 작성한다.
3. loadAd도 작성한다.
```

## 8. 완료

```txt
코드를 작성할 때는 Admob 테스트용 아이디를 사용하도록 한다. 본인의 아이디를 사용할 경우 계정 정지등 불이익을 당할 수 있음
```
