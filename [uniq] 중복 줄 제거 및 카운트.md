# 🪄 uniq

**연속된 중복 줄을 제거하거나 카운트**하는 명령어.  
반드시 `sort` 후 사용해야 비연속 중복도 처리됨.

---

## 기본 문법

```bash
uniq [옵션] [입력파일] [출력파일]
```

---

## 기본 사용

```bash
# 연속된 중복 줄 제거
uniq file.txt

# 정렬 후 전체 중복 제거 (일반적인 사용법)
sort file.txt | uniq

# 결과를 파일로 저장
sort file.txt | uniq > result.txt
```

---

## 중복 관련 필터

```bash
# 중복된 줄만 출력 (1번 이상 반복된 줄)
sort file.txt | uniq -d

# 중복되지 않은 줄만 출력 (딱 1번만 등장한 줄)
sort file.txt | uniq -u
```

---

## 카운트

```bash
# 각 줄의 등장 횟수 출력
sort file.txt | uniq -c

# 빈도 높은 순으로 정렬
sort file.txt | uniq -c | sort -rn

# 상위 5개만 출력
sort file.txt | uniq -c | sort -rn | head -5
```

---

## 대소문자 / 부분 비교

```bash
# 대소문자 구분 없이 중복 제거
sort file.txt | uniq -i

# 앞 N개 문자만 비교해서 중복 처리
sort file.txt | uniq -w 5

# 앞 N개 필드(공백 구분) 건너뛰고 비교
sort file.txt | uniq -f 2
```

---

## 자주 쓰는 조합

```bash
# 로그에서 접속 IP 빈도 분석
awk '{print $1}' access.log | sort | uniq -c | sort -rn | head -20

# 특정 단어 등장 횟수 세기
grep -o '단어' file.txt | sort | uniq -c

# 중복 없는 고유 값 목록 추출
cut -d',' -f1 data.csv | sort | uniq

# 중복 파일명 찾기
find . -type f | xargs basename -a | sort | uniq -d
```

---

## 주요 옵션 정리

| 옵션 | 설명 |
|------|------|
| `-c` | 각 줄 등장 횟수 앞에 출력 |
| `-d` | 중복된 줄만 출력 |
| `-u` | 중복되지 않은 줄만 출력 |
| `-i` | 대소문자 구분 없이 비교 |
| `-w N` | 앞 N개 문자만 비교 |
| `-f N` | 앞 N개 필드 건너뛰고 비교 |

---

## 팁

- `uniq` 는 **인접한 줄만 비교**하므로 `sort` 없이 쓰면 전체 중복을 제거하지 못함
- `-c` 옵션 출력은 앞에 공백이 포함되므로 `awk '{print $1}'` 로 숫자만 추출 가능
- `sort -u` 가 `sort | uniq` 보다 성능이 약간 더 좋음
