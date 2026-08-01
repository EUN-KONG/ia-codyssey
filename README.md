# AI/SW 개발 워크스테이션 구축

## 1. 프로젝트 개요
터미널, Docker, Git을 활용해 어디서나 재현 가능한 실행 환경을 구축하고, 협업과 배포의 기반이 되는 인프라 설계 원칙을 체득한다.

## 2. 실행 환경
| 항목 | 내용 |
|------|------|
| OS | macOS (Darwin 24.6.0) |
| Shell | zsh |
| Terminal | macOS 기본 터미널 |
| Docker | 28.5.2 |
| Git | 2.53.0 |

#### 확인 명령어
```bash
uname -a       # OS 확인
echo $SHELL    # Shell 확인
docker --version  # Docker 버전 확인
git --version     # Git 버전 확인
```

## 3. 수행 항목 체크리스트
- [x] **터미널**
- [x] **권한**
- [x] **Docker**
- [x] **Dockerfile**
- [] **포트**
- [] **볼륨**
- [] **마운트**
- [] **Git**
- [] **GitHub**
     
## 4. 검증 방법 및 결과 위치  
| 항목 | 결과 위치 |
|------|-----------|
| 터미널 조작 | [바로가기](#1-터미널-조작-로그-기록) |
| 권한 실습 | [바로가기](#2-권한-실습) |
| Docker 설치 점검 | [바로가기](#3-docker-설치-및-기본-점검) |
| Docker 운영 명령 | [바로가기](#4-docker-기본-운영-명령) |
| 컨테이너 실행 | [바로가기](#5-컨테이너-실행-실습) |
| Dockerfile 커스텀 | [바로가기](#6-커스텀-이미지-제작) |
| 포트 매핑 | [바로가기](#7-포트-매핑-및-접속-증거) |
| 볼륨 영속성 | [바로가기](#8-docker-볼륨-영속성-검증) |
| Git/GitHub 연동 | [바로가기](#9-git-설정-및-github-연동) |

---

## 1. 터미널 조작 로그 기록

**현재 위치 확인**
```bash
pwd
```
```
ieunbin@eunbin-ui-MacBookAir ~ % pwd
/Users/ieunbin
```

**목록 확인 (숨김 파일 포함)**
```bash
ls -a
```
```
ieunbin@eunbin-ui-MacBookAir ~ % ls -a
.			.vscode			Music
..			.zsh_sessions		OrbStack
.CFUserTextEncoding	Desktop			Pictures
.docker			Documents		Public
.orbstack		Downloads		
.ssh			Library
.Trash			Movies
```

**디렉토리 생성**
```bash
mkdir 폴더명
```
```
ieunbin@eunbin-ui-MacBookAir ~ % mkdir test
ieunbin@eunbin-ui-MacBookAir ~ % ls
Applications	Downloads	Music		Public
Desktop		Library		Pictures	nextjs-blog
Documents	Movies		Postman		test
```

**디렉토리 이동**
```bash
cd 폴더명        # 하위 폴더로 이동
cd ..           # 상위 폴더로 이동
```
```
ieunbin@eunbin-ui-MacBookAir ~ % cd test
ieunbin@eunbin-ui-MacBookAir test % cd ..
ieunbin@eunbin-ui-MacBookAir ~ %
```

**폴더 복사**
```bash
cp -r 원본폴더 복사할폴더
```
```
ieunbin@eunbin-ui-MacBookAir ~ % cp -r test test2 
ieunbin@eunbin-ui-MacBookAir ~ % ls
Applications	Downloads	Music		Public		test2
Desktop		Library		Pictures	nextjs-blog
Documents	Movies		Postman		test
```

**폴더 이동/이름변경**
```bash
mv 원본폴더 이동/이름변경할폴더
```
```
ieunbin@eunbin-ui-MacBookAir ~ % mv test2 test3
ieunbin@eunbin-ui-MacBookAir ~ % ls
Applications	Downloads	Music		Public		test3
Desktop		Library		Pictures	nextjs-blog
Documents	Movies		Postman		test
ieunbin@eunbin-ui-MacBookAir ~ % mv test test3  
ieunbin@eunbin-ui-MacBookAir ~ % ls
Applications	Downloads	Music		Public
Desktop		Library		Pictures	nextjs-blog
Documents	Movies		Postman		test3
ieunbin@eunbin-ui-MacBookAir ~ % cd test3
ieunbin@eunbin-ui-MacBookAir test3 % ls
test
```

**폴더 삭제**
```bash
rm -r 폴더명
```
```
ieunbin@eunbin-ui-MacBookAir test3 % rm -r test
ieunbin@eunbin-ui-MacBookAir test3 % ls
ieunbin@eunbin-ui-MacBookAir test3 % 
```

**빈 파일 생성**
```bash
touch 파일명
```
```
ieunbin@eunbin-ui-MacBookAir ~ % touch file
ieunbin@eunbin-ui-MacBookAir ~ % ls
Applications	Downloads	Music		Public		test3
Desktop		Library		Pictures	file
Documents	Movies		Postman		nextjs-blog
```

**파일 내용 확인**
```bash
cat 파일명
```
```
ieunbin@eunbin-ui-MacBookAir ~ % echo "안녕" > file  
ieunbin@eunbin-ui-MacBookAir ~ % cat file
안녕
```

---

## 2. 권한 실습

**권한 확인**
```bash
ls -l 파일명
```
```
ieunbin@eunbin-ui-MacBookAir ~ % mkdir directory
ieunbin@eunbin-ui-MacBookAir ~ % touch file.txt
ieunbin@eunbin-ui-MacBookAir ~ % ls -l
drwxr-xr-x   2 ieunbin  staff    64  7 29 21:38 directory
-rw-r--r--   1 ieunbin  staff     0  7 29 21:33 file.txt
```

**권한 변경**
```bash
chmod 000 파일명
```
```
ieunbin@eunbin-ui-MacBookAir ~ % chmod 644 directory
ieunbin@eunbin-ui-MacBookAir ~ % chmod 777 file.txt
```

**권한 변경 후 확인**
```bash
ls -l 파일명
```
```
ieunbin@eunbin-ui-MacBookAir ~ % ls -l directory
drw-r--r--   2 ieunbin  staff    64  7 29 21:38 directory
ieunbin@eunbin-ui-MacBookAir ~ % ls -l file.txt
-rwxrwxrwx   1 ieunbin  staff     0  7 29 21:33 file.txt
```

**권한 변경 전/후 비교**
```
directory
- 변경 전:drwxr-xr-x 
- 변경 후:drw-r--r-- 
```
```
file.txt
- 변경 전:-rw-r--r-- 
- 변경 후:-rwxrwxrwx
```

---

## 3. Docker 설치 및 기본 점검

**버전 확인**
```bash
docker --version
```
```
leeeunbin2199174@c3r1s4 ~ % docker --version
Docker version 28.5.2, build ecc6942
```

**데몬 동작 확인**
```bash
docker info
```
```
leeeunbin2199174@c3r1s4 ~ % docker info
Client:
 Version:    28.5.2
 Context:    orbstack
 Debug Mode: false
 Plugins:
  buildx: Docker Buildx (Docker Inc.)
    Version:  v0.29.1
    Path:     /Users/leeeunbin2199174/.docker/cli-plugins/docker-buildx
  compose: Docker Compose (Docker Inc.)
    Version:  v2.40.3
    Path:     /Users/leeeunbin2199174/.docker/cli-plugins/docker-compose

Server:
 Containers: 0
  Running: 0
  Paused: 0
  Stopped: 0
 Images: 0
 Server Version: 28.5.2
 Storage Driver: overlay2
  Backing Filesystem: btrfs
  Supports d_type: true
  Using metacopy: false
  Native Overlay Diff: true
  userxattr: false
 Logging Driver: json-file
 Cgroup Driver: cgroupfs
 Cgroup Version: 2
 Plugins:
  Volume: local
  Network: bridge host ipvlan macvlan null overlay
  Log: awslogs fluentd gcplogs gelf journald json-file local splunk syslog
 CDI spec directories:
  /etc/cdi
  /var/run/cdi
 Swarm: inactive
 Runtimes: io.containerd.runc.v2 runc
 Default Runtime: runc
 Init Binary: docker-init
 containerd version: 1c4457e00facac03ce1d75f7b6777a7a851e5c41
 runc version: d842d7719497cc3b774fd71620278ac9e17710e0
 init version: de40ad0
 Security Options:
  seccomp
   Profile: builtin
  cgroupns
 Kernel Version: 6.17.8-orbstack-00308-g8f9c941121b1
 Operating System: OrbStack
 OSType: linux
 Architecture: x86_64
 CPUs: 6
 Total Memory: 15.67GiB
 Name: orbstack
 ID: e9587c7b-d32b-46d2-9afa-352b9bc61985
 Docker Root Dir: /var/lib/docker
 Debug Mode: false
 Experimental: false
 Insecure Registries:
  ::1/128
  127.0.0.0/8
 Live Restore Enabled: false
 Product License: Community Engine
 Default Address Pools:
   Base: 192.168.97.0/24, Size: 24
   Base: 192.168.107.0/24, Size: 24
   Base: 192.168.117.0/24, Size: 24
   Base: 192.168.147.0/24, Size: 24
   Base: 192.168.148.0/24, Size: 24
   Base: 192.168.155.0/24, Size: 24
   Base: 192.168.156.0/24, Size: 24
   Base: 192.168.158.0/24, Size: 24
   Base: 192.168.163.0/24, Size: 24
   Base: 192.168.164.0/24, Size: 24
   Base: 192.168.165.0/24, Size: 24
   Base: 192.168.166.0/24, Size: 24
   Base: 192.168.167.0/24, Size: 24
   Base: 192.168.171.0/24, Size: 24
   Base: 192.168.172.0/24, Size: 24
   Base: 192.168.181.0/24, Size: 24
   Base: 192.168.183.0/24, Size: 24
   Base: 192.168.186.0/24, Size: 24
   Base: 192.168.207.0/24, Size: 24
   Base: 192.168.214.0/24, Size: 24
   Base: 192.168.215.0/24, Size: 24
   Base: 192.168.216.0/24, Size: 24
   Base: 192.168.223.0/24, Size: 24
   Base: 192.168.227.0/24, Size: 24
   Base: 192.168.228.0/24, Size: 24
   Base: 192.168.229.0/24, Size: 24
   Base: 192.168.237.0/24, Size: 24
   Base: 192.168.239.0/24, Size: 24
   Base: 192.168.242.0/24, Size: 24
   Base: 192.168.247.0/24, Size: 24
   Base: fd07:b51a:cc66:d000::/56, Size: 64

WARNING: DOCKER_INSECURE_NO_IPTABLES_RAW is set
```

---

## 4. Docker 기본 운영 명령

**이미지 다운로드**
```bash
docker pull 이미지명
```
```
ieunbin@eunbin-ui-MacBookAir ~ % docker pull ubuntu
Using default tag: latest
latest: Pulling from library/ubuntu
ed819469700f: Pull complete 
a3679419df18: Pull complete 
Digest: sha256:3131b4cc82a783df6c9df078f86e01819a13594b865c2cad47bd1bca2b7063bb
Status: Downloaded newer image for ubuntu:latest
docker.io/library/ubuntu:latest
```

**이미지 목록 확인**
```bash
docker images
```
```
ieunbin@eunbin-ui-MacBookAir ~ % docker images
REPOSITORY   TAG       IMAGE ID       CREATED       SIZE
ubuntu       latest    de7345b16e94   2 weeks ago   100MB
```

**컨테이너 실행**
```bash
docker run 이미지명
```
```
ieunbin@eunbin-ui-MacBookAir ~ % docker run ubuntu
ieunbin@eunbin-ui-MacBookAir ~ % docker run -d ubuntu sleep 300
```
<img width="972" height="147" alt="image" src="https://github.com/user-attachments/assets/0cf0e641-ebf2-409e-ad15-80f7ca106c85" />

**실행 중인 컨테이너 목록**
```bash
docker ps
```
```
ieunbin@eunbin-ui-MacBookAir ~ % docker ps
CONTAINER ID   IMAGE     COMMAND       CREATED              STATUS              PORTS     NAMES
c7ae8682c6a5   ubuntu    "sleep 300"   About a minute ago   Up About a minute             great_ritchie
```

**컨테이너 중지**
```bash
docker stop 컨테이너명
```
```
ieunbin@eunbin-ui-MacBookAir ~ % docker stop great_ritchie
great_ritchie
```
<img width="970" height="103" alt="image" src="https://github.com/user-attachments/assets/57002971-63a0-4fc0-9a13-028039bf6aad" />

**전체 컨테이너 목록**
```bash
docker ps -a
```
```
ieunbin@eunbin-ui-MacBookAir ~ % docker ps -a
CONTAINER ID   IMAGE     COMMAND       CREATED          STATUS                       PORTS     NAMES
cccb94cb6cc5   ubuntu    "/bin/bash"   32 seconds ago   Exited (0) 31 seconds ago              amazing_goodall
c7ae8682c6a5   ubuntu    "sleep 300"   13 minutes ago   Exited (137) 3 minutes ago             great_ritchie
```

**로그 확인**
```bash
docker logs 컨테이너명
```
```
ieunbin@eunbin-ui-MacBookAir ~ % docker logs keen_feistel
/docker-entrypoint.sh: /docker-entrypoint.d/ is not empty, will attempt to perform configuration
/docker-entrypoint.sh: Looking for shell scripts in /docker-entrypoint.d/
/docker-entrypoint.sh: Launching /docker-entrypoint.d/10-listen-on-ipv6-by-default.sh
10-listen-on-ipv6-by-default.sh: info: Getting the checksum of /etc/nginx/conf.d/default.conf
10-listen-on-ipv6-by-default.sh: info: Enabled listen on IPv6 in /etc/nginx/conf.d/default.conf
/docker-entrypoint.sh: Sourcing /docker-entrypoint.d/15-local-resolvers.envsh
/docker-entrypoint.sh: Launching /docker-entrypoint.d/20-envsubst-on-templates.sh
/docker-entrypoint.sh: Launching /docker-entrypoint.d/30-tune-worker-processes.sh
/docker-entrypoint.sh: Configuration complete; ready for start up
2026/07/29 17:44:53 [notice] 1#1: using the "epoll" event method
2026/07/29 17:44:53 [notice] 1#1: nginx/1.31.3
2026/07/29 17:44:53 [notice] 1#1: built by gcc 14.2.0 (Debian 14.2.0-19) 
2026/07/29 17:44:53 [notice] 1#1: OS: Linux 5.15.49-linuxkit
2026/07/29 17:44:53 [notice] 1#1: getrlimit(RLIMIT_NOFILE): 1048576:1048576
2026/07/29 17:44:53 [notice] 1#1: start worker processes
2026/07/29 17:44:53 [notice] 1#1: start worker process 29
2026/07/29 17:44:53 [notice] 1#1: start worker process 30
```

**리소스 확인**
```bash
docker stats
```
```
ieunbin@eunbin-ui-MacBookAir ~ % docker stats
CONTAINER ID   NAME           CPU %     MEM USAGE / LIMIT     MEM %     NET I/O       BLOCK I/O     PIDS
9d3031be6b1f   keen_feistel   0.00%     3.367MiB / 3.842GiB   0.09%     1.23kB / 0B   0B / 12.3kB   3
```

---

## 5. 컨테이너 실행 실습

**hello-world 실행**
```bash
docker run hello-world
```
```
ieunbin@eunbin-ui-MacBookAir ~ % docker run hello-world
Unable to find image 'hello-world:latest' locally
latest: Pulling from library/hello-world
4f55086f7dd0: Pull complete 
Digest: sha256:c3cbe1cc1aa588a64951ac6286e0df7b27fe2e6324b1001c619bb358770c0178
Status: Downloaded newer image for hello-world:latest

Hello from Docker!
This message shows that your installation appears to be working correctly.
```

**ubuntu 컨테이너 실행 및 진입**
```bash
docker run -it ubuntu
```
```
ieunbin@eunbin-ui-MacBookAir ~ % docker run -it ubuntu
root@c30ca9fa3f33:/#
```

**ubuntu 내부 명령 수행**
```bash
ls
echo "hello"
```
```
root@c30ca9fa3f33:/# ls
bin   dev  home  lib64  mnt  proc  run   srv  tmp  var
boot  etc  lib   media  opt  root  sbin  sys  usr
root@c30ca9fa3f33:/# echo "hello"
hello
```

**컨테이너 종료/유지(attach vs exec)차이**
| 구분 | 명령어 | 컨테이너 상태 | 특징 |
| :--- | :--- | :--- | :--- |
| **종료** | `exit` | **정지** | 컨테이너 내부 쉘을 종료하며 컨테이너도 함께 정지됨 |
| **유지** | `Ctrl + P, Q` | **실행 중** | 컨테이너는 살려두고 터미널만 빠져나옴 (Detach) |
| **진입 (직접)** | `attach` | **실행 중** | 실행 중인 컨테이너의 메인 프로세스에 다시 연결함 |
| **진입 (도구)** | `exec` | **실행 중** | 실행 중인 컨테이너에 별도의 프로세스를 추가로 실행함 |

attach 후 exit 하면 -> 컨테이너 종료<br>
exec 후 exit 하면 -> 컨테이너 유지

---

## 6. 커스텀 이미지 제작
ieunbin@eunbin-ui-MacBookAir my-nginx % open -e Dockerfile
<img width="418" height="172" alt="image" src="https://github.com/user-attachments/assets/5e1a6ad4-05d5-4910-aadd-65ee90636353" />



**선택한 베이스 이미지**
```
uduntu:22.04
```

**커스텀 포인트 및 목적**
```
결과값 입력
```

**Dockerfile**
```dockerfile
결과값 입력
```

**빌드 명령**
```bash
docker build -t 이미지명 .
```
```
결과값 입력
```

**실행 명령**
```bash
docker run -d -p 8080:80 이미지명
```
```
결과값 입력
```

---

## 7. 포트 매핑 및 접속 증거

**포트 매핑 확인**
```bash
docker ps
```
```
결과값 입력
```

**curl 접속 확인**
```bash
curl http://localhost:8080
```
```
결과값 입력
```

**브라우저 접속 화면**
```
스크린샷 첨부
```

---

## 8. Docker 볼륨 영속성 검증

**볼륨 생성**
```bash
docker volume create 볼륨명
```
```
결과값 입력
```

**볼륨 연결 후 컨테이너 실행**
```bash
docker run -v 볼륨명:/경로 이미지명
```
```
결과값 입력
```

**컨테이너 삭제 전 데이터 확인**
```bash
docker exec 컨테이너명 cat /경로/파일명
```
```
결과값 입력
```

**컨테이너 삭제**
```bash
docker rm 컨테이너명
```
```
결과값 입력
```

**컨테이너 삭제 후 데이터 확인**
```bash
docker run -v 볼륨명:/경로 이미지명 cat /경로/파일명
```
```
결과값 입력
```

---

## 9. Git 설정 및 GitHub 연동

**Git 사용자 정보 설정**
```bash
$ git config --global user.name "이름"
$ git config --global user.email "이메일"
```
```
결과값 입력
```

**기본 브랜치 설정**
```bash
git config --global init.defaultBranch main
```
```
결과값 입력
```

**설정 확인**
```bash
git config --list
```
```
결과값 입력
```

**GitHub 저장소 연동 확인**
```bash
git remote -v
```
```
결과값 입력
```

**연동 증거**
```
스크린샷 첨부
```

## 5. 트러블슈팅 2건 이상(문제 → 원인 가설 → 확인 → 해결/대안)
