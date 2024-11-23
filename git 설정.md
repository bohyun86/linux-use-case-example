
## 1. **Git 설치**
### Red Hat 계열 (CentOS, Fedora, RHEL 등)
```bash
sudo dnf install git -y
```

### Debian 계열 (Ubuntu, Debian 등)
```bash
sudo apt update
sudo apt install git -y
```

### 설치 확인
```bash
git --version
```
출력 예시:
```
git version 2.x.x
```

---

## 2. **Git 초기 설정**
Git 사용을 위해 사용자 이름과 이메일을 설정해야 합니다. 이 정보는 커밋 메시지에 포함됩니다.

### 사용자 이름 설정
```bash
git config --global user.name "Your Name"
```

### 이메일 설정
```bash
git config --global user.email "your_email@example.com"
```

### 설정 확인
```bash
git config --list
```
출력 예시:
```
user.name=Your Name
user.email=your_email@example.com
```

---

## 3. **GitHub와 SSH 연결 설정**
GitHub와 연결하기 위해 SSH 키를 생성하고 등록합니다.

### 3.1. SSH 키 생성
```bash
ssh-keygen -t ed25519 -C "your_email@example.com"
```
- 기본 저장 경로는 `~/.ssh/id_ed25519`입니다.
- "비밀번호를 입력하라"는 메시지가 나오면 **엔터 키**를 눌러 생략 가능합니다.

### 3.2. SSH 에이전트 활성화
```bash
eval "$(ssh-agent -s)"
```

### 3.3. SSH 키 추가
```bash
ssh-add ~/.ssh/id_ed25519
```

### 3.4. SSH 공개 키 확인
```bash
cat ~/.ssh/id_ed25519.pub
```
출력된 공개 키를 복사합니다.

### 3.5. GitHub에 SSH 키 등록
1. [GitHub SSH Settings](https://github.com/settings/keys)로 이동.
2. **New SSH key** 클릭.
3. 제목을 입력하고 복사한 공개 키를 붙여넣기.
4. **Add SSH key** 클릭.

---

## 4. **GitHub 리포지토리 생성**
1. [GitHub](https://github.com)에 로그인.
2. 오른쪽 상단의 **New repository** 버튼 클릭.
3. 리포지토리 이름과 설명 입력.
4. **Create repository** 클릭.

---

## 5. **로컬 디렉터리 초기화 및 연결**
GitHub 리포지토리와 연결합니다.

### 5.1. 로컬 디렉터리 생성
```bash
mkdir my-project
cd my-project
```

### 5.2. Git 초기화
```bash
git init
```

### 5.3. GitHub 리포지토리 연결
GitHub 리포지토리 URL로 원격 리포지토리를 추가합니다:
```bash
git remote add origin git@github.com:username/my-project.git
```
`username`과 `my-project`는 GitHub 계정 및 리포지토리 이름에 맞게 변경하세요.

---

## 6. **파일 업로드**
### 6.1. 파일 추가
디렉터리 안에 파일을 생성하거나 복사합니다. 예:
```bash
echo "Hello, GitHub!" > README.md
```

### 6.2. 파일 스테이징
```bash
git add .
```

### 6.3. 커밋 생성
```bash
git commit -m "Initial commit"
```

### 6.4. 파일 푸시
```bash
git push -u origin main
```

---

## 7. **파일 업로드 확인**
GitHub 리포지토리 페이지를 열어 업로드된 파일이 표시되는지 확인하세요.

---

## 자주 발생하는 문제 해결
### 1. GitHub SSH 인증 실패
- SSH 키 등록이 제대로 되었는지 확인합니다.
- GitHub와 연결 테스트:
  ```bash
  ssh -T git@github.com
  ```
  출력 예시:
  ```
  Hi username! You've successfully authenticated, but GitHub does not provide shell access.
  ```

### 2. 기본 브랜치가 `main`이 아니거나 다른 브랜치에서 작업 중
- 기본 브랜치를 `main`으로 설정:
  ```bash
  git branch -M main
  ```

---



## 1. **연결된 Remote 확인**
먼저 현재 로컬 Git 저장소에 연결된 remote repository를 확인합니다:
```bash
git remote -v
```

출력 예시:
```
origin  git@github.com:username/repository.git (fetch)
origin  git@github.com:username/repository.git (push)
```
여기서 `origin`이 remote 이름입니다. 연결을 끊으려는 remote 이름을 확인하세요.

---

## 2. **Remote 삭제**
Git에서는 `git remote remove` 명령어를 사용해 remote 연결을 끊을 수 있습니다.

### 명령어:
```bash
git remote remove <remote_name>
```

예:
```bash
git remote remove origin
```

---

## 3. **삭제 후 확인**
다시 remote 설정을 확인하여 삭제되었는지 확인합니다:
```bash
git remote -v
```

출력:
```
(아무것도 출력되지 않음)
```

---

## 4. **다른 Remote 제거 (필요 시)**
만약 여러 개의 remote가 설정되어 있다면 각각에 대해 위 과정을 반복하여 삭제할 수 있습니다.

---

## 추가 정보
### 1. **remote 이름 변경 (대신 끊지 않고 변경)**
만약 remote 연결을 끊지 않고 이름만 변경하고 싶다면:
```bash
git remote rename <old_name> <new_name>
```
예:
```bash
git remote rename origin upstream
```

---

### 2. **remote 재설정**
나중에 다시 remote repository를 연결하려면:
```bash
git remote add origin <repository_url>
```
예:
```bash
git remote add origin git@github.com:username/new-repository.git
```

---


