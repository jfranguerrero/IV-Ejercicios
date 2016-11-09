# Tema 3

## Ejercicio 1. Darse de alta en algún servicio PaaS tal como Heroku, Nodejitsu, BlueMix u OpenShift.

De los servicios Paas enunciados nos registraremos en OpenShift. Para ello lo primero
que haremos será acceder a openshift.com.

Nos aparecerá la siguiente pantalla:

![alt text](http://i67.tinypic.com/wv48lk.png)

Pincharemos en Sign up for free y nos registraremos mediante GitHub. Nos pedirá GitHub que
autoricemos la aplicación.

![alt text](http://i65.tinypic.com/29w92co.png)

Nos aparecerá un formulario con la información extraída de GitHub y algún cuando aún por
rellenar. Simplemente le indicamos lo solicitado y pulsamos en registrar.

![alt text](http://i67.tinypic.com/10elmba.png)

Con esto ya estaremos registados y tan solo deberemos esperar a la activación de nuestra
cuenta.


## Ejercicio 2. Crear una aplicación en OpenShift o en algún otro PaaS en el que se haya dado uno de alta. Realizar un despliegue de prueba usando alguno de los ejemplos.

Como ejemplo se va a instalar wordpress en Heroku. Obviaremos que tenemos ya instalada la línea
de comandos de Heroku.

Lo primero que debemos hacer es un git clone de donde están ubicados los ficheros de Wordpress.

```
git clone git://github.com/mhoofman/wordpress-heroku.git
```

Accedemos al directorio y creamos un proyecto mediante heroku create.

Nos generará algo parecido a lo siguiente:

```sh
francisco@francisco-Lenovo-IdeaPad-Y510P:~/wordpress-heroku$ heroku create
Creating app... done, ⬢ damp-citadel-82518
https://damp-citadel-82518.herokuapp.com/ | https://git.heroku.com/damp-citadel-82518.git

```
Esa será la URL de nuestra aplicación por la cual deberemos acceder.

A continuación crearemos una base de datos postgresql con:

```
heroku addons:create heroku-postgresql
heroku pg:promote heroku-postgresql
```
Habilitaremos la posibilidad de mandar emails con:

```
heroku addons:create sendgrid:starter
```

Se recomienda crear una nueva rama para modificaciones de la configuración. lo hacemos
con git checkout -b production.

Le indicamos las variables de entorno y las claves únicas. Las podemos generar de
forma aleatoria y nunca se deben mostrar pero como nuestra aplicación solo tiene un fin
educativo no tendrá transcendencia el poner la introducidas en el ejemplo.

```
heroku config:set AUTH_KEY='cw2S}*^fv-sgf{^krm`b6eRAQ)lQ!60/64rx +J*p@|fx-[+k Se-w0svUoeRz6Y' \
  SECURE_AUTH_KEY='!++axdt<vhQz{%Cs6P9?NSL[(~T~ZrW,G.(3/&-z)[DBO(RK~_/f2p3!LP0dD{Cg' \
  LOGGED_IN_KEY='@l(;9`-^cWAzh*~NI3R-:h%dZ<}%(]AiLhxc1QQ5I&*zR$;WO4)]H[A~ymXz,,1=' \
  NONCE_KEY='c@Gu@Bk3&L_awgr^0?FY11%JUlD-WO.9FK=6y5cX-K=@1[S]$dl6rJ+e2(&22WWk' \
  AUTH_SALT='iZC/8C#pty:isqR{ogzej7LiJhFHD%sgf0cb.I3dKgr-pWs/:SZmF(xN#YL+2#2Z' \
  SECURE_AUTH_SALT='aV8U~~ZoTd9+1hRCz|0LtP3e!Pz^nD[Ey;/Y/qk3mZ$jff@UInwrU%P)l]&LVc-2' \
  LOGGED_IN_SALT='BNraKk=1TPXmAGr-<:D*&-51S&QNc`%SMWWGN uG[u.GiGF>t9YQ)!!g{Oyu+YWp' \
  NONCE_SALT='#q=^`pD{4+-XjU{+f9MX!`QLBAAZPK+H*h`$XAn!(B|SqKDj[,%78zg8++BhGG#E'

```

Lo último que nos queda por hacer es el despliegue y lo haremos con:
```
git push heroku production:master
```
Si accedemos a nuestra url de Heroku ya tendremos nuestra página de instalación de
Wordpress.

![alt text](http://i68.tinypic.com/10ie2p4.png)

Nuestro blog quedará de la siguiente forma:

![alt text](http://i63.tinypic.com/11bizxj.png)

## Ejercicio 3. Realizar una app en express (o el lenguaje y marco elegido) que incluya variables como en el caso anterior.

Para realizar el ejercicio se ha realizado una simple aplicación el Flask que según
la URL introducida devuelve un string en nuestro navegador. Para lo que necesitamos la aplicación,
nos es suficiente.

```
from flask import Flask, url_for
app = Flask(__name__)

@app.route('/')
def index():
    return 'Bienvenido'

@app.route('/articulos')
def api_articles():
    return 'Listado de ' + url_for('api_articles')

@app.route('/articulos/<articleid>')
def api_article(articleid):
    return 'Es el artículo número ' + articleid


@app.route('/test')
def test():
    return 'Testeo de la miniaplicación para IV'

@app.errorhandler(404)
def page_not_found(e):
    return "Error, no se encuntra la página", 404


if __name__ == "__main__":
    app.run(host='0.0.0.0')


```
![alt text](http://i65.tinypic.com/1zlsn4w.png)

![alt text](http://i67.tinypic.com/2pr9xlh.png)

![alt text](http://i67.tinypic.com/20p31fk.png)

![alt text](http://i64.tinypic.com/2ypji14.png)


## Ejercicio 4. Crear pruebas para las diferentes rutas de la aplicación.

Según la documentación de Flask se ha realizado el siguiente fichero de tests.

```
import unittest
import os
import EjIV
from flask_testing import TestCase
import tempfile

class EjercicioIVTestCase(unittest.TestCase):

    def setUp(self):
        self.db_fd, EjIV.app.config['DATABASE'] = tempfile.mkstemp()
        EjIV.app.config['TESTING'] = True
        self.app = EjIV.app.test_client()


    def tearDown(self):
        os.close(self.db_fd)
        os.unlink(EjIV.app.config['DATABASE'])

    def test_articulos(self):
        result = self.app.get('/articulos')
        self.assertEqual(result.status_code, 200)
        self.assertEqual(result.data, "Listado de /articulos")


    def test_articulo4(self):
        result = self.app.get('/articulos/4')
        self.assertEqual(result.status_code, 200)
        self.assertEqual(result.data, "Es el articulo numero 4")


    def test_error(self):
        result = self.app.get('/error')
        self.assertEqual(result.status_code, 404)
        self.assertEqual(result.data, "Error, no se encuntra la pagina")


    def test_index(self):
        result = self.app.get('/')
        self.assertEqual(result.status_code, 200)
        self.assertEqual(result.data, "Bienvenido")
        print result.data

if __name__ == '__main__':
    unittest.main()

```
Aquí se adjunta una captura para mostrar que los tests se realizaron con éxito.


![alt text](http://i67.tinypic.com/25s2bfl.png)


## Ejercicio 5. Instalar y echar a andar tu primera aplicación en Heroku.

Aunque ya hemos hecho funcionar Wordpress en Heroku volveremos a hacer lo mismo pero
con nuestra pequeña aplicación.

Lo primero que hacemos es crear un directorio y un git init para hacerlo un repositorio donde introducir nuestra aplicación.

Creamos un fichero Procfile con ```web gunicorn EjIV:app --log-file -```

Realizado esto cremos nuestro proyecto con ```heroku create --buildpack heroku/python```.

Posteriormente nos queda realizar un git push heroku master.

Nuestra URL es: https://fast-chamber-91870.herokuapp.com/

![alt text](http://i65.tinypic.com/2gvl9bl.png)


## Ejercicio 6. Usar como base la aplicación de ejemplo de heroku y combinarla con la aplicación en node que se ha creado anteriormente. Probarla de forma local con foreman. Al final de cada modificación, los tests tendrán que funcionar correctamente; cuando se pasen los tests, se puede volver a desplegar en heroku.

Para ejecutarlo de forma local instalados foreman. Esto lo hacemos con:

```
sudo gem install foreman -v 0.61

```

En el directorio donde está Procfile realizamos un foreman start y nuestra aplicación
debe funcionar correctamente.

![alt text](http://i63.tinypic.com/2088qrr.png)


## Ejercicio 7. Haz alguna modificación a tu aplicación en node.js para Heroku, sin olvidar añadir los tests para la nueva funcionalidad, y configura el despliegue automático a Heroku usando Snap CI o alguno de los otros servicios, como Codeship, mencionados en StackOverflow.

Aprovechando los conocimientos de tema anterior lo más fácil sería usar Travis-CI. Cuando se hayan superado los tests en Travis-CI se desplegará automáticamente en Heroku.

Esto lo llevamos a cabo desde las opciones de nuestra aplicación en Heroku sincronizando nuestro repositorio de GitHub y marcando "Wait for CI to pass before deploy".

![alt text](http://i67.tinypic.com/2uygdw0.png)

De esta form Heroku cargará las novedades de nuestra aplicación sólo cuando se hayan realizado los tests de forma correcta.


## Ejercicio 8. Preparar la aplicación con la que se ha venido trabajando hasta este momento para ejecutarse en un PaaS, el que se haya elegido.

Este ejercicio se ha realizado en el ejercicio 5.
