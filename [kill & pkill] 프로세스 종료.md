# ⚡ kill / pkill 명령어 치트시트

> 실행 중인 **프로세스에 신호(Signal)를 보내** 종료하거나 제어하는 리눅스/유닉스 명령어

---

## 📌 기본 문법

```
kill  [옵션] [PID...]
pkill [옵션] [프로세스명]
killall [옵션] [프로세스명]
```

> 💡 `kill` 은 **PID** 로, `pkill` / `killall` 은 **프로세스 이름** 으로 종료

---

## ⚙️ 주요 시그널

| 시그널 | 번호 | 설명 |
|--------|------|------|
| `SIGTERM` | `15` | 정상 종료 요청 (기본값) — 프로세스가 정리 후 종료 |
| `SIGKILL` | `9` | 강제 종료 — 즉시 죽임, 무시 불가 |
| `SIGHUP` | `1` | 재시작 요청 — 설정 파일 재로드에 자주 사용 |
| `SIGINT` | `2` | 인터럽트 (Ctrl+C 와 동일) |
| `SIGSTOP` | `19` | 프로세스 일시 정지 (무시 불가) |
| `SIGCONT` | `18` | 일시 정지된 프로세스 재개 |

```bash
# 사용 가능한 모든 시그널 목록 확인
kill -l
```

---

## 💡 실전 예시

### kill — PID로 종료
```bash
# PID 확인 (ps 또는 pgrep 활용)
ps aux | grep nginx
pgrep nginx

# 정상 종료 (SIGTERM, 기본값)
kill 1234

# 강제 종료 (SIGKILL)
kill -9 1234
kill -SIGKILL 1234

# 설정 재로드 (SIGHUP)
kill -1 1234
kill -HUP 1234

# 여러 PID 한 번에 종료
kill -9 1234 5678 9012
```

### pkill — 프로세스 이름으로 종료
```bash
# 프로세스 이름으로 종료
pkill nginx

# 강제 종료
pkill -9 nginx

# 정확히 일치하는 이름만 종료 (부분 일치 방지)
pkill -x nginx

# 특정 사용자의 프로세스만 종료
pkill -u username nginx

# 종료된 프로세스 이름 출력
pkill -e nginx
```

### killall — 같은 이름의 프로세스 전체 종료
```bash
# 같은 이름의 모든 프로세스 종료
killall nginx

# 강제 종료
killall -9 nginx

# 종료 전 확인
killall -i nginx

# 특정 사용자의 프로세스만 종료
killall -u username nginx
```

### PID 찾기
```bash
# 프로세스 이름으로 PID 조회
pgrep nginx

# 프로세스 이름과 PID 함께 조회
pgrep -l nginx

# ps로 PID 찾기
ps aux | grep nginx

# 포트로 PID 찾기
lsof -i :8080
ss -tlnp | grep 8080
```

---

## 🔄 kill vs pkill vs killall 비교

| 항목 | `kill` | `pkill` | `killall` |
|------|--------|---------|-----------|
| 대상 지정 | PID | 프로세스명 (부분 일치) | 프로세스명 (정확 일치) |
| 여러 프로세스 | PID 나열 | 일치하는 전체 | 동일 이름 전체 |
| 사용자 필터 | ❌ | ✅ (`-u`) | ✅ (`-u`) |
| 확인 메시지 | ❌ | ❌ | ✅ (`-i`) |

---

## ⚠️ 주의사항

- `kill -9` 는 프로세스가 **정리 작업 없이 즉시 종료** → 데이터 유실 가능성 있음
- 먼저 `kill` (SIGTERM) 시도 후, 응답 없을 때 `kill -9` 사용 권장
- `pkill` 은 **부분 일치**로 동작하므로 의도치 않은 프로세스가 종료될 수 있음 → `-x` 옵션으로 정확히 일치하는 것만 종료

```bash
# 권장 순서
kill 1234          # 1. 먼저 정상 종료 시도
sleep 3            # 2. 잠시 대기
kill -9 1234       # 3. 그래도 살아있으면 강제 종료
```
