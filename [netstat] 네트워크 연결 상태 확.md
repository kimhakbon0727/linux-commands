# 🌐 Linux `netstat` 명령어 완벽 정리

> 네트워크 연결, 라우팅 테이블, 인터페이스 통계를 확인하는 `netstat` 명령어 가이드

---

## 📋 목차

- [개요](#-개요)
- [설치 방법](#-설치-방법)
- [기본 문법](#-기본-문법)
- [주요 옵션](#-주요-옵션)
- [사용 예시](#-사용-예시)
- [소켓 상태(State) 종류](#-소켓-상태state-종류)
- [출력 필드 설명](#-출력-필드-설명)
- [ss 와의 비교](#-ss-와의-비교)
- [자주 쓰는 패턴](#-자주-쓰는-패턴)

---

## 🔍 개요

`netstat`은 **net**work **stat**istics의 약자로, 네트워크 연결 상태, 라우팅 테이블,  
인터페이스 통계 등을 확인하는 명령어입니다.

> ⚠️ **참고:** `netstat`은 `net-tools` 패키지에 포함되어 있으며,  
> 최신 리눅스 배포판에서는 **기본 설치되지 않습니다.**  
> 현재는 `ss` 명령어로 대체가 권장되지만, 여전히 널리 사용됩니다.

| 항목 | 내용 |
|------|------|
| 명령어 | `netstat` |
| 패키지 | `net-tools` (별도 설치 필요) |
| 대체 명령어 | `ss` (현재 권장) |
| 주 용도 | 포트 확인, 연결 상태 조회, 라우팅 테이블 확인 |

---

## 📦 설치 방법

### Ubuntu / Debian 계열

```bash
$ sudo apt update
$ sudo apt install net-tools

# 설치 확인
$ netstat --version
net-tools 2.10
```

### CentOS / RHEL / Rocky Linux 계열

```bash
$ sudo yum install net-tools
# 또는 (CentOS 8 이상)
$ sudo dnf install net-tools
```

### Amazon Linux 계열

```bash
$ sudo yum install net-tools
```

---

## 📌 기본 문법

```bash
netstat [옵션]
```

---

## ⚙️ 주요 옵션

### 소켓 종류 선택

| 옵션 | 설명 |
|------|------|
| `-t` | TCP 소켓만 표시 |
| `-u` | UDP 소켓만 표시 |
| `-x` | Unix 도메인 소켓만 표시 |
| `-l` | **LISTEN 상태** 소켓만 표시 (열린 포트) |
| `-a` | 모든 상태의 소켓 표시 |

### 출력 형식

| 옵션 | 설명 |
|------|------|
| `-n` | 서비스명 대신 **포트 번호** 그대로 표시 |
| `-p` | 소켓을 사용하는 **프로세스명 / PID** 표시 |
| `-e` | 확장 정보 표시 (User, Inode 포함) |
| `-v` | 상세(verbose) 출력 |
| `-c` | 1초마다 결과를 반복 출력 |

### 네트워크 정보

| 옵션 | 설명 |
|------|------|
| `-r` | 라우팅 테이블 출력 |
| `-i` | 네트워크 인터페이스 통계 출력 |
| `-s` | 프로토콜별 통계 요약 출력 |
| `-g` | 멀티캐스트 그룹 정보 출력 |

---

## 💡 사용 예시

### 1️⃣ LISTEN 중인 포트 확인 (열린 포트)

```bash
$ sudo netstat -tlnp
Active Internet connections (only servers)
Proto  Recv-Q  Send-Q  Local Address    Foreign Address  State   PID/Program name
tcp    0       0       0.0.0.0:22       0.0.0.0:*        LISTEN  1023/sshd
tcp    0       0       0.0.0.0:80       0.0.0.0:*        LISTEN  2048/nginx
tcp    0       0       127.0.0.1:3306   0.0.0.0:*        LISTEN  3091/mysqld
```

---

### 2️⃣ 모든 TCP 연결 확인

```bash
$ netstat -tan
Active Internet connections (servers and established)
Proto  Recv-Q  Send-Q  Local Address       Foreign Address     State
tcp    0       0       0.0.0.0:22          0.0.0.0:*           LISTEN
tcp    0       0       192.168.1.10:22     192.168.1.1:52345   ESTABLISHED
tcp    0       0       192.168.1.10:443    10.0.0.5:38921      ESTABLISHED
```

---

### 3️⃣ TCP + UDP 동시 확인

```bash
$ sudo netstat -tuln
Active Internet connections (only servers)
Proto  Recv-Q  Send-Q  Local Address    Foreign Address  State
tcp    0       0       0.0.0.0:22       0.0.0.0:*        LISTEN
tcp    0       0       0.0.0.0:80       0.0.0.0:*        LISTEN
udp    0       0       0.0.0.0:68       0.0.0.0:*
udp    0       0       0.0.0.0:123      0.0.0.0:*
```

---

### 4️⃣ 라우팅 테이블 확인 (`-r`)

```bash
$ netstat -r
Kernel IP routing table
Destination   Gateway       Genmask         Flags  MSS  Window  irtt  Iface
0.0.0.0       192.168.1.1   0.0.0.0         UG     0    0       0     eth0
192.168.1.0   0.0.0.0       255.255.255.0   U      0    0       0     eth0
```

---

### 5️⃣ 네트워크 인터페이스 통계 (`-i`)

```bash
$ netstat -i
Kernel Interface table
Iface   MTU    RX-OK  RX-ERR  RX-DRP  RX-OVR  TX-OK  TX-ERR  TX-DRP  TX-OVR  Flg
eth0    1500   52341  0       0       0       41230  0       0       0       BMRU
lo      65536  1024   0       0       0       1024   0       0       0       LRU
```

---

### 6️⃣ 프로토콜별 통계 요약 (`-s`)

```bash
$ netstat -s
Ip:
    123456 total packets received
    0 forwarded
    0 incoming packets discarded
Tcp:
    10234 active connection openings
    8901 passive connection openings
    12 failed connection attempts
    ...
```

---

### 7️⃣ 1초마다 반복 출력 (`-c`)

```bash
# 1초마다 갱신하며 LISTEN 포트 모니터링
$ netstat -tlnc
```

---

## 🔄 소켓 상태(State) 종류

| 상태 | 설명 |
|------|------|
| `LISTEN` | 연결 대기 중 (포트가 열린 상태) |
| `ESTABLISHED` | 현재 연결된 상태 |
| `TIME_WAIT` | 연결 종료 후 대기 중 |
| `CLOSE_WAIT` | 상대방이 연결 종료 후 로컬 종료 대기 |
| `SYN_SENT` | 연결 요청을 보낸 상태 |
| `SYN_RECV` | 연결 요청을 받은 상태 |
| `FIN_WAIT1` | 연결 종료 요청 보낸 상태 |
| `FIN_WAIT2` | 종료 확인 후 대기 중 |
| `CLOSING` | 양쪽 동시 종료 중 |
| `CLOSED` | 연결 완전히 종료 |

---

## 📊 출력 필드 설명

```bash
Proto  Recv-Q  Send-Q  Local Address       Foreign Address     State   PID/Program
tcp    0       0       192.168.1.10:22     10.0.0.1:52345      ESTAB   1023/sshd
  ↑      ↑       ↑            ↑                  ↑               ↑         ↑
프로토콜 수신대기 송신대기  로컬 IP:포트       원격 IP:포트      상태    PID/프로세스명
```

| 필드 | 설명 |
|------|------|
| `Proto` | 프로토콜 (tcp, udp, unix) |
| `Recv-Q` | 수신 버퍼에 쌓인 데이터 크기 (바이트) |
| `Send-Q` | 송신 버퍼에 쌓인 데이터 크기 (바이트) |
| `Local Address` | 로컬 IP 주소와 포트 |
| `Foreign Address` | 원격 IP 주소와 포트 |
| `State` | 연결 상태 |
| `PID/Program name` | 프로세스 ID / 프로그램 이름 (`-p` 옵션 시) |

---

## 🆚 ss 와의 비교

| 항목 | `netstat` | `ss` |
|------|-----------|------|
| 기본 설치 | ❌ 별도 설치 필요 (`net-tools`) | ✅ 기본 설치 (`iproute2`) |
| 속도 | 🐢 느림 | ✅ 빠름 |
| 정보량 | 보통 | ✅ 더 상세 |
| 필터 기능 | 제한적 | ✅ 강력한 필터 지원 |
| 유지보수 | ⚠️ Deprecated | ✅ 현재 활발히 유지 |
| 친숙함 | ✅ 오래된 레퍼런스 많음 | 익숙해지는 중 |

### 명령어 대응표

| 목적 | `netstat` | `ss` |
|------|-----------|------|
| LISTEN 포트 확인 | `netstat -tlnp` | `ss -tlnp` |
| 모든 TCP 연결 | `netstat -tan` | `ss -tan` |
| UDP 소켓 확인 | `netstat -uln` | `ss -uln` |
| 통계 요약 | `netstat -s` | `ss -s` |
| 라우팅 테이블 | `netstat -r` | `ip route` |
| 인터페이스 통계 | `netstat -i` | `ip -s link` |

---

## 🛠️ 자주 쓰는 패턴

### 특정 포트가 열려 있는지 확인

```bash
$ sudo netstat -tlnp | grep 8080
tcp  0  0  0.0.0.0:8080  0.0.0.0:*  LISTEN  4021/node
```

### 현재 SSH 접속자 확인

```bash
$ netstat -tn | grep :22 | grep ESTABLISHED
```

### 특정 프로세스의 포트 확인

```bash
$ sudo netstat -tlnp | grep nginx
tcp  0  0  0.0.0.0:80   0.0.0.0:*  LISTEN  2048/nginx
tcp  0  0  0.0.0.0:443  0.0.0.0:*  LISTEN  2048/nginx
```

### ESTABLISHED 연결 수 확인

```bash
$ netstat -tn | grep ESTABLISHED | wc -l
```

### TIME_WAIT 연결 수 확인 (서버 부하 진단)

```bash
$ netstat -tn | grep TIME_WAIT | wc -l
```

### 상태별 연결 수 통계

```bash
$ netstat -tan | awk '{print $6}' | sort | uniq -c | sort -rn
     10 ESTABLISHED
      5 LISTEN
      3 TIME_WAIT
      1 CLOSE_WAIT
```

---

## 📝 요약

```
LISTEN 포트 확인          →  sudo netstat -tlnp
모든 TCP 연결 확인        →  netstat -tan
TCP + UDP 동시 확인       →  sudo netstat -tuln
라우팅 테이블 확인        →  netstat -r
인터페이스 통계 확인      →  netstat -i
프로토콜별 통계 요약      →  netstat -s
특정 포트 필터            →  sudo netstat -tlnp | grep 포트번호
상태별 연결 수 통계       →  netstat -tan | awk '{print $6}' | sort | uniq -c
```

---

> 📌 **참고:** 최신 환경에서는 `ss` 사용이 권장되지만, 오래된 서버나 레퍼런스 문서에서는  
> `netstat`이 여전히 자주 등장합니다. 두 명령어 모두 익혀두면 좋습니다.
