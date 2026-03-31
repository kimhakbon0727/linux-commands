# 🐱 파일 내용 출력 - cat 명령어

> 파일의 **내용을 터미널에 출력**하거나 **파일을 합치는** 명령어  
> 단순하지만 파이프(`|`)와 조합하면 강력한 텍스트 처리 도구

---

## 📌 기본 문법

```bash
cat [옵션] [파일명]
```

---

## ⚙️ 주요 옵션

| 옵션 | 설명 |
|------|------|
| `-n` | 모든 줄에 줄 번호 표시 |
| `-b` | 내용이 있는 줄에만 줄 번호 표시 (빈 줄 제외) |
| `-A` | 줄 끝(`$`)과 탭(`^I`) 등 특수문자 표시 |
| `-T` | 탭 문자를 `^I`로 표시 |
| `-E` | 각 줄 끝에 `$` 표시 |
| `-s` | 연속된 빈 줄을 하나로 압축 |
| `-v` | 출력 불가 문자를 `^` 형태로 표시 |

---

## 💡 기본 사용 예시

```bash
# 파일 내용 출력
cat filename.txt

# 줄 번호와 함께 출력
cat -n filename.txt

# 내용 있는 줄만 번호 표시
cat -b filename.txt

# 줄 끝 표시 (공백 확인 시 유용)
cat -A filename.txt

# 여러 파일 동시 출력
cat file1.txt file2.txt

# 연속 빈 줄 압축해서 출력
cat -s filename.txt
```

---

## 🖥️ 출력 화면 예시

```bash
$ cat -n filename.txt
     1  # 서버 설정
     2
     3  server_name=web01
     4  port=8080
     5  timeout=30
```

```bash
$ cat -A filename.txt
# 서버 설정$
$
server_name=web01$
port=8080$         ← 줄 끝 공백 확인 가능
timeout=30$
```

> 💡 `-A` 옵션은 **줄 끝 불필요한 공백**이나 **탭 문자** 확인 시 매우 유용합니다

---

## 🛠️ 실전 활용 예시

### 📄 파일 내용 확인

```bash
# 로그 파일 확인
cat /var/log/syslog

# 시스템 정보 확인
cat /etc/os-release
cat /proc/cpuinfo
cat /proc/meminfo

# 네트워크 설정 확인
cat /etc/hosts
cat /etc/resolv.conf
```

---

### ✍️ 파일 생성 & 내용 작성

```bash
# 새 파일 생성 후 내용 입력 (Ctrl+D로 저장 종료)
cat > newfile.txt
첫 번째 줄
두 번째 줄
Ctrl+D

# 기존 파일에 내용 추가 (>> 사용)
cat >> newfile.txt
추가할 내용
Ctrl+D
```

---

### 🔗 파일 합치기 (Concatenate)

```bash
# 두 파일 합쳐서 새 파일로 저장
cat file1.txt file2.txt > merged.txt

# 여러 파일 순서대로 합치기
cat header.txt body.txt footer.txt > full_document.txt

# 파일 내용 + 다른 파일에 추가
cat newdata.txt >> existing.txt
```

---

### 🔍 파이프와 조합

```bash
# 파일 내용에서 특정 단어 검색
cat filename.txt | grep "error"

# 줄 수 세기
cat filename.txt | wc -l

# 내용 정렬
cat filename.txt | sort

# 중복 줄 제거
cat filename.txt | sort | uniq

# 특정 줄만 출력 (2~5번 줄)
cat -n filename.txt | sed -n '2,5p'

# 대용량 파일 페이지 단위로 보기
cat filename.txt | more
cat filename.txt | less
```

---

### 🖥️ 리다이렉션 활용

```bash
# 파일 내용을 다른 파일로 복사
cat source.txt > destination.txt

# 여러 파일 합쳐서 저장
cat *.log > all_logs.txt

# 파일 내용 추가 (덮어쓰기 아님)
cat newfile.txt >> existing.txt

# 빈 파일 생성
cat /dev/null > emptyfile.txt
```

---

## 🔁 cat vs 다른 명령어 비교

| 명령어 | 용도 | 특징 |
|--------|------|------|
| `cat` | 파일 전체 출력 | 빠르고 간단, 전체 한 번에 출력 |
| `more` | 페이지 단위 출력 | 스페이스로 다음 페이지, 뒤로 못 감 |
| `less` | 페이지 단위 출력 | 위아래 자유롭게 스크롤 가능 ✅ |
| `head` | 파일 앞부분 출력 | 기본 10줄, `-n`으로 줄 수 지정 |
| `tail` | 파일 뒷부분 출력 | 로그 실시간 모니터링(`-f`) 가능 |
| `tac` | 역순 출력 | cat의 반대 (마지막 줄부터 출력) |
| `grep` | 특정 패턴 검색 | 파이프와 조합해서 많이 사용 |

```bash
# 대용량 파일은 cat 대신 less 권장
less /var/log/syslog

# 로그 실시간 모니터링은 tail -f
tail -f /var/log/syslog

# 파일 첫 10줄만 확인
head -n 10 filename.txt
```

---

## ⚡ 자주 쓰는 조합 모음

```bash
cat -n filename.txt           # 줄 번호와 함께 출력
cat -A filename.txt           # 숨겨진 공백·탭 확인
cat file1.txt file2.txt > merged.txt   # 파일 합치기
cat filename.txt | grep "키워드"       # 특정 단어 검색
cat filename.txt | wc -l              # 전체 줄 수 확인
cat /dev/null > filename.txt          # 파일 내용 비우기
cat >> filename.txt                   # 파일에 내용 추가 입력
```

---

## ⚠️ 주의사항

```bash
# ❌ 대용량 파일에 cat 사용 (터미널이 멈출 수 있음)
cat /var/log/huge_file.log

# ✅ 대신 less 사용
less /var/log/huge_file.log

# ❌ > 리다이렉션 실수 (파일이 덮어써짐!)
cat newdata.txt > important.txt   # 기존 내용 사라짐!

# ✅ 추가할 때는 >> 사용
cat newdata.txt >> important.txt  # 기존 내용 유지하며 추가
```

---

> 💬 **Tip**: `cat` 이름은 **con-cat-enate(연결하다)** 의 줄임말입니다.  
> 파일 내용 출력보다 **여러 파일을 하나로 합치는** 것이 원래 목적이에요!
> ```bash
> # /etc/os-release 로 현재 리눅스 버전 빠르게 확인
> cat /etc/os-release
>
> # 자주 보는 시스템 파일들
> cat /proc/cpuinfo    # CPU 정보
> cat /proc/meminfo    # 메모리 정보
> cat /etc/passwd      # 사용자 계정 목록
> cat /etc/hosts       # 호스트 설정
> ```
