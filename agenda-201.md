# TAP HOL 201
Tanzu Application Platform Hands-On-Labs 201 페이지 입니다.

## 설치 제품
본 가이드에서 다루는 Tanzu 설치 솔루션 및 버전은 다음과 같습니다.
|제품명|Version|
|---|---|
|TAP|1.4.2|

TAP 설치를 위해서는 아래와 같은 Kubernetes Cluster가 필요합니다. 이 Lab에서는 TKG를 이용해서 진행합니다.
- Kubernetes v1.23 ~ v1.25 필요
- AKS / EKS / GKE / Minikube / TKGm / TKGs (vSphere with Tanzu v7.0 U3f)


## 0. 사전 준비
- TAP 설치
  - Cluster 2개 : (profile : full, run)
- 워크로드 배포 테스트
  - Supply Chain: basic으로 설치된 TAP에서 워크로드 배포 테스트
- [VS Code IDE 설치하기](./tap/ide.md)

## 1. 개발자용 네임스페이스 설정
- [개발자용 네임스페이스 설정](./install/dev-namespace.md)

## 2. TAP GUI 둘러보기
- [catalog 등록](./tap/catalog.md)
- Postgres 연결해서 TAP GUI 메타 정보 저장
- [App Live View]()

## 3. Supply Chain 구성
OOTB Basic Supply Chain으로 설치된 TAP를 Testing and Scanning으로 변경합니다. 
- [Supply Chain - Testing and Scanning](./tap/ootb-testing-and-scanning.md)

## 4. GitOps 구성
구성 정보를 자동적으로 git repository에 push하도록 gitops 환경을 구성합니다.
- [GitOps](./tap/gitops.md)

## 5. 개발 환경 개선을 위한 IDE
- [VS Code IDE 설치하기](./tap/ide.md)

## 6. 동적 배포와 원격 디버깅
- [동적 배포와 원격 디버깅](./tap/hotdeploy_debug.md)

## 7. Accelerator 커스터마이징
- [Accelerator 커스터마이징]()

