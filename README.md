# Python/Flask web app

## Versión simple: un servidor

Se facilitan todos los elementos para la puesta en marcha de una aplicación web implementada con Flask. Para su funcionamiento requiere un servidor Redis. El fichero Compose "compose_simple.yaml" contiene lo necesario para lanzarla. El proceso incluye la construcción de una imagen de la aplicación web, para lo que se incluye el correspondiente Dockerfile. 

Renombrar o copiar el fichero Compose citado como "compose.yaml". Lanzar el sistema con `docker compose up -d`. Una conexión a "http://localhost:4000" dará acceso a la aplicación. En el navegador aparecerá

```
Hello World!
Hostname: 9b71507f400a <-- este valor es siempre el mismo, el nombre dado por Docker al servidor web
Visits: 3              <-- esto se incrementará de uno en uno cada vez que hay un acceso
```
La aplicación se detiene con `docker compose down`.

## Versión escalable con múltiples servidores y un balanceador de carga

Añadimos a la aplicación anterior un balanceador de carga basado en Nginx. El fichero Compose correspondiente es "compose_lb.yaml". Se necesita además un fichero de configuración para Nginx, ya incluido. Renombrar o copiar este fichero como "compose.yaml" y ejecutar la aplicación como antes (`docker compose up -d`), lo que lanzará dos instancias del servidor web. La conexión a "http://localhost:4000" se conecta a Nginx, que redirigirá el tráfico hacia uno cualquiera de los servidores reales:

```
Hello World!
Hostname: ad74069d28af <-- esto cambiará en función del número de servidores
Visits: 3              <-- esto se incrementará de uno en uno cada vez que hay un acceso
```
Podemos incrementar el número de servidores con `docker compose up --scale web=4`. Así se crearán dos servidores web adicionales. 

Paramos la aplicación con `docker compose down`.

## Referencia de los ficheros
1. Dockerfile -- necesario para construir la imagen del servidor web
2. app.py -- código de la aplicación, se usa en el Dockerfile
3. requirements.txt -- pre-requisitos de la aplicación web, se usa en el Dockerfile
4. compose_simple.yaml -- fichero Compose para la construcción de la versión mono-servidor de la aplicación
5. nginx.conf -- configuración del balanceador de carga Nginx
6. compose_lb.yaml -- fichero Compose para la construcción de la aplicación con múltiples servidores
7. README.md -- este documento
