#Learning #CI/CD
## Proceso de Publicación Selectiva con Validación de QA y Aprobación del PM

---
### 1. Objetivo

Evitar que se publiquen en producción cambios no aprobados, mediante un proceso estructurado y gobernado por el equipo de DevOps, con participación activa de QA y el PM. Esta propuesta surge como respuesta a incidentes en los que se desplegó contenido no validado, debido a la falta de control en el manejo de la rama develop.

---
### 2. Problema Identificado

Se estaba realizando un merge completo de la rama develop hacia main, lo que provocaba que se liberaran todos los features. Algunos de ellos sin ser aprobados por el PM. Esto ocasionaba que contenido no aprobado quedara expuesto en el entorno productivo.

---
### 3. Roles Involucrados

|   |   |
|---|---|
|Rol|Función|
|Dev A / Dev B|Desarrollan funcionalidades y suben ramas feature/*|
|PM|Verifica y aprueba formalmente qué funcionalidades pueden publicarse|
|DevOps|Ejecuta la integración, prepara los PRs y realiza el despliegue|

---
### 4. Ejemplo de Caso Real

#### Situación

- Dev A desarrolla la funcionalidad de autenticación en la rama feature/login.    

- Dev B desarrolla una pieza de contenido promocional en la rama feature/promo-ciencia.  

- Ambas ramas son integradas a develop por DevOps para su validación.  

#### Validación

- El PM valida ambas funcionalidades pero aprueba únicamente la funcionalidad de login.    

- La funcionalidad promocional no es aprobada en esta iteración.  
#### Error

DevOps realiza un merge de develop hacia main y se publica el contenido de ambas ramas.  
Al estar conectada la rama main a AWS Amplify, el contenido se despliega automáticamente.  
Como resultado, se publica en producción contenido no aprobado por el PM.

---
### 5. Solución Propuesta: Despliegue Selectivo mediante Cherry-pick

Cuando una o más funcionalidades integradas a develop no están aprobadas por el PM, DevOps evitará hacer merge completo de develop hacia main. En su lugar, creará una rama release/* desde main y aplicará cherry-pick únicamente a los commits aprobados.

---

### 6. Procedimiento Técnico Paso a Paso

#### 6.1. Confirmación de Aprobación

DevOps debe recibir confirmación oficial del PM o, en su defecto, del desarrollador responsable, indicando qué funcionalidades están aprobadas para ser liberadas.  
Esta validación debe estar documentada en un formato sencillo (tabla, checklist o en la misma incidencia) agregando el hash de los commits.

|                       |       |          |
| --------------------- | ----- | -------- |
| Feature               | PM OK | Publicar |
| feature/login         | Sí    | Sí       |
| feature/promo-ciencia | No    | No       |

---

#### 6.2. Identificación de Commits Aprobados

Desde la rama develop, DevOps identifica los commits que corresponden a la funcionalidad aprobada:

git checkout develop

git log --oneline

  

Ejemplo:

abc123 feat(login): validación de formulario  

def456 feat(login): integración backend  

ghi789 feat(promo): pieza promocional

  

Solo los commits abc123 y def456 deben ser considerados para publicación.

---
#### 6.3. Creación de Rama de Publicación

DevOps crea una rama desde main con el siguiente comando:

git checkout main

git pull origin main

git checkout -b release/login

---
#### 6.4. Aplicación de Cherry-pick

Se aplican únicamente los commits aprobados:

git cherry-pick abc123 def456

En caso de conflictos:

# Resolver manualmente

git add .

git cherry-pick --continue

---
#### 6.5. Push y Creación del Pull Request

DevOps realiza el push de la rama:

```bash
git push origin release/login
```

Y luego crea un Pull Request desde release/login hacia main.

Descripción del PR:

Este Pull Request contiene únicamente los commits relacionados con la funcionalidad de login, los cuales fueron validados por QA y aprobados por el PM.

Funcionalidades incluidas:

- feature/login  

Commits:

- abc123  

- def456    

Funcionalidades excluidas:

- feature/promo-ciencia (no aprobada)  

---
#### 6.6. Merge y Despliegue

Una vez aprobado el PR por los revisores asignados, DevOps realiza el merge:

git checkout main

git merge release/login

git push origin main

Al estar conectada la rama main a AWS Amplify, el contenido se despliega automáticamente a producción.

---
### 7. Recomendaciones para el Desarrollador

#### 7.1. Trabajar siempre sobre ramas feature/* individuales

Cada funcionalidad o requerimiento debe ser desarrollado en una rama independiente con nombre claro y descriptivo:

feature/login-form  

feature/promo-ciencia  

feature/ajuste-footer

Esto facilita la identificación de cada feature, el cherry-pick selectivo, y el control de versiones en producción.

---
#### 7.2. Nunca hacer merge directo a develop ni a main

Los desarrolladores no deben hacer merges ni Pull Requests hacia develop o main.  
Estas acciones deben ser realizadas exclusivamente por el rol de DevOps, quien administra la integración y el despliegue con base en las aprobaciones formales.

---
#### 7.3. Comunicar claramente cuándo una funcionalidad está lista para validación

Una vez terminada la funcionalidad en la rama feature/*, el desarrollador debe informar (por ticket, herramienta de gestión o mensaje directo) que la rama está lista para revisión técnica y validación funcional.

Debe incluir:

- Descripción de la funcionalidad  
  
- Alcance de los cambios  
  
- Posibles dependencias o componentes afectados  
  
---
#### 7.4. Documentar correctamente los commits

Usar mensajes de commit descriptivos, claros y coherentes. Por ejemplo:

`feat(login)`: validación de credenciales en frontend  

`fix(login)`: error en conexión con API


Esto facilita que DevOps pueda identificar fácilmente los commits que deben ser cherry-pickeados para el despliegue.

---
### 8. Recomendaciones para el PM

Para evitar retrasos innecesarios y errores en el despliegue, el PM debe:

- Revisar con prontitud las funcionalidades cargadas a develop.  

- Confirmar formalmente qué features están autorizadas para publicación.  

- Proporcionar visibilidad clara sobre el estado de aprobación.  

Un proceso lento de aprobación puede provocar bloqueos innecesarios o presiones para publicar cambios no validados completamente.

---
### 9. Conclusión

Este procedimiento permite a DevOps mantener el control sobre los despliegues, evitar errores de publicación y garantizar que solamente se libere en producción el código validado y aprobado por el PM. Se recomienda aplicar este flujo de manera estricta para cada despliegue y reforzar la comunicación entre todos los roles involucrados.
