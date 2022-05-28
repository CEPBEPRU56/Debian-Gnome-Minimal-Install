# Debian-Gnome-Minimal-Install-SERBEP_RU56
## Минимальное руководство по установке Debian Gnome

У запуска Linux в качестве настольной операционной системы есть свои преимущества и недостатки. Если вы любите редактирование видео, создание музыки. Может быть, вам лучше работать с macOS? Если вы любите игры, создавайте офисные документы и регистрируйте все нажатия клавиш в Skynet. Может быть, вам лучше работать с Windows?

При установке macOS или Windows. У тебя нет особого выбора. Ты получаешь то, что получаешь. Вы никогда не можете использовать iTunes или Apple Maps. Не имеет значения, они являются частью пакета. Если вы попытаетесь удалить их, ваша система может стать нестабильной.

Именно здесь Linux (и FreeBSD) возвышаются над ведущими настольными операционными системами. Они дают вам выбор. (Иногда этот выбор может быть непростым. Так много дистрибутивов, сред рабочего стола, оконных менеджеров, тем, цветовых схем, параметров конфигурации.) Этот выбор гарантирует многообещающее будущее для настольных компьютеров Linux, потому что всегда будет значительная часть населения, которая хочет компьютер, который они могут программировать. Не устройство, которое пытается их запрограммировать.

Минимальная установка Linux может сократить использование дискового пространства и оперативной памяти, повысить безопасность и конфиденциальность за счет уменьшения поверхности атаки. Повысьте производительность за счет сокращения времени, затрачиваемого на обновление и устранение неполадок. Торо сказал это лучше всех:

> * Наша жизнь разбазаривается по мелочам. Честному человеку вряд ли нужно считать больше, чем на своих десяти пальцах, или, в крайнем случае, он может добавить свои десять пальцев, а остальное сложить в кучу. Простота, простота, простота! Я говорю: пусть ваших дел будет два или три, а не сто или тысяча; вместо миллиона насчитайте полдюжины и ведите свои счета на ногтях большого пальца*.

Стандартный процесс установки Debian для Gnome desktop включает дополнительные пакеты, которые могут быть не нужны или не нужны многим пользователям. Это руководство позволит вам установить минимальный рабочий стол Gnome, добавив дополнительные пакеты по мере необходимости.

## Требования

* Установка debian (аппаратная или виртуальная машина) с соответствующими видеодрайверами.

* привилегии sudo для установки пакетов и запуска дополнительных скриптов.

* Установка `git` для клонирования этого репозитория `sudo pkg install git`

* Установка `bash` для запуска установочного скрипта `sudo pkg install bash'

## ISO для установки Debian

* [debian-11.3.0-amd64-netinst.iso](https://cdimage.debian.org/debian-cd/current/amd64/iso-cd/debian-11.3.0-amd64-netinst.iso)

* [Installing Debian 11.3](https://www.debian.org/releases/bullseye/debian-installer/)

* [Debian “bullseye” Release Information](https://www.debian.org/releases/bullseye/)

## Установка Debian без среды рабочего стола

По мере прохождения процесса установки debian ближе к концу вам будет представлен следующий экран для выбора программного обеспечения:

Снимите флажок **Среда рабочего стола Debian**, чтобы установить минимальную систему debian.

## Обновление источников до тестовых или нестабильных (необязательно)

Обновите источники до `книжный червь.` Текущая ветвь тестирования.

`sudo $EDITOR /etc/apt/sources`:

```bash
deb http://deb.debian.org/debian bookworm main
deb-src http://deb.debian.org/debian bookworm main

deb http://deb.debian.org/debian-security/ bookworm-security main
deb-src http://deb.debian.org/debian-security/ bookworm-security main

deb http://deb.debian.org/debian bookworm-updates main
deb-src http://deb.debian.org/debian bookworm-updates main

# deb http://deb.debian.org/debian bookworm-backports main
# deb-src http://deb.debian.org/debian bookworm-backports main
```

Добавляйте "contrib non-free" после каждой записи "main", если вам нужны специальные драйверы или дополнительная прошивка.

Другим вариантом был бы debian `unstable` (sid). Обновите "источники" следующим образом:

```bash
deb http://deb.debian.org/debian/ unstable main
deb-src http://deb.debian.org/debian/ unstable main
```

Обновите свою систему:

```bash
sudo apt update && apt upgrade
```

Перезагрузитесь, чтобы загрузить обновленное ядро и службы.

## Быстрая установка Минимального Gnome

```bash
# clone the repo
git clone https://github.com/coonrad/Debian-Gnome-Minimal-Install.git
# cd to repo
cd Debian-Gnome-Minimal-Install
# run install script and call the gnome function
./install-debian gnome
reboot
```
Минимальный список пакетов gnome установит gdm (login manager), и после перезагрузки у вас должна быть полностью функциональная минимальная установка gnome.

Простой скрипт примет аргумент командной строки (в данном случае 'gnome') и сопоставит его с функцией для установки выбранных пакетов, связанных с gnome. ((Он также проверяет, что вы используете Debian, прежде чем что-либо делать.) Вы можете добавлять или вычитать пакеты в соответствии с вашими потребностями. (требуется gnome-сессия.)

```bash
#!/usr/bin/env bash

set -e

gnome() {
    sudo apt install -y \
        eog \
        evince \
        gnome-calculator \
        gnome-disk-utility \
        gnome-screenshot \
        gnome-session \
        gnome-shell-extensions \
        gnome-system-monitor \
        gnome-terminal \
        gnome-tweaks \
        nautilus \
        nautilus-wipe \
        network-manager-gnome \
        network-manager-openvpn \
        network-manager-openvpn-gnome \
        wl-clipboard \
        xsel
}

if [[ $(uname) == 'Linux' ]]; then
    if [ "$(/bin/grep ^ID= /etc/os-release)" = "ID=debian" ]; then
        "$@" && echo
    fi
fi
```

Вы также можете добавлять функции. Возможно, у вас есть какие-то базовые пакеты, программы или приложения на python, которые вы хотите установить:

```bash
base() {
    sudo apt install -y \
        open-vm-tools-desktop \
        openssh-server \
        pandoc \
        ripgrep \
        rsync \
        shellcheck \
        shfmt \
        tcpdump \
        tmux \
        tree
}

py() {
    sudo apt install -y \
        python3-autopep8 \
        python3-bs4 \
        python3-pynvim \
        python3-pip
}

apps() {
    sudo apt install -y \
        firefox \
        keepassxc \
        inkscape
}
```

call the new function `./install-debian base`
