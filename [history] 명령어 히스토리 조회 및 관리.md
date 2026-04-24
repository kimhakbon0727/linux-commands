# 📜 [history] 명령어 히스토리 조회 및 관리

## 📌 개요

`history`는 현재 셸 세션에서 사용한 **명령어 이력을 조회**하는 명령어입니다.  
과거에 실행한 명령어를 재사용하거나, 작업 이력을 추적할 때 유용합니다.

---

## 📐 기본 문법

```bash
history [옵션] [숫자]
```

---

## 🧪 기본 사용 예시

### 전체 히스토리 출력
```bash
history
```
```
  1  ls -al
  2  cd /etc
  3  cat passwd
  4  history
```

### 최근 N개만 출력
```bash
history 10
```
> 최근 10개의 명령어만 출력합니다.

---

## ⚙️ 주요 옵션

| 옵션 | 설명 |
|------|------|
| `history N` | 최근 N개의 명령어 출력 |
| `history -c` | 현재 세션의 히스토리 전체 삭제 |
| `history -d N` | N번째 항목 삭제 |
| `history -w` | 현재 히스토리를 파일에 저장 (`~/.bash_history`) |
| `history -r` | 히스토리 파일에서 히스토리 다시 읽기 |
| `history -a` | 현재 세션 히스토리를 파일에 추가 |

---

## 🔁 히스토리 재실행

| 단축 명령어 | 설명 |
|-------------|------|
| `!!` | 바로 직전 명령어 재실행 |
| `!N` | N번째 명령어 재실행 |
| `!문자열` | 해당 문자열로 시작하는 가장 최근 명령어 재실행 |
| `!?문자열` | 해당 문자열을 포함하는 가장 최근 명령어 재실행 |
| `^old^new` | 직전 명령어에서 old를 new로 바꿔 재실행 |

```bash
# 예시
!!                # 직전 명령어 재실행
!3                # 3번째 명령어 재실행
!grep             # grep으로 시작하는 가장 최근 명령어 재실행
^cat^less         # 직전 명령어의 cat을 less로 바꿔 재실행
```

---

## 🔍 히스토리 검색

### grep으로 필터링
```bash
history | grep "docker"
history | grep "ssh"
```

### 역방향 검색 (대화형)
```bash
Ctrl + R
```
> 입력창이 나타나면 검색어를 타이핑하면 실시간으로 매칭되는 명령어를 찾아줍니다.  
> `Enter` → 실행 / `Ctrl + R` 반복 → 이전 결과 / `Ctrl + G` → 취소

---

## 💾 히스토리 파일

기본적으로 히스토리는 `~/.bash_history` 파일에 저장됩니다.

```bash
cat ~/.bash_history        # 히스토리 파일 직접 확인
wc -l ~/.bash_history      # 총 히스토리 라인 수 확인
```

---

## 🛠️ 환경변수 설정 (`~/.bashrc` 또는 `~/.bash_profile`)

```bash
# 저장할 히스토리 개수 설정
HISTSIZE=10000          # 메모리에 유지할 히스토리 수
HISTFILESIZE=20000      # 파일에 저장할 히스토리 수

# 중복 명령어 제거
HISTCONTROL=ignoredups   # 연속 중복 제거
HISTCONTROL=ignoreboth   # 중복 + 공백으로 시작하는 명령 제거

# 특정 명령어 히스토리에서 제외
HISTIGNORE="ls:ll:clear:history"

# 히스토리에 타임스탬프 추가
HISTTIMEFORMAT="%Y-%m-%d %H:%M:%S "
```

설정 적용:
```bash
source ~/.bashrc
```

타임스탬프 설정 후 출력 예시:
```
  1  2025-04-22 09:10:05  ls -al
  2  2025-04-22 09:10:12  cd /var/log
  3  2025-04-22 09:11:03  tail -f syslog
```

---

## 🗑️ 히스토리 삭제

```bash
history -c              # 현재 세션 히스토리 전체 삭제
history -d 5            # 5번째 항목만 삭제
history -w              # 삭제 내용을 파일에 반영

# 히스토리 파일 자체를 비우기
> ~/.bash_history
```

---

## 💡 활용 팁

```bash
# 히스토리를 번호 없이 출력 후 파일로 저장
history | awk '{$1=""; print $0}' > my_commands.txt

# 가장 많이 쓴 명령어 Top 10 분석
history | awk '{print $2}' | sort | uniq -c | sort -rn | head -10

# 히스토리 공유 없이 명령어 실행 (앞에 공백 추가)
 export SECRET_KEY=abc123   # 공백으로 시작하면 히스토리에 저장 안 됨
# (HISTCONTROL=ignorespace 또는 ignoreboth 설정 필요)
```

---

## ⚠️ 주의사항

- 패스워드나 민감한 정보가 포함된 명령어는 히스토리에 남을 수 있으므로 주의합니다.
- `history -c`는 현재 세션 메모리만 지우므로, 파일에도 반영하려면 `history -w`를 함께 실행합니다.
- 여러 터미널 세션을 동시에 사용하면 세션 종료 순서에 따라 히스토리가 덮어써질 수 있습니다.
