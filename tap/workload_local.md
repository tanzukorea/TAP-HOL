**이번 실습은 개발자의 Workshop Session 환경에 저장된 Local Source를 Workload로 배포하는 실습입니다.**     
      
*Note) 앞서 수행한 방식과는 다르게 배포에 필요한 Source는 아직 git 에 올라가 있지 않으며            
      개발자 Local 환경에 있는 Source를 통해 배포됩니다.*           
    
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

1. Teminal1) Local개발을 위해 acccelerator 에 등록된 App 확인
```copy
clear
tanzu accelerator list --server-url http://accelerator.$EXPLORE_ID.dha-tkgdemo.net
```

1. Teminal1) 다운드로할 hello-nodejs source template을 찾습니다.     
```copy
clear
tanzu accelerator list --server-url http://accelerator.$EXPLORE_ID.dha-tkgdemo.net | grep hello-nodejs
```

1. Teminal1) App(hello-nodejs) 다운로드
```copy
clear
cd $HOME/workload/
tanzu accelerator generate hello-nodejs --options '{"artifactId":"hello-nodejs", "gitUrl":"https://gitlab.explore.dha-tkgdemo.net/workshop/hello-nodejs-accelerator.git","gitBranch":"main","apiSpecLocation":"remote"}' --server-url https://accelerator.$EXPLORE_ID.dha-tkgdemo.net
```

1. Teminal1) App(hello-nodejs) 다운로드 확인(hello-nodejs.zip)
```copy
clear
ls -lrt $HOME/workload/hello-nodejs.zip
```

1. Teminal1) 다운받은 hello-nodejs-template.zip 에 대한 압축해제
```copy
clear
unzip -o $HOME/workload/hello-nodejs.zip
ls -lrt $HOME/workload/hello-nodejs
``` 

1. Teminal1) Source Directory 접근 및 확인   
```            
URL 접근시 Hello NodeJs! 를 출력하는 샘플입니다.  
```    
```copy
clear
cat $HOME/workload/hello-nodejs/server.js
``` 

1. Teminal1) 배포할 workload 확인
```
Note) git-repo/git-branch 대신 local-path/source-image를 사용하여 배포합니다.
```  
```copy
clear
cat $HOME/workload/basic/hello-nodejs-workload-basic-local-settings-xml.sh
```

1. Teminal1) workload 배포
```copy
clear
$HOME/workload/basic/hello-nodejs-workload-basic-local-settings-xml.sh
```

1. Teminal1) workload 배포 로그 확인             
```copy
clear
tanzu apps workload tail $(echo $SESSION_NAME | awk -F'-' '{print $3}')-node-lc -n ${NAMESPACE}
```

1. Teminal2) workload 배포 모니터링(이미 watch가 수행되고 있으면 아래 명령을 실행시키지 않아도 됩니다.)
```copy
clear
watch -n 2 kubectl get po,gitrepository,taskrun,sourcescan,build,imagescan,deliverable,ksvc -n ${NAMESPACE}
```

1. Teminal1) workload 배포 확인(URL 접속)
```  
Terminal 2 번 화면에서 Pod(s0xx-node-lc-00001)가 배포되면 terminal1에서 아래 명령어를 수행하여 https URL을 클릭합니다.    
```   
```copy
clear
tanzu apps workload get $(echo $SESSION_NAME | awk -F'-' '{print $3}')-node-lc -n ${NAMESPACE}
```      

1. Teminal1) 개발자 Portal URL 확인 및 클릭
```copy
clear
echo https://tap-gui.$EXPLORE_ID.dha-tkgdemo.net
```
![](../images/03-tap-gui-supplychain-local.png)

1. Teminal1) 배포한 workload 삭제   
```copy
clear
tanzu apps workload delete $(echo $SESSION_NAME | awk -F'-' '{print $3}')-node-lc -n $NAMESPACE --yes
``` 

1. 실습정리              
