# 🔐 파일 권한 관리 - chmod 명령어

> 파일이나 디렉토리의 **읽기·쓰기·실행 권한을 변경**하는 명령어  
> 보안 설정과 스크립트 실행 권한 부여에 필수

---

## 📌 기본 문법

```bash
chmod [옵션] 권한 파일명
```

---

## 🧩 권한 구조 이해

```
 -  rwx     r-x    r--  1 user group 1234 Mar 31 15:00 script.sh
 ↑  ↑↑↑     ↑↑↑    ↑↑↑
 │  └┬┘     └┬┘    └┬┘
 │ 소유자   그룹    그외
 │ (user) (group)
 │
 └ 파일 타입 (- 파일 / d 디렉토리 / l 심볼릭링크)
```

| 문자 | 의미 | 숫자 |
|------|------|------|
| `r` | 읽기 (read) | 4 |
| `w` | 쓰기 (write) | 2 |
| `x` | 실행 (execute) | 1 |
| `-` | 권한 없음 | 0 |

---

## 🔢 숫자 방식 (Octal)

> 세 자리 숫자로 `소유자 / 그룹 / others` 권한을 한 번에 지정

```bash
chmod 755 script.sh    # rwxr-xr-x
chmod 644 index.html   # rw-r--r--
chmod 600 secret.key   # rw-------
chmod 777 share.sh     # rwxrwxrwx (모두 허용 - 보안 주의!)
chmod 000 locked.txt   # ---------- (모두 차단)
```

### 📊 자주 쓰는 숫자 조합

| 숫자 | 권한 | 용도 |
|------|------|------|
| `755` | rwxr-xr-x | 실행 파일, 디렉토리 |
| `644` | rw-r--r-- | 일반 파일 (HTML, 설정) |
| `600` | rw------- | SSH 키, 민감한 파일 |
| `700` | rwx------ | 개인 스크립트 |
| `777` | rwxrwxrwx | 임시 테스트 (운영 금지!) |
| `400` | r-------- | 읽기 전용 (공개키 등) |

---

## 🔤 문자 방식 (Symbolic)

> `대상 + 연산자 + 권한` 조합으로 세밀하게 설정

### 대상

| 문자 | 대상 |
|------|------|
| `u` | 소유자 (user) |
| `g` | 그룹 (group) |
| `o` | 그 외 (others) |
| `a` | 전체 (all = u+g+o) |

### 연산자

| 기호 | 동작 |
|------|------|
| `+` | 권한 추가 |
| `-` | 권한 제거 |
| `=` | 권한 지정 (나머지 제거) |

```bash
# 소유자에게 실행 권한 추가
chmod u+x script.sh

# 그룹과 others 쓰기 권한 제거
chmod go-w file.txt

# 전체에게 읽기 권한 추가
chmod a+r file.txt

# 소유자만 읽기·쓰기, 나머지 권한 없음
chmod u=rw,go= secret.txt

# 그룹에게 읽기·실행만 허용
chmod g=rx script.sh
```

---

## ⚙️ 주요 옵션

| 옵션 | 설명 |
|------|------|
| `-R` | 디렉토리 하위 전체 재귀 적용 |
| `-v` | 변경 내용 출력 (verbose) |
| `-c` | 실제로 변경된 파일만 출력 |
| `--reference=파일` | 특정 파일 권한을 그대로 복사 |

---

## 🛠️ 실전 활용 예시

```bash
# 쉘 스크립트 실행 권한 부여
chmod +x deploy.sh

# SSH 키 파일 보안 설정 (필수!)
chmod 600 ~/.ssh/id_rsa
chmod 644 ~/.ssh/id_rsa.pub
chmod 700 ~/.ssh/

# 웹 서버 파일 권한 일괄 설정
chmod -R 755 /var/www/html/
chmod -R 644 /var/www/html/*.html

# 설정 파일 읽기 전용으로 보호
chmod 444 /etc/myapp/config.conf

# 디렉토리 전체 권한 변경
chmod -R 750 /app/

# 권한 변경 확인하며 적용
chmod -v 755 script.sh

# 다른 파일 권한 그대로 복사
chmod --reference=origin.sh target.sh
```

---

## 🚨 보안 주의사항

```bash
# ❌ 절대 운영 환경에서 사용 금지
chmod 777 /var/www/html/    # 누구나 수정 가능 → 해킹 위험
chmod 777 config.php         # DB 비밀번호 노출 위험

# ✅ 권장 설정
chmod 755 /var/www/html/     # 디렉토리
chmod 644 /var/www/html/*.php # PHP 파일
chmod 600 .env                # 환경변수 파일
chmod 600 ~/.ssh/id_rsa       # SSH 개인키
```

---

## 🔁 chmod vs chown 비교

| 명령어 | 역할 |
|--------|------|
| `chmod` | 권한 변경 (읽기·쓰기·실행) |
| `chown` | 소유자/그룹 변경 |

```bash
# 소유자 변경
chown user:group file.txt

# 권한 + 소유자 함께 변경
chown www-data:www-data /var/www/html/
chmod -R 755 /var/www/html/
```

---

## ⚡ 자주 쓰는 조합 모음

```bash
chmod +x 파일           # 실행 권한 빠르게 부여
chmod 600 파일           # SSH 키·민감 파일 보안 설정
chmod -R 755 디렉토리    # 디렉토리 전체 일괄 적용
chmod 644 파일           # 일반 파일 기본 권한
chmod u+x,go-w 파일      # 소유자 실행 추가 + 그룹/others 쓰기 제거
```

---

> 💬 **Tip**: 권한 확인은 `ls -l` 로, 숫자 권한 확인은 `stat -c "%a %n" 파일명` 으로 할 수 있어요!
> ```bash
> stat -c "%a %n" script.sh
> # 출력: 755 script.sh
> ```
