# 📋 cp (Copy) 명령어 치트시트
> 파일 또는 디렉토리를 복사하는 리눅스/유닉스 명령어

---

## 📌 기본 문법
```
cp [옵션] <원본> <대상>
cp [옵션] <원본1> <원본2> ... <대상_디렉토리>
```

---

## ⚙️ 주요 옵션
| 옵션 | 설명 |
|------|------|
| `-r`, `-R` | 디렉토리를 재귀적으로 복사 |
| `-i` | 덮어쓰기 전 확인 프롬프트 |
| `-f` | 강제 복사 (확인 없이 덮어쓰기) |
| `-n` | 기존 파일 덮어쓰지 않음 |
| `-u` | 원본이 더 새로운 경우에만 복사 |
| `-v` | 복사 과정 상세 출력 |
| `-p` | 파일 속성(권한, 타임스탬프 등) 유지 |
| `-a` | 아카이브 모드 (`-dpR` 조합, 속성/링크/재귀 모두 유지) |
| `-l` | 복사 대신 하드 링크 생성 |
| `-s` | 복사 대신 심볼릭 링크 생성 |
| `--backup` | 덮어쓰기 전 기존 파일 백업 |

---

## 💡 실전 예시
```bash
# 파일 복사
cp file.txt file_backup.txt

# 다른 디렉토리로 파일 복사
cp file.txt /home/user/docs/

# 여러 파일을 디렉토리로 복사
cp file1.txt file2.txt file3.txt /home/user/docs/

# 디렉토리 전체 복사 (-r 필수)
cp -r mydir/ /home/user/backup/

# 속성 유지하며 복사 (권한, 타임스탬프)
cp -p file.txt /backup/file.txt

# 아카이브 모드 (속성 + 심볼릭링크 + 재귀 모두 유지)
cp -a mydir/ /backup/mydir/

# 덮어쓰기 전 확인
cp -i file.txt /backup/

# 복사 과정 출력
cp -v file.txt /backup/

# 더 새로운 파일만 복사 (동기화 용도)
cp -u *.txt /backup/

# 덮어쓰기 방지
cp -n file.txt /backup/

# 복사 전 기존 파일 백업
cp --backup file.txt /backup/file.txt
```

---

## 🔀 원본 vs 대상 패턴 정리
| 원본 | 대상 | 결과 |
|------|------|------|
| `file.txt` | `new.txt` | 파일을 new.txt로 복사 |
| `file.txt` | `dir/` | dir 안에 file.txt로 복사 |
| `dir/` | `newdir/` | newdir 없으면 생성 후 복사 |
| `dir/` | `existing/` | existing/dir/ 형태로 복사 |
| `*.txt` | `dir/` | 모든 .txt 파일을 dir에 복사 |

---

## ⚠️ 주의사항
- 디렉토리 복사 시 `-r` 옵션 없으면 오류 발생
- 대상 경로가 이미 존재하는 디렉토리면 그 안에 복사됨
- `-f`와 `-i`를 함께 쓰면 마지막 옵션이 우선 적용
- 중요한 파일 덮어쓸 때는 `-i` 또는 `--backup` 활용 권장
