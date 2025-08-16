#Learning  #Seo

### [Video como medir clics en botones con GTM y GA4](https://www.youtube.com/watch?v=7OA_u8x_g68)
### Paso 1: Acceder a Google Tag Manager y a Variables

1. Ingresa a tu cuenta de Google Tag Manager.
    
2. Dirígete a la sección de **Variables** en el menú lateral.
    
3. Dentro de **Variables**, haz clic en **Configurar**.
    
4. Activa las variables relacionadas con **clics** y **formularios**.
    
5. Cierra la configuración de variables.
    

---
### Paso 2: Crear una Nueva Etiqueta

1. Ve a la sección de **Etiquetas** en el menú lateral.
    
2. Haz clic en **Nueva** para crear una nueva etiqueta.
    
3. Haz clic en **Configuración de la etiqueta**.
    
4. Selecciona la etiqueta de **Google Analytics 4**.
    
    - **Nota**: Este paso solo es posible si ya tienes creada y configurada la propiedad de Google Analytics 4 en tu sitio.
        
5. Selecciona la etiqueta de configuración de **GA4** que creaste previamente.
    
6. En el campo de "Nombre del evento", escribe un nombre descriptivo, como **"botón de contacto"**.
    

---

### Paso 3: Crear el Activador

1. En la sección de la etiqueta, haz clic en el cuadro de **Activación**.
    
2. Haz clic en el ícono "+" para crear un nuevo activador.
    
3. Selecciona el tipo de activador **"Clic en todos los elementos"**.
    
4. Elige la opción **"Algunos clics"** para definir una condición.
    
5. Selecciona la variable **"Clic en texto"**.
    
6. Elige la condición **"contiene"**.
    
7. En el campo de texto, escribe el texto exacto del botón que quieres medir, en este caso **"contacto"**.
    
8. Asigna un nombre al activador, por ejemplo, **"clic en contacto"**, y guárdalo.
    

---

### Paso 4: Guardar y Publicar la Etiqueta

1. Asigna un nombre a la etiqueta, por ejemplo, **"evento GA4: clic en contacto"**.
    
2. Haz clic en **Guardar**.
    
3. Una vez que la etiqueta esté lista, haz clic en **Enviar** para publicar los cambios.
    

---

### Paso 5: Probar la Configuración

1. Haz clic en el botón de **Vista previa** en la parte superior derecha de Google Tag Manager.
    
2. Ingresa la URL de tu sitio web para conectar el modo de prueba.
    
3. En tu sitio web, haz clic en el botón que configuraste (el botón de "contacto").
    
4. Regresa a la ventana de la vista previa de Google Tag Manager y verifica que la etiqueta **"evento GA4: clic en contacto"** se haya activado correctamente.
    
5. Para confirmar, puedes ir a tu cuenta de Google Analytics 4, en la sección de **Tiempo real**, para ver si el evento personalizado aparece en el listado.