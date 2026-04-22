# 📄 awk — 열 단위 텍스트 처리

> 텍스트 파일이나 명령어 출력에서 **특정 열(필드)을 추출하거나 조건에 맞는 행을 처리**하는 강력한 텍스트 처리 도구

---

## 📌 기본 구조

```bash
awk '패턴 { 액션 }' 파일명
```

| 구성 요소 | 설명 |
|-----------|------|
| `패턴` | 조건식 (생략 시 모든 행에 적용) |
| `액션` | 패턴이 일치할 때 실행할 명령 |
| `$0` | 현재 행 전체 |
| `$1, $2, ...` | 공백으로 구분된 1번째, 2번째 ... 열 |
| `NR` | 현재 행 번호 (Number of Record) |
| `NF` | 현재 행의 총 열 수 (Number of Field) |

---

## 🔍 기본 사용법

### 특정 열 출력
```bash
# 첫 번째 열 출력
awk '{ print $1 }' file.txt

# 첫 번째, 세 번째 열 출력
awk '{ print $1, $3 }' file.txt

# 행 전체 출력
awk '{ print $0 }' file.txt
```

### 구분자 지정 `-F`
```bash
# 콜론(:)으로 구분된 파일에서 첫 번째 열 출력
awk -F: '{ print $1 }' /etc/passwd

# 쉼표(,)로 구분 (CSV 파일)
awk -F, '{ print $1, $2 }' data.csv

# 탭으로 구분
awk -F'\t' '{ print $1 }' file.tsv
```

---

## 🎯 패턴 조건 사용

### 문자열 포함 여부
```bash
# "error" 문자열이 포함된 행 출력
awk '/error/ { print }' log.txt

# "error"가 포함되지 않은 행 출력
awk '!/error/ { print }' log.txt
```

### 특정 열 값 비교
```bash
# 3번째 열이 100보다 큰 행 출력
awk '$3 > 100 { print }' file.txt

# 2번째 열이 "active"인 행 출력
awk '$2 == "active" { print }' file.txt

# 1번째 열이 "root"인 행의 3번째 열 출력
awk '$1 == "root" { print $3 }' /etc/passwd
```

### 행 번호 조건
```bash
# 5번째 행부터 10번째 행 출력
awk 'NR >= 5 && NR <= 10 { print }' file.txt

# 짝수 행만 출력
awk 'NR % 2 == 0 { print }' file.txt

# 첫 번째 행 제외 (헤더 스킵)
awk 'NR > 1 { print }' file.txt
```

---

## 🛠️ 자주 쓰는 실전 예제

### 프로세스에서 특정 정보 추출
```bash
# 실행 중인 프로세스의 PID와 이름 출력
ps aux | awk '{ print $2, $11 }'

# CPU 사용량이 50% 이상인 프로세스 출력
ps aux | awk '$3 > 50 { print $2, $3, $11 }'
```

### 로그 분석
```bash
# 특정 날짜의 로그만 출력
awk '/2024-01-15/ { print }' access.log

# HTTP 200 응답만 출력
awk '$9 == 200 { print }' access.log
```

### 합계 / 평균 계산
```bash
# 2번째 열의 합계 출력
awk '{ sum += $2 } END { print "합계:", sum }' file.txt

# 평균 계산
awk '{ sum += $2; count++ } END { print "평균:", sum/count }' file.txt
```

### 디스크 사용량에서 특정 열 추출
```bash
# df 출력에서 마운트 포인트와 사용률만 출력
df -h | awk '{ print $6, $5 }'

# 사용률 80% 이상인 파티션만 출력 (% 제거 후 비교)
df -h | awk 'NR > 1 { gsub(/%/, "", $5); if ($5 > 80) print $6, $5"%" }'
```

---

## ⚙️ BEGIN / END 블록

```bash
# BEGIN : 파일 읽기 전에 실행
# END   : 파일 읽기 후 실행

awk '
  BEGIN { print "=== 시작 ===" }
  { print NR": "$0 }
  END   { print "=== 총 " NR "행 ===" }
' file.txt
```

---

## 🔗 파이프와 함께 사용

```bash
# 현재 디렉토리에서 가장 큰 파일 5개 출력
ls -lh | awk '{ print $5, $9 }' | sort -rh | head -5

# 네트워크 연결 상태 집계
netstat -an | awk '{ print $6 }' | sort | uniq -c | sort -rn

# 메모리 사용량 MB 단위로 출력
free -m | awk '/^Mem/ { print "전체:", $2"MB", "사용:", $3"MB", "여유:", $4"MB" }'
```

---

## 📝 옵션 요약

| 옵션 | 설명 | 예시 |
|------|------|------|
| `-F` | 필드 구분자 지정 | `awk -F: ...` |
| `-v` | 변수 지정 | `awk -v x=10 '$1 > x'` |
| `-f` | awk 스크립트 파일 실행 | `awk -f script.awk` |

---

## ⚡ sed / grep 과의 차이

| 명령어 | 주요 용도 |
|--------|-----------|
| `grep` | 행 단위 **검색** |
| `sed` | 행 단위 **치환/편집** |
| `awk` | 열 단위 **추출/계산/조건 처리** |

---

## 💡 팁

- `$NF` 는 **마지막 열**을 의미한다
  ```bash
  awk '{ print $NF }' file.txt
  ```
- `-v` 옵션으로 외부 변수를 awk 내부에서 사용할 수 있다
  ```bash
  threshold=100
  awk -v t=$threshold '$3 > t { print }' file.txt
  ```
- 여러 줄의 awk 처리는 스크립트 파일(`.awk`)로 분리하면 관리가 편하다
