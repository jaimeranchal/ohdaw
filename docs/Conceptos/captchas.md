# Captchas

El término "Captcha" es el acrónimo de _Completely Automated Public Turing test_. Un **test de Turing**, resumiendo mucho, es una forma de distinguir un humano de una máquina; son una herramienta de **seguridad** habitual y necesaria cuando trabajamos con formularios o queremos controlar el acceso a ciertos recursos en internet.

!!! info "Mecanismo anti-spam"
    La función principal de las captchas es impedir el _spam_. Por ejemplo, que un programa pueda registrar datos de forma automatizada en un formulario para crear cuentas de email falsas, saturar un servidor etc.

Hay **muchas formas** de crear captchas; todos se basan en exigir al usuario que demuestre su "humanidad", normalmente reconociendo una serie de caracteres, patrones o entidades en una imagen, algo que las máquinas realizan con dificultad, o directamente no pueden.

## Seguridad

Para que una captcha funcione, debe cumplir una serie de requisitos:

1. Cada captcha debe ser **única**: evitar algoritmos fáciles de reproducir o desentrañar (`1+1` etc.).
2. Limitar el **acceso al script**: normalmente las funciones que generan la captcha están en el lado del servidor; si el envío de la respuesta del usuario está encriptado, aún mejor.
3. Si se usan **imágenes** deben ser _seguras_: cuanto menos distorsionada está una imagen, más vulnerable es ante ataques automatizados.
4. **Accesibilidad**: hay que tener en cuenta a usuarios que no puedan ver, incluyendo opciones para captchas con _sonido_.

## Ejemplos

### Php captcha

!!! info "Fuente"
    - https://www.w3schools.in/php-script/captcha/

### Php personalizado + JS

!!! info "Fuente"
    - https://www.w3school.info/2016/07/28/create-own-captcha-for-your-website/
    - https://www.thecrazyprogrammer.com/2016/03/javascript-captcha-example.html (sólo JS)

### Google reCaptcha

!!! info "Fuente"
    - https://www.easyappcode.com/como-utilizar-el-nuevo-recaptcha-v3-de-google-en-nuestro-proyecto-web
