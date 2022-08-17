---
title: Getting Started with Google Kubernetes Engine  
author: Kyuwan Kim
date: 2022-08-13 ~ 
category: Kubernetes
layout: post
---

> 1주차

## Google Cloud 소개

### 모듈 소개

- Cloud Computing의 이해
- Identify Google Cloud's compute services
- region과 zone에 대한 이해
- Cloud Resource 계층구조에 대한 이해
- Google Cloud Resource 관리 방법

### 클라우드 컴퓨팅과 Google Cloud

- Cloud Computing의 5가지 기본 속성
  - Computing resource가 주문형 및 self service 방식으로 제공 됨
  - 어디서나 Network를 통해 resource에 access 가능
  - 제공업체에서 대규모 pool의 resource를 고객에게 할당하므로 고객은 규모의 경제가 주는 이점을 얻음
  - Resource가 빠르고 탄력적으로 공급 됨
  - 사용하거나 예약하는 만큼만 지불하면 됨

- Google Cloud
  - Compute Engine: Cloud 환경에서 주문형 가상 머신을 실행하는 IAAS
  - Google Kubernetes Engine(GKE): Cloud 환경에서 컨테이너화 된 App 실행. 사용자에게 권한 부여
  - App Engine: GCP의 완전 관리형 PAAS Framework. (infra에 대한 고민 없이 code 실행)
  - Cloud Functions: 완전 Serverless 실행 환경

### 리소스 관리

#### regions와 zones에 대한 이해

Google Cloud는 multi-region, region, zone을 통해 resource를 제공

- Multi-region: 전 세계를 3개 지역으로 나눔(아메리카, 유럽, 아시아 태평양)
- Region
  - 같은 대륙 내에서 독립된 지리적 위치
  - Region 내에서는 network 연결이 빠름
- Zone
  - 집중된 지리적 위치 내에 있는 GCP resource의 배포 위치
  - 한 region 내에 위치한 data center == zone
  - 엄밀히 말하면 하나의 zone이 반드시 하나의 data center인 것은 아님
  - Compute Engine 가상 머신 instance는 특정 zone 내에 위치  

Region과 zone이 GCP resource를 물리적으로 구성한다면 Project는 논리적으로 구성한다고 할 수 있다.

- Resource는 계층 구조를 가진다.
  - Projects  
    - 각 project는 고유한 project ID와 project 번호로 식별됨
    - project의 이름과 label은 변경 가능하지만, ID와 번호는 고정됨
  - Folders
    - Project를 group으로 묶음
    - 기업의 계층 구조를 반영하고 정책을 알맞은 수준에서 적용 할 수 있음
    - Folder간 중첩 가능 (하위 folder)
  - Organization(조직)
    - Folder를 group으로 묶음
    - GCP resource 계층 구조의 root node
    - Organization 없이 GCP를 사용 할 수 있다
    - 기업의 전반에 적용되는 정책을 설정 가능
    - Folder를 사용하려면 Organization 필요
    - Organization ID는 고정, 표시 이름은 변경 가능

- Cloud IAM(Cloud Identity and Access Management)
  - IAM 정책을 정의하면 사용자가 사용하는 모든 GCP resource에 세부적인 사용자 access의 제어가 가능
  - 사용자가 선택한 수준에서 적용한 정책은 낮은 수준으로 상속 됨
  - 결제는 project 수준에서 누적 됨

- Cloud security and Google Cloud
  - Onpremises infra에 app을 구축할 경우 사용자가 stack 전체에 대한 보안을 책임져야하나, GCP를 이용하면 Google에서 하위 layer의 보안을 담당
  - Google에서 하위 layer의 보안을 담당하므로 대체로 안전함
  - 상위 layer의 보안은 여전히 사용자의 책임

### 청구

#### 결제 작동 방식

GCP 결제는 GCP project 수준에서 설정
GCP project를 정의할 때 결제 계정을 연결해야 함
결제 계정에서 사용자는 지불 option을 비롯한 모든 결제 정보를 설정함

- 결제 계정은 하나 이상의 project에 연결 할 수 있음
- 결제 계정을 연결하지 않은 프로젝트는 무료 GCP 서비스만 사용 가능
- 자동으로 청구 및 invoice 되도록 설정 할 수 있음
- 결제 하위 계정을 설정하면 project 결제를 분리 할 수 있음

#### 결제 청구를 관리하는 방법

- 예산 및 알림
- 결제 내보내기에서 보기
- 보고서 보기
결제 계정 수준 또는 project 수준에서 예산을 정의할 수 있음

#### 할당량, 유용한 제한(limits)

##### 속도 할당량(Rate quota)

GCP project에서 API에 보내는 호출을 100초당 1,000 건으로 제한하는 할당량을 구현
100초가 지나면 한도가 재설정 됨
GKE에서 실행되는 App에서 받는 호출의 비율이 아닌 GKE cluster 자체에서 받는 호출을 제한하는 방식
짧은 시간 안에 많은 호출을 보내는 일반적이지 않은 오류성 행동을 할당량을 통해서 차단

##### 배정 할당량(Allocation quota)

- project 내에서 보유할 수 있는 resource 수를 제어
- 시간이 지나도 재설정되지 않음  
- 5 network / project

할당량은 변경 가능하다.

### Google Cloud와 상호작용

#### Google Cloud와 상호작용 할 수 잇는 방법

- Google Cloud Console (Web user interface)
- Cloud SDK and Cloud Shell (Command-line interface)
- Cloud Console mobile app (For iOS and Android)
- REST-based API (For custom applications)

#### The Cloud Console

- 모든 Google 클라우드 resource를 관리하는 웹 기반 GUI
- 간단한 마우스 클릭을 사용하여 일반적인 작업을 실행
- Google Cloud project 및 resource에 대한 가시성을 제공

#### Cloud Shell

- Command-line은 browser를 통하여 cloud resource에 직접 access 가능
- Gcloud command-line 도구 및 기타 유틸리티의 지속적인 가용성 제공
- 임시 Computing Engine 가상 머신 인스턴스 사용(비용X)

#### Cloud Console mobile app

- Compute Engine instance에 ssh 연결, 시작, 중지 가능
- 최신 청구 정보 및 경고 수신 가능
- 주요 metric을 보여주는 사용자 정의 가능한 graph를 설정 가능

### 컴퓨팅 옵션

| Service Name | Description |
|---|---|
| Kubernetes Engine | 배포 container화 된 application을 위한 관리 환경 |
| Compute Engine | 가상 머신을 배포하기위한 관리 환경 |
| App Engine | Application을 배포하기위한 관리 된 Serverless platform |
| Cloud Funtions | Event-driven Function을 배포하기위한 관리되는 Serverless platform |

Kubernetes Engine is the focus of this specialization. You will learn more about all of these choices in a later module.

### 실습: Google Cloud Console 및 Cloud Shell에 액세스하기


### 요약: 동영상

### Google Cloud 소개: Test

## 컨테이너 및 Kubernetes 소개

### 모듈 소개

 소프트웨어 컨테이너와 애플리케이션 배포 측면의 이점을 알아봅니다. 컨테이너를 구성하고 빌드하며, Kubernetes 등의 관리 솔루션에서 제공하는 함수 컨테이너에 대해서도 학습합니다. 자체 컨테이너 관리 인프라를 구축할 경우와 비교하여 Google Kubernetes Engine의 이점을 살펴봅니다.

### 컨테이너 및 컨테이너 이미지

우리는 기존에 각각의 서버에 application을 build하여 사용했다. 
> Dedicated server: Hardware > Kernel > Dependencies > Application code

Hypervisor: 가상 머신의 생성 및 관리
> Virtual machine: Hardware + Hypervisor > Kernel > Dependencies > Application code





### 실습: Cloud Build로 작업하기

### Kubernetes 소개

### Google Kubernetes Engine 소개

### 컴퓨팅 옵션 세부정보

### 요약

### 퀴즈: 컨테이너 및 컨테이너 이미지

## Kubernetes 아키텍처

### 모듈 소개

### Kubernetes 개념

### Kubernetes 제어 영역

### Google Kubernetes Engine 개념

### Kubernetes 객체 관리

#### Kubernetes 객체 관리

#### 배포 및 ReplicaSet 참고사항

#### Kubernetes 객체 관리 연습 퀴즈

#### 서비스 참고사항

#### 알아야 할 컨트롤러 객체

#### Kubernetes 컨트롤러 객체 연습 퀴즈

### 실습: Google Kubernetes Engine에 배포

### Migrate for Anthos

### 요약

### 퀴즈: Kubernetes 아키텍처

### 결론

## Kubernetes 워크로드 소개

### 모듈 소개

### Kubectl 명령어

### 배포

### 포드 네트워킹

### 볼륨

### 요약
