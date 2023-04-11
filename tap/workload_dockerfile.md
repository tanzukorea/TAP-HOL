## Dockerfile 기반 워크로드 배포

**이번 실습은 기존에 사용중인 Dockerfile을 이용하여 workload를 배포하는 실습입니다.**    
TAP는 TBS(Tanzu Build Service)를 사용하기 때문에 별도의 Dockerfile을 필요로 하지 않습니다. 하지만 기존에 사용중인 Dockerfile을 그대로 사용하고자 할 경우도 있습니다. (예: docker image에 커스텀한 패키지를 같이 설치해야 하는 경우) 이 경우를 위해 기존에 사용중인 Dockerfile을 사용할 수 있습니다.

1. kaniko Overview
Kubernetes 에서 docker를 build하기 위해서는 container 안에서 다시 docker engine을 띄워야 하는데, 이것을 지원하는 것이 kaniko입니다.
```
Kaniko는 Container 또는 Kubernetes 클러스터 내부의 Dockerfile에서 컨테이너 이미지를 빌드하는 도구입니다.
Kaniko는 Docker 데몬에 의존하지 않기 때문에, 일반적인 표준 Kubernetes 클러스터와 같이 Container Runtime이
Docker 가 아닌 환경에서 컨테이너 이미지를 쉽게 빌드할 수 있습니다.
https://github.com/GoogleContainerTools/kaniko
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

1.  Kaniko 및 Kpack 관련 clusterimagetemplate 확인     
```
kubectl get clusterimagetemplate
```

1. Dockerfile을 아래와 같이 생성합니다.
```
FROM node:8-alpine

# Create app directory
WORKDIR /usr/src/app

# Install app dependencies
# A wildcard is used to ensure both package.json AND package-lock.json are copied
# where available (npm@5+)
COPY package*.json ./

RUN npm install --only=production

# Bundle app source
COPY . .

EXPOSE 8080
CMD [ "npm", "start" ]

```

1. workload 배포
```
tanzu apps workload apply ${APP_NAME} \
  --app hello-nodejs \
  --git-repo  https://github.com/tanzukorea/hello-nodejs-dockerfile \
  --git-branch main \
  --type web \
  --annotation autoscaling.knative.dev/minScale=1 \
  --param dockerfile=./Dockerfile \
  --param docker_build_context=./ \
  --request-memory 256Mi \
  --namespace ${NAMESPACE} 
  ```

1.  workload 배포 로그 확인                 
```
tanzu apps workload tail hello-nodejs
```

1. workload 배포 모니터링(이미 watch가 수행되고 있으면 아래 명령어 생략)  
```
watch -n 2 kubectl get po,gitrepository,taskrun,sourcescan,build,imagescan,deliverable,ksvc -n ${NAMESPACE}
```

1.  workload 배포 확인(URL 접속)  
화면에서 Pod가 배포되면 아래 명령어를 수행합니다.
```
tanzu apps workload get hello-nodejs
```       

1. Local PC의 host파일 등록
수행하여 나오는 https URL을 클릭합니다.    
이때 Local PC의 host 파일에 위 URL등록 (contour를 사용하므로 tap-gui와 같은 IP등록)

* 운영환경에서는 app의 주소는 자동으로 생성이 되게 되는데, 이때 DNS 서버에서 모든 application의 주소에 대하여 IP(contour ip)를 설정할 수 있는 경우에는 등록을 해줍니다. (*.default.tap.도메인명)

https://hello-nodejs.default.tap.tanzukorea.net


1. 배포한 workload 삭제       
```
tanzu apps workload delete hello-nodejs
``` 
