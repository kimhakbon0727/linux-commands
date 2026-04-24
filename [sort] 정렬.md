# 🔤 sort

텍스트 파일이나 표준 입력의 **줄(line) 단위로 정렬**하는 명령어.

---

## 기본 문법

```bash
sort [옵션] [파일]
```

---

## 기본 사용

```bash
# 알파벳 오름차순 정렬 (기본)
sort file.txt

# 내림차순 정렬
sort -r file.txt

# 정렬 결과를 파일로 저장
sort file.txt -o sorted.txt

# 여러 파일 합쳐서 정렬
sort file1.txt file2.txt
```

---

## 숫자 정렬

```bash
# 숫자로 정렬 (기본은 문자열 정렬이라 10이 2보다 앞에 옴)
sort -n numbers.txt

# 숫자 내림차순
sort -rn numbers.txt

# 사람이 읽기 쉬운 단위 정렬 (1K, 2M, 3G 등)
sort -h sizes.txt
```

---

## 특정 열 기준 정렬

```bash
# 2번째 열 기준 정렬 (공백 구분)
sort -k2 file.txt

# 2번째 열 숫자 기준 정렬
sort -k2n file.txt

# 3번째 열 내림차순 정렬
sort -k3r file.txt

# 구분자를 ':'로 지정하고 2번째 열 기준 정렬
sort -t: -k2 /etc/passwd
```

---

## 중복 제거

```bash
# 정렬 후 중복 줄 제거
sort -u file.txt

# uniq 와 동일한 효과
sort file.txt | uniq
```

---

## 파일 비교 / 병합

```bash
# 이미 정렬된 파일 2개를 병합 정렬
sort -m sorted1.txt sorted2.txt

# 정렬 여부 확인 (정렬되어 있으면 아무 출력 없음)
sort -c file.txt
```

---

## 자주 쓰는 조합

```bash
# 프로세스 CPU 사용량 상위 10개
ps aux | sort -k3rn | head -10

# 디렉토리 용량 큰 순서로 정렬
du -sh * | sort -rh

# 로그에서 IP 주소 빈도 순 정렬
awk '{print $1}' access.log | sort | uniq -c | sort -rn | head -20

# 단어 빈도 분석
cat file.txt | tr ' ' '\n' | sort | uniq -c | sort -rn
```

---

## 주요 옵션 정리

| 옵션 | 설명 |
|------|------|
| `-r` | 내림차순 정렬 |
| `-n` | 숫자 정렬 |
| `-h` | 사람이 읽기 쉬운 단위 정렬 (1K, 2M 등) |
| `-u` | 중복 제거 후 정렬 |
| `-k N` | N번째 열 기준 정렬 |
| `-t` | 열 구분자 지정 |
| `-o` | 결과를 파일로 저장 |
| `-m` | 정렬된 파일 병합 |
| `-c` | 정렬 여부 확인 |
| `-f` | 대소문자 구분 없이 정렬 |

---

## 팁

- 기본 정렬은 **문자열(사전순)** 이므로 숫자는 반드시 `-n` 옵션 필요
- `sort -u` 는 `sort | uniq` 보다 빠름
- 대용량 파일 정렬 시 `--parallel=N` 옵션으로 멀티코어 활용 가능
