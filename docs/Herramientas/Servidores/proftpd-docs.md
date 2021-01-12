### DefaultRoot

- Establece el directorio al que acceden los usuarios cuando inicien sesión.
- Puedes añadir tantos parámetros como usuarios o grupos diferentes necesites

```conf
# Sintaxis
DefaultRoot [dir] [grupo de usuarios con acceso a dir]

DefaultRoot /home/ftp A # A solo puede acceder a /home/ftp
DefaultRoot / B         # B puede acceder a todo el disco
```

### ServerName

Asigna un nombre al servidor

### AccessGrantMsg

Añade un mensaje de bienvenida.

```conf
AccessGrantMsg  "Bienvenido a mis archivos"
```

### AccessDenyMsg

Añade un mensaje de error al iniciar sesión.

```conf
AccessDenyMsg  "Acceso denegado"
```


