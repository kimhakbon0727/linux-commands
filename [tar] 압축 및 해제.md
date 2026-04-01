# 🗜️ tar (Tape Archive) 명령어 치트시트

> 파일 또는 폴더를 **압축**하거나 **해제**하는 리눅스/유닉스 명령어

---

## 📌 기본 문법

```
tar [동작] [옵션] [아카이브 파일] [대상 파일/폴더...]
```

---

## ⚙️ 동작 옵션 (하나만 선택)

| 옵션 | 설명 |
|------|------|
| `-c` | 아카이브 생성 (create) |
| `-x` | 아카이브 해제 (extract) |
| `-t` | 아카이브 내용 목록 출력 (list) |
| `-r` | 기존 아카이브에 파일 추가 (append) |
| `-u` | 변경된 파일만 업데이트 (update) |

---

## ⚙️ 보조 옵션

| 옵션 | 설명 |
|------|------|
| `-f` | 아카이브 파일명 지정 (file) ← **항상 필요** |
| `-v` | 처리되는 파일 이름 출력 (verbose) |
| `-z` | gzip 압축/해제 (`.tar.gz`, `.tgz`) |
| `-j` | bzip2 압축/해제 (`.tar.bz2`) |
| `-J` | xz 압축/해제 (`.tar.xz`) |
| `-C` | 압축 해제할 경로 지정 |
| `-p` | 원본 파일 권한 유지 (preserve) |
| `--exclude` | 특정 파일/폴더 제외 |

---

## 💡 실전 예시

### 압축 생성
```bash
# 폴더를 tar로 묶기 (압축 없음)
tar -cvf archive.tar myfolder/

# gzip 압축 (.tar.gz)
tar -czvf archive.tar.gz myfolder/

# bzip2 압축 (.tar.bz2) — gzip보다 압축률 높음
tar -cjvf archive.tar.bz2 myfolder/

# xz 압축 (.tar.xz) — 가장 높은 압축률
tar -cJvf archive.tar.xz myfolder/

# 여러 파일/폴더를 하나로 묶기
tar -czvf archive.tar.gz file1.txt file2.txt folder/

# 특정 파일/폴더 제외하고 압축
tar -czvf archive.tar.gz myfolder/ --exclude="myfolder/logs" --exclude="*.tmp"
```

### 압축 해제
```bash
# 현재 위치에 해제
tar -xzvf archive.tar.gz

# 특정 경로에 해제
tar -xzvf archive.tar.gz -C /home/user/dest/

# tar만 해제 (압축 없는 경우)
tar -xvf archive.tar

# bzip2 해제
tar -xjvf archive.tar.bz2

# xz 해제
tar -xJvf archive.tar.xz
```

### 내용 확인 (해제 없이)
```bash
# 아카이브 안의 파일 목록 출력
tar -tvf archive.tar.gz

# 특정 파일만 확인
tar -tvf archive.tar.gz | grep ".txt"
```

### 특정 파일만 해제
```bash
# 아카이브에서 특정 파일만 꺼내기
tar -xzvf archive.tar.gz myfolder/config.txt
```

---

## 📦 압축 형식 비교

| 형식 | 확장자 | 옵션 | 압축률 | 속도 |
|------|--------|------|--------|------|
| 압축 없음 | `.tar` | (없음) | ❌ | ⚡⚡⚡ |
| gzip | `.tar.gz` / `.tgz` | `-z` | ⭐⭐ | ⚡⚡ |
| bzip2 | `.tar.bz2` | `-j` | ⭐⭐⭐ | ⚡ |
| xz | `.tar.xz` | `-J` | ⭐⭐⭐⭐ | 🐢 |

> 💡 일반적으로 **속도 중시 → `.tar.gz`**, **용량 중시 → `.tar.xz`**

---

## 🔤 명령어 외우는 법

```
tar -czvf  →  "Create Zip Verbose File"   (압축 생성)
tar -xzvf  →  "eXtract Zip Verbose File"  (압축 해제)
tar -tzvf  →  "lisT Zip Verbose File"     (목록 확인)
```

---

## ⚠️ 주의사항

- `-f` 옵션은 **항상 마지막**에 위치해야 하며, 바로 뒤에 파일명이 와야 함
- 해제 경로(`-C`)를 지정하지 않으면 **현재 디렉토리**에 풀림
- 압축 해제 전 `-t` 옵션으로 내용을 먼저 확인하는 습관 권장

```bash
# 해제 전 내용 먼저 확인
tar -tzvf archive.tar.gz

# 확인 후 해제
tar -xzvf archive.tar.gz -C /원하는/경로/
```
