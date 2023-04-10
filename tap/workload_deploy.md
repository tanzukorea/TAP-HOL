## 다양한 방식의 워크로드 배포
1. TAP에서 지원하고 있는 SupplyChain을 통한 Build 방법은 3가지가 있으며, 본 워크샵에서는 Git Source 및 Dockerfile 방식을 통한 Build를 실슬할 예정입니다.             
- Building from source     
  : 기본 방식으로 Git 혹은 Local 에 있는 Source를 참조하여 Build 하는 방식입니다.    
      예시)      
      tanzu apps workload create tanzu-java-web-app \      
      --app tanzu-java-web-app \          # 배포되는 Application 이름      
      --type web \                        # Application default Type      
      --git-repo https://github.com/sample-accelerators/tanzu-java-web-app \   # Git Source URL       
      --git-branch main     # Source Branch       
- Using a prebuilt image       
  : 기존 Container Image를 사용하여 Build 가능합니다.        
  : 다만, 이를 위해서는 몇가지 선행 작업이 필요합니다.         
    -> Application 에서 사용되는 Container Port는 8080으로 제한됩니다.(TAP의 delivery에 사용되는 Knative 의 표준 Port 사용을 위해)     
    -> 기본으로 Pod 기동시 root가 아닌 Linux의 최초 user인 1000으로 수행 (runAsUser: 1000)     
      예시)      
      tanzu apps workload create WORKLOAD-NAME \     
      --app APP-NAME \      
      --type TYPE \      
      --image IMAGE      
- Dockerfile-based builds      
  : pre-built된 Container image 가 아닌 Git Repo내 정의된 Dockerfile 을 통해 새롭게 Container Image를 Build 하며     
    이때 Kaniko(Docker Daemon 없이 build 수행) 라는 오픈소스를 사용하게 됩니다.       
      예시)       
      tanzu apps workload create foo \      
      --git-repo https://github.com/foo/bar \      
      --git-branch dev \       
      --param dockerfile=./Dockerfile     # Git Repo내 Dockerfile 활용      

