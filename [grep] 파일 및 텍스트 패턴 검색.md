# 🔍 grep 명령어 치트시트

> 파일이나 텍스트에서 **패턴을 검색**하는 리눅스/유닉스 필수 명령어

---

## 📌 기본 문법

```bash
grep [옵션] 패턴 [파일...]
```

---

## ⚙️ 주요 옵션

| 옵션 | 설명 |
|------|------|
| `-i` | 대소문자 무시 (ignore case) |
| `-r` | 디렉토리 재귀 검색 (recursive) |
| `-n` | 줄 번호 표시 (line number) |
| `-v` | 패턴 **불일치** 행 출력 (invert) |
| `-l` | 파일명만 출력 (list files) |
| `-c` | 일치하는 줄 수 출력 (count) |
| `-w` | 단어 단위로 검색 (word) |
| `-A n` | 일치 행 + **이후** n줄 출력 (after) |
| `-B n` | 일치 행 + **이전** n줄 출력 (before) |
| `-C n` | 일치 행 + **앞뒤** n줄 출력 (context) |
| `-E` | 확장 정규식 사용 (extended regex) |
| `-o` | 일치하는 부분만 출력 (only matching) |
| `--color` | 일치 부분 색상 강조 |

---

## 💡 기본 사용 예시

```bash
# 파일에서 단순 검색
grep "error" app.log

# 대소문자 무시
grep -i "hello" file.txt

# 줄 번호와 함께 출력
grep -n "TODO" main.py

# 일치하지 않는 행 출력 (필터링)
grep -v "debug" app.log

# 파일명만 출력
grep -l "TODO" *.py

# 일치하는 줄 수만 출력
grep -c "error" app.log
```

---

## 📂 재귀 검색

```bash
# 디렉토리 전체에서 검색
grep -r "keyword" ./src/

# 재귀 + 줄 번호
grep -rn "TODO" ./src/

# 재귀 + 파일명만 출력
grep -rl "TODO" ./src/
```

---

## 🎯 정규식 패턴

```bash
grep "^시작"    file.txt   # 줄의 시작
grep "끝$"      file.txt   # 줄의 끝
grep "."        file.txt   # 임의의 한 문자
grep "[0-9]"    file.txt   # 숫자 포함 줄
grep "[^abc]"   file.txt   # a, b, c 제외한 문자
grep "a\{3\}"   file.txt   # a가 정확히 3번 반복

# 확장 정규식 (-E 또는 egrep)
grep -E "a+"    file.txt   # a가 1개 이상
grep -E "a?"    file.txt   # a가 0개 또는 1개
grep -E "foo|bar" file.txt # foo 또는 bar
```

---

## 🔭 컨텍스트 출력 (전후 줄)

```bash
# ERROR 이후 3줄
grep -A 3 "ERROR" log.txt

# ERROR 이전 2줄
grep -B 2 "ERROR" log.txt

# ERROR 앞뒤 2줄
grep -C 2 "ERROR" log.txt
```

---

## 🔗 파이프라인과 조합

```bash
# 프로세스 목록에서 검색
ps aux | grep "nginx"

# 특정 포트 확인
netstat -an | grep ":8080"

# 로그에서 ERROR 제외하고 출력
cat app.log | grep -v "DEBUG"

# 여러 명령 조합
cat access.log | grep "POST" | grep -v "200" | grep -c ""

# find 와 조합
find . -name "*.py" | xargs grep "import os"
```

---

## 🛠️ 실전 활용 예시

```bash
# 로그에서 에러 찾기
grep -n "ERROR\|WARN" app.log

# 특정 IP 접근 로그 확인
grep "192.168.1.1" access.log

# 함수 정의 찾기 (Python)
grep -rn "^def " ./src/

# 환경변수 확인
env | grep "PATH"

# 히스토리에서 명령어 검색
history | grep "docker"

# 일치하는 부분만 추출 (이메일)
grep -oE "[a-zA-Z0-9._%+-]+@[a-zA-Z0-9.-]+\.[a-zA-Z]{2,}" file.txt

# 빈 줄 제거
grep -v "^$" file.txt
```

---

## 📝 grep vs egrep vs fgrep

| 명령어 | 설명 | 동일한 명령 |
|--------|------|------------|
| `grep` | 기본 정규식 (BRE) | - |
| `egrep` | 확장 정규식 (ERE) | `grep -E` |
| `fgrep` | 고정 문자열 검색 (빠름) | `grep -F` |

---

## ⚡ 자주 쓰는 조합 모음

```bash
grep -rn   # 재귀 + 줄 번호 (가장 자주 쓰는 조합)
grep -rin  # 재귀 + 대소문자 무시 + 줄 번호
grep -rl   # 재귀 + 파일명만 (어떤 파일에 있는지 빠르게 확인)
grep -v    # 제외 필터 (로그 노이즈 제거)
grep -oE  # 정규식으로 특정 값만 추출
```

---

> 💬 **Tip**: `alias grep='grep --color=auto'` 를 `.bashrc`에 추가하면 항상 컬러 출력!
