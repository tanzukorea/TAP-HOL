# Namespace Provisioner

TAP1.4 이전에는 워크로드를 배포하기 위해서는 Namespace에 TAP 빌드와 관련된 리소스와 권한이 부여되어야 했습니다.
1.4부터는 이 부분을 쉽게 자동으로 배포해주는 Namespace provisioner가 새롭게 추가되었습니다.
namespace를 만들고 단순히 아래의 설정을 추가해주면 됩니다.

```
kubectl label namespaces default apps.tanzu.vmware.com/tap-ns=""
```

## 1. 레지스트리 크리덴셜 추가
이미지 레지스트리 크리덴셜을 다음과 같이 추가합니다.
```
tanzu secret registry add registry-credentials --server $INSTALL_REGISTRY_HOSTNAME --username $INSTALL_REGISTRY_USERNAME --password $INSTALL_REGISTRY_PASSWORD --namespace default
```
