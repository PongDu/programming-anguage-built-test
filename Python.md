# Python

1. 파이썬3 및 pip 설치
pip:Python 패키지를 설치하고 관리하는 패키지 매니저

```bash
sudo apt install python3-pip python3-venv -y
```

2. 가상환경 만들기
```bash
python3 -m venv venv
```

3. 가상환경 활성화
-> 작업할 때 꼭 가상환경 활성화를 해주어야 한다.
```bash
source venv/bin/activate
```

> 비활성화 `deactivate`

4. Flask 설치(가상환경에서)
    ->간단한 웹서버 구축 기능 제공
```bash
pip3 install Flask
```

5. python 파일 만들기

`hello.py`

```python
from flask import Flask

app = Flask(__name__)

@app.route("/")
def hello_world():
     return "<h1> Hello World </h1>"
```

6. 실행
```bash
export FLASK_APP=hello
flask run --host="0.0.0.0"
```

7. requirements.txt에 패키지 리스트 freeze 
-> pip freeze라는 명령어는 현재 작업 환경  
   (가상환경)에 설치되어있는 패키지의 리스트를 모두 출력
-> requirements.txt파일에 출력결과를 저장
-> 파일 이름은 관행적으로 requirements를 쓴다.

```bash
pip3 freeze > requirments.txt.txt
```

requirements.txt에 있는 내용을 가지고 자동으로 패키지를 설치
> `pip3 install -r requirments.txt`


8. .gitignore 파일 작성
-> Git 버전 관리에서 제외할 파일 목록을 지정하는 파일
-> __pycache__ 는 컴파일 하면서 생기는 캐시파일
-> 프로젝트를 할 때 깃허브에 올리면 충돌 가능성이 있어 올리지 않고 삭제 권장

```bash
.gitignore
```
-> 캐시파일과 가상환경 지정

9. 캐시파일 및 가상환경 삭제
   
```bash
rm -rf __pycache__ 
rm -rf venv
```
---

도커파일로 해보기

1. 도커파일 작성
`Dockerfile`

```bash
FROM python:3.8-slim-buster

COPY . /src/myflask/
WORKDIR /src/myflask

RUN pip3 install -r requirments.txt
ENV FLASK_APP=hello
CMD ["flask", "run", "--host=0.0.0.0"]
EXPOSE 5000/tcp
```

2. 이미지 생성
```bash
docker image build -t myflask .
```

3. 이미지 만들기
```bash
docker run -d -p 5000:5000 myflask
```

4. 확인
```bash
curl localhost:5000
```
-> <h1> Hello World </h1>% 출력

---

## Git에 올려보기

git에 올라가야하는건 hello.py와 requirments.txt 두개 뿐!
위에 과정처럼 가상환경과 캐시파일은 삭제하자

1. 로컬 저장소 생성

```bash
git init
```

2. 원격 저장소 만들기
```bash
git remote add origin git@github.com:[유저 이름]/[저장소 이름].git
```

3) 브랜치 생성
git branch -M main

4) 원격 저장소에 로컬 저장소 파일 올리기
git push -u origin main