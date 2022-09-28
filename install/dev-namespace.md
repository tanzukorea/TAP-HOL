# 워크로드 배포를 위한 개발자용 네임스페이스 구성

본 가이드는 TAP에서 워크로드 배포를 위한 개발자용 네임스페이스를 구성하는 방법에 대해 설명합니다. 워크로드를 생성하려면 네임스페이스에 권한이 필요하며, 다음과 같이 두 가지 방법을 지원합니다.
- 단일 사용자 액세스 활성화
- 쿠버네티스 RBAC을 사용하여 추가 사용자 액세스 활성화

본 가이드에서는 단일 사용자 액세스 활성화 방법에 대해서만 설명하며, 쿠버네티스 RBAC을 사용한 추가 사용자 엑세스 활성화 방법에 대해서는 [네임스페이스 구성 방법에 대한 링크](https://docs.vmware.com/en/VMware-Tanzu-Application-Platform/1.2/tap/GUID-set-up-namespaces.html)를 참고하시기 바랍니다.

본 가이드에서는 개발자용 네임스페이스로 default 네임스페이스를 사용합니다.

## 1. 레지스트리 크리덴셜 추가
이미지 레지스트리 크리덴셜을 다음과 같이 추가합니다.
```
tanzu secret registry add registry-credentials --server $INSTALL_REGISTRY_HOSTNAME --username $INSTALL_REGISTRY_USERNAME --password $INSTALL_REGISTRY_PASSWORD --namespace default
```

만일 에러가 발생하면, 다음과 같이 kubectl 명령어를 이용하여 크리덴셜을 추가해도 됩니다.
```
kubectl create secret docker-registry registry-credentials --docker-server=$INSTALL_REGISTRY_HOSTNAME --docker-username=$INSTALL_REGISTRY_USERNAME --docker-password=$INSTALL_REGISTRY_PASSWORD --namespace default
```

명령어에 사용된 관련 변수 정보
|변수명|설명|
|------|---|
|REGISTRY-SERVER|사설 레지스트리 접속 FQDN|
|REGISTRY-USERNAME|사설 레지스트리 접속 ID|
|REGISTRY-PASSWORD|사설 레지스트리 접속 비밀번호|
|YOUR-NAMESPACE|워크로드를 배포할 네임스페이스 명|

## 2. 네임스페이스에 권한 부여
네임스페이스에 부여할 권한 관련 YAML 파일은 [여기](https://raw.githubusercontent.com/tanzukorea/tanzu-install/main/tap/set-up-ns.yaml)를 참고하여 생성합니다.
```
kubectl -n YOUR-NAMESPACE apply -f set-up-ns.yaml
```
> **_NOTE:_** 위의 YAML 파일에는 추가 시크릿도 포함되어 있습니다. 만약 추가할 시크릿이 필요하지 않을 경우 삭제하셔도 됩니다.
