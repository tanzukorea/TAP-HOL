# TAP Lab 사전 준비사항

## Kubernetes Cluster
TAP 설치를 위해서는 아래와 같은 Kubernetes Cluster가 필요합니다.
- Kubernetes v1.23 ~ v1.25 필요
- AKS / EKS / GKE / Minikube / TKGm / TKGs (vSphere with Tanzu v7.0 U3f)
이 Lab에서는 TKG를 이용해서 진행합니다.

### 각 클러스터 별 최소 필요 리소스
- 8 CPUs for i9 (or equivalent) available to Tanzu Application Platform components.
- 12 CPUs for i7 (or equivalent) available to Tanzu Application Platform components.
- 8 GB of RAM across all nodes available to Tanzu Application Platform.
- 12 GB of RAM is available to build and deploy applications, including Minikube. VMware recommends 16 GB of RAM for an optimal experience.
- 70 GB of disk space available per node.

## TAP 설치
-  본 가이드에서 다루는 Tanzu 설치 솔루션 및 버전은 다음과 같습니다.
   |제품명|Version|
   |---|---|
   |TAP|1.4.2|
- 각 클러스터 별 설정 정보는 다음과 같습니다. 설치 가이드는 각 클러스터 명을 클릭하시기 바랍니다.
   |클러스터 명|프로파일 명|Supply Chain|용도|
   |---|---|---|---|
   |[full](./multicluster/full/install-full.md)|full|basic|1) 개발자가 사용할 iterate 클러스터 및 2)TAP GUI가 설치될 view 클러스터|
   |[build]|build|testing_scanning|소스 및 이미지를 테스트, 스캐닝하고 워크로드를 빌드하여 이미지를 생성할 클러스터|
   |[run]|run|-|워크로드가 구동될 클러스터|
- TAP cluster(full profile : worker 3 node)<br>
  run, build (TKG cluster : worker 3 node)
- TAP Multi Cluster 설치에 대한 자세한 설치 방법은 [여기](https://docs.vmware.com/en/VMware-Tanzu-Application-Platform/1.4/tap/multicluster-installing-multicluster.html)를 참조하시기 바랍니다.

## 워크로드 배포 테스트
- full 클러스터의 Supply Chain이 basic으로 설정되었는지 확인합니다.
- [여기](https://github.com/tanzukorea/TAP-HOL/blob/main/tap/app-deploy.md)를 참조해서 full 클러스터에 워크로드 배포합니다. build, run 클러스터는 TAP Hands-On-Lab 201 행사 당일 gitOps 실습을 위해 사용할 예정입니다.


## 개발 환경 준비
1. 개발 환경을 위해 Local PC에 아래의 툴들을 설치합니다.
   - Visual Studio Code 설치 : https://code.visualstudio.com/download
   - Git 설치 : https://git-scm.com/downloads
   
2. 본 Lab에서는 개발자 소스 저장소로 Github을 사용합니다. github 계정을 생성합니다.
   - Github 계정 생성 : https://github.com
