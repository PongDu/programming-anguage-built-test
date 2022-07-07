# C

- 미리 도커를 깔아둡니다.

1. hello.c 파일 만들기

`hello.c`

```C
#include <stdio.h>
int main() {
            printf("Hello World!\n");
                return 0;
}
```

2. gcc 설치
-> c언어의 컴파일러
```bash
sudo apt install gcc
```

3. gcc로 hello.c 파일을 실행파일로 만들기

```bash
gcc hello.c -o hello
```

파일 확인
```bash
ldd hello
file hello
```
-> hello: ELF 64-bit LSB shared object, x86-64, version 1 (SYSV), dynamically linked
    -> 다이나믹 파일임을 확인

4. 실행해보기

```bash
./hello
```
-> Hello World! 출력


### dockerfile로 이미지 생성

1. 도커 이미지 생성

`Dockerfile`

```dockerfile
FROM ubuntu:focal
COPY hello /hello
CMD ["/hello"]
```

2. 이미지 생성
```bash
docker image build -t hello-c .
```

3. 이미지 실행
```bash
docker run hello-c
```
-> hello world 출력
-> alpine 베이스 이미지에서는 출력되지 않는다.
   스태틱 파일이 아니기 때문

---

### 스태틱 바이너리 (정적 링크)
예전엔 /sibn 다 스태틱
컨테이너 들어오고난 뒤 요즘에도 쓰는 추세

1. 스태틱 실행파일 만들기
```bash
gcc hello.c --static -o hello-static
```

2. 확인
```bash
ldd hello-static
file hello-static
```
-> hello-static: ELF 64-bit LSB executable, x86-64, version 1 (GNU/Linux), statically linked, (... 생략)
    -> 스택틱 실행파일임을 확인

3. 도커파일 작성
```dockerfile
FROM alpine
COPY hello-static /hello
CMD ["/hello"]
```

4. 이미지 생성
```bash
docker image build -t hello-c:alpine .
```

5. 이미지 실행
```bash
docker run hello-c:alpine
```
-> hello world 출력

---

### dockerfile로 멀티 생성

`Dockerfile-multi`

1. 도커파일 작성
```bash
FROM gcc:12 as cbuilder
COPY hello.c /src/app/
WORKDIR /src/app/
RUN gcc hello.c --static -o hello

FROM scratch
COPY --from=cbuilder /src/app/hello /hello
CMD ["/hello"]
```
-> 파일 자체에서 gcc를 실행하고 아래의 이미지를 만들기

2. 이미지 생성
```bash
docker image build -t hello-c:scratch -f Dockerfile-multi .
```
