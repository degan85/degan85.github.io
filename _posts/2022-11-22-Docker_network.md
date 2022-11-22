---
layout: post
title:  "Docker Network 정리"
author: Degan
categories: DOCKER 
comments: true
tags: DOCKER NETWORK CONTAINER
---

## Docker Network 정리

- 실행중인 Docker Container 간의 통신을 위한 논리적 네트워크

- 네트워크 조회

```bash
docker network ls

# bridge: the default network driver. 하나의 호스트 컴퓨터 내에서 여러 컨테이너들이 서로 네트워킹
# host:호스트 컴퓨터와 동일한 네트워크에서 컨테이너 실행
# none: 여러 호스트에 분산되어 실행되는 컨테이너들간의 네트워킹
```


- 네트워크 생성

```bash
docker network create ${network-name}
# docker network create test-net
```

- 네트워크 상세 정보

```bash
docker network inspect ${network-name}
# docker network inspect test-net

# Containers 항목에서 연결된 네트워크 확인
```

- 네트워크 컨테이너 연결 및 해제

```bash
# 컨테이너 one이라는 이름으로 실행
docker run -itd -name one busybox

# --network 옵션을 명시하지 않으면 기본적으로 bride로 기본 설정
docker network inspect bridge

# "Containers": {
# 	"...9c3689d": {
# 		"Name": "one",
# 		"EndpointID": "...cb994298f2af4c6d45bae5215a4f4",
# 		"MacAddress": "02:42:ac:11:00:02",
# 		"IPv4Address": ${ip1},
# 		"IPv6Address": ""
# 	}
# },

# one 컨테이너에 test-net 연결
docker network connect test-net one

# "Containers": {
# 	"...9c3689d": {
# 		"Name": "one",
# 		"EndpointID": "...cb994298f2af4c6d45bae5215a4f4",
# 		"MacAddress": "02:42:ac:12:00:02",
# 		"IPv4Address": ${ip1},
# 		"IPv6Address": ""
# 	}
# },

# bfidge에서 network 해제
docker network disconnect bridge one

# container two도 생성해서 test-net 네트워크에 연결
docker run -itd --name two --network test-net busybox

docker network inspect test-net
# "Containers": {
# 	"...9c3689d": {
# 		"Name": "two",
# 		"EndpointID": "...cb994298f2af4c6d45bae5215a4f4",
# 		"MacAddress": "02:42:ac:12:00:03",
# 		"IPv4Address": ${ip2},
# 		"IPv6Address": ""
# 	},
# 	"...ecd011528d622ed649c3689d": {
# 		"Name": "one",
# 		"EndpointID": "...6e0560b2b10c79bea23bf785d",
# 		"MacAddress": "02:42:ac:12:00:02",
# 		"IPv4Address": ${ip1},
# 		"IPv6Address": ""
# 	}
# },
```

- ping으로 네트워킹 확인

```bash
# one -> two container name으로 확인
docker exec one ping two

# PING two (${ip2}): 56 data bytes
# 64 bytes from ${ip2}: seq=0 ttl=64 time=0.216 ms
# 64 bytes from ${ip2}: seq=1 ttl=64 time=0.086 ms
# 64 bytes from ${ip2}: seq=2 ttl=64 time=0.086 ms

# two -> one container ip로 확인
docker exec two ping ${ip1}
# PING ${ip1} (${ip1}): 56 data bytes
# 64 bytes from ${ip1} seq=0 ttl=64 time=0.262 ms
# 64 bytes from ${ip1} seq=1 ttl=64 time=0.109 ms
# 64 bytes from ${ip1} seq=2 ttl=64 time=0.140 ms
```

- 네트워크 제거

```bash
# 컨테이너 중지
docker stop one two

# 네트워크 제거
docker network rm test-net

# 네트워크 청소
docker network prune
```

---

## Reference

- [Networking overview](https://docs.docker.com/network/)
- [Docker 네트워크 사용법](https://www.daleseo.com/docker-networks/)

