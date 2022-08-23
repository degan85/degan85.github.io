---
layout: post
title:  "Docker 기초정리"
author: Degan
categories: DOCKER
tags:	docker 
comments: true
---

# docker 

## 도커 환경 확인

```bash
docker version
```

- linux 위에 올라가 있는걸 볼 수 있음(OS)
- cGroup, namespace 사용 가능(linux container)
        -> container
- image -> container
        -> container


### docker container 생성

```bash
docker run <이미지>
```

- Docker Clinet에서 명령 실행
- Docker daemon에서 image가 없으면 Docker Registry에서 image pull
- container 생성

---

## 기본 명령어

### 이미지 내부 파일 시스템 구조 보기

```bash
docker run <이미지이름> ls
```

### container list

```bash
docker ps
docker ps -a
```

- CONTAINER ID : 컨테이너 고유한 hash id
- IMAGE : 컨테이너 생성 시 사용한 docker image
- COMMAND : 컨테이너 시작 시 실행될 명령어
- CREATED : 컨테이너 생성된 시간
- STATUS : 컨테이너 상태
    - up : 실행 중
    - Exited : 종료
    - Pause : 일시정지 
- PORTS : 컨테이너가 개방한 포트와 호스트에 연결한 포트
- NAMES     
    - container 고유한 이름
    - container 생성 시 --name 옵션으로 지정
    - 지정하지 않으면 docker 엔진이 임의로 설정
    - docker remane original-name chaged-name 변경

### 도커 컨테이너 생명주기

- 생성(create) -> 시작(start) -> 실행(running)

```bash
docker run <이미지 이름>
```

    - 생성(create)

    ```bash
    docker create <이미지 이름>
    ```
    
    - 시작(start) -> 실행(running)

    ```bash
    docker start -a <시작할 컨테이너 아이디/이름>
    ```
    - attach 옵션을 넣어줘야 실행 output도 stream 됨

- 중지(stopped)

```bash
docker stop <중지할 컨테이너 아이디/이름>
docker kill <중지할 컨테이너 아이디/이름>
```

- stop은 **Gracefully**하게 중지(자비롭게)
- kill은 기다리지 않고 바로 중지

- 삭제(deleted)

```bash
docker rm <삭제할 컨테이너 아이디/이름>

#전체 삭제
docker rm `docker ps -a -q`

#image 삭제
docker rmi <삭제할 image id>
docker rmi `docker images -q`

#컨테이너, 이미지, 네트워크 모두 삭제
#실행중인 것은 삭제 안 됨
docker system prune
```

- 실행중인 컨테이너는 중지 후 삭제 가능


### 실행중인 컨테이너에 명령어 전달

```bash
docker run alpine ping localhost

#다른 터미널
docker ps
docker exec <컨테이너 아이디> ls(명령어)
docker exec -it <컨테이너 아이디> sh
```

- run은 새로 컨테이너를 만들어서 실행
- exec은 기존 컨터에너에서 실행
- it (interactive terminal) 옵션
- sh 또는 bash 또는 zsh 등 사용해서 shell 접근 가능

### redis 실행해 보기

```bash
docker run redis
docker exec -it <컨테이너 아이디> redis-cli

#cli 접근
set key1 hello
get key1
```

---

## docker image 생성

- Dockerfile -> docker client -> docker daemon -> image 생성
- Dockerfile : 설정파일
    - 베이스 이미지 명시
    - 추가적인 명령어(파일 스냅샷)
    - 컨테이너 시작 시 실행 될 명령어


### node server image
- package.json

```json
{
  "name": "simple-nodejs",
  "version": "1.0.0",
  "description": "",
  "main": "server.js",
  "scripts": {
    "start": "node server.js"
  },
  "dependencies": {
    "express": "^4.18.1"
  },
  "author": "",
  "license": "ISC"
}
```

server.js

```js
const express = require('express');

const PORT = 8080;
const HOST = '0.0.0.0';

const app = express();
app.get('/', (req, res) => {
    res.send('Hello World');
})

app.listen(PORT, HOST);
console.log('Running on');
```

```Dockerfile
FROM node:10

WORKDIR /usr/src/app

#소스 변경되도 빌드 다시 안하게
COPY package.json ./
RUN npm install

#컨테이너 WORKDIR 경로에 복사
#소스파일 변경될때 마다 빌드 다시해야해서 volume 사용
COPY ./ ./

CMD ["node", "server.js"]
```

```bash
docker build -t <image 이름> ./
docker build -t <dockerId/프로젝트이름/versin> ./

#docker run -d -p 5000:8080 <dockerId/프로젝트이름/versin>
#소스파일 변경될때 마다 빌드 다시해야해서 volume 사용
#node_modules는 컨테이너 사용, workdir 경로 소스는 local 소스 매핑
docker run -d -p 5000:8080 -v /usr/src/app/node_modules -v $(pwd):/usr/src/app <dockerId/프로젝트이름/versin>
```

- localhost:5000으로 접속


---

## docker-compose

### docker-compose.yml

```yml
version: '3'
services:
  redis-server:
    image: "redis"
  node-app:
    build: .
    ports:
      - "5000:8080"
```
- versino
    - docker compose 버전
- services
    - 사용되는 container 정의(이름 정의)
    - build : 현 디렉토리에 있는 Dockerfile 사용
    - port : 포트매핑 ( 로컬 port:컨테이너 port) 
   
### Dockerfile

```Dockerfile
FROM node:18

WORKDIR /src

COPY ./ ./

RUN npm install

CMD ["node", "server.js"]
```

### server.js

```js
const express = require('express');
const redis = require('redis');

// docker-compose.yml에 명시한 컨테이너 이름을 host에 넣음
const client = redis.createClient({
    socket: {
        host:"redis-server",
        port: 6379
    }
});

const app = express();

app.get('/', async (req, res) => {

    await client.connect();
    let number = await client.get("number");

    if(number===null) {
        number = 0;
    }
    console.log(`number : ${number}`);
    res.send(`number : ${number}`);

    await client.set("number", parseInt(number)+1);

    await client.disconnect();

});

app.listen(8080);
console.log('Server is running');
```

### 실행
```bash
docker-compose up

#output없이 백그라운드에서 실행
docker-compose up -d

#재빌드
docker-compose up --build
```

### 중단
```bash
docker-compose down
```

---
## Reference
[따라하며 배우는 도커와 CI환경](https://www.inflearn.com/course/%EB%94%B0%EB%9D%BC%ED%95%98%EB%A9%B0-%EB%B0%B0%EC%9A%B0%EB%8A%94-%EB%8F%84%EC%BB%A4-ci/dashboard)
