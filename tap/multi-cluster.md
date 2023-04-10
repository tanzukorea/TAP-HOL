### 멀티 클러스터 구성 및 설정하기

다양한 환경과 구성으로 유연하게 Tanzu Application Platform을 설치 할 수 있습니다. VMware는 Production용으로 멀티클러스터 토폴로지를 테스트하고 권장합니다. 
유연성과 선택권이 Tanzu Application Platform의 설계의 핵심이기 때문에 구현 권장 사항은 명확하게 정해져 있지 않습니다. 이번 실습은 TAP를 멀티 클러스터로 구성하여 POC를 진행 할 경우 최소의 클러스터로
구성 및 설정을 진행해보겠습니다. Full로 설치된 클러스터는 View,Iterator 공통으로 사용되는 클러스터 이며, Build 클러스터는 OOTB 파이프라인으로 실제로 빌드가 실행되는 클러스터, 
마지막으로 Run 클러스터는 배포할 workload가 배포되는 클러스터입니다.


![](../images/multi-cluster-Architecture.png)


- 각 클러스터 별 설정 정보는 다음과 같습니다. 설치 가이드는 각 클러스터 명을 클릭하시기 바랍니다.
   |클러스터 명|프로파일 명|Supply Chain|용도|
   |---|---|---|---|
   |[full](../install/multicluster/full/install-full.md)|full|basic|1) 개발자가 사용할 iterate 클러스터 및 2)TAP GUI가 설치될 view 클러스터|
   |[build](../install/multicluster/buildc/install-build.md)|build|testing_scanning|소스 및 이미지를 테스트, 스캐닝하고 워크로드를 빌드하여 이미지를 생성할 클러스터|
   |[run](../install/multicluster/run/install-run.md)|run|-|워크로드가 구동될 클러스터|
- TAP Multi Cluster 설치에 대한 자세한 설치 방법은 [여기](https://docs.vmware.com/en/VMware-Tanzu-Application-Platform/1.4/tap/multicluster-installing-multicluster.html)를 참조하시기 바랍니다.




