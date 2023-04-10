## Local에 있는 파일 워크로드 배포

**이번 실습은 개발자의 Workshop Session 환경에 저장된 Local Source를 Workload로 배포하는 실습입니다.**     
      
*Note) 앞서 수행한 방식과는 다르게 배포에 필요한 Source는 아직 git 에 올라가 있지 않으며            
      개발자 Local 환경에 있는 Source를 통해 배포됩니다.*  

1. TAP package accelerator update
이번 Lab에서는 accelerator 에 직접 접근해서 다운로드 받는 방식을 사용합니다.
accelerator는 tap-gui를 통해 다운로드를 받을 수 있기 때문에 별도의 세팅을 하지 않아도 되지만, CLI를 통해 다운로드를 받을 경우에는 accelerator server를 설정해 주어야 합니다.
tap-values.yaml파일에 아래의 내용을 추가한 후 update 합니다.

```
accelerator:
  samples:
    include: false
  domain: tap.tanzukorea.net
  ingress:
    include: true
    enable_tls: true
  tls:
    secret_name: tap-gui-secret
    namespace: tap-gui
  server:
    service_type: "ClusterIP"
    watched_namespace: "accelerator-system"
```
- samples: include: false는 기본 샘플을 포함하지 않는 방식입니다. 기본 샘플의 주소는 https://github.com/sample-accelerators 입니다. 이 주소에 있는 repository들이 자동으로 추가가 되게 되는데, air-gap환경에서는 이 주소를 사용할 수 없고 이 샘플들을 사용하기 위해서는 각 파일들을 직접 다운로드해서 내부에 있는 git server에 upload 를 한 후에 accelerator에 개별로 등록을 해주어야 합니다. 
- tls secret는 처음 설치시에(tap-gui) 사용한 도메인의 root ca의 secret를 그대로 사용하도록 합니다.
- ClusterIP를 입력하여 contour를 사용하도록 합니다.

1. package를 update합니다.
```
tanzu package installed update tap --values-file tap-values.yaml -n tap-install
```

1. package가 정상적으로 업데이트 되었는지 확인합니다.
package update 상태와 httpproxy tls가 valid한지 확인합니다.
```
tanzu package installed list -A
kubectl get httpproxy -A
```


1. Local개발을 위해 acccelerator 에 등록된 App 확인
```
tanzu accelerator list
```

1.  다운드로할 nodex-express source template을 찾습니다.     
```
tanzu accelerator list | grep node-express
```

1.  App(node-express) 다운로드
```
cd $HOME/workload/
tanzu accelerator generate node-express --options '{"projectName": "hello-nodejs", "artifactId":"hello-nodejs", "gitUrl":"https://github.com/[본인계정]/hello-nodejs.git","gitBranch":"main","apiSpecLocation":"remote"}' --server-url https://accelerator.tap.tanzukorea.net
```

1.  App(hello-nodejs) 다운로드 확인(hello-nodejs.zip)
```
ls -lrt $HOME/workload/hello-nodejs.zip
```

1.  다운받은 hello-nodejs-template.zip 에 대한 압축해제
```
unzip -o $HOME/workload/hello-nodejs.zip
ls -lrt $HOME/workload/hello-nodejs
``` 

1.  Source Directory 접근 및 확인   
```            
URL 접근시 Hello from the server! 를 출력하는 샘플입니다.  
```    
```
cat $HOME/workload/hello-nodejs/server.js
``` 

1.  배포할 workload 확인
```
Note) git-repo/git-branch 대신 local-path/source-image를 사용하여 배포합니다.
```  

1.  workload 배포
환경변수를 세팅합니다.
```
APP_NAME=hello-nodejs
LOCAL_PATH=$HOME/workload/hello-nodejs/
NAMESPACE=default
```
- namespace는 사전 준비사항으로 workload를 1개 배포한 곳과 동일한 것을 사용합니다.

소스는 압축이 되어 image registry(harbor)에 올라가게 됩니다. Image registry에 login을 하여 credential을 local에 저장합니다.

```
docker login $CONTAINER_REGISTRY_HOSTNAME -u $CONTAINER_REGISTRY_USERNAME --password-stdin
```
- Local PC에 docker를 설치할 수 없는 경우에는(예:windows) 아래의 링크를 참고하여 설치합니다.
[dockerDesktop없이 credential만 설치](tap/create_docker_credential.md)

```
tanzu apps workload apply ${APP_NAME} \
  --app hello-nodejs \
  --local-path ${LOCAL_PATH} \
  --source-image ${CONTAINER_REGISTRY_HOSTNAME}/${SOURCE_IMAGE_REPO}/${APP_NAME}-source \
  --type web \
  --annotation autoscaling.knative.dev/minScale=1 \
  --request-memory 256Mi \
  --namespace ${NAMESPACE}
  ```

1.  workload 배포 로그 확인             
```
tanzu apps workload tail hello-nodejs
```

1. 새로운 터미널을 열어서 workload 배포 모니터링(이미 watch가 수행되고 있으면 아래 명령을 실행시키지 않아도 됩니다.)
```
watch -n 2 kubectl get po,gitrepository,taskrun,sourcescan,build,imagescan,deliverable,ksvc -n ${NAMESPACE}
```

1.  workload 배포 확인(URL 접속)
```  
Pod가 배포되면 아래 명령어를 수행하여 https URL을 클릭합니다.    
```   
```
tanzu apps workload get hello-nodejs
```      

1. Local PC의 host파일 등록
수행하여 나오는 https URL을 클릭합니다.    
이때 Local PC의 host 파일에 위 URL등록 (contour를 사용하므로 tap-gui와 같은 IP등록)

* 운영환경에서는 app의 주소는 자동으로 생성이 되게 되는데, 이때 DNS 서버에서 모든 application의 주소에 대하여 IP(contour ip)를 설정할 수 있는 경우에는 등록을 해줍니다. (*.default.tap.도메인명)

https://hello-nodejs.default.tap.tanzukorea.net


1.  배포한 workload 삭제   
```
tanzu apps workload delete hello-nodejs
``` 
