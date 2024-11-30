 목표는 MySQL 서버를 설치하고, 백그라운드에서 작동하도록 설정하며, 새로운 사용자를 생성하여 특정 스키마에서 모든 작업을 수행할 수 있도록 권한을 부여하고, 외부 모든 IP에서 접근 가능하도록 설정하는 것입니다.

### 단계 1: MySQL 서버 설치 (dnf 사용)
먼저, MySQL 서버를 설치하고 백그라운드에서 실행하도록 설정하는 과정을 진행하겠습니다.

1. **MySQL 서버 설치**:
   ```bash
   sudo dnf install mysql-server
   ```
   - 이 명령어는 MySQL 서버와 관련된 모든 패키지를 설치합니다.

2. **MySQL 서버 시작 및 자동 시작 설정**:
   설치가 완료되면 MySQL 서버를 시작하고, 시스템이 재부팅될 때 자동으로 시작되도록 설정합니다.

   ```bash
   # MySQL 서버 시작
   sudo systemctl start mysqld

   # MySQL 서버 자동 시작 설정
   sudo systemctl enable mysqld
   ```

3. **MySQL 상태 확인**:
   MySQL 서버가 올바르게 실행 중인지 확인하려면 다음 명령어를 사용합니다.

   ```bash
   sudo systemctl status mysqld
   ```

### 단계 2: MySQL 초기 설정 및 보안 설정
MySQL 설치 후 기본적인 보안 설정을 해야 합니다.

1. **보안 설정**:
   MySQL 보안 스크립트를 실행하여 기본적인 보안 설정을 완료합니다.

   ```bash
   sudo mysql_secure_installation
   ```
   - 이 스크립트를 실행하면 `root` 계정 비밀번호 설정, 익명 사용자 제거, 원격 root 로그인 비활성화, 테스트 데이터베이스 제거 등의 옵션을 제공합니다. 필요에 따라 설정합니다.

### 단계 3: MySQL 접속 및 사용자 생성
MySQL에 접속하여 새로운 사용자를 생성하고 특정 데이터베이스에 대해 권한을 부여하겠습니다.

1. **MySQL 접속**:
   ```bash
   mysql -u root -p
   ```
   `root` 비밀번호를 입력하여 접속합니다.

2. **데이터베이스 생성**:
   새 사용자가 사용할 데이터베이스를 생성합니다.

   ```sql
   CREATE DATABASE my_schema;
   ```

3. **새 사용자 생성 및 권한 부여**:
   - 새로운 사용자를 생성하고 특정 스키마(`my_schema`)에 대한 모든 권한을 부여합니다.
   - 여기서 새 사용자 이름을 `'new_user'`, 비밀번호를 `'user_password'`로 설정합니다. 외부 어느 IP에서든 접근 가능하도록 `%`를 사용합니다.

   ```sql
   CREATE USER 'new_user'@'%' IDENTIFIED BY 'user_password';
   GRANT ALL PRIVILEGES ON my_schema.* TO 'new_user'@'%' WITH GRANT OPTION;
   FLUSH PRIVILEGES;
   ```
   - `CREATE USER` 명령어를 통해 `'new_user'` 사용자를 생성하고, `%`를 사용하여 모든 IP 주소에서 접근 가능하도록 설정합니다.
   - `GRANT ALL PRIVILEGES ON my_schema.*` 명령어를 통해 사용자가 `my_schema` 데이터베이스 내에서 테이블 생성, 삭제, 수정 등의 모든 권한을 갖도록 설정합니다.

### 단계 4: MySQL 설정 파일 수정 (`bind-address`)
외부 IP에서 MySQL 서버에 접근할 수 있도록 설정을 변경합니다.

1. **설정 파일 열기**:
   MySQL 설정 파일을 열어 `bind-address`를 설정합니다. 보통 이 파일은 `/etc/my.cnf` 또는 `/etc/mysql/my.cnf`에 위치해 있습니다.

   ```bash
   sudo nano /etc/my.cnf
   ```
   또는
   ```bash
   sudo nano /etc/mysql/my.cnf
   ```

2. **`bind-address` 설정 변경**:
   아래와 같이 설정을 추가하거나 수정합니다.

   ```conf
   [mysqld]
   bind-address = 0.0.0.0
   ```

   이 설정은 MySQL 서버가 **모든 IP 주소**에서 접근할 수 있도록 허용합니다.

3. **MySQL 서버 재시작**:
   설정 파일을 변경한 후에는 MySQL 서버를 재시작하여 설정이 적용되도록 합니다.

   ```bash
   sudo systemctl restart mysqld
   ```

### 단계 5: 방화벽 설정 (포트 열기)
MySQL 서버가 외부에서 접근 가능하도록 방화벽 설정을 변경합니다.

1. **방화벽에서 MySQL 포트 열기**:
   MySQL은 기본적으로 **3306번 포트**를 사용합니다. 방화벽에서 해당 포트를 허용해야 합니다.

   ```bash
   sudo firewall-cmd --permanent --add-port=3306/tcp
   sudo firewall-cmd --reload
   ```

   위 명령은 MySQL 포트(3306)를 TCP로 열고, 변경 사항을 적용하는 역할을 합니다.

### 단계 6: 포트 확인 방법
리눅스에서 특정 포트가 열려 있고 MySQL 서버가 올바르게 리스닝하고 있는지 확인하려면 다음 명령어를 사용할 수 있습니다.

1. **netstat 또는 ss 사용**:
   - **netstat**를 사용하는 방법:

   ```bash
   sudo netstat -tuln | grep 3306
   ```
   - **ss**를 사용하는 방법:

   ```bash
   sudo ss -tuln | grep 3306
   ```

   위 명령어는 MySQL 서버가 **포트 3306**에서 리스닝하고 있는지 확인할 수 있습니다. 결과에서 `0.0.0.0:3306`과 같은 형태로 표시되면 모든 IP에서 접근 가능한 상태임을 의미합니다.

### 요약
1. **MySQL 서버 설치 및 설정**:
   - `dnf`를 통해 MySQL 서버 설치 (`sudo dnf install mysql-server`).
   - MySQL 서버 시작 및 자동 시작 설정 (`systemctl start mysqld`, `systemctl enable mysqld`).
2. **새 사용자 생성 및 권한 부여**:
   - `CREATE USER 'new_user'@'%' IDENTIFIED BY 'user_password'`로 사용자 생성.
   - 특정 스키마(`my_schema`)에 대해 `GRANT ALL PRIVILEGES` 명령으로 모든 권한 부여.
3. **MySQL 설정 파일 수정**:
   - `bind-address`를 `0.0.0.0`으로 설정하여 모든 IP 접근 허용.
4. **방화벽 설정**:
   - 방화벽에서 MySQL 포트(3306) 허용 (`firewall-cmd` 사용).
5. **포트 확인**:
   - `netstat` 또는 `ss`를 사용하여 포트 3306이 열려 있는지 확인.
