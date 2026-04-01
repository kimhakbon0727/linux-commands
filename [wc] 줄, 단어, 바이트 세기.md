# 🔢 wc (Word Count) 명령어 치트시트
> 파일의 **줄 수 / 단어 수 / 바이트 수**를 세는 리눅스/유닉스 명령어

---

## 📌 기본 문법
```bash
wc [옵션] [파일...]
```

---

## ⚙️ 주요 옵션
| 옵션 | 설명 |
|---|---|
| `-l` | 줄 수 (line) |
| `-w` | 단어 수 (word) |
| `-c` | 바이트 수 (byte) |
| `-m` | 문자 수 (character) |

---

## 💡 기본 사용 예시
```bash
# 줄 수만 출력
wc -l file.txt

# 단어 수만 출력
wc -w file.txt

# 바이트 수만 출력
wc -c file.txt

# 옵션 없이 실행 → 줄/단어/바이트 한번에 출력
wc file.txt
# 출력 예: 42 318 2048 file.txt
#          줄  단어 바이트 파일명
```

---

## 🔗 파이프라인과 조합
```bash
# grep 결과 개수 확인
grep "error" app.log | wc -l

# 디렉토리 내 파일 개수 확인
ls | wc -l

# 특정 프로세스 개수 확인
ps aux | grep "nginx" | wc -l
```

---

## 🛠️ 실전 활용 예시
```bash
# 검색 결과 파일 줄 수 확인 (항목 몇 개인지)
wc -l daum_to_kakao_targets.txt

# 여러 파일 한번에
wc -l *.log

# 가장 긴 줄 길이 확인 (최대 바이트)
wc -L file.txt
```
