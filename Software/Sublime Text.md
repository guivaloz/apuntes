
# Sublime Text Configuración

## Instalar

Instale la llave GPG

    sudo rpm -v --import https://download.sublimetext.com/sublimehq-rpm-pub.gpg

Agregue el repositorio estable

    sudo dnf config-manager --add-repo https://download.sublimetext.com/rpm/stable/x86_64/sublime-text.repo

Actualice e instale

    sudo dnf update
    sudo dnf install sublime-text

## Configurar

Instalar Package Control con

    CTRL-SHIFT-P > Install Package Control

Instalar tema Afterglow con

    CTRL-SHIFT-P > Package Control: Install Package > Theme Afterglow

Agregue en settings

    "tabs_small": true

Lea

    https://github.com/YabataDesign/afterglow-theme

Mejore la configuración agregando estas líneas

    "ensure_newline_at_eof_on_save": false,
    "font_face": "Hack",
    "font_size": 13,
    "translate_tabs_to_spaces": true,
    "trim_trailing_white_space_on_save": false,
    "line_padding_bottom": 1
