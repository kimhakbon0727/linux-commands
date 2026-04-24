# 🔄 tr

**문자를 변환(translate)하거나 삭제**하는 명령어.  
파일이 아닌 표준 입력(stdin)만 처리하므로 파이프와 함께 사용.

---

## 기본 문법

```bash
tr [옵션] SET1 [SET2]
```

- `SET1` → 변환 대상 문자 집합
- `SET2` → 변환될 문자 집합

---

## 문자 변환

```bash
# 소문자 → 대문자
echo "hello world" | tr 'a-z' 'A-Z'
# 출력: HELLO WORLD

# 대문자 → 소문자
echo "HELLO WORLD" | tr 'A-Z' 'a-z'
# 출력: hello world

# 특정 문자 변환
echo "hello" | tr 'el' 'EL'
# 출력: hELLo

# 공백을 밑줄로 변환
echo "hello world" | tr ' ' '_'
# 출력: hello_world

# ':' 를 줄바꿈으로 변환
echo "a:b:c:d" | tr ':' '\n'
# 출력:
# a
# b
# c
# d
```

---

## 문자 삭제 (-d)

```bash
# 특정 문자 삭제
echo "hello 123 world" | tr -d '0-9'
# 출력: hello  world

# 공백 삭제
echo "h e l l o" | tr -d ' '
# 출력: hello

# 줄바꿈 삭제 (여러 줄 → 한 줄)
cat file.txt | tr -d '\n'

# 특수문자 삭제
echo "hello!@#world" | tr -d '!@#'
# 출력: helloworld
```

---

## 연속된 문자 압축 (-s)

```bash
# 연속된 공백을 하나로 압축
echo "hello    world" | tr -s ' '
# 출력: hello world

# 연속된 줄바꿈을 하나로 압축
cat file.txt | tr -s '\n'

# 연속된 특정 문자 압축
echo "aaabbbccc" | tr -s 'a-z'
# 출력: abc
```

---

## 문자 집합 보수 (-c, 지정한 문자 외 모두 처리)

```bash
# 숫자 외 모든 문자 삭제
echo "abc123def456" | tr -cd '0-9'
# 출력: 123456

# 알파벳과 숫자 외 모두 삭제
echo "hello, world! 123" | tr -cd 'a-zA-Z0-9\n'
# 출력: helloworld123

# 출력 가능한 문자 외 모두 삭제
cat binary_file | tr -cd '[:print:]\n'
```

---

## 문자 클래스

```bash
# 대문자 → 소문자
tr '[:upper:]' '[:lower:]'

# 소문자 → 대문자
tr '[:lower:]' '[:upper:]'

# 숫자만 삭제
tr -d '[:digit:]'

# 공백류(스페이스, 탭 등) 압축
tr -s '[:space:]'
```

| 클래스 | 설명 |
|--------|------|
| `[:alpha:]` | 알파벳 (a-z, A-Z) |
| `[:digit:]` | 숫자 (0-9) |
| `[:alnum:]` | 알파벳 + 숫자 |
| `[:upper:]` | 대문자 |
| `[:lower:]` | 소문자 |
| `[:space:]` | 공백류 (스페이스, 탭, 줄바꿈 등) |
| `[:print:]` | 출력 가능한 문자 |
| `[:punct:]` | 구두점 |

---

## 자주 쓰는 조합

```bash
# CSV → 줄바꿈으로 변환
echo "a,b,c,d" | tr ',' '\n'

# 파일 내 단어를 한 줄씩 출력
cat file.txt | tr ' ' '\n' | sort | uniq -c | sort -rn

# 대소문자 구분 없이 처리
cat file.txt | tr '[:upper:]' '[:lower:]' | grep "keyword"

# 줄바꿈 제거 후 한 줄로 합치기
cat file.txt | tr '\n' ' '

# 탭을 공백으로 변환
cat file.txt | tr '\t' ' '
```

---

## 주요 옵션 정리

| 옵션 | 설명 |
|------|------|
| `-d SET` | SET에 속한 문자 삭제 |
| `-s SET` | SET에 속한 연속된 문자를 하나로 압축 |
| `-c SET` | SET의 보수 (SET에 없는 문자들) 대상으로 처리 |

---

## 팁

- `tr` 은 파일을 직접 읽지 못하므로 항상 `cat file | tr` 또는 `tr ... < file` 형태로 사용
- 정규표현식을 지원하지 않아 복잡한 패턴은 `sed` 사용 권장
- 단순 문자 치환/삭제는 `tr` 이 `sed` 보다 빠름
