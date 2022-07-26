# 면접을 위한 CS 전공지식 노트 - 2.3 네트워크 기기

# 네트워크 기기의 처리 범위

애플리케이켠 계층 : L7 스위치

인터넷 계층 : 라우터, L3 스위치

데이터 링크 계층 : L2 스위치, 브리지

물리 계층 : NIC, 리피터, AP

# 애플리케이션 계층을 처리하는 기기

## L7 스위치

통신 네트워크 장비

로드밸런서

트래픽 모니터링 가능

장애 발생 시 트래픽 분산 대상에서 제외 → 헬스 체크 통해서 확인

### L4 스위치 vs L7 스위치

**L4 스위치**

- 인터넷 계층을 처리
- 스트리밍 관련 서비스에서는 사용 불가
- 메시지를 기반으로 인식하지 못하고 IP와 포트를 기반으로 트래픽 분산
- ALB(Application Load Balancer) 컴포넌트

**L7 로드밸러서**

- IP, 포트 외에도 URL, HTTP 헤더, 쿠키 등을 기반으로 트래픽 분산
- NLB(Network Load Balancer) 컴포넌트

### 로드밸런서를 이용한 서버 이중화

2대 이상의 서버를 기반으로 가상 IP를 제공

→ 프록시 서버

# 인터넷 계층을 처리하는 기기

## L3 스위치

L2 스위치의 기능과 라우팅 기능을 갖춘 장비

| 구분 | L2 스위치 | L3 스위치 |
| --- | --- | --- |
| 참조 테이블 | MAC 주소 테이블 | 라우팅 테이블 |
| 참조 PDU | 이더넷 프레임 | IP 패킷 |
| 참조 주소 | MAC 주소 | IP 주소 |

### **라우터**

여러 개의 네트워크를 연결, 분할, 구분

다른 네트워크에 존재하는 장치끼리 서로 데이터를 주고받을 때 패킷 소모를 최소화하고 경로를 최적화하여 최소 경로로 패킷을 포워딩

# 데이터 링크 계층을 처리하는 기기

## L2 스위치

MAC 주소를 MAC 주소 테이블을 통해 관리

연결된 장치로부터 패킷이 왔을 때 패킷 전송을 담당

## 브리지

두 개의 근거리 통신만(LAN)을 상호 접속할 수 있도록 하는 통신망 연결 장치

포트와 포트 사이의 다리 역할

# 물리 계층을 처리하는 기기

## NIC

LAN 카드

네트워크와 빠른 속도로 데이터를 송수신할 수 있도록 컴퓨터 내에 설치하는 확장 카드

MAC 주소를 지님

## 리피터

들어오는 약해진 신호 정도를 증폭하여 다른 쪽으로 전달

## AP (Access Point)

패킷을 복사하는 기기

유선 LAN을 연결할 후 다른 장치에서 무선 LAN 기술을 사용하여 무선 연결 가능