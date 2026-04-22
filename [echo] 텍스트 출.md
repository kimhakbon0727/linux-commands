# 📢 Linux `echo` 명령어 완벽 정리

> 텍스트 출력, 변수 확인, 파일 저장까지 활용도 높은 `echo` 명령어 가이드

---

## 📋 목차

- [개요](#-개요)
- [기본 문법](#-기본-문법)
- [주요 옵션](#-주요-옵션)
- [사용 예시](#-사용-예시)
- [특수 문자 이스케이프 (-e 옵션)](#-특수-문자-이스케이프--e-옵션)
- [변수와 함께 사용](#-변수와-함께-사용)
- [파일 저장과 함께 사용](#-파일-저장과-함께-사용)
- [작은따옴표 vs 큰따옴표](#-작은따옴표-vs-큰따옴표)
- [자주 쓰는 패턴](#-자주-쓰는-패턴)

---

## 🔍 개요

`echo`는 인자로 전달된 **텍스트나 변수 값을 화면에 출력**하는 명령어입니다.  
단순 출력 외에도 **파일 생성, 변수 확인, 스크립트 디버깅** 등 다양한 용도로 활용됩니다.

| 항목 | 내용 |
|------|------|
| 명령어 | `echo` |
| 위치 | `/bin/echo` (외부), `builtin echo` (쉘 내장) |
| 주 용도 | 텍스트 출력, 변수 확인, 파일 내용 작성 |

---

## 📌 기본 문법

```bash
echo [옵션] [문자열]
```

---

## ⚙️ 주요 옵션

| 옵션 | 설명 |
|------|------|
| (없음) | 문자열 출력 후 자동 줄바꿈 |
| `-n` | 출력 후 줄바꿈 **없음** |
| `-e` | 백슬래시 이스케이프 문자 **해석** |
| `-E` | 백슬래시 이스케이프 문자 해석 **안 함** (기본값) |

---

## 💡 사용 예시

### 1️⃣ 기본 텍스트 출력

```bash
$ echo "Hello, World!"
Hello, World!

$ echo Hello World
Hello World
```

---

### 2️⃣ 줄바꿈 없이 출력 (`-n`)

```bash
$ echo -n "Hello"
Hello$                  # 줄바꿈 없이 프롬프트가 바로 이어짐

# 활용 예: 입력 프롬프트처럼 보이게
$ echo -n "이름을 입력하세요: "
이름을 입력하세요: 
```

---

## 🔤 특수 문자 이스케이프 (`-e` 옵션)

`-e` 옵션을 사용하면 아래 이스케이프 문자를 해석합니다.

| 이스케이프 | 의미 |
|-----------|------|
| `\n` | 줄바꿈 (newline) |
| `\t` | 탭 (tab) |
| `\r` | 캐리지 리턴 |
| `\\` | 백슬래시 (`\`) |
| `\a` | 경고음 (bell) |
| `\b` | 백스페이스 |
| `\c` | 이후 출력 없음 (줄바꿈 포함 억제) |

```bash
# 줄바꿈
$ echo -e "Line1\nLine2\nLine3"
Line1
Line2
Line3

# 탭 정렬
$ echo -e "Name:\tAlice"
Name:    Alice

# 혼합 사용
$ echo -e "=====\n항목1\n항목2\n====="
=====
항목1
항목2
=====
```

---

## 🔧 변수와 함께 사용

### 환경변수 출력

```bash
$ echo $HOME
/home/alice

$ echo $USER
alice

$ echo $PATH
/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin

$ echo $SHELL
/bin/bash
```

### 사용자 정의 변수 출력

```bash
$ NAME="Alice"
$ AGE=30

$ echo $NAME
Alice

$ echo "이름: $NAME, 나이: $AGE"
이름: Alice, 나이: 30
```

### 명령어 결과를 변수로 출력 (명령어 치환)

```bash
$ echo "현재 날짜: $(date)"
현재 날짜: Wed Apr 22 10:00:00 KST 2026

$ echo "현재 경로: $(pwd)"
현재 경로: /home/alice

$ echo "파일 수: $(ls | wc -l)"
파일 수: 15
```

---

## 💾 파일 저장과 함께 사용

### `>` 덮어쓰기

```bash
# 파일 새로 생성 또는 덮어쓰기
$ echo "Hello, World!" > hello.txt

$ cat hello.txt
Hello, World!
```

### `>>` 추가(append)

```bash
# 기존 파일 내용 뒤에 추가
$ echo "첫 번째 줄" > log.txt
$ echo "두 번째 줄" >> log.txt
$ echo "세 번째 줄" >> log.txt

$ cat log.txt
첫 번째 줄
두 번째 줄
세 번째 줄
```

### 환경변수 설정 파일에 추가

```bash
# .bashrc에 환경변수 추가
$ echo 'export MY_VAR="hello"' >> ~/.bashrc

# PATH에 경로 추가
$ echo 'export PATH=$PATH:/usr/local/myapp/bin' >> ~/.bashrc
```

---

## 🔑 작은따옴표 vs 큰따옴표

| 구분 | 변수 해석 | 특수문자 해석 | 용도 |
|------|----------|-------------|------|
| `"큰따옴표"` | ✅ 해석함 | 일부 해석 | 변수 값을 포함할 때 |
| `'작은따옴표'` | ❌ 해석 안 함 | 해석 안 함 | 문자 그대로 출력할 때 |

```bash
$ NAME="Alice"

# 큰따옴표: 변수 해석 O
$ echo "Hello, $NAME"
Hello, Alice

# 작은따옴표: 변수 해석 X
$ echo 'Hello, $NAME'
Hello, $NAME

# 이스케이프로 $ 문자 그대로 출력
$ echo "가격: \$100"
가격: $100
```

---

## 🛠️ 자주 쓰는 패턴

### 스크립트 디버깅 메시지

```bash
#!/bin/bash
echo "=== 배포 시작 ==="
echo "서버: $(hostname)"
echo "시간: $(date)"
echo "작업 디렉토리: $(pwd)"
```

### 빠른 파일 생성

```bash
# 설정 파일 빠르게 생성
$ echo "[server]\nhost=localhost\nport=8080" > config.ini
```

### 로그 기록

```bash
$ echo "[$(date '+%Y-%m-%d %H:%M:%S')] 작업 완료" >> deploy.log
```

### 줄 구분선 출력

```bash
$ echo "================================"
$ echo -e "\n===== 결과 =====\n"
```

### 조건 확인

```bash
# 변수가 비어있는지 확인
$ MY_VAR=""
$ echo ${MY_VAR:-"값이 없습니다"}
값이 없습니다

$ MY_VAR="hello"
$ echo ${MY_VAR:-"값이 없습니다"}
hello
```

---

## 📝 요약

```
기본 출력                →  echo "텍스트"
줄바꿈 없이 출력         →  echo -n "텍스트"
이스케이프 문자 해석      →  echo -e "줄1\n줄2"
변수 출력                →  echo $변수명
명령어 결과 출력         →  echo "$(명령어)"
파일에 덮어쓰기          →  echo "내용" > 파일명
파일에 내용 추가         →  echo "내용" >> 파일명
```

---

> 📌 **참고:** `echo`는 쉘 내장(builtin) 명령어와 외부(`/bin/echo`) 두 가지가 있으며, 동작이 약간 다를 수 있습니다.  
> 스크립트에서 이식성이 중요하다면 `printf` 사용을 고려해 보세요.
