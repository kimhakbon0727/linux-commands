# 🔒 nohup — 터미널 종료 후에도 프로세스 유지

> 터미널 세션이 끊겨도 **프로세스가 계속 실행**되도록 하는 명령어  
> `nohup` = **No Hang UP** (HUP 시그널 무시)

---

## 📌 기본 구조

```bash
nohup 명령어 [인자] &
```

> `&` 를 붙이면 **백그라운드**에서 실행 (붙이지 않으면 포그라운드 실행)

---

## 🔍 기본 사용법

```bash
# 기본 실행 (출력은 nohup.out 에 저장)
nohup python3 app.py &

# 출력 파일 직접 지정
nohup python3 app.py > app.log 2>&1 &

# 출력 무시
nohup python3 app.py > /dev/null 2>&1 &
```

---

## 📁 출력 파일 (nohup.out)

```bash
# nohup 실행 시 출력 저장 우선순위
# 1. > 로 직접 지정한 파일
# 2. 현재 디렉토리의 nohup.out
# 3. ~/nohup.out (현재 디렉토리에 쓰기 권한 없을 때)

# 실시간으로 로그 확인
tail -f nohup.out
tail -f app.log
```

---

## 🔎 2>&1 의 의미

```bash
nohup python3 app.py > app.log 2>&1 &
#                      ──────  ─────
#                      stdout  stderr도 stdout으로 합침
```

| 표현 | 의미 |
|------|------|
| `>` | 표준 출력(stdout) 을 파일로 저장 |
| `2>` | 표준 에러(stderr) 를 파일로 저장 |
| `2>&1` | stderr 를 stdout 과 같은 곳으로 보냄 |
| `> /dev/null 2>&1` | 출력을 완전히 무시 |

---

## 🛠️ 자주 쓰는 실전 예제

### Python / Node.js 서버 실행
```bash
# Python
nohup python3 server.py > server.log 2>&1 &

# Node.js
nohup node app.js > node.log 2>&1 &

# 실행 후 PID 바로 확인
echo "PID: $!"
```

### 쉘 스크립트 백그라운드 실행
```bash
nohup bash deploy.sh > deploy.log 2>&1 &
```

### 실행 즉시 PID 저장 (관리용)
```bash
nohup python3 app.py > app.log 2>&1 &
echo $! > app.pid
echo "실행됨 - PID: $(cat app.pid)"
```

### 나중에 프로세스 종료
```bash
# PID 파일로 종료
kill $(cat app.pid)

# 프로세스 이름으로 찾아서 종료
ps aux | grep app.py
kill 12345

# 또는
pkill -f app.py
```

---

## 🔎 프로세스 확인

```bash
# 실행 중인 nohup 프로세스 확인
ps aux | grep app.py

# 백그라운드 작업 목록 확인
jobs

# PID로 상태 확인
ps -p 12345
```

---

## 🔄 nohup vs 관련 명령어 비교

| 구분 | 설명 | 터미널 종료 후 |
|------|------|---------------|
| `명령어 &` | 백그라운드 실행 | 프로세스 종료 ❌ |
| `nohup 명령어 &` | HUP 시그널 무시 + 백그라운드 | 프로세스 유지 ✅ |
| `screen` | 터미널 세션 자체를 유지 | 세션 재접속 가능 ✅ |
| `tmux` | screen의 향상된 버전 | 세션 재접속 가능 ✅ |
| `systemd` | 서비스로 등록하여 관리 | 재부팅 후에도 유지 ✅ |

---

## ⚙️ & (백그라운드) 동작 이해

```bash
# 포그라운드 실행 (터미널 점유)
nohup python3 app.py

# 백그라운드 실행 (터미널 점유 안함) ← 보통 이렇게 사용
nohup python3 app.py &

# 백그라운드 작업을 포그라운드로 전환
fg %1

# 포그라운드 작업을 백그라운드로 전환
# Ctrl + Z 로 일시정지 후
bg %1
```

---

## 💡 팁

- 로그 파일을 반드시 지정하는 습관을 들이자. `nohup.out` 은 여러 프로세스가 섞여 관리가 어렵다
  ```bash
  # ✅ 권장
  nohup python3 app.py > /var/log/app.log 2>&1 &

  # ⚠️ 비권장
  nohup python3 app.py &
  ```
- 장기 운영 서비스라면 `nohup` 보다 `systemd` 서비스 등록을 권장한다 (재부팅 자동 시작, 상태 관리 용이)
- `$!` 는 **직전에 실행한 백그라운드 프로세스의 PID** 를 담고 있다
  ```bash
  nohup python3 app.py > app.log 2>&1 &
  echo $!   # 방금 실행한 PID 출력
  ```
