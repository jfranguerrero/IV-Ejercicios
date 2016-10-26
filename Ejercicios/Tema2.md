# Tema 2

## Ejercicio 1. Instalar alguno de los entornos virtuales de node.js (o de cualquier otro lenguaje con el que se esté familiarizado) y, con ellos, instalar la última versión existente, la versión minor más actual de la 4.x y lo mismo para la 0.11 o alguna impar (de desarrollo).

En mi caso se va a instalar virtualenv para Python. Para ello lo primero que hacemos
es instalarlo mediante:

  ```sh
  sudo pip install virtualenv
  ```
Si no tenemos pip instalado también lo podemos hacer de la siguiente manera:

```sh
$ curl -O https://pypi.python.org/packages/source/v/virtualenv/virtualenv-X.X.tar.gz
$ tar xvfz virtualenv-X.X.tar.gz
$ cd virtualenv-X.X
$ [sudo] python setup.py install
```
Siendo X la versión deseada por el usuario.

Para crear un entorno introducimos:
```
virtualenv nombreentorno
```
Para activarlo simplemente hacemos dentro del directorio del entorno source bin/activate.

En mi caso he creado nos entornos uno con Python 2.7 y otro con Python 3.4.

Esto lo he hecho con:

```
virtualenv -p /usr/bin/python2.7 entorno1
virtualenv -p /usr/bin/python3.4 entorno2
```
Solo queda activar el deseado con source bin/activate.



## Ejercicio 2. Como ejercicio, algo ligeramente diferente: una web para calificar las empresas en las que hacen prácticas los alumnos. Las acciones serían

    Crear empresa
    Listar calificaciones para cada empresa
    crear calificación y añadirla (comprobando que la persona no la haya añadido ya)
    borrar calificación (si se arrepiente o te denuncia la empresa o algo)
    Hacer un ránking de empresas por calificación, por ejemplo
    Crear un repositorio en GitHub para la librería y crear un pequeño programa
    que use algunas de sus funcionalidades.



## Ejercicio 3. Ejecutar el programa en diferentes versiones del lenguaje. ¿Funciona en todas ellas?

La aplicación se ha desarrollado en Python y con el framework Django. Esto se llevó
a cabo con Python 2.7.

Para comprobar el correcto funcionamiento se crearán dos entornos virtuales, uno con
Python2 y otro con Python3.4

Tras instalar Django en ellas ambas han funcionado sin problema alguno, se han podido
crear empresas, asignarle notas y eliminarlas.

Por tanto en este caso la versión del lenguaje no nos ha influido.

```sh
francisco@francisco-Lenovo-IdeaPad-Y510P:~$ virtualenv -p /usr/bin/python3.4 entorno2
Running virtualenv with interpreter /usr/bin/python3.4
cUsing base prefix '/usr'
dNew python executable in /home/francisco/entorno2/bin/python3.4
Also creating executable in /home/francisco/entorno2/bin/python
Installing setuptools, pip, wheel.done.
francisco@francisco-Lenovo-IdeaPad-Y510P:~$ cd entorno2
francisco@francisco-Lenovo-IdeaPad-Y510P:~/entorno2$ source bin/activate
(entorno2) francisco@francisco-Lenovo-IdeaPad-Y510P:~/Escritorio/practicaEmpresa$ pip install django
Collecting django
  Using cached Django-1.10.2-py2.py3-none-any.whl
Installing collected packages: django
Successfully installed django-1.10.2
(entorno2) francisco@francisco-Lenovo-IdeaPad-Y510P:~/Escritorio/practicaEmpresa$ python manage.py runserver
Performing system checks...

System check identified no issues (0 silenced).
October 23, 2016 - 22:09:46
Django version 1.10.2, using settings 'practicaEmpresa.settings'
Starting development server at http://127.0.0.1:8000/
Quit the server with CONTROL-C.

```

```sh
francisco@francisco-Lenovo-IdeaPad-Y510P:~/entorno1$ source bin/activate
(entorno1) francisco@francisco-Lenovo-IdeaPad-Y510P:~/entorno1$ cd ..
(entorno1) francisco@francisco-Lenovo-IdeaPad-Y510P:~$ pip install django
Collecting django
  Using cached Django-1.10.2-py2.py3-none-any.whl
Installing collected packages: django
Successfully installed django-1.10.2
(entorno1) francisco@francisco-Lenovo-IdeaPad-Y510P:~$ cd Escritorio/
(entorno1) francisco@francisco-Lenovo-IdeaPad-Y510P:~/Escritorio$ cd practicaEmpresa
(entorno1) francisco@francisco-Lenovo-IdeaPad-Y510P:~/Escritorio/practicaEmpresa$ python manage.py runserver
Performing system checks...

System check identified no issues (0 silenced).
October 23, 2016 - 22:12:04
Django version 1.10.2, using settings 'practicaEmpresa.settings'
Starting development server at http://127.0.0.1:8000/
Quit the server with CONTROL-C.

```


## Ejercicio 4. Crear una descripción del módulo usando package.json. En caso de que se trate de otro lenguaje, usar el método correspondiente.

En Python se indican las referencias con requirements.txt. Este fichero se puede
generar de forma automática con: pip freeze > requirements.txt

En nuestro caso lo vamos a hacer a mano. Para ello lo primero que hacemos es un pip list para que nos muestre todo lo instalado en nuestro entorno.

```
Django (1.10.2)
pip (8.1.2)
setuptools (28.6.1)
urllib3 (1.18)
wheel (0.30.0a0)

```
Esta información la indicamos en el fechero mencionado pero introducidos estos datos de forma manual.

```
Django==1.10.2
urllib3==1.18
wheel==0.30.0a0
setuptools==28.6.1

```
De esta forma no perdemos dependencia alguna ya que algunas no aparecen si se genera de forma automática.

## Ejercicio 5. Automatizar con grunt y docco (o algún otro sistema) la generación de documentación de la librería que se cree. Previamente, por supuesto, habrá que documentar tal librería.

En Python para generar documentación se usa la aplicación Sphinx. Para generar
la documentación de nuestro proyecto simplemente haremos lo siguiente con la ruta del proyecto:

sphinx-apidoc -F -o docs practicaEmpresa

Esto creará la documentación en el directorio docs. De forma automática no localiza
los módulos de Django por lo que debemos configurarlo de forma manual editando el
fichero conf.py. introduciremos lo siguiente:

```
import django
sys.path.insert(0, os.path.abspath('..'))
os.environ['DJANGO_SETTINGS_MODULE'] = 'myproject.settings'
django.setup()
```
Con esto podremos hacer un make html y se generará correctamente. Dicha documentación
está subida también a GitHub.

![alt text](http://i67.tinypic.com/2d9sk6r.png)


## Ejercicio 6. Para la aplicación que se está haciendo, escribir una serie de aserciones y probar que efectivamente no fallan. Añadir tests para una nueva funcionalidad, probar que falla y escribir el código para que no lo haga (vamos, lo que viene siendo TDD).

Se han realizado vamos tests con assert en el fichero tests.py (que es donde se hacen en Django). Los tests pretenden comprobar que los formularios funcionan correctamente y que se puede crear una empresa sin problema. Aquí se adjunta el código de los tests aunque también están en GitHub.

```
class empresaTest(TestCase):

    def test_formularioCreacion(self):
        datos_formulario={'nombre_empresa': 'Empresa Prueba', 'cif': '12345A'}
        form= empresaForm(data=datos_formulario)
        self.assertTrue(form.is_valid())



    def test_asignarCalificacion(self):
        datos_f={'nombre_empresa': 'Empresa Prueba', 'calificacion': '6'}
        form1= calificacionForm(data=datos_f)
        self.assertTrue(form1.is_valid())

    def test_deleteCalificacion(self):
        datos_f={'nombre_empresa': 'Empresa Prueba'}
        form1= deleteForm(data=datos_f)
        self.assertTrue(form1.is_valid())


    def test_crea_empresa(self):
		empress = empresa(nombre_empresa='test_empresa',cif='12345A')
		empress.crear()
		self.assertEqual(empress.__str__(), 'test_empresa')


```


## Ejercicio 7. Convertir los tests unitarios anteriores con assert a programas de test y ejecutarlos desde mocha, usando descripciones del test y del grupo de test de forma correcta. Si hasta ahora no has subido el código que has venido realizando a GitHub, es el momento de hacerlo, porque lo vas a necesitar un poco más adelante.

Al haber realizado la web mediante el framework Django, éste posee ya un método propio para realizar tests de forma automática y en el que nos muestra si han sido un éxito o un fracaso y el tiempo en el que se han realizado los tests.

Para realizar la ejecución tan solo deberemos introducir:

    python manage.py test empresa


## Ejercicio 8 A realizar con Travis


    Darse de alta. Muchos están conectados con GitHub por lo que puedes autentificarte directamente desde ahí. A través de un proceso de autorización, puedes acceder al contenido e incluso informar del resultado de los tests a GitHub.

    Activar el repositorio en el que se vaya a aplicar la integración continua. Travis permite hacerlo directamente desde tu configuración; en otros se dan de alta desde la web de GitHub.

    Crear un fichero de configuración para que se ejecute la integración y añadirlo al repositorio.



Nos damos de alta en Travis desde su web y mediante GitHub. Para ello debemos autorizar
a Travis a acceder a nuestra cuenta.

![alt text](http://i66.tinypic.com/21jaa04.png)

En nuestro perfil activamos el repositorio de GitHub deseado.

![alt text](http://i65.tinypic.com/2wd33vm.png)

Por úñtimo nos queda crear un fichero de configuración .travis.yml para poder ejecutar los tests. En él se deben lanzar los tests e indicarle los paquetes necesarios para la ejecución. En nuestro caso sería de la siguiente forma:

```
language: python
python:
 - "2.7"

install:
 - sudo apt-get install python-dev
 - pip install --upgrade pip
 - pip install Django

script:
- python manage.py test empresa
```

Cuando subamos el fichero a GitHub automáticamente se ejecutarán los tests desde Travis.
Podemos ver que se han completado con éxito en la siguiente imagen:

![alt text](http://i66.tinypic.com/9bm8ar.png)

Adicionamente podemos generar una etiqueta como la que se muestra a continuación que corrobora que se han superado los tests con éxito.

[![Build Status](https://travis-ci.org/jfranguerrero/Empresa_Ejercicio_IV.svg?branch=master)](https://travis-ci.org/jfranguerrero/Empresa_Ejercicio_IV)
