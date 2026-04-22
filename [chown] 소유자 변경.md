# 👤 Linux `chown` 명령어 완벽 정리

> 파일 및 폴더의 **소유자(owner)** 와 **소유 그룹(group)** 을 변경하는 `chown` 명령어 가이드

---

## 📋 목차

- [개요](#-개요)
- [소유자와 권한의 관계](#-소유자와-권한의-관계)
- [기본 문법](#-기본-문법)
- [주요 옵션](#-주요-옵션)
- [사용 예시](#-사용-예시)
- [chmod 와의 차이](#-chmod-와의-차이)
- [자주 쓰는 패턴](#-자주-쓰는-패턴)
- [주의사항](#-주의사항)

---

## 🔍 개요

`chown`은 **ch**ange **own**er의 약자로, 파일이나 폴더의 소유자 및 소유 그룹을 변경하는 명령어입니다.  
리눅스는 모든 파일에 **소유자(user)** 와 **소유 그룹(group)** 이 지정되어 있으며,  
이를 기반으로 접근 권한을 제어합니다.

| 항목 | 내용 |
|------|------|
| 명령어 | `chown` |
| 위치 | `/bin/chown` |
| 권한 필요 | `root` 또는 `sudo` |
| 관련 명령어 | `chmod`, `chgrp`, `ls -l` |

---

## 🔐 소유자와 권한의 관계

```bash
$ ls -l myfile.txt
-rw-r--r-- 1 alice developers 1024 Apr 22 10:00 myfile.txt
             ↑     ↑
           소유자  소유그룹
```

| 구분 | 설명 |
|------|------|
| 소유자 (user) | 파일을 만든 사용자, 가장 넓은 권한을 가짐 |
| 소유 그룹 (group) | 그룹에 속한 모든 사용자에게 동일한 권한 적용 |
| 기타 (others) | 소유자도 그룹도 아닌 나머지 사용자 |

---

## 📌 기본 문법

```bash
chown [옵션] [소유자][:그룹] 파일명
```

### 소유자/그룹 지정 방식

```bash
chown user          파일       # 소유자만 변경
chown user:group    파일       # 소유자 + 그룹 동시 변경
chown :group        파일       # 그룹만 변경
chown user:         파일       # 소유자 변경 + 그룹을 소유자의 기본 그룹으로 변경
```

---

## ⚙️ 주요 옵션

| 옵션 | 긴 옵션 | 설명 |
|------|---------|------|
| `-R` | `--recursive` | 폴더 및 하위 파일 모두 재귀적으로 변경 |
| `-v` | `--verbose` | 변경된 파일 정보를 상세 출력 |
| `-c` | `--changes` | 실제로 변경된 파일만 출력 |
| `--reference` | | 지정한 파일의 소유자/그룹을 참조하여 동일하게 설정 |
| `-f` | `--silent` | 오류 메시지 출력 안 함 |
| `--no-dereference` | | 심볼릭 링크 자체의 소유자 변경 (링크 대상 파일 변경 안 함) |

---

## 💡 사용 예시

### 1️⃣ 소유자만 변경

```bash
$ sudo chown alice myfile.txt

$ ls -l myfile.txt
-rw-r--r-- 1 alice developers 1024 Apr 22 10:00 myfile.txt
```

---

### 2️⃣ 소유자 + 그룹 동시 변경

```bash
$ sudo chown alice:devteam myfile.txt

$ ls -l myfile.txt
-rw-r--r-- 1 alice devteam 1024 Apr 22 10:00 myfile.txt
```

---

### 3️⃣ 그룹만 변경

```bash
$ sudo chown :devteam myfile.txt

$ ls -l myfile.txt
-rw-r--r-- 1 alice devteam 1024 Apr 22 10:00 myfile.txt
```

---

### 4️⃣ 폴더 전체 재귀 변경 (`-R`)

```bash
# /var/www/html 폴더 및 하위 모든 파일의 소유자를 www-data로 변경
$ sudo chown -R www-data:www-data /var/www/html

# 변경 확인
$ ls -la /var/www/html
drwxr-xr-x 3 www-data www-data 4096 Apr 22 10:00 .
drwxr-xr-x 4 root     root     4096 Apr 22 09:00 ..
-rw-r--r-- 1 www-data www-data 1024 Apr 22 10:00 index.html
```

---

### 5️⃣ 변경 내역 출력 (`-v`, `-c`)

```bash
# 모든 처리 파일 출력
$ sudo chown -Rv alice:alice /home/alice/
changed ownership of '/home/alice/file1.txt' from root:root to alice:alice
changed ownership of '/home/alice/file2.txt' from bob:bob to alice:alice

# 실제 변경된 파일만 출력
$ sudo chown -Rc alice:alice /home/alice/
changed ownership of '/home/alice/file2.txt' from bob:bob to alice:alice
```

---

### 6️⃣ 참조 파일 기준으로 변경 (`--reference`)

```bash
# ref_file.txt 의 소유자/그룹과 동일하게 target.txt 를 설정
$ sudo chown --reference=ref_file.txt target.txt
```

---

### 7️⃣ UID / GID 숫자로 변경

```bash
# 사용자 이름 대신 UID(1001), 그룹 대신 GID(1002) 사용 가능
$ sudo chown 1001:1002 myfile.txt
```

---

## 🆚 chmod 와의 차이

| 구분 | `chown` | `chmod` |
|------|---------|---------|
| 역할 | **소유자/그룹** 변경 | **권한(읽기/쓰기/실행)** 변경 |
| 대상 | 파일의 주인을 바꿈 | 파일의 접근 권한을 바꿈 |
| 예시 | `chown alice file` | `chmod 755 file` |
| 함께 사용 | 소유자 변경 후 권한 설정하는 경우 많음 | |

```bash
# 실무에서 자주 함께 사용하는 패턴
$ sudo chown -R www-data:www-data /var/www/html
$ sudo chmod -R 755 /var/www/html
```

---

## 🛠️ 자주 쓰는 패턴

### 웹 서버 배포 시 (Apache / Nginx)

```bash
$ sudo chown -R www-data:www-data /var/www/html
$ sudo chmod -R 755 /var/www/html
```

### 홈 디렉토리 소유권 복구

```bash
$ sudo chown -R alice:alice /home/alice
```

### 루트 소유 파일을 일반 유저에게 이전

```bash
$ sudo chown alice:alice /tmp/report.csv
```

### 소유자를 root 로 되돌리기

```bash
$ sudo chown root:root /etc/myconfig.conf
```

---

## ⚠️ 주의사항

- 🔒 `chown`은 **반드시 `sudo` 또는 root 권한** 이 필요합니다.
- ⚠️ `-R` 옵션 사용 시 **경로를 반드시 확인** 하세요. 잘못된 경로에 적용하면 시스템 파일 소유권이 변경될 수 있습니다.
- 🚫 `/`, `/etc`, `/bin` 같은 시스템 디렉토리에는 절대 사용하지 마세요.
- 📝 변경 전 `ls -l` 로 현재 소유자/그룹을 먼저 확인하는 습관을 들이세요.

---

## 📝 요약

```
소유자만 변경              →  sudo chown alice 파일
소유자 + 그룹 변경         →  sudo chown alice:devteam 파일
그룹만 변경                →  sudo chown :devteam 파일
폴더 전체 재귀 변경        →  sudo chown -R alice:alice 폴더/
변경 내역 확인하며 변경    →  sudo chown -Rv alice:alice 폴더/
참조 파일 기준 변경        →  sudo chown --reference=ref.txt 파일
```

---

> 📌 **참고:** 소유자 및 그룹 목록은 `cat /etc/passwd`, `cat /etc/group` 으로 확인할 수 있습니다.
