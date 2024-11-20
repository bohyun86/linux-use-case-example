### GRUB(Grand Unified Bootloader) 개요

**GRUB**는 부트로더(boot loader)로, 컴퓨터를 부팅하는 과정에서 운영 체제를 선택하고 실행하는 중요한 역할을 합니다. 운영 체제를 로드하기 전에 시스템 하드웨어를 초기화하고 사용자가 선택한 운영 체제 커널을 메모리에 적재한 후 제어를 넘깁니다.

GRUB는 현재 **GRUB Legacy(1.x)**와 **GRUB 2(2.x)** 버전으로 나뉘며, 대부분의 현대 시스템에서는 **GRUB 2**를 사용합니다.

---

### GRUB의 주요 기능

1. **다중 부팅 지원**  
   - GRUB은 여러 운영 체제를 설치한 경우 사용자에게 선택 메뉴를 제공하여 부팅할 OS를 선택할 수 있도록 합니다.

2. **고급 부팅 옵션 제공**  
   - 커널 파라미터를 전달하거나, 복구 모드로 부팅하는 등의 고급 부팅 옵션을 설정할 수 있습니다.

3. **파일 시스템 탐색**  
   - GRUB은 파일 시스템을 직접 탐색할 수 있어, 운영 체제의 커널이나 초기 램 디스크(initrd)를 로드할 수 있습니다.

4. **유연한 구성**  
   - GRUB 구성 파일(`grub.cfg` 또는 `menu.lst`)을 수정하여 부팅 옵션, 부트 메뉴, 시간 초과 설정 등을 커스터마이징할 수 있습니다.

5. **암호화 및 보안**  
   - 부팅 메뉴에 암호를 설정하여 특정 부팅 항목에 대한 접근을 제한할 수 있습니다.

---

### GRUB 2의 주요 특징

**GRUB 2**는 GRUB Legacy와 비교하여 더 강력하고 유연한 기능을 제공합니다:

- **모듈식 아키텍처**: 필요한 기능만 모듈로 로드하여 부팅 프로세스를 최적화.
- **다양한 파일 시스템 지원**: ext4, Btrfs, XFS, NTFS 등 다양한 파일 시스템을 지원.
- **스크립트 기반 설정**: `/etc/default/grub` 및 `/etc/grub.d/` 디렉토리의 스크립트를 기반으로 GRUB 설정 파일(`grub.cfg`)을 자동 생성.
- **그래픽 메뉴 지원**: 더 나은 사용자 경험을 제공하는 테마 및 그래픽 인터페이스 지원.
- **UEFI 및 BIOS 지원**: UEFI와 전통적인 BIOS 시스템에서 모두 작동 가능.
- **LVM 및 소프트웨어 RAID 지원**: LVM(논리 볼륨 관리) 및 RAID 장치에서 부팅 지원.

---

### GRUB 2로 할 수 있는 일

1. **다중 부팅 환경 구성**
   - 여러 운영 체제를 설치하고 부팅 시 선택할 수 있도록 설정 가능.
   - 예:
     - Windows와 Linux의 멀티부팅 설정.
     - Ubuntu, Fedora, Arch Linux와 같은 다중 Linux 설치.

2. **기본 부팅 OS 설정**
   - 특정 운영 체제를 기본값으로 설정하거나 부팅 메뉴에서 시간 초과를 설정하여 자동으로 부팅되도록 구성.
   - `GRUB_DEFAULT=0` 또는 `GRUB_TIMEOUT=10` 설정을 통해 제어.

3. **복구 모드 부팅**
   - GRUB 메뉴에서 커널 파라미터를 수정하여 특정 커널로 부팅하거나 복구 모드로 부팅 가능.
   - 예: `init=/bin/bash`를 사용해 루트 쉘을 제공.

4. **커널 파라미터 설정**
   - 커널에 특정 파라미터를 전달하여 하드웨어 설정이나 디버깅 환경을 제어.
   - 예: `nomodeset`(그래픽 문제 해결), `quiet`(부팅 메시지 숨기기).

5. **부팅 메뉴 암호화**
   - GRUB 메뉴의 항목에 암호를 설정하여 승인된 사용자만 부팅할 수 있도록 보안 강화.
   - 예: 
     ```bash
     set superusers="admin"
     password admin secret_password
     ```

6. **테마 및 그래픽 설정**
   - 사용자 정의 테마를 설정하여 부팅 메뉴를 더 보기 좋게 만들 수 있습니다.
   - `/boot/grub/themes/` 경로에 테마를 추가하고 활성화.

7. **RAM 디스크 초기화**
   - 특정 `initrd`를 로드하여 특정 초기화 작업이나 커널 문제 해결 가능.

8. **UEFI 환경에서의 부팅 관리**
   - EFI 파티션의 항목을 관리하여 UEFI 부팅 옵션을 설정 및 관리.

---

### GRUB 2 설정 파일

1. **주요 파일 경로**
   - `/etc/default/grub`: GRUB 기본 설정 파일.
   - `/etc/grub.d/`: 추가 스크립트 파일.
   - `/boot/grub2/grub.cfg`: GRUB이 실제로 사용하는 최종 설정 파일. `grub2-mkconfig`로 생성.

2. **`/etc/default/grub`의 주요 설정 옵션**
   - `GRUB_DEFAULT`: 기본 부팅 항목(0부터 시작하는 인덱스).
   - `GRUB_TIMEOUT`: 부팅 메뉴 시간 초과(초 단위).
   - `GRUB_CMDLINE_LINUX`: 커널에 전달할 커맨드라인 파라미터.

3. **설정 파일 업데이트**
   - `/etc/default/grub` 또는 `/etc/grub.d/`의 스크립트를 수정한 후, 아래 명령어로 설정을 반영:
     ```bash
     grub2-mkconfig -o /boot/grub2/grub.cfg
     ```

---

### GRUB 2 사용 예제

#### 1. 다중 부팅 구성
Windows와 Linux 멀티부팅 환경에서 Linux가 기본 부팅이 되도록 설정:
```bash
GRUB_DEFAULT=0
GRUB_TIMEOUT=5
GRUB_CMDLINE_LINUX="quiet splash"
```
적용 후 업데이트:
```bash
grub2-mkconfig -o /boot/grub2/grub.cfg
```

#### 2. 암호화된 부팅 항목
특정 부팅 항목에 암호를 설정:
```bash
set superusers="admin"
password admin secret_password
```
이 설정은 `/etc/grub.d/40_custom`에 추가하고 적용:
```bash
grub2-mkconfig -o /boot/grub2/grub.cfg
```

#### 3. 커널 파라미터 추가
NVIDIA 그래픽 드라이버 문제 해결을 위해 `nomodeset` 파라미터 추가:
```bash
GRUB_CMDLINE_LINUX="quiet splash nomodeset"
```
업데이트 후 재부팅.

#### 4. 테마 설정
테마를 `/boot/grub/themes/my_theme/`에 추가한 후 설정:
```bash
GRUB_THEME="/boot/grub/themes/my_theme/theme.txt"
```
적용:
```bash
grub2-mkconfig -o /boot/grub2/grub.cfg
```

---

### 요약
GRUB 2는 운영 체제 부팅 과정을 유연하고 강력하게 제어할 수 있는 부트로더입니다. 다중 부팅 설정, 커널 파라미터 전달, 복구 모드 부팅, 암호화 설정 등 다양한 기능을 제공합니다. 설정 파일 수정 후 항상 `grub2-mkconfig` 명령어로 변경 사항을 반영해야 합니다.