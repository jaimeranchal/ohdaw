# Servidor Apache

## 1. Introducción

### ¿Qué es Apache?

## 2. Primeros pasos

### Instalación
Hay varias formas de instalación:

=== "Con apt"
    ```sh
    sudo su
    apt install apache2
    # Para montar una plataforma LAMP, buscamos el resto de paquetes
    apt search php | grep apache2
    # Instalamos la opción que más nos convenga. Empezará por libapache2-mod-...
    apt search mysql-server
    apt search mysql | grep php
    ```
=== "Manual"
    ```sh
    # Lo que sigue es orientativo: habrá que ajustarlo a la máquina particular
    #Instalamos los paquetes base para poder compilar apache en Ubuntu:
    sudo apt-get install build-essential checkinstall
    sudo apt-get install libpcre3-dev

    #Creamos los directorios donde se alojará el código fuente
    mkdir $HOME/src

    #Descargamos los paquetes necesario, el servidor httpd y dos paquetes de los que depende.
    cd $HOME/src
    wget http://apache.rediris.es/httpd/httpd-2.4.25.tar.gz
    wget http://apache.rediris.es//apr/apr-1.5.2.tar.gz
    wget http://apache.rediris.es//apr/apr-util-1.5.4.tar.gz

    #Descomprimimos el código fuente de apache y de los otros paquetes necesarios
    cd $HOME/src
    tar xvzf httpd-2.2.19.tar.gz
    mkdir $HOME/src/httpd-2.4.25/srclib/apr
    tar xvfz apr-1.5.2.tar.gz -C $HOME/src/httpd-2.4.25/srclib/
    mv $HOME/src/httpd-2.4.25/srclib/apr-1.5.2 $HOME/src/httpd-2.4.25/srclib/apr
    tar xvfz apr-util-1.5.4.tar.gz -C /home/test/src/httpd-2.4.25/srclib/
    mv $HOME/src/httpd-2.4.25/srclib/apr-util-1.5.4 $HOME/src/httpd-2.4.25/srclib/apr-util

    #Configuramos la compilación de apache
    cd $HOME/src/httpd-2.4.25
    ./configure --with-included-apr --prefix=/opt/apache --enable-module=most --enable-mods-shared=most

    #Compilamos e instalamos (en este caso se instalará en la carpeta /opt/apache)
    make
    sudo make install
    ```

### Esquema de archivos

Este es el árbol de carpetas y ficheros que se crean por defecto al instalar Apache:

```sh
/etc/apache2
├── conf-available/
├── conf-enabled/
├── envvars
├── magic
├── mods-available/
|   └── mime.conf
├── mods-enabled/
├── ports.conf
├── sites-available/
|   ├── 000-default.conf
|   └── default-ssl.conf
├── sites-enabled/
└── apache2.conf
```


### Iniciar y detener Apache

=== "Iniciar"
    ```sh
    /opt/apache/bin/apachectl -f /opt/apache/conf/httpd.conf -k start
    service apache2 start
    apache2ctl start
    /etc/init.d/apache2 start
    ```
=== "Detener"
    ```sh
    /opt/apache/bin/apachectl -f /opt/apache/conf/httpd.conf -k stop
    service apache2 stop
    apache2ctl stop
    /etc/init.d/apache2 stop
    ```
=== "Reiniciar"
    ```sh
    /opt/apache/bin/apachectl -f /opt/apache/conf/httpd.conf -k restart
    service apache2 restart
    apache2ctl restart
    /etc/init.d/apache2 restart
    ```

!!! note "Permisos"
    Salvo excepciones, apache requiere permisos de **superusuario**.

- El proceso principal (`httpd`) se ejecuta como _root_, pero los hijos tienen **menos privilegios**
- Se puede especificar el **grupo** y **usuario** de los procesos hijo con las directivas `User` y `Group`

## 3. El archivo de configuración

### Cómo funciona
Apache se configura mediante instrucciones escritas en un **fichero de configuración** en texto plano. Estas instrucciones se llaman **directivas**.

- Las **configuración global** (para todos los sitios) se guarda en:
    - `/etc/apache2/` (Ubuntu, Debian, SUSE).
    - `/etc/httpd/conf/` (RedHat y derivados).
- Podemos añadir **opciones específicas para un sitio web** en un archivo de configuración separado, guardado en `sites-available/sitio1.conf`, p.e.
    - Para que tengan efecto, debemos incluirlas en la configuración global, con la directiva _Include_ o _IncludeOptional_:

    ```apacheconf
    # Include da error si el archivo indicado no existe
    Include ports.conf
    # IncludeOptional, no
    IncludeOptional sites-enabled/*.conf
    IncludeOptional mods-enabled/*.conf
    IncludeOptional conf-enabled/*.conf
    ```

!!! note "Actualizar los cambios"
    **Apache2** sólo _reconoce los cambios_ de los archivos de configuración **cuando se inicia** o se **reinicia**.

