
# Squid Proxy

Instalar Squid

    sudo su -
    dnf install squid

Mueva la configuración original

    cd /etc/squid/
    mv squid.conf squid.conf.original

Editar /etc/squid/squid.conf

    # ACL
    acl SSL_ports port 443
    acl SSL_ports port 5228         # Google talk
    acl Safe_ports port 80          # http
    acl Safe_ports port 21          # ftp
    acl Safe_ports port 443         # https
    acl Safe_ports port 1025-65535  # unregistered ports
    acl CONNECT method CONNECT

    # La red local
    acl lan src 172.16.0.0/24

    # Definir sitios permitidos y prohibidos
    acl SitiosPermitidos  dstdomain  "/etc/squid/sitios-permitidos.txt"
    acl SitiosProhibidos  dstdomain  "/etc/squid/sitios-prohibidos.txt"

    # Definir publicidad, ejecute
    #   wget "http://pgl.yoyo.org/adservers/serverlist.php?hostformat=squid-dstdom-regex&showintro=0&mimetype=plaintext" -O regexp-publicidad.txt
    acl RegExpPublicidad  dstdom_regex -i "/etc/squid/regexp-publicidad.txt"

    #
    # Reglas
    #

    # Deny requests to certain unsafe ports
    http_access deny !Safe_ports

    # Deny CONNECT to other than secure SSL ports
    http_access deny CONNECT !SSL_ports

    # Only allow cachemgr access from localhost
    http_access allow localhost manager
    http_access deny manager

    # Permitir sitios permitidos
    http_access allow SitiosPermitidos

    # NEGAR prohibidos y publicidad
    http_access deny SitiosProhibidos
    http_access deny RegExpPublicidad

    # Permitir TODO a la red local
    http_access allow lan

    # And finally deny all other access to this proxy
    http_access deny all

    #
    # Opciones
    #

    # Squid normally listens to port 3128
    http_port 3128

    # Disk cache directory
    cache_dir ufs /var/spool/squid 8192 64 256

    # Leave coredumps in the first cache dir
    coredump_dir /var/spool/squid

    # Refresh pattern
    refresh_pattern ^ftp:           1440    20%     10080
    refresh_pattern ^gopher:        1440    0%      1440
    refresh_pattern -i (/cgi-bin/|\?) 0     0%      0
    refresh_pattern .               0       20%     4320

    # Reducir tiempo de espera de conexiones para apagar rápido
    shutdown_lifetime 4 seconds

Editar /etc/squid/sitios-permitidos.txt

    .movimientolibre.com
    .wikipedia.org
    .github.com
    .firefox.com
    .mozilla.com
    .mozilla.org
    .google-analytics.com
    analytics.google.com

Editar /etc/squid/sitios-prohibidos.txt

    .windowsupdate.com
    .msftncsi.com
    .akamaihd.net
    .caliente.mx

Crear /etc/squid/regexp-publicidad.txt

    wget "http://pgl.yoyo.org/adservers/serverlist.php?hostformat=squid-dstdom-regex&showintro=0&mimetype=plaintext" -O regexp-publicidad.txt

Prepare el directorio para el caché

    mkdir /var/cache/squid
    chown squid /var/cache/squid
    squid -z -N -f /etc/squid/squid.conf

Antes de arrancar hay que permitir a Squid guardar su bitácora

    dnf install policycoreutils-devel
    mkdir /root/squid-policy
    cd /root/squid-policy
    nano SquidPolicy.te

Escribir este contenido en SquidPolicy.te

    module SquidPolicy 1.0;

    require {
        type squid_t;
        type user_tmp_t;
        type var_t;
        class file { append create getattr open read rename unlink write };
    }

    allow squid_t user_tmp_t:file unlink;
    allow squid_t var_t:file unlink;
    allow squid_t var_t:file { append create getattr open read rename write };

Compile e instale

    checkmodule -M -m -o SquidPolicy.mod SquidPolicy.te
    semodule_package -o SquidPolicy.pp -m SquidPolicy.mod
    semodule -i SquidPolicy.pp

Arranque y habilite el daemon

    systemctl start squid.service
    systemctl status squid.service
    systemctl enable squid.service

Abra su puerto en el muro de fuego

    firewall-cmd --get-active-zones
    firewall-cmd --zone=FedoraServer --list-services
    firewall-cmd --zone=FedoraServer --add-service=squid
    firewall-cmd --zone=FedoraServer --list-services
    firewall-cmd --runtime-to-permanent

Para mostrar la bitácora de accesos

    tail -f /var/log/squid/access.log

Para ver la bitácora del sistema

    sudo journalctl -u squid -f
