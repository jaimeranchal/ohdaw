# VsFTPd

!!! note "Servicios"
    Si tenemos más de un servidor FTP instalado, debemos **detener** completamente el que esté corriendo antes de arrancar otro. Por ejemplo, si hemos instalado previamente _proftpd_ y queremos iniciar _vsftpd_, antes debemos "matar" el primero:
    ```bash
    # Detenemos el servicio
    sudo systemctl proftpd stop

    # El comando anterior solo cambia el estado del servicio a "inactivo"
    # Para comprobar si todavía está escuchando:
    sudo lsof -i:21

    # Si lo está, tomamos el PID del proceso y lo matamos
    sudo kill 9999
    ```


