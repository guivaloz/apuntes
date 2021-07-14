
# Google Cloud SDK

Descargar en

    https://cloud.google.com/sdk/docs/install

Mover el comprimido a 

    cd ~/Descargas
    mv google-cloud-sdk-339.0.0-linux-x86_64.tar.gz ~/Descargas/Software/

Descomprimir con

    tar xf google-cloud-sdk-339.0.0-linux-x86_64.tar.gz

Instalar

    cd ~/Descargas/Software
    ./google-cloud-sdk/install.sh

Corto las líneas de que agrega el instalador en 

    nano -w ~/.bashrc

Y las agrego a mi propio bash modular en 71-gcloud.sh

    # The next line updates PATH for the Google Cloud SDK.
    if [ -f '/home/guivaloz/Descargas/Software/google-cloud-sdk/path.bash.inc' ]; then . '/home/guivaloz/Descargas/Software/google-cloud-sdk/path.bash.inc'; fi

    # The next line enables shell command completion for gcloud.
    if [ -f '/home/guivaloz/Descargas/Software/google-cloud-sdk/completion.bash.inc' ]; then . '/home/guivaloz/Descargas/Software/google-cloud-sdk/completion.bash.inc'; fi

    export GOOGLE_APPLICATION_CREDENTIALS="/home/guivaloz/.gcp-api-keys/guivaloz-en-minos.json"

    alias proxy-sql-ceres="cloud_sql_proxy -instances=pjecz-268521:us-central1:pjecz-ceres=tcp:3306"

Abrir una nueva terminar y probar

    gcloud components update

Instalar el SQL proxy

    gcloud components install cloud_sql_proxy
