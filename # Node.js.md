# Node.js
설치

```bash
curl -fsSL https://deb.nodesource.com/setup_18.x | sudo -E bash -
sudo apt-get install -y nodejs
```

`app.js`

```javascript
const http = require('http')

const hostname = '0.0.0.0'
const port = 3000

const server = http.createServer((req,res) => {
        res.statusCode = 200
        res.setHeader('Content-Type', 'text/plan')
        res.end('Hello World')
})

server.listen(port,hostname,() => {
        console.log(`Server running at http://${hostname}:${port}/`)
})
```

실행
```bash
node app.js
```

다른 터미널 열어서 확인
```bash
curl localhost:8080
```
-> hello world 출력

### Express

`package.json`
```json
{
  "name": "hello-express",
  "version": "1.0.0",
  "dependencies": {
    "express": "~4.18.1"
  },
  "scripts": {
    "start": "node server.js"
  },
  "main": "server.js"
}
```

`server.js`

```javascript
'use strict'

const express = require('express')

const port = 8080
const hostname = '0.0.0.0'

const app = express()
app.get('/', (req,res) => {
	res.send('hello world')
})

app.listen(port, hostname)
console.log('running on http://${hostname}:${port}')
```

노드 패키지 매니저 설치
```bash
npm install
```

실행
```bash
node server.js
```

다른 터미널 열어서 확인
```bash
curl localhost:8080
```
-> hello world 출력

node_modules 제외

`.dockerignore`
```dockerfile
node_modules
```
-> node_modules 디렉토리에 패키지 설치
   깃허브 및 도커파일에서 제외해야함
