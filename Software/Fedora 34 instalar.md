# Fedora 34 Instalar

Arrancar con Fedora Server Netinstall porque Wayland no es compatible con la tarjeta de video NVIDIA

Al elegir qué paquetes instalar se eligen

    Espacios de trabajo KDE Plasma
    Herramientas de Administración
    Administración de contenedores
    LibreOffice
    Python Classroom
    KDE

Instalar GIMP

    sudo dnf install gimp

Instalar Libreoffice

    sudo dnf groupinstall LibreOffice
    sudo dnf install libreoffice-help-es libreoffice-langpack-es

Instalar LibVirt

    sudo dnf group install 'Virtualización' --with-optional

Instalar Google Chrome

    sudo dnf install fedora-workstation-repositories
    sudo dnf update
    sudo dnf repolist --all
    sudo dnf config-manager --set-enabled google-chrome
    sudo dnf --refresh update
    sudo dnf install google-chrome-stable
    sudo dnf install google-roboto-condensed-fonts google-roboto-fonts google-roboto-mono-fonts
    sudo dnf install google-droid-fonts-all
    sudo dnf install mozilla-fira-mono-fonts mozilla-fira-sans-fonts

Instalar VSCode

    sudo rpm --import https://packages.microsoft.com/keys/microsoft.asc
    sudo sh -c 'echo -e "[code]\nname=Visual Studio Code\nbaseurl=https://packages.microsoft.com/yumrepos/vscode\nenabled=1\ngpgcheck=1\ngpgkey=https://packages.microsoft.com/keys/microsoft.asc" > /etc/yum.repos.d/vscode.repo'
    sudo dnf check-update
    sudo dnf install code

Instalar Sublime Text

    sudo rpm -v --import https://download.sublimetext.com/sublimehq-rpm-pub.gpg
    sudo dnf config-manager --add-repo https://download.sublimetext.com/rpm/stable/x86_64/sublime-text.repo
    sudo dnf update
    sudo dnf install sublime-text

Instalar utilerías de KDE

    sudo dnf install kate
    sudo dnf install ark

Instalar RClone

    sudo dnf install rclone

Instalar NextCloud client

    sudo dnf install nextcloud-client

Instalar aplicaciones multimedia

    sudo dnf install mplayer mencoder
    sudo dnf install ffmpeg ffmpegthumbs
    sudo dnf install moc

Instalar aplicaciones de terminal

    sudo dnf install figlet pwgen youtube-dl hwinfo htop system-storage-manager

Instalar Calibre

    sudo dnf install calibre

Instalar KDEnlive

    sudo dnf install kdenlive frei0r-plugins

Instalar Google Chrome

    sudo dnf --refresh update
    sudo dnf install fedora-workstation-repositories
    sudo dnf update
    sudo dnf repolist --all
    sudo dnf config-manager --set-enabled google-chrome
    sudo dnf update
    sudo dnf install google-chrome-stable
    sudo dnf install google-roboto-condensed-fonts google-roboto-fonts google-roboto-mono-fonts
    sudo dnf install google-droid-fonts-all
    sudo dnf install mozilla-fira-mono-fonts mozilla-fira-sans-fonts

Instalar soporte de impresoras HP

    sudo dnf install hplip
