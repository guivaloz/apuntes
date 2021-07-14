
# KDE Akonadi

The Akonadi framework is responsible for providing applications with a centralized database to store, index and retrieve the user's personal information. This includes the user's emails, contacts, calendars, events, journals, alarms, notes, etc. In SC 4.4, KAddressBook became the first application to start using the Akonadi framework. In SC 4.7, KMail, KOrganizer, KJots, etc. were updated to use Akonadi as well. In addition, several Plasma widgets also use Akonadi to store and retrieve calendar events, notes, etc.

## Deshabilitar con

Editar

    nano ~/.config/akonadi/akonadiserverrc

Cambiar

    StartServer=false

Parar

    akonadictl status
    akonadictl stop
    akonadictl status

## Akonadi con PostgreSQL

Instalar qt5-qtbase-postgresql

    sudo dnf install qt5-qtbase-postgresql

Crear la base de datos

    createdb -O USUARIO --locale=es_MX.UTF-8 -T template0 akonadi_USUARIO

Verificar

    psql -l

Editar

    nano ~/.config/akonadi/akonadiserverrc

Con este contenido

    [%General]
    Driver=QPSQL

    [QPSQL]
    Host=/run/postgresql
    Name=akonadi_USUARIO
    StartServer=false

Custom port, username and password can be specified with options Port=, User=, Password= in the [QPSQL] section.

Arranque y revise

    akonadictl start
    akonadictl status
