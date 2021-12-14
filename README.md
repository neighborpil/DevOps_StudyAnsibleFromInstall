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

### 설치 체크
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
### 앤서블 파일 확인

 - 환경설정 파일 : /etc/ansible/ansible.cfg
 - 호스트 파일 : /etc/ansible/hosts

### 앤서블 옵션값
 - -i(--inventory-file): 적용될 호스트들에 대한 파일
 - -m(--module-name): 모듈을 선택할 수 있도록
 - -k(--ask-pass): 패스워드를 물어보도록 설정
 - -K(--asnk-become-pass): 관리자로 권한 상승
 - --list-hosts: 적용되는 호스트들을 확인
 - -a : shell 명령어 입력
```
# ansible all -m ping -k

# vi test
--
192.168.100.181
192.168.100.182
--

# ansible all -i test -m ping -k

# ansible nginx -m ping -k

# ansible nginx -m ping -k -K # 관리자 권한 획득

# ansible nginx -m ping --list-hosts # 적용되게될 호스트의 리스트를 보여줌
# ansible all -i test -m ping --list-hosts

# ansible all -m shell -a "uptime" -k # 실시간 확인
# ansible all -m shell -a "df -h" -k # 디스크 용량 확인
# ansible all -m shell -a "free -h" -k # 메모리 상태 확인
# ansible all -m user -a "name=feelong password=1234" -k # 유저생성
# ansible nginx -m copy -a "src=./feelong.file dest=/tmp/" -k # 파일복사
# ansible nginx -m yum -a "name=httpd status=present" -k # 패키지 설치
# ansible nginx -m copy -a "src=/etc/resolv.conf dest=/etc/resolv.conf" -k # DNS파일 복사
# ansible nginx -m yum -a "name=httpd state=present" -k # httpd 설치

# yum list installed | grep httpd # 설치된 것들중 httpd찾음
# systemctl status httpd # 프로세스의 상태를 확인

```

### Playbook
 - 멱등성: 여러번 실행하더라도 결과는 하나만
 - feelong.yml
   + blockinfile: 특정 블럭을 특정 파일에 기록하겠다는 모듈
```
---
- name: Ansible_vim
  hosts: localhost

  tasks:
    - name: Add ansible hosts
      blockinfile:
        path: /etc/ansible/hosts
        block: |
          [feelong]
          192.168.100.182
```
```
# ansible-playbook feelong.yml
```


### Installing nginx to 3 nodes
 - nginx.yml
```
---
-
  hosts: nginx
  remote_user: root
  tasks:
    - name: install epel-release
      yum: name=epel-release state=latest
    - name: install nginx web server
      yum: name=tomcat state=present
    - name: Upload default index.html for web server
      copy: src=index.html dest=/usr/share/nginx/html/ mode=0664
    - name: Start nginx web server
      service: name=tomcat state=started
```

```
# ansible-playbook nginx.yml -k

```
 - firewall 끄기
```
# ansible nginx -m shell -a "systemctl stop firewalld" -k
```
