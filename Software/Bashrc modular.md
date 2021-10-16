# .bashrc modular en Fedora

El .bashrc que tiene Fedora termina con un cargador de modulos

    # User specific aliases and functions
    if [ -d ~/.bashrc.d ]; then
        for rc in ~/.bashrc.d/*; do
            if [ -f "$rc" ]; then
                . "$rc"
            fi
        done
    fi

    unset rc

Para usarlo vamos a crear el directorio

    mkdir ~/.bashrc.d
    cd ~/.bashrc.d

Escriba 01-welcome.sh

    # Letras grandes (requiere figlet)
    #figlet M I N O S

    # Logotipo de la distribucion e informacion (requiere screenfetch)
    screenfetch

Escriba 02-prompt.sh

    # User specific colors
    blue="\[\033[34m\]"
    cyan="\[\033[36m\]"
    green="\[\033[32m\]"
    purple="\[\033[35m\]"
    red="\[\033[31m\]"
    white="\[\033[37m\]"
    yellow="\[\033[33m\]"
    reset="\[\033[00m\]"

    # My prompt is username@host in directory
    PS1="${green}\u${white}@${yellow}\h "
    PS1+="${white}in ${blue}\W "

    # Git branch at prompt
    # https://www.shellhacks.com/show-git-branch-terminal-command-prompt/
    git_branch() {
        git branch 2> /dev/null | sed -e '/^[^*]/d' -e 's/* \(.*\)/(\1) /'
    }
    PS1+="${purple}\$(git_branch)"

    # Dolar sign to finish
    PS1+="${white}\$ "
    PS1+="${reset}"

Escriba 91-utilities.sh

    alias t2="tree -d -L 2"
    alias t3="tree -d -L 3"
    alias t4="tree -d -L 4"

    alias unzipall="for i in *.zip; do unzip $i -d ${i%.*}; rm $i; done"
