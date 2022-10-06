# Selenium

- 자바스크립트까지 크롤링 가능한 라이브러리

## 라이브러리 설치

```python
!pip install selenium
```

## Selenium 크롤링 과정

1. 크롬 드라이버(Chrome Driver)를 통해서 웹 서치
2. 크롬 드라이버(Chrome Driver)를 통해 보고 있는 웹의 스크립트 포함 모든 코드를 불러온다.

- 고로 셀레니엄(Selenium)을 사용하기 위해서는 크롬 드라이버(Chrome Driver)가 필요하다.
- 크롬 드라이버(Chrome Driver)는 가상의 크롬이라고 생각하면 된다.

## Chrome Driver 라이브러리 설치

```python
!pip install chromedriver-autoinstaller
```

## Chrome Driver 자동 설치

```python
import chromedriver_autoinstaller

# 크롬의 최신 버전을 가져온다.
now_chrome_version = chromedriver_autoinstaller.get_chrome_version()

# 사용한 크롬 드라이버의 버전을 저장한다.
version = now_chrome_version.split(".")[0]

# 해당 크롬 버전의 폴더 안에 드라이버가 설치되므로 그 위치를 저장한다.
save_path = f'./{version}/chromedriver.exe'

# 크롬 드라이버의 최신버전을 설치한다.
chromedriver_autoinstaller.install(1)
```

## 셀레니엄으로 크롬 웹 드라이버 실행

```python
from selenium import webdriver

options = webdriver.ChromeOptions() # 드라이버의 옵션을 넣을 수 있다.
options.add_argument('headless')    # 웹 화면을 표시하지 않고 드라이버를 실행
address = '크롤링할 웹페이지 주소'

# 드라이버 exe 파일의 위치
driver = webdriver.Chrome(save_path, option=options) # 옵션이 없을 경우 option 속성 x

driver.maximize_window() # 웹 최대화
driver.minimize_window() # 웹 최소화
driver.get(address)      # 웹 페이지 실행
```

## 셀레니엄 요소 찾기

```python
from selenium.webdriver.common.by import By
from selenium.webdriver.support.ui import WebDriverWait
from selenium.webdriver.support import expected_conditions as EC
from selenium.webdriver.common.keys import Keys
import pyperclip

# 스크립트 실행시 표시가 느린 요소들이 보일때까지 기다린다. (10초간 기다려봄)
WebDriverWait(driver, 10).until(EC.presence_of_element_located((By.ID, 'html id name')))

driver.find_element(By.ID, 'html id name')
driver.find_element(By.ID, 'html id name').click()

# 값을 복사하는 효과를 낼 수 있다.
pyperclip.copy('user id')

# 키보드의 ctrl + v 를 누른 효과를 낼 수 있다.
driver.find_element(By.XPATH, 'html XPATH name').send_keys(Keys.CONTROL, 'v')
```
