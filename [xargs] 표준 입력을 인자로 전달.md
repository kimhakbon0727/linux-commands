# 🔗 xargs

**표준 입력(stdin)으로 받은 값을 다른 명령어의 인자로 전달**하는 명령어.  
파이프(`|`)만으로 인자 전달이 안 될 때 사용.

---

## 기본 문법

```bash
명령어 | xargs [옵션] [실행할 명령어]
```

---

## 기본 사용

```bash
# 파이프 결과를 echo의 인자로 전달
echo "file1 file2 file3" | xargs echo

# 파일 목록을 rm에 전달
find . -name "*.log" | xargs rm

# 파일 목록을 cat에 전달
ls *.txt | xargs cat
```

---

## 한 번에 처리할 인자 수 제한 (-n)

```bash
# 한 번에 1개씩 처리
echo "a b c d" | xargs -n1 echo

# 한 번에 2개씩 처리
echo "a b c d" | xargs -n2 echo
# 출력:
# a b
# c d
```

---

## 인자 위치 지정 (-I)

```bash
# {} 자리에 입력값 삽입
ls *.txt | xargs -I{} cp {} /backup/{}

# 다른 플레이스홀더도 가능
find . -name "*.log" | xargs -I FILE mv FILE FILE.bak

# 명령어 앞뒤로 인자 활용
echo "world" | xargs -I{} echo "hello {}"
# 출력: hello world
```

---

## 병렬 처리 (-P)

```bash
# 4개 프로세스로 병렬 처리
find . -name "*.png" | xargs -P4 -I{} convert {} {}.jpg

# CPU 코어 수만큼 병렬 처리
find . -name "*.gz" | xargs -P$(nproc) -I{} gzip -d {}
```

---

## 공백/특수문자 처리 (-0)

```bash
# 파일명에 공백이 있을 경우 null 구분자 사용
find . -name "*.txt" -print0 | xargs -0 rm

# find 의 -print0 과 xargs -0 은 세트로 사용
find . -type f -print0 | xargs -0 chmod 644
```

> 파일명에 공백이 있으면 일반 `|` 로는 처리 오류 발생 → `-print0 | xargs -0` 으로 해결

---

## 빈 입력 무시 (-r)

```bash
# 입력이 없을 때 명령어 실행 안 함 (GNU xargs)
find . -name "*.tmp" | xargs -r rm
```

---

## 실행 전 확인 (-p)

```bash
# 각 명령 실행 전 확인 프롬프트 표시
find . -name "*.bak" | xargs -p rm
```

---

## 자주 쓰는 조합

```bash
# 특정 확장자 파일 일괄 삭제
find . -name "*.log" | xargs rm -f

# 파일명에 공백 있어도 안전하게 삭제
find . -name "*.log" -print0 | xargs -0 rm -f

# 여러 파일에서 특정 문자열 검색
find . -name "*.py" | xargs grep "import os"

# 파일 권한 일괄 변경
find . -type f -name "*.sh" | xargs chmod +x

# 오래된 도커 이미지 삭제
docker images -q --filter "dangling=true" | xargs docker rmi

# 여러 파일 한꺼번에 압축
find . -name "*.log" | xargs tar -czf logs.tar.gz

# 패키지 목록 파일로 일괄 설치
cat packages.txt | xargs apt install -y
```

---

## 주요 옵션 정리

| 옵션 | 설명 |
|------|------|
| `-n N` | 한 번에 N개 인자씩 처리 |
| `-I {}` | 인자 삽입 위치 지정 |
| `-P N` | N개 프로세스로 병렬 처리 |
| `-0` | null 문자를 구분자로 사용 (공백 파일명 대응) |
| `-r` | 입력 없으면 실행 안 함 |
| `-p` | 실행 전 확인 프롬프트 |
| `-t` | 실행할 명령어를 화면에 출력 (디버그용) |

---

## 파이프 vs xargs

```bash
# 파이프는 stdout을 다음 명령의 stdin으로 전달
ls | grep ".txt"       # grep은 stdin으로 읽음 → OK

# 일부 명령은 stdin이 아닌 인자(argument)로만 받음
ls | rm                # rm은 인자가 필요 → 에러
ls *.txt | xargs rm    # xargs로 인자로 변환 → OK
```

---

## 팁

- 파일명에 **공백이나 특수문자**가 있을 수 있으면 항상 `-print0 | xargs -0` 조합 사용
- `-P` 옵션으로 병렬 처리 시 대용량 파일 작업 속도를 크게 향상 가능
- `-t` 옵션으로 실제 실행되는 명령어를 확인하면 디버깅에 유용
