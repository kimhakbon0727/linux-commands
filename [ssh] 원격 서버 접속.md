# 🔐 ssh (Secure Shell) 명령어 치트시트

> 네트워크를 통해 **원격 서버에 안전하게 접속**하는 리눅스/유닉스 명령어

---

## 📌 기본 문법

```
ssh [옵션] [사용자명@][호스트] [명령어]
```

---

## ⚙️ 주요 옵션

| 옵션 | 설명 |
|------|------|
| `-p` | 포트 지정 (기본값: 22) |
| `-i` | 인증에 사용할 개인키 파일 지정 |
| `-l` | 접속할 사용자명 지정 |
| `-v` | 디버그 정보 출력 (verbose) |
| `-vv` / `-vvv` | 더 상세한 디버그 정보 출력 |
| `-N` | 원격 명령 실행 없이 터널만 유지 |
| `-f` | 백그라운드로 실행 |
| `-L` | 로컬 포트 포워딩 |
| `-R` | 원격 포트 포워딩 |
| `-D` | 동적 포트 포워딩 (SOCKS 프록시) |
| `-X` | X11 포워딩 (GUI 앱 원격 실행) |

---

## 💡 실전 예시

### 기본 접속
```bash
# 기본 접속
ssh user@192.168.0.1

# 포트 지정
ssh -p 2222 user@192.168.0.1

# 개인키로 접속
ssh -i ~/.ssh/my_key.pem user@192.168.0.1

# 접속과 동시에 명령어 실행 후 종료
ssh user@192.168.0.1 "ls -al /var/www"
```

### SSH 키 생성 및 등록
```bash
# RSA 키 생성 (4096비트 권장)
ssh-keygen -t rsa -b 4096 -C "my@email.com"
# ~/.ssh/id_rsa       (개인키)
# ~/.ssh/id_rsa.pub   (공개키)

# Ed25519 키 생성 (더 짧고 안전 — 최신 권장 방식)
ssh-keygen -t ed25519 -C "my@email.com"

# 공개키를 원격 서버에 등록 (이후 비밀번호 없이 접속 가능)
ssh-copy-id user@192.168.0.1

# ssh-copy-id 없을 때 수동 등록
cat ~/.ssh/id_rsa.pub | ssh user@192.168.0.1 "mkdir -p ~/.ssh && cat >> ~/.ssh/authorized_keys"
```

### config 파일로 단축 접속
```bash
# ~/.ssh/config 파일 작성
Host myserver
    HostName 192.168.0.1
    User ubuntu
    Port 2222
    IdentityFile ~/.ssh/my_key.pem

# 이후 간단하게 접속
ssh myserver
```

### 파일 전송 (scp)
```bash
# 로컬 → 원격 파일 전송
scp file.txt user@192.168.0.1:/home/user/

# 원격 → 로컬 파일 다운로드
scp user@192.168.0.1:/home/user/file.txt ./

# 폴더 전체 전송
scp -r myfolder/ user@192.168.0.1:/home/user/

# 포트 지정
scp -P 2222 file.txt user@192.168.0.1:/home/user/
```

### 포트 포워딩
```bash
# 로컬 포트 포워딩
# 로컬 8080 → 원격 서버의 80 포트로 연결
ssh -L 8080:localhost:80 user@192.168.0.1

# 원격 포트 포워딩
# 원격 서버의 8080 → 로컬 80 포트로 연결
ssh -R 8080:localhost:80 user@192.168.0.1

# 터널만 유지 (백그라운드)
ssh -N -f -L 8080:localhost:80 user@192.168.0.1
```

---

## 📁 SSH 관련 주요 파일

| 파일 경로 | 설명 |
|-----------|------|
| `~/.ssh/id_rsa` | 개인키 (절대 외부 유출 금지) |
| `~/.ssh/id_rsa.pub` | 공개키 (서버에 등록) |
| `~/.ssh/authorized_keys` | 접속 허용된 공개키 목록 (서버 측) |
| `~/.ssh/config` | SSH 접속 설정 단축키 파일 |
| `~/.ssh/known_hosts` | 접속했던 서버의 호스트키 저장 |
| `/etc/ssh/sshd_config` | SSH 서버 설정 파일 |

---

## 🔒 보안 권장 설정 (`/etc/ssh/sshd_config`)

```bash
# 루트 직접 로그인 금지
PermitRootLogin no

# 비밀번호 로그인 금지 (키 인증만 허용)
PasswordAuthentication no

# 기본 포트 변경 (22 → 다른 포트)
Port 2222

# 최대 인증 시도 횟수 제한
MaxAuthTries 3

# 설정 변경 후 SSH 서비스 재시작
sudo systemctl restart sshd
```

---

## ⚠️ 주의사항

- 개인키(`id_rsa`)는 **권한을 반드시 600으로 설정** — 그렇지 않으면 SSH가 거부함
- `known_hosts`에 저장된 호스트키가 변경되면 접속 거부 → 서버 재설치 등의 경우 해당 줄 삭제 필요

```bash
# 개인키 권한 설정
chmod 600 ~/.ssh/id_rsa
chmod 700 ~/.ssh/

# known_hosts에서 특정 호스트 삭제
ssh-keygen -R 192.168.0.1
```
