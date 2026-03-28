# 🔍 nslookup 정리

DNS 조회 도구로, 도메인의 IP 주소나 DNS 레코드를 확인할 때 사용한다.

---

## 📌 기본 사용법
```bash
nslookup <도메인>
```
```bash
# 예시
nslookup google.com
```

---

## 🛠️ 실무 예제

### 1. 도메인 → IP 조회 (A 레코드)
```bash
nslookup google.com
```
```
Server:   8.8.8.8
Address:  8.8.8.8#53

Non-authoritative answer:
Name:     google.com
Address:  142.250.196.110
```

---

### 2. 특정 DNS 서버 지정해서 조회
```bash
nslookup google.com 8.8.8.8        # Google DNS
nslookup google.com 1.1.1.1        # Cloudflare DNS
nslookup google.com 168.126.63.1   # KT DNS
```

> 내부 DNS와 외부 DNS 결과가 다를 때 원인 파악에 유용하다.

---

### 3. 역방향 조회 (IP → 도메인)
```bash
nslookup 142.250.196.110
```
```
Non-authoritative answer:
110.196.250.142.in-addr.arpa  name = nrt12s36-in-f14.1e100.net.
```

---

### 4. 레코드 타입 지정 조회
```bash
# MX 레코드 (메일 서버)
nslookup -type=MX gmail.com

# NS 레코드 (네임서버)
nslookup -type=NS google.com

# TXT 레코드 (SPF, DKIM 등)
nslookup -type=TXT google.com

# CNAME 레코드
nslookup -type=CNAME www.github.com

# A 레코드 (기본값)
nslookup -type=A google.com

# AAAA 레코드 (IPv6)
nslookup -type=AAAA google.com

# SOA 레코드
nslookup -type=SOA google.com

# ANY (모든 레코드)
nslookup -type=ANY google.com
```

---

### 5. 인터랙티브 모드
```bash
nslookup        # 명령어만 입력하면 인터랙티브 모드 진입
```
```
> server 8.8.8.8       # DNS 서버 변경
> set type=MX          # 레코드 타입 변경
> gmail.com            # 조회
> exit                 # 종료
```

---

### 6. 실무 활용 시나리오

#### 🔸 도메인 연결 확인 (배포 후 DNS 전파 확인)
```bash
nslookup mysite.com
nslookup mysite.com 8.8.8.8   # 외부에서도 확인
```

#### 🔸 메일 발송 문제 디버깅
```bash
nslookup -type=MX company.com
nslookup -type=TXT company.com   # SPF 레코드 확인
```

#### 🔸 CDN / CNAME 체인 확인
```bash
nslookup -type=CNAME cdn.mysite.com
```

#### 🔸 내부 DNS vs 외부 DNS 비교
```bash
nslookup internal.company.com 192.168.1.1    # 내부 DNS
nslookup internal.company.com 8.8.8.8        # 외부 DNS
```

---

## ⚠️ 참고 사항

| 항목 | 설명 |
|------|------|
| Non-authoritative answer | 캐시된 응답 (권한 없는 서버에서 반환) |
| Authoritative answer | 해당 도메인을 관리하는 서버에서 직접 반환 |
| TTL | DNS 캐시 유지 시간 (초 단위) |
| DNS 전파 시간 | 변경 후 전 세계 반영까지 최대 48시간 소요 |

---

## 🔗 관련 명령어
```bash
dig google.com          # nslookup보다 상세한 DNS 조회 (Linux/Mac)
host google.com         # 간단한 DNS 조회
ping google.com         # 연결 및 응답 속도 확인
ipconfig /flushdns      # DNS 캐시 초기화 (Windows)
sudo dscacheutil -flushcache  # DNS 캐시 초기화 (Mac)
```
