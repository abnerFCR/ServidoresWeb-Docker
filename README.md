
# Creacion de Servidores Web Nodejs con Docker

Para este tutorial se utilizara una maquina virtual Ubuntu 18.04 de google cloud.

Una vez preparado el equipo, se procedera a realizar los siguientes pasos.

1. Actualizar la lista de repositorios.
```sh
sudo apt-get update
```

2. Instalar nodejs.
```sh
sudo apt-get install nodejs
```

3. Instalar npm.
```sh
sudo apt-get install npm
```

4. Instalar docker
```sh
sudo apt-get install docker.io
```

5. Dar permisos de docker al usuario con el que estamos trabajando

```sh
sudo usermod -aG docker [NOMBRE_USUARIO]
```

6. Cerrar la terminal y volverla a abrir. (Reiniciar terminal)

## Servidor Web con imagen ubuntu:latest

7. Bajamos la imagen con la que trabajaremos en este caso ubuntu:latest
```sh 
docker pull ubuntu:latest
```

8. Creamos el contenedor y ejecutamos su bash enlazando el puerto 3000 de la maquina anfitrion con el puerto 3000 del contenedor
```sh
docker run -it -d -p 3000:3000 --name=servidor1 ubuntu:latest /bin/bash
```

9. Una vez dentro del contenedor actualizamos e instalamos nodejs y npm (siguiendo los pasos 1, 2, 3)

Nota: Verificar que la instalacion se haya hecho de manera correcta (nodejs -v, npm --version)

10. Instalamos la libreria express de npm de manera global.

```sh
sudo npm install -g express
```

11. (Opcional) Instalar nano para poder trabajar con un editor de texto en consola.

```sh
sudo apt install nano
```

12. Elegimos el directorio en el que tendremos nuestros archivos.

13. Creamos un archivo index.js

```sh
nano index.js
```
14. Editamos el contenido del archivo con este contenido
```sh
var express = require('express');

var app = express();

app.get('/', function (req, res) {
  res.send('Inicio');
  console.log("Página de inicio...")
})

app.get('/app', function (req, res) {
  res.send('Abner Fernando Cardona Ramirez - 201603095  (Distroless)');
  console.log("Informacion");
})

app.listen(3000);
```

15. Instalamos express de manera local (ubicarnos en la carpeta que contiene nuestro index.js)

```sh
sudo npm install express
```

16. Corremos el servidor web

```sh
nodejs index.js
```

## Servidor Web con imagen distroless de nodejs




