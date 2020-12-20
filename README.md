
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
  res.send('Abner Fernando Cardona Ramirez - 201603095');
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


7. Creamos un directorio que enlazaremos posteriormente con el contenedor
```sh
mkdir nuevaCarpeta
```
8. Ingresamos en el contenedor y creamos un archivo Dockerfile
```sh
cd nuevaCarpeta
nano Dockerfile
```
9. El archivo Dockerfile tendra el siguiente contenido 

```sh
FROM node:10 AS build-env
ADD . /app
WORKDIR /app
RUN npm install --production

FROM gcr.io/distroless/nodejs:10
COPY --from=build-env /app /app
WORKDIR /app
EXPOSE 3000
CMD ["hello_express.js"]
```

10. Creamos un archivo hello_express.js (nano hello_express.js) con el siguiente contenido

```sh 
nano hello_express.js
```
```sh
const express = require('express')
const app = express()
const port = 3000

app.get('/', (req, res) => res.send('Abner Fernando Cardona - 201603095 (Distroless)'))

app.listen(port, () => console.log(`Example app listening on port ${port}!`))
```

11. Creamos otro archivo llamado package.json (nano package.json) con el siguiente contenido:

```sh
nano package.json
``

```sh
{
  "name": "distroless-express",
  "version": "1.0.0",
  "description": "Distroless express node.js",
  "repository": {
    "type": "git",
    "url": "https://github.com/GoogleContainerTools/distroless.git"
  },
  "dependencies": {
    "express": "4.16.3"
  },
  "author": "Bryant Hagadorn",
  "license": "ISC"
}
```

12. Instalamos las dependencias de npm. 

```sh
npm install
```

13. Construimos la imagen que utilizaremos con el archivo Dockerfile

```sh
docker build -t [miapp] .
```

14. Creamos el contenedor con docker un y exponemos el puerto del contenedor a uno de la maquina local conectando la carpeta donde esta nuestro servidor con una carpeta de nuestro contenedor distroless.

```sh
docker run -p 4000:3000 -v /home/abner_cardona1997/tarea3:/app -t myappdistroless
```

Ref:
https://www.hostinger.es/tutoriales/instalar-nano-text-editor
https://github.com/GoogleContainerTools/distroless
https://platzi.com/tutoriales/1050-programacion-basica/296-instalar-e-iniciar-un-servidor-nodejs-en-linux-ubuntu-1404/
https://github.com/sergioarmgpl/taller-docker
