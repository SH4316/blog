---
title: Untitled
slug: untitled-2
date: 2026-10-26
description: ""
tags: []
draft: true
---
### 시작한 이유
Sandolri 프로젝트에 필요해서

### 동작 구조
로그인 page 를 keycloak에서 제공해서 인증, 인증정보를 연결된 service 에서 동시에 사용할 수 있는 구조

### 실행 방법
실행은 docker를 이용하기로 결정

```bash
docker run --privileged=true --port "9090:9090" -d \ # port 설정
		--env KC_BOOTSTRAP_ADMIN_USERNAME=admin \ 
		--env KC_BOOTSTRAP_ADMIN_PASSWORD=1234 \
		quay.io/keycloak/keycloak \ # image
		start-dev --http-port=9090 --hostname="192.168.0.5" # dev 용으로 실행(start-dev)
```

개발 환경이 "공유기 vpn 이용 -> 내부망에서 Linux server 접근" 이라 hostname 지정

- 처음에 접근 안되서 `--hostname 0.0.0.0` 으로 설정
- 호스트에서 접근하라고 해서 ssh port forwarding(-L 9090:9090) 이용해서 접근 시도
- 어드민 설정 안한걸 알고 설정함
- 안돼서 마지막으로 실 접속 주소인 `--hostname 192.168.0.5` 으로 설정

### 개념
Realm
### 연동


### 부족한점
네트워크 제한이 빡빡하게 걸린 상태로 운영하는게 기본일 것 같은데 관련 설정을 숙지하지 못한 느낌


