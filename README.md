# flask
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

Al cerrar la aplicación se elimina la imagen.
