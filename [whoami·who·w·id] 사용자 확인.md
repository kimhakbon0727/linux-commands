# 👤 Linux 사용자 확인 명령어 정리

> `whoami`, `who`, `w`, `id`, `users`, `last` 등  
> 현재 사용자 및 로그인 정보를 확인하는 명령어 모음 가이드

---

## 📋 목차

- [명령어 한눈에 보기](#-명령어-한눈에-보기)
- [whoami](#-whoami)
- [who](#-who)
- [w](#-w)
- [id](#-id)
- [users](#-users)
- [last](#-last)
- [lastlog](#-lastlog)
- [자주 쓰는 패턴](#-자주-쓰는-패턴)

---

## 📊 명령어 한눈에 보기

| 명령어 | 주요 용도 |
|--------|----------|
| `whoami` | 현재 내가 누구인지 (사용자명 출력) |
| `who` | 현재 시스템에 로그인한 사용자 목록 |
| `w` | 로그인 사용자 + 현재 실행 중인 작업까지 |
| `id` | 현재 사용자의 UID, GID, 소속 그룹 확인 |
| `users` | 로그인 중인 사용자명만 간단히 출력 |
| `last` | 과거 로그인 기록 확인 |
| `lastlog` | 각 계정의 마지막 로그인 시간 확인 |

---

## 🙋 whoami

현재 **내가 어떤 사용자로 실행 중인지** 사용자명만 출력합니다.  
`sudo`로 권한 전환 후 현재 사용자를 빠르게 확인할 때 유용합니다.

### 기본 사용법

```bash
$ whoami
alice
```

### sudo 전환 후 확인

```bash
$ sudo whoami
root

$ sudo -u bob whoami
bob
```

### 스크립트에서 활용

```bash
#!/bin/bash
if [ "$(whoami)" != "root" ]; then
    echo "root 권한이 필요합니다."
    exit 1
fi
echo "root로 실행 중입니다."
```

---

## 👥 who

현재 시스템에 **로그인한 사용자 목록**을 출력합니다.

### 기본 옵션

| 옵션 | 설명 |
|------|------|
| (없음) | 로그인 사용자 목록 출력 |
| `-a` | 모든 정보 출력 |
| `-b` | 마지막 시스템 부팅 시간 출력 |
| `-r` | 현재 런레벨 출력 |
| `-q` | 사용자 수만 간단히 출력 |
| `-H` | 헤더(컬럼명) 포함하여 출력 |

### 사용 예시

```bash
# 기본 출력
$ who
alice    pts/0   2026-04-22 09:00 (192.168.1.1)
bob      pts/1   2026-04-22 10:30 (192.168.1.2)

# 헤더 포함
$ who -H
NAME     LINE     TIME             COMMENT
alice    pts/0    2026-04-22 09:00 (192.168.1.1)
bob      pts/1    2026-04-22 10:30 (192.168.1.2)

# 현재 나 자신만 확인
$ who am i
alice    pts/0   2026-04-22 09:00 (192.168.1.1)

# 마지막 부팅 시간 확인
$ who -b
         system boot  2026-04-20 08:00

# 접속자 수만 출력
$ who -q
alice bob
# users=2
```

### 출력 필드 설명

```
alice    pts/0   2026-04-22 09:00 (192.168.1.1)
  ↑        ↑           ↑                ↑
사용자명  터미널    로그인 시간       접속 IP
```

| 필드 | 설명 |
|------|------|
| NAME | 로그인한 사용자명 |
| LINE | 사용 중인 터미널 (pts/0 = SSH, tty1 = 로컬) |
| TIME | 로그인 시간 |
| COMMENT | 접속 IP 또는 터미널 정보 |

---

## 📋 w

`who`보다 더 상세하게, 로그인 사용자의 **현재 실행 중인 작업**까지 함께 출력합니다.

### 기본 옵션

| 옵션 | 설명 |
|------|------|
| (없음) | 전체 정보 출력 |
| `-h` | 헤더 출력 안 함 |
| `-s` | 간략하게 출력 (JCPU, PCPU 제외) |
| `-f` | FROM(접속 IP) 컬럼 표시/숨김 토글 |
| `-u` | 사용자명 대신 UID 출력 |

### 사용 예시

```bash
$ w
 10:45:00 up 2 days,  2:45,  2 users,  load average: 0.10, 0.15, 0.12
USER     TTY      FROM           LOGIN@   IDLE  JCPU   PCPU  WHAT
alice    pts/0    192.168.1.1    09:00    0.00s 0.05s  0.01s  w
bob      pts/1    192.168.1.2    10:30    5:20  0.10s  0.02s  vim app.py
```

### 출력 필드 설명

| 필드 | 설명 |
|------|------|
| `USER` | 사용자명 |
| `TTY` | 사용 중인 터미널 |
| `FROM` | 접속 IP |
| `LOGIN@` | 로그인 시간 |
| `IDLE` | 마지막 입력 후 경과 시간 |
| `JCPU` | 해당 터미널에서 사용한 총 CPU 시간 |
| `PCPU` | 현재 프로세스가 사용 중인 CPU 시간 |
| `WHAT` | 현재 실행 중인 명령어 |

> 💡 첫 줄의 **load average**는 1분 / 5분 / 15분 평균 시스템 부하를 나타냅니다.

---

## 🔑 id

현재 사용자의 **UID, GID, 소속 그룹** 정보를 출력합니다.  
권한 문제 디버깅 시 매우 유용합니다.

### 기본 옵션

| 옵션 | 설명 |
|------|------|
| (없음) | UID, GID, 그룹 전체 출력 |
| `-u` | UID만 출력 |
| `-g` | 기본 GID만 출력 |
| `-G` | 소속된 모든 GID 출력 |
| `-n` | 숫자 대신 이름으로 출력 (`-u`, `-g`, `-G` 와 함께 사용) |

### 사용 예시

```bash
# 기본 출력
$ id
uid=1001(alice) gid=1001(alice) groups=1001(alice),27(sudo),1002(developers)

# UID만 출력
$ id -u
1001

# UID를 이름으로 출력
$ id -un
alice

# 소속 그룹 이름 출력
$ id -Gn
alice sudo developers

# 다른 사용자 정보 확인
$ id bob
uid=1002(bob) gid=1002(bob) groups=1002(bob),1002(developers)
```

### 출력 구조

```
uid=1001(alice)  gid=1001(alice)  groups=1001(alice),27(sudo),1002(developers)
    ↑                 ↑                         ↑
사용자 ID(이름)   기본 그룹 ID(이름)         소속된 모든 그룹
```

---

## 👫 users

현재 로그인 중인 **사용자명만** 한 줄로 간단하게 출력합니다.

```bash
$ users
alice alice bob

# 중복 제거하여 유일한 사용자만 출력
$ users | tr ' ' '\n' | sort -u
alice
bob
```

> 💡 같은 사용자가 여러 터미널로 접속하면 중복으로 표시됩니다.

---

## 📜 last

시스템의 **과거 로그인/로그아웃 기록**을 출력합니다.  
(`/var/log/wtmp` 파일 기반)

### 기본 옵션

| 옵션 | 설명 |
|------|------|
| (없음) | 전체 로그인 기록 출력 |
| `-n [숫자]` | 최근 N개만 출력 |
| `-a` | 호스트명을 마지막 컬럼에 출력 |
| `-d` | IP를 호스트명으로 변환 |
| `-i` | 호스트명을 IP로 출력 |
| `-x` | 시스템 종료/재부팅 기록 포함 |
| `-F` | 전체 날짜/시간 형식으로 출력 |

### 사용 예시

```bash
# 전체 로그인 기록
$ last
alice    pts/0    192.168.1.1    Wed Apr 22 09:00   still logged in
bob      pts/1    192.168.1.2    Wed Apr 22 10:30   still logged in
alice    pts/0    192.168.1.1    Tue Apr 21 08:00 - 18:00  (10:00)
reboot   system boot              Mon Apr 20 08:00   still running

# 최근 5개만 출력
$ last -n 5

# 재부팅 기록 포함
$ last -x | grep -E "reboot|shutdown"
reboot   system boot   Mon Apr 20 08:00   still running
shutdown system down   Sun Apr 19 23:00 - Mon Apr 20 08:00  (09:00)

# 특정 사용자의 기록만 확인
$ last alice
alice    pts/0    192.168.1.1    Wed Apr 22 09:00   still logged in
alice    pts/0    192.168.1.1    Tue Apr 21 08:00 - 18:00  (10:00)
```

---

## 🕐 lastlog

시스템의 **각 계정별 마지막 로그인 시간**을 한눈에 출력합니다.  
(`/var/log/lastlog` 파일 기반)

### 기본 옵션

| 옵션 | 설명 |
|------|------|
| (없음) | 모든 계정의 마지막 로그인 출력 |
| `-u [사용자]` | 특정 사용자만 출력 |
| `-b [일수]` | N일 이전에 로그인한 계정만 출력 |
| `-t [일수]` | 최근 N일 이내에 로그인한 계정만 출력 |

### 사용 예시

```bash
# 전체 계정 마지막 로그인 확인
$ lastlog
Username    Port    From           Latest
root        pts/0   192.168.1.1    Wed Apr 22 08:00:00 +0900 2026
alice       pts/0   192.168.1.1    Wed Apr 22 09:00:00 +0900 2026
bob         pts/1   192.168.1.2    Wed Apr 22 10:30:00 +0900 2026
daemon              **Never logged in**
nobody              **Never logged in**

# 특정 사용자만 확인
$ lastlog -u alice
Username    Port    From           Latest
alice       pts/0   192.168.1.1    Wed Apr 22 09:00:00 +0900 2026

# 한 번도 로그인 안 한 계정 확인 (보안 점검)
$ lastlog | grep "Never logged in"
```

---

## 🛠️ 자주 쓰는 패턴

### 현재 접속자 수 확인

```bash
$ who | wc -l
2
```

### root로 실행 중인지 스크립트에서 확인

```bash
if [ "$(whoami)" != "root" ]; then
    echo "이 스크립트는 root 권한이 필요합니다."
    exit 1
fi
```

### sudo 권한 여부 확인

```bash
$ id -Gn | grep sudo
alice sudo developers    # sudo 포함되어 있으면 권한 있음
```

### 특정 사용자가 로그인 중인지 확인

```bash
$ who | grep bob
bob  pts/1  2026-04-22 10:30 (192.168.1.2)
```

### 마지막 재부팅 시간 확인

```bash
$ last reboot | head -1
reboot   system boot   Mon Apr 20 08:00   still running
```

---

## 📝 요약

```
내 사용자명 확인                →  whoami
로그인 사용자 목록              →  who
로그인 사용자 + 실행 작업 확인  →  w
UID / GID / 그룹 확인          →  id
로그인 사용자명만 간단히        →  users
과거 로그인 기록                →  last
계정별 마지막 로그인 시간       →  lastlog
```

---

> 📌 **참고:** `who`와 `w`는 현재 상태를, `last`와 `lastlog`는 과거 기록을 확인할 때 사용합니다.  
> 서버 보안 점검 시 `lastlog | grep "Never logged in"` 으로 미사용 계정을 주기적으로 확인하는 것을 권장합니다.
