# Kаждый проект в WSL свой отдельный Git-аккаунт

- Проект A → GitHub/ GitLab Account #1
- Проект B → GitHub/ GitLab Account #2

И чтобы это **автоматически** работало при коммите/push (разные email, юзернейм, SSH-ключи).

Ниже даю **чёткую и рабочую схему**: 3 отдельных SSH-ключа + локальные git-конфиги + SSH-конфиг, который выбирает ключ по папке.

---

## 🟦 ИТОГОВЫЙ ПОДХОД

**Каждый проект имеет:**

- свой SSH-ключ
- свой локальный `.git/config` с email/username
- SSH сам выбирает ключ в зависимости от пути

После настройки ты сможешь работать:

```txt
~/projects/project-node16   → account_1
~/projects/project-node22   → account_2
```

И git будет сам использовать разные аккаунты.

---

## 🟩 Шаг 1 — создать SSH-ключи (в WSL)

Делаем по одному на каждый проект:

### Проект A (node16 → account1)

```bash
ssh-keygen -t ed25519 -f ~/.ssh/id_account1 -C "email-account1@example.com"
```

### Проект B (node22 → account2)

```bash
ssh-keygen -t ed25519 -f ~/.ssh/id_account2 -C "email-account2@example.com"
```

Публичный ключи (нужно добавить в GitHub/GitLab):

```bash
~/.ssh/id_account1.pub
~/.ssh/id_account2.pub
```

---

## 🟩 Шаг 2 — настроить SSH config на выбор ключа по папке

Открыть и создать файл:

```bash
nano ~/.ssh/config
```

Вставить:

```text
# === Project Node16: account1 ===
Host account1.github.com
    HostName github.com
    User git
    IdentityFile ~/.ssh/id_account1
    IdentitiesOnly yes

# === Project Node22: account2 ===
Host account2.github.com
    HostName github.com
    User git
    IdentityFile ~/.ssh/id_account2
    IdentitiesOnly yes

# === Project prj3: account3 ===
# ...
```

Права на файл:

```bash
chmod 600 ~/.ssh/config
```

---

## 🟩 Шаг 3 — настроить Git remote в каждом проекте

Теперь **для каждого проекта свой Host**, не `github.com`, а `project-node16`, `project-node22`, `project-python`.

## Проект A — node16 (account1)

Перейти в проект:

```bash
cd ~/projects/project-node16
```

Настроить локальный git-пользователь:

```bash
git init
git config user.name "Account1 Name"
git config user.email "email-account1@example.com"
```

И изменить remote:

```bash
git remote add origin git@project-node16:USERNAME/REPO.git
```

---

## Проект B — node22 (account2)

```bash
cd ~/projects/project-node22

git init
git config user.name "Account2 Name"
git config user.email "email-account2@example.com"

git remote add origin git@project-node22:USERNAME/REPO.git
```

### Или глобально использовать `includeIf` для автоматического применения на основе пути:

В ~/.gitconfig:

```text
[user]
    name = Default Name
    email = default@example.com

[includeIf "gitdir:~/projects/project-a/"]
    path = ~/.gitconfig-personal

[includeIf "gitdir:~/projects/project-b/"]
    path = ~/.gitconfig-work
```

Создай файлы `~/.gitconfig-personal` и `~/.gitconfig-work` с нужными `user.name`/`user.email`.

---

## 🟩 Шаг 4 — проверить работу

Например для node16:

```bash
cd ~/projects/project-node16
ssh -T git@project-node16
```

Ты должен увидеть:

```bash
Hi USERNAME! You've successfully authenticated...
```

Для node22:

```bash
cd ~/projects/project-node22
ssh -T git@project-node22
```

Если всё правильно → push работает автоматически для каждого аккаунта.

---

## 🟦 Уровень PRO — автоматизация через `.envrc`

Если ты подключил `direnv`, можно сделать авто-переключение GIT author/email:

В `.envrc` проекта, например `~/projects/project-node22/.envrc`:

```bash
export GIT_AUTHOR_NAME="Account2 Name"
export GIT_AUTHOR_EMAIL="email-account2@example.com"
export GIT_COMMITTER_NAME="Account2 Name"
export GIT_COMMITTER_EMAIL="email-account2@example.com"
```

И выполнить:

```bash
direnv allow
```

Но это **не обязательно**, локальный `.git/config` уже решает вопрос.

---
