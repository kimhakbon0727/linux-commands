# 🔗 ln — 링크 생성 (하드 링크 / 심볼릭 링크)

> 파일이나 폴더에 대한 **링크(바로가기)를 생성**하는 명령어  
> 링크 종류에 따라 **하드 링크**와 **심볼릭 링크(소프트 링크)** 로 나뉜다

---

## 📌 기본 구조

```bash
ln [옵션] 원본 링크명
```

---

## 🧩 하드 링크 vs 심볼릭 링크

| 구분 | 하드 링크 | 심볼릭 링크 (소프트 링크) |
|------|-----------|--------------------------|
| 옵션 | 없음 (기본값) | `-s` |
| 원본 삭제 시 | 데이터 유지 ✅ | 링크 깨짐 ❌ |
| 디렉토리 링크 | 불가 ❌ | 가능 ✅ |
| 다른 파티션 | 불가 ❌ | 가능 ✅ |
| inode | 원본과 동일 | 별도 inode |
| 용도 | 파일 백업/공유 | 바로가기, 경로 단축 |

---

## 🔍 기본 사용법

### 하드 링크 생성
```bash
# 기본 형식
ln 원본파일 링크명

# 예시
ln file.txt hard_link.txt
```
> 원본과 링크가 **같은 inode**를 공유 → 원본 삭제해도 데이터 유지

### 심볼릭 링크 생성 `-s`
```bash
# 기본 형식
ln -s 원본 링크명

# 파일에 심볼릭 링크
ln -s /etc/nginx/nginx.conf nginx.conf

# 디렉토리에 심볼릭 링크
ln -s /var/www/html /home/user/html
```
> Windows의 **바로가기**와 유사한 개념

---

## 🛠️ 자주 쓰는 실전 예제

### 긴 경로를 짧게 단축
```bash
# 자주 접근하는 깊은 경로를 홈 디렉토리에 링크
ln -s /var/log/nginx /home/user/nginx-log

# 이후 접근
cd ~/nginx-log
```

### 버전 관리 (현재 버전 가리키기)
```bash
# 특정 버전 디렉토리에 current 링크 생성
ln -s /opt/node-v20.0.0 /usr/local/node-current

# 버전 업그레이드 시 링크만 교체
ln -sf /opt/node-v22.0.0 /usr/local/node-current
```

### 설정 파일 공유
```bash
# 하나의 설정 파일을 여러 위치에서 참조
ln -s /home/user/.config/shared.conf /etc/app/shared.conf
```

### nginx / apache 사이트 활성화
```bash
# sites-available → sites-enabled 심볼릭 링크 (실제 서버 운영 패턴)
ln -s /etc/nginx/sites-available/mysite.conf /etc/nginx/sites-enabled/mysite.conf
```

---

## ⚙️ 주요 옵션

| 옵션 | 설명 | 예시 |
|------|------|------|
| `-s` | 심볼릭 링크 생성 | `ln -s 원본 링크명` |
| `-f` | 링크명이 이미 존재하면 덮어쓰기 | `ln -sf 원본 링크명` |
| `-v` | 생성 과정 출력 (verbose) | `ln -sv 원본 링크명` |
| `-n` | 링크 대상이 디렉토리여도 링크로 처리 | `ln -sn 원본 링크명` |

---

## 🔎 링크 확인 방법

```bash
# 심볼릭 링크 확인 (-> 로 원본 경로 표시)
ls -l

# 출력 예시
# lrwxrwxrwx 1 user user 20 Jan 15 10:00 nginx.conf -> /etc/nginx/nginx.conf

# inode 번호 확인 (하드 링크는 원본과 동일)
ls -li

# 심볼릭 링크만 찾기
find . -type l

# 링크가 가리키는 실제 경로 확인
readlink 링크명
readlink -f 링크명   # 절대 경로로 출력
```

---

## ❌ 링크 삭제

```bash
# 링크 삭제 (원본 파일은 영향 없음)
rm 링크명

# 또는
unlink 링크명
```
> ⚠️ 디렉토리 심볼릭 링크 삭제 시 끝에 `/` 붙이지 말 것  
> `rm link/` → 디렉토리 내용 삭제 위험  
> `rm link` → 링크만 안전하게 삭제 ✅

---

## 💡 팁

- 심볼릭 링크는 **절대 경로** 사용을 권장한다 (상대 경로는 링크 위치 기준이라 혼란스러울 수 있음)
  ```bash
  # ✅ 권장
  ln -s /etc/nginx/nginx.conf ~/nginx.conf

  # ⚠️ 주의 (링크 위치 기준 상대 경로)
  ln -s ../etc/nginx/nginx.conf ~/nginx.conf
  ```
- `-sf` 조합으로 **기존 링크를 새 버전으로 교체**할 때 자주 사용한다
  ```bash
  ln -sf /opt/python3.12 /usr/local/python
  ```
- `ls -l` 에서 맨 앞 글자가 `l` 이면 심볼릭 링크, `-` 이면 일반 파일, `d` 이면 디렉토리다
