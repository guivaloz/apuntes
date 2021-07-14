
# Google Cloud App Engine

Describir la app por defecto

    gcloud app describe

Abrir una nueva pestaña en el navegador de internet con la app por defecto

    gcloud app browse

Abrir una versión en particular

    gcloud app browse --version=$VERSION

Listar los servicios

    gcloud app services list

Listar las versiones de las apps

    gcloud app versions list

BORRAR una versión de la app

    gcloud app versions delete $VERSION

## Subir

Con deploy

    gcloud app deploy

## Depurar

Con debug

    gcloud app logs tail -s SERVICE
