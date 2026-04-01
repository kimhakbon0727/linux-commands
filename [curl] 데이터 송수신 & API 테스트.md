# 🌐 curl 명령어 치트시트
> URL을 통해 데이터를 전송하거나 받아오는 리눅스/유닉스 명령어 (HTTP, FTP, SMTP 등 다양한 프로토콜 지원)

---

## 📌 기본 문법
```
curl [옵션] <URL>
```

---

## ⚙️ 주요 옵션
| 옵션 | 설명 |
|------|------|
| `-X` | HTTP 메서드 지정 (GET, POST, PUT, DELETE 등) |
| `-H` | 요청 헤더 추가 |
| `-d` | 요청 바디 데이터 전송 (POST) |
| `-o` | 응답을 파일로 저장 (파일명 지정) |
| `-O` | 응답을 원본 파일명으로 저장 |
| `-L` | 리다이렉트 자동 추적 |
| `-i` | 응답 헤더 포함 출력 |
| `-I` | 응답 헤더만 출력 (HEAD 요청) |
| `-s` | 진행상황 출력 숨김 (silent) |
| `-v` | 요청/응답 상세 출력 (verbose) |
| `-u` | 인증 정보 입력 (`user:password`) |
| `-k` | SSL 인증서 검증 무시 |
| `-b` | 쿠키 전송 |
| `-c` | 쿠키 파일로 저장 |
| `--retry` | 실패 시 재시도 횟수 지정 |
| `-x` | 프록시 서버 지정 |

---

## 💡 실전 예시
```bash
# 기본 GET 요청
curl https://example.com

# 응답 헤더 포함 출력
curl -i https://example.com

# 응답 헤더만 출력
curl -I https://example.com

# 리다이렉트 추적
curl -L https://example.com

# 파일 다운로드 (파일명 지정)
curl -o output.html https://example.com

# 파일 다운로드 (원본 파일명 유지)
curl -O https://example.com/file.zip

# POST 요청 (form 데이터)
curl -X POST -d "name=kim&age=30" https://example.com/api

# POST 요청 (JSON 데이터)
curl -X POST \
  -H "Content-Type: application/json" \
  -d '{"name": "kim", "age": 30}' \
  https://example.com/api

# PUT 요청
curl -X PUT \
  -H "Content-Type: application/json" \
  -d '{"name": "lee"}' \
  https://example.com/api/1

# DELETE 요청
curl -X DELETE https://example.com/api/1

# Authorization 헤더 (Bearer 토큰)
curl -H "Authorization: Bearer <토큰>" https://example.com/api

# Basic 인증
curl -u username:password https://example.com

# SSL 인증서 무시
curl -k https://self-signed.example.com

# 쿠키 전송
curl -b "session=abc123" https://example.com

# 쿠키 파일로 저장
curl -c cookies.txt https://example.com

# 저장된 쿠키 파일로 요청
curl -b cookies.txt https://example.com

# 진행상황 숨기고 결과만 출력
curl -s https://example.com

# 상세 디버그 출력
curl -v https://example.com

# 실패 시 3회 재시도
curl --retry 3 https://example.com

# 프록시 사용
curl -x http://proxy.example.com:8080 https://example.com

# 응답 코드만 출력
curl -s -o /dev/null -w "%{http_code}" https://example.com

# 요청 시간 측정
curl -s -o /dev/null -w "time: %{time_total}s\n" https://example.com
```

---

## 🔀 HTTP 메서드 정리
| 메서드 | 용도 | 예시 |
|--------|------|------|
| `GET` | 데이터 조회 | `curl https://api.example.com/users` |
| `POST` | 데이터 생성 | `curl -X POST -d '{}' https://api.example.com/users` |
| `PUT` | 데이터 전체 수정 | `curl -X PUT -d '{}' https://api.example.com/users/1` |
| `PATCH` | 데이터 일부 수정 | `curl -X PATCH -d '{}' https://api.example.com/users/1` |
| `DELETE` | 데이터 삭제 | `curl -X DELETE https://api.example.com/users/1` |

---

## 📤 Content-Type 별 요청 형식
| 형식 | 헤더 | 데이터 예시 |
|------|------|------------|
| JSON | `Content-Type: application/json` | `-d '{"key": "value"}'` |
| Form | `Content-Type: application/x-www-form-urlencoded` | `-d 'key=value&key2=value2'` |
| 파일 업로드 | `Content-Type: multipart/form-data` | `-F "file=@/path/to/file"` |

---

## ⚠️ 주의사항
- `-d` 옵션 사용 시 기본 메서드가 자동으로 `POST`로 설정됨
- `-O` 옵션은 URL에 파일명이 포함되어 있어야 정상 동작
- `-k` 옵션은 보안상 위험하므로 테스트 환경에서만 사용 권장
- 비밀번호를 `-u` 옵션에 직접 입력 시 shell history에 남을 수 있으므로 주의
