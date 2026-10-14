# Documentación Técnica y Hoja de Ruta — SaaS de Barberías / Peluquerías (MVP)


Este documento detalla la arquitectura, el modelo de datos, la estructura multi-inquilino (*multi-tenant*) y el flujo de implementación paso a paso para el MVP de la plataforma SaaS de gestión de turnos para barberías.

## 1. Visión General del Proyecto

Plataforma SaaS *multi-tenant* diseñada para permitir a dueños de barberías y peluquerías gestionar su personal, agendas y turnos, mientras ofrece a los clientes finales un portal de agendamiento directo rápido y sin fricciones.
## 2. Stack Tecnológico

| Capa | Tecnología Seleccionada | Justificación |

 | ----- | ----- | ----- |

| **Frontend** | Next.js (App Router) + Tailwind CSS + Shadcn UI | Desempeño optimizado para SEO/Web en el portal público y renderizado de componentes interactivos para el dashboard. |

| **Backend & Base de Datos** | Django (PostgreSQL + Auth + Storage) | Acelera el desarrollo del MVP integrando autenticación, almacenamiento y motor relacional con Row Level Security (RLS). |

| **Calendario Interactivo** | FullCalendar / React Big Calendar | Visualización multi-columna por estilista y gestión dinámica de bloques de tiempo. |

| **Programación de Tareas** | Upstash QStash / Inngest | Motor de trabajos asíncronos en segundo plano para el envío de recordatorios (Cron Jobs). |

| **Envío de Correos** | Resend API | Infraestructura confiable para correos transaccionales y de notificación. |


## 3. Fases de Desarrollo del MVP

  
```

+-------------------------------------------------------------------+

|                            FASE 1                                 |

|          Autenticación, Tenants & Onboarding Multi-paso           |

+-------------------------------------------------------------------+

                                 |

                                 v

+-------------------------------------------------------------------+

|                            FASE 2                                 |

|            Calendario, Conflictos & Agendamiento Público          |

+-------------------------------------------------------------------+

                                 |

                                 v

+-------------------------------------------------------------------+

|                            FASE 3                                 |

|                 Notificaciones & Tareas Cron                      |

+-------------------------------------------------------------------+

```


### FASE 1: Autenticación & Onboarding Multi-paso (Wizard de Registro Trial)


El proceso de registro inicial para activar el período de prueba (*Trial*) utiliza un stepper wizard de 3 pasos basado en la siguiente estructura:
  

```

[ PASO 1: Datos de la Peluquería ] ----> [ PASO 2: Configuración de Sucursales ] ----> [ PASO 3: Servicios & Peluqueros ]

 (Información General & Cuenta Admin)          (Ubicación, Dirección & Contacto)          (Servicios, Precios & Estilistas)

```

#### Detalle de los 3 Pasos del Formulario:


1. **Paso 1: Datos de la Peluquería (Negocio & Cuenta)**

   * Nombre comercial de la marca / peluquería.

   * Nombre del Administrador/Dueño, Correo Electrónico y Contraseña.

   * Razón Social o Documento Fiscal (opcional).

  
2. **Paso 2: Configuración de Sucursal / Sede**

   * Nombre de la primera sede (Ej. "Sede Principal", "Sucursal Poblado").

   * Dirección física, teléfono de contacto y ciudad.

   * Generación automática del `slug` único para el portal público de reserva.

  
3. **Paso 3: Servicios, Peluqueros y Precios (Cierre del Onboarding)**

   * **Servicios Base:** Formulario ágil para registrar al menos 1 servicio inicial (Nombre, Duración en minutos y Precio).

   * **Equipo de Peluqueros:** Registro de estilistas iniciales (hasta 3 peluqueros para el plan Starter) y asignación de sus horarios y días de atención.

  
---

### FASE 2: Calendario y Portal de Reserva Pública

  
#### 1. Panel Privado (Dueño y Estilistas):

* **Vista Calendario:** Visualización multi-columna (Grid por estilista).

* **Bloques y Conflictos:**

  * Control de solapamiento mediante validación previa en Backend/Database antes de insertar en `appointments`.

* **Creación Manual:** Permite al administrador o estilista registrar turnos recibidos vía llamada o presenciales.

#### 2. Portal Público de Agendamiento (`/reserva/[slug-barberia]`):

* **Sin Registro Obligatorio:** El cliente final no crea cuenta.

* **Pasos de Reserva:**

  1. Selección de Servicio.

  2. Selección de Estilista (o asignación automática del primero disponible).

  3. Selección de Fecha y Slot de Hora (calculado en tiempo real evaluando la duración del servicio frente a los turnos ocupados y el horario laboral del estilista).

  4. Formulario de Datos Básicos (`Nombre`, `Correo`, `Teléfono`).

  5. Confirmación en pantalla.

  
---

### FASE 3: Sistema de Notificaciones Automatizadas

  
#### 1. Confirmación Inmediata:

* Disparo de evento vía Webhook o Supabase Function al crear la cita.

* Envío de correo electrónico al cliente con los detalles de la reserva.

  
#### 2. Recordatorios Automatizados (Cron Job):

* **Frecuencias de disparo:** 24 horas antes y 3 horas antes del `start_time`.

* **Lógica del Job (Ejecutado cada 15 minutos vía QStash/Inngest):**

  1. Busca citas en estado `confirmed`.

  2. Verifica citas cuyo `start_time` esté dentro de las ventanas temporales objetivo.

  3. Ejecuta el envío de correo (vía Resend) y actualiza `notified_24h = true` o `notified_3h = true` para evitar duplicados.


---

## 4. Estrategia de Suscripciones y Estructura de Planes SaaS


```

+-----------------------------------------------------------------------------------+

|                              MATRIZ DE PLANES SAAS                                |

+-----------------------------------------------------------------------------------+

|   STARTER (MVP)         |            PRO            |          PREMIUM            |

|-------------------------|---------------------------|-----------------------------|

| • 1 Sucursal            | • 2 Sucursales            | • Hasta 5 Sucursales        |

| • 1 Admin               | • 6 Estilistas (Doble)    | • Estilistas ILIMITADOS     |

| • 3 Estilistas          | • Recordatorios Email/WA  | • Reportes Avanzados        |

| • Agendamiento Web      | • Métricas de Ocupación   | • Soporte Prioritario       |

+-----------------------------------------------------------------------------------+

```

  

### Detalle de Capacidades por Plan

  

* **Starter (Entrada / MVP):**

  * **Sucursales:** 1

  * **Equipo:** 1 Administrador + 3 Estilistas / Barberos.

  * **Funcionalidades:** Onboarding asistido en 3 pasos, portal de agendamiento público, gestión de turnos manuales y recordatorios automatizados básicos.


* **Pro:**

  * **Sucursales:** Hasta 2 sucursales.

  * **Equipo:** 6 Estilistas (el doble de capacidad respecto a Starter).

  * **Funcionalidades:** Todo lo de Starter + panel de métricas de ocupación por barbero y recordatorios multicanal.

  

* **Premium:**

  * **Sucursales:** Hasta 5 sucursales.

  * **Equipo:** Estilistas **ilimitados**.

  * **Funcionalidades:** Todo lo de Pro + gestión multi-sucursal avanzada y soporte prioritario.

  

---

  

## 5. Proyección Futura: Integración de Inteligencia Artificial (IA)


Para versiones posteriores al MVP, se prevé adaptar la arquitectura para soportar capacidades inteligentes que incrementen el valor retenido por el cliente:


1. **Agente/Bot de Agendamiento Conversacional (WhatsApp / Web Widget):**

   * Integración de un LLM capacitado para consultar disponibilidad de la base de datos en tiempo real y agendar turnos respondiendo a lenguaje natural (Ej. *"¿Tienes espacio el jueves en la tarde con Carlos?"*).

  
2. **Optimización Inteligente de Huecos (Smart Scheduling):**

   * Algoritmos para sugerir horarios al cliente final que reduzcan tiempos muertos (*dead time*) entre cortes de cada estilista.


3. **Predicción de Inasistencias (No-Show Prediction):**

   * Análisis de historial de clientes para identificar perfiles con alta probabilidad de cancelación o inasistencia, activando solicitudes de confirmación anticipada.

## 6. Estructura de Planes Sugerida

| **Característica / Módulo**   | **BASIC (Ideal para independientes o salones pequeños)** | **PRO (Salones en crecimiento)**                     | **PREMIUM (Cadenas o salones consolidados)**                       |
| ----------------------------- | -------------------------------------------------------- | ---------------------------------------------------- | ------------------------------------------------------------------ |
| **Sedes**                     | 1 sede                                                   | 2 sedes                                              | Hasta 5 sedes (extensible)                                         |
| **Empleados**                 | 5 (incluye Owner)                                        | 10 empleados                                         | 20 empleados / Ilimitados                                          |
| **Servicios**                 | 10 servicios                                             | 30 servicios                                         | Ilimitados                                                         |
| **Agenda y Citas Online**     | Incluido                                                 | Incluido                                             | Incluido                                                           |
| **Recordatorios de Citas**    | Solo Email                                               | WhatsApp Estándar (Plantilla/Plantillas programadas) | WhatsApp con IA interactiva                                        |
| **CRM / Ficha de Clientes**   | Datos básicos                                            | Historial de servicios + Notas                       | Ficha completa + Fotos Antes/Después + Fórmulas de Tinte           |
| **Módulo de Productos / POS** | ❌ No incluido                                            | Venta básica de productos + Control de stock         | Venta + Inventario de uso interno + Comisiones por venta           |
| **Finanzas y Caja**           | Registro simple de ingresos                              | Cierre de caja diario + Reportes básicos             | Métricas avanzadas, comisión por estilista y cobro de señas online |
| **Agendamiento con IA**       | ❌ No incluido                                            | ❌ No incluido                                        | **Agente IA en WhatsApp / Web (Agendamiento 24/7)**                |
| **Fidelización y Marketing**  | ❌ No incluido                                            | Mensajes de Cumpleaños                               | Programa de Puntos + Campañas masivas de promoción                 |
## Hoja de ruta recomendada

Para no abrumarte construyendo todo al mismo tiempo, implementa estas funciones de manera progresiva según el impacto en tus clientes:

1. **Fase 1: Punto de Venta (POS) e Inventario Básico**
    
    - _Por qué:_ Es una necesidad inmediata para los salones (muchos ganan entre el 20% y 30% vendiendo champús, ceras, etc.). Agrega valor directo al plan **Pro**.
        
2. **Fase 2: Pagos Online con Señas y Cierre de Caja**
    
    - _Por qué:_ Resuelve el problema número 1 de las peluquerías: el cliente que reserva y no llega (_no-show_).
        
3. **Fase 3: Integración de IA para Agendamiento**
    
    - _Por qué:_ Es el gran diferenciador de venta para el plan **Premium**. Le permite al dueño del salón delegar completamente la atención al cliente por WhatsApp.