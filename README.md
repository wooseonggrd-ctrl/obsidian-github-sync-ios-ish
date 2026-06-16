# Sync Obsidian Vaults with GitHub on Desktop and iPhone

[English](#english) | [한국어](#한국어)

This guide explains how to sync an Obsidian vault across macOS, Windows, and iPhone using GitHub.
On iPhone, it uses the iSH mount method, so Working Copy is not required.

Example names used in this guide:

```text
Vault name: vault_example
GitHub username: <github-username>
Repository name: <repo-name>
Repository URL: https://github.com/<github-username>/<repo-name>.git
```

---

# English

## Recommended workflow

```text
Mac/Windows:
Main editing devices.
Use the Obsidian Git plugin or regular Git commands to commit and push changes.

GitHub:
Acts as the source repository for the vault.

iPhone:
Secondary viewing device.
Mostly pull and read notes.
Make small edits only when needed, then commit and sync immediately.
```

Basic rules:

```text
Pull before editing.
Commit and sync after editing.
Do not edit the same file on multiple devices at the same time.
```

On iPhone, manual `Pull` and manual `Commit-and-sync` are recommended over aggressive automatic syncing.

---

## Overall structure

```text
GitHub
└── <repo-name>

Mac or Windows
└── Use the cloned repository as an Obsidian vault

iPhone
└── On My iPhone / Obsidian / vault_example
    ├── .git
    ├── .obsidian
    ├── README.md
    └── notes...
```

Do not use iCloud sync and GitHub sync on the same vault at the same time.
On iPhone, keep iCloud sync off and use GitHub as the single sync source.

---

## 1. Create the vault on macOS or Windows

If the GitHub repository already exists, clone it and open the cloned folder as an Obsidian vault.

### macOS example

```bash
mkdir -p ~/Obsidian
cd ~/Obsidian
git clone https://github.com/<github-username>/<repo-name>.git
```

Open the folder in Obsidian:

```text
Open folder as vault
→ ~/Obsidian/<repo-name>
```

### Windows example

Use Git Bash, PowerShell, or another terminal:

```bash
cd ~/Documents
git clone https://github.com/<github-username>/<repo-name>.git
```

Open the folder in Obsidian:

```text
Open folder as vault
→ Documents/<repo-name>
```

---

## 2. Install the Obsidian Git plugin on desktop

In Obsidian:

```text
Settings
→ Community plugins
→ Turn off Restricted mode
→ Browse
→ Search for Git
→ Install the Git plugin by Vinzent
```

Plugin information:

```text
Name: Git
Author: Vinzent
Plugin ID: obsidian-git
```

Recommended settings:

```text
Auto pull on startup: ON
Pull before push: ON
Auto push: OFF, or use carefully
Vault backup interval: manual or 10-30 minutes
```

At first, using `Git: Commit-and-sync` manually is safer than relying on full automation.

---

## 3. Create a GitHub Personal Access Token

To push from iPhone, use a GitHub Personal Access Token instead of your GitHub password.

In GitHub:

```text
Settings
→ Developer settings
→ Personal access tokens
→ Fine-grained tokens
→ Generate new token
```

Recommended permissions:

```text
Repository access:
Only selected repositories
→ Select the repository you want to sync

Permissions:
Contents: Read and write
Metadata: Read
```

Save the token somewhere safe. It is usually shown only once.

---

## 4. Create an empty Obsidian vault on iPhone

In Obsidian for iPhone:

```text
Create new vault
→ Vault name: vault_example
→ Store in iCloud: OFF
→ Create
```

This creates an empty vault folder. The GitHub repository will be cloned directly into this folder later.

---

## 5. Install Git in iSH

Install the iSH Shell app on iPhone.

Open iSH and run:

```sh
apk update
apk add git
```

---

## 6. Mount the Obsidian vault folder in iSH

In iSH, run:

```sh
mkdir -p /mnt/obsidian
mount -t ios . /mnt/obsidian
```

An iOS folder picker will appear.
Select the empty Obsidian vault folder:

```text
On My iPhone
→ Obsidian
→ vault_example
```

After selecting it, check the mounted folder in iSH:

```sh
cd /mnt/obsidian
ls -a
```

---

## 7. Clone the GitHub repository into the mounted vault

Run the following commands inside iSH:

```sh
cd /mnt/obsidian
rm -rf .obsidian .trash
git clone https://github.com/<github-username>/<repo-name>.git .
```

The final `.` is important.
It means the repository will be cloned into the current folder itself.

Correct structure:

```text
On My iPhone / Obsidian / vault_example / .git
On My iPhone / Obsidian / vault_example / .obsidian
On My iPhone / Obsidian / vault_example / README.md
On My iPhone / Obsidian / vault_example / notes...
```

Incorrect structure:

```text
On My iPhone / Obsidian / vault_example / <repo-name> / .git
```

---

## 8. Check the vault in Obsidian on iPhone

Fully close and reopen Obsidian on iPhone.

```text
Open the vault_example vault
→ Check the file list
→ Check the graph view
```

If the files and graph view appear correctly, the iPhone setup is complete.

---

## 9. Install the Git plugin on iPhone

In Obsidian for iPhone:

```text
Settings
→ Community plugins
→ Browse
→ Search for Git
→ Check that the author is Vinzent
→ Install and enable the plugin
```

Set the following values:

```text
Author name: <github-username>
Author email: <github-email>
Username: <github-username>
Password / Token: <github-personal-access-token>
```

Recommended iPhone settings:

```text
Auto pull on startup: ON
Pull before push: ON
Auto push: OFF
Vault backup interval: OFF or long interval
```

---

## Daily usage

### On macOS or Windows

Before editing:

```sh
git pull
```

After editing:

```sh
git add .
git commit -m "docs: update notes"
git push
```

Or use the Obsidian Git command:

```text
Git: Commit-and-sync
```

### On iPhone, when reading notes

```text
Open Obsidian
→ Git: Pull
→ Read notes
```

### On iPhone, when editing notes

```text
Git: Pull
→ Edit notes
→ Git: Commit-and-sync
```

---

## Remove the old repository inside iSH, if any

If you previously cloned the repository inside the iSH root directory, it can be removed after the mounted vault setup works.

Check the status first:

```sh
cd ~/<repo-name>
git status
```

If there are no needed changes, remove it:

```sh
cd ~
rm -rf <repo-name>
```

Keep this folder:

```text
On My iPhone / Obsidian / vault_example
```

That folder is the actual Obsidian vault on iPhone.

---

## Recommended .gitignore

```gitignore
# macOS system files
.DS_Store

# Obsidian local UI state
.obsidian/workspace.json
.obsidian/workspace-mobile.json

# Obsidian trash
.trash/

# Obsidian Git plugin local settings / credentials
.obsidian/plugins/obsidian-git/data.json
```

Ignoring the entire `.obsidian` directory is not recommended.
If you ignore all of `.obsidian`, plugin lists, themes, and basic settings must be managed separately on each device.

---

# 한국어

## 추천 운영 방식

```text
Mac/Windows:
메인 편집 기기입니다.
Obsidian Git 플러그인 또는 일반 Git 명령어로 commit/push합니다.

GitHub:
Vault의 원본 저장소 역할을 합니다.

iPhone:
보조 확인 기기입니다.
주로 Pull해서 읽는 용도로 사용합니다.
필요할 때만 짧게 수정하고 바로 Commit-and-sync합니다.
```

기본 규칙:

```text
수정 전 Pull
수정 후 Commit-and-sync
여러 기기에서 같은 파일을 동시에 수정하지 않기
```

iPhone에서는 강한 자동 동기화보다 수동 `Pull`과 수동 `Commit-and-sync` 중심으로 사용하는 것을 추천합니다.

---

## 전체 구조

```text
GitHub
└── <repo-name>

Mac or Windows
└── clone한 repository를 Obsidian vault로 사용

iPhone
└── On My iPhone / Obsidian / vault_example
    ├── .git
    ├── .obsidian
    ├── README.md
    └── notes...
```

같은 vault에 iCloud 동기화와 GitHub 동기화를 동시에 사용하지 않는 것이 좋습니다.
iPhone에서는 iCloud 동기화를 끄고 GitHub만 단일 동기화 기준으로 사용합니다.

---

## 1. macOS 또는 Windows에서 vault 만들기

이미 GitHub repository가 있다면 새 vault를 만드는 것보다 기존 repository를 clone해서 Obsidian vault로 여는 방식이 좋습니다.

### macOS 예시

```bash
mkdir -p ~/Obsidian
cd ~/Obsidian
git clone https://github.com/<github-username>/<repo-name>.git
```

Obsidian에서 다음 폴더를 엽니다.

```text
Open folder as vault
→ ~/Obsidian/<repo-name>
```

### Windows 예시

Git Bash, PowerShell, 또는 다른 터미널에서 실행합니다.

```bash
cd ~/Documents
git clone https://github.com/<github-username>/<repo-name>.git
```

Obsidian에서 다음 폴더를 엽니다.

```text
Open folder as vault
→ Documents/<repo-name>
```

---

## 2. 데스크톱에서 Obsidian Git 플러그인 설치

Obsidian에서 다음 순서로 설치합니다.

```text
Settings
→ Community plugins
→ Restricted mode 끄기
→ Browse
→ Git 검색
→ Vinzent가 만든 Git 플러그인 설치
```

플러그인 정보는 다음과 같습니다.

```text
Name: Git
Author: Vinzent
Plugin ID: obsidian-git
```

추천 설정:

```text
Auto pull on startup: ON
Pull before push: ON
Auto push: OFF 또는 신중하게 사용
Vault backup interval: 수동 또는 10-30분
```

처음에는 완전 자동화보다 `Git: Commit-and-sync`를 수동으로 사용하는 것이 안전합니다.

---

## 3. GitHub Personal Access Token 만들기

iPhone에서 push하려면 GitHub 비밀번호 대신 Personal Access Token을 사용해야 합니다.

GitHub에서 다음 경로로 이동합니다.

```text
Settings
→ Developer settings
→ Personal access tokens
→ Fine-grained tokens
→ Generate new token
```

추천 권한:

```text
Repository access:
Only selected repositories
→ 동기화할 repository 선택

Permissions:
Contents: Read and write
Metadata: Read
```

토큰은 보통 한 번만 보이므로 안전한 곳에 저장해둡니다.

---

## 4. iPhone에서 빈 Obsidian vault 만들기

iPhone Obsidian에서 다음처럼 빈 vault를 만듭니다.

```text
Create new vault
→ Vault name: vault_example
→ Store in iCloud: OFF
→ Create
```

이 단계에서는 빈 vault 폴더만 만들어집니다.
이후 이 폴더 안에 GitHub repository를 직접 clone합니다.

---

## 5. iSH에 Git 설치

iPhone에 iSH Shell 앱을 설치합니다.

iSH를 열고 다음 명령을 실행합니다.

```sh
apk update
apk add git
```

---

## 6. iSH에서 Obsidian vault 폴더 mount

iSH에서 다음 명령을 실행합니다.

```sh
mkdir -p /mnt/obsidian
mount -t ios . /mnt/obsidian
```

iOS 폴더 선택 화면이 뜨면 빈 Obsidian vault 폴더를 선택합니다.

```text
On My iPhone
→ Obsidian
→ vault_example
```

선택 후 iSH에서 확인합니다.

```sh
cd /mnt/obsidian
ls -a
```

---

## 7. mount된 vault 폴더에 GitHub repository clone

iSH에서 다음 명령을 실행합니다.

```sh
cd /mnt/obsidian
rm -rf .obsidian .trash
git clone https://github.com/<github-username>/<repo-name>.git .
```

마지막의 `.`이 중요합니다.
현재 폴더 자체에 repository를 clone한다는 뜻입니다.

정상 구조:

```text
On My iPhone / Obsidian / vault_example / .git
On My iPhone / Obsidian / vault_example / .obsidian
On My iPhone / Obsidian / vault_example / README.md
On My iPhone / Obsidian / vault_example / notes...
```

잘못된 구조:

```text
On My iPhone / Obsidian / vault_example / <repo-name> / .git
```

---

## 8. iPhone Obsidian에서 확인

iPhone에서 Obsidian을 완전히 종료했다가 다시 엽니다.

```text
vault_example vault 열기
→ 파일 목록 확인
→ 그래프 뷰 확인
```

파일과 그래프 뷰가 정상적으로 보이면 iPhone 설정은 완료된 것입니다.

---

## 9. iPhone에도 Git 플러그인 설치

iPhone Obsidian에서도 커뮤니티 플러그인에서 `Git`을 설치합니다.

```text
Settings
→ Community plugins
→ Browse
→ Git 검색
→ Author가 Vinzent인지 확인
→ 설치 및 활성화
```

설정에는 다음 정보를 입력합니다.

```text
Author name: <github-username>
Author email: <github-email>
Username: <github-username>
Password / Token: <github-personal-access-token>
```

iPhone 추천 설정:

```text
Auto pull on startup: ON
Pull before push: ON
Auto push: OFF
Vault backup interval: OFF 또는 길게
```

---

## 실제 사용 방식

### macOS 또는 Windows에서 작업할 때

수정 전:

```sh
git pull
```

수정 후:

```sh
git add .
git commit -m "docs: update notes"
git push
```

또는 Obsidian Git 명령을 사용합니다.

```text
Git: Commit-and-sync
```

### iPhone에서 읽기만 할 때

```text
Obsidian 열기
→ Git: Pull
→ 읽기
```

### iPhone에서 수정할 때

```text
Git: Pull
→ 노트 수정
→ Git: Commit-and-sync
```

---

## iSH root에 남은 repository 정리

이전에 iSH root 안에 repository를 따로 clone했다면, mount 방식 설정이 정상 동작한 뒤에는 지워도 됩니다.

먼저 상태를 확인합니다.

```sh
cd ~/<repo-name>
git status
```

필요한 변경사항이 없다면 삭제합니다.

```sh
cd ~
rm -rf <repo-name>
```

유지해야 하는 폴더는 다음입니다.

```text
On My iPhone / Obsidian / vault_example
```

이 폴더가 실제 iPhone Obsidian vault입니다.

---

## 추천 .gitignore

```gitignore
# macOS system files
.DS_Store

# Obsidian local UI state
.obsidian/workspace.json
.obsidian/workspace-mobile.json

# Obsidian trash
.trash/

# Obsidian Git plugin local settings / credentials
.obsidian/plugins/obsidian-git/data.json
```

`.obsidian` 전체를 ignore하는 것은 비추천입니다.
전체를 ignore하면 플러그인 목록, 테마, 기본 설정을 기기마다 따로 관리해야 해서 오히려 불편해질 수 있습니다.
