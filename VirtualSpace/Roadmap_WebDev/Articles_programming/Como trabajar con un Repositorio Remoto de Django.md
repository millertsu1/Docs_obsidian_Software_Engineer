#Learning #Django #HowToDo 

En ocasiones hay oportunidades de trabajar con repositorios externos, clonándolos o haciendo `fork`, para trabajar en mejoras de proyectos open source, así que en este articulo plasmamos lo que debes tener en cuenta a la hora de clonar tu repositorio  y los pasos siguientes para que no te preocupes por esta fase y solo te dediques a editar y mejorar.

### Pasos para trabajar con un repo remoto de Django

1. ve al tu proyecto de GitHub y usa el link para clonar
2. En la carpeta donde va a bajar el repo, vas abrir una terminal y vas a escribir el comando para clonar el repo:

```git
git clone url_del_repositorio
```
> *Esto creara una carpeta y pondrá todo el contenido del repo ya clonado en nuestra terminal local, así que ya podremos interactuar con el repositorio*

***recordatorio: antes de usar el entorno virtual debemos tenerlo instalado usando el comando:* 

```python
pip install virtualenv
```


3. Ahora ingresamos a la carpeta del repo clonado y  creamos una `entorno virtual`. Con el comando:
```python
python -m venv nombre_del_entorno
```

4. Activamos nuestro entorno virtual, esto para instalar nuestra dependencias solo para nuestro proyecto. Lo hacemos con el comando:
```python
nombre_del_entorno/Scripts/activate
```
>*Este comando nos permite activar en Windows nuestro entorno*

```python
source nombre_del_entorno/bin/activate
```
>*Este comando nos permite activar el entorno virtual en Linux*

**Advertencia**

>*El repositorio debe contener el archivo `requirements.txt`, este archivo contiene toda la información sobre las dependencias instaladas en el proyecto y es necesario para poder hacer uso del proyecto una vez instalado en nuestro entorno*

5. Ya teniendo nuestro entorno virtual activo, ingresamos al proyecto que clonamos e instalamos nuestro archivo con las dependencias. O sea el archivo requirements.txt, con el siguiente comando:

```python
pip install -r requirements.txt
```
> *Con este comando se instalaran todas las dependencias del proyecto para que este funcione correctamente al desplegarlo o al usarlo  en nuestro servidor local.*

Si hacemos una verificación con el comando `pip list`, veremos que se instalaron las dependencias del proyecto.

6. Finalmente desplegamos nuestro servidor local de pruebas para poder ver funcionado nuestro proyecto.

```python
python manage.py runserver
```
>*Con este comando levantamos nuestro servidor local para ver como esta quedando.*

Debemos tener en cuenta que todos los proyectos de Django, particularmente tengan el archivo `requirements.txt`. Si no cuentan con este archivo debemos hacer el proceso de identificar manualmente en nuestro `settings.py`, cuales fueron las herramientas instaladas y así poder instalarlas y hacer funcionar nuestro proyecto.


