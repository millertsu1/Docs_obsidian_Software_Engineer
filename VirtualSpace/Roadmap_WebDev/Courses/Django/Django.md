#Learning 

[[Como trabajar con un Repositorio Remoto de Django]]

## Plan de estudios
### Semana 1: Fundamentos de Django y Modelos

> Objetivo: Entender la estructura de un proyecto Django, crear modelos y manejar la base de datos.
#### Día 1 (2 horas):

- Conceptos: Instalación de Django, `startproject vs startapp`, estructura de directorios.

- Práctica: Configura un nuevo proyecto Django. Crea tu primera aplicación (blog, por ejemplo).
#### Día 2 (2 horas):

- Conceptos: Modelos de Django, tipos de campos (CharField, DateTimeField, ForeignKey), makemigrations y migrate.

- Práctica: Define un modelo Post con campos como titulo, contenido, fecha_publicacion y un ForeignKey a un User.
#### Día 3 (2 horas):

- Conceptos: Django Admin, personalización del panel de administración.

- Práctica: Registra tu modelo Post en admin.py y usa el panel de administración para crear, editar y eliminar publicaciones.
#### Día 4 (2 horas):

- Conceptos: Vistas basadas en funciones (FBV), URLconf.

- Práctica: Crea una vista simple que muestre un mensaje "Hola mundo" y configúrala en urls.py.
#### Día 5 (2 horas):

- Conceptos: Plantillas de Django, lenguaje de plantillas (DTL).

- Práctica: Pasa datos desde una vista a una plantilla HTML y renderiza una lista de tus posts.
#### Día 6 y 7:

- Proyecto 1: Crea un blog simple donde puedas ver una lista de posts y el detalle de cada uno.

---
### Semana 2: Vistas, Formularios y Vistas Basadas en Clases (CBV)

> Objetivo: Gestionar el flujo de datos con vistas y formularios, y optimizar el código con CBV.
#### Día 1 (2 horas):

- Conceptos: Formularios de Django, ModelForm.

- Práctica: Crea un formulario para agregar un nuevo post y otro para editarlo.
#### Día 2 (2 horas):

- Conceptos: Procesamiento de formularios en vistas (manejo de POST).

- Práctica: Implementa la lógica para guardar un nuevo post en la base de datos a través del formulario.
#### Día 3 (2 horas):

- Conceptos: Vistas Basadas en Clases (CBV) para crear, editar y eliminar (CreateView, UpdateView, DeleteView).

- Práctica: Refactoriza tus vistas basadas en funciones a CBV. Esto es muy útil para reducir la cantidad de código.
#### Día 4 (2 horas):

- Conceptos: Sistema de URLs con path y name, y redirecciones (redirect).

- Práctica: Usa path(..., name='...') y redirect('nombre_de_url') en tus vistas.
#### Día 5 (2 horas):

- Conceptos: Clases de vista genéricas para mostrar listas y detalles (ListView, DetailView).

- Práctica: Refactoriza tus vistas del blog para usar estas CBV.
#### Día 6 y 7:

- Proyecto 2: Mejora tu blog con la capacidad de crear, editar y eliminar posts a través de formularios.

---

### Semana 3: Autenticación, Usuarios y Permisos

> Objetivo: Implementar un sistema de registro, inicio de sesión y gestión de usuarios.
#### Día 1 (2 horas):

- Conceptos: Sistema de autenticación de Django (User, login, logout).

- Práctica: Configura las URLs de inicio de sesión y cierre de sesión.
#### Día 2 (2 horas):

- Conceptos: Vistas de registro de usuario.

- Práctica: Crea un formulario de registro y una vista para que los nuevos usuarios puedan registrarse.
#### Día 3 (2 horas):

- Conceptos: Decoradores (@login_required) y Mixins (LoginRequiredMixin).

- Práctica: Restringe el acceso a las vistas de crear y editar posts solo a usuarios autenticados.
#### Día 4 (2 horas):

- Conceptos: Permisos de usuario.

- Práctica: Restringe la edición y eliminación de posts solo al usuario que los creó.
#### Día 5 (2 horas):

- Conceptos: Archivos estáticos y de medios (STATIC_URL, MEDIA_URL).

- Práctica: Sube una imagen a tus posts y configúrala para que se muestre correctamente.
#### Día 6 y 7:

- Proyecto 3: Agrega funcionalidades de autenticación a tu blog. Ahora, cada post está asociado a un usuario, y solo ese usuario puede editarlo o eliminarlo.

---

### Semana 4: API REST con Django REST Framework y Despliegue

> Objetivo: Exponer datos a través de una API y aprender a desplegar tu proyecto.
#### Día 1 (2 horas):

- Conceptos: Introducción a DRF (Django REST Framework), Serializers.

- Práctica: Instala DRF y crea un Serializer para tu modelo Post.
#### Día 2 (2 horas):

- Conceptos: Vistas de API (APIView, generics.ListAPIView).

- Práctica: Crea una vista de API para listar todos los posts y otra para ver el detalle de un post.
####  Día 3 (2 horas)

- Conceptos: ViewSet y Router.

- Práctica: Simplifica tus vistas de API usando ModelViewSet y un Router para generar las URLs automáticamente.
#### Día 4 (2 horas):

- Conceptos: Despliegue de una aplicación Django (servidor web, Gunicorn, Nginx).

- Práctica: Prepara tu proyecto para el despliegue (cambia DEBUG=False, configura las variables de entorno, etc.).
#### Día 5 (2 horas):

- Conceptos: Heroku o un servicio similar de PaaS (Platform as a Service).

- Práctica: Despliega tu proyecto en Heroku o PythonAnywhere.
#### Día 6 y 7:

- Proyecto 4: Agrega una API a tu blog y despliega la versión final del proyecto.**

## Lista de comandos y funcionalidades
### [docs](https://docs.djangoproject.com/en/5.2/)

``` bash
python -m django --version
```

> *con este comando podemos conocer la version instalada de Django en nuestra computadora*


``` python
django-admin startproject recetas_perros
``` 

> *Con este comando le estamos indicando a Django que cree un nuevo proyecto llamado **recetas_perros***

```
recetas_perros/
    manage.py
    recetas_perros/
        __init__.py
        settings.py
        urls.py
        asgi.py
        wsgi.py
```

> *Esta seria al estructura del proyecto hasta el momento, se crea el proyecto  **recetas_perros***

veamos para que funciona cada archivo y carpeta interna del proyecto y fuera de el:

1. **manage.py**: este es un archivo, puede considerarse el mas importante de todos ya que nos permite interactuar de muchas manera con el proyecto.
2. **mysite**: es la carpeta que maneja el paquete del proyecto actual, al usar el comando `django-admin startproject [nombre del proyecto]`, se crea esta carpeta y dentro todo el paquete del proyecto, para que este funcione.
3. **`__init__.py`**: Es un una archivo vacío que le dice a Python que este directorio debe considerarse como un paquete de python.  
4. **`setting.py`**: es una archivo que nos permite configurar nuestro proyecto, maneja el funcionamiento de la configuración.
5. **`urls.py`**: Se encarga de manejar las URLS para el proyecto.
6. **`asgi.py`**: Es el punto de entrada para la configuración de servidores del proyecto.
7. **`wsgi.py`**: Es el punto de entrada para la configuración de servidores del proyecto.

```python
python manage.py runserver
```

>*Es el comando que nos permite correr el servidor para ver y probar nuestra aplicación, nos genera una URL de nuestro servidor local donde podremos ver nuestro **sitio web***. 

![[activacion EntornoVirtual.png]]


> *Este comando crea el servidor en el puerto 8000 por defecto y allí podremos ver nuestra aplicación corriendo*

![[runserver command.png]]

```python
python manage.py startapp recetas
```

>*Con este comando podemos crear aplicaciones en nuestro proyecto. Las aplicaciones son como las secciones funcionales del sitio, en el comando estamos creando la  aplicación **RECETAS** que es la que se encargara de manejar las recetas  de nuestra pagina de recetas.
>
>Al ejecutar el comando, se creara una carpeta al mismo nivel de nuestro proyecto y tendrá el nombre que le dimos, en nuestro caso `recetas`.*



```bash
recetas/
    __init__.py
    admin.py
    apps.py
    migrations/
        __init__.py
    models.py
    tests.py
    views.py
```

>*Esta carpeta contendrá una estructura de archivos y capetas para que nuestra aplicación funcione.*

### Vistas(views en Django)

las vistas(views), en Django son funcionalidades que toman requerimientos web y retornan una respuesta, estas respuestas pueden ser contenidos HTML de nuestra pagina web, una redirección, una imagen o en realidad cualquier cosa. **La vista en si misma contiene la lógica necesaria para retornar esa respuesta**.

Estas vistas se crean en el archivo `views.py`, y por cada aplicación se genera una archivo que guardara la lógica de esta aplicación específicamente.

## Pasos iniciales con Django I
#### [[Primeros pasos]]

1. creamos la carpeta que contendrá nuestro proyecto. Esta creación puede ser manual o mediante el código.

```bash
mkdir nombre_de_la_carpeta
```

2. creamos un entorno virtual dentro de esa carpeta, esto nos ayudara a separar nuestros proyectos en carpetas separadas e instalar solo lo necesario en cada proyecto.

```python
python -m venv nombre_del_entorno
```

> *nótese que que en nombre del entorno se lo damos nosotros mismos, así que aquí podemos elegir nuestro nombre de entorno.*

3. Activamos nuestro entorno. Los entornos se pueden encender y apagar, por lo general al activarlo la terminal o consola, nos informa que nuestro entorno esta activado

```shell
env/Scripts/activate
```
>*Así se activa el entorno en `Windows`⬆️*

```bash
source env/bin/activate
```
>*Así se activa en `Linux` ⬆️*



![[activacion EntornoVirtual.png]]

>* En la terminal al inicio de la ruta de nuestro proyecto, sale el nombre del entorno, esto significa que nuestro entorno esta activado, de hecho la terminal los resalta en color `verde`*

4. Luego de activar nuestro entorno `debemos instalar Django` en nuestro entorno, recordemos que nuestro entorno es un espacio de trabajo vacío que no tiene nada instalado, así que necesitamos indicarle con que herramientas vamos a trabajar.

```python
pip install django
```
>*con este comando instalaremos la ultima version de Django, la que esta vigente a la fecha de usar el comando.*

Si deseamos instalar o porque estamos trabajando con proyectos legacy que en su momento usaron una version de Django especifica, podemos instalar esa version, si conocemos la version a instalar.

```python
pip install django==numero_de_version
```
>*Debemos reemplazar **numero_de_version** con el numero de la version a usar*

```python
pip install django==5.2.4
```
>*En este ejemplo, estamos instalando la version `5.4.2`, de Django*

-  Podemos verificar nuestra instalación y que dependencia(herramientas), tenemos instaladas en nuestro entorno.
```python
pip list | pip freeze
```
>*Con `pip list` o con `pip freeze`, podemos verificar que tenemos instalado en nuestro entorno y nuestro proyecto*

![[pip list VS pip freeze.png]]
>*la diferencia radica en la presentación de que esta instalado nada mas*

5. Creamos nuestro proyecto, en este punto debemos conocer que lo anterior es solo para hacer las instalaciones de la herramientas, ahora vamos a instalar nuestro proyecto.

```python
django-admin startproject nombre_del_proyecto
```
>*para crear una carpeta dentro de la carpeta del proyecto de Django creamos una carpeta de nuestro proyecto personal, al mismo nivel del entorno virtual*

o

```python
django-admin startproject nombre_del_proyecto .
```
>*Este comando hace lo mismo que el anterior solo que no crea una carpeta nueva, sino que instala los archivos al mismo nivel de la carpeta del entrono virtual, **este es el mas recomendado ya que no crea anidaciones de carpetas***

6. Instalar aplicaciones de proyecto, estas son las instalaciones de aplicaciones del proyecto, son como las funcionalidades del proyecto en donde cada aplicación manejara una parte del proyecto, por ejemplo los `usuarios`.

```python
python manage.py startapp nombre_de_la_aplicacion
```
>*Este es el comando para crear aplicaciones, es muy similar al del proyecto por la sintaxis, se reconoce, pero este comando necesita de `startproject`, para funcionar correctamente, ya que usa las configuraciones del archivo `manage.py`.*

Cuando nosotros creamos una nueva aplicacion debemos dirigirnos al archivo `settings.py`, de nuestro proyecto y agregar la aplicacion que hemos creado a nuestras aplicaciones instaladas en `INSTALLED_APPS`, y agregar a la lista nuestra nueva app. Esta lista contiene todas las aplicaciones por defecto y a medida que avancemos y creemos mas, las iremos agregando a este apartado. Veamos la lista de apps por defecto que se instalan al usar el comando para crear nuestro proyecto:

- django.contrib.admin', => maneja el administrador del los contendidos del sitio
- 'django.contrib.auth', => un sistema que maneja la autenticacion
- 'django.contrib.contenttypes', => un framework para manejar los content types
- 'django.contrib.sessions', => un framework para manejar las sesiones
- 'django.contrib.messages', => un framework para manejar los mensajes
- 'django.contrib.staticfiles', => un framework para manejar los archivos estáticos

7. crear un usuario para interactuar con el contenido de nuestro sitio desde la parte administrativa del mismo.

```python
python manage.py create superuser
```
>*Este comando nos permite crear un usuario, un correo asociado a nuestro usuario y una contrasena para poder ingresar a la parte administrativa, la idea cuando se trabaje sobre un proyecto real es poder asegurar nuestro sitio con un usuario, correo y contrasena fuerte para evitar fallos que comprometan la seguridad del sitio.*

Cuando usamos este comando, por defecto crea un usuario superadministrador, ya que el comando en si, lleva el atributo `is_staff` con valor `true`, lo cual asegura el rol de administrador total de la plataforma.

## Django Rest Framework DRF
### Sintaxis de un serializer 

Un **serializador** es como un **"traductor" o "empaquetador"** que toma sus objetos de Django (nuestra camiseta del almacén) y los convierte en este formato JSON (la ficha de producto digital), listo para ser enviado a la aplicación del celular.

Pero no solo eso, ¡también funciona al revés! Si el celular quiere comprar una camiseta, nos enviará una "ficha de pedido" en formato JSON. El serializador también puede tomar ese JSON, **validar** que la información sea correcta (que el precio sea un número, que el stock no sea negativo, etc.) y luego **convertirla de nuevo** en un objeto de Django para guardarlo en nuestra base de datos.

[[Serializers sintaxis]]


