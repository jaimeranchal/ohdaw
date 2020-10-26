# Manejando tipos MIME

## ¿Qué es MIME?
El estándar Extensiones Multipropósito de Correo de Internet o MIME (Multipurpose Internet Mail Extensions), especifica cómo debe transferir un programa archivos de texto, imagen, audio, vídeo o cualquier archivo que no esté codificado en US-ASCII. 

## ¿Cómo funciona?
Cuando un navegador intenta abrir un archivo el estándar MIME le permite saber con qué tipo de archivo está trabajando para que el programa asociado pueda abrirlo correctamente. Si el archivo no tiene un tipo MIME especificado el programa asociado puede suponer el tipo de archivo mediante la extensión del archivo.

Las **referencias al tipo MIME** se producen en el intercambio cliente-servidor, dentro de la _cabecera HTTP_.

## Identificador MIME
Es la cadena de texto que identifica el tipo de dato de un recurso: `categoría/tipo`. Por ejemplo, `text/html`.

Aparecen en tres lugares:

- El **servidor**
- Como atributos de algunas etiquetas HTML
```html
<link href="./miarchivo.css" rel="stylesheet" type="text/css">
<!-- Lo siguiente ya es un poco anticuado -->
<script language="JavaScript" type="text/javascript" src="scripts/mijavascript.js"></script>
<meta http-equiv="Content-Type" content="text/html; charset=iso-8859-1"/>
```

- En el **navegador**, dentro de la cabecera _Accept_:
```
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8
```

## Configuración del servidor
En un servidor web Apache podemos especificar el tipo MIME para aquellos archivos que el servidor no pueda identificar automáticamente como pertenecientes a un tipo concreto. Para ello necesitamos el **módulo `mod_mime`**.

### Mod_mime
Este módulo suele estar activado por defecto en sistemas Debian, en el archivo `/etc/apache2/mods-available/mime.conf`. El archivo `/etc/mime.types` contiene la lista de tipos reconocidos.

#### Directivas

- `DefaultType` (obsoleta): establece la cabecera `Content-Type` a cualquier archivo cuyo MIME no se pueda determinar por la extensión.
- `ForceType`: fuerza el tipo indicado a todos los archivos.
    - Sólo en un **contexto de directorio** (`Directory`, `Files` y `Location`), y en archivos `.htaccess`.
```apacheconf
<Directory /var/www/html/misimagenes>
    ForceType image/png
</Directory>
```
- `AddType`: indica el tipo MIME para una o varias extensiones concretas (normalmente no reconocidas por Apache). Se admite a **nivel global**, de **directorio** o en archivos `.htaccess`.
```apacheconf
# sirve los archivos con extensión .imagen como PNG
AddType image/png imagen
```
