# 사용자 및 그룹 관리
-> 관련 폴더\
&nbsp;&nbsp;&nbsp;&nbsp;-> /etc/passwd : 시스템에 등록된 사용자 정보\
&nbsp;&nbsp;&nbsp;&nbsp;-> /etc/shadow : 시스템에 등록된 사용자의 패스워드와 패스워드에 대한 설정\
&nbsp;&nbsp;&nbsp;&nbsp;-> /etc/group : 시스템에 등록된 그룹정보\
&nbsp;&nbsp;&nbsp;&nbsp;-> /etc/gshadow : 시스템에 등록된 그룹의 패스워드와 패스워드에 대한 설정\
&nbsp;&nbsp;&nbsp;&nbsp;-> /etc/skel : 뼈대라는 의미로 사용자에 대한 기본적인 초기화 파일들을 저장하고 있는 디렉토리\
&nbsp;&nbsp;&nbsp;&nbsp;-> /etc/login.defs : 사용자나 그룹을 생성할 때 참고하는 기본 값들이 저장되어 있음\
&nbsp;&nbsp;&nbsp;&nbsp;-> /etc/sudoers : sudo 명령을 사용하기 위한 조건이 있는 파일\
&nbsp;&nbsp;&nbsp;&nbsp;-> /var/log/secure : 인증과 관련된 로그를 담는 파일\
\
-> 사용자 생성관리 명령어\
&nbsp;&nbsp;&nbsp;&nbsp;-> useradd [option] user-name : 사용자 생성\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;-> useradd -D : 기본 설정 확인\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;-> ex) useradd -u 2000 -g 10 -m -d /home/guest/user03 -s /bin/sh user03\
&nbsp;&nbsp;&nbsp;&nbsp;-> usermod [option] user-name : 사용자 정보 수정\
&nbsp;&nbsp;&nbsp;&nbsp;-> userdel [option] user-name : 사용자 삭제\
\
-> 그룹 생성관리 명령어\
&nbsp;&nbsp;&nbsp;&nbsp;-> groupadd [option] group-name : 그룹 생성\
&nbsp;&nbsp;&nbsp;&nbsp;-> groupmod [option] group-name : 그룹 정보 수정\
&nbsp;&nbsp;&nbsp;&nbsp;-> groupdel group-name : 그룹 삭제\
\
-> 기타 권한 명령어\
&nbsp;&nbsp;&nbsp;&nbsp;-> su [-] [user-name] : 사용자 전환\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;-> ex) sudo -i : 루트 사용자 전환\
&nbsp;&nbsp;&nbsp;&nbsp;-> sudo [option] [user-name] command : 로그아웃을 하지않고 특정 사용자 권한으로 수행\
&nbsp;&nbsp;&nbsp;&nbsp;-> chage [option] [argument] user-name :  /etc/shadow 파일에서 패스워드의 속성을 변경하는 명령어
