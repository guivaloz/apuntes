
# Podman PgAdmin4

Es necesario arrancar el contenedor con sudo para que tenga una dirección IP

    sudo podman run --rm --name pgadmin4 -p 8080:80 -e 'PGADMIN_DEFAULT_EMAIL=FULANO@gmail.com' -e 'PGADMIN_DEFAULT_PASSWORD=CONTRASENA' -d dpage/pgadmin4

Las opciones definen

- Que se va al fondo con -d
- Que sea elimado el contedor al matar con --rm
- Nombre pgadmin4
- Que el puerto local 8080 apunte al puerto 80 del contenedor
- Define FULANO@gmail.com como el usuario para ingresar
- Define CONTRASENA como contraseña para ingresar

Inspeccione para saber la dirección IP

    sudo podman inspect pgadmin4 | grep IPAddress
    "IPAddress": "10.88.0.2"

Hay que permitir el acceso desde ese rango de IPs, edite

    sudo nano /var/lib/pgsql/data/pg_hba.conf

Agregar estas líneas

    # PgAdmin4
    # TYPE  DATABASE                USER        ADDRESS             METHOD
    host    all                     USUARIO    10.88.0.0/24        trust

Reinicie

    sudo systemctl restart postgresql

Use el navegador de Internet

    http://localhost:8080

Configure:

- Dirección IP: 10.88.0.1
- Usuario: USUARIO
- Contraseña vacía
