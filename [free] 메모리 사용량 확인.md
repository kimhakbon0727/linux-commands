# 🧠 free

시스템의 **메모리(RAM) 및 스왑 사용량**을 확인하는 명령어.

---

## 기본 문법

```bash
free [옵션]
```

---

## 기본 사용

```bash
# 기본 출력 (KiB 단위)
free

# 사람이 읽기 쉬운 단위 자동 변환 (KB/MB/GB)
free -h

# 메가바이트 단위
free -m

# 기가바이트 단위
free -g
```

---

## 출력 예시

```
              total        used        free      shared  buff/cache   available
Mem:           15Gi       3.2Gi       8.1Gi       312Mi       4.0Gi        11Gi
Swap:         2.0Gi          0B       2.0Gi
```

---

## 출력 컬럼 설명

| 컬럼 | 설명 |
|------|------|
| `total` | 전체 메모리 용량 |
| `used` | 실제 사용 중인 메모리 |
| `free` | 완전히 비어 있는 메모리 |
| `shared` | 여러 프로세스가 공유하는 메모리 (tmpfs 등) |
| `buff/cache` | 버퍼 + 캐시로 사용 중인 메모리 |
| `available` | 실제로 사용 가능한 메모리 (캐시 회수 포함) |

> ✅ 실제 가용 메모리는 `free` 가 아닌 **`available`** 을 기준으로 봐야 함

---

## 주기적 출력

```bash
# 3초마다 갱신
free -h -s 3

# 5번 반복 후 종료
free -h -c 5

# 3초 간격으로 5번 반복
free -h -s 3 -c 5
```

---

## watch 와 조합

```bash
# 2초마다 실시간 갱신
watch -n 2 free -h
```

---

## 단위 옵션 정리

| 옵션 | 단위 |
|------|------|
| `-b` | 바이트 (Bytes) |
| `-k` | 킬로바이트 (KiB, 기본값) |
| `-m` | 메가바이트 (MiB) |
| `-g` | 기가바이트 (GiB) |
| `-h` | 자동 단위 변환 (Human-readable) |

---

## 스왑(Swap) 확인

```bash
# 스왑 사용량 확인
free -h

# 스왑 상세 확인
swapon --show
```

> 스왑이 과도하게 사용되고 있다면 메모리 부족 신호

---

## 메모리 캐시 해제 (주의)

```bash
# 페이지 캐시만 해제
sync; echo 1 > /proc/sys/vm/drop_caches

# 캐시 + dentries/inodes 해제
sync; echo 3 > /proc/sys/vm/drop_caches
```

> ⚠️ 운영 서버에서는 신중하게 사용. 캐시 해제 후 성능이 일시적으로 저하될 수 있음

---

## 자주 쓰는 조합

```bash
# 현재 메모리 상태 한눈에
free -h

# 실시간 메모리 모니터링
watch -n 1 free -h

# available 값만 추출
free -h | awk '/^Mem:/ {print $7}'
```

---

## 팁

- 리눅스는 남는 메모리를 **buff/cache로 적극 활용**하기 때문에 `free` 값이 작아도 정상
- 메모리 부족 여부는 `available` 이 지속적으로 낮은지로 판단
- OOM(Out of Memory) 이슈는 `dmesg | grep -i oom` 으로 확인 가능
