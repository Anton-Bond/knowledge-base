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
git config --global core.quotepath
git config --global core.autocrlf input
git config --global core.safecrlf warn
```

# Усановить Nerd Font

Открыть `https://nerdfonts.com/font-downloads`, скачать *Hack Nerd Font*. Распокавать архив -> выделить все файлы кроме `Licence` и `Readme` -> ПКМ -> Установить

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

# Подключим nvm в текущую сессию (выполняется также автоматически при новом shell)
export NVM_DIR="$HOME/.nvm"
[ -s "$NVM_DIR/nvm.sh" ] && \. "$NVM_DIR/nvm.sh"

# Установим версии
nvm install 24
nvm install 16

# (опционально) установить default
nvm alias default 16

# Проверка:
nvm ls
node -v    # если активирована версия по-умолчанию
```
