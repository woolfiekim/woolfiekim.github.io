---
layout: single
title:  "[spring http] Network 기본 지식"
categories: 
    - spring  #카테고리
tag: [Spring, http] #태그
toc: true  #오른쪽에 있는 목차
toc_sticky: true #목차 고정
author_profile: true  #왼쪽에 자기 소개란 프로필을 이 페이지에 들어갈때 끄는 기능
sidebar:
    nav: "sidebar-category" #navigation.yml에 있는 docs를 뜻한다.
search: true  #이 페이지는 검색에 나옴.
---

## IP(Internet protocol)
- 역할
  - IP 주소에 데이터 전달
  - Packet(통신단위)로 데이터 전달
- 한계
  - 비연결성 : packet을 받을 대상이 없거나 서비스 불능 상태여도 패킷 전송(대상 서버가 패킷을 받을 수 있는 상태인지 모른다.)
  - 비신뢰성 : packet이 중간에 사라지거나 순서대로 안 올 수도 있다.
  - 프로그램 구분 : 같은 IP를 사용하는 서버에서 통신하는 앱이 둘 이상일 경우

## TCP(Transmission Control Protocol)
TCP/IP 패킷 정보
1. IP 패킷 : 출발지 IP, 목적지 IP....
2. TCP 세그먼트 : 출발지 PORT, 목적지 PORT, 전송 제어, 전송 순서, 검증 정보...
3. 전송데이터

- 특징
  - 연결지향 : 연결이 되야한다.
  - 데이터가 전달이 되었다는 것을 보증한다.
  - 순서를 보장한다. 
  - 신뢰할 수 있는 프로토콜
  - 현재는 대부분 TCP 사용

- TCP 3 way handshake(데이터 전송 전 연결 과정)
  1. 클라이언트 -> 서버 : SYN(접속 요청)
  2. 서버 -> 클라이언트 : SYN(접속 요청) + ACK(요청수락)
  3. 클라이언트 -> 서버 : ACK(요청 수락) (+ 데이터 전송도 가능)
  4. 서버와 클라이언트간 데이터 전송

- 데이터 전달 보증
  1. 클라이언트 -> 서버 : 데이터 전송
  2. 서버 -> 클라이언트 : 데이터 잘 받았다고 보냄

- 순서 보장
  1. 패킷1, 패킷2, 패킷3 순서대로 전송
  2. 패킷1, 패킷3, 패킷2 순서대로 도착하면
  3. 패킷2 부터 다시 보내라고 연락한다.

## UDP(User Datagram Protocol)
- IP와 거의 같다. + PORT + 체크섬 정도만 추가
- 순서보장안하고, 데이터 전달 보증안한다.
- 단순하지만 빠르다.
- 최근에 뜨고 있다.
  
## PORT
- 같은 IP 내에서 프로세스 구분해준다.
  
- 포트 번호
  - 0 ~ 65535 할당가능
  - 0 ~ 1023 : 잘 알려진 포트, 사용하지 않는 것이 좋음
    - FTP : 20, 21
    - TELNET : 23
    - HTTP : 80
    - HTTPS : 443

## DNS(Domain Name System)
- IP는 기억하기 어렵고, 변경될 수 있다.
- 그래서 이것을 기억하기 쉽도록 이름으로 대신 쓰는 것이다.


> 본 포스팅은 김영한 선생님의 강의를 보고 정리한 글입니다. 