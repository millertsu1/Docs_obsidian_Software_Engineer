  ### prompt de testeo de proyecto

``` html
1. identifica los flujos críticos y asegúrate de que tengan test automáticos.
2. agrega test de regresión para los bug que ya ocurrieron.
3. revisa el tipado validaciones  y contratos entre el frontend, backend y APIs.
4. detecta código duplicado, dependencias ocultas y componentes demasiado acoplados.
5. Asegúrate que las migraciones sean seguras y tengan una estrategia de rollback.
6. configura checks automáticos antes de cada deploy, para que no se publique código roto.
7. chequea linting, test, build y type checking.
8. identifica las partes mas frágiles del proyecto y explica que cambios podrían romperlas.
9. haz un cambio pequeño  en cada parte critica y comprobar que los test detectan cualquier regresión.
10. Encuentra cualquier otro lugar  donde el proyecto dependa de que nadie cometa un error y no cometas errores.
```

***Sobre la visión de largo plazo que mencionas (propietario global con consentimiento de compartir historia entre clínicas), eso requeriría un modelo más profundo: separar el concepto de "persona/propietario" del de "paciente en una clínica", con campos de consentimiento GDPR. Es perfectamente viable pero es una feature separada que vale la pena planificar cuando el core esté estable.**

***Sobre la visión de largo plazo que mencionas (propietario global con consentimiento de compartir historia entre clínicas), eso requeriría un modelo más profundo: separar el concepto de "persona/propietario" del de "paciente en una clínica", con campos de consentimiento GDPR. Es perfectamente viable pero es una feature separada que vale la pena planificar cuando el core esté estable.**


```html
Investigar que metricas se pueden agregar al dashboard por ejemplo carnets por mes o historial clinicas por mes o pacientes nuevos en el mes.
```

``` js
¿Estás utilizando Checkout Pro, Checkout Bricks o la API de preferencias directamente para manejar las suscripciones de los tres planes?
```

Aquí están los pasos exactos para que el botón "Suscribirme a Pro/Premium" te lleve al checkout.
  
  ## Obtener tus credenciales de MercadoPago
  
  Entra a tu panel de desarrollador: https://www.mercadopago.com.co/developers/panel/app → crea/selecciona tu aplicación → sección
  Credenciales. Para probar, usa las credenciales de prueba (TEST). Necesitas:
  - Access Token (empieza con TEST-... o APP_USR-...)
  - Public Key (opcional para tu flujo actual, pero cópiala)
  
  ## Crear el archivo .env
  
  El archivo debe ir exactamente en backend/carnet_project/.env (al lado de settings.py, que es donde el load_dotenv lo busca). Te lo creo:
• Read backend/carnet_project
• Grep "\.env"
    ╰ path=., include=.gitignore
  5 matches in 2 files
    → C:\Users\mille\OneDrive\Escritorio\DEV\carnet-vacunas\.gitignore (4)
      2:.env
      3:.env.local
      4:.env.*.local
      ...+1 more
    → C:\Users\mille\OneDrive\Escritorio\DEV\carnet-vacunas\frontend\.gitignore (1)
      34:.env*
    ...+1 matches
    (ctrl+o to toggle)

• .env está correctamente ignorado por git (línea 2 de .gitignore), así que crearlo no subirá secretos al repo. Confirmado que no existe aún en
  carnet_project/.
  
  Te creo el .env con una plantilla. Importante: deja los placeholders y reemplázalos con tus credenciales reales (no las pongas en el chat).
• Write C:\Users\mille\OneDrive\Escritorio\DEV\carnet-vacunas\backend\carnet_project\.env
  added 24 lines in .env
     1+  # ─────────────────────────────────────────────────────────────                                                                       
     2+  # Configuración de entorno - backend (NO subir a git)                                                                                 
     3+  # ─────────────────────────────────────────────────────────────                                                                       
     4+                                                                                                                                        
     5+  # Django                                                                                                                              
     6+  SECRET_KEY=dev-secret-key-cambiar-en-produccion                                                                                       
     7+  DEBUG=True                                                                                                                            
     8+  ALLOWED_HOSTS=localhost,127.0.0.1                                                                                                     
     7+  DEBUG=True                                                                                                                            
     8+  ALLOWED_HOSTS=localhost,127.0.0.1                                                                                                     
     9+                                                                                                                                        
    10+  # ── Mercado Pago ──────────────────────────────────────────────                                                                      
    11+  # Credenciales de PRUEBA (TEST) desde:                                                                                                
    12+  # https://www.mercadopago.com.co/developers/panel/app -> Credenciales                                                                 
    13+  # Reemplaza los valores de abajo por los TUYOS.                                                                                       
    14+  MERCADOPAGO_ACCESS_TOKEN=TEST-REEMPLAZA-CON-TU-ACCESS-TOKEN                                                                           
    15+  MERCADOPAGO_PUBLIC_KEY=TEST-REEMPLAZA-CON-TU-PUBLIC-KEY                                                                               
    16+                                                                                                                                        
    17+  # Secreto para validar la firma de los webhooks (opcional en desarrollo).                                                             
    18+  # Si lo dejas vacío, se omite la validación de firma (solo para dev).                                                                 
    19+  MERCADOPAGO_WEBHOOK_SECRET=                                                                                                           
    20+                                                                                                                                        
    21+  # URLs base. En local, Mercado Pago no puede llamar a localhost para el webhook;                                                      
    22+  # usa un túnel (ngrok) y pon aquí la URL pública si quieres probar webhooks.                                                          
    23+  BACKEND_PUBLIC_URL=http://localhost:8000                                                                                              
    24+  FRONTEND_URL=http://localhost:3000                                                                                                    

• 3. Reemplazar las credenciales
  
  Abre backend/carnet_project/.env y pega tu Access Token y Public Key reales de MercadoPago (los de prueba TEST-... para empezar),
  reemplazando los placeholders.
  
  4. Poblar los precios de los planes
  
  Esto es crítico y es un segundo requisito que te faltaría aunque arregles el token. Tu SubscribeAPIView busca un PlanPrice activo para el
  plan+ciclo; si no existe, el botón falla con "No existe un precio configurado para ese plan y ciclo". Tienes un comando que los crea (pro =
  178.000/mes, premium = 333.000/mes). Ejecútalo:
  
  cd backend
  python manage.py migrate
  python manage.py seed_plan_prices
  
  5. Reiniciar el servidor Django
  
  load_dotenv solo lee el .env al arrancar, así que reinícialo para que tome el token:
  
  python manage.py runserver
