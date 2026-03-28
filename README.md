nslookup - DNS 조회 명령어

도메인 이름과 IP 주소를 확인하고, 다양한 DNS 레코드를 조회하는 명령어입니다.
네트워크 관리자와 개발자가 DNS 문제를 진단할 때 가장 기본적으로 사용하는 도구예요.


목차
1. 개요
2. 기본 사용법
3. 레코드 타입 조회
4. 인터랙티브 모드
5. 실용 예제
6. 대체 도구
7. 설치 방법


1. 개요

nslookup (Name Server Lookup)은 DNS 서버에 질의하여 도메인 정보를 가져오는 명령줄 도구입니다.

- 정방향 조회: 도메인 → IP 주소
- 역방향 조회: IP 주소 → 도메인 이름
- MX, NS, TXT, SOA 등 다양한 DNS 레코드 확인 가능
- Windows, Linux, macOS 모두 지원


2. 기본 사용법

# 기본 조회 (A 레코드)
nslookup google.com

# 특정 DNS 서버 지정
nslookup google.com 8.8.8.8      # Google DNS
nslookup naver.com 1.1.1.1        # Cloudflare DNS

# 역방향 조회 (IP → 도메인)
nslookup 8.8.8.8


3. 레코드 타입 조회

nslookup -type=레코드타입 도메인

레코드 타입 표:
- A      : IPv4 주소                   → nslookup -type=A google.com
- AAAA   : IPv6 주소                   → nslookup -type=AAAA google.com
- MX     : 메일 서버                   → nslookup -type=MX naver.com
- NS     : 네임 서버                   → nslookup -type=NS google.com
- TXT    : 텍스트 기록 (SPF, DMARC)    → nslookup -type=TXT google.com
- CNAME  : 별칭 레코드                 → nslookup -type=CNAME www.naver.com
- SOA    : 권한 시작 레코드            → nslookup -type=SOA naver.com

디버그 모드:
nslookup -debug google.com


4. 인터랙티브 모드

nslookup

> server 8.8.8.8          # DNS 서버 변경
> set type=mx             # 조회할 레코드 타입 설정
> naver.com               # 조회 실행
> exit                    # 종료


5. 실용 예제

# 메일 서버 확인
nslookup -type=mx gmail.com

# 네임서버 확인
nslookup -type=ns daum.net

# SPF 레코드 확인
nslookup -type=txt _spf.google.com

# TXT 레코드 전체 확인
nslookup -type=txt google.com


6. 대체 도구

도구       특징                              추천 상황
nslookup   Windows에서 편리, 인터랙티브 강점   빠른 확인, 초보자
dig        출력이 가장 자세하고 구조적         전문가, 상세 분석
host       가장 간단하고 깔끔함                빠른 한 줄 조회

dig 예시:
dig google.com MX
dig +short naver.com NS


7. 설치 방법

- Windows: 명령 프롬프트에 기본 설치되어 있음

- Ubuntu / Debian:
  sudo apt install dnsutils -y

- Fedora / RHEL / CentOS:
  sudo dnf install bind-utils -y

- macOS: 대부분 기본 설치됨 (없으면 brew install bind)
