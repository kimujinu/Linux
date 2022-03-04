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

# 3. 작업 스케줄링
-> at [option] time-spec : 단일성 작업 예약\
-> crontab -e : 주기적인 작업 예약

# 4. 디스크 관리
-> fdisk file-name : 파티션 구성 디스크 명령어

# 5. 파일시스템 및 스왑 메모리
-> 파일시스템 : 구조화된 일련의 정보를 구성하는 파일과 디렉토리의 집합, 파일 및 디렉토리를 저장하는 방식\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;-> ex) mkfs -t [filesystem-type] partition : 파일시스템 생성하는 명령\
\
-> 마운트(mount) : 파일시스템을 생성한 후에는 파일시스템이 생성된 파티션 장치에 접근할 수 있도록 경로를 생성해야 한다.\
&nbsp;&nbsp;&nbsp;&nbsp;파일시스템이 생성된 파티션 장치에서 데이터를 읽거나 쓸 때 일일이 장치 파일을 통해서 접근하는 방식은 매우 불편하다.\
&nbsp;&nbsp;&nbsp;&nbsp;그렇기 때문에 파일시스템이 생성된 파티션에 디렉토리 형태로 접근할 수 있도록 연결하는 작업을 수행하는데 이를 마운트라고 한다.\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;-> ex) mount [option] {partition | UUID} mount-point\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;-> ex) umount [option] {partition | nount-point | UUID}\
-> blkid : 파일시스템 정보 확인

## 리눅스 파일시스템
### 디스크 기반 파일시스템(Disk-Based File System)
-> EXT : 리눅스 초기 개발 시 리눅스에서 사용하기 위하여 만들어진 확장 파일시스템이다.\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;현재 ext파일시스템은 ext2,ext3,ext4 세가지 버전이 존재한다.\
-> XFS : RHEL7, CentOS7, OL7 등 최신 리눅스 버전에서 기본 파일시스템으로 사용되고 있는 파일시스템\
-> FAT : USB등 이동식 저장장치에 주로 사용되고 있는 파일 시스템
### 분산 파일시스템(Distributed File System)
-> NFS(Network File System) : 대부분 유닉스/리눅스에서 사용할 수 있는 분산파일시스템 방식.\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;공유된 자원을 로컬 시스템의 자원처럼 사용할 수 있다.\
-> SMB(Server Message Block) : 파일 및 장치 공유 프로토콜인 'SMB'기반의 분산파일시스템이다.\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;윈도우 운영체제와 유닉스/리눅스 운영체제간 디렉토리 및 파일 공유기능을 제공한다.\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;리눅스에서는 삼바(Samba)서비스를 통해 SMB 공유를 제공할 수 있다.
### Pseudo 파일 시스템
-> Pseudo 파일 시스템은 메모리 기반의 파일시스템으로 시스템 성능을 높이고 커널정보에 접근할 수 있도록 지원한다.\
-> swapfs : 스왑 파일 시스템(Swap file System)은 물리 메모리를 보조하기 위한 디스크 내의 스왑영역에서 사용하는 파일 시스템이다.\
-> tmpfs : 임시 파일시스템은 디스크 기반의 쓰기 오버헤드를 줄이기 위해 메모리에 파일을 기록하는 시스템\
-> fdfs : 파일 설명자 파일시스템은 /dev/fd 디렉토리의 파일 설명자를 사용할 수 있는 명시적인 이름을 제공하고 있다.\
-> procfs : 현재 동작중인 프로세스의 목록을 관리하는 파일시스템\
-> devfs : 시스템에서 사용하는 모든 디바이스의 이름 공간을 관리하기 위해 사용한다.
### 주요 파일시스템 구조
-> 데이터 저장 용도로 주로 사용하는 파일시스템은 EXT 파일시스템 계열의 ext4 파일시스템과 SGI사의 xfs 파일시스템을 주로 사용한다.\
-> ext4 파일시스템 기본구조\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;-> 파일시스템 전체에 대한 주요 정보는 슈퍼블록에 저장된다.\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;-> ext4 파일시스템 내에 여러 개의 블록 그룹이 존재한다.\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;-> 슈퍼 블록의 백업이 일부 블록 그룹에 저장된다.\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;-> inode를 사용하여 파일의 메타정보와 데이터를 분리하여 저장한다.\
-> xfs 파일시스템 기본구조\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;-> inode를 사용한다. 내부 구조는 ext4 파일시스템과 같지 않다.\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;-> ext4의 블록그룹 대신 할당 그룹 용어를 사용한다.\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;-> 기본적으로 볼륨을 8개의 할당그룹으로 분할한다.\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;-> 8개 이상으로 분할할 수 있다.\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;-> 파일 탐색을 위해 B+트리를 사용한다.
### 스왑 메모리
-> 메모리 확보를 위한 기법\
-> mkswap file-name : 스왑 파일 생성\
-> swapon [option] {partition | file-name} : 스왑 영역 활성화\
-> swapoff [option] {partition | file-name | UUID } : 스왑 영역 활성화 해제





          
