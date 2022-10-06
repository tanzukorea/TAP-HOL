## Jumpbox 준비 및 TKG 설치하기

### 사전 준비사항
- DNS Server<br>
기존에 운영 중인 DNS서버에 harbor.tanzukr.com를 등록 합니다.
DNS서버가 없는 경우 다음의 방법으로 bind9으로 설치하셔도 됩니다.<br>
[bind9 DNS 서버 설치](./dns-install.md)

- Avi Load Balancer
- [Jumpbox 다운로드](https://onevmw.sharepoint.com/:f:/r/teams/TAPHOLWorkshop/Shared%20Documents/General/jumpbox?csf=1&web=1&e=1Zi1TL)
  위 경로에 있는 4개의 파일을 다운로드 받습니다. jumpbox-1.vmdk 는 210GB로 로컬 저장소 공간의 확보가 필요합니다.

### Jumpbox 준비
1. 다운로드 받은 jumpbox 파일을 vCenter에서 'OVF 템플릿 배포'를 해서 업로드 합니다.
1. 업로드한 VM을 시작합니다.
1. jumpbox VM에 GUI 콘솔에 접속
1. jumpbox VM의 IP 변경
 ![](./jumpbox/jumpbox_v1.png)
네트워크 설정으로 들어가서 IP와 DNS를 현재의 환경에 맞게 변경합니다.<br>
DNS서버는 비워두고 Automatic으로 합니다. <br>네트워크를 비활성화 했다가 다시 활성화 합니다.<br>
CLI로 변경할 경우에는 아래의 경로 하위에 있는 파일을 변경합니다.<br>
```
cd /etc/NetworkManager/system-connections/
vi *
sudo service network-manager restart
```

5. jumpbox에 접속<br>
jumpbox에 있는 계정은 root와 tap 계정이고 비밀번호는 모두 VMware1! 입니다.<br>
``` ssh root@10.220.58.99 ```

6. DHCP서버 수정<br>
현재의 환경에 맞게 dhcp range를 수정합니다.
```
subnet 172.20.22.0 netmask 255.255.255.0 {
  range 172.20.22.20 172.20.22.100;
  option domain-name-servers 172.10.0.8;
  option subnet-mask 255.255.255.0;
  option routers 172.20.22.1;
  option broadcast-address 172.20.22.255;
  default-lease-time 600;
  max-lease-time 7200;
}
```
만약 domain-name-servers 를 같은 jumpbox에 설치한 경우에는 127.0.0.1을 입력합니다.

7. DHCP 서버 재기동<br>
```
systemctl restart isc-dhcp-server.service
systemctl status isc-dhcp-server.service
```
dhcp server가 active인 것을 확인합니다.

8. Harbor 기동<br>
```
cd /tanzu/harbor
docker-compose up -d
```

### TKG 설치하기
1. TKG 설치 준비<br>
아래의 과정은 tap 계정으로 switch해서 수행합니다.
```
su - tap
```

DNS 서버가 없는 경우에는 아래의 설정을 반드시 추가해 주어야 합니다.
DNS 서버가 있다고 가정하므로 여기서는 skip 합니다.
```
cd dns
vi vsphere-overlay.yaml
172.20.22.10   harbor.tanzukr.com 부분을 새로 할당한 jumpbox ip를 입력합니다.
cp vsphere-overlay.yaml /home/tap/.config/tanzu/tkg/providers/infrastructure-vsphere/ytt
vi tkr_overlay.lib.yaml
172.20.22.10   harbor.tanzukr.com 부분을 새로 할당한 jumpbox ip를 입력합니다.
cp tkr_overlay.lib.yaml ~/.config/tanzu/tkg/providers/ytt/03_customizations/01_tkr/tkr_overlay.lib.yaml
```

2. TKG BaseOS 이미지 다운로드<br>
[TKG Download Page](https://customerconnect.vmware.com/en/downloads/details?downloadGroup=TKG-154&productId=988&rPId=93384)<br>
TKG 1.5.4를 선택하고 Photon v3 Kubernetes v1.22.9 OVA 를 다운로드 합니다.

3. TKG BaseOS 이미지 vCenter에 업로드<br>
Kubernetes v1.22.9+vmware.1-tkg.1 ovf 템플릿 파일을 다운로드 하여 vSphere에 업로드를 해 둡니다.
photon-3-kube-v1.22.9+vmware.1-tkg.1-06852a87cc9526f5368519a709525c68.ova

4. TKG Mgmt Cluster 생성<br>
자세한 Guide는 아래의 링크를 참조합니다. 여기서는 대략적인 Guide로 진행합니다.
[TKG 설치 상세 Guide](https://github.com/tanzukorea/TKGm-HOL/blob/main/tkgm/1.TKGm-setting/4.md)

tap 계정으로 다음 명령어를 실행합니다.
```
tanzu mc create --ui -b 0.0.0.0:9090 
```

5. TKG 설치 UI 진행<br>
브라우저에서 http://ip:9090 으로 접근하여 TKG 설치 UI를 통해 TKG를 설치합니다.
설치는 vSphere with Tanzu(TKGs) 가 아니고 TKGm으로 설치를 진행하셔야 합니다.
Deploy TKG Managment Cluster를 선택해서 설치를 진행합니다.

6. SSH public key 다운로드<br>
설치시에 입력하는 SSH public key는 편의를 위해 jumpbox 의 tap 계정에 있는 ssh key(id_rsa.pub) 파일을 로컬 PC에 다운받아서 사용합니다.
Dev모드, 인스턴스 Type은 medium으로 설치합니다.
control plane endpoint주소는 dhcp주소와 겹치지 않도록 네트워크 범위에 있는 ip를 입력합니다.

7. LB가 필요하기 때문에 AVI와 설치시에 연동을 해도 됩니다.<br>
이 Lab에서는 TKG 클러스터를 모두 설치한 후에 연동하는 방식으로 진행합니다.
Management Cluster가 정상적으로 설치되면 다음의 명령어로 kubeconfig를 얻습니다.
```
tanzu mc  kubeconfig get --admin
kubectl config use-context mgmt1-admin@mgmt1
```

8. TKG Workload Cluster 생성<br>
TAP는 TKG Workload Cluster 위에 추가 패키지의 형태로 설치가 됩니다.
이제 TAP를 설치할 TKG Cluster를 하나 생성해보겠습니다.
```
cd ~/.config/tanzu/tkg/clusterconfigs 
```
Management Cluster 생성시에 만들어진 yaml파일을 복사해서 새로운 파일을 만듭니다.

```
tap@harbor:~/.config/tanzu/tkg/clusterconfigs$ cp vdwmd173jo.yaml cluster1.yaml
tap@harbor:~/.config/tanzu/tkg/clusterconfigs$ vi cluster1.yaml
CLUSTER_NAME와 VSPHERE_CONTROL_PLANE_ENDPOINT 를 적절히 변경합니다.

tap@harbor:~/.config/tanzu/tkg/clusterconfigs$ tanzu cluster create --file cluster1.yaml -v 9
```

9. 설치 확인<br>
```
tanzu cluster kubeconfig get 클러스터명 --admin
kubectl config use-context 클러스트명-admin@클러스터명
```

```
kubectl get pods -A

NAMESPACE      NAME                                                     READY   STATUS    RESTARTS      AGE
kube-system    antrea-agent-447jk                                       2/2     Running   0             35m
kube-system    antrea-agent-kjdq5                                       2/2     Running   0             35m
kube-system    antrea-controller-868b865c59-npbqc                       1/1     Running   0             35m
kube-system    coredns-d457ffd95-9qgv5                                  1/1     Running   0             36m
kube-system    coredns-d457ffd95-br9q2                                  1/1     Running   0             36m
kube-system    etcd-cluster1-control-plane-cf8q9                        1/1     Running   0             37m
kube-system    kube-apiserver-cluster1-control-plane-cf8q9              1/1     Running   0             37m
kube-system    kube-controller-manager-cluster1-control-plane-cf8q9     1/1     Running   0             37m
kube-system    kube-proxy-74fcp                                         1/1     Running   0             36m
kube-system    kube-proxy-jj7k8                                         1/1     Running   0             37m
kube-system    kube-scheduler-cluster1-control-plane-cf8q9              1/1     Running   0             37m
kube-system    kube-vip-cluster1-control-plane-cf8q9                    1/1     Running   0             37m
kube-system    metrics-server-5fd77695b5-mdtfc                          1/1     Running   0             35m
kube-system    vsphere-cloud-controller-manager-blnqn                   1/1     Running   0             34m
kube-system    vsphere-csi-controller-6449695bd4-kgt7l                  6/6     Running   0             35m
kube-system    vsphere-csi-node-kg7rd                                   3/3     Running   5 (33m ago)   35m
kube-system    vsphere-csi-node-xj8ds                                   3/3     Running   5 (33m ago)   35m
tanzu-system   secretgen-controller-5c8fc4b9f7-x8z8q                    1/1     Running   0             35m
tkg-system     kapp-controller-7886988d97-6dnpc                         1/1     Running   0             36m
tkg-system     tanzu-capabilities-controller-manager-7b5d47b5f4-d2z82   1/1     Running   0             36m
```


10. Avi 연동<br>
tap 계정의 home에 avi 폴더가 있습니다.
~/avi/ako/로 이동한 후 다음의 두 파일을 수정합니다.<br>
<b>install-tanzu-int.sh</b> : AVI의 controller IP와 username, password 를 입력합니다.<br>
<b>values-tap.yaml</b> 에서는 
NetworkSettings 부분과 ControllerSettings 부분을 avi의 설정에 맞게 변경합니다.

```./install-tanzu-int.sh``` 를 실행해서 현재 Cluster에 ako를 설치합니다.

kubectl get pods -A 를 해서 ako-0 pod가 새롭게 생성된 것을 확인할 수 있습니다.
```
tap@harbor:~/avi/ako$ kubectl get pods -A
NAMESPACE      NAME                                                     READY   STATUS    RESTARTS      AGE
avi-system     ako-0                                                    1/1     Running   0             8s
...
```

설치가 모두 정상적으로 끝났습니다. 준비가 완료되었습니다.
