# 🔄 rsync — 파일 및 디렉토리 동기화 & 원격 전송

> 로컬 또는 원격 간 파일을 **빠르고 효율적으로 동기화**하는 명령어  
> **변경된 부분만 전송**하기 때문에 `cp`, `scp` 보다 훨씬 빠르다

---

## 📌 기본 구조

```bash
rsync [옵션] 출발지 목적지
```

---

## 🔍 기본 사용법

### 로컬 → 로컬 복사
```bash
# 파일 복사
rsync file.txt /backup/file.txt

# 디렉토리 복사
rsync -r /source/ /destination/
```

### 로컬 → 원격 전송
```bash
rsync -avz /local/path/ user@remote:/remote/path/
```

### 원격 → 로컬 수신
```bash
rsync -avz user@remote:/remote/path/ /local/path/
```

---

## ⚠️ 경로 끝 슬래시(`/`) 주의

```bash
# 슬래시 있음 : 디렉토리 안의 내용을 복사
rsync -av /source/ /destination/
# 결과: /destination/file1, /destination/file2 ...

# 슬래시 없음 : 디렉토리 자체를 복사
rsync -av /source /destination/
# 결과: /destination/source/file1, /destination/source/file2 ...
```
> ⚠️ 출발지 경로 끝 `/` 유무에 따라 결과가 달라지므로 주의

---

## ⚙️ 주요 옵션

| 옵션 | 설명 |
|------|------|
| `-a` | 아카이브 모드 (권한, 타임스탬프, 심볼릭 링크 등 모두 보존) |
| `-v` | 전송 과정 출력 (verbose) |
| `-z` | 전송 시 압축 (네트워크 절약) |
| `-P` | 진행률 표시 + 이어받기 (`--progress --partial` 합침) |
| `-n` | 실제 실행 없이 결과만 미리 확인 (dry-run) |
| `-r` | 디렉토리 재귀 복사 |
| `-u` | 목적지 파일이 더 최신이면 덮어쓰지 않음 |
| `-e` | 원격 쉘 지정 (주로 ssh 포트 지정 시 사용) |
| `--delete` | 출발지에 없는 파일을 목적지에서 삭제 (완전 동기화) |
| `--exclude` | 특정 파일/디렉토리 제외 |
| `--progress` | 파일별 전송 진행률 표시 |

---

## 🛠️ 자주 쓰는 실전 예제

### 기본 백업 (가장 많이 쓰는 패턴)
```bash
rsync -avz /home/user/ user@192.168.0.10:/backup/home/
```

### dry-run으로 먼저 확인 후 실행
```bash
# 먼저 어떤 파일이 전송될지 확인
rsync -avzn /source/ /destination/

# 확인 후 실제 실행
rsync -avz /source/ /destination/
```

### SSH 포트가 기본(22)이 아닐 때
```bash
rsync -avz -e "ssh -p 2222" /local/path/ user@remote:/remote/path/
```

### 완전 동기화 (삭제 포함)
```bash
# 출발지와 목적지를 완전히 동일하게 맞춤
# 출발지에 없는 파일은 목적지에서도 삭제됨
rsync -avz --delete /source/ /destination/
```

### 특정 파일/디렉토리 제외
```bash
# 단일 제외
rsync -avz --exclude='*.log' /source/ /destination/

# 여러 개 제외
rsync -avz --exclude='*.log' --exclude='tmp/' --exclude='.git' /source/ /destination/
```

### 대용량 파일 이어받기
```bash
rsync -avzP user@remote:/path/large-file.tar.gz /local/
```

### 주기적 백업 (cron 연동)
```bash
# crontab에 등록 - 매일 새벽 2시에 백업
0 2 * * * rsync -avz --delete /home/user/ user@backup-server:/backup/home/
```

---

## 🔎 scp와의 차이

| 구분 | `scp` | `rsync` |
|------|-------|---------|
| 전송 방식 | 항상 전체 복사 | 변경분만 전송 |
| 이어받기 | ❌ | ✅ (`-P`) |
| 삭제 동기화 | ❌ | ✅ (`--delete`) |
| 속도 | 느림 (대용량 불리) | 빠름 |
| 제외 패턴 | ❌ | ✅ (`--exclude`) |
| 주요 용도 | 단순 파일 전송 | 백업 / 동기화 |

---

## 💡 팁

- `-avzP` 조합이 가장 자주 쓰이는 패턴이다
  ```bash
  rsync -avzP /source/ user@remote:/destination/
  ```
- 처음 실행할 때는 반드시 `-n` (dry-run) 으로 먼저 확인하는 습관을 들이자
- `--delete` 는 강력한 옵션이므로 dry-run 확인 후 사용 권장
- 백업 자동화는 `cron` 과 함께 쓰면 강력하다
