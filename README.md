## 1. 오류와의 씨름
### 도커는 별도의 자체 폴더구조를 활용하는 점을 몰라서 맥운영체제에 존재하는 디렉토리 경로를 도커파일에 입력하는 바람에 도커이미지 빌드에 실패하였습니다.
맥운영체제의 폴더구조를 잘못 반영해서 작성한 도커파일
```
FROM python
WORKDIR /Users/minsugoogl4745/custom_docker_image
COPY /custom_docker_image /custom_docker_copy
CMD ["python", "python_puzzle.py"]
```
도커의 폴더구조를 반영하여 작성한 도커파일
```
FROM python:latest
WORKDIR /app
COPY . .
CMD ["python", "python_puzzle.py"]
```
##
### dockerfile에 사소한 오타가 있어 도커 이미지 빌드에 실패했습니다.
파이썬 파일이름에 pyton_puzzle.py에 h가 빠져 도커이미지 빌드에 필요한 파이썬 파일을 못찾았습니다.
```
python: can't open file '/Users/minsugoogl4745/custom_docker_image/pyton_puzzle.py': [Errno 2] No such file or directory
```
오타가 난 dockerfile
```
FROM python:latest
WORKDIR /app
COPY . .
CMD ["python", "pyton_puzzle.py"]
```
##
### 도커 컨테이너 내부에서 파이선 파일을 실행하려면 교육생에게 권한이 허가된 디렉토리에 도커파일, 파이썬 파일이 모두 존재하야되는지 몰라서 파이썬 도커이미지 실행을 못했습니다.
```
minsugoogl4745@c4r4s8 ~ % docker build -t python_puzzle .    
[+] Building 9.8s (6/7)                                                                                                                                           docker:orbstack
 => [internal] load build definition from dockerfile                                                                                                                         0.1s
 => => transferring dockerfile: 108B                                                                                                                                         0.0s
 => [internal] load metadata for docker.io/library/python:latest                                                                                                             0.0s
 => [internal] load .dockerignore                                                                                                                                            0.1s
 => => transferring context: 2B                                                                                                                                              0.0s
 => CACHED [1/3] FROM docker.io/library/python:latest                                                                                                                        0.0s
 => ERROR [internal] load build context                                                                                                                                      9.1s
 => => transferring context: 868.32MB                                                                                                                                        9.1s
 => [2/3] WORKDIR /app                                                                                                                                                       0.2s
------
 > [internal] load build context:
------
ERROR: failed to build: failed to solve: error from sender: failed to xattr Library/Group Containers/group.com.apple.secure-control-center-preferences/Library/Preferences/group.com.apple.secure-control-center-preferences.av.plist: permission denied
```
## 드디어 도커환경에서 파이썬 언어로 개발준비 성공!~~
위와 같은 오류들을 모두 해결하여 제게 많은 도움을 준 제미나이가 실행해보라고 건네준 파이썬 코드내용입니다.
```
print("========================================")
print("🎉 도커에서 파이썬 퍼즐이 성공적으로 실행되었습니다! 🎉")
print("========================================")
print("축하합니다! 험난한 과정을 이겨내셨군요!")
```
도커 컨테이너 내부에서 파이썬 코드가 실제로 실행되는 것을 확인했습니다. 이건 마치 도커 헬로 월드 이미지 같은 느낌입니다.
```
minsugoogl4745@c4r4s8 custom_docker_image % docker run python_puzzle
========================================
🎉 도커에서 파이썬 퍼즐이 성공적으로 실행되었습니다! 🎉
========================================
축하합니다! 험난한 과정을 이겨내셨군요!
```
