# Предпосылки (что уже нужно)

Windows 11 с включённой виртуализацией (VT-x/AMD-V).

Учетная запись с правами администратора (для установки и включения служб).

Рекомендуется 16+ GB RAM, ~50+ GB свободного места.

# Установить WSL2 и базовую Linux-дистрибуцию

Открой PowerShell от имени Админа и выполни:

```PowerShell
wsl --install
```

(это установит WSL2 + Ubuntu по умолчанию).
Если уже установлен — обнови ядро:

```PowerShell
wsl --update
wsl --set-default-version 2

wsl --install -d Ubuntu
wsl -l -v
wsl --set-default Ubuntu
```

Запусти Ubuntu (в меню “Ubuntu”) и создай пользователя.

# Настройка терминала

Если не установлен установить Windows Terminal. (Можно из Microsoft Store).
Открыть. Settings -> Startup -> Default profile = Ubuntu

# Обновление ubuntu

Открыть Ubuntu терминал:

```bash
sudo apt update && sudo apt upgrade -y
```

# Установка необходимых инструментов

```bash
sudo apt install -y \
    git gpg zip unzip \
    curl wget gcc bsdmainutils htop fzf bat ripgrep build-essential

sudo ln -s $(which batcat) /usr/local/bin/bat
```

# Настройка Git

```bash
git config --global alias.st status
git config --global user.name "..."
git config --global user.email "..."
git config --global init.defaultBranch main
# чтобы кириллические имена файлов нормально выводились
git config --global core.quotepath false
git config --global core.autocrlf input
git config --global core.safecrlf warn
```

# Усановить Nerd Font

Открыть `https://nerdfonts.com/font-downloads`, скачать _Hack Nerd Font_. Распокавать архив -> выделить все файлы кроме `Licence` и `Readme` -> ПКМ -> Установить

# Install zsh and oh-my-zsh

```bash
sudo apt install -y zsh

# https://ohmyz.sh/#install
sh -c "$(curl -fsSL https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"
```

# Установить Docker Desktop и включить интеграцию с WSL

Скачай и установи Docker Desktop (следуя инструкциям на экране).

В Docker Desktop → Settings → General: включи Use the WSL 2 based engine.

В Resources → WSL Integration: включи интеграцию для твоей Ubuntu-дистрибуции.

После этого команды docker будут работать внутри WSL и с VS Code Dev Containers.

# Установить VS Code и расширения

Установи VS Code на Windows и в Marketplace установи:

Remote - WSL

Dev Containers

Docker

GitLens (опционально)

GitHub Pull Requests and Issues / GitHub Repositories (опционально)

Также открой VS Code внутри WSL для лучшей интеграции: в PowerShell code . в папке WSL (если установлен code CLI).

# Установка nvm и Node 16/22 (для проектов A и B)

```bash
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.40.3/install.sh | bash

# => If you wish to uninstall them at a later point (or re-install them under your
# => `nvm` node installs), you can remove them from the system Node as follows:
#
#     $ nvm use system
#     $ npm uninstall -g a_module
#
# => Close and reopen your terminal to start using nvm or run the following to use it now:
# export NVM_DIR="$HOME/.nvm"
# [ -s "$NVM_DIR/nvm.sh" ] && \. "$NVM_DIR/nvm.sh"
# [ -s "$NVM_DIR/bash_completion" ] && \. "$NVM_DIR/bash_completion

# Установим версии
nvm install 24
nvm install 16

# (опционально) установить default
nvm alias default 16

# Проверка:
nvm ls
node -v    # если активирована версия по-умолчанию
npm -v

sudo chmod u+s /bin/ping

npm i -g npm
npm install -g typescript
```

# Установка Python 3.13 из исходников

Проверка если уже установлен

```bash
python3 --version
```

If you need a specific version of Python or want to customize the installation, you can build it from source. This method gives you full control over the Python installation and allows customization during the build process:

Step 1: Install Required Development Packages
Before building Python, ensure you have the necessary development tools and libraries installed.

```bash
sudo apt install build-essential libssl-dev libbz2-dev libreadline-dev libsqlite3-dev wget curl llvm libffi-dev zlib1g-dev
```

Step 2: Download the Latest Version of Python

Visit the official Python website (https://www.python.org/downloads/source/) and download the latest source code tarball. Alternatively, use wget to download it directly:

```bash
wget https://www.python.org/ftp/python/X.Y.Z/Python-X.Y.Z.tgz 
```

Replace <version> with the Python version you want (e.g., 3.13.1). This downloads the source code as a .tgz archive.

Step 3: Extract Tarball and Navigate to the Source Directory
After downloading, extract the tarball:

```bash
tar -xvf Python-<version>.tgz
```

Step 4: Configure the Build Environment
Navigate to the extracted directory and configure the build environment:

```bash
cd Python-X.Y.Z
./configure --enable-optimizations 
```

The --enable-optimizations flag can improve performance.
This unpacks the source code into a directory for building and installation.

Step 5: Compile and Install
Compile the source code and install it:

This uses all available cores for faster compilation:

```bash
make -j $(nproc) 
```

Use altinstall to prevent overwriting any existing python binary:

```bash
sudo make altinstall
```

Step 5: Verify the Installation
After installation, verify that Python is installed correctly by checking its version:

```bash
python3 --version
```

Replace 3.10 with the installed version to confirm it’s correctly installed.

ПРИМЕР:

```bash
# Build Python from sources
sudo apt install -y build-essential libssl-dev libbz2-dev libreadline-dev libsqlite3-dev wget curl llvm libffi-dev zlib1g-dev

wget https://www.python.org/ftp/python/3.13.1/Python-3.13.1.tgz

tar -xzvf Python-3.13.1.tgz

cd Python-3.13.1

./configure --enable-optimizations --prefix=$HOME/.python3.13

make -j 8

sudo make altinstall

python3 --version
```

! Если python был установлен другой версии, то проверить работоспособность только что установленной:

```bash
~/.python3.13/bin/python3.13
# Python 3.13.1 (main, Dec  9 2025, 17:47:40) [GCC 13.3.0] on linux
# Type "help", "copyright", "credits" or "license" for more information.
# >>>

# Установка pip
~/.python3.13/bin/python3.13 -m pip install -U pip

# Делаем дефолтным
cd ~
echo "export PATH=\$PATH:\$HOME/.python3.13/bin" >> ~/.zshrc
```