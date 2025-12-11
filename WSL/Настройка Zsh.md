# Настройка Zsh

Для начала, установим Zsh (если он уже установлен, например, как в Manjaro, можете пропустить этот пункт):

```bash
sudo apt install zsh
```

Oh-My-Zsh — популярный и активно развивающийся фреймворк Zsh, который позволяет гибко настроить оболочку терминала. Установим его:

```bash
sh -c "$(curl -fsSL https://raw.github.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"
```

Подсветка синтаксиса. Гораздо проще ориентироваться по содержимому терминала, когда разные части команд подсвечены разными цветами. Например, директории будут подчеркиваться, а команды — выделяться цветом, отличным от обычного текста. Установим плагин `zsh-syntax-highlighting`:

```bash
git clone https://github.com/zsh-users/zsh-syntax-highlighting.git $ZSH_CUSTOM/plugins/zsh-syntax-highlighting
```
Чтоб плагин заработал, его надо подключить.

В файле `~/.zshrc` меняем строку с `plugins=`:

```bash
plugins=(git zsh-syntax-highlighting)
```

Если такой строки нет — добавьте её.

Готово! Получаем удобный и функциональный терминал. Теперь сделаем его визуально приятным.

## Настраиваем внешний вид

Устанавливаем тему PowerLevel10K:

```bash
git clone https://github.com/romkatv/powerlevel10k.git $ZSH_CUSTOM/themes/powerlevel10k
```

Скачиваем и добавляем в систему шрифт `JetBrains Mono Nerd` (c иконками):
Выберитеодин из списка (https://github.com/ryanoasis/nerd-fonts/tree/master/patched-fonts/JetBrainsMono), в папке `шрифт/complete` выберите шрифт без «Windows Compatible», с окончанием «Mono».

Подключаем шрифт и тему.

Редактируем `~/.zshrc`.

Если в файле эти строки уже есть — замените их.

```text
ZSH_THEME="powerlevel10k/powerlevel10k"

POWERLEVEL9K_MODE="nerdfont-complete"
```

Запускаем конфигурацию темы: `p10k configure` (открыть новое окно терминала).
Настройте тему, выбирая варианты отображения, которые вам больше нравятся.
