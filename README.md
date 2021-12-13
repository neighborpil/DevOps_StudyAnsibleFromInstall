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
3. DNS 서버 설정
```
# vi /etc/resolv.conf

-- DNS 서버 입력
nameserver 8.8.8.8
```
```
# yum repolist
```
5. 추가 패키지를 위한 공간 설정
```
# yum -y install epel-release
```
6. ansible 설치
```
# yum -y install ansible
```

### 실행
```
# ansible all -m ping -k
-- 에러뜸

# vi /etc/ansible/hosts
-- 제일 아래 부분에 호스트 설정 192.168.100.181

-- public key 교환을 위한 yes/no가 뜨면 yes 해주면 됨
# ansible all -m ping

-- 3번 반복 후
-- 키교환 생랴하는 옵션 추가해서 핑 해봄
# ansible all -m ping -k


```
