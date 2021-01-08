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

=== "Clase principal"
    ```php
    <?php
    /*phptext class, version 1.0
    created by www.w3schools.in (Gautam kumar)
    April 26, 2014
    */class phptextClass {
        public function phpcaptcha($textColor,$backgroundColor,$imgWidth,$imgHeight,$noiceLines=0,$noiceDots=0,$noiceColor='#162453') {
            /* Settings */
            $text=$this->random();
            $font = './font/monofont.ttf';/* font */$textColor=$this->hexToRGB($textColor);
            $fontSize = $imgHeight * 0.75;

            $im = imagecreatetruecolor($imgWidth, $imgHeight);
            $textColor = imagecolorallocate($im, $textColor['r'],$textColor['g'],$textColor['b']);

            $backgroundColor = $this->hexToRGB($backgroundColor);
            $backgroundColor = imagecolorallocate($im, $backgroundColor['r'],$backgroundColor['g'],$backgroundColor['b']);

            /* generating lines randomly in background of image */
            if($noiceLines>0){
                $noiceColor=$this->hexToRGB($noiceColor);
                $noiceColor = imagecolorallocate($im, $noiceColor['r'],$noiceColor['g'],$noiceColor['b']);

                for( $i=0; $i<$noiceLines; $i++ ) {
                    imageline($im, mt_rand(0,$imgWidth), mt_rand(0,$imgHeight),
                    mt_rand(0,$imgWidth), mt_rand(0,$imgHeight), $noiceColor);
                }
            }
            /* generating the dots randomly in background */
            if($noiceDots>0){
                for( $i=0; $i<$noiceDots; $i++ ) {
                imagefilledellipse($im, mt_rand(0,$imgWidth),
                mt_rand(0,$imgHeight), 3, 3, $textColor);
                }
            }

            imagefill($im,0,0,$backgroundColor);
            list($x, $y) = $this->ImageTTFCenter($im, $text, $font, $fontSize);
            imagettftext($im, $fontSize, 0, $x, $y, $textColor, $font, $text);
            
            /* Showing image */
            imagejpeg($im,NULL,90);
            /* defining the image type to be shown in browser widow */
            header('Content-Type: image/jpeg');
            /* Destroying image instance */
            imagedestroy($im);
            /* set random text in session for captcha validation*/
            if(isset($_SESSION)){
                $_SESSION['captcha_code'] = $text;
            }
        }

        /*function to convert hex value to rgb array*/
        protected function hexToRGB($colour) {
            if ( $colour[0] == '#' ) {
                $colour = substr( $colour, 1 );
            }
            if ( strlen( $colour ) == 6 ) {
                list( $r, $g, $b ) = array( $colour[0] . $colour[1], $colour[2] . $colour[3], $colour[4] . $colour[5] );
            } elseif ( strlen( $colour ) == 3 ) {
                list( $r, $g, $b ) = array( $colour[0] . $colour[0], $colour[1] . $colour[1], $colour[2] . $colour[2] );
            } else {
                return false;
            }
            $r = hexdec( $r );
            $g = hexdec( $g );
            $b = hexdec( $b );
            return array( 'r' => $r, 'g' => $g, 'b' => $b );
        }


        /*function to get center position on image*/
        protected function ImageTTFCenter($image, $text, $font, $size, $angle = 8) {
            $xi = imagesx($image);
            $yi = imagesy($image);
            $box = imagettfbbox($size, $angle, $font, $text);
            $xr = abs(max($box[2], $box[4]))+5;
            $yr = abs(max($box[5], $box[7]));
            $x = intval(($xi - $xr) / 2);
            $y = intval(($yi + $yr) / 2);
            return array($x, $y);
        }
    }
    ?>
    ```
=== "Recoger y validar"
    ```php
    <?php session_start();

    if(isset($_POST['Submit'])){
        // code for check server side validation
        if(empty($_SESSION['captcha_code'] ) || strcasecmp($_SESSION['captcha_code'], $_POST['captcha_code']) != 0){  
            $msg="<span style='color:red'>The Validation code does not match!</span>";// Captcha verification is incorrect.
        }else{// Captcha verification is Correct. Final Code Execute here!
            $msg="<span style='color:green'>The Validation code has been matched.</span>";
        }
    }
    ?>
      
    <form action="" method="post" name="form1" id="form1" >
        <table width="400" border="0" align="center" cellpadding="5" cellspacing="1" class="table">
        <?php if(isset($msg)){?>
            <tr>
                <td colspan="2" align="center" valign="top"><?php echo $msg;?></td>
            </tr>
        <?php } ?>
            <tr>
                <td align="right" valign="top"> Validation code:</td>
                <td><img src="captcha.php?rand=<?php echo rand();?>" id='captchaimg'><br>
                <label for='message'>Enter the code above here :</label>
                <br>
                <input id="captcha_code" name="captcha_code" type="text">
                <br>
                Can't read the image? click <a href='javascript: refreshCaptcha();'>here</a> to refresh.</td>
            </tr>
            <tr>
                <td> </td>
                <td><input name="Submit" type="submit" onclick="return validate();" value="Submit" class="button1"></td>
            </tr>
        </table>
    </form>
    ```
=== "Generar captcha"
    ```php
    <?php
    session_start();
    include("./phptextClass.php");

    /*create class object*/
    $phptextObj = new phptextClass();
    /*phptext function to genrate image with text*/
    $phptextObj->phpcaptcha('#162453','#fff',120,40,10,25);
     ?>
    ```

### Php personalizado + JS

!!! info "Fuente"
    1. https://www.thecrazyprogrammer.com/2016/03/javascript-captcha-example.html (sólo JS)
    2. https://www.w3school.info/2016/07/28/create-own-captcha-for-your-website/

#### Ejemplo 1 (sólo JS)

=== "Genera captcha de 4 dígitos con JS"
    ```html
    <html>
        <head><title>JavaScript Captcha Example</title></head>
        <body onload="generateCaptcha()">
         
            <script>
                var captcha;
                 
                /* Genera 4 números aleatorios entre 1 y 10 */
                function generateCaptcha() {
                    var a = Math.floor((Math.random() * 10));
                    var b = Math.floor((Math.random() * 10));
                    var c = Math.floor((Math.random() * 10));
                    var d = Math.floor((Math.random() * 10));
                 
                    // une los números en una cadena
                    captcha=a.toString()+b.toString()+c.toString()+d.toString();
                    // inserta la cadena en el html 
                    document.getElementById("captcha").value = captcha;
                }
                 
                /* Comprueba que el texto introducido coincide con la captcha */
                function check(){
                    var input=document.getElementById("inputText").value;
                 
                    if(input==captcha){
                        alert("Equal");
                    }
                    else{
                        alert("Not Equal");
                    }
                }
            </script>
         
            <input type="text" id="captcha" disabled/><br/><br/>
            <input type="text" id="inputText"/><br/><br/>
            <button onclick="generateCaptcha()">Refresh</button>
            <button onclick="check()">Submit</button>
         
        </body>
    </html>
    ```

#### Ejemplo 2: php + JS

!!! note "Requisitos"
    Este ejemplo requiere instalar la **librería GD** en tu servidor php.

=== "Captcha.php"
    ```php
    <?php session_start();
    if(isset($_SESSION['custom_captcha'])) {
        unset($_SESSION['custom_captcha']); // destroy the session if already there
    }
    $string1="abcdefghijklmnopqrstuvwxyz";
    $string2="1234567890";
    $string=$string1.$string2;
    $string= str_shuffle($string);

    $random_text= substr($string,0,8); // change the number to change number of chars

    $_SESSION['custom_captcha'] = $random_text;

    $im = @ImageCreate (80, 20)
    or die ("Cannot Initialize new GD image stream");

    $background_color = ImageColorAllocate ($im, 204, 204, 204); // Assign background color
    $text_color = ImageColorAllocate ($im, 51, 51, 255); // text color is given 

    ImageString($im,5,5,2,$_SESSION['custom_captcha'],$text_color); // Random string from session added 
    ImagePng ($im); // image displayed
    imagedestroy($im); // Memory allocation for the image is removed. 
    ?>
    ```
=== "Captcha.js (+JQuery)"
    ```javascript
    "use strict"
    function reload() {
        img = document.getElementById("capt");
        img.src = "captcha.php";
    }

    $(document).ready(function(){
        let html = `<p><img src="captcha.php" id = "capt">&nbsp;
            <input type="image" src="reload.png" onClick="reload();"></p>
            <p><input type="text" name="g-recaptcha-response">`;
        // ajusta el contenido de la captcha (cambia #custom_captcha por tu id)
        $('#custom_captcha').html(html);
    })
    ```
=== "Form.html"
    ```html
    <head>
        ...
        <script src="https://ajax.googleapis.com/ajax/libs/jquery/1.12.4/jquery.min.js"></script>
        <script src="captcha.js"></script>
        ...
    </head>
    ...
    <?php session_start(); ?>
    ...
    <!--inside the form tag-->
    <!--this custom_captcha id is used in js file,you can change in js and html both-->
    <div id="custom_captcha"></div>
    <!--inside the form tag-->
    ```
=== "submit.php"
    ```php
    <?php

    if(!isset($_REQUEST['g-recaptcha-response']) || empty($_POST['g-recaptcha-response'])) {
        
        $error = "Please fill the captcha.";
        //you can use this error var later to show error message on your page
    } else {
     
        $cresponse = urlencode($_REQUEST['g-recaptcha-response']);
     
        if($cresponse!=$_SESSION['custom_captcha']){
            $error = "INVALID CAPTCHA";
            //you can use this var to show error invalid captcha
        }
    }

    //Now if no error then do your further process//
    if($errors=="") {
        //go ahed with form submission or data manipulation
    }

    ?>
    ```
    

### Google reCaptcha

!!! info "Fuente"
    - https://www.easyappcode.com/como-utilizar-el-nuevo-recaptcha-v3-de-google-en-nuestro-proyecto-web

Lo primero es entrar en la [página para desarrolladores de Google](https://www.google.com/recaptcha/intro/v3.html):

1. Añadimos la **etiqueta** que identifique nuestro dominio.
2. Escogemos la **versión** (v3 p.e).
3. Introducimos el **dominio**
4. Aceptar las condiciones y enviar.

Al poco recibiremos unas **claves** (dos: pública y secreta) que habrá que integrar en nuestra página.

=== "JavaScript"
    ```html
    <!-- en CLAVE_PUBLICA introduce la que te proporcione Google -->
    <script src='https://www.google.com/recaptcha/api.js?render=CLAVE_PUBLICA'></script>
    <script>
        grecaptcha.ready(function() {
            // en CLAVE_PUBLICA introduce la que te proporcione Google
            grecaptcha.execute('CLAVE_PUBLICA', {action: 'homepage'})
                .then(function(token) {
                document.getElementById('g-recaptcha-response').value=token;
            });
        });
    </script>
    ```
=== "Html"
    ```html
    <form method="POST" action="EnviarDatos.php">
        <label for="email">Correo Eléctronico</label>
        <input type="email" id="email" name="email" placeholder="Escriba su email" required>

        <!-- Input Oculto del recaptcha-->
        <input type="hidden" id="g-recaptcha-response" name="g-recaptcha-response">  
    </form>
    ```
=== "submit.php"
    ```php
    <?php
    class Captcha {
        public function getCaptcha($SecretKey){
            $respuesta = file_get_contents("https://www.google.com/recaptcha/api/
            siteverify?secret=CLAVE_SECRETA&response={$SecretKey}");
        }
        // Devuelve la respuesta de la api de Google en formato JSON:
        // - Contiene una puntuación que usaremos para decidir si es un bot o una persona
        $retorno = json_decode($respuesta);
        return $retorno;
        }
    }

    function envioMensaje(){
        if(isset($_POST['email'])){
            $email = htmlentities($_POST['email'], ENT_QUOTES);
            $ObjCaptcha = new Captcha();
            $retorno = $ObjCaptcha->getCaptcha($_POST['g-recaptcha-response']);

            if($email === ""){
                echo 'El campo Email es obligatorio';
            } elseif($retorno->success == true && $retorno->score > 0.5){
                // script php (envia mensaje, inserta en bbdd etc)
            } else {
                echo 'No pareces muy humano...';
            }
        }
    }
    ?>
    ```
