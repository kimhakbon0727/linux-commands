# 🖥️ Linux `hostname` 명령어 완벽 정리

> 리눅스 시스템에서 호스트명을 조회하고 설정하는 `hostname` 명령어 가이드

---

## 📋 목차

- [개요](#-개요)
- [기본 문법](#-기본-문법)
- [주요 옵션](#-주요-옵션)
- [사용 예시](#-사용-예시)
- [호스트명 영구 변경](#-호스트명-영구-변경)
- [관련 명령어](#-관련-명령어)
- [호스트명 명명 규칙](#-호스트명-명명-규칙)

---

## 🔍 개요

`hostname` 명령어는 현재 시스템의 **호스트명(hostname)** 을 조회하거나 임시로 변경하는 데 사용됩니다.  
네트워크 환경에서 시스템을 식별하는 이름으로, 서버 관리 및 네트워크 설정 시 자주 활용됩니다.

| 항목 | 내용 |
|------|------|
| 명령어 | `hostname` |
| 위치 | `/bin/hostname` |
| 관련 파일 | `/etc/hostname`, `/etc/hosts` |

---

## 📌 기본 문법

```bash
hostname [옵션] [새로운_호스트명]
```

---

## ⚙️ 주요 옵션

| 옵션 | 긴 옵션 | 설명 |
|------|---------|------|
| (없음) | | 현재 호스트명 출력 |
| `-f` | `--fqdn` | FQDN(완전한 도메인 이름) 출력 |
| `-s` | `--short` | 짧은 호스트명 출력 (도메인 제외) |
| `-d` | `--domain` | DNS 도메인명 출력 |
| `-i` | `--ip-address` | 호스트에 연결된 IP 주소 출력 |
| `-I` | `--all-ip-addresses` | 모든 네트워크 인터페이스 IP 출력 |
| `-A` | `--all-fqdns` | 모든 FQDN 출력 |
| `-b` | `--boot` | 호스트명이 없을 경우 기본값 설정 |
| `-v` | `--verbose` | 상세 출력 모드 |
| `-V` | `--version` | 버전 정보 출력 |

---

## 💡 사용 예시

### 1️⃣ 현재 호스트명 확인

```bash
$ hostname
myserver
```

---

### 2️⃣ FQDN(완전한 도메인 이름) 확인

```bash
$ hostname -f
myserver.example.com
```

---

### 3️⃣ 짧은 호스트명 확인

```bash
$ hostname -s
myserver
```

---

### 4️⃣ 도메인명 확인

```bash
$ hostname -d
example.com
```

---

### 5️⃣ IP 주소 확인

```bash
# 기본 IP 주소
$ hostname -i
192.168.1.100

# 모든 인터페이스 IP 주소
$ hostname -I
192.168.1.100 10.0.0.1
```

---

### 6️⃣ 호스트명 임시 변경 (재부팅 시 초기화)

```bash
# root 권한 필요
$ sudo hostname newserver
$ hostname
newserver
```

> ⚠️ **주의:** 이 방법은 **임시 변경**으로 재부팅하면 원래 호스트명으로 돌아갑니다.

---

## 💾 호스트명 영구 변경

### 방법 1: `hostnamectl` 사용 (권장, systemd 기반)

```bash
$ sudo hostnamectl set-hostname newserver

# 변경 확인
$ hostnamectl
   Static hostname: newserver
         Icon name: computer-vm
           Chassis: vm
        Machine ID: abc123...
           Boot ID: xyz789...
  Operating System: Ubuntu 22.04 LTS
            Kernel: Linux 5.15.0
      Architecture: x86-64
```

---

### 방법 2: `/etc/hostname` 파일 직접 수정

```bash
# 파일 내용 확인
$ cat /etc/hostname
myserver

# 파일 수정
$ sudo nano /etc/hostname
# 또는
$ echo "newserver" | sudo tee /etc/hostname
```

---

### 방법 3: `/etc/hosts` 파일도 함께 수정

```bash
$ sudo nano /etc/hosts
```

```
127.0.0.1   localhost
127.0.1.1   newserver        # ← 여기도 변경
::1         localhost ip6-localhost ip6-loopback
```

> 💬 **권장:** `hostnamectl`을 사용하면 `/etc/hostname`과 관련 설정을 자동으로 처리해 줍니다.

---

## 🔗 관련 명령어

| 명령어 | 설명 |
|--------|------|
| `hostnamectl` | systemd 기반 호스트명 관리 (영구 변경 권장) |
| `uname -n` | 커널 기준 호스트명 출력 |
| `cat /etc/hostname` | 호스트명 설정 파일 직접 확인 |
| `nmcli general hostname` | NetworkManager를 통한 호스트명 확인 |
| `nslookup $(hostname)` | 호스트명의 DNS 조회 |
| `ping $(hostname)` | 호스트명으로 자기 자신에게 핑 테스트 |

---

## 📏 호스트명 명명 규칙

리눅스 호스트명 설정 시 아래 규칙을 따르는 것을 권장합니다.

- ✅ 영문자 (a-z, A-Z), 숫자 (0-9), 하이픈 (`-`) 사용 가능
- ✅ 최대 **63자** 이하 (FQDN은 253자 이하)
- ❌ 언더스코어 (`_`) 사용 불가 (일부 환경에서 문제 발생)
- ❌ 숫자로 시작하거나 하이픈으로 시작/끝낼 수 없음
- ❌ 공백 포함 불가

```
✅ 올바른 예: web-server01, db-primary, myserver
❌ 잘못된 예: web_server, 1server, -myserver, my server
```

---

## 📝 요약

```
현재 호스트명 확인          →  hostname
FQDN 확인                  →  hostname -f
IP 주소 확인               →  hostname -I
임시 변경 (재부팅 시 초기화) →  sudo hostname [새이름]
영구 변경 (권장)            →  sudo hostnamectl set-hostname [새이름]
```

---

> 📌 **참고:** 호스트명 변경 후 일부 서비스는 재시작이 필요할 수 있습니다.  
> 특히 SSH, 웹 서버 등의 서비스는 `/etc/hosts` 설정도 함께 확인하세요.
