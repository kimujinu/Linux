# 1. 사용자 및 그룹 관리
##  관련 폴더
&nbsp;&nbsp;&nbsp;&nbsp;-> /etc/passwd : 시스템에 등록된 사용자 정보\
&nbsp;&nbsp;&nbsp;&nbsp;-> /etc/shadow : 시스템에 등록된 사용자의 패스워드와 패스워드에 대한 설정\
&nbsp;&nbsp;&nbsp;&nbsp;-> /etc/group : 시스템에 등록된 그룹정보\
&nbsp;&nbsp;&nbsp;&nbsp;-> /etc/gshadow : 시스템에 등록된 그룹의 패스워드와 패스워드에 대한 설정\
&nbsp;&nbsp;&nbsp;&nbsp;-> /etc/skel : 뼈대라는 의미로 사용자에 대한 기본적인 초기화 파일들을 저장하고 있는 디렉토리\
&nbsp;&nbsp;&nbsp;&nbsp;-> /etc/login.defs : 사용자나 그룹을 생성할 때 참고하는 기본 값들이 저장되어 있음\
&nbsp;&nbsp;&nbsp;&nbsp;-> /etc/sudoers : sudo 명령을 사용하기 위한 조건이 있는 파일\
&nbsp;&nbsp;&nbsp;&nbsp;-> /var/log/secure : 인증과 관련된 로그를 담는 파일

## 사용자 생성관리 명령어
&nbsp;&nbsp;&nbsp;&nbsp;-> useradd [option] user-name : 사용자 생성\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;-> useradd -D : 기본 설정 확인\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;-> ex) useradd -u 2000 -g 10 -m -d /home/guest/user03 -s /bin/sh user03\
&nbsp;&nbsp;&nbsp;&nbsp;-> usermod [option] user-name : 사용자 정보 수정\
&nbsp;&nbsp;&nbsp;&nbsp;-> userdel [option] user-name : 사용자 삭제

## 그룹 생성관리 명령어
&nbsp;&nbsp;&nbsp;&nbsp;-> groupadd [option] group-name : 그룹 생성\
&nbsp;&nbsp;&nbsp;&nbsp;-> groupmod [option] group-name : 그룹 정보 수정\
&nbsp;&nbsp;&nbsp;&nbsp;-> groupdel group-name : 그룹 삭제

## 기타 권한 명령어
&nbsp;&nbsp;&nbsp;&nbsp;-> su [-] [user-name] : 사용자 전환\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;-> ex) sudo -i : 루트 사용자 전환\
&nbsp;&nbsp;&nbsp;&nbsp;-> sudo [option] [user-name] command : 로그아웃을 하지않고 특정 사용자 권한으로 수행\
&nbsp;&nbsp;&nbsp;&nbsp;-> chage [option] [argument] user-name :  /etc/shadow 파일에서 패스워드의 속성을 변경하는 명령어

# 2. 고급 권한 관리
-> 개념 : 대부분의 경우, 읽기, 쓰기, 실행과 같은 기본 권한을 설정하여 파일에 대한 접근을 제어할 수 있지만,\
 &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
 일부 특수한 목적으로 사용되는 파일은 기본 권한만으로 파일에 대한 접근을 제어하기 어려울 수도 있다.\
 &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
 이를 해결하기 위해 확장 권한을 사용한다.\
 \
 -> setuid : 파일을 실행하는 주체가 파일을 실행한 사용자가 아닌 파일을 소유하고 있는 사용자의 권한으로 프로세스를 실행하게 된다.\
 &nbsp;&nbsp;&nbsp;&nbsp;-> ex) chmod 4000\
 \
 -> setgid : 명령을 실행할 때 프로세스의 사용자 그룹이 파일의 사용자 그룹으로 실행된다.\
 &nbsp;&nbsp;&nbsp;&nbsp;-> ex) chmod 2000\
 \
 -> sticky bit : 파일을 소유한 사용자만 파일을 삭제할 수 있다.\
 &nbsp;&nbsp;&nbsp;&nbsp;-> ex) chmod 1000
 
 ## 접근 제어 리스트(Access Control List, ACL)
-> 리눅스의 파일 권한은 사용자, 사용자 그룹, 기타 사용자로 분류되고 각각 읽기, 쓰기, 실행 권한이 제공된다.\
&nbsp;&nbsp;&nbsp;&nbsp; 이 권한 체계로 파일을 관리하는데 큰 문제는 없다.\
&nbsp;&nbsp;&nbsp;&nbsp; 하지만, 권한을 세부적으로 설정할 수 없다는 단점이 존재한다.\
&nbsp;&nbsp;&nbsp;&nbsp; ACL을 사용하면 파일을 소유한 사용자와 사용자 그룹을 제외한 사용자와 그룹에게 별도로 권한을 부여 할 수 있다.\
&nbsp;&nbsp;&nbsp;&nbsp;-> getfacl file-name : 접근 제어 리스트 정보 확인\
&nbsp;&nbsp;&nbsp;&nbsp;-> setfacl [option] ENTRY:NAME:PERMS file-name : 접근 제어 리스트 설정\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;-> ex) setfacl -m u:user01:rwx /root/acl/dirA\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;-> ex) setfacl -x g:group01 /root/acl/dirA #권한제거\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;-> ex) setfacl -RM u:user01:rwx /root/acl/dirA #재귀적 사용

## 작업 스케줄링
-> at [option] time-spec : 단일성 작업 예약\
-> crontab -e : 주기적인 작업 예약

## 디스크 관리
-> fdisk file-name : 파티션 구성 디스크 명령어
          
