# LinM

## Introduction

LinM is a text-mode visual file manager for Linux — a clone of Mdir, the famous file manager from the MS-DOS era.
It inherits the keyboard shortcuts and screen layout from Mdir for a familiar experience.

**Features:**
- File copy, move, delete, and directory tree navigation
- Built-in viewer and editor
- FTP / SFTP support
- Archive browsing: tar, zip, alz, RPM, DEB
- Sub-shell (Ctrl+O)
- Real-time file name filter (Alt+F)
- Mouse support (optional)
- Multi-panel view

Original project: https://github.com/la9527/linm  
Bug reports / contact: zirize@gmail.com (fork) / la9527@daum.net (original)

---

## Requirements

| Library | Min Version | Notes |
|---------|-------------|-------|
| ncursesw | 5.3+ | Wide-char ncurses required |
| cmake | 3.x+ | Build system |
| openssl | 0.9.6+ | SFTP support |
| libssh2 | any | SFTP support |
| source-highlight | any | Syntax highlighting (optional) |

### Debian / Ubuntu

```bash
sudo apt-get update
sudo apt-get install -y build-essential cmake git gettext \
    libncursesw5-dev libssl-dev libssh2-1-dev \
    libsource-highlight-dev
```

### Arch Linux

```bash
sudo pacman -S --needed base-devel cmake git gettext \
    ncurses openssl libssh2 source-highlight
```

### Fedora / RHEL / CentOS

```bash
# Fedora
sudo dnf install -y gcc-c++ cmake git make gettext \
    ncurses-devel openssl-devel libssh2-devel source-highlight-devel

# CentOS/RHEL (enable EPEL first)
sudo yum install -y gcc-c++ cmake3 git make gettext \
    ncurses-devel openssl-devel libssh2-devel source-highlight-devel
```

---

## Building & Installing

### User-local install (recommended, no sudo)

```bash
git clone https://github.com/la9527/linm.git
cd linm

mkdir -p build && cd build

cmake -DCMAKE_INSTALL_PREFIX=$HOME/.local ..
make
make install
```

Make sure `$HOME/.local/bin` is in your `PATH`:

```bash
# Add to ~/.bashrc or ~/.bash_profile
export PATH="$HOME/.local/bin:$PATH"
```

### System-wide install

```bash
mkdir -p build && cd build
cmake -DCMAKE_INSTALL_PREFIX=/usr/local ..
make
sudo make install
```

### CMake options

| Option | Default | Description |
|--------|---------|-------------|
| `CMAKE_INSTALL_PREFIX` | `/usr/local` | Install prefix |
| `LINM_CFGPATH` | `<prefix>/etc/linm` | System config file path |

---

## Post-install: linm.sh path fix

To preserve the current directory on exit, run linm via the wrapper script (`linm.sh`) rather than the binary directly.

Open `$HOME/.local/bin/linm.sh` and update the exec line to the actual binary path:

```bash
/home/<username>/.local/bin/linm $@
```

---

## Bash alias

```bash
# Add to ~/.bash_aliases
alias linm='. $HOME/.local/bin/linm.sh'
```

The leading `.` (source) is required so that the directory change is applied to the current shell session.

```bash
source ~/.bash_aliases
```

---

## Configuration

Config files are stored in `$HOME/.config/linm/`:

| File | Purpose |
|------|---------|
| `default.cfg` | General settings (sort, mouse, transparency, …) |
| `keyset.cfg` | Key bindings |
| `colorset.cfg` | Color scheme |
| `syntexset.cfg` | Syntax highlighting rules |

When the version changes, LinM automatically merges new keys from the system defaults into your config, backs up the old files to `back/`, and restarts itself — your customizations are never overwritten.

---

## Key Bindings

### Panel (file manager)

| Key | Action |
|-----|--------|
| `F1` | Help |
| `F2` | Rename |
| `F3` | View file |
| `F4` | Editor |
| `F5` | Refresh |
| `F7` | Make directory |
| `F8` / `Alt+D` | Delete |
| `F10` | MCD (directory tree) |
| `F11` | QCD (quick change dir) |
| `F12` | Menu |
| `Alt+S` | Sort mode popup (None/Name/Ext/Size/Time/Color) |
| `Alt+F` | File name filter (real-time, dirs excluded, persists across navigation) |
| `Alt+C` | Copy |
| `Alt+M` | Move |
| `Alt+Z` | Toggle hidden files |
| `Alt+X` / `Ctrl+Q` / `Ctrl+D` | Quit |
| `Ctrl+A` | Select all |
| `Ctrl+U` | Invert selection |
| `Space` | Toggle select |
| `Ctrl+W` | Split / unsplit panel |
| `Tab` / `Ctrl+E` | Switch to next panel |
| `Ctrl+O` | Sub-shell (Ctrl+O to return) |
| `Ctrl+B` | Settings |
| `Ctrl+F` | Find file |
| `BS` | Parent directory |
| `\` | Root directory |
| `~` | Home directory |
| `Alt+Q` / `Alt+W` | Back / Forward directory |
| `Ctrl+R` | FTP/SFTP quick connect |
| `Alt+R` | FTP/SFTP disconnect |

### MCD (directory tree)

| Key | Action |
|-----|--------|
| `F3` | Scan all directories |
| `F4` | Scan depth 3 |
| `F5` | Refresh |
| `F8` | Remove directory |
| `F9` | Directory size info |

---

## Mouse Control

Mouse input can be disabled in several ways:

```bash
# Command-line option
linm --nomouse
linm -M

# Environment variable
LINM_NO_MOUSE=1 linm

# In default.cfg
Mouse = Off
```

---

## Changes

### v0.9.3

- **Alt+F** real-time file name filter (directories always shown; filter persists on directory change; ESC or Alt+F again to clear)
- **Alt+S** sort mode popup — choose None / Name / Ext / Size / Time / Color on the fly
- **Mouse control**: `--nomouse` / `-M` CLI flag, `LINM_NO_MOUSE` env var, `Mouse = On/Off` in `default.cfg`
- **Smart config merge**: on version upgrade, missing keys are merged from system defaults; existing settings preserved; old files backed up; process auto-restarts via `execvp` so all settings take effect immediately
- **Ctrl+D** reassigned to Quit only (removed from Delete)
- F1 help reorganized into logical groups

### v0.9.2

- Config directory changed from `.linm` to `.config/linm`
- Fixed segfault when `LastPath` is invalid
- Fixed repeated copy prompt on config version mismatch
- CMake options `LINM_CFGPATH` / `LINM_BINPATH` fixed

### v0.9.0

- Build system migrated from automake to CMake

---

## Supported Platforms

- Linux
- Apple macOS
- FreeBSD / OpenBSD
- Solaris / AIX

---

## License

LinM is distributed under the GNU General Public License v3.  
See `LICENSE` for details.

---

## 한국어 설치 가이드 (Korean Installation Guide)

LinM은 Linux용 텍스트 UI 파일 관리자(Mdir 클론)입니다.  
이 섹션은 **한국어 사용자**를 위한 단계별 설치 안내입니다.

### 1단계) 의존성 설치 (관리자 권한 필요)

#### Debian / Ubuntu

```bash
sudo apt-get update
sudo apt-get install -y build-essential cmake git gettext \
    libncursesw5-dev libssl-dev libssh2-1-dev \
    libsource-highlight-dev
```

#### Arch Linux

```bash
sudo pacman -S --needed base-devel cmake git gettext \
    ncurses openssl libssh2 source-highlight
```

#### Fedora

```bash
sudo dnf install -y gcc-c++ cmake git make gettext \
    ncurses-devel openssl-devel libssh2-devel \
    source-highlight-devel
```

#### CentOS / RHEL (EPEL 저장소 활성화 필요)

```bash
sudo yum install -y gcc-c++ cmake3 git make gettext \
    ncurses-devel openssl-devel libssh2-devel \
    source-highlight-devel
```

---

### 2단계) 소스 클론 및 사용자 경로 설치

`sudo` 없이 사용자 홈 경로(`$HOME/.local`)에 설치합니다. 시스템 전역 환경에 영향을 주지 않아 안전합니다.

```bash
git clone https://github.com/la9527/linm.git
cd linm

mkdir -p build
cd build

cmake -DCMAKE_INSTALL_PREFIX=$HOME/.local ..
make
make install
```

설치 후 `$HOME/.local/bin`이 `PATH`에 없다면 `~/.bashrc`에 추가합니다:

```bash
export PATH="$HOME/.local/bin:$PATH"
source ~/.bashrc
```

---

### 3단계) `linm.sh` 경로 보정

종료 후 마지막 디렉토리를 유지하려면 바이너리(`linm`) 대신 래퍼 스크립트(`linm.sh`)로 실행해야 합니다.

1. 스크립트 파일 열기:
   ```bash
   nano $HOME/.local/bin/linm.sh
   ```

2. 실행 라인을 확인합니다. 설치 직후 `@bindir@/linm $@` 또는 `OFF/linm $@` 형태로 되어 있을 수 있습니다. **실제 바이너리 경로**로 바꿉니다:
   ```bash
   /home/<사용자명>/.local/bin/linm $@
   ```
   예) 사용자명이 `zirize`인 경우:
   ```bash
   /home/zirize/.local/bin/linm $@
   ```

---

### 4단계) Bash alias 등록

`~/.bashrc`를 직접 수정하는 대신 `~/.bash_aliases`에 alias를 등록하는 방식을 권장합니다.

1. 파일 열기 (없으면 자동 생성):
   ```bash
   nano ~/.bash_aliases
   ```

2. 다음 한 줄을 추가합니다:
   ```bash
   alias linm='. $HOME/.local/bin/linm.sh'
   ```
   > ⚠️ 앞의 점(`.`)과 공백이 반드시 있어야 합니다. 없으면 종료 후 디렉토리 이동이 현재 셸에 반영되지 않습니다.

3. 적용:
   ```bash
   source ~/.bash_aliases
   source ~/.bashrc
   ```

`~/.bash_aliases`가 자동으로 로드되지 않는다면 `~/.bashrc`에 다음이 있는지 확인하세요:

```bash
if [ -f ~/.bash_aliases ]; then
    . ~/.bash_aliases
fi
```

---

### 5단계) 동작 확인

```bash
linm
```

실행 후 아무 디렉토리로 이동하고 종료했을 때, 터미널 현재 경로가 마지막 작업 위치로 바뀌면 정상 설치된 것입니다.

---

### 버전 업그레이드 시 참고

새 버전을 빌드·설치한 후 처음 실행하면 설정 파일을 자동으로 업그레이드합니다:
- 기존 설정을 `~/.config/linm/back/`에 백업
- 새로운 설정 키만 병합 (기존 커스터마이징 유지)
- 설정 반영을 위해 자동 재시작

별도로 설정을 초기화할 필요가 없습니다.

