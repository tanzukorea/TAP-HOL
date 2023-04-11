# TAP Hands-On-Lab 201
![](./images/taphol_logo.png)

## Introduction
본 핸즈온 문서는 Tanzu Application Platform에서 제공하는 개발자 포탈인 TAP GUI를 살펴보고, 내장되어 있는 supply chain을 이용해 쉽게 application 워크로드를 배포하는 실습을 해보게 됩니다. 또한 멀티클러스터 환경에서 배포하기 위한 방법으로 gitops를 이용하여 승인과정을 거쳐 운영 클러스터에 배포를 하는 과정을 실습을 합니다. 마지막으로 개발자들을 생산성 향상을 위해 제공하는 IDE 플러그인을 사용하여 워크로드를 배포하는 부분을 실습합니다.

## Objective
- 다양한 워크로드 배포 방식의 이해
- TAP GUI 둘러보기
- Application의 자동화된 배포를 위한 SupplyChain
- 개발자 생산성 향상을 위한 개발툴 사용 방법
- 멀티 클러스터 환경에서의 TAP 활용방법의 이해


## 설치 제품
본 가이드에서 다루는 Tanzu 설치 솔루션 및 버전은 다음과 같습니다.
|제품명|Version|
|---|---|
|TAP|1.4.2|

## 0. 사전 준비 사항
- [Lab 환경을 위한 사전 준비사항](./install/lab_prepare.md)

## 1. 워크로드 배포
- [워크로드 배포를 위한 네임스페이스 설정](./install/dev-namespace.md)
- [다양한 방식의 워크로드 배포](./tap/workload_deploy.md)
- [Local에 있는 파일 워크로드 배포](./tap/workload_local.md)
- [Dockerfile 기반 워크로드 배포](./tap/workload_dockerfile.md)

## 2. TAP GUI
- [Catalog 등록](./tap/catalog.md)
- [Postgres 연결해서 TAP GUI 메타 정보 저장](./tap/gui-meta.md)
- [Application Live View 둘러보기](./tap/alv.md)

## 3. Supply Chain 구성
OOTB Basic Supply Chain으로 설치된 TAP를 Testing and Scanning으로 변경합니다. 
- [Supply Chain - Testing and Scanning](./tap/ootb-testing-and-scanning.md)

## 4. 싱글 클러스터 환경을 위한 GitOps 구성 
구성 정보를 자동적으로 git repository에 push하도록 gitops 환경을 구성합니다.
- [GitOps 구성](./tap/gitops.md)

## 5. 개발 환경 구성
- [개발 IDE 구성하기](./tap/ide.md)
- [IDE 살펴보기](./tap/ide2.md)
- [IDE에서 Accelerator로 개발 시작하기](./tap/ide_accelerator.md)
- [동적 배포와 원격 디버깅](./tap/hotdeploy_debug.md)

## 6. Accelerator 커스터마이징
- [Accelerator 커스터마이징](./tap/accelerator.md)

## 7. 멀티 클러스터 구축 및 설정
- [멀티 클러스터 구축 및 설정](./tap/multi-cluster.md)
- [멀티 클러스터 GitOps 구성 ](./tap/multi-gitops.md)
