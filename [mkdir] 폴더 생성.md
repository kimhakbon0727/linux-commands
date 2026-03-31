# 📁 디렉토리 생성 - mkdir 명령어

> 새로운 **디렉토리(폴더)를 생성**하는 명령어  
> 단순한 폴더 생성부터 **중첩된 디렉토리 구조**까지 한 번에 만들 수 있는 필수 명령어

---

## 📌 기본 문법

```bash
mkdir [옵션] [디렉토리명]
```

---

## ⚙️ 주요 옵션

| 옵션 | 설명 |
|------|------|
| `-p` | 중간 경로의 디렉토리가 없어도 한 번에 생성 (parents) |
| `-m` | 생성 시 권한(퍼미션) 지정 (예: `-m 755`) |
| `-v` | 생성된 디렉토리를 출력하며 진행 상황 표시 |
| `--help` | 도움말 출력 |

---

## 💡 기본 사용 예시

```bash
# 디렉토리 생성
mkdir mydir

# 여러 디렉토리 동시 생성
mkdir dir1 dir2 dir3

# 중첩 디렉토리 한 번에 생성 (-p)
mkdir -p parent/child/grandchild

# 권한 지정하며 생성
mkdir -m 755 mydir

# 생성 과정 출력하며 만들기 (-v)
mkdir -v newdir

# -p와 -v 함께 사용
mkdir -pv project/src/utils
```

---

## 🖥️ 출력 화면 예시

```bash
$ mkdir -v dir1 dir2 dir3
mkdir: created directory 'dir1'
mkdir: created directory 'dir2'
mkdir: created directory 'dir3'
```

```bash
$ mkdir -pv project/src/utils
mkdir: created directory 'project'
mkdir: created directory 'project/src'
mkdir: created directory 'project/src/utils'
```

> 💡 `-pv` 조합은 **중첩 디렉토리 생성 과정을 단계별로 확인**할 때 매우 유용합니다

---

## 🛠️ 실전 활용 예시

### 📂 기본 디렉토리 생성

```bash
# 현재 위치에 새 폴더 생성
mkdir project

# 절대 경로로 생성
mkdir /home/user/documents/newdir

# 상대 경로로 생성
mkdir ../sibling_dir
```

---

### 🏗️ 프로젝트 구조 한 번에 만들기

```bash
# 웹 프로젝트 기본 구조 생성
mkdir -p myproject/{src,public,tests}

# Node.js 프로젝트 구조
mkdir -p myapp/src/{components,pages,utils,hooks}

# 백엔드 프로젝트 구조
mkdir -p server/{controllers,models,routes,middlewares}

# 날짜별 로그 디렉토리 구조
mkdir -p logs/2025/{01,02,03,04,05,06,07,08,09,10,11,12}
```

---

### 🔐 권한과 함께 생성

```bash
# 소유자만 읽기/쓰기/실행 가능 (700)
mkdir -m 700 private_dir

# 모든 사용자 읽기/실행, 소유자만 쓰기 (755)
mkdir -m 755 public_dir

# 그룹 공유 디렉토리 (770)
mkdir -m 770 shared_dir

# 권한 확인
ls -la
```

---

### 🔍 조건부 생성 (이미 존재해도 에러 없음)

```bash
# 이미 존재해도 에러 없이 넘어감 (-p 활용)
mkdir -p already_exists_dir

# 스크립트 내에서 안전하게 사용
mkdir -p /tmp/workdir && echo "작업 디렉토리 준비 완료"
```

---

### 🔗 다른 명령어와 조합

```bash
# 디렉토리 생성 후 바로 이동
mkdir newdir && cd newdir

# 디렉토리 생성 후 파일 생성
mkdir -p config && touch config/settings.json

# 여러 프로젝트 폴더 한 번에 생성 후 확인
mkdir -p project/{src,dist,docs} && tree project

# 조건부 생성 (없을 때만 생성)
[ ! -d "mydir" ] && mkdir mydir

# 생성된 디렉토리 권한 확인
mkdir -m 755 newdir && ls -ld newdir
```

---

### 🖥️ 자동화 스크립트 활용

```bash
# 날짜 기반 백업 디렉토리 자동 생성
mkdir -p backup/$(date +%Y-%m-%d)

# 사용자 홈 디렉토리 구조 자동 생성
mkdir -p ~/projects ~/downloads ~/documents ~/scripts

# 배포 환경 폴더 구조 생성
mkdir -pv /var/www/html/{assets,uploads,logs}
```

---

## 🔁 mkdir vs 관련 명령어 비교

| 명령어 | 용도 | 특징 |
|--------|------|------|
| `mkdir` | 디렉토리 생성 | 빠르고 간단, `-p`로 중첩 생성 |
| `rmdir` | 빈 디렉토리 삭제 | 비어있는 디렉토리만 삭제 가능 |
| `rm -rf` | 디렉토리 강제 삭제 | 내용물까지 전부 삭제 ⚠️ 주의 |
| `cp -r` | 디렉토리 복사 | 내용물 포함 재귀 복사 |
| `mv` | 디렉토리 이동/이름 변경 | 경로 또는 이름 변경 |
| `ls -d` | 디렉토리 목록 확인 | 디렉토리만 필터링해서 출력 |
| `tree` | 디렉토리 구조 시각화 | 트리 형태로 계층 구조 출력 |

```bash
# 디렉토리 구조 확인
tree myproject

# 생성 후 바로 권한 확인
ls -ld mydir

# 빈 디렉토리만 삭제
rmdir emptydir

# 내용물 포함 삭제 (주의!)
rm -rf fulldir
```

---

## ⚡ 자주 쓰는 조합 모음

```bash
mkdir -p path/to/dir            # 중첩 경로 한 번에 생성
mkdir -pv path/to/dir           # 생성 과정 출력하며 생성
mkdir -m 755 dirname            # 권한 지정하며 생성
mkdir dir1 dir2 dir3            # 여러 디렉토리 동시 생성
mkdir -p project/{src,dist,docs}  # 중괄호 확장으로 구조 생성
mkdir newdir && cd newdir       # 생성 후 바로 이동
mkdir -p /tmp/dir && touch /tmp/dir/file.txt  # 생성 후 파일 추가
```

---

## ⚠️ 주의사항

```bash
# ❌ 이미 존재하는 디렉토리 생성 시 에러 발생
mkdir existing_dir
# mkdir: cannot create directory 'existing_dir': File exists

# ✅ -p 옵션으로 에러 없이 처리
mkdir -p existing_dir

# ❌ 중간 경로 없이 중첩 디렉토리 생성 시 에러
mkdir a/b/c
# mkdir: cannot create directory 'a/b/c': No such file or directory

# ✅ -p 옵션으로 중간 경로까지 모두 생성
mkdir -p a/b/c

# ❌ rm -rf는 복구 불가! 반드시 경로 확인 후 사용
rm -rf /  # 절대 금지!

# ✅ 삭제 전 경로 확인 습관화
echo /path/to/dir && rm -rf /path/to/dir
```

---

> 💬 **Tip**: `mkdir` 이름은 **make directory**의 줄임말입니다.  
> `-p` 옵션과 **중괄호 확장(`{}`)** 을 함께 쓰면 복잡한 프로젝트 구조도 한 줄로 생성할 수 있어요!
> ```bash
> # 풀스택 프로젝트 구조를 한 줄로!
> mkdir -p myapp/{frontend/{src,public},backend/{routes,models},docs}
>
> # 확인
> tree myapp
> # myapp
> # ├── backend
> # │   ├── models
> # │   └── routes
> # ├── docs
> # └── frontend
> #     ├── public
> #     └── src
> ```
