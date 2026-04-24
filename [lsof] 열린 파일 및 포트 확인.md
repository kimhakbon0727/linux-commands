# 🔍 lsof

**List Open Files** – 현재 열려 있는 파일, 소켓, 포트를 확인하는 명령어.  
리눅스에서 "모든 것은 파일"이므로 프로세스·네트워크 연결·장치까지 조회 가능.

---

## 기본 문법

```bash
lsof [옵션] [파일/디렉토리]
```

---

## 기본 조회

```bash
# 열려 있는 모든 파일 목록 (매우 많음)
lsof

# 출력 줄 수 확인
lsof | wc -l
```

---

## 프로세스 기준 조회

```bash
# 특정 프로세스가 열고 있는 파일
lsof -p 1234

# 특정 명령어(프로세스명)로 조회
lsof -c nginx

# 특정 사용자가 열고 있는 파일
lsof -u ubuntu

# 특정 사용자 제외
lsof -u ^root
```

---

## 파일 / 디렉토리 기준 조회

```bash
# 특정 파일을 사용 중인 프로세스
lsof /var/log/nginx/access.log

# 특정 디렉토리 하위 파일을 사용 중인 프로세스
lsof +D /var/www/html
```

---

## 네트워크 / 포트 조회

```bash
# 모든 네트워크 연결 (소켓) 목록
lsof -i

# 특정 포트를 사용 중인 프로세스
lsof -i :80
lsof -i :3306

# TCP만 조회
lsof -i TCP

# UDP만 조회
lsof -i UDP

# 특정 포트 범위
lsof -i :8000-9000

# LISTEN 상태인 포트만
lsof -i -s TCP:LISTEN

# IPv4만 조회
lsof -i 4
```

---

## 삭제된 파일 조회

```bash
# 삭제됐지만 아직 점유 중인 파일 (디스크 용량 회수 안 된 상태)
lsof | grep deleted
```

> 디스크가 꽉 찼는데 `du`로 안 잡힐 때 유용

---

## 출력 형식

```bash
# 반복 갱신 (3초마다)
lsof -i :80 -r 3

# PID만 출력
lsof -t -i :80

# PID를 활용한 프로세스 강제 종료
kill -9 $(lsof -t -i :80)
```

---

## 주요 출력 컬럼

| 컬럼 | 설명 |
|------|------|
| `COMMAND` | 프로세스 이름 |
| `PID` | 프로세스 ID |
| `USER` | 실행 사용자 |
| `FD` | 파일 디스크립터 (`cwd`, `txt`, `mem`, 숫자 등) |
| `TYPE` | 파일 타입 (`REG`, `DIR`, `IPv4`, `IPv6`, `unix` 등) |
| `DEVICE` | 장치 번호 |
| `SIZE/OFF` | 파일 크기 또는 오프셋 |
| `NODE` | inode 번호 |
| `NAME` | 파일 경로 또는 네트워크 주소 |

---

## 자주 쓰는 조합

```bash
# 80 포트 점유 프로세스 확인
lsof -i :80

# 특정 포트 점유 프로세스 강제 종료
kill -9 $(lsof -t -i :8080)

# 삭제된 파일로 인한 디스크 낭비 확인
lsof | grep deleted | awk '{print $7, $9}' | sort -rh | head -20
```

---

## 팁

- `ss` 나 `netstat` 보다 **어느 프로세스가 해당 포트를 쓰는지** 확인할 때 더 직관적
- 권한이 없는 프로세스 정보는 `sudo` 없이 조회 불가
- `lsof -i :포트` 는 포트 충돌 디버깅 시 가장 먼저 쓰는 명령어
