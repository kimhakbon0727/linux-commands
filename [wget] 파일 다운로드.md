# 📥 wget 명령어 치트시트
> 웹 서버로부터 파일을 다운로드하는 리눅스/유닉스 명령어 (HTTP, HTTPS, FTP 프로토콜 지원)

---

## 📌 기본 문법
```
wget [옵션] <URL>
```

---

## ⚙️ 주요 옵션
| 옵션 | 설명 |
|------|------|
| `-O` | 저장할 파일명 지정 |
| `-P` | 저장할 디렉토리 지정 |
| `-q` | 출력 숨김 (quiet) |
| `-v` | 상세 출력 (verbose, 기본값) |
| `-c` | 이어받기 (중단된 다운로드 재개) |
| `-b` | 백그라운드 다운로드 |
| `-r` | 재귀적 다운로드 |
| `-l` | 재귀 깊이 지정 (기본값 5) |
| `-np` | 상위 디렉토리로 올라가지 않음 |
| `-N` | 서버 파일이 더 새로운 경우에만 다운로드 |
| `-t` | 재시도 횟수 지정 (0이면 무한) |
| `--limit-rate` | 다운로드 속도 제한 |
| `--user` | 인증 아이디 |
| `--password` | 인증 비밀번호 |
| `--no-check-certificate` | SSL 인증서 검증 무시 |
| `-i` | 파일에서 URL 목록 읽어 다운로드 |
| `--spider` | 실제 다운로드 없이 URL 존재 여부 확인 |
| `-w` | 요청 간 대기 시간(초) 지정 |
| `--mirror` | 미러링 모드 (`-r -N -l inf --no-remove-listing` 조합) |

---

## 💡 실전 예시
```bash
# 기본 다운로드
wget https://example.com/file.zip

# 파일명 지정해서 저장
wget -O myfile.zip https://example.com/file.zip

# 특정 디렉토리에 저장
wget -P /home/user/downloads/ https://example.com/file.zip

# 중단된 다운로드 이어받기
wget -c https://example.com/largefile.zip

# 백그라운드에서 다운로드
wget -b https://example.com/file.zip

# 백그라운드 다운로드 진행 확인
tail -f wget-log

# 재시도 횟수 지정
wget -t 5 https://example.com/file.zip

# 무한 재시도
wget -t 0 https://example.com/file.zip

# 다운로드 속도 제한 (500KB/s)
wget --limit-rate=500k https://example.com/file.zip

# 인증이 필요한 파일 다운로드
wget --user=username --password=password https://example.com/secret.zip

# SSL 인증서 무시
wget --no-check-certificate https://self-signed.example.com/file.zip

# 여러 URL을 파일로 지정해서 한번에 다운로드
wget -i urls.txt

# URL 존재 여부만 확인 (다운로드 X)
wget --spider https://example.com/file.zip

# 요청 간 1초 대기 (서버 부하 방지)
wget -w 1 https://example.com/file.zip

# 재귀 다운로드 (웹사이트 전체)
wget -r https://example.com

# 재귀 깊이 3단계로 제한
wget -r -l 3 https://example.com

# 상위 디렉토리 이동 없이 재귀 다운로드
wget -r -np https://example.com/files/

# 웹사이트 미러링
wget --mirror https://example.com

# 조용히 다운로드 (출력 없음)
wget -q https://example.com/file.zip

# 다운로드 성공 여부만 확인 (스크립트용)
wget -q --spider https://example.com/file.zip
echo $?   # 0이면 성공, 1이면 실패
```

---

## 🔀 curl vs wget 비교
| 항목 | curl | wget |
|------|------|------|
| 주 용도 | API 요청, 데이터 전송 | 파일 다운로드 |
| HTTP 메서드 | GET/POST/PUT/DELETE 등 | GET 위주 |
| 이어받기 | `-C -` | `-c` |
| 재귀 다운로드 | ❌ | ✅ |
| 백그라운드 실행 | ❌ | `-b` |
| 파이프 출력 | ✅ 기본 | `-O -` 필요 |
| 설치 여부 | 대부분 기본 설치 | 별도 설치 필요할 수 있음 |

---

## ⚠️ 주의사항
- `-r` 옵션으로 재귀 다운로드 시 서버에 과부하를 줄 수 있으므로 `-w` 옵션으로 딜레이 추가 권장
- `--mirror` 사용 시 대용량 트래픽이 발생할 수 있으므로 주의
- `--password` 옵션 직접 입력 시 shell history에 남을 수 있으므로 주의
- 백그라운드 다운로드 시 진행 상황은 `wget-log` 파일에 저장됨
