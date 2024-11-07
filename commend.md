### 시스템 종료
- shutdonw -P now
- halt -p
- poweroff
- init 0

### 시스템 재부팅
- reboot
- init 6
- shutdown -r now: now는 즉시 실행을 의미
- shutdown -r 10: 10분 후 재부팅
- shutdown -r 20:00: 20:00에 재부팅
- shutdown -k +15: 15분 후에 종료 된다고 메시지 나오지만 종료 되지 않음
- shutdown -c: 예약된 재부팅 취소
- shutdown -r +5 "system will be rebooted in 5 minutes": 5분 후 재부팅, 메시지 출력
- shutdown -P +10: 10분 후 종료

### 로그아웃
- logout
- exit
- ctrl + d

### 사용자 계정 변경
- su - 계정명
- su - root


