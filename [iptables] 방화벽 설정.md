# 🛡️ iptables 명령어 치트시트
> 리눅스 커널의 패킷 필터링 방화벽을 설정하는 명령어

---

## 📌 기본 문법
```
iptables [옵션] <체인> [조건] [-j 타깃]
```

---

## 🗂️ 테이블 종류
| 테이블 | 설명 |
|--------|------|
| `filter` | 기본 테이블, 패킷 허용/차단 (INPUT, OUTPUT, FORWARD) |
| `nat` | 네트워크 주소 변환 (PREROUTING, POSTROUTING, OUTPUT) |
| `mangle` | 패킷 헤더 수정 |
| `raw` | 연결 추적 제외 설정 |

---

## ⛓️ 체인 종류
| 체인 | 설명 |
|------|------|
| `INPUT` | 서버로 들어오는 패킷 |
| `OUTPUT` | 서버에서 나가는 패킷 |
| `FORWARD` | 서버를 경유하는 패킷 |
| `PREROUTING` | 라우팅 전 패킷 처리 (nat, mangle) |
| `POSTROUTING` | 라우팅 후 패킷 처리 (nat, mangle) |

---

## ⚙️ 주요 옵션
| 옵션 | 설명 |
|------|------|
| `-A` | 체인 끝에 규칙 추가 (Append) |
| `-I` | 체인 앞에 규칙 삽입 (Insert) |
| `-D` | 규칙 삭제 (Delete) |
| `-R` | 규칙 교체 (Replace) |
| `-L` | 규칙 목록 출력 (List) |
| `-F` | 체인의 모든 규칙 삭제 (Flush) |
| `-X` | 사용자 정의 체인 삭제 |
| `-Z` | 패킷/바이트 카운터 초기화 |
| `-N` | 새로운 체인 생성 |
| `-P` | 체인 기본 정책 설정 |
| `-n` | IP/포트를 숫자로 출력 |
| `-v` | 상세 출력 |
| `--line-numbers` | 규칙에 번호 표시 |
| `-t` | 테이블 지정 (기본값: filter) |

---

## 🎯 조건 옵션
| 옵션 | 설명 |
|------|------|
| `-p` | 프로토콜 지정 (tcp, udp, icmp, all) |
| `-s` | 출발지 IP/대역 지정 |
| `-d` | 목적지 IP/대역 지정 |
| `-i` | 입력 네트워크 인터페이스 지정 |
| `-o` | 출력 네트워크 인터페이스 지정 |
| `--sport` | 출발지 포트 지정 |
| `--dport` | 목적지 포트 지정 |
| `-m state --state` | 연결 상태 지정 |
| `-m multiport --dports` | 여러 포트 지정 |

---

## 🚦 타깃 (정책) 종류
| 타깃 | 설명 |
|------|------|
| `ACCEPT` | 패킷 허용 |
| `DROP` | 패킷 차단 (응답 없음) |
| `REJECT` | 패킷 차단 (거부 응답 전송) |
| `LOG` | 패킷 로그 기록 후 다음 규칙 계속 |
| `MASQUERADE` | 동적 IP에서 NAT 처리 |
| `SNAT` | 출발지 IP 변환 |
| `DNAT` | 목적지 IP 변환 |

---

## 💡 실전 예시
```bash
# 현재 규칙 목록 확인
iptables -L

# 번호와 상세 정보 포함 출력
iptables -L -n -v --line-numbers

# 특정 테이블 규칙 확인
iptables -t nat -L -n -v

# 모든 규칙 삭제
iptables -F

# SSH (22번 포트) 허용
iptables -A INPUT -p tcp --dport 22 -j ACCEPT

# HTTP (80번 포트) 허용
iptables -A INPUT -p tcp --dport 80 -j ACCEPT

# HTTPS (443번 포트) 허용
iptables -A INPUT -p tcp --dport 443 -j ACCEPT

# 특정 IP 허용
iptables -A INPUT -s 192.168.1.100 -j ACCEPT

# 특정 IP 대역 허용
iptables -A INPUT -s 192.168.1.0/24 -j ACCEPT

# 특정 IP 차단
iptables -A INPUT -s 192.168.1.100 -j DROP

# 여러 포트 한번에 허용
iptables -A INPUT -p tcp -m multiport --dports 80,443,8080 -j ACCEPT

# 루프백 인터페이스 허용 (로컬 통신)
iptables -A INPUT -i lo -j ACCEPT

# 이미 연결된 세션 허용
iptables -A INPUT -m state --state ESTABLISHED,RELATED -j ACCEPT

# 새 연결 + 기존 연결 허용
iptables -A INPUT -m state --state NEW,ESTABLISHED -j ACCEPT

# ICMP (ping) 허용
iptables -A INPUT -p icmp -j ACCEPT

# ICMP (ping) 차단
iptables -A INPUT -p icmp -j DROP

# 특정 규칙 삭제 (번호로)
iptables -D INPUT 3

# 특정 규칙 삭제 (내용으로)
iptables -D INPUT -p tcp --dport 80 -j ACCEPT

# 기본 정책을 DROP으로 설정 (화이트리스트 방식)
iptables -P INPUT DROP
iptables -P FORWARD DROP
iptables -P OUTPUT ACCEPT

# 포트 포워딩 (8080 → 80)
iptables -t nat -A PREROUTING -p tcp --dport 8080 -j REDIRECT --to-port 80

# IP 포워딩 (NAT)
iptables -t nat -A POSTROUTING -o eth0 -j MASQUERADE

# 패킷 로그 기록
iptables -A INPUT -p tcp --dport 22 -j LOG --log-prefix "SSH ACCESS: "

# 규칙 저장 (Ubuntu/Debian)
iptables-save > /etc/iptables/rules.v4

# 규칙 복원
iptables-restore < /etc/iptables/rules.v4
```

---

## 🔄 패킷 흐름
```
외부 패킷 수신
      │
 PREROUTING   ← DNAT, 포트포워딩
      │
   라우팅 판단
   ┌──┴──┐
INPUT  FORWARD  ← 경유 패킷
   │      │
로컬처리  POSTROUTING ← SNAT, MASQUERADE
   │      │
OUTPUT    외부로
   │
POSTROUTING
   │
 외부로
```

---

## 📋 연결 상태 (--state)
| 상태 | 설명 |
|------|------|
| `NEW` | 새로운 연결 요청 |
| `ESTABLISHED` | 이미 연결된 세션 |
| `RELATED` | 기존 연결과 관련된 새 연결 |
| `INVALID` | 알 수 없거나 잘못된 패킷 |

---

## ⚠️ 주의사항
- 규칙은 **위에서 아래 순서대로** 적용되며 첫 번째 매칭 규칙에서 처리됨
- `-P INPUT DROP` 설정 전 반드시 SSH 허용 규칙을 먼저 추가할 것 (원격 접속 차단 주의)
- 재부팅 시 규칙이 초기화되므로 `iptables-save` 로 저장 필수
- Ubuntu 20.04 이후부터는 `nftables` 또는 `ufw` 사용을 권장
