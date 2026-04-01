# ✂️ sed (Stream Editor) 명령어 치트시트
> 텍스트 스트림을 라인 단위로 읽어 치환, 삭제, 삽입 등을 수행하는 리눅스/유닉스 명령어

---

## 📌 기본 문법
```
sed [옵션] '명령어' <파일>
sed [옵션] -e '명령어1' -e '명령어2' <파일>
```

---

## ⚙️ 주요 옵션
| 옵션 | 설명 |
|------|------|
| `-i` | 파일 직접 수정 (in-place) |
| `-i.bak` | 원본 백업 후 직접 수정 |
| `-e` | 명령어 여러 개 지정 |
| `-n` | 자동 출력 억제 (p 명령어와 함께 사용) |
| `-r`, `-E` | 확장 정규표현식 사용 |
| `-f` | 스크립트 파일에서 명령어 읽기 |

---

## 🔧 주요 명령어
| 명령어 | 설명 |
|--------|------|
| `s/찾기/바꾸기/` | 치환 (substitution) |
| `d` | 라인 삭제 (delete) |
| `p` | 라인 출력 (print) |
| `i\텍스트` | 해당 라인 앞에 삽입 (insert) |
| `a\텍스트` | 해당 라인 뒤에 추가 (append) |
| `c\텍스트` | 해당 라인을 텍스트로 교체 (change) |
| `q` | 해당 라인에서 종료 (quit) |
| `y/문자열1/문자열2/` | 문자 단위 변환 (translate) |
| `=` | 라인 번호 출력 |

---

## 🔀 치환 플래그 (s 명령어)
| 플래그 | 설명 |
|--------|------|
| `g` | 라인 내 모든 매칭 치환 (global) |
| `i` | 대소문자 무시 |
| `p` | 치환된 라인 출력 |
| `2`, `3`... | n번째 매칭만 치환 |
| `w 파일` | 치환된 라인을 파일에 저장 |

---

## 💡 실전 예시
```bash
# 첫 번째 매칭만 치환
sed 's/foo/bar/' file.txt

# 라인 내 모든 매칭 치환
sed 's/foo/bar/g' file.txt

# 대소문자 무시하고 치환
sed 's/foo/bar/gi' file.txt

# 2번째 매칭만 치환
sed 's/foo/bar/2' file.txt

# 파일 직접 수정
sed -i 's/foo/bar/g' file.txt

# 원본 백업 후 직접 수정
sed -i.bak 's/foo/bar/g' file.txt

# 특정 라인 삭제 (3번째 라인)
sed '3d' file.txt

# 특정 범위 라인 삭제 (3~5번째)
sed '3,5d' file.txt

# 빈 줄 삭제
sed '/^$/d' file.txt

# 특정 패턴이 있는 라인 삭제
sed '/foo/d' file.txt

# 특정 라인만 출력 (3번째 라인)
sed -n '3p' file.txt

# 특정 범위 라인만 출력 (3~5번째)
sed -n '3,5p' file.txt

# 패턴 매칭 라인만 출력
sed -n '/foo/p' file.txt

# 3번째 라인 앞에 텍스트 삽입
sed '3i\새로운 라인' file.txt

# 3번째 라인 뒤에 텍스트 추가
sed '3a\새로운 라인' file.txt

# 3번째 라인을 텍스트로 교체
sed '3c\교체된 라인' file.txt

# 여러 명령어 동시 실행
sed -e 's/foo/bar/g' -e 's/baz/qux/g' file.txt

# 명령어를 세미콜론으로 연결
sed 's/foo/bar/g; s/baz/qux/g' file.txt

# 앞뒤 공백 제거
sed 's/^[ \t]*//; s/[ \t]*$//' file.txt

# 주석 라인(#으로 시작) 삭제
sed '/^#/d' file.txt

# 첫 번째 라인 삭제
sed '1d' file.txt

# 마지막 라인 삭제
sed '$d' file.txt

# 소문자를 대문자로 변환
sed 'y/abcdefghijklmnopqrstuvwxyz/ABCDEFGHIJKLMNOPQRSTUVWXYZ/' file.txt

# 라인 번호 출력
sed '=' file.txt

# 5번째 라인에서 종료
sed '5q' file.txt

# 파이프와 함께 사용
cat file.txt | sed 's/foo/bar/g'
echo "hello world" | sed 's/world/sed/'
```

---

## 📐 주소 지정 방식
| 표현 | 설명 |
|------|------|
| `n` | n번째 라인 |
| `$` | 마지막 라인 |
| `n,m` | n번째 ~ m번째 라인 |
| `n~m` | n번째부터 m 간격으로 (예: `1~2` → 홀수 라인) |
| `/패턴/` | 패턴과 매칭되는 라인 |
| `/패턴1/,/패턴2/` | 패턴1 매칭 라인부터 패턴2 매칭 라인까지 |
| `n,/패턴/` | n번째 라인부터 패턴 매칭 라인까지 |

---

## ⚠️ 주의사항
- `-i` 옵션으로 직접 수정 시 되돌릴 수 없으므로 `-i.bak` 으로 백업 권장
- `s` 명령어에서 `/` 대신 `|`, `#`, `@` 등 다른 구분자 사용 가능 (경로 치환 시 유용)
- `-n` 없이 `p` 사용 시 매칭 라인이 중복 출력됨
- macOS의 sed는 GNU sed와 문법이 일부 다름 (`-i` 옵션 사용 시 `-i ''` 형태로 사용)
