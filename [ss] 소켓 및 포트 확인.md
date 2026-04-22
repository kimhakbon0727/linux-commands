# 🔌 Linux `ss` 명령어 완벽 정리

> 소켓 및 네트워크 연결 상태를 확인하는 `ss` 명령어 가이드  
> (`netstat`의 현대적 대체 명령어)

---

## 📋 목차

- [개요](#-개요)
- [기본 문법](#-기본-문법)
- [주요 옵션](#-주요-옵션)
- [사용 예시](#-사용-예시)
- [소켓 상태(State) 종류](#-소켓-상태state-종류)
- [출력 필드 설명](#-출력-필드-설명)
- [필터링 문법](#-필터링-문법)
- [netstat 과의 비교](#-netstat-과의-비교)
- [자주 쓰는 패턴](#-자주-쓰는-패턴)

---

## 🔍 개요

`ss`는 **S**ocket **S**tatistics의 약자로, 시스템의 **네트워크 소켓 연결 상태**를 조회하는 명령어입니다.  
기존 `netstat`보다 **빠르고 상세한 정보**를 제공하며, 최신 리눅스 배포판에서는 `ss` 사용을 권장합니다.

| 항목 | 내용 |
|------|------|
| 명령어 | `ss` |
| 패키지 | `iproute2` |
| `netstat` 대비 | 더 빠름, 더 많은 정보 제공 |
| 주 용도 | 포트 확인, 연결 상태 조회, 소켓 디버깅 |

---

## 📌 기본 문법

```bash
ss [옵션] [필터]
```

---

## ⚙️ 주요 옵션

### 소켓 종류 선택

| 옵션 | 긴 옵션 | 설명 |
|------|---------|------|
| `-t` | `--tcp` | TCP 소켓만 표시 |
| `-u` | `--udp` | UDP 소켓만 표시 |
| `-x` | `--unix` | Unix 도메인 소켓만 표시 |
| `-l` | `--listening` | **LISTEN 상태** 소켓만 표시 (열린 포트) |
| `-a` | `--all` | 모든 상태의 소켓 표시 |

### 출력 형식

| 옵션 | 긴 옵션 | 설명 |
|------|---------|------|
| `-n` | `--numeric` | 서비스명 대신 **포트 번호** 그대로 표시 |
| `-p` | `--processes` | 소켓을 사용하는 **프로세스명/PID** 표시 |
| `-e` | `--extended` | 확장 소켓 정보 표시 |
| `-s` | `--summary` | 소켓 통계 요약 출력 |
| `-r` | `--resolve` | IP 주소를 호스트명으로 변환 |
| `-4` | `--ipv4` | IPv4만 표시 |
| `-6` | `--ipv6` | IPv6만 표시 |

---

## 💡 사용 예시

### 1️⃣ 모든 TCP 연결 확인

```bash
$ ss -t
State    Recv-Q  Send-Q  Local Address:Port    Peer Address:Port
ESTAB    0       0       192.168.1.10:ssh      192.168.1.1:52345
ESTAB    0       0       192.168.1.10:443      10.0.0.5:38921
```

---

### 2️⃣ LISTEN 중인 포트 확인 (열린 포트)

```bash
$ ss -tln
State    Recv-Q  Send-Q  Local Address:Port  Peer Address:Port
LISTEN   0       128     0.0.0.0:22          0.0.0.0:*
LISTEN   0       511     0.0.0.0:80          0.0.0.0:*
LISTEN   0       511     0.0.0.0:443         0.0.0.0:*
LISTEN   0       128     127.0.0.1:3306      0.0.0.0:*
```

---

### 3️⃣ 프로세스 정보 함께 확인 (`-p`)

```bash
$ sudo ss -tlnp
State    Recv-Q  Send-Q  Local Address:Port  Peer Address:Port  Process
LISTEN   0       128     0.0.0.0:22          0.0.0.0:*          users:(("sshd",pid=1023,fd=3))
LISTEN   0       511     0.0.0.0:80          0.0.0.0:*          users:(("nginx",pid=2048,fd=6))
LISTEN   0       128     127.0.0.1:3306      0.0.0.0:*          users:(("mysqld",pid=3091,fd=21))
```

> 💡 `-p` 옵션은 **sudo** 권한이 있어야 다른 사용자의 프로세스 정보까지 확인 가능합니다.

---

### 4️⃣ UDP 소켓 확인

```bash
$ ss -uln
State    Recv-Q  Send-Q  Local Address:Port  Peer Address:Port
UNCONN   0       0       0.0.0.0:68          0.0.0.0:*
UNCONN   0       0       0.0.0.0:123         0.0.0.0:*
```

---

### 5️⃣ TCP + UDP 동시 확인

```bash
$ ss -tuln
Netid  State    Recv-Q  Send-Q  Local Address:Port  Peer Address:Port
tcp    LISTEN   0       128     0.0.0.0:22          0.0.0.0:*
tcp    LISTEN   0       511     0.0.0.0:80          0.0.0.0:*
udp    UNCONN   0       0       0.0.0.0:68          0.0.0.0:*
```

---

### 6️⃣ 소켓 통계 요약 (`-s`)

```bash
$ ss -s
Total: 312
TCP:   28 (estab 10, closed 5, orphaned 0, timewait 5)

Transport  Total  IP   IPv6
RAW        0      0    0
UDP        8      5    3
TCP        23     14   9
INET       31     19   12
FRAG       0      0    0
```

---

### 7️⃣ IPv4 / IPv6 구분 확인

```bash
# IPv4만
$ ss -tln -4

# IPv6만
$ ss -tln -6
```

---

## 🔄 소켓 상태(State) 종류

| 상태 | 설명 |
|------|------|
| `LISTEN` | 연결 대기 중 (포트가 열린 상태) |
| `ESTABLISHED` | 현재 연결된 상태 |
| `TIME-WAIT` | 연결 종료 후 대기 중 |
| `CLOSE-WAIT` | 상대방이 연결 종료 후 로컬 종료 대기 |
| `SYN-SENT` | 연결 요청을 보낸 상태 |
| `SYN-RECV` | 연결 요청을 받은 상태 |
| `FIN-WAIT-1` | 연결 종료 요청 보낸 상태 |
| `FIN-WAIT-2` | 종료 확인 후 대기 중 |
| `CLOSED` | 연결 완전히 종료 |
| `UNCONN` | UDP 비연결 상태 |

---

## 📊 출력 필드 설명

```bash
State    Recv-Q  Send-Q  Local Address:Port    Peer Address:Port  Process
ESTAB    0       0       192.168.1.10:ssh      10.0.0.1:52345     sshd
  ↑        ↑       ↑            ↑                   ↑               ↑
상태    수신대기  송신대기   로컬 IP:포트        원격 IP:포트      프로세스
```

| 필드 | 설명 |
|------|------|
| `State` | 소켓 연결 상태 |
| `Recv-Q` | 수신 버퍼에 쌓인 데이터 크기 (바이트) |
| `Send-Q` | 송신 버퍼에 쌓인 데이터 크기 (바이트) |
| `Local Address:Port` | 로컬 IP 주소와 포트 |
| `Peer Address:Port` | 원격 IP 주소와 포트 |
| `Process` | 해당 소켓을 사용하는 프로세스 (`-p` 옵션 시) |

> 💡 `Recv-Q` 또는 `Send-Q` 값이 계속 높다면 네트워크 병목이나 연결 문제를 의심해 볼 수 있습니다.

---

## 🔎 필터링 문법

`ss`는 강력한 필터 문법을 지원합니다.

### 특정 포트 필터링

```bash
# 목적지 포트 80 필터
$ ss -tn dst :80

# 출발지 포트 22 필터
$ ss -tn src :22

# 특정 포트 번호로 필터 (dport, sport)
$ ss -tn '( dport = :443 or sport = :443 )'
```

### 특정 IP 필터링

```bash
# 특정 IP와의 연결만 조회
$ ss -tn dst 192.168.1.1

# 특정 대역과의 연결 조회
$ ss -tn dst 10.0.0.0/24
```

### 상태별 필터링

```bash
# ESTABLISHED 상태만
$ ss -t state established

# LISTEN 상태만
$ ss -t state listening

# TIME-WAIT 상태만
$ ss -t state time-wait
```

---

## 🆚 netstat 과의 비교

| 항목 | `ss` | `netstat` |
|------|------|-----------|
| 속도 | ✅ 빠름 | 🐢 느림 |
| 정보량 | ✅ 더 상세 | 보통 |
| 필터 기능 | ✅ 강력한 필터 지원 | 제한적 |
| 기본 설치 | `iproute2` 패키지 | `net-tools` 패키지 |
| 유지보수 | ✅ 현재 활발히 유지 | ⚠️ Deprecated |

### 명령어 대응표

| 목적 | `ss` | `netstat` |
|------|------|-----------|
| LISTEN 포트 확인 | `ss -tlnp` | `netstat -tlnp` |
| 모든 TCP 연결 | `ss -tan` | `netstat -tan` |
| UDP 소켓 확인 | `ss -uln` | `netstat -uln` |
| 통계 요약 | `ss -s` | `netstat -s` |

---

## 🛠️ 자주 쓰는 패턴

### 특정 포트가 열려 있는지 확인

```bash
$ sudo ss -tlnp | grep 8080
LISTEN  0  511  0.0.0.0:8080  0.0.0.0:*  users:(("node",pid=4021,fd=18))
```

### 현재 SSH 접속자 확인

```bash
$ ss -tn state established '( dport = :22 or sport = :22 )'
```

### 특정 프로세스의 포트 확인

```bash
$ sudo ss -tlnp | grep nginx
LISTEN  0  511  0.0.0.0:80   0.0.0.0:*  users:(("nginx",pid=2048,fd=6))
LISTEN  0  511  0.0.0.0:443  0.0.0.0:*  users:(("nginx",pid=2048,fd=7))
```

### ESTABLISHED 연결 수 확인

```bash
$ ss -t state established | wc -l
```

### TIME-WAIT 연결 수 확인 (서버 부하 진단)

```bash
$ ss -t state time-wait | wc -l
```

---

## 📝 요약

```
LISTEN 포트 확인          →  sudo ss -tlnp
모든 TCP 연결 확인        →  ss -tan
UDP 소켓 확인             →  ss -uln
TCP + UDP 동시 확인       →  ss -tuln
프로세스 정보 포함        →  sudo ss -tlnp
소켓 통계 요약            →  ss -s
특정 포트 필터            →  ss -tn dst :포트번호
상태별 필터               →  ss -t state established
```

---

> 📌 **참고:** `ss -tlnp` 는 서버 점검 시 가장 많이 사용하는 조합입니다.  
> `netstat`이 없는 환경에서도 `ss`는 기본 설치되어 있는 경우가 많습니다.
