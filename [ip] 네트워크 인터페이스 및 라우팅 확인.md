# 🌐 ip

네트워크 **인터페이스, IP 주소, 라우팅, ARP** 등을 조회하고 설정하는 명령어.  
`ifconfig` 의 현대적 대체 명령어.

---

## 기본 문법

```bash
ip [옵션] [오브젝트] [명령]
```

| 오브젝트 | 축약 | 설명 |
|----------|------|------|
| `address` | `a` | IP 주소 |
| `link` | `l` | 네트워크 인터페이스 |
| `route` | `r` | 라우팅 테이블 |
| `neighbour` | `n` | ARP / NDP 테이블 |

---

## IP 주소 확인

```bash
# 모든 인터페이스의 IP 주소 확인
ip address
ip a

# 특정 인터페이스만 조회
ip a show eth0

# IPv4만 조회
ip -4 a

# IPv6만 조회
ip -6 a
```

---

## 네트워크 인터페이스 확인

```bash
# 모든 인터페이스 상태 확인
ip link
ip l

# 특정 인터페이스 확인
ip link show eth0

# 인터페이스 활성화 (UP)
ip link set eth0 up

# 인터페이스 비활성화 (DOWN)
ip link set eth0 down
```

---

## IP 주소 설정 (임시, 재부팅 시 초기화)

```bash
# IP 주소 추가
ip address add 192.168.1.100/24 dev eth0

# IP 주소 삭제
ip address del 192.168.1.100/24 dev eth0
```

---

## 라우팅 테이블

```bash
# 라우팅 테이블 조회
ip route
ip r

# 기본 게이트웨이 확인
ip route show default

# 특정 IP로 가는 경로 확인
ip route get 8.8.8.8

# 라우트 추가
ip route add 10.0.0.0/8 via 192.168.1.1

# 라우트 삭제
ip route del 10.0.0.0/8

# 기본 게이트웨이 설정
ip route add default via 192.168.1.1
```

---

## ARP 테이블 (neighbour)

```bash
# ARP 테이블 조회
ip neighbour
ip n

# ARP 캐시 초기화
ip neighbour flush all
```

---

## 출력 형식

```bash
# 색상 없이 출력 (스크립트에서 파싱할 때)
ip -o a

# JSON 형식 출력
ip -j a
ip -j route

# 간략 출력
ip -br a
ip -br link
```

---

## 자주 쓰는 조합

```bash
# 서버 IP 빠르게 확인
ip -br a

# 기본 게이트웨이 확인
ip route show default

# 특정 목적지까지 경로 확인
ip route get 1.1.1.1

# 인터페이스 이름 목록만 출력
ip -o link show | awk '{print $2}' | tr -d ':'
```

---

## ifconfig 와 비교

| ifconfig | ip | 설명 |
|----------|----|------|
| `ifconfig` | `ip a` | 인터페이스 및 IP 확인 |
| `ifconfig eth0 up` | `ip link set eth0 up` | 인터페이스 활성화 |
| `ifconfig eth0 192.168.1.1` | `ip a add 192.168.1.1/24 dev eth0` | IP 설정 |
| `route -n` | `ip r` | 라우팅 테이블 |
| `arp -n` | `ip n` | ARP 테이블 |

---

## 팁

- `ip -br a` 는 인터페이스 상태와 IP를 한눈에 보기 좋은 가장 짧은 명령어
- 설정 변경은 **임시**이므로 영구 적용은 `/etc/netplan/` (Ubuntu) 또는 `/etc/sysconfig/network-scripts/` (CentOS) 수정 필요
- `ip route get [IP]` 는 해당 목적지로 패킷이 어느 인터페이스를 통해 나가는지 확인할 때 유용
