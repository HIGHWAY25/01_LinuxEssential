Linux Essential 과정 정리

Linux 기초 관리자 과정

============
제1장. 리눅스 소개
============

쉘 특성 *
프로세스 관리 *

직업관리자(컨트롤+시프트+esc)

=============
제2장. 리눅스 설치
=============

복제된 운영체제에서 변경 사항
* 호스트 이름
# hostnamectl 
* ip 설정
#nm-connection-editor

# yum install gnome-tweaks
인터넷상에 리눅스 패키지를 가지고 있는 서버


================
제3장. 리눅스 환경 설정
================

* SELinux(Secure Enhanced Linux) 끄기
-프로세스가 파일/디렉토리/포트를 보안 레이블(Security Label)을 통해 제어하는 기능

* (CentOS 6 이하) Runlevel? 동작 수준/동작 레벨, (CentOS 7 이상) Target? 동작 수준/동작 레벨

* 관리자 암호 변경
-@localhost=자기자신의 호스트 암호 변경
-사용자는 간단한 암호로 변경이 불가능

리눅스의 선수지식
* Runlevel == Target
  runlevel 3 == multi-user.target
  runlevel 5 == graphical.target

그래픽 모드 변경
  #systemctl isolate multi-user.target | graphical.target
  #systemctl set-default multi-user.target | graphical.target

* 서비스 기동
 #systemctl enable|disable firewalld
 #systemctl start|stop firewalld

* 제어 문자(Control Character)
- <CTRL + C>, <CTRL + D>

* 운영체제 전원 끄기
#power off
#halt
#init 0
#shutdown -h now

* 운영체제 재부팅
#reboot
#init 6

* 도움말과 암호변경
man CMD
	# ls --help
	# man ls 		 	/* 명령어나 파일의 이름으로 검색하는 경우 */
	# man -k calendar	 	/* keyword로 검색하는 경우 */
	# man -f passwd 	 	/* 목록으로 검색하는 경우 */
	# man -s 5 passwd 	/* section번호로 검색하는 경우 */

passwd CMD
	#passwd fedora  		/일반사용자는 아이디와 암호를 동일하게 설정할 수 없다.(관리자만 가능)/

* 옵션에 대한 설명
- 명령어의 옵션은 자리를 바꾸거나 혹은 합쳐서 사용해도 같은 의미로 동작한다.
	# ls -a -l -F
	# ls -al -F
	# ls -alF
	# ls -Fla

- 명령어의 옵션을 약자 형태로 쓰지 않기 위해서는 "--(double dash)"을 붙인다.
	# ls --all (# ls -a)

* 옵션의 인자 가 존재하는 경우의 예
- 명령의 인자가 아닌 옵션의 인자 값인 경우에는 옵션의 위치가 변경이 될 때, 같이 따라다녀야 한다.
	예) # find / -name core -type f
	# cmd -i -f arg1 (0)
	# cmd -f arg1 -i (0)
	# cmd -if arg1 (0)
	# cmd -fi arg1 (X)


명령어에 대한 man 페이지 확인
(간략하게 정보 확인) # ls --help (# CMD --help)
(자세하게 정보 확인) # man ls (# man CMD)

[명령어를 알지 못하는 경우]
[root@server1 ~]# man -k calendar 
cal (1) - display a calendar
cal (1p) - print a calendar
difftime (3p) - compute the difference between two calendar time values

* 메뉴얼 1 section-> 명령어, 파일에 대한 메뉴얼
* 매뉴얼 2,3 section -> 라이브러리에 대한 메뉴얼

[섹션 별로 검색할 경우]
[root@server1 ~]# man -f passwd
passwd (5) - password file
openssl-passwd (1ssl) - compute password hashes
passwd (1) - update user's authentication tokens

* 메뉴얼 section 1: passwd 명령어 메뉴얼
* 메뉴얼 section 5: /etc/passwd 파일에 대한 메뉴얼

[root@server1 ~]# man -s 1 passwd /* passwd 명령어 매뉴얼 페이지 */
[root@server1 ~]# man -s 5 passwd /* /etc/passwd 파일 매뉴얼 페이지 *

[추가적인 정보 확인 명령어]
# which ls 
# ls --help

[메뉴얼 섹션]
Section 1 : 사용자 명령(실행가능한 명령 및 쉘 프로그램)
Section 5 : 파일 형식(많은 구성 파일 및 구조의 경우)
Section 8 : 시스템 관리 및 권한 명령(유지 보수 작업)

[실무예] 시스템 관리자가 알아 두어야 하는 매뉴얼 섹션
- Section 1 : 명령어 및 데몬등에 대한 매뉴얼
- Section 5 : 운영체제 설정 파일 및 데몬의 설정 파일 매뉴얼
- Section 8 : 관리명령어 매뉴얼
[실무예] 개발자가 알아 두어야 하는 매뉴얼 섹션
- Section 2 : 시스템 콜 함수 매뉴얼
- Section 3 : 일반 함수 매뉴얼
- Section 9 : 리눅스 커널 API 매뉴얼

*암호 변경 권한 차이 
-root 사용자 : 모든 사용자의 암호를 변경 가능
-일반 사용자 : 자신의 암호만 변경 가능
*암호 변경 시 이전 암호 입력 여부 
-root 사용자 : 암호 변경시 이전 암호 물어보지 않고 변경 가능
-일반 사용자 : 이전 암호를 반드시 맞추어야만 새로운 암호 입력 가능

[명령어 형식](사용자는 자기 암호만 변경 가능, 관리자는 전부 가능)
# passwd 
# passwd fedora 

			

====================
제4장. 리눅스 기본 정보 확인	
====================

* 시스템 기본 정보 확인
uname CMD 
	# uname -a
	# cat /etc/redhat-release

[참고] rehat documentation ( https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/8 )

date CMD (시스템 시간과 날짜를 출력/설정한다.)
	[명령어 형식]
	# date 		/* 시스템 시간 출력 */
	# date 07241300   /* 시스템 시간 변경 */ (월일시분)
	# date +%m%d     /* 시간 출력 형태 변경 */

[실무예] NTP(Network Time Protocol) 서버에 시간 동기화
# vi /etc/chrony.conf	
server time.bora.net iburst	
# systemctl stop chronyd	
# systemctl start chronyd	

cal CMD (달력 출력)
	[명령어 형식]
	# cal 	    (현 년도 달력 출력)
	# cal 2002   (달력 출력)
	# cal 6 2002 (월과 달력 출력)

====================
제5장. 파일 및 디렉토리 관리
====================

1.디렉토리 이동 관련 명령어
[참고] 파일시스템 기본 구조
# man 7 hier  

pwd CMD (현재/작업 디렉토리명을 출력한다)
	[참고] PS1 변수="쉘 프롬프트"를 정의할 때 사용하는 변수($HOME/.bashrc)
	# export PS1='[\u@\h \w]$ '

cd CMD (작업 디렉토리를 변경한다.)
	* 경로(PATH) (/로 시작하냐의 차이점)
	- 상대경로(Relative PATH) 	# cd dir1
	- 절대경로(Absolute PATH) 	# cd /dir1

	(1) 상대경로를 사용한 디렉토리 이동
	# cd .. /* 상위 디렉토리로 이동 */
	# cd dir1 /* 현재 디렉토리 하위의 dir1 으로 이동 */
	[참고] 디렉토리 예약어
	. : 현재 디렉토리
	.. : 상위 디렉토리

	(2) 절대경로를 사용한 디렉토리 이동
	/etc/sysconfig/network-scripts
	↑ ↑ 	    ↑
	 a  b 	     c
	a는 최상위 디렉토리인 /(root) 디렉토리를 뜻한다.
	b, c는 디렉토리의 구분자이다. 절대경로를 사용하는 다른 예이다.
	# cd /tmp 
	# cd /etc/sysconfig 
	# cd /usr 

	[참고] 자신의 홈디렉토리 이동
	# cd
	# cd ~
	# cd $HOME

	[참고] 지정된 사용자 홈디렉토리 이동
	# cd ~fedora

	[실무예] 이전 디렉토리로 이동하기
	# cd -

	[실무예] 옆에 있는 디렉토리로 이동하기
	# cd ../dir2

2.디렉토리 관리 명령어(생성,확인,변경,삭제를 할 수 있는 명령어)

ls CMD(경로의 내용을 나열한다.)
	[명령어 형식]
	# ls -l 		/로그라인 정보 출력/
	# ls -ld		/현 디렉토리 정보 출력/
	# ls -l dir1 	/dir1 디렉토리 내용 출력/
	# ls -ld dir1 	/dir1 디렉토리 속성 정보 출력/
	# ls -lR		/하위 디렉토리까지 정보 출력/(R=Recursive)
	# ls -al 		/모든 디렉토리 정보 출력/
	# ls -F		/지시자를 표현하는 명령어/
	# ls -i 		/inode(index node) 번호를 확인하는 명령어/
	# ls -h		/사람이 이해하기 쉬운 형태로 변환하는 명령어/
	옵션: -l, -d, -a, -R, -i, -t ,-h, -r

	[참고] alias
	(선언) # alias ls='ls -l  |  grep "^-"'
	(확인) # alias 
	(해제) # unalias ls 

	[실무예] 실무에서 많이 사용되는 ls CMD
	# cd /Log_dir
	# ls -altr

mkdir CMD (경로 만들기)
	[명령어 형식]
	# mkdir dir1 		/* 현 디렉토리에 dir1 디렉토리 1개 생성 */
	# mkdir dir1 dir2 		/* 현 디렉토리에 dir1, dir2 디렉토리 2개 생성 */
	# mkdir -p dir3/dir2/dir1 	/* dir3 디렉토리 안에 dir2를 생성하고 dir2 안에 dir1을 생성 */

rmdir CMD (비어 있는 경로 지우기)
	# rm -rf dir1 	/* -r : recursive, -f : force */ 
	(주의) 파일/디렉토리 삭제 주의해야한다.

3.파일 관리 명령어

touch CMD (파일 생성)
	[명령어 형식]
	# touch file2 		/* file2 파일 1개 생성 */
	# touch file1 file2 		/* file1, file2 파일 2개 생성 */
	# touch -t 08081230 file1 	/* file1 수정 시간 변경(월,일,시,분) */

cp CMD (파일 복사)
	[명령어 형식]
	# cp file1 file2 	/* file1 파일내용을 file2로 생성 */
	# cp file1 dir1 	/* file1 파일내용을 dir1디렉토리에 file1 생성 */
	# cp -r dir1 dir2 	/* dir1 디렉토리를 dir2디렉토리로 복사 */
	OPTIONS:  -r, -p, -a,-i, -t

	[실무예] 설정 파일을 백업하는 경우
	# cp -p httpd.conf httpd.conf.orig
	# cp -a /src /src.orig

	[실무예] 로그 파일 비우기
	# cp /dev/null file.log 
	# cat /dev/null > file.log 
	# > file.log 

mv CMD(파일 옮기기)
	[명령어 형식]
	# mv file1 file2 	/* file1 파일이 이름이 file2로 변함 */
	# mv file1 dir1 	/* file1 파일이 dir1 디렉토리에 하위경로로 이동 */
	# mv dir1 dir2 	/* dir1 디렉토리가 dir2 디렉토리에 하위경로로 이동 */
	옵션: -i, -f
	*용어: 파일 시스템(File System)이란? 파일을 저장하고 관리하는 구조 체계
     	 -ex) ext4, xfs, fat32, ntfs

	[참고] 와일드 캐릭터: 하나의 문자가 여러개의 문자의 의미를 포함하는 문자
	* : 0 or more character (except .file) 
	? : one charater 
	{ } : 선택적인 하나의 문자열(단어) 
	[ ] : 선택적인 하나의 문자

rm CMD(파일 지우기)
	[명령어 형식]
	# rm file1 	/* file1 파일 1개 삭제 */
	# rm file1 file2 	/* file1, file2 파일 2개 삭제 */ 
	# rm -r dir1 	/* dir1 디렉토리 하위경로까지 삭제 */

	[실무예] rm 명령어로 지운 파일 복원하기
	(TUI) extundelete CMD 
	(GUI) TestDisk 툴

4.파일 내용 확인 명령어
cat CMD (파일의 내용을 보여준다.)
	# cat -n file1	 	/* file1 파일내용을 줄번호와 함께 출력 */
	# cat file1 file2 > file3 	/* file1, file2 출력 결과를 file3에 저장 */

more CMD
	# CMD | more
	# ps -ef | more
	# cat /etc/services | more
	# netstat -an | more
	# systemctl list-unit-files
	[참고] 파이프(|) 기호

head CMD (파일 첫번째 행을 기분으로 출력)
	# head /etc/passwd 	(# head -10 /etc/passwd, # head -n 10 /etc/passwd)
	# head -n 5 /etc/passwd 	/* 숫자에 해당하는 라인 번호 수 만큼만 출력 (기본은 10줄) */
	# alias pps='ps -ef | head -1 ; ps -ef | grep $1'
	# alias nstate='netstat -antup | head -2 ; netstat -ant up | grep $1'

tail CMD (파일의 마지막 행을 기준으로 출력)
	# top
	# tail -f /var/log/messages 

	# tail -f /var/log/messages | egrep -i '(warn|fail|error|crit|alert|emerg)'
	# tail -f /var/log/messages /var/log/secure

	[참고] telent 서비스 기동하기
	# yum install telnet telnet-server
	# systemctl start telnet.socket 
	# systemctl enable telnet.socket
	(# systemctl enable --now telnet.socket -> start와 enable을 전부 실행한다.)

5.기타 관리용 명령어

wc CMD
	[참고] 데이터 수집(Data Gathering)
	# ps -ef | tail -n +2 | wc -l
	# ps -ef | grep httpd | wc -l
	# cal etc/passwd | wc -l
	# rpm -qa | wc -l
	# df -k / tail -l | awk '{print $5}'
	# cat /var/log/messages | grep 'Jan 19' | grep 'Started Telnet Server' | wc -l

su CMD (사용자와 그룹 ID 를 교체하여 쉘을 실행한다)
	[명령어 형식]
	# su oracle
	# su - fedora 	/사용자의 계정 권한을 모두 가지고 있다./
	# su - root
sudo CMD (/etc/sudoers, /etc/sudoers.d/*)
	root 암호를 공유할 필요가 없다. -> 사용자 암호 입력
	사용자가 관리자권한으로 수행하는 사용할 수 있는 명령어의 제한을 둘수 있다. 
	[명령어 형식]	
	# sudo CMD ($ sudo su - root) 
	# sudo -l 
	# sudo -i ($ su - root)

id CMD (실제, 유효 UID와 GID를 출력)
groups CMD (사용자가 속한 그룹들을 출력)
	[명령어 형식]
	# groups /* 현재 사용중인 사용자의 그룹을 보여줌 */
	# groups user1 /* user1의 그룹을 보여줌 */
	# groups user1 user2 /* user1, user2의 그룹을 보여줌 */

last CMD (/var/log/wtmp)
	# last -i
	# last -f /var/log/wtmp.20230128	/백업파일/

lastlog (/var/log/lastlog) 	/사용자의 마지막 로그인을 한 시간 출력/
	
lastb CMD (/var/log/btmp)	/접속 실패를 기록한 파일을 출력/
	#
who CMD (var/run/utmp)
	# who		/* 현재 시스템에 접속 중인 모든 사용자 */
	# who -r 	/* 현재 사용자의 Runlevel 확인 */
	# who am i 	/* 로그인한 사용자 정보 확인 */
	# who -H 	/* 헤드라인과 같이 출력 */
	# whoami 	/* 현재 사용자명 확인 */
w CMD
	[참고] 모니터링 구문
	# while true
	# do
		# echo "--'date'--"
		# CMD
		# sleep 2
	# done
	[참고] watch CMD (모니터링)

exit CMD	(현재의  쉘을 종료한다.)
	# exit [number]	/* 값을 지정해준다면 0은 정상종료, 1~255는 비정상 종료 */
