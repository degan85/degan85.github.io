---
layout: post
title:  "docker compose에 환경변수 반영"
author: Degan
categories: DOCKER 
comments: true
tags: DOCKER 환경변수 env
---

## docker compose에 환경변수 반영

- docker-compose의 경우 따로 설정없이 
- .env 파일에서 변수 사용 가능
```
KEY1=VALUE1
MYSQL_ROOT_PASSWORD=VALUE2
```

```yml
version: '3.9'
services:
  mysql:
  environment:
	  MYSQL_ROOT_PASSWORD: ${MYSQL_ROOT_PASSWORD}
```


- 개발, 운영 구분하려면
- --env-file ./config/.env.dev 플래그 이용
	
```yml
# env.dev
MYSQL_ROOT_PASSWORD=DEV_PASSWORD


# env.prod
MYSQL_ROOT_PASSWORD=PROD_PASSWORD
```

```bash
$ docker-compose --env-file ./.env.dev config
# or
$ docker-compose --env-file ./.env.dev up
```

---
## Reference

- [Environment variables in Compose](https://docs.docker.com/compose/environment-variables/)

