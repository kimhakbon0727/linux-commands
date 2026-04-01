# 📦 mv (Move) 명령어 치트시트

> 파일 또는 폴더를 **이동**하거나 **이름을 변경**하는 리눅스/유닉스 명령어

---

## 📌 기본 문법

```
mv [옵션] [원본] [대상]
```

---

## ⚙️ 주요 옵션

| 옵션 | 설명 |
|------|------|
| `-i` | 덮어쓰기 전에 확인 메시지 표시 (interactive) |
| `-f` | 확인 없이 강제 덮어쓰기 (force) |
| `-n` | 대상 파일이 있으면 덮어쓰지 않음 (no-clobber) |
| `-v` | 이동되는 파일 이름을 출력 (verbose) |
| `-b` | 덮어쓰기 전에 백업 파일 생성 |

---

## 💡 실전 예시

### 파일 이동
```bash
# 파일을 다른 디렉토리로 이동
mv file.txt /home/user/docs/

# 여러 파일을 한 번에 이동
mv file1.txt file2.txt file3.txt /home/user/docs/

# 현재 디렉토리의 모든 .log 파일 이동
mv *.log /var/log/backup/
```

### 파일 이름 변경
```bash
# 파일 이름 변경
mv old_name.txt new_name.txt

# 폴더 이름 변경
mv old_folder new_folder
```

### 폴더 이동
```bash
# 폴더를 다른 위치로 이동
mv /home/user/project /var/www/

# 폴더를 상위 디렉토리로 이동
mv myfolder ../
```

### 옵션 활용
```bash
# 덮어쓰기 전에 확인
mv -i file.txt /backup/

# 이동 과정 출력
mv -v *.txt /backup/
# 'file1.txt' -> '/backup/file1.txt'
# 'file2.txt' -> '/backup/file2.txt'

# 대상에 같은 파일이 있으면 덮어쓰지 않음
mv -n file.txt /backup/

# 덮어쓰기 전에 백업 생성 (file.txt~ 형태로 백업됨)
mv -b file.txt /backup/
```

---

## ⚠️ 주의사항

- `mv`는 **복사 후 삭제**가 아니라 **실제 이동**이므로 원본이 사라짐
- 같은 파티션 내 이동은 **즉각적** (inode만 변경)
- 다른 파티션 간 이동은 **복사 후 삭제** 방식으로 동작
- 기본적으로 대상 파일이 존재하면 **경고 없이 덮어씀** → 중요한 파일은 `-i` 옵션 권장

---

## 🔄 cp vs mv 비교

| 항목 | `cp` | `mv` |
|------|------|------|
| 원본 파일 | 유지됨 | 사라짐 |
| 용도 | 복사 | 이동 / 이름 변경 |
| 같은 파티션 속도 | 느림 (복사 발생) | 빠름 (inode 변경) |
