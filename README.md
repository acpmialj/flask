# Python/Flask web app

Se facilitan todos los elementos para la puesta en marcha de una aplicación web implementada con Flask. Para su funcionamiento requiere un servidor Redis. El fichero Compose "compose_simple.yaml" contiene lo necesario para lanzarla. El proceso incluye la construcción de una imagen de la aplicación web, para lo que se incluye el correspondiente Dockerfile. 

Renombrar o copiar el fichero Compose citado como "compose.yaml". Lanzar el sistema con "docker compose up -d". Una conexión a "http://localhost:4000" dará acceso a la aplicación. En el navegador aparecerá

```shell
Hello World!
Hostname: 9b71507f400a <-- este valor es siempre el mismo, el nombre dado por Docker al servidor web
Visits: 3              <-- esto se incrementará de uno en uno cada vez que hay un acceso
```

# Haciendo la aplicación escalable con un balanceador de carga

Añadimos a la aplicación anterior un balanceador de carga basado en Nginx. El fichero Compose correspondiente es "compose_lb.yaml". Se necesita además un fichero de configuración para Nginx, ya incluido. Renombrar o copiar este fichero como "compose.yaml" y ejecutar la aplicación como antes ("docker compose up -d"), lo que lanzará dos instancias del servidor web. La conexión a "http://localhost:4000" se conecta a Nginx, que redirigirá el tráfico hacia uno cualquiera de los servidores reales:

```shell
Hello World!
Hostname: 9b71507f400a <-- esto cambiará en función del número de servidores
Visits: 3              <-- esto se incrementará de uno en uno cada vez que hay un acceso
```



Los pasos a dar para ejecutar la aplicación son
```shell
$ mkdir flask
$ cd flask
$ nano Dockerfile
$ nano requirements.txt
$ nano app.py
$ ls
app.py	Dockerfile	requirements.txt
$ docker build -t myflask .
Sending build context to Docker daemon   5.12kB
Step 1/7 : FROM python:2.7-slim
2.7-slim: Pulling from library/python
123275d6e508: Pull complete
…
Collecting Flask
  Downloading Flask-1.1.4-py2.py3-none-any.whl (94 kB)
Collecting Redis
  Downloading redis-3.5.3-py2.py3-none-any.whl (72 kB)
…
Successfully installed Flask-1.1.4 Jinja2-2.11.3 MarkupSafe-1.1.1 Redis-3.5.3 Werkzeug-1.0.1 click-7.1.2 itsdangerous-1.1.0
…
Successfully built da163f72de52
Successfully tagged myflask:latest
$ docker image ls
REPOSITORY                    TAG            IMAGE ID       CREATED          SIZE
myflask                       latest         da163f72de52   22 seconds ago   159MB
$ docker run --rm -p 4000:80 myflask
 * Serving Flask app "app" (lazy loading)
 * Environment: production
   WARNING: This is a development server. Do not use it in a production deployment.
   Use a production WSGI server instead.
 * Debug mode: off
 * Running on http://0.0.0.0:80/ (Press CTRL+C to quit)
 ```

Hecho esto, podemos conectarnos a la aplicación desde un navegador, usando http://dir_del_host:4000. Si el navegador está en el host, se puede usar localhost:4000. Se obtendrá esta salida:

```shell
Hello World!
Hostname: 9b71507f400a
Visits: cannot connect to Redis, counter disabled
```

El error en el apartado "Visits:" es porque no hay servidor Redis en ejecución. 

Al cerrar la aplicación se elimina la imagen.
