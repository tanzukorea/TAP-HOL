# TAP Lab 사전 준비사항

## Kubernetes Cluster
TAP 설치를 위해서는 아래와 같은 Kubernetes Cluster가 필요합니다.
- Kubernetes v1.23 ~ v1.25 필요
- AKS / EKS / GKE / Minikube / TKGm / TKGs (vSphere with Tanzu v7.0 U3<font color="red">f</font>)
이 Lab에서는 TKG를 이용해서 진행합니다.

## TAP 설치
본 가이드에서 다루는 Tanzu 설치 솔루션 및 버전은 다음과 같습니다.
|제품명|Version|
|---|---|
|TAP|1.4.2|
- 3개의 클러스터에 profile이 full, build, run인 TAP 각각 설치
- cluster 이름은 profile이름과 동일하게 full, build, run으로 각각 지정합니다.
- TAP Mutlti Cluster 설치는 [여기](https://docs.vmware.com/en/VMware-Tanzu-Application-Platform/1.4/tap/multicluster-installing-multicluster.html) 참조

## 워크로드 배포 테스트
- Supply Chain: basic으로 설치된 TAP에서 워크로드 배포
- full 클러스터에서 워크로드를 배포하면 됩니다. build, run 클러스터는 TAP Hands-On-Lab 201 행사 당일 gitOps 실습을 위해 사용할 예정입니다.
- 워크로드 배포는 [여기](https://github.com/tanzukorea/TAP-HOL/blob/main/tap/app-deploy.md) 참조
  
## 개발 환경 준비
1. 개발 환경을 위해 Local PC에 아래의 툴들을 설치합니다.
   - Visual Studio Code 설치 : https://code.visualstudio.com/download
   - Git 설치 : https://git-scm.com/downloads
   - Docker Desktop 설치 : https://www.docker.com/products/docker-desktop/

2. 본 Lab에서는 개발자 소스 저장소로 Github을 사용합니다. github 계정을 생성합니다.
   - Github 계정 생성 : https://github.com

## 각 클러스터 별 최소 리소스
- 8 CPUs for i9 (or equivalent) available to Tanzu Application Platform components.
- 12 CPUs for i7 (or equivalent) available to Tanzu Application Platform components.
- 8 GB of RAM across all nodes available to Tanzu Application Platform.
- 12 GB of RAM is available to build and deploy applications, including Minikube. VMware recommends 16 GB of RAM for an optimal experience.
- 70 GB of disk space available per node.

