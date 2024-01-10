# yoloV5

근로 기간 중 진행한 yolo v5 기반 커스텀 데이터 전이학습 방법 기록(2024-01-10)

## 목차

## 개발환경

근로지에 있는 서버를 사용하였다.

- OS: Ubuntu 20.04.6 LTS
- CPU: Intel(R) Xeon(R) Gold 6248R CPU @ 3.00GHz
- GPU: NVIDIA RTX A6000 2개

### 우분투 버전 확인 명령어

```powershell
$ lsb_release -a
```

### CPU 정보 확인

```powershell
$ cat /proc/cpuinfo
```

- 많은 출력문이 뜨겠지만 "model name" 부분만 읽어보면 될 것 같다.

### GPU 정보 확인

```powershell
$ nvidia-smi
```

## yolo 커스텀 데이터 생성

### yolo 데이터 구조

라벨 데이터 구조

```text
{라벨} {정규화된 왼쪽 상단 x좌표} {정규화된 왼쪽 상단 y좌표} {정규화된 오른쪽 하단 x좌표} {정규화된 오른쪽 하단 y좌표}
```

- yolo의 데이터는 이미지 데이터(img), 라벨 데이터(txt)가 한 세트이다.

---

### yolo용 디렉토리 구조 만들기

```text
- dataset
    - train
        - images
        - labels
    - val
        - images
        - labels
    - test(optional)
        - images
        - labels
```
- 위의 구조로 디렉토리 구조를 만든 후 사용하면 편리하게 학습할 수 있다.

---

```text
- data
    - datas1(images, labels)
    - datas2(images, labels)
    - datas3(images, labels)
```

- 여기저기서 긁어온 이미지와 라벨 데이터의 모음(datas)이 있다면, data 폴더에 위의 디렉토리 방식처럼 정리 후 아래의 코드를 실행시키면, 학습 준비가 끝난다.

---

```python
import os
import shutil
import random

path_dataset = "./dataset"
path_train = os.path.join(path_dataset, "train")
path_train_images = os.path.join(path_train, "images")
path_train_labels = os.path.join(path_train, "labels")
path_test = os.path.join(path_dataset, "test")
path_test_images = os.path.join(path_test, "images")
path_test_labels = os.path.join(path_test, "labels")
val_ratio = 0.8

delete_list = [path_train_images, path_train_labels, path_test_images, path_test_labels]
create_list = [path_dataset, path_train, path_test, path_train_images, path_train_labels, path_test_images, path_test_labels]

def delete_contents(path:str):
    try:
        path_dir = os.listdir(path)
    except:
        return 0
    
    for i in path_dir:
        os.remove(os.path.join(path, i))
        
    print(f"Delete {len(path_dir)} files in '{path}'")

def create_folders(path:str):
    try:
        os.mkdir(path)
        print(f"Create {path} folder.")
    except:
        return 0

def preprocessing():
    path = "./data"
    
    all_files = []
    for i in os.listdir(path):
        
        dir_path = os.path.join(path, i)
        for j in os.listdir(dir_path):
            name, ext = j.split(".")
            
            if ext == "txt" and os.path.getsize(os.path.join(dir_path, j)) != 0:
                all_files.append([dir_path, name])
                
    all_count = len(all_files)
    train_count = int(all_count * val_ratio)

    train_list = []
    test_list = None
    for _ in range(train_count):
        rand = random.randrange(0, len(all_files))
        file = all_files.pop(rand)
        train_list.append(file)
    
    test_list = all_files
    
    for i in train_list:
        filename = i[1] + ".png"
        start = os.path.join(i[0], filename)
        dest = os.path.join(path_train_images, filename)
        shutil.copy(start, dest)
        
        filename = i[1] + ".txt"
        start = os.path.join(i[0], filename)
        dest = os.path.join(path_train_labels, filename)
        shutil.copy(start, dest)
        
    for i in test_list:
        filename = i[1] + ".png"
        start = os.path.join(i[0], filename)
        dest = os.path.join(path_test_images, filename)
        shutil.copy(start, dest)
        
        filename = i[1] + ".txt"
        start = os.path.join(i[0], filename)
        dest = os.path.join(path_test_labels, filename)
        shutil.copy(start, dest)
        
    print(f"Train size: {len(train_list)}")
    print(f"Test size: {len(test_list)}")

if __name__ == "__main__":
    for i in delete_list:
        delete_contents(i)
    print("Delete Complete.\n")
    
    for i in create_list:
        create_folders(i)
    print("Create Complete.\n")
    
    preprocessing()

```

---

## yolo v5 가져오기

### Github에서 yolo v5 clone 해오기

```powershell
$ git clone https://github.com/ultralytics/yolov5.git
```

- 위의 Github 레파지토리에서 yolo v5를 가져온다.

### 생성한 커스텀 데이터 yolo에 넣어두기

```text
- yolov5
    - dataset
```

- 위의 구조처럼 만든 dataset 폴더를 넣어둔다.

---

## yolo v5 커스텀 데이터 학습시키기

### yaml 파일 생성하기

```yaml
path: ../dataset
train: train/images
val: test/images
test: # optional

names:
  0: person
  1: goods
```

- 파일명은 custom.yaml 으로 지정한다.
- yolo는 yaml 파일을 통해 데이터를 읽기 때문에 꼭 필요하다.
- 모든 value 값들은 폴더의 위치와 이름에 따라 변경해주어야 한다.
- names 또한 분류하고 싶은 태그에 따라 추가해주어야 한다.

### 최종 학습시키기

```powershell
$ python train.py --img 1024 --epochs 3 --data custom.yaml --weight yolov5m.pt
```

- img: 입력할 이미지 크기
- epochs: 반복횟수
- data: 사용할 yaml 파일명
- weight: 전이학습을 할 때 사용할 매개변수 모델

### weight 옵션의 선택지

```powershell
yolov5n.pt
yolov5s.pt (recommanded)
yolov5m.pt
yolov5l.pt
yolov5x.pt
```

- 아래로 갈수록 무거운 모델이지만, 가장 빠르고 추천하는 모델은 yolov5s 모델이다.
