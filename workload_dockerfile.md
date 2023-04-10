**이번 실습은 개발자가 기존 Dockerfile을 이용하여 workload를 배포하는 실습입니다.**    

1. kaniko Overview    
```
Kaniko는 Container 또는 Kubernetes 클러스터 내부의 Dockerfile에서 컨테이너 이미지를 빌드하는 도구입니다.
Kaniko는 Docker 데몬에 의존하지 않기 때문에, 일반적인 표준 Kubernetes 클러스터와 같이 Container Runtime이
Docker 가 아닌 환경에서 컨테이너 이미지를 쉽게 빌드할 수 있습니다.
https://github.com/GoogleContainerTools/kaniko
```

1. Teminal1) 환경변수 선언
```copy
clear
source $HOME/config/explore-env.sh
kubectl config get-contexts
echo $NAMESPACE
echo $SESSION
```

1. Teminal2) 환경변수 선언(이미 watch가 수행되고 있으면 아래 명령어 생략)      
```copy
clear
source $HOME/config/explore-env.sh
kubectl config get-contexts
```

1. Teminal1) 배포할 workload 확인
```copy
clear
cat $HOME/workload/basic/hello-nodejs-workload-basic-dockerfile-settings-xml.sh
```
```
dockerfile 을 통한 빌드시 default 인 kpack 대신 Kaniko 를 사용하여 빌드하게 되며, 
workload 배포은 다음과 같습니다.
###################################################################################
dockerfile              # build context의 Dockerfile 파일에 대한 상대 경로 ./dockerfile
docker_build_context    # build context 가 있는 디렉토리의 상대 경로  ./
docker_build_extra_args # Kaniko에 전달할 flag 목록(빌드시 argement) build-arg=FOO=BAR
###################################################################################
```

1. Teminal1) Kaniko 및 Kpack 관련 clusterimagetemplate 확인     
```copy
clear
kubectl get clusterimagetemplate
```

1. Teminal1) Dockerfile 확인
```dashboard:open-url
url: https://gitlab.explore.dha-tkgdemo.net/workshop/hello-nodejs-dockerfile/-/blob/main/Dockerfile
```

1. workload 배포
```copy
clear
$HOME/workload/basic/hello-nodejs-workload-basic-dockerfile-settings-xml.sh
```

1. Teminal1) workload 배포 로그 확인                 
```copy
clear
tanzu apps workload tail $(echo $SESSION_NAME | awk -F'-' '{print $3}')-node-df -n ${NAMESPACE}
```

1. Teminal2) workload 배포 모니터링(이미 watch가 수행되고 있으면 아래 명령어 생략)  
```copy
clear
watch -n 2 kubectl get po,gitrepository,taskrun,sourcescan,build,imagescan,deliverable,ksvc -n ${NAMESPACE}
```

1. Teminal1) workload 배포 확인(URL 접속)  
Terminal 2 번 화면에서 Pod가 배포되면(s0xx-node-df-00001-deployment) Terminal1번에서 아래 명령어를 수행하여 https URL을 클릭합니다.    
```copy
clear
tanzu apps workload get $(echo $SESSION_NAME | awk -F'-' '{print $3}')-node-df -n ${NAMESPACE}
```       

1. Teminal1) 개발자 Portal URL 확인 및 클릭
```copy
clear
echo https://tap-gui.$EXPLORE_ID.dha-tkgdemo.net
```
![](../images/02-tap-gui-supplychain-dockerfile.png)

1. Teminal1) 배포한 workload 삭제       
```copy
clear
tanzu apps workload delete $(echo $SESSION_NAME | awk -F'-' '{print $3}')-node-df -n $NAMESPACE --yes
``` 

1. 실습정리              