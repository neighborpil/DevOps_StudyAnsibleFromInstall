# DevOps_StudyAnsibleFromInstall
Example codes


### ※호스트네임 변경
```
# hostnamectl set-hostname my-hostname
```

## 환경설정
 - Virtualbox에 구성
 - Server 1
 - Node 3개 설정
 - 브릿지 네트워크 구성
 - IP: server: 192.168.100.180 255.255.255.0 191.168.100.1
       node01: 192.168.100.181
       node02: 192.168.100.182
       node03: 192.168.100.183

 1. hostname 변경
```
# hostnamectl set-hostname Ansible-Server
# hostnamectl set-hostname Ansible-Node01
```

2. ip 변경
```
# nmtui
```
 - gui 환경으로 변경되면 ip세팅함
 - 나와서 재시작 및 핑테스트
```
# systemctl restart network
# ping 192.168.100.1
```
4. 
