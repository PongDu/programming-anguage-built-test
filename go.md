# Golang
빌드 속도가 빠르다

1. golang 설치

```bash
sudo apt install golang
```

2. hello.go 파일 만들기
`hello.go`

```go
package main

import "fmt"

func main() {
        fmt.Println("Hello World")
}
```

3. 스크립트처럼 실행
```bash
go run .
```

4. 빌드하기
```bash
go build .
``` 

5. 확인
```bash
./hello-go
``` 

6. 파일 보기 (스태틱인 것 확인 가능)
```bash
file hello-go
```    

---
go로 webserver 만들기

1. go 파일 만들기

`myweb.go`

```go
package main

import (
	"fmt"
	"net/http"
)

func main() {
	http.HandleFunc("/", HelloServer)
	http.ListenAndServe(":3000", nil)
}

func HelloServer(w http.ResponseWriter, r *http.Request) {
	fmt.Fprintf(w, "Hello World \n")
}
```

2. 도커파일 만들기
`Dockerfile`

```dockerfile
FROM golang:1.18-bullseye as gobuilder
COPY myweb.go /src/myweb/
WORKDIR /src/myweb
ENV GO111MODULE=off \
    CGO_ENABLED=0 \
    GOOS=linux \
    GOARCH=amd64
RUN go build .

FROM scratch
COPY --from=gobuilder /src/myweb/myweb /myweb
CMD ["/myweb"]
EXPOSE 3000/tcp
```

3. 이미지 빌드
```bash
docker image built -t hello-myweb:go .
```

4. 포트 열기
```bash
docker run -d -p 3000:3000 hello-myweb:go
```

5. 확인
```bash
curl localhost:3000
```