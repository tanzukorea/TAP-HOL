# TAP-HOL
Tanzu Application Platform Hands-On-Labs 페이지 입니다.

## 설치 제품
본 가이드에서 다루는 Tanzu 설치 솔루션 및 버전은 다음과 같습니다.
|제품명|Version|
|---|---|
|TAP|1.2.1|

* TAP설치를 위해서는 아래와 같은 Kubernetes Cluster가 필요합니다. 이 Lab에서는 TKG를 이용해서 진행합니다.
  - Kubernetes v1.22 or v1.23 이상 필요
  - AKS / EKS / GKE / Minikube / TKGm / TKGs (vSphere with Tanzu v7.0 U3a)


## 0. 사전 준비
- DNS Server
- Avi Load Balancer
- [Jumpbox 준비 및 TKG 설치하기](./tap/jumpbox-prepare.md)

## 1. TAP 환경 구성
- [TAP 설치](./install/install-on-vsphere-hol.md)
- [개발자용 네임스페이스 설정](./install/dev-namespace.md)

## 1. TAP 둘러보기
TAP GUI를 둘러보며 기능을 알아봅니다.
- [TAP GUI](./tap/gui.md)

## 2. 애플리케이션 배포
간단한 Spring Boot 애플리케이션을 TAP에 배포 후 CLI 및 GUI로 확인합니다.
- [애플리케이션 배포](./tap/app-deploy.md)

## 3. TAP Supply Chain 구성
OOTB Basic Supply Chain을 Testing and Scanning으로 변경합니다.    
구성 정보를 자동적으로 git repository에 push하도록 gitops 환경을 구성합니다.
- [Supply Chain - Testing and Scanning](./tap/ootb-testing-and-scanning.md)
- [GitOps](./tap/gitops.md)

## 4. 개발 환경 개선을 위한 IDE 경험
- IDE에서 앱 동적 배포하기
- 원격 디버깅

## 5. TAP Learning Center
TAP에서 제공하는 Learning Center를 확인합니다
- [Learning Center](./tap/learning-center.md)

## 6. Optional Lab
다음 Hands On Lab은 선택 사항입니다.
- [멀티 클러스터](./tap/multi-cluster.md)


