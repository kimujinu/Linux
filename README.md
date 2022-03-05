# 1. 사용자 및 그룹 관리
##  관련 폴더
<pre>
-> /etc/passwd : 시스템에 등록된 사용자 정보
-> /etc/shadow : 시스템에 등록된 사용자의 패스워드와 패스워드에 대한 설정
-> /etc/group : 시스템에 등록된 그룹정보
-> /etc/gshadow : 시스템에 등록된 그룹의 패스워드와 패스워드에 대한 설정
-> /etc/skel : 뼈대라는 의미로 사용자에 대한 기본적인 초기화 파일들을 저장하고 있는 디렉토리
-> /etc/login.defs : 사용자나 그룹을 생성할 때 참고하는 기본 값들이 저장되어 있음
-> /etc/sudoers : sudo 명령을 사용하기 위한 조건이 있는 파일
-> /var/log/secure : 인증과 관련된 로그를 담는 파일
</pre>
## 사용자 생성관리 명령어
<pre>
-> useradd [option] user-name : 사용자 생성
    -> useradd -D : 기본 설정 확인
    -> ex) useradd -u 2000 -g 10 -m -d /home/guest/user03 -s /bin/sh user03
-> usermod [option] user-name : 사용자 정보 수정
-> userdel [option] user-name : 사용자 삭제
</pre>
## 그룹 생성관리 명령어
<pre>
-> groupadd [option] group-name : 그룹 생성
-> groupmod [option] group-name : 그룹 정보 수정
-> groupdel group-name : 그룹 삭제
</pre>
## 기타 권한 명령어
<pre>
-> su [-] [user-name] : 사용자 전환
    -> ex) sudo -i : 루트 사용자 전환
-> sudo [option] [user-name] command : 로그아웃을 하지않고 특정 사용자 권한으로 수행
-> chage [option] [argument] user-name :  /etc/shadow 파일에서 패스워드의 속성을 변경하는 명령어
</pre>
# 2. 고급 권한 관리
<pre>
-> 개념 : 대부분의 경우, 읽기, 쓰기, 실행과 같은 기본 권한을 설정하여 파일에 대한 접근을 제어할 수 있지만,
         일부 특수한 목적으로 사용되는 파일은 기본 권한만으로 파일에 대한 접근을 제어하기 어려울 수도 있다.
         이를 해결하기 위해 확장 권한을 사용한다.

 -> setuid : 파일을 실행하는 주체가 파일을 실행한 사용자가 아닌 파일을 소유하고 있는 사용자의 권한으로 프로세스를 실행하게 된다.
    -> ex) chmod 4000

 -> setgid : 명령을 실행할 때 프로세스의 사용자 그룹이 파일의 사용자 그룹으로 실행된다.
    -> ex) chmod 2000
 
 -> sticky bit : 파일을 소유한 사용자만 파일을 삭제할 수 있다.
     -> ex) chmod 1000
</pre>
 ## 접근 제어 리스트(Access Control List, ACL)
 <pre>
-> 리눅스의 파일 권한은 사용자, 사용자 그룹, 기타 사용자로 분류되고 각각 읽기, 쓰기, 실행 권한이 제공된다.
   이 권한 체계로 파일을 관리하는데 큰 문제는 없다.
   하지만, 권한을 세부적으로 설정할 수 없다는 단점이 존재한다.
   ACL을 사용하면 파일을 소유한 사용자와 사용자 그룹을 제외한 사용자와 그룹에게 별도로 권한을 부여 할 수 있다.
-> getfacl file-name : 접근 제어 리스트 정보 확인
-> setfacl [option] ENTRY:NAME:PERMS file-name : 접근 제어 리스트 설정
    -> ex) setfacl -m u:user01:rwx /root/acl/dirA
    -> ex) setfacl -x g:group01 /root/acl/dirA #권한제거
    -> ex) setfacl -RM u:user01:rwx /root/acl/dirA #재귀적 사용
</pre>
# 3. 작업 스케줄링
<pre>
-> at [option] time-spec : 단일성 작업 예약\
-> crontab -e : 주기적인 작업 예약
</pre>
# 4. 디스크 관리
<pre>
-> fdisk file-name : 파티션 구성 디스크 명령어
</pre>
# 5. 파일시스템 및 스왑 메모리
<pre>
-> 파일시스템 : 구조화된 일련의 정보를 구성하는 파일과 디렉토리의 집합, 파일 및 디렉토리를 저장하는 방식
    -> ex) mkfs -t [filesystem-type] partition : 파일시스템 생성하는 명령

-> 마운트(mount) : 파일시스템을 생성한 후에는 파일시스템이 생성된 파티션 장치에 접근할 수 있도록 경로를 생성해야 한다.
                  파일시스템이 생성된 파티션 장치에서 데이터를 읽거나 쓸 때 일일이 장치 파일을 통해서 접근하는 방식은 매우 불편하다.
                  그렇기 때문에 파일시스템이 생성된 파티션에 디렉토리 형태로 접근할 수 있도록 연결하는 작업을 수행하는데 이를 마운트라고 한다.
                  -> ex) mount [option] {partition | UUID} mount-point
                  -> ex) umount [option] {partition | nount-point | UUID}
                  
-> blkid : 파일시스템 정보 확인
</pre>
## 리눅스 파일시스템
### 디스크 기반 파일시스템(Disk-Based File System)
<pre>
-> EXT : 리눅스 초기 개발 시 리눅스에서 사용하기 위하여 만들어진 확장 파일시스템이다.
         현재 ext파일시스템은 ext2,ext3,ext4 세가지 버전이 존재한다.
-> XFS : RHEL7, CentOS7, OL7 등 최신 리눅스 버전에서 기본 파일시스템으로 사용되고 있는 파일시스템
-> FAT : USB등 이동식 저장장치에 주로 사용되고 있는 파일 시스템
</pre>
### 분산 파일시스템(Distributed File System)
<pre>
-> NFS(Network File System) : 대부분 유닉스/리눅스에서 사용할 수 있는 분산파일시스템 방식.
                              공유된 자원을 로컬 시스템의 자원처럼 사용할 수 있다.
-> SMB(Server Message Block) : 파일 및 장치 공유 프로토콜인 'SMB'기반의 분산파일시스템이다.
                               윈도우 운영체제와 유닉스/리눅스 운영체제간 디렉토리 및 파일 공유기능을 제공한다.
                               리눅스에서는 삼바(Samba)서비스를 통해 SMB 공유를 제공할 수 있다.
</pre>
### Pseudo 파일 시스템
<pre>
-> Pseudo 파일 시스템은 메모리 기반의 파일시스템으로 시스템 성능을 높이고 커널정보에 접근할 수 있도록 지원한다.
-> swapfs : 스왑 파일 시스템(Swap file System)은 물리 메모리를 보조하기 위한 디스크 내의 스왑영역에서 사용하는 파일 시스템이다.
-> tmpfs : 임시 파일시스템은 디스크 기반의 쓰기 오버헤드를 줄이기 위해 메모리에 파일을 기록하는 시스템
-> fdfs : 파일 설명자 파일시스템은 /dev/fd 디렉토리의 파일 설명자를 사용할 수 있는 명시적인 이름을 제공하고 있다.
-> procfs : 현재 동작중인 프로세스의 목록을 관리하는 파일시스템
-> devfs : 시스템에서 사용하는 모든 디바이스의 이름 공간을 관리하기 위해 사용한다.
</pre>
### 주요 파일시스템 구조
<pre>
-> 데이터 저장 용도로 주로 사용하는 파일시스템은 EXT 파일시스템 계열의 ext4 파일시스템과 SGI사의 xfs 파일시스템을 주로 사용한다.
-> ext4 파일시스템 기본구조
    -> 파일시스템 전체에 대한 주요 정보는 슈퍼블록에 저장된다.
    -> ext4 파일시스템 내에 여러 개의 블록 그룹이 존재한다.
    -> 슈퍼 블록의 백업이 일부 블록 그룹에 저장된다.
    -> inode를 사용하여 파일의 메타정보와 데이터를 분리하여 저장한다.
-> xfs 파일시스템 기본구조
    -> inode를 사용한다. 내부 구조는 ext4 파일시스템과 같지 않다.
    -> ext4의 블록그룹 대신 할당 그룹 용어를 사용한다.
    -> 기본적으로 볼륨을 8개의 할당그룹으로 분할한다.
    -> 8개 이상으로 분할할 수 있다.
    -> 파일 탐색을 위해 B+트리를 사용한다.
</pre>
### 스왑 메모리
<pre>
-> 메모리 확보를 위한 기법
-> mkswap file-name : 스왑 파일 생성
-> swapon [option] {partition | file-name} : 스왑 영역 활성화
-> swapoff [option] {partition | file-name | UUID } : 스왑 영역 활성화 해제
</pre>
# 6. 논리볼륨 관리
<pre>
-> 전통적인 디스크 파티셔닝은 장치의 설정이나 사이즈를 변경하는 작업이 어렵기 때문에 확장성이나 유연성 확보를 위한 작업
-> 논리 볼륨 구성
    -> 디스크 장치 - 물리 볼륨 - 볼륨 그룹 - 논리 볼륨
-> 논리 볼륨 생성
    -> 물리볼륨을 생성하기 위한 파티션 생성
       fdisk -l /dev/sdb 
       lvmdiskscan : 파티션정보 확인 
    -> 물리 볼륨 생성
       pcvreate partition1 partition2...
    -> 볼륨 그룹 구성
       vgcreate [option] volume-group-name physical-volume1 2...
       vgremove volume-group-name : 볼륨그룹 삭제 
    -> 논리 볼륨 생성
       lvcreate [option] volume-group-name
       lvremove logical-volume-path : 논리 볼륨 삭제
-> 스트라이프 볼륨(Striped Volume)
    -> 스토리지의 사이즈와 성능을 중시하는 논리 볼륨 생성 방식
-> 미러 볼륨
    -> 서비스 이상시 서비스가 중단되지 않고 정상적으로 서비스를 제공하며 데이터 손실을 방지하도록 하며 
       이러한 내결함성을 중시한 논리 볼륨 방식
-> RAID-5, RAID-6 볼륨
    -> 스트라이프 볼륨의 사이즈의 효율성으로 성능을 향상 시킬수 있고, 미러 볼륨만큼은 아니지만, 내결함성을 지원한다.
-> RAID-10 볼륨
    -> RAID-1 볼륨인 미러 볼륨을 먼저 구성하고, 생성된 미러 볼륨을 RAID-0 볼륨, 스트라이프 볼륨으로 연결하는 방식을 의미
</pre>
### 씬 프로비저닝 구성
<pre>
-> 볼륨의 크기를 실제 디스크에 할당되는 크기가 아닌 가상의 크기를 사용하는 방식
-> 씬 프로비저닝을 사용하여 논리 볼륨을 생성하면 볼륨 그룹에서 지정한 사이즈 전체를 바로 할당받지 않고,
   실제로 사용할 크기만큼만 사이즈를 할당받아 사용하다가 임계치에 도달하면 다시 볼륨의 사이즈를 증가시킨다.
</pre>
### 논리 볼륨 관련 명령어
<pre>
-> pvdisplay : 물리 볼륨 상태 확인
-> vgdisplay : 볼륨 그룹 상태 확인
-> lvdisplay : 논리 볼륨 상태 확인

-> 볼륨 그룹 확장
    -> vgextend volume-group-name physical-volume1 2...
-> 볼륨 그룹 축소
    -> vgreduce volume-group-name physical-volume1 2...
-> 논리 볼륨 확장
    -> lvextend [option] logical-volume-path
-> 논리 볼륨 축소
    -> lvreduce [option] logical-volume-path
-> 파일시스템 확장 명령어
    -> xfs_growfs mount-point
    -> resize2fs logical-volume-path
-> 파일시스템 축소 명령어\
    -> resize2fs logical-volume-path size
</pre>
# 7. Systemd
<pre>
-> 개요 : 기존의 init 프로세스를 대체하며 최신 리눅스에 도입되고 있는 새로운 PID 1번 프로세스이다.
          systemd는 시스템 관리, 로그 관리, 서비스 관리, 초기화 스크립트 관리 등의 시스템관리의 전반적인 작업을 수행한다.
-> ps- ef : 시스템에 동작중인 프로세스 목록중 systemd의 상태를 확인하는 명령어
-> systemd 기능 및 특징
    -> init 프로세스에 대한 호환성 제공
    -> systemd 유닛 사용
    -> 시스템 부팅 시 서비스 병렬 시작
    -> 사용자 요구에 맞게 서비스 실행
    -> 시스템 상태 스냅샷 지원
    -> 의존성 기반의 서비스 제어 로직 제공
    -> Upstart 대체
    -> CGroup(Contorl Group) 관리
    -> systemctl을 사용한 사용자 정의 명령 미지원
    -> systemd에 의해 실행된 서비스만 관리
    -> 시스템 셧다운 시 실행중인 서비스만 중지
    -> 서비스에 대해서 5분의 timeout 적용
    -> 소켓 기반 활성화
    -> 버스 기반 활성화
    -> 장치 기반 활성화
    -> 경로 기반 활성화
    -> 마운트 포인트와 자동마운트 포인트 관리
    -> 통합 로그 관리
</pre>
## system 유닛
<pre>
-> systemd 유닛 파일 위치
    -> /etc/systemd/system : 시스템 관리자가 수동으로 생성 및 관리하는 유닛들이 저장되는 디렉토리
    -> /run/systemd/system : 시스템이 런타임 상태일 때 임시로 유닛파일을 저장하는 디렉토리
    -> /usr/lib/systemd/system : 사용자가 패키지를 특정 유닛이 포함된 패키지를 설치하면 저장된느 디렉토리

-> systemd 유닛의 종류
    -> 확장자에 따라 유닛의 종류가 달라짐.
    -> 서비스 유닛, 장치 유닛, 마운트 유닛, 자동마운트 유닛, 스왑 유닛 등등..
</pre>

## systemctl 명령어 예제
<pre>
-> systemctl start 서비스명 : 서비스 시작
-> systemctl stop 서비스명 : 서비스 중지
-> systemctl enable unit-name : 유닛 활성화
-> systemctl status 서비스명 : 상태확인
-> systemctl list-units : 유닛의 실행 상태 확인
-> systemctl is-active unit-name : 특정 유닛의 실행 상태 확인
-> systemctl is-enabled unit-name : 특정 유닛의 활성화 상태 확인
-> systemctl list-dependencies unit-name : 특정 유닛의 의존성 확인
-> systemctl mask unit-name : 마스킹
-> systemctl unmask unit-name : 마스킹 해제
</pre>

# 8. 로그 관리
<pre>
-> 시스템에서 발생된 문제의 원인을 파악하거나 또는 인가되지 않은 접근으로부터 칩입 경로를 조회할 수 있다.
   systemd-journald를 통해서 시스템에서 발생한 모든 로그를 수집하고, 데이터 형식으로 관리한다.
-> system-journald에 의해서 수집되는 로그는 저널 데이터라고 부르며, /run/log/journal 디렉토리에 바이너리 파일 형태로 저장된다.
-> rsyslogd에 의해서 수집되는 로그의 파일 위치
    -> /var/log/messages : 대부분의 로그 기록
    -> /var/log/secure : 인증과 관련된 로그를 기록
    -> /var/log/mailog : 메일과 관련된 로그를 기록
    -> /var/log/cron : 주기적인 작업과 관련된 로그를 기록
    -> /var/log/boot.log : 부팅과정에 발생된 로그를 기록
-> rsyslogd
    -> 유닉스나 리눅스 계열 시스템에서 로그를 기록하기 위한 표준 프로토콜인 syslog를 사용하여 로그를 저장하는 프로세스이다.
       syslog에 비해 로그 전송 시 암호화옵션 등을 사용할 수 있어 보안이 강화되었고, 로그 처리 및 저장 성능이 우수하다.
-> /etc/rsyslog.conf
    -> 파일의 룰 부분에는 rsyslogd에 의해 전달되는 로그의 규칙들이 정의되어 있다.
-> 필터(Filter)
    -> 로그 메시지를 분류하기 위한 기준, 필터 조건에 따라 로그 메시지가 처리되는 방식을 결정할 수 있다.
-> 행동(Action)
    -> 필터에 의해서 선택된 로그들이 처리되는 방법을 정의하는 부분
       주로 파일에 메시지를 저장하거나, 시스템에 로그인된 사용자에게 메시지를 전달한다.
-> journalctl 명령어 : journalctl [option] [arg] : 저널 데이터 조회
</pre>

# 9. 리눅스 부트 프로세스 
<pre>
-> 부팅 절차 : BIOS/UEFI -> Boot Loader -> Kernel -> init -> Read inittab -> rc.sysinit -> runlevel1 2 3 ...
-> systemd 부팅 절차 : BIOS/UEFI -> Boot Loader -> Kernel -> systemd -> default.target -> grapgical.target ....
</pre>

# 10. 소프트웨어 패키지
<pre>
-> 개요 : 과거 리눅스에선 특정 소프트웨어를 설치하기 위해서는 아카이브 파일이나 압축파일로 되어 있는 파일에서 원본 소스 파일을 추출하고
          해당 파일을 컴파일하여 별도로 설치해야 하는 번거로운 작업 과정을 거쳐야 했다.
          그렇기 때문에 소프트웨어를 좀 더 쉽게 설치할 수 있는 방법이 필요했다.
          이 때 등장한 것이 바로 소프트웨어 패키지이다.
          
-> RPM(RedHat Package Manager)을 사용하여 패키지 관리
    -> RPM은 패키지를 관리하는 도구로써, 리눅스 사용자들이 보다 쉽게 소프트웨어 설치를 할 수 있도록 레드햇(RedHat)에서 개발한 방식이다.
    -> 근래에는 RPM은 설치에 사용하기 보다는 패키지에 대한 정보를 수집하거나 관리하는데 주로 사용되고 있다.
    -> 패키지 설치는 RPM보다는 YUM이라는 패키지 관리 도구를 주로 사용하고 있다.
    -> 패키지 확인 방법 : rpm -q [query-option[ [query-arg] 
    
-> YUM(Yellowdog Updater Modified)을 사용하여 패키지 관리
    -> YUM 저장소(repository)
        -> YUM 저장소는 패키지들을 저장해놓은 하나의 서버를 의미한다.
    -> yum subcommand [arg] : YUM 패키지 정보 확인
    -> yum [-y] install package-name : YUM 패키지 설치
    -> yum [-y] update [package-name] : YUM 패키지 업데이트
    -> yum [-y] remove package-name : YUM 패키지 제거
    -> yum groups subcommand [arg] : YUM 그룹 패키지
</pre>

# 11. 네트워크 
## 네트워크 관리
<pre>
-> ip address show [interface-name] : IP 정보 확인
   ifconfig [interface-name] 
-> ip route : 라우팅 테이블 확인
-> traceroute [option] destination : 목적지까지 가는 라우터 경로를 출력하는 명령어
-> nslookup [interface-name]: Domain의 IP를 조회하는 명령어
</pre>

## 네트워크 관리자
<pre>
-> 네트워크 관리자는 네트워크와 관련된 모든 설정을 관리하고 모니터링 하는 서비스이다.
   서비스 이름은 NetworkManager이며, systemd에서는 systemctl로 제어한다.
-> yum -y install NetworkManager : 네트워크 관리자 설치
   systemctl status NetworkManager : 네트워크 관리자를 설치한 뒤 서비스 확인

-> 레거시(Legacy) 네트워크 구성
    -> 기존 방식을 통한 네트워크 서비스 구성
    -> /etc/sysconfig/network-scripts : 설정 파일
    -> 레거시 네트워크 사용
        -> 기존 레거시 네트워크 사용을 하고싶다면 네트워크 관리를 중지 및 비활성화 해야함.
        -> systemctl stop NetworkManager 
        -> systemctl disable NetworkManager
        -> systemctl mask NetworkManager
        -> systemctl start network
        -> systemctl enable network
    -> 레거시 네트워크 설정
        -> /etc/sysconfig/network-scripts : ifcfg 파일 수정
    -> systemctl restart network : 변경후, 재시작
    -> ip address add ip/netmask dev interface-name : ip 명령을 사용하여 인터페이스에 IP 추가

-> 네트워크 관리자 도구 활용
    -> nmcli(Network Manager Command Line Interface)
        -> 네트워크 관리자가 제공하는 가장 강력한 커맨드 라인 도구
    -> nmcli connection show : 네트워크 연결 목록 확인
    -> nmcli connection add [subcommand1] [arg1] : 연결 생성
    -> nmcli connection delete connection-name : 연결설정 삭제
    -> nmcli connection modify connection-name subcommand arg : 연결 수정
    -> nmcli connection {up|down} connection-name : 설정 활성화/비활성화
</pre>

## 호스트이름 설정
<pre>
-> 개요 : 사용자는 다른 시스템과 통신하려면 대상 시스템의 IP주소를 알고 있어야 하지만, IP를 외우는 것은 매우 어렵다.
          따라서 주소로 변환하는 방식 DNS(Domain Name System) 서비스를 사용한다.
-> hostname : 시스템의 호스트이름을 확인
-> hostnamectl : 호스트이름의 정보를 확인
-> hostnamectl set-hostname hostname : 호스트이름 변경
</pre>

## OpenSSH(Open Secure Shell)
<pre>
-> 개요 :  관리자가 시스템을 관리할 때 직접 서버에서 접속하는 방식과 외부에서 네트워크를 통해 접속하는 방식이 있다.
           직접 서버에서 작업하는 경우는 서버의 모니터와 키보드를 직접 연결하여 사용하는 방식과 콘솔 연결방식으로 나누어진다.
           외부에서 네트워크를 통해 원격 접속할 때 사용하는 방식은 과거에는 텔넷(Telnet)이나 리모트쉘(rsh)을 사용했으며,
           근래에는 보안쉘(SSH)을 이용한 방식을 주로 사용하고 있다.
           SSH는 데이터 암호화할 때 비대칭 키 암호화와 대칭키 암호화 알고리즘을 사용한다.
-> ex) ssh user01@example.co.kr : example.co.kr 시스템의 user01로 원격 접속하는 예시

-> 연결과정
    -> 클라이언트는 서버에게 접속 요청
    -> 서버는 클라이언트에게 공개 키 전송
    -> 클라이어트는 비밀 키 생성
    -> 클라이언트는 비밀 키 암호화
    -> 클라이언트는 서버에 비밀 키 전송
    -> 서버는 비밀 키 복호화

-> /etc/ssh : OpenSSH의 구성 파일 위치
-> /etc/ssh/sshd_config : OpenSSH 서버 설정파일

-> OpenSSH 키 기반 인증
    -> 개요 : 원격접속 할 땐 기본적으로 사용자의 아이디나 패스워드를 알아야한다.
              하지만 키 기반 인증은 암호를 알고있지 않아도 접속이 가능하다.
    -> 키 파일 생성 : ssh-keygen [option] [arg] 
    -> 키 파일 복사 : ssh-copy-id [option] [arg] [user-name]@address 

-> 원격 파일 전송
    -> scp [option] source1 source2 ..dstination : scp(Secure Copy) 명어 사용, rcp를 대체하기 위한 명령어 rcp는 평문전송 이기때문에 대체하였다.
    -> sftp [user-name]@address : sftp 명령 사용
</pre>

# 12. NTP(Network Time Protocol) 서버 관리
<pre>
-> 개요 : NTP는 컴퓨터의 시스템의 시간을 동기화할 때 사용하는 프로토콜이다.

-> NTP 동작 방식
    -> 시간정보를 동기화시키고자 하는 클라이언트 NTP 서버로 정확한 시간정보 요청을 전송한다.
       이 때, 자신이 가지고 있는 정확하지 않은 시간 정보를 함께 전송한다.
    -> NTP 서버는 요청을 수신 후 응답을 준비한다.
    -> NTP 서버는 요청을 전송받은 정확한 시간정보와, 요청 수신 후 응답하는 정확한 시간정보를 포함한 정보를 응답한다.

-> NTP 계층 구조
    -> NTP 서버의 특징
        -> Stratum0 : 가장 상위 계층, Stratum 1에게 정확한 시간정보를 제공
        -> Stratum1 : Stratum0으로 부터 시간정보 수신 및 동기화, 일반적으로 Stratum2 서버 이외의 접근을 차단한다.
        -> Stratum2 : 일반적인 시간동기화 요청에 사용할 수 있는 최상위 NTP 서버
        -> Stratum n : 각각 자신보다 상위 단계의 서버로부터 시간을 동기화한다.
                       계층이 낮아질수록 시간정확도가 낮을 가능성이 높다.
</pre>

## chrony 서비스
<pre>
-> 이전 버전의 리눅스는 NTP서비스를 사용하기 위해 NTP 도구를 상요하여 시간동기화를 수행하였으나,
   최신 리눅스에서 chrony로 교체되었다.
-> /etc/chrony.conf : chronyd 서비스는 파일을 참조하여 동작환경을 설정한다.
-> chronyc tracking : 동기화 시간정보 확인
-> chronyc sources : 시간 소스 정보 확인
-> chronyc sourcestats : drift rate 및 offset 추정 정보 확인
-> ex) 설정변경
   -> chronyc
   -> add server ntp.ewha.or.kr
   -> authhash SHA1
   -> vi /etc/chronyc.conf : vi를 사용하여 영구 설정 변경
     -> systemctl restart chronyd : 서비스 재시작

-> 수동시간 설정
    -> 네트워크를 통해 NTP 서버에서 시간정보를 수신할 수 없는 경우, 또는 사용자가 임의로 시간을 변경하고 싶을 경우 date나 timedatectl를 사용
</pre>
# 13. 방화벽 관리
<pre>
-> 개요 : 리눅스 방화벽은 외부의 네트워크에서 내부의 시스템으로 접근하는 네트워크 패킷을 차단하는 서비스이다.
          리눅스에서 제공하는 방화벽은 Netfilter에 의해서 적용된다.
          Netfilter는 시스템에 접근하는 네트워크 패킷을 시스템 내부로 전달할지 아니면 폐기할지 결정하는 커널 모듈이다.
          사용자들이 Netfilter를 사용하여 네트워크 접근을 직접 제어하지 않고 서비스 관리 도구를 이용하여 제어한다.
</pre>
## firewalld
<pre>
-> firewalld는 systemd와 함께 도입된 방화벽 서비스이다.
   기존의 iptables의 한계와 단점을 보완한느 방화벽 서비스이다.
   iptables는 룰을 변경할 때 서비스를 중지해야 하기 때문에 네트워크 변화가 수시로 발생되는 오픈스택이나 가상화에서 사용할 때 제한적이다.
   하지만 firewalld는 동적으로 방화벽 설정을 변경할 수 있기 때문에 수시로 변하는 네트워크 변화에 대해 제한 없이 대응 할 수 있다는 장점이 존재한다.
-> /usr/lib/firewalld , /etc/firewalld : firewalld 서비스 설정 관련 파일, 기본 구성 대비용 적용되는 설정 파일
-> 동작 원리
    -> firewalld에서는 사전에 정의된 영역이 제공된다. 
       하지만 이 영역이 모두 사용되는 것은 아니고, 실제로는 활성화된 영역(Active Zone)만 사용된다.
       /etc/firewalld 디렉토리 내에 영역의 이름으로 된 XML파일이 존재하면 해당 영역은 활성화 영역이다.
       활성화 영역의 조건은 두가지 이다.
       -> 출발지의 주소(Source Address) 규칙 존재
       -> 연결되어 있는 인터페이스(Interface) 존재
-> 관련 명령어
    -> 상태 및 정보 확인 옵션
        -> firewall-cmd --state : firewalld 실행 상태 확인
        -> firewall-cmd --get-zones :  사전에 정의된 영역 확인
        -> firewall-cmd --get-services : 사전에 정의된 서비스 확인
        -> firewall-cmd --get-active-zones : 활성화된 영역 확인
        -> firewall-cmd --get-default-zone : 기본 영역 확인
        -> firewall-cmd --list-all --zone=home : 설정된 규칙 확인
    -> 규칙 설정 옵션
        -> firewall-cmd --set-default-zone=home : 기본 영역 설정
        -> firewall-cmd --add-interface=ens37 --zone=hone : 특정 영역에 인터페이스 연결 추가
        -> firewall-cmd --change-interface=ens37 --zone=hone : 영역에 연결된 인터페이스 변경
        -> firewall-cmd --add-source=192.168.0.0/24 --zone=public --permanent : 출발지 주소 규칙 추가
        -> firewall-cmd --remove-source=192.168.0.0/24 : 출발지 주소 규칙 제거
-> 리치 규칙(Rich Rule)
    -> firewall-cmd에서 제공하는 명령으로 대부분의 규칙 설정을 할 수 있지만 세부적인 설정을 수정하는 기능이 부족하다.
       firewalld에서 방화벽 규칙을 세부적으로 설정할 때 사용되는 것.
</pre>

# 14. 네트워크 티밍
<pre>
-> 개요 : 일반적인 네트워크 구성은 하나의 인터페이스에 하나의 IP주소를 설정하는 것이다.
          하지만, 티밍 구성은 여러 개의 물리 인터페이스를 묶어 하나의 논리 인터페이스를 구성하고 이 논리 인터페이스에 IP주소를 부여하는 방식이다.
          이러한 티밍 구성을 하는 이유는 대역폭을 늘려 데이터 처리량을 높여주거나, 부하분산을 통해 효율성을 높이고,
          트래픽 처리 속도를 향상시킬 수 있고, 네트워크 인터페이스에 장애 발생 시 좀 더 안전한 네트워크 구성을 위해 설정한다.
       
-> 네트워크 티밍 구성 방식:
    -> 포트 트렁킹(Port Trunking)
    -> 링크 집계(Link Aggregation)
    -> 채널 본딩(Channel Bonding)
    -> 이더넷 본딩(Ethernet Bonding)
    -> 채널 티밍(Channel Teaming)
    -> NIC 티밍(NIC Teaming)

-> 부가설명 : 리눅스에서는 예전부터 이런 구성을 본딩(Bonding) 이라고 하였으며,
             최신 엔터프라이즈 리눅스에서는 기존의 본딩 방식과는 별개로 티밍 방식을 제공한다. 
             물론 여전히 기존 본딩 방식으로 네트워크를 구성할 수 있다.
             네트워크 티밍의 장점은 새롭게 설계된 커널 모듈로서 팀 인터페이스로 유입되는 트래픽을 더 빠르게 처리하거나,
             LACP(Link Aggregation Control Protocol)를 완벽하게 지원하며, IPv6 링크 모니터링 및 D-Bus 인터페이스까지 지원한다.
             또한 본딩의 동작은 커널 영역에서 이뤄지지만, 티밍은 사용자 영역에서 동작하기 때문에 쉽게 관리하고 운영이 가능하다.
</pre>

## 티밍(teaming)
<pre>
-> 여러 개의 물리적인 인터페이스를 하나의 논리적인 인터페이스로 구성하는 네트워크 구성 방식이다.
-> 여기에서 사용되는 물리적인 인터페이스를 티밍에서는 슬레이브 혹은 포트 인터페이스라고 부르고, 
   해당 인터페이스들의 묶음인 논리적인 인터페이스를 마스터 혹은 팀 인터페이스라고 한다.
-> 이러한 물리적인 인터페이스인 포트 인터페이스는 실질적으로 데이터가 이동하는 통로 역할을 수행하게 되며,
   가상의 인터페이스인 팀 인터페이스에는 데이터의 처리 방식과 통신에 사용할 IP주소와 같은 논리적인 구성이 포함되는 인터페이스이다.
-> 티밍이 활성화되려면 팀 인터페이스에 연결된 포트 인터페이스들이 활성화되어야 한다.
-> 티밍은 팀 인터페이스에 IP주소를 부여할 수  있는데 이 때 IP주소를 DHCP서버에서 동적으로 할당 받을 수 도 있고, 정적 설정도 가능하다.
-> 만약 동적으로 IP 주소로 할당받으려면 포트 인터페이스는 반드시 네트워크에 연결되어 있어야 한다.
   정적 IP 설정은 포트 인터페이스가 연결되기 전에 설정할 수 있지만 동작하지 않는다.
</pre>
## 러너(Runner)
<pre>
-> 티밍은 설정에 따라 트래픽 처리 방식이 달라지는데 여기에서 처리 방식을 결정해주는 것이 러너이다.
-> 러너는 그 설정에 따라 데이터 처리량을 높여주거나, 효율성을 높이고 처리 속도를 향상시킬 수도 있고,
   네트워크 연결에 장애가 발생했을 때 끊어짐 없이 안정적인 서비스를 제공할 수 있도록 네트워크를 구성할 수 있다.
-> 처리방식
    -> broadcast : 모든 포트로 데이터 전송
    -> roundRobin : 각 포트로 순차적 데이터 전송
    -> loadbalance : 부하분산 방식의 패킷 전송
    -> activebackup : 장애조치를 위한 설정
    -> lacp : LACP 구현 스위치 기능상의 지원이 필요
</pre>
## 티밍(teaming)과 본딩(Bonding)
<pre>
-> 공통점 : 모두 대역폭의 확장이나 부하분산, 장애조치와 같은 목적을 위해 
           여러 개의 물리적인 네트워크를 묶어 하나의 논리적인 네트워크 인터페이스를 구성한다.
-> 차이점 : 본딩(Bonding)은 동작 레벨이 커널 레벨(Kernel level)에서의 드라이버 제어였다면
            티밍(teaming)은 사용자 레벨(User level)에서의 동작 제어와 설정을 제공하기 때문에
            기존의 드라이버에 영향을 미치지 않는 새로운 방식이다.
            티밍은 사용자 레벨에서 동작하면서 Team Netlink API라는 도구를 통해 드라이버를 관리한다.
-> 요약
    -> 티밍을 사용하면 기존에 본딩으로 구성할 때보다 훨씬 편리하고 유연하게 설정할 수 있다.
       또한 모듈 방식으로 제공하기 때문에 확장성이 좋고 성능 면에서도 더 나은 성능을 기대할 수 있다.
    -> 티밍의 장점
        -> 유저 레벨의 동작으로 유연성 및 패킷 처리량이 향상
        -> 사용자가 해시함수를 지정할 수 있다.
        -> IPv6에 대한 모니터링이 가능해졌다.
        -> 포트간의 우선순위를 지정할 수 있다.
        -> 오버헤드가 낮아져 성능이 향상되었다.
</pre>

          
