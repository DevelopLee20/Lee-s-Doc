# Ubuntu OS 딥러닝 서버 CUDA 설치 기록

- 1달의 기간과 10번의 포멧으로 모든 오류를 겪어본 후 적는 기록

## 목차

- [1. 서버(PC) 사양 확인](#1.-서버(PC)-사양-확인)
  - [1-1. 우분투 버전 확인](#1-1-%EC%9A%B0%EB%B6%84%ED%88%AC-%EB%B2%84%EC%A0%84-%ED%99%95%EC%9D%B8)
  - [1-2. CPU 정보 확인](#1-2-cpu-%EC%A0%95%EB%B3%B4-%ED%99%95%EC%9D%B8)
  - [1-3. GPU 정보 확인](#1-3-gpu-%EC%A0%95%EB%B3%B4-%ED%99%95%EC%9D%B8)
- [2. 버전 맞추기](#2-%EB%B2%84%EC%A0%84-%EB%A7%9E%EC%B6%94%EA%B8%B0)
- [3. 그래픽 드라이버 설치](#3-%EA%B7%B8%EB%9E%98%ED%94%BD-%EB%93%9C%EB%9D%BC%EC%9D%B4%EB%B2%84-%EC%84%A4%EC%B9%98)
  - [3-1. 그래픽 드라이버가 설치되어있을 때](#3-1-%EA%B7%B8%EB%9E%98%ED%94%BD-%EB%93%9C%EB%9D%BC%EC%9D%B4%EB%B2%84%EA%B0%80-%EC%84%A4%EC%B9%98%EB%90%98%EC%96%B4%EC%9E%88%EC%9D%84-%EB%95%8C)
  - [3-2. 그래픽 드라이버가 설치되어있지 않을 때](#3-2-%EA%B7%B8%EB%9E%98%ED%94%BD-%EB%93%9C%EB%9D%BC%EC%9D%B4%EB%B2%84%EA%B0%80-%EC%84%A4%EC%B9%98%EB%90%98%EC%96%B4%EC%9E%88%EC%A7%80-%EC%95%8A%EC%9D%84-%EB%95%8C)
- [4. Anaconda 가상환경 설치](#4-anaconda-%EA%B0%80%EC%83%81%ED%99%98%EA%B2%BD-%EC%84%A4%EC%B9%98)
  - [4-1. apt update](#4-1-apt-update)
  - [4-2. curl 설치](#4-2-curl-%EC%84%A4%EC%B9%98)
  - [4-3. 파일 가져오기](#4-3-anaconda-%ED%8C%8C%EC%9D%BC-%EA%B0%80%EC%A0%B8%EC%98%A4%EA%B8%B0)
  - [4-4. Anaconda 파일 무결성 검사](#4-4-Anaconda-%ED%8C%8C%EC%9D%BC-%EB%AC%B4%EA%B2%B0%EC%84%B1-%EA%B2%80%EC%82%AC)
  - [4-5. Anaconda 설치](#4-5-anaconda-%EC%84%A4%EC%B9%98)
  - [4-6. 버전 확인](#4-6-%EB%B2%84%EC%A0%84-%ED%99%95%EC%9D%B8)
  - [4-7. 부록 Anaconda 가상환경 만들기](#4-7-%EB%B6%80%EB%A1%9D-anaconda-%EA%B0%80%EC%83%81%ED%99%98%EA%B2%BD-%EB%A7%8C%EB%93%A4%EA%B8%B0)
- [5. CU7DA Toolkit 설치](#5-cuda-toolkit-%EC%84%A4%EC%B9%98)
  - [5-1. CUDA Toolkit 다운로드](#5-1-cuda-toolkit-%EB%8B%A4%EC%9A%B4%EB%A1%9C%EB%93%9C)
  - [5-2. 환경변수 설정](#5-2-%ED%99%98%EA%B2%BD%EB%B3%80%EC%88%98-%EC%84%A4%EC%A0%95)
  - [5-3. CUDA Toolkit 버전 확인](#5-3-cuda-toolkit-%EB%B2%84%EC%A0%84-%ED%99%95%EC%9D%B8)
- [6. CuDNN 설치](#6-cudnn-%EC%84%A4%EC%B9%98)
  - [6-1. xz 파일 압축 풀기](#6-1-xz-%ED%8C%8C%EC%9D%BC-%EC%95%95%EC%B6%95-%ED%92%80%EA%B8%B0)
  - [6-2. tar 파일 해제](#6-2-tar-%ED%8C%8C%EC%9D%BC-%ED%95%B4%EC%A0%9C)
  - [6-3. CuDNN 파일 복사](#6-3-cudnn-%ED%8C%8C%EC%9D%BC-%EB%B3%B5%EC%82%AC)
  - [6-4. CuDNN 파일 링크 연결](#6-4-cudnn-%ED%8C%8C%EC%9D%BC-%EB%A7%81%ED%81%AC-%EC%97%B0%EA%B2%B0)
- [7. Truble shooting list(컴퓨터가 날 싫어해)](#7-truble-shooting-list%EC%BB%B4%ED%93%A8%ED%84%B0%EA%B0%80-%EB%82%A0-%EC%8B%AB%EC%96%B4%ED%95%B4)
  - [7-1. 그래픽 드라이버 충돌(검은 화면)](#7-1-%EA%B7%B8%EB%9E%98%ED%94%BD-%EB%93%9C%EB%9D%BC%EC%9D%B4%EB%B2%84-%EC%B6%A9%EB%8F%8C%EA%B2%80%EC%9D%80-%ED%99%94%EB%A9%B4)
  - [7-2. 재부팅 후 인터넷 연결 안됨](#7-2-%EC%9E%AC%EB%B6%80%ED%8C%85-%ED%9B%84-%EC%9D%B8%ED%84%B0%EB%84%B7-%EC%97%B0%EA%B2%B0-%EC%95%88%EB%90%A8)
  - [7-3. ssh key 에러](#7-3-ssh-key-%EC%97%90%EB%9F%AC)
  - [7-4. 재부팅 후 nvidia-smim nvcc -V 명령어 미작동](#7-4-%EC%9E%AC%EB%B6%80%ED%8C%85-%ED%9B%84-nvidia-smi-nvcc--v-%EB%AA%85%EB%A0%B9%EC%96%B4-%EB%AF%B8%EC%9E%91%EB%8F%99)
  - [7-5. apt upgrade 명령어 납용](#7-5-apt-upgrade-%EB%AA%85%EB%A0%B9%EC%96%B4-%EB%82%A9%EC%9A%A9)
  - [7-6. root 폴더 권한 문제](#7-6-root-%ED%8F%B4%EB%8D%94-%EA%B6%8C%ED%95%9C-%EB%AC%B8%EC%A0%9C)
  - [7-7. source ~/.bashrc](#7-7-source-bashrc)
  - [7-8. CuDNN의 .deb 파일](#7-7-source-bashrc)
  - [7-9. 장착한 그래픽 카드가 아닌 다른 출력되는 문제](#7-7-source-bashrc)
  - [7-10. 설치가능한 그래픽 드라이버가 아무것도 뜨지 않는 문제](#7-7-source-bashrc)

## 1. 서버(PC) 사양 확인

학부 연구실에 있는 서버용 컴퓨터인 seolak(설악산)을 사용하여 CUDA를 설치하였다.

- OS: Ubuntu 22.04.2 LTS
- CPU: AMD Ryzen 9 5950X 16-Core Processor
- GPU: GeForce RTX 3090

아래에는 OS, CPU, GPU를 확인하는 linux 명령어를 나열해보았다.

### 1-1. 우분투 버전 확인

```powershell
$ lsb_release -a
```

```powershell
No LSB modules are available.
Distributor ID: Ubuntu
Description:  Ubuntu 22.04.2 LTS
Release:  22.04
Codename:   
```

### 1-2. CPU 정보 확인

```powershell
$ cat /proc/cpuinfo
```

```powershell
model name  : AMD Ryzen 9 5950X 16-Core Processor
```

많은 출력문이 뜨겠지만 model name 부분만 읽어보면 될 것 같다.

### 1-3. GPU 정보 확인

```powershell
$ lspci | grep -i VGA
```

```powershell
2b:00.0 VGA compatible controller: NVIDIA Corporation GA102 [GeForce RTX 3090] (rev a1)
```

## 2\. 버전 맞추기

CUDA를 설치하면서 중요한 것은 각 종속성들의 버전을 맞춰주는 것이다. 버전은 [Tensorflow 설치 사이트](https://www.tensorflow.org/install/source?hl=ko) 해당 링크에서 GPU 챕터로 가서 버전 확인 후 맞춰주면 된다. 내가 채택한 버전은 다음과 같다.

- Tensorflow: 2.12.0
- Python: 3.10
- Compiler: GCC 9.3.1
- Build Tool: Barzel 5.3.0
- cuDNN: 8.6
- CUDA: 11.8

위의 내용에서 우리가 고려해야할 부분은 아래와 같다.

- Tensorflow: 2.12.0
- Python: 3.10
- cuDNN: 8.6
- CUDA: 11.8

중요: _위의 CUDA 버전은 CUDA Toolkit의 버전입니다._

이 4가지의 버전만 맞는다면 호환성에는 문제가 없을 것이다.

## 3\. 그래픽 드라이버 설치

Ubuntu에서의 그래픽 드라이버를 확인하는 명령어는 다음과 같다.

```powershell
$ nvidia-smi
```

### 3-1. 그래픽 드라이버가 설치되어있을 때

만약 해당 명령어를 입력했을 때, 그래픽 드라이버가 설치되어있다면 아래의 내용이 출력될 것이다. 아래의 내용에서 **CUDA Version: 12.2**에 주목하자. 만약 **위에서 정해두었던 CUDA의 버전**이 **해당 출력에서 나온 CUDA 버전보다 높다면** 더 높은 버전의 그래픽 드라이버를 설치해야한다. 현재 그래픽 드라이버의 버전은 **Driver Version: 535.54.03**이며 이 또한 아래의 출력에서 확인할 수 있다.

```powershell
Tue Jul 25 11:52:03 2023
+---------------------------------------------------------------------------------------+
| NVIDIA-SMI 535.54.03    Driver Version: 535.54.03  CUDA Version: 12.2 |
|-----------------------------------------+----------------------+----------------------+
| GPU  Name   Persistence-M | Bus-Id  Disp.A | Volatile Uncorr. ECC |
| Fan  Temp Perf  Pwr:Usage/Cap |   Memory-Usage | GPU-Util  Compute M. |
|       |    |   MIG M. |
|=========================================+======================+======================|
| 0  NVIDIA GeForce RTX 3090  Off | 00000000:2B:00.0 Off |    N/A |
|  0% 49C  P8    34W / 420W | 33MiB / 24576MiB |  0%  Default |
|       |    |    N/A |
+-----------------------------------------+----------------------+----------------------+

+---------------------------------------------------------------------------------------+
| Processes:              |
|  GPU GI CI  PID Type Process name      GPU Memory |
|  ID ID           Usage  |
|=======================================================================================|
|  0 N/A  N/A  1467  G /usr/lib/xorg/Xorg     15MiB |
|  0 N/A  N/A  1612  G /usr/bin/gnome-shell      8MiB |
+---------------------------------------------------------------------------------------+
```

### 3-2. 그래픽 드라이버가 설치되어있지 않을 때

그래픽 드라이버가 설치되어있지 않을 때 위의 출력이 나오지 않을 것이다. 이제 그래픽 드라이버를 설치해보자.

```powershell
ubuntu-drivers devices
```

해당 명령어를 입력하면 설치 가능한 그래픽 드라이버 목록이 출력된다.

```powershell
== /sys/devices/pci0000:00/0000:00:03.1/0000:2b:00.0 ==
modalias : pci:v000010DEd00002204sv00001462sd00003882bc03sc00i00
vendor : NVIDIA Corporation
model  : GA102 [GeForce RTX 3090]
driver : nvidia-driver-470-server - distro non-free
driver : nvidia-driver-470 - distro non-free
driver : nvidia-driver-525 - distro non-free
driver : nvidia-driver-525-server - distro non-free
driver : nvidia-driver-525-open - distro non-free
driver : nvidia-driver-535-server-open - distro non-free recommended
driver : nvidia-driver-535-open - distro non-free
driver : nvidia-driver-535 - distro non-free
driver : nvidia-driver-535-server - distro non-free
driver : xserver-xorg-video-nouveau - distro free builtin
```

출력된 명령어에서 **recommended**라는 단어를 찾을 수 있는데, 해당 그래픽 드라이버를 설치하면 된다.

이때 **nvidia-driver-숫자** 하고 뒤에 다른 단어들이 붙는 드라이버들이 보일 텐데 다 무시하고 숫자까지만 된 드라이버를 설치하도록 한다. 드라이버를 설치하는 명령어는 다음과 같다.

```powershell
$ sudo apt install nvidia-driver-535
```

그 후 재부팅 한다.

```powershell
$ sudo reboot now
```

마지막으로 드라이버 설치 확인을 했을 때, 위 처럼 정상적으로 뜬다면 성공

## 4. Anaconda 가상환경 설치

### 4-1. apt update

```powershell
$ sudo apt update
```

### 4-2. curl 설치

```powershell
$ sudo apt install curl
```

만약 curl이 이미 있다면 지나가도 된다.

### 4-3. Anaconda 파일 가져오기

[Anaconda Achieve](https://repo.anaconda.com/archive/) 해당 링크에 접속해서 원하는 버전의 Anaconda .sh 파일의 url을 가져와 아래의 명령어를 입력한다.

```powershell
$ curl --output anaconda.sh {가져온 url}
```

anaconda.sh의 이름을 가진 sh 파일이 저장된다.

### 4-4. Anaconda 파일 무결성 검사

파일이 정상적으로 다운로드 되었는지 무결성 검사를 진행한다.

```powershell
$ sha256sum anaconda.sh
```

해당 코드를 실행해 출력된 값과 [Anaconda Achieve](https://repo.anaconda.com/archive/)에 있는 파일의 SHA256과 같은지 확인한다. 만약 같다면 정상, 다르다면 파일을 다시 다운로드 받자.

## 4-5. Anaconda 설치

이제 아래 명령어를 통해 Anaconda를 설치하자

```powershell
$ bash anaconda.sh
```

많은 양의 글이 나올텐데, 라이센스 권한 설명이므로 넘기면서 마지막에 **Please answer 'yes' or 'no':'**에서 yes를 누를 수 있도록 하자. _필자는 작성당시 기본값이 no로 되어있어 무지성 no를 눌렀다가 몇 번을 다시 실행했다_  
또한 **running conda init?** 이라는 문구가 나오면 yes를 선택해준다. Anaconda를 자동으로 실행시킬 것이냐는 질문이다.

마지막으로 환경변수를 추가해주면 설치가 끝이 난다.

```powershell
$ sudo vim ~/.bashrc
```

- .bashrc 파일 내부에 해당 코드를 추가한다.

```powershell
export PATH=~/anaconda3/bin:~/anaconda3/condabin:$PATH
```

## 4-6. 버전 확인

정상적으로 설치되었는지 버전을 확인한다.

```powershell
$ conda -V
```

```powershell
conda 23.5.2
```

출력이 나온다면 정상적으로 설치된 것이다.

## 4-7. 부록 Anaconda 가상환경 만들기

```powershell
$ conda create -n {환경이름} python={파이썬버전}
```

## 5\. CUDA Toolkit 설치

### 5-1. CUDA Toolkit 다운로드

[CUDA Toolkit Archive](https://developer.nvidia.com/cuda-toolkit-archive) 해당 링크에 들어가 원하는 정해둔 버전의 CUDA Toolkit을 다운로드 받는다. 필자는 **Installer Type**을 **runfile**로 선택해 진행하였다. 실행하고 권환 관련에 대해 accept를 선택하고나면 해당 화면이 나오는데,

```powershell
┌──────────────────────────────────────────────────────────────────────────────┐
│ CUDA Installer           │
│ - [ ] Driver           │
│  [ ] 520.61.05           │
│ + [X] CUDA Toolkit 11.8          │
│ [X] CUDA Demo Suite 11.8         │
│ [X] CUDA Documentation 11.8        │
│ - [ ] Kernel Objects           │
│  [ ] nvidia-fs           │
│ Options            │
│ Install            │
│              │
│              │
│              │
│              │
│              │
│              │
│              │
│              │
│              │
│              │
│              │
│              │
│ Up/Down: Move | Left/Right: Expand | 'Enter': Select | 'A': Advanced options │
└──────────────────────────────────────────────────────────────────────────────┘
```

위의 그림처럼 Driver에 X표시를 없애준다. X표시를 그대로 두게 되면 Driver가 자동으로 설치되는데, 기존에 설치했던 드라이버와 충돌이 발생하기 때문에 뺀 후 **Install** 버튼을 눌러준다.

### 5-2. 환경변수 설정

그대로 기다린 후 화면에 나오는 환경 변수 2개를 설정해주면 되는데, CUDA Toolkit 버전이 11.8 일때는 다음과 같다.

```powershell
$ sudo vim ~/.bashrc
```

```powershell
export PATH="/usr/local/cuda-11.8/bin:$PATH"
export LD_LIBRARY_PATH="/usr/local/cuda-11.8/lib64:$LD_LIBRARY_PATH"
```

위의 두 개의 환경변수를 추가해 준 후 해당 명령어를 입력해서 환경 변수를 추가해준다.

### 5-3. CUDA Toolkit 버전 확인

```powershell
$ nvcc -V
```

아래의 출력이 나오게 된다.

```powershell
nvcc: NVIDIA (R) Cuda compiler driver
Copyright (c) 2005-2022 NVIDIA Corporation
Built on Wed_Sep_21_10:33:58_PDT_2022
Cuda compilation tools, release 11.8, V11.8.89
Build cuda_11.8.r11.8/compiler.31833905_0
```

만약 나온다면 정상적으로 설치가 된 것이며, 아니라면 다시 시도해보자.

## 6\. CuDNN 설치

[cuDNN Archive](https://developer.nvidia.com/rdp/cudnn-archive) 해당 링크에 들어가 정해두었던 cuDNN 버전에 따른 파일을 다운로드 받는다. 이때 주의할 점은 **tar 파일**을 받을 수 있도록 한다.

### 6-1. xz 파일 압축 풀기

```powershell
unxz {파일명.xz}
```

위의 명령어로 압축을 풀고

### 6-2. tar 파일 해제

```powershell
tar xvf {파일명.tar}
```

위의 명령어로 파일을 해제한다.

### 6-3. CuDNN 파일 복사

그러면 "파일명"을 가진 폴더가 생성되는데, 이때 아래의 명령어를 실행시킨다.

```powershell
sudo cp {파일명}/include/cudnn* /usr/local/{cuda-버전}/include
sudo cp {파일명}/lib64/libcudnn* /usr/local/{cuda-버전}/lib64
```

- cp: copy의 줄임말, 파일을 복사한다.
- 사실 위의 명령어가 정확하지 않다. 자신에게 맞는 path로 수정하는 것이 중요할 것 같다.
- 우분투 버전에 따라서 "lib64" 폴더가 "lib" 로 보일 수 있다.

### 6-4. CuDNN 파일 링크 연결

```powershell
sudo ln -sf /usr/local/{cuda-버전}/targets/x86_64-linux/lib/libcudnn_adv_train.so.8.0.5 /usr/local/{cuda-버전}/targets/x86_64-linux/lib/libcudnn_adv_train.so.8
sudo ln -sf /usr/local/{cuda-버전}/targets/x86_64-linux/lib/libcudnn_ops_infer.so.8.0.5  /usr/local/{cuda-버전}/targets/x86_64-linux/lib/libcudnn_ops_infer.so.8
sudo ln -sf /usr/local/{cuda-버전}/targets/x86_64-linux/lib/libcudnn_cnn_train.so.8.0.5  /usr/local/{cuda-버전}/targets/x86_64-linux/lib/libcudnn_cnn_train.so.8
sudo ln -sf /usr/local/{cuda-버전}/targets/x86_64-linux/lib/libcudnn_adv_infer.so.8.0.5  /usr/local/{cuda-버전}/targets/x86_64-linux/lib/libcudnn_adv_infer.so.8
sudo ln -sf /usr/local/{cuda-버전}/targets/x86_64-linux/lib/libcudnn_ops_train.so.8.0.5  /usr/local/{cuda-버전}/targets/x86_64-linux/lib/libcudnn_ops_train.so.8
sudo ln -sf /usr/local/{cuda-버전}/targets/x86_64-linux/lib/libcudnn_cnn_infer.so.8.0.5 /usr/local/{cuda-버전}/targets/x86_64-linux/lib/libcudnn_cnn_infer.so.8
sudo ln -sf /usr/local/{cuda-버전}/targets/x86_64-linux/lib/libcudnn.so.8.0.5  /usr/local/{cuda-버전}/targets/x86_64-linux/lib/libcudnn.so.8

ldconfig
```

위의 코드처럼 링크를 차례로 연결 시킨 후 아래의 코드를 실행시키면 CuDNN 버전이 출력되게 된다.

```powershell
ldconfig -N -v $(sed 's/:/ /' <<< $LD_LIBRARY_PATH) 2>/dev/null | grep libcudnn
```

- 만약 아무런 출력도 뜨지 않는다면 다시 시도해야할 것 같다.

이제 모든 설치가 끝이 났다. GPU를 사용하는 코드를 실행해 본 후 **nvidia-smi**명령어를 실행하면

```powershell
Tue Jul 25 14:29:20 2023
+---------------------------------------------------------------------------------------+
| NVIDIA-SMI 535.54.03    Driver Version: 535.54.03  CUDA Version: 12.2 |
|-----------------------------------------+----------------------+----------------------+
| GPU  Name   Persistence-M | Bus-Id  Disp.A | Volatile Uncorr. ECC |
| Fan  Temp Perf  Pwr:Usage/Cap |   Memory-Usage | GPU-Util  Compute M. |
|       |    |   MIG M. |
|=========================================+======================+======================|
| 0  NVIDIA GeForce RTX 3090  Off | 00000000:2B:00.0 Off |    N/A |
|  0% 51C  P2   127W / 420W |  293MiB / 24576MiB |  0%  Default |
|       |    |    N/A |
+-----------------------------------------+----------------------+----------------------+

+---------------------------------------------------------------------------------------+
| Processes:              |
|  GPU GI CI  PID Type Process name      GPU Memory |
|  ID ID           Usage  |
|=======================================================================================|
|  0 N/A  N/A  1467  G /usr/lib/xorg/Xorg     15MiB |
|  0 N/A  N/A  1612  G /usr/bin/gnome-shell      8MiB |
|  0 N/A  N/A 86386  C ...adoop/anaconda3/envs/rtx/bin/python  254MiB |
+---------------------------------------------------------------------------------------+
```

아래의 python 프로세스가 GPU 메모리를 사용하고 있는 것을 알 수 있다.

## 7\. Truble shooting list(컴퓨터가 날 싫어해)

내가 1달간 설치를 하는 과정에서 당했던 문제를 정리해보았다.

### 7-1. 그래픽 드라이버 충돌(검은 화면)

- 그래픽 드라이버의 버전을 바꾸다보면 컴퓨터가 검은 화면에 진입할 때가 있다.
- 필자는 어차피 꼬인 드라이버이기 때문에 포멧을 선택했지만(포멧에 자유도가 매우 높았음) 포멧이 불가능한 경우
- Ubuntu의 안전모드로 접속해 드라이버 재설치를 시도해보자.

### 7-2. 재부팅 후 인터넷 연결 안됨

- 재부팅 후 netplan 관련 도구가 작동하지 않아 간간히 인터넷이 전혀 잡히지 않고, **netplan apply** 등등 모든 네트워크 관련 명령어가 작동하지 않을 때가 있었다.
- 이 문제 역시 포멧을 선택했지만, 이 문제는 해당 PC의 apt에 네트워크 관련 도구가 설치되어있다면 해결 가능성이 조금은 있다.

### 7-3. ssh key 에러

- 필자는 서버에 직접 모니터와 키보드를 꽂아 작업하는 것은 매우 불편했기 때문에 ssh를 이용해 원격으로 설치를 진행했었다.
- 몇 번의 포멧에 의해 ssh를 계속 다시 설치 했고, local PC에서 ssh key 문제가 발생하기도 했다. 이때는 아래의 명령어를 입력하고, 다시 ssh 접속을 시도해보자.

```powershell
$ ssh-keygen -R {접속할 아이피}
```

### 7-4. 재부팅 후 nvidia-smi, nvcc -V 명령어 미작동

- 재부팅 후 환경변수가 날아가 되던 각종 명령어들이 안되는 경우가 있었다.
- 이 경우 대부분 ~/.bashrc에 적어두면 해결되지만 그저 **export** 명령어를 이용해 쉘에 입력할 때 자주 발생했다.
- 꼭 ~/.bashrc 파일에 적어주도록 하자.

### 7-5. apt upgrade 명령어 납용

- 6번째 포멧을 하게 되었을 때, 뭣도 모르고 사용한 **apt upgrade**가 문제였었다.
- update 명령어와 비슷해 그냥 썼지만 사실 upgrade 명령어는 모든 설치된 라이브러리들의 가능한 최신버전으로 업그레이드 하는 것이었다....
- 이로 인해 그래픽 카드 드라이버의 버전도 바뀌게 되었고, 그대로 화면은 켜지지 않았다....
- 포멧으로 해결했다.

### 7-6. root 폴더 권한 문제

- 가끔 설치하는 파일들을 **root 폴더**에 저장하는 경우가 있는데, 이 경우 권한 문제와 파일을 찾지 못하는 무수한 문제가 발생하기 때문에 꼭 프로그램을 다운 받을 때에는 **home 폴더**에 다운 받기를 권장한다.

### 7-7. source ~/.bashrc

- 전원을 껏다 키는 것처럼 효과를 주는 **source ~/.bashrc**는 그냥 생각날 때마다 한번씩 꼭 해주자.
- 원래는 환경변수나 새로운 프로그램 설치시에만 해주면 된다.
- 가끔 까먹어서 헤메던 경험이 있다..

### 7-8. CuDNN의 .deb 파일

- CuDNN 공식 Archive에서 .deb 파일을 지원해주길래 다운받아서 바로 사용했는데, 아무런 효과가 없었다..
- 오히려 왜 설치했는데 안돼!! 라는 생각 밖에 하지 못했기 때문에 그냥 **tar 파일**을 다운받아 쓰도록 하자

### 7-9. 장착한 그래픽 카드가 아닌 다른 출력되는 문제

- 패키지 업데이트를 통해 해결할 수 있으며, 아래의 명령어를 입력합니다.

```powershell
$ sudo update-pciids
```

### 7-10. 설치가능한 그래픽 드라이버가 아무것도 뜨지 않는 문제

- 아래의 명령어를 입력해도 아무것도 뜨지 않을 때

```powershell
$ ubuntu-drivers devices
```

- 이 또한 아래의 명령어를 통한 패키지 업데이트를 통해 해결 가능하다.

```powershell
$ sudo apt-get update
$ sudo apt-get upgrade
```

수정.

2023-07-25 초안
2024-01-18 트러블 슈팅 내용 추가
