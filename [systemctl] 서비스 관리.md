# ⚙️ systemctl

systemd 기반 리눅스에서 **서비스(데몬)를 시작·중지·활성화·상태 확인**하는 명령어.

---

## 기본 문법

```bash
systemctl [옵션] [명령] [서비스명]
```

---

## 서비스 제어

```bash
# 서비스 시작
systemctl start nginx

# 서비스 중지
systemctl stop nginx

# 서비스 재시작
systemctl restart nginx

# 설정 변경 후 재로드 (무중단)
systemctl reload nginx

# 서비스 상태 확인
systemctl status nginx
```

---

## 부팅 시 자동 시작

```bash
# 부팅 시 자동 시작 등록
systemctl enable nginx

# 자동 시작 해제
systemctl disable nginx

# 자동 시작 등록 + 즉시 시작
systemctl enable --now nginx

# 자동 시작 해제 + 즉시 중지
systemctl disable --now nginx

# 자동 시작 여부 확인
systemctl is-enabled nginx
```

---

## 서비스 목록 조회

```bash
# 현재 실행 중인 서비스 목록
systemctl list-units --type=service

# 실행 중인 서비스만 필터
systemctl list-units --type=service --state=running

# 실패한 서비스 확인
systemctl list-units --type=service --state=failed

# 부팅 시 자동 시작 목록
systemctl list-unit-files --type=service
```

---

## 시스템 전체 제어

```bash
# 시스템 재부팅
systemctl reboot

# 시스템 종료
systemctl poweroff

# 시스템 일시 중단 (슬립)
systemctl suspend
```

---

## 자주 쓰는 서비스 예시

```bash
# Nginx 웹서버
systemctl status nginx
systemctl restart nginx

# SSH 서버
systemctl status sshd
systemctl enable sshd

# MySQL / MariaDB
systemctl start mysql
systemctl enable mariadb

# 방화벽 (firewalld)
systemctl status firewalld
```

---

## 주요 옵션

| 옵션 | 설명 |
|------|------|
| `start` | 서비스 시작 |
| `stop` | 서비스 중지 |
| `restart` | 서비스 재시작 |
| `reload` | 설정 재로드 (프로세스 유지) |
| `status` | 서비스 상태 확인 |
| `enable` | 부팅 시 자동 시작 등록 |
| `disable` | 자동 시작 해제 |
| `is-active` | 실행 중인지 확인 (`active` / `inactive`) |
| `is-enabled` | 자동 시작 등록 여부 확인 |
| `list-units` | 유닛 목록 조회 |
| `daemon-reload` | systemd 설정 파일 재로드 |

---

## 서비스 파일 수정 후 적용

```bash
# /etc/systemd/system/ 아래 유닛 파일 수정 후 반드시 실행
systemctl daemon-reload
systemctl restart [서비스명]
```

---

## 팁

- `systemctl status` 출력에서 **Active: active (running)** 이면 정상 동작 중
- 서비스 로그는 `journalctl -u [서비스명]` 으로 확인
- `reload` 는 nginx, apache처럼 reload를 지원하는 서비스에만 사용 가능
