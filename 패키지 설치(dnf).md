### dnf 사용법

`dnf`는 Fedora, Red Hat, CentOS 등의 배포판에서 사용되는 패키지 관리 도구입니다. DNF는 이전의 YUM(또는 Yellowdog Updater, Modified)을 대체한 패키지 관리자입니다. 이 도구를 사용하면 소프트웨어 패키지를 쉽게 설치, 업데이트, 제거할 수 있으며, 의존성 문제를 자동으로 해결하는 기능도 갖추고 있습니다. `dnf`는 YUM보다 빠르고 메모리 사용량이 적으며, 더 나은 의존성 해결 알고리즘을 사용합니다.

아래에서 `dnf`의 주요 명령어들과 사용 예시를 설명하겠습니다.

### 1. **패키지 설치**
- **기본 패키지 설치:**
  ```bash
  dnf install 패키지이름
  ```
  예: `dnf install vim` — Vim 편집기를 설치합니다.

- **질문 없이 설치 진행:**
  ```bash
  dnf -y install 패키지이름
  ```
  예: `dnf -y install httpd` — 질문 없이 Apache HTTP Server를 설치합니다.

- **여러 패키지 동시 설치:**
  ```bash
  dnf install 패키지1 패키지2
  ```
  예: `dnf install nginx mariadb-server` — Nginx와 MariaDB 서버를 동시에 설치합니다.

- **특정 버전 설치:**
  ```bash
  dnf install 패키지이름-버전
  ```
  예: `dnf install kernel-4.18.0` — 커널의 특정 버전을 설치합니다.

- **의존성 최적화하지 않고 설치:**
  ```bash
  dnf install 패키지이름 --nobest
  ```
  예: `dnf install httpd --nobest` — 최적의 의존성이 아닌 경우에도 설치 진행.

- **의존성 충돌 시 삭제 후 설치:**
  ```bash
  dnf install 패키지이름 --allowerasing
  ```
  예: `dnf install new_package --allowerasing` — 충돌되는 패키지를 삭제하고 새 패키지를 설치합니다.

- **의존성 문제 무시하고 설치:**
  ```bash
  dnf install 패키지이름 --skip-broken
  ```
  예: `dnf install vim --skip-broken` — 의존성 문제를 무시하고 가능한 파일을 설치합니다.

- **약한 의존성 무시:**
  ```bash
  dnf install 패키지이름 --setopt=install_weak_deps=False
  ```
  예: `dnf install git --setopt=install_weak_deps=False` — Git 설치 시 약한 의존성은 무시합니다.

- **문서 파일 설치 생략:**
  ```bash
  dnf install 패키지이름 --setopt=tsflags=nodocs
  ```
  예: `dnf install python --setopt=tsflags=nodocs` — Python 설치 시 문서 파일을 생략합니다.

- **패키지 삭제 시 의존성 패키지도 삭제:**
  ```bash
  dnf install 패키지이름 --setopt=clean_requirements_on_remove=True
  ```
  예: `dnf install nginx --setopt=clean_requirements_on_remove=True` — Nginx 삭제 시 필요없는 의존성도 제거합니다.

- **최신 3개 버전만 유지:**
  ```bash
  dnf install 패키지이름 --setopt=installonly_limit=3
  ```
  예: `dnf install kernel --setopt=installonly_limit=3` — 커널의 최신 3개 버전만 유지합니다.

- **특정 패키지만 최신 버전 유지:**
  ```bash
  dnf install 패키지이름 --setopt=installonlypkgs=패키지이름
  ```
  예: `dnf install my_package --setopt=installonlypkgs=my_package` — 해당 패키지의 최신 버전만 유지합니다.

- **GPG 체크 생략:**
  ```bash
  dnf install --nogpgcheck 패키지이름
  ```
  예: `dnf install --nogpgcheck custom-package` — GPG 서명 확인 없이 패키지를 설치합니다.

### 2. **그룹 패키지 관련 명령어**
- **그룹 패키지 설치:**
  ```bash
  dnf groupinstall "Development Tools"
  ```
  — 개발 도구 그룹 패키지를 설치합니다.

- **그룹 패키지 삭제:**
  ```bash
  dnf groupremove "Development Tools"
  ```
  — 개발 도구 그룹 패키지를 삭제합니다.

- **그룹 패키지 업데이트:**
  ```bash
  dnf groupupdate "Server with GUI"
  ```
  — "GUI가 있는 서버" 그룹 패키지를 업데이트합니다.

### 3. **패키지 관리 명령어**
- **파일이 포함된 패키지 확인:**
  ```bash
  dnf provides /usr/bin/wget
  ```
  — `/usr/bin/wget` 파일을 제공하는 패키지를 찾습니다.

- **패키지 삭제:**
  ```bash
  dnf remove 패키지이름
  ```
  예: `dnf remove vim` — Vim 패키지를 삭제합니다.

  또는 `dnf erase`도 동일한 효과를 가집니다:
  ```bash
  dnf erase 패키지이름
  ```

- **더 이상 필요하지 않은 패키지 삭제:**
  ```bash
  dnf autoremove
  ```
  — 시스템에서 더 이상 사용되지 않는 패키지를 자동으로 삭제합니다.

- **캐시 파일 삭제:**
  ```bash
  dnf clean all
  ```
  — 캐시 파일을 모두 삭제하여 공간을 절약합니다.

### 4. **패키지 정보 확인 명령어**
- **설치된 패키지 목록 출력:**
  ```bash
  dnf list installed
  ```
  — 현재 시스템에 설치된 모든 패키지 목록을 출력합니다.

- **설치 가능한 패키지 목록 출력:**
  ```bash
  dnf list available
  ```
  — 설치 가능한 모든 패키지 목록을 출력합니다.

- **업데이트 가능한 패키지 목록 출력:**
  ```bash
  dnf list updates
  ```
  — 현재 시스템에서 업데이트할 수 있는 패키지를 출력합니다.

- **업데이트 가능한 패키지 목록 확인:**
  ```bash
  dnf check-update
  ```
  — 업데이트 가능한 패키지가 있는지 확인합니다.

### 5. **패키지 업그레이드/다운그레이드 명령어**
- **모든 패키지 업그레이드:**
  ```bash
  dnf upgrade
  ```
  — 시스템의 모든 패키지를 최신 버전으로 업그레이드합니다.

- **특정 패키지 업그레이드:**
  ```bash
  dnf upgrade 패키지이름
  ```
  예: `dnf upgrade nginx` — Nginx 패키지만 업그레이드합니다.

- **패키지 다운그레이드:**
  ```bash
  dnf downgrade 패키지이름
  ```
  예: `dnf downgrade nginx` — Nginx의 이전 버전으로 다운그레이드합니다.

### 6. **기타 패키지 관련 정보 확인**
- **패키지 정보 확인:**
  ```bash
  dnf info 패키지이름
  ```
  예: `dnf info httpd` — Apache HTTP Server 패키지의 상세 정보를 확인합니다.

- **캐시 데이터 삭제:**
  ```bash
  dnf clean all
  ```
  — DNF가 다운로드한 기존 패키지와 메타데이터를 삭제하여 캐시를 정리합니다.

### 7. **DNF 명령어 사용 예시**
1. **Apache 웹 서버 설치:**
   ```bash
   dnf install httpd
   ```

2. **Nginx와 MariaDB 동시 설치:**
   ```bash
   dnf install nginx mariadb-server
   ```

3. **특정 패키지의 정보 확인:**
   ```bash
   dnf info git
   ```

4. **더 이상 필요 없는 패키지 제거:**
   ```bash
   dnf autoremove
   ```

5. **시스템의 모든 패키지를 최신 버전으로 업데이트:**
   ```bash
   dnf upgrade
   ```



이러한 `dnf` 명령어들을 사용하면 Linux 시스템에서 패키지 설치, 업데이트, 삭제를 보다 효율적으로 관리할 수 있습니다. `dnf`는 의존성을 자동으로 처리해 주기 때문에 설치하고자 하는 패키지와 연관된 다른 라이브러리나 프로그램도 자동으로 설치되며, 이 덕분에 매우 편리하게 소프트웨어를 관리할 수 있습니다.
