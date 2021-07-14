
# MariaDB

## Instalar

En Fedora

    sudo dnf install mariadb mariadb-server
    sudo systemctl status mariadb
    sudo systemctl start mariadb
    sudo mysql_secure_installation

Defina la contraseña root

Responda

    Switch to unix_socket authentication [Y/n] n
    Change the root password? [Y/n] n
    Remove anonymous users? [Y/n] Y
    Disallow root login remotely? [Y/n] Y
    Remove test database and access to it? [Y/n] Y
    Reload privilege tables now? [Y/n] Y

Abra el puerto en el muro de fuego

    sudo firewall-cmd --list-services
    sudo firewall-cmd --add-service=mysql
    sudo firewall-cmd --list-services

Haga el cambio en el muro de fuego permanente

    sudo firewall-cmd --runtime-to-permanent

## Comando mysql

Ingresar como root

    mysql -u root -p

Ingresar a un servidor con una dirección IP (127.0.0.1) y a una base de datos dada

    mysql --host=127.0.0.1 --user=adminuser --password database

## Crear una nueva base de datos y un usuario administrador

    CREATE DATABASE database CHARACTER SET = 'utf8';
    CREATE USER adminuser@'%' IDENTIFIED BY 'password';
    GRANT ALL PRIVILEGES ON database.* TO adminuser@'%';
    FLUSH PRIVILEGES;
    EXIT;

## Usarios y privilegios

Para crear un usuario que solo trabaje local

    CREATE USER user1@localhost IDENTIFIED BY 'password1';

Ver los usuarios

    SELECT user FROM mysql.user;

Para otorgar TODOS los privilegios en UNA base de datos de modo local

    GRANT ALL PRIVILEGES ON database.* TO user1@'localhost';
    FLUSH PRIVILEGES;

Para otorgar privilegios de consulta en una base de datos en el mismo equipo

    GRANT SELECT ON database.* TO user1@'localhost';
    FLUSH PRIVILEGES;

Por ejemplo, para que lo use una API y no pueda agregar, cambiar o eliminar registros

    CREATE USER queryuser@'%' IDENTIFIED BY 'password1';
    GRANT SELECT ON database.* TO queryuser@'%';
    FLUSH PRIVILEGES;

Mostrar los privilegios

    SHOW GRANTS FOR user1@'localhost';

## Crear un usuario para respaldar

El usuario 'bdrespaldos'

- Puede conectarse desde cualquier dirección remota o local
- Puede accesar a todas las bases de datos
- No debe poder agregar, cambiar o eliminar registros

Para que se incluyan las vistas se usa LOCK TABLES

    GRANT LOCK TABLES, PROCESS, SELECT ON *.* TO 'bdrespaldos'@'%' IDENTIFIED BY 'password';
    FLUSH PRIVILEGES;

## Respaldar

Respaldar TODAS las bases de datos

    mysqldump -u bdrespaldos -p password --all-databases > /path/to/DATABASE-$(date +%Y-%m-%d).sql

Respaldar una base de datos

    mysqldump -u bdrespaldos -p password DATABASE > /path/to/DATABASE-$(date +%Y-%m-%d).sql

Respaldar una base de datos y comprimirla

    mysqldump -u bdrespaldos -p password DATABASE | gzip -c > /path/to/DATABASE-$(date +%Y-%m-%d).sql.gz

Respaldar una tabla

    mysqldump -u bdrespaldos -p password DATABASE TABLE > /path/to/DATABASE-$(date +%Y-%m-%d).sql

Respaldar sólo la estructura de una tabla con --no-data

    mysqldump -u bdrespaldos -p password --no-data DATABASE TABLE > /path/to/DATABASE-$(date +%Y-%m-%d).sql

Respaldar sólo los datos de una tabla con --no-create-info

    mysqldump -u bdrespaldos -p password --no-create-info DATABASE TABLE > /path/to/DATABASE-$(date +%Y%m%d%H%M%S).sql

## Restaurar

Para restablecer se ejecuta el comando mysql que toma el contenido de un archivo SQL como órdenes a seguir

    mysql -h 127.0.0.1 -u adminuser –p password DATABASE < /path/to/DATABASE-YYYY-MM-DD.sql

Por ejemplo, para restaurar una por una las tablas

    mysql -h 127.0.0.1 -u admin_plataforma_web -p DATABASE < DATABASE-TABLE1-202105101835.sql
    mysql -h 127.0.0.1 -u admin_plataforma_web -p DATABASE < DATABASE-TABLE2-202105101835.sql
    mysql -h 127.0.0.1 -u admin_plataforma_web -p DATABASE < DATABASE-TABLE3-202105101835.sql

## Guardar el usuario y contraseña

Crear un archivo .my.cnf en su home con

    [client]
    user=bdrespaldos
    password=password

Asegure cambiando los permisos

    chmod 600 .my.cnf

Puede probar con

    export MYSQL_HOST=127.0.0.1
    mysql -e 'SHOW DATABASES'

## Corrección

La tabla edictos tiene opcionales los campos expediente y numero_publicacion.

Flask con SQLAlchemy están creando las columnas que permiten valores NULOS.

Una columna string con valor nulo truena con FastAPI!!!

    DESCRIBE edictos;

    | expediente         | varchar(16)  | YES  |     | NULL
    | numero_publicacion | varchar(16)  | YES  |     | NULL

    UPDATE edictos SET expediente='' WHERE expediente IS NULL;
    ALTER TABLE edictos MODIFY expediente varchar(16) NOT NULL DEFAULT "";

    UPDATE edictos SET numero_publicacion='' WHERE numero_publicacion IS NULL;
    ALTER TABLE edictos MODIFY numero_publicacion varchar(16) NOT NULL DEFAULT "";

    DESCRIBE edictos;

    | expediente         | varchar(16)  | NO   |     |
    | numero_publicacion | varchar(16)  | NO   |     |

Fin.
