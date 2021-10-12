# Fedora 35 instalar

## Use root

Cambie a root

    sudo su -

## Revision

Mostrar los grupos instalados con Spin KDE

    dnf grouplist

Muestra estos

    Grupos de entorno instalados:
        Espacios de trabajo KDE Plasma
    Grupos instalados:
        LibreOffice

## Instalar

Instalar lenguaje español para LibreOffice

    dnf install libreoffice-help-es libreoffice-langpack-es

Instalar soporte para contenedores

    dnf groupinstall 'Container Management' --with-optional

Instalar Libvirt

    dnf groupinstall 'Virtualization' --with-optional

Instalar Kate

    dnf install kate
    dnf install falkon

Instalar Google Chrome

    dnf install fedora-workstation-repositories
    dnf repolist --all
    dnf config-manager --set-enabled google-chrome
    dnf --refresh update
    dnf install google-chrome-stable

Instalar tipografías

    dnf install google-roboto-condensed-fonts google-roboto-fonts google-roboto-mono-fonts
    dnf install google-droid-fonts-all
    dnf install mozilla-fira-mono-fonts mozilla-fira-sans-fonts
    dnf install terminus-fonts

Instalar VSCode

    rpm --import https://packages.microsoft.com/keys/microsoft.asc
    sh -c 'echo -e "[code]\nname=Visual Studio Code\nbaseurl=https://packages.microsoft.com/yumrepos/vscode\nenabled=1\ngpgcheck=1\ngpgkey=https://packages.microsoft.com/keys/microsoft.asc" > /etc/yum.repos.d/vscode.repo'
    dnf --refresh update
    dnf install code

Instalar Sublime Text

    rpm -v --import https://download.sublimetext.com/sublimehq-rpm-pub.gpg
    dnf config-manager --add-repo https://download.sublimetext.com/rpm/stable/x86_64/sublime-text.repo
    dnf --refresh update
    dnf install sublime-text

Instalar Calibre

    dnf install calibre

Instalar GIMP

    dnf install gimp

Instalar Inkscape

    dnf install inkscape

Instalar soporte de impresoras HP

    dnf install hplip

Instalar aplicaciones de terminal

    dnf install figlet pwgen youtube-dl hwinfo htop system-storage-manager

Instalar RClone

    dnf install rclone

## Eliminar KDE PIM

    dnf remove kdepim-runtime kdepim-addons
    dnf remove akregator
    dnf autoremove \*akonadi\*

## RMP Fusion

Ejecutar

    dnf install https://mirrors.rpmfusion.org/free/fedora/rpmfusion-free-release-$(rpm -E %fedora).noarch.rpm https://mirrors.rpmfusion.org/nonfree/fedora/rpmfusion-nonfree-release-$(rpm -E %fedora).noarch.rpm

Actualizar

    dnf --refresh update
    dnf groupupdate core
    dnf groupupdate multimedia --setop="install_weak_deps=False" --exclude=PackageKit-gstreamer-plugin
    dnf groupupdate sound-and-video

Instalar aplicaciones multimedia

    dnf install audacious audacious-plugins-freeworld audacious-plugins-freeworld-aac audacious-plugins-freeworld-ffaudio audacious-plugins-freeworld-mms
    dnf install mplayer mencoder
    dnf install ffmpeg ffmpegthumbs
    dnf install moc

Instalar KDEnlive

    dnf install kdenlive frei0r-plugins

## Desarrollo

Instalar todas las herramientas para Python

    dnf groupinstall 'Python Classroom'

Instalar PostgreSQL

    dnf install postgresql postgresql-server

Instalar MariaDB

    dnf install mariadb qt5-qtbase-mysql

Instalar Redis y wkhtmltopdf para crear PDF con Python

    dnf install redis
    dnf install wkhtmltopdf

Instalar seahorse para administrar las contrasenas de Gnome

    dnf install seahorse

## Limpieza

Ejecutar

    dnf autoremove
