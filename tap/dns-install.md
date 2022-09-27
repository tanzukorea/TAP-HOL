# DNS 설치 - bind9

* 기존 DNS서버가 있을 경우에 bind9을 별도로 설치하는 것보다 기존 DNS 이용하는 것을 권고합니다. 기존 DNS를 사용할 수 없는 경우에 아래와 같이 설치를 해서 사용할 수 있고, 이 경우 기존 DNS서버를 secondary dns 서버로 설정하거나 vcenter 도메인 주소와 avi의 도메인 주소를 bind9에 입력해서 사용하시면 됩니다.

1. bind9 설치
같은 jumpbox 서버에 bind9을 설치하도록 하겠습니다. 
jumpbox가 인터넷이 연결되어 있는 상태에서 수행하셔야 합니다.

``` sudo apt install bind9 ```

2. tanzukr.com zone 설정<br>
vi /etc/bind/named.conf.local 
아래 내용 추가
zone "tanzukr.com" {
    type master;
    file "/etc/bind/db.tanzukr.com";
}
3. tanzukr.com 설정<br>
```
cp /etc/bind/db.local /etc/bind/db.tanzukr.com

$TTL    604800
@       IN      SOA     tanzukr.com. root.tanzukr.com. (
                              2         ; Serial
                         604800         ; Refresh
                          86400         ; Retry
                        2419200         ; Expire
                         604800 )       ; Negative Cache TTL
;
@       IN      NS      ns.tanzukr.com.
@       IN      A       10.220.58.99
@       IN      AAAA    ::1
ns      IN      A       10.220.58.99
harbor      IN      A       10.220.58.99
```

```
vi /etc/default/named
OPTIONS="-u bind -4"
```

```
sudo systemctl restart bind9.service
sudo systemctl status bind9.service

```
서비스가 정상적으로 active인지 확인합니다.

4. local dns 변경
현재 jumpbox의 name server를 local로 변경합니다.
```
vi /etc/systemd/resolved.conf
[Resolve]
DNS=127.0.0.1

```
systemctl restart systemd-resolved

systemd-resolve --status
Global
       LLMNR setting: no
MulticastDNS setting: no
  DNSOverTLS setting: no
      DNSSEC setting: no
    DNSSEC supported: no
         DNS Servers: 127.0.0.1
```

5. dns 확인
nslookup tanzukr.com
nslookup vcenter 주소 
nslookup avi 도메인명
위와 같이 모두 접근이 가능해야 합니다. 만약 vCenter에 접근이 안되거나 avi가 접근이 안될 경우 기존 DNS서버에 매핑이 된 것처럼 새로운 zone을 만들어서 설정해야 합니다.

