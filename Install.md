# Установка системы

## Базовая настройка

Включаем HiDPI для TTY:

```sh
sudo dnf install terminus-fonts-console
```

И записываем в `/etc/vconsole.conf`:

```
KEYMAP="us"
FONT="ter-v32n"
```

```sh
sudo systemctl start systemd-vconsole-setup.service
```

Выключаем сканирование ФС:

```sh
dconf write /org/freedesktop/tracker/miner/files/crawling-interval -2
```

Выключаем засыпание компьютера при закрытии крышки:

1. `sudo vi /etc/systemd/logind.conf`
2. Ставим `HandleLidSwitch=lock`

Перезагружаемся.


## Текстовые редакторы

Устанавливаем GNOME Builder и nano:

```sh
sudo dnf install nano gnome-builder wmctrl
su -c 'echo "export EDITOR=nano" >> /etc/profile'
```

Установить Атом:

```sh
sudo flatpak remote-add --if-not-exists flathub https://flathub.org/repo/flathub.flatpakrepo
flatpak install flathub io.atom.Atom
```

Устанавливаем темы и плагины из [`Atom.md`](./Atom.md).


## Личные файлы

[dotfiles](https://github.com/nook-ru/dotfiles) && fresh

## Терминал

Устанавливаем zsh:

```sh
sudo dnf install zsh
chsh -s /bin/zsh
rm ~/.bash_history ~/.bash_logout
```

Устанавливаем Antigen:

```sh
curl -L git.io/antigen > ~/.antigen.zsh
source ~/.antigen.zsh
```

Создаём `/root/.zshrc`:

```
if [ -f /home/ai/.antigen.zsh ]; then
  ANTIGEN_MUTEX=false
  source /home/ai/.antigen.zsh
  antigen bundle yarn
  antigen bundle zsh-users/zsh-syntax-highlighting
  antigen bundle zsh-users/zsh-history-substring-search
  antigen theme denysdovhan/spaceship-prompt
  antigen apply
fi

SPACESHIP_PROMPT_ORDER=(time user dir host git exit_code line_sep char)
```

### fzf

Устанавливаем [fzf](https://github.com/junegunn/fzf)
Устанавливаем [ripgrep](https://github.com/BurntSushi/ripgrep#installation)

```
$ sudo dnf install fzf
```

```
sudo apt-get install fzf
```

### [HTTPie](https://httpie.io/cli)

```
sudo apt-get install httpie
```
### [Bat](https://github.com/sharkdp/bat)

```
sudo apt-get install bat
```

### [ncdu](sudo apt install ncdu)

```
sudo apt-get install ncdu
```

### [fd](https://github.com/sharkdp/fd#on-ubuntu)

```
sudo apt install fd-find
```

### [exa](https://the.exa.website/docs)

Ubuntu 20.10+:

```
sudo apt install exa
```

Ubuntu 20.04:

```
sudo add-apt-repository ppa:spvkgn/exa
sudo apt-get update
sudo apt-get install exa
```

### [duf](https://github.com/muesli/duf)

```
sudo apt install duf
```

### [tldr](https://tldr.sh/)

```
npm install -g tldr
```

### Цветовая схема

1. Monokai Dark из https://github.com/Mayccoll/Gogh
2. Цветовая схема MC: `modarin256-thin`
   `~/.config/mc/ini`:
   ```
   [Midnight-Commander]
   skin=modarin256-thin
   ```

Перезагружаемся.


## Настройка GNOME (outdated)

Ставим `seahorse` и выключаем пароль со связки ключей.

Открываем Настройки:

- **Поиск:** оставляем только «Калькулятор».
- **Фон:** ставим треугольники на экран блокировки.
- **Сетевые учётные записи:** подключить Google.
- **Энергопитание:** выключаем «Уменьшать яркость при простое»,
  ставим «Выключение экрана» в «Никогда».
- **Устройства → Дисплей:** включаем ночную подсветку с 23:00 до 06:00.
- **Устройства → Мышь и сенсорная панель:** скорость мыши на максимум,
  скорость панели поднимаем, включаем «Нажатие касанием».
- **Подробности → Пользователи:** ставим аватарку из этой папки
  и автоматический вход.

Выставляем настройки клавиатуры:

```sh
dconf write /org/gnome/desktop/input-sources/xkb-options "['grp:alt_shift_toggle', 'lv3:ralt_switch', 'misc:typo', 'nbsp:level3', 'caps:none', 'grp_led:scroll']"
```

В Терминале:

- Параметры → Общие: выключить «Показывать панель меню …».
- Параметры профиля → Общие: выключить «Подавать гудок».

В Nautilus:

- Параметры → Вид: включить «Помещать папки перед файлами».
- Параметры → Поведение: включить «Открыть объекты одним щелчком».

Ставим расширения из [`GNOME.md`](./GNOME.md).

Добавляем Сан-Франциско, Москву, Пекин и Владивосток в Часы.

Установить шрифт Fira Mono и Fire Code:

```sh
sudo dnf install mozilla-fira-mono-fonts
sudo dnf copr enable evana/fira-code-fonts
sudo dnf install fira-code-fonts
```

Устанавливаем шрифты Ubuntu [^ubuntu-fonts]:

```shell=
sudo dnf copr enable chenxiaolong/ubuntu-fonts
sudo dnf install ubuntu-family-fonts fontconfig-ubuntu ubuntu-font-gsettings
```

[^ubuntu-fonts]: [chenxiaolong/ubuntu-fonts](https://copr.fedorainfracloud.org/coprs/chenxiaolong/ubuntu-fonts/)

Установить GNOME Tweek Tool:

```sh
sudo dnf install gnome-tweak-tool
```

И выставить в нём настройки:

- **Основное:** включить «Сверхусиление».
- **Верхняя панель:** включить «заряд в процентах», «дату» и «секунды».
- **Внешний вид:** обои берём из этой папки.
- **Заголовки окон:** двойное нажатие — «toggle maximize vertical»,
  средней — «toggle maximize».
- **Клавиатура и мышь:** выключить «Вставка при нажатии средней кнопки мышки»
  и ставим «Adaptive» в профиле ускорения.
- **Окна:** выключаем «Активные края».
- **Расширения:**
  - **Emoji selector:** выключаем «Always show the icon».
  - **Gsconnect:** подключаем телефон.
  - **Icon Hider:** убираем `appMenu` и `keyboard`. Скрываю её иконку.
  - **Screenshot Tool:** убираем «Показывать иконку». Ставим «Автоматически
    сохранять скриншот» в «Загрузки» с именем `{Y}{m}{d}{H}{M}{S}`. Включаем
    Imgur с автоматическим открытием ссылки.
- **Шрифты:** моноширный в «Fira Code».

Удаляем папки иконок:

```sh
gsettings set org.gnome.desktop.app-folders folder-children "['']"
```

Оставить в доке по-умолчанию только Фаерфокс, Наутилус и Терминал.


## Папки (outdated)

Создаём шаблон пустого файла:

```sh
mkdir ~/.local/share/desktop
mkdir ~/.local/share/templates
touch ~/.local/share/templates/Пустой\ файл
```

Исправляем папки по-умолчанию `~/.config/user-dirs.dirs`:

```sh
XDG_DESKTOP_DIR="$HOME/.local/share/desktop"
XDG_DOWNLOAD_DIR="$HOME/Загрузки"
XDG_TEMPLATES_DIR="$HOME/.local/share/templates"
XDG_PUBLICSHARE_DIR="$HOME/"
XDG_DOCUMENTS_DIR="$HOME/"
XDG_MUSIC_DIR="$HOME/"
XDG_PICTURES_DIR="$HOME/"
XDG_VIDEOS_DIR="$HOME/Видео"
```

Чистим закладки:

```sh
echo "" > ~/.config/gtk-3.0/bookmarks
```

Удаляем лишние папки:

```sh
rm -R ~/Документы ~/Изображения ~/Музыка ~/Общедоступные ~/Шаблоны ~/Рабочий\ стол
```


## Остальное ПО (outdated)

Устанавливаем кодеки:

```sh
sudo dnf install amrnb amrwb faac faad2 flac gstreamer1-libav gstreamer1-plugins-bad-freeworld gstreamer-ffmpeg gstreamer-plugins-bad-nonfree gstreamer-plugins-espeak gstreamer-plugins-fc gstreamer-plugins-ugly gstreamer-rtsp lame libdca libmad libmatroska x264 x265 xvidcore gstreamer1-plugins-bad-free gstreamer1-plugins-base gstreamer1-plugins-good gstreamer-plugins-bad gstreamer-plugins-bad-free gstreamer-plugins-base gstreamer-plugins-good gstreamer1-plugins-ugly-free
```

Устанавливаем программы:

```sh
sudo dnf install man-pages-ru mpv unrar p7zip p7zip-plugins
```

Устанавливаем шрифты от Microsoft:

```sh
sudo dnf install https://downloads.sourceforge.net/project/mscorefonts2/rpms/msttcore-fonts-installer-2.6-1.noarch.rpm
```

Ставим Хром через Приложения.

Ставим VPN:

1. Скачиваем [файлы настроек VPN](https://www.expressvpn.com/ru/setup#manual)
2. Добавляем их под именами `Германия` и `Гонконг`.


Устанавливаем GIMP и Transmission:

```sh
flatpak install flathub com.transmissionbt.Transmission
flatpak install flathub org.gimp.GIMP
```


## Разработка

Устанавливаем пакеты:

```sh
sudo dnf install git tig ripgrep golang redis postgresql postgresql-server postgresql-contrib
```

Запускаем PostgreSQL:

```sh
sudo postgresql-setup initdb
sudo systemctl enable redis
sudo systemctl enable postgresql
sudo systemctl start postgresql
sudo su postgres -c 'createuser -s ai'
```

В `/var/lib/pgsql/data/pg_hba.conf` меняем строчку `ident` на `trust`.

Устанавливаем `node` и `yarn`:

```sh
sudo dnf install https://rpm.nodesource.com/pub_12.x/fc/29/x86_64/nodesource-release-fc29-1.noarch.rpm
sudo dnf install nodejs
sudo wget https://dl.yarnpkg.com/rpm/yarn.repo -O /etc/yum.repos.d/yarn.repo
sudo dnf install yarn
```

Устанавливаем Ruby:

```sh
sudo dnf install https://copr-be.cloud.fedoraproject.org/results/nwallace/ruby-tools/fedora-26-x86_64/00140262-chruby/chruby-0.3.9-1.noarch.rpm
sudo dnf install gcc automake gdbm-devel libffi-devel libyaml-devel openssl-devel ncurses-devel readline-devel zlib-devel gcc-c++ libxml2 libxml2-devel libxslt libxslt-devel postgresql-devel sqlite-devel
~/Dev/environment/bin/build-ruby 2.6.3
source /usr/share/chruby/chruby.sh
chruby 2.6.3
gem install bundler --version "<2.0.0"
```

Устанавливаем [Helm](https://github.com/helm/helm/releases).

Устанавливаем Kubernetes:

```sh
sudo cat <<EOF > /etc/yum.repos.d/kubernetes.repo
[kubernetes]
name=Kubernetes
baseurl=https://packages.cloud.google.com/yum/repos/kubernetes-el7-x86_64
enabled=1
gpgcheck=1
repo_gpgcheck=1
gpgkey=https://packages.cloud.google.com/yum/doc/yum-key.gpg https://packages.cloud.google.com/yum/doc/rpm-package-key.gpg
EOF
sudo dnf install kubectl
```

Устанавливаем Docker:

```sh
sudo dnf config-manager --add-repo https://download.docker.com/linux/fedora/docker-ce.repo
sudo dnf install docker-ce
sudo groupadd docker
sudo usermod -aG docker $USER
sudo systemctl enable docker
sudo systemctl start docker
sudo curl -L "https://github.com/docker/compose/releases/download/1.23.1/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
sudo chmod +x /usr/local/bin/docker-compose
```

Устанавливаем Keybase:

```sh
sudo dnf install https://prerelease.keybase.io/keybase_amd64.rpm
run_keybase
```

Удаляем Keybase из автозагрузки.
