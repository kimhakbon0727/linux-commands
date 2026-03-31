# 🖥️ 실시간 모니터링 - watch 명령어

> 지정한 명령어를 **일정 간격으로 반복 실행**하며 결과를 실시간으로 화면에 표시하는 명령어  
> 서버 모니터링, 로그 감시, 리소스 추적에 특히 유용

---

## 📌 기본 문법

```bash
watch [옵션] 명령어
```

---

## ⚙️ 주요 옵션

| 옵션 | 설명 |
|------|------|
| `-n 초` | 갱신 주기 설정 (기본값: 2초) |
| `-d` | 변경된 부분 **하이라이트** 표시 |
| `-t` | 상단 헤더(시간/명령어) 숨기기 |
| `-e` | 명령어 오류 발생 시 자동 종료 |
| `-g` | 출력 결과가 변경되면 자동 종료 |
| `-x` | 명령어를 `exec`로 직접 실행 (쉘 없이) |

---

## 💡 기본 사용 예시

```bash
# 2초마다 날짜 출력 (기본)
watch date

# 5초마다 갱신
watch -n 5 date

# 1초마다 갱신 (빠른 모니터링)
watch -n 1 "df -h"

# 변경된 부분 강조 표시
watch -d "ls -l"

# 헤더 숨기고 깔끔하게 출력
watch -t "date"
```

---

## 🌐 네트워크 모니터링

```bash
# 🔥 HTTPS(443포트) 현재 연결된 세션 수 실시간 확인
watch "netstat -an | grep :443.*ESTABLISHED | wc -l"

# 1초마다 더 빠르게 갱신
watch -n 1 "netstat -an | grep :443.*ESTABLISHED | wc -l"

# 변경 시 강조 표시 추가
watch -d "netstat -an | grep :443.*ESTABLISHED | wc -l"

# 80포트(HTTP) 연결 수 확인
watch "netstat -an | grep :80.*ESTABLISHED | wc -l"

# 전체 포트별 연결 상태 요약
watch "netstat -an | grep ESTABLISHED | awk '{print $4}' | cut -d: -f2 | sort | uniq -c | sort -rn"

# 특정 IP 접속 현황
watch "netstat -an | grep 192.168.1.100"
```

> 💬 `netstat -an | grep :443.*ESTABLISHED | wc -l` 해석
> - `netstat -an` → 모든 네트워크 연결 목록 출력
> - `grep :443.*ESTABLISHED` → 443포트 중 연결 확립된 것만 필터
> - `wc -l` → 해당 줄 수(= 세션 수) 카운트

---

## 💻 시스템 리소스 모니터링

```bash
# CPU/메모리 사용률 실시간 확인
watch -n 1 "top -bn1 | head -20"

# 디스크 사용량 변화 감시
watch -d "df -h"

# 메모리 상태
watch -n 2 "free -h"

# 특정 프로세스 감시
watch -n 1 "ps aux | grep nginx | grep -v grep"

# 로드 애버리지 확인
watch -n 1 "uptime"
```

---

## 📁 파일/디렉토리 감시

```bash
# 디렉토리 파일 변화 감시
watch -d "ls -lh /var/log/"

# 로그 파일 크기 변화
watch "du -sh /var/log/app.log"

# 특정 디렉토리 파일 수 카운트
watch "find /tmp -type f | wc -l"
```

---

## 🛠️ 실전 활용 예시

```bash
# 배포 후 프로세스 뜨는지 확인
watch -n 1 "ps aux | grep myapp"

# DB 커넥션 풀 모니터링
watch -n 2 "netstat -an | grep :5432 | grep ESTABLISHED | wc -l"

# 특정 포트 LISTEN 상태 감시 (서버 재시작 감지)
watch -g "netstat -an | grep :8080.*LISTEN"

# Kubernetes Pod 상태 감시
watch -n 2 "kubectl get pods"

# Docker 컨테이너 상태
watch -n 3 "docker ps"

# nginx 접속자 수 실시간
watch -n 1 "cat /var/log/nginx/access.log | wc -l"
```

---

## ⌨️ 단축키

| 키 | 동작 |
|----|------|
| `q` | 종료 |
| `Ctrl + C` | 종료 |
| `d` | 변경 강조 토글 |
| `t` | 헤더 토글 |
| `Space` | 즉시 갱신 |

---

## ⚡ 자주 쓰는 조합 모음

```bash
watch -n 1 -d          # 1초 갱신 + 변경 강조 (모니터링 기본 세트)
watch -n 1 "..."       # 빠른 실시간 확인
watch -g "..."         # 상태 변경 감지 후 자동 종료 (스크립트 연동)
watch -t -n 1 "..."   # 헤더 없이 깔끔하게 출력
```

---

> 💬 **Tip**: `watch -g` 는 특정 상태가 될 때까지 기다렸다가 다음 명령을 실행하는 스크립트에서 매우 유용해요!
> ```bash
> watch -g "netstat -an | grep :8080.*LISTEN" && echo "서버 준비 완료!"
> ```
