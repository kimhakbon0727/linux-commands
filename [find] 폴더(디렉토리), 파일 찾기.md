# 🔍 find 명령어 치트시트

## 기본 구조

```bash
find [경로] [옵션] [조건] [액션]
```

---

## 📁 이름으로 찾기

```bash
# 파일명 정확히 일치
find . -name "config.txt"

# 대소문자 무시
find . -iname "readme.md"

# 와일드카드 — 확장자로 찾기
find . -name "*.log"
find . -name "*.py"

# 특정 이름 제외
find . -not -name "*.log"
```

---

## 📂 타입으로 찾기

```bash
# 파일만
find . -type f

# 디렉토리만
find . -type d

# 심볼릭 링크만
find . -type l
```

---

## 🕐 시간으로 찾기

```bash
# 수정된 지 N일 이내
find . -mtime -7        # 7일 이내
find . -mtime +30       # 30일 이상 지난 것

# 접근한 지 N일 이내
find . -atime -1        # 24시간 이내

# 변경된 지 N분 이내
find . -mmin -60        # 60분 이내
```

---

## 📏 크기로 찾기

```bash
# 정확한 크기
find . -size 100c       # 100 bytes
find . -size 50k        # 50 KB
find . -size 1M         # 1 MB

# 범위 지정
find . -size +10M       # 10MB 초과
find . -size -1k        # 1KB 미만
find . -size +1M -size -100M   # 1MB~100MB 사이
```

---

## 🔑 권한으로 찾기

```bash
# 퍼미션 정확히 일치
find . -perm 644
find . -perm 755

# 실행 가능한 파일
find . -perm /u+x

# 소유자로 찾기
find . -user username
find . -group groupname
```

---

## 📂 경로 / 깊이 제어

```bash
# 현재 디렉토리에서만 (하위 탐색 안 함)
find . -maxdepth 1 -name "*.sh"

# 최소 깊이 지정
find . -mindepth 2 -name "*.conf"

# 특정 디렉토리 제외
find . -path "./node_modules" -prune -o -name "*.js" -print
find . -path "./.git" -prune -o -type f -print
```

---

## ⚙️ 액션 (찾은 후 실행)

```bash
# 결과 출력 (기본값)
find . -name "*.txt" -print

# 상세 정보 출력 (ls -l 형식)
find . -name "*.log" -ls

# 명령어 실행 — {} 는 찾은 파일, \; 는 각각 실행
find . -name "*.tmp" -exec rm {} \;

# + 는 한 번에 묶어서 실행 (더 빠름)
find . -name "*.log" -exec ls -lh {} +

# 삭제
find . -name "*.tmp" -delete

# 내용 출력
find . -name "*.conf" -exec cat {} \;
```

---

## 🔗 조건 조합

```bash
# AND (기본값, -a 생략 가능)
find . -type f -name "*.py" -size +1k

# OR
find . -name "*.jpg" -o -name "*.png"

# NOT
find . -not -name "*.git"
find . ! -name "*.git"
```

---

## 💡 자주 쓰는 패턴 모음

```bash
# 빈 파일 찾기
find . -type f -empty

# 빈 디렉토리 찾기
find . -type d -empty

# 최근 수정된 파일 10개
find . -type f -printf '%T@ %p\n' | sort -n | tail -10

# 특정 확장자 파일 개수 세기
find . -name "*.py" | wc -l

# 파일 찾아서 다른 디렉토리로 복사
find . -name "*.conf" -exec cp {} /backup/ \;

# 오래된 로그 삭제 (30일 이상)
find /var/log -name "*.log" -mtime +30 -delete

# 심볼릭 링크 깨진 것 찾기
find . -type l ! -exec test -e {} \; -print

# node_modules, .git 제외하고 검색
find . \( -path ./node_modules -o -path ./.git \) -prune -o -name "*.js" -print
```

---

## 🧩 xargs 와 조합

```bash
# find 결과를 xargs로 넘기기 (공백 있는 파일명은 -print0 + -0 사용)
find . -name "*.log" -print0 | xargs -0 rm

# grep과 조합 — 파일 내용 검색
find . -name "*.py" | xargs grep "def main"

# 파일 권한 일괄 변경
find . -type f -name "*.sh" | xargs chmod +x
```

---

## ⚠️ 주의사항

| 상황 | 주의 |
|------|------|
| `-exec rm` | 반드시 먼저 `-print`로 확인 후 삭제 |
| 파일명에 공백 | `-print0 \| xargs -0` 조합 사용 |
| `/` 루트 검색 | 시스템 전체 탐색으로 느릴 수 있음 |
| `*` 와일드카드 | 반드시 따옴표로 감싸기 (`"*.log"`) |
