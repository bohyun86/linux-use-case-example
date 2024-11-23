### **1. 텔넷 서버 (Telnet Server)**
`텔넷`은 보안 취약점이 많아 일반적으로 사용하지 않지만, 학습 목적이나 테스트 목적으로 설정할 수 있습니다.

#### **텔넷 서버 구축 방법 (RHEL)**
1. **Telnet 설치**
   
   ```sh
   sudo rpm -qa telnet-server # 설치 여부 확인
   sudo dnf -y install telnet-server
   ```

2. **telnet 서비스 시작 및 활성화**
   ```sh
   sudo systemctl start telnet.socket
   sudo systemctl status telnet.socket
   sudo systemctl enable telnet.socket
   ```

3. **방화벽 설정**
   방화벽에서 텔넷의 포트(23)를 허용해야 합니다.
   ```sh
   sudo firewall-cmd --permanent --add-port=23/tcp
   sudo firewall-cmd --reload
   ```

4. **사용자 등록**

   텔넷을 사용할 수 있는 사용자를 등록합니다.
   ```sh
   sudo useradd <사용자이름>
   sudo passwd <사용자이름>
   ```

5. **연결 테스트**
   다른 시스템에서 텔넷 연결을 테스트합니다.
   ```sh
   telnet <서버 IP 주소>
   ```

---

### **2. SSH 서버 (SSH Server)**
`SSH`는 보안된 원격 연결을 제공하며, RHEL에서 기본적으로 제공되는 가장 보편적인 원격 접속 방법입니다.

#### **SSH 서버 구축 방법 (RHEL)**
1. **SSH 설치**
   기본적으로 SSH는 RHEL에 설치되어 있습니다. 그렇지 않다면 다음 명령어로 설치할 수 있습니다.
   ```sh
   sudo yum install openssh-server -y
   ```

2. **SSH 서비스 시작 및 활성화**
   ```sh
   sudo systemctl start sshd
   sudo systemctl enable sshd
   ```

3. **설정 파일 편집**
   SSH 설정 파일은 `/etc/ssh/sshd_config`에 위치해 있습니다. 보안을 강화하려면 다음과 같은 설정을 수정할 수 있습니다:
   - **기본 포트 변경** (선택 사항):
     ```sh
     sudo vi /etc/ssh/sshd_config
     ```
     ```sh
     # 포트를 22에서 2222로 변경
     Port 2222
     ```
   - **루트 로그인 비활성화**:
     ```sh
     PermitRootLogin no
     ```

4. **SSH 서비스 재시작**
   설정을 변경한 후에는 SSH 서비스를 재시작해야 합니다.
   ```sh
   sudo systemctl restart sshd
   ```

5. **방화벽 설정**
   방화벽에서 SSH 포트를 허용해야 합니다.
   ```sh
   sudo firewall-cmd --permanent --add-port=22/tcp
   # 만약 포트를 변경했다면, 해당 포트로 설정
   sudo firewall-cmd --permanent --add-port=2222/tcp
   sudo firewall-cmd --reload
   ```

6. **SSH 접속 테스트**
   다른 컴퓨터에서 SSH를 사용하여 연결할 수 있습니다.
   ```sh
   ssh 사용자이름@<서버 IP 주소>
   ```

---

### **3. XRDP 서버 (XRDP Server)**
`XRDP`는 리눅스 서버에 GUI 기반의 데스크탑 환경으로 원격 연결을 제공하며, 특히 윈도우 사용자들이 쉽게 리눅스에 접근할 수 있게 해줍니다.

#### **XRDP 서버 구축 방법 (RHEL)**
1. **데스크탑 환경 설치**
   `XRDP`를 사용하려면 GUI 환경이 필요합니다. RHEL에 GNOME과 같은 데스크탑 환경을 설치합니다.
   ```sh
   sudo yum groupinstall "Server with GUI" -y
   ```

2. **XRDP 설치**
   - 기본 RHEL 리포지토리에는 XRDP가 포함되지 않을 수 있으므로 `EPEL` 리포지토리를 활성화해야 합니다.
   ```sh
   sudo yum install epel-release -y
   sudo yum install xrdp -y
   ```

3. **XRDP 서비스 시작 및 활성화**
   ```sh
   sudo systemctl start xrdp
   sudo systemctl enable xrdp
   ```

4. **방화벽 설정**
   XRDP의 기본 포트인 3389번 포트를 허용합니다.
   ```sh
   sudo firewall-cmd --permanent --add-port=3389/tcp
   sudo firewall-cmd --reload
   ```

5. **XRDP와 데스크탑 환경 연결 설정**
   - XRDP는 기본적으로 `sesman`을 통해 데스크탑 세션을 관리합니다.
   - `/etc/xrdp/startwm.sh` 파일을 편집하여 GNOME과 같은 데스크탑 환경을 설정할 수 있습니다.
   ```sh
   sudo vi /etc/xrdp/startwm.sh
   ```
   - GNOME 환경을 지정하는 줄을 추가합니다.
   ```sh
   # 기존의 끝 부분을 주석 처리하고 아래 내용을 추가
   # . /etc/X11/xinit/Xsession
   exec gnome-session
   ```

6. **XRDP 서비스 재시작**
   설정을 변경한 후 XRDP 서비스를 재시작해야 합니다.
   ```sh
   sudo systemctl restart xrdp
   ```

7. **윈도우에서 원격 접속**
   윈도우에서 **원격 데스크톱 연결**을 열고 RHEL 서버의 IP 주소를 입력하여 연결합니다. 사용자 이름과 비밀번호를 입력하여 로그인합니다.

---

### **보안 고려사항**
- **텔넷**은 **보안에 취약**하므로 절대로 프로덕션 환경에서는 사용하지 않는 것이 좋습니다. 텔넷을 사용해야 하는 경우에는 VPN을 통해 접속하는 등 추가적인 보안 조치를 고려하세요.
- **SSH**는 가능한 기본 포트를 변경하고, 공개키 인증을 설정하여 **보안을 강화**하는 것이 좋습니다. 또한, Fail2ban과 같은 툴을 사용하여 무차별 대입 공격을 방지할 수 있습니다.
- **XRDP**는 **RDP** 프로토콜을 사용하므로 외부에서 접근 시 암호화된 터널(VPN 등)을 사용하는 것이 좋습니다.

이와 같은 설정으로 RHEL 리눅스에서 텔넷, SSH, XRDP 서버를 구축할 수 있으며, 각각의 목적에 맞게 사용할 수 있습니다. 보안은 항상 최우선으로 고려해야 하므로 각 서버의 보안 설정을 잘 관리해 주세요.

---
### **SSH 키란?**
SSH 키는 **Secure Shell**(SSH) 프로토콜을 사용해 네트워크를 통한 보안 연결을 설정하기 위해 사용되는 **암호화 키 쌍**입니다. 보통 서버와 클라이언트 간의 인증 및 데이터 암호화를 위해 사용됩니다.

---

### **SSH 키의 구성**
1. **비공개 키 (Private Key)**:
   - 본인만 소유하는 키입니다.
   - 외부로 유출되면 보안 위협이 발생하므로 절대 공유하지 않아야 합니다.
   - 디폴트 위치: `~/.ssh/id_ed25519`

2. **공개 키 (Public Key)**:
   - 인증을 위해 서버나 원격 시스템에 등록합니다.
   - 이 키는 누구와 공유해도 무방합니다.
   - 디폴트 위치: `~/.ssh/id_ed25519.pub`

---

### **SSH 키의 역할**
SSH 키는 비밀번호를 입력하지 않고도 서버나 원격 시스템에 인증할 수 있도록 도와줍니다. 이 방식은 **비밀번호 기반 인증**보다 안전하고 편리합니다.

1. **공개 키 등록**:
   - 서버에 공개 키를 등록합니다.
   - 이 키는 클라이언트에서 제공하는 비공개 키와 매칭되면 인증을 승인합니다.

2. **암호화된 연결**:
   - 데이터가 키를 사용해 암호화되어 전송됩니다.

---

### **SSH 키 인증 과정**
1. 사용자가 서버에 연결을 요청합니다.
2. 서버는 클라이언트에 **Challenge**(난수 같은 요청)를 보냅니다.
3. 클라이언트는 비공개 키로 Challenge를 암호화해 서버로 보냅니다.
4. 서버는 등록된 공개 키로 이를 검증하여 인증을 승인합니다.

---

### **SSH 키의 장점**
1. **보안성**: 비밀번호를 사용하지 않아도 되므로, 브루트 포스 공격 등에 안전합니다.
2. **편리함**: 매번 비밀번호를 입력할 필요 없이 자동 인증이 가능합니다.
3. **자동화**: 스크립트나 애플리케이션에서 비밀번호 없이 안전하게 원격 연결을 사용할 수 있습니다.

---

### **GitHub와 SSH 키**
GitHub에서는 SSH 키를 이용해 사용자의 컴퓨터와 GitHub 리포지토리 간에 안전한 연결을 설정할 수 있습니다. 
- 공개 키를 GitHub에 등록하면, 비공개 키를 이용해 Git 명령을 실행할 때 비밀번호 입력 없이 인증이 가능합니다.

---

### **SSH 키 생성 명령**
```bash
ssh-keygen -t ed25519 -C "your_email@example.com"
```
- `-t ed25519`: SSH 키의 타입으로, 더 짧고 안전한 `ed25519` 알고리즘을 사용.
- `-C "your_email@example.com"`: 키를 구분하기 위해 사용하는 코멘트(주로 이메일).

---

SSH 키는 GitHub와 같은 플랫폼뿐만 아니라, 서버 관리, 클라우드 연결, 자동화 스크립트 등에 널리 사용됩니다. 