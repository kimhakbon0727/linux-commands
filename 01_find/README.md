일회성 관리자 권한
sudo 명령어

관리자로 사용자 전환
su -

**파일명으로 바로 찾기**

find / -name “파일명”

find /[경로] -name "파일명.class"

**특정 문자열이 포함된 .class 찾기**

find /[경로] -name "*.class" | xargs grep -l "찾을문자열"

---

**특정 문자열이 포함된 .java 찾기**

find /[경로] -name "*.java" | xargs grep -l "찾을문자열"

---

**특정 문자열이 포함된 .jsp 찾기**

find /[경로] -name "*.jsp" | xargs grep -l "찾을문자열"

---

**특정 문자열이 포함된 .javascript 찾기**

find /[경로] -name "*.js" | xargs grep -l "찾을문자열"

---

**특정 문자열이 포함된 .xml 찾기**

find /[경로] -name "*.**xml** " | xargs grep -l "찾을문자열"

---

**이름 일부만 알 때 찾기**

find / "문자열*”

find / "*문자열”

---

**하위 디렉토리 포함 특정 확장자 검색**

find /[경로] -name "**.java"*

*find /[경로] -name "**.jsp"

*find /[경로] -name "**.js"
*find /[경로] -name "**.xml"
