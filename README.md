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
<pre>

</pre>




          
