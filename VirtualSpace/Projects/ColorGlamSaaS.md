# Documentación Técnica y Hoja de Ruta — SaaS de Barberías / Peluquerías (MVP)


Este documento detalla la arquitectura, el modelo de datos, la estructura multi-inquilino (*multi-tenant*) y el flujo de implementación paso a paso para el MVP de la plataforma SaaS de gestión de turnos para barberías.

## 1. Visión General del Proyecto

Plataforma SaaS *multi-tenant* diseñada para permitir a dueños de barberías y peluquerías gestionar su personal, agendas y turnos, mientras ofrece a los clientes finales un portal de agendamiento directo rápido y sin fricciones.
## 2. Stack Tecnológico

| Capa | Tecnología Seleccionada | Justificación |

 | ----- | ----- | ----- |

| **Frontend** | Next.js (App Router) + Tailwind CSS + Shadcn UI | Desempeño optimizado para SEO/Web en el portal público y renderizado de componentes interactivos para el dashboard. |

| **Backend & Base de Datos** | Supabase (PostgreSQL + Auth + Storage) | Acelera el desarrollo del MVP integrando autenticación, almacenamiento y motor relacional con Row Level Security (RLS). |

| **Calendario Interactivo** | FullCalendar / React Big Calendar | Visualización multi-columna por estilista y gestión dinámica de bloques de tiempo. |

| **Programación de Tareas** | Upstash QStash / Inngest | Motor de trabajos asíncronos en segundo plano para el envío de recordatorios (Cron Jobs). |

| **Envío de Correos** | Resend API | Infraestructura confiable para correos transaccionales y de notificación. |

## 3. Modelo de Datos (PostgreSQL / Supabase Schema)


```sql

-- 1. Tenants (Sucursales / Barberías)

CREATE TABLE tenants (

    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),

    name VARCHAR(255) NOT NULL,

    slug VARCHAR(100) UNIQUE NOT NULL,

    phone VARCHAR(50),

    address TEXT,

    logo_url TEXT,

    created_at TIMESTAMP WITH TIME ZONE DEFAULT NOW()

);

  

-- 2. Perfiles de Usuarios (Admins y Estilistas)

CREATE TYPE user_role AS ENUM ('admin', 'stylist');

  

CREATE TABLE profiles (

    id UUID PRIMARY KEY REFERENCES auth.users(id) ON DELETE CASCADE,

    tenant_id UUID REFERENCES tenants(id) ON DELETE CASCADE NOT NULL,

    full_name VARCHAR(255) NOT NULL,

    role user_role NOT NULL DEFAULT 'stylist',

    phone VARCHAR(50),

    is_active BOOLEAN DEFAULT TRUE,

    created_at TIMESTAMP WITH TIME ZONE DEFAULT NOW()

);

  

-- 3. Disponibilidad Semanal del Estilista

CREATE TABLE stylist_schedules (

    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),

    tenant_id UUID REFERENCES tenants(id) ON DELETE CASCADE NOT NULL,

    stylist_id UUID REFERENCES profiles(id) ON DELETE CASCADE NOT NULL,

    day_of_week INT NOT NULL CHECK (day_of_week BETWEEN 0 AND 6), -- 0: Domingo, 1: Lunes...

    start_time TIME NOT NULL,

    end_time TIME NOT NULL,

    CONSTRAINT valid_schedule_time CHECK (end_time > start_time)

);

  

-- 4. Servicios

CREATE TABLE services (

    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),

    tenant_id UUID REFERENCES tenants(id) ON DELETE CASCADE NOT NULL,

    name VARCHAR(150) NOT NULL,

    duration_minutes INT NOT NULL, -- Ej: 30, 45, 60

    price DECIMAL(10, 2) NOT NULL,

    is_active BOOLEAN DEFAULT TRUE,

    created_at TIMESTAMP WITH TIME ZONE DEFAULT NOW()

);

  

-- 5. Citas / Turnos

CREATE TYPE appointment_status AS ENUM ('confirmed', 'completed', 'cancelled');

  

CREATE TABLE appointments (

    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),

    tenant_id UUID REFERENCES tenants(id) ON DELETE CASCADE NOT NULL,

    stylist_id UUID REFERENCES profiles(id) ON DELETE CASCADE NOT NULL,

    service_id UUID REFERENCES services(id) ON DELETE CASCADE NOT NULL,

    client_name VARCHAR(255) NOT NULL,

    client_email VARCHAR(255) NOT NULL,

    client_phone VARCHAR(50) NOT NULL,

    start_time TIMESTAMP WITH TIME ZONE NOT NULL,

    end_time TIMESTAMP WITH TIME ZONE NOT NULL,

    status appointment_status DEFAULT 'confirmed',

    notified_24h BOOLEAN DEFAULT FALSE,

    notified_3h BOOLEAN DEFAULT FALSE,

    created_at TIMESTAMP WITH TIME ZONE DEFAULT NOW(),

    CONSTRAINT valid_appointment_time CHECK (end_time > start_time)

);

  

-- Habilitar Row Level Security (RLS)

ALTER TABLE tenants ENABLE ROW LEVEL SECURITY;

ALTER TABLE profiles ENABLE ROW LEVEL SECURITY;

ALTER TABLE services ENABLE ROW LEVEL SECURITY;

ALTER TABLE appointments ENABLE ROW LEVEL SECURITY;

```


## 4. Fases de Desarrollo del MVP

  
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

## 5. Estrategia de Suscripciones y Estructura de Planes SaaS


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

  

## 6. Proyección Futura: Integración de Inteligencia Artificial (IA)


Para versiones posteriores al MVP, se prevé adaptar la arquitectura para soportar capacidades inteligentes que incrementen el valor retenido por el cliente:


1. **Agente/Bot de Agendamiento Conversacional (WhatsApp / Web Widget):**

   * Integración de un LLM capacitado para consultar disponibilidad de la base de datos en tiempo real y agendar turnos respondiendo a lenguaje natural (Ej. *"¿Tienes espacio el jueves en la tarde con Carlos?"*).

  
2. **Optimización Inteligente de Huecos (Smart Scheduling):**

   * Algoritmos para sugerir horarios al cliente final que reduzcan tiempos muertos (*dead time*) entre cortes de cada estilista.


3. **Predicción de Inasistencias (No-Show Prediction):**

   * Análisis de historial de clientes para identificar perfiles con alta probabilidad de cancelación o inasistencia, activando solicitudes de confirmación anticipada.