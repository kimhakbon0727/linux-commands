# 📋 journalctl

systemd의 **로그(journal)를 조회**하는 명령어. 서비스 오류 분석, 시스템 이벤트 추적에 사용.

---

## 기본 문법

```bash
journalctl [옵션]
```

---

## 기본 조회

```bash
# 전체 로그 조회 (오래된 것부터)
journalctl

# 최신 로그부터 조회
journalctl -r

# 마지막 N줄 출력
journalctl -n 50

# 실시간 로그 스트리밍 (tail -f 와 유사)
journalctl -f
```

---

## 서비스별 로그 조회

```bash
# 특정 서비스 로그
journalctl -u nginx

# 특정 서비스 실시간 로그
journalctl -u nginx -f

# 특정 서비스 마지막 100줄
journalctl -u nginx -n 100

# 여러 서비스 동시 조회
journalctl -u nginx -u mysql
```

---

## 시간 기준 필터

```bash
# 오늘 로그만
journalctl --since today

# 특정 시간 이후
journalctl --since "2024-01-15 09:00:00"

# 특정 시간 범위
journalctl --since "2024-01-15 09:00:00" --until "2024-01-15 18:00:00"

# 1시간 전부터
journalctl --since "1 hour ago"

# 어제 로그
journalctl --since yesterday --until today
```

---

## 로그 레벨 필터

```bash
# 에러 이상만 출력
journalctl -p err

# 경고 이상만 출력
journalctl -p warning
```

| 레벨 | 번호 | 설명 |
|------|------|------|
| `emerg` | 0 | 시스템 사용 불가 |
| `alert` | 1 | 즉시 조치 필요 |
| `crit` | 2 | 치명적 오류 |
| `err` | 3 | 일반 오류 |
| `warning` | 4 | 경고 |
| `notice` | 5 | 주요 알림 |
| `info` | 6 | 정보 |
| `debug` | 7 | 디버그 |

---

## 부팅 기준 조회

```bash
# 이번 부팅 로그만
journalctl -b

# 이전 부팅 로그
journalctl -b -1

# 2번 전 부팅 로그
journalctl -b -2

# 부팅 목록 확인
journalctl --list-boots
```

---

## 출력 형식

```bash
# JSON 형식 출력
journalctl -u nginx -o json

# 한 줄 JSON 형식
journalctl -u nginx -o json-pretty

# 타임스탬프 포함 짧은 형식
journalctl -u nginx -o short-iso
```

---

## 로그 용량 관리

```bash
# 현재 저장된 로그 용량 확인
journalctl --disk-usage

# 2주 이전 로그 삭제
journalctl --vacuum-time=2weeks

# 500MB 초과분 삭제
journalctl --vacuum-size=500M
```

---

## 자주 쓰는 조합

```bash
# 서비스 오류 원인 파악
journalctl -u nginx -n 50 -p err

# 실시간 에러 모니터링
journalctl -f -p err

# 오늘 특정 서비스 전체 로그
journalctl -u mysql --since today
```

---

## 팁

- 로그가 많을 경우 `| grep` 과 조합하면 빠르게 필터링 가능
- `-f` 옵션은 서비스 배포 후 실시간 모니터링에 유용
- `systemctl status [서비스]` 는 최근 로그 일부만 보여주고, 전체는 `journalctl -u` 로 확인
