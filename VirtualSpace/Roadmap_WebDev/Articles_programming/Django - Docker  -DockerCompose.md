#Learning  #Django #Docker 
### **Paso 1: Configuración del Proyecto Django (Local)**

Primero, configuramos el proyecto en tu máquina local, como lo harías normalmente.

1. **Crea la carpeta del proyecto y navega dentro de ella:**
  
```
mkdir api-con-docker
cd api-con-docker
```

2. **Crea y activa un entorno virtual:**

```
python -m venv venv
source venv/bin/activate  # En Linux/Mac
venv\Scripts\activate     # En Windows
```

3. **Instala Django y otros paquetes necesarios:**

```
pip install django djangorestframework psycopg2-binary
```

4. **Guarda las dependencias en un archivo:**

```
pip freeze > requirements.txt
```

5. **Crea el proyecto y la aplicación de Django:**

```
django-admin startproject mi_proyecto_django .
python manage.py startapp recetas
```


---

### **Paso 2: Containerización con Docker**

Una vez que el proyecto está listo, preparamos los archivos de Docker para que tu aplicación pueda ejecutarse dentro de contenedores.

1. **Crea el `Dockerfile`**: Este archivo le dice a Docker cómo construir la imagen de tu aplicación. Debe contener estas instrucciones:

```
FROM python:3.10
WORKDIR /api
COPY requirements.txt .
RUN pip install -r requirements.txt
COPY . .
EXPOSE 8000
CMD ["python", "manage.py", "runserver", "0.0.0.0:8000"]
```

2. **Crea el `docker-compose.yml`**: Este archivo coordina el servicio de tu API y el de la base de datos de PostgreSQL.

![[docker-compose services config.png]]

---

### **Paso 3: Puesta en Marcha y Migraciones**

Finalmente, usamos Docker para arrancar la aplicación, crear los modelos y aplicar las migraciones.

1. **Ejecuta los contenedores por primera vez**: `docker-compose up --build`

2. **Define tu modelo `Receta` en `recetas/models.py`**: Asegúrate de que tu aplicación (`recetas`) también esté en la lista `INSTALLED_APPS` en `mi_proyecto_django/settings.py`.

3. **Genera los archivos de migración**: `docker-compose exec api python manage.py makemigrations`

4. **Aplica las migraciones a la base de datos**: `docker-compose exec api python manage.py migrate`

#### Recordatorio

Debemos tener en cuenta que la configuración de la base de datos debe realizarse en el archivo settings del proyecto de Django porque allí esta la configuración de conexión a la nueva base de datos que usaremos que sera Postgres cambiando la que viene por defecto que es SQLite. La configuración debería verse así:

![[DB Postgres en Django settings.png]]
>*Esta configuración debe concordar con el servicio que queremos levantar del archivo  `docker-compose`*

