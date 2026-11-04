# CSA Platform

CSA Platform es una aplicación web desarrollada para CSA Salud Integral SpA que centraliza la gestión de pacientes y conecta sus procesos clínicos, ópticos y comerciales. La solución busca reducir la duplicación de información, evitar retranscripciones manuales y mejorar la trazabilidad entre las distintas áreas de la organización.

> **Estado actual:** CSA Platform se ejecuta en un entorno local de desarrollo. Actualmente no dispone de un entorno de producción ni de una URL pública de despliegue.

## Tecnologías y prerrequisitos

Para instalar y ejecutar el proyecto se utilizan las siguientes tecnologías y herramientas:

### Frontend

- Node.js 22.22.3
- npm 11.17.0 (versión definida por el proyecto)
- Angular 22.1.6
- Angular CLI 22.1.7
- TypeScript 6.0.3
- RxJS 7.8.2
- Vitest 4.1.11

### Backend

- Python 3.12.4
- Django 6.1.1
- Django REST Framework 3.18.1
- djangorestframework-simplejwt 5.5.1
- mysqlclient 2.2.8

### Base de datos

- MySQL 8.4.11
- Motor InnoDB
- Codificación `utf8mb4`

### Otras herramientas y componentes

- Git
- GitHub CLI (`gh`)
- Django Authentication
- JWT/Bearer para autenticación
- RBAC para autorización basada en roles y permisos
- OpenAPI/Swagger para documentación de la API REST

> Docker y Docker Compose no forman parte actualmente del entorno de ejecución del proyecto.

## Instalación

El código fuente de CSA Platform se mantiene en un repositorio privado de GitHub administrado por CSA Salud Integral SpA. Para clonarlo es necesario utilizar una cuenta de GitHub que disponga de acceso autorizado al repositorio.

### 1. Clonar el repositorio

Con GitHub CLI autenticado:

```powershell
gh repo clone csa-salud-integral/CSA-Platform
cd CSA-Platform
```

Si GitHub CLI todavía no se encuentra autenticado en el equipo, se puede iniciar el proceso de autenticación mediante:

```powershell
gh auth login
```

### 2. Crear el entorno virtual de Python

Desde la raíz de `CSA-Platform`:

```powershell
python -m venv .venv
```

Activar el entorno virtual:

```powershell
.\.venv\Scripts\Activate.ps1
```

### 3. Instalar las dependencias del backend

Desde la raíz del proyecto:

```powershell
python -m pip install -r .\backend\requirements.txt
```

Las dependencias Python y sus versiones se encuentran definidas en:

```text
backend/requirements.txt
```

### 4. Instalar las dependencias del frontend

Ingresar al directorio del frontend:

```powershell
cd frontend
```

Instalar las dependencias declaradas en `package-lock.json`:

```powershell
npm ci
```

Regresar a la raíz del proyecto:

```powershell
cd ..
```

## Configuración

CSA Platform obtiene la configuración sensible del backend mediante variables de entorno.

Actualmente el proyecto no utiliza un archivo `.env.example`, por lo que las variables necesarias para el entorno local deben establecerse en la misma sesión de PowerShell desde la que posteriormente se ejecutará Django.

### Variables principales

Ejemplo de configuración para un entorno local:

```powershell
$env:DJANGO_SECRET_KEY="clave_local_de_desarrollo"

$env:DB_NAME="csa_platform_dev"
$env:DB_USER="csa_dev"
$env:DB_PASSWORD="password_mysql_local"
$env:DB_HOST="127.0.0.1"
$env:DB_PORT="3308"
```

Los valores de `DJANGO_SECRET_KEY` y `DB_PASSWORD` del ejemplo son únicamente demostrativos y deben reemplazarse localmente por valores válidos. No se deben almacenar contraseñas, claves secretas, tokens ni otras credenciales reales en el repositorio.

Las variables principales utilizadas para la conexión y ejecución del backend son:

```text
DJANGO_SECRET_KEY
DB_NAME
DB_USER
DB_PASSWORD
DB_HOST
DB_PORT
```

Algunas funcionalidades específicas, como integraciones con servicios de correo, Gmail o almacenamiento, utilizan variables de entorno adicionales que dependen de los servicios autorizados y configurados por CSA Salud Integral SpA.

### Base de datos

La configuración utilizada en el entorno local de desarrollo es:

```text
Motor: MySQL 8.4.11
Base de datos: csa_platform_dev
Host: 127.0.0.1
Puerto: 3308
Usuario de desarrollo: csa_dev
```

Antes de ejecutar el backend debe existir una instancia de MySQL disponible y la base de datos debe encontrarse configurada con las credenciales correspondientes.

### Aplicar migraciones

Con el entorno virtual activado:

```powershell
cd backend
python manage.py migrate
cd ..
```

## Uso

El backend y el frontend deben ejecutarse de manera independiente, utilizando dos terminales.

### Backend

Desde la raíz del proyecto, con las variables de entorno previamente configuradas:

```powershell
.\.venv\Scripts\Activate.ps1
cd backend
python manage.py runserver
```

El servidor Django queda disponible localmente en:

```text
http://127.0.0.1:8000/
```

La API REST utiliza como ruta base:

```text
http://127.0.0.1:8000/api/v1/
```

### Frontend

En una segunda terminal, desde la raíz del proyecto:

```powershell
cd frontend
npm start
```

El script `npm start` ejecuta internamente:

```text
ng serve
```

La aplicación Angular queda disponible localmente en:

```text
http://localhost:4200/
```

### Ejemplo mínimo de funcionamiento

1. Iniciar el backend y el frontend utilizando los comandos anteriores.
2. Abrir `http://localhost:4200/` en un navegador.
3. Iniciar sesión con un usuario autorizado.
4. Buscar un paciente por RUT para acceder a su información y a las funcionalidades habilitadas según los roles y permisos asignados al usuario.

Entre los principales procesos disponibles en CSA Platform se encuentran la gestión de pacientes, atención clínica y Programas de Salud, Salud Visual y Óptica, inventario y productos, ventas y pagos, CRM Comercial, Servicio al Paciente, documentos, auditoría y dashboards operacionales.

## Pruebas

El proyecto incorpora pruebas y validaciones técnicas para backend y frontend.

### Backend

Desde el directorio `backend`:

```powershell
python manage.py check
python manage.py makemigrations --check --dry-run
python manage.py test
```

Estos comandos permiten comprobar la configuración de Django, detectar migraciones pendientes y ejecutar la suite de pruebas automatizadas del backend.

### Frontend

Desde el directorio `frontend`:

```powershell
npm run build
npx tsc --noEmit
npx ng test --watch=false
```

Estos comandos permiten validar la compilación de Angular, comprobar TypeScript y ejecutar la suite de pruebas automatizadas del frontend.

Las pruebas y validaciones técnicas forman parte del proceso utilizado antes de integrar cambios relevantes al proyecto.

## Enfoque de trabajo en equipo

El equipo utiliza **Scrum**, mediante Sprints semanales y entregas incrementales. **Jira** se emplea como herramienta de gestión para organizar el Product Backlog y dar seguimiento al trabajo de cada Sprint.

## Estructura del proyecto

La estructura principal del repositorio es:

```text
CSA-Platform/
├── backend/
│   ├── config/             # Configuración general de Django
│   ├── media/              # Archivos gestionados localmente
│   └── modules/            # Módulos funcionales del backend
│
├── frontend/
│   └── src/
│       └── app/
│           ├── core/       # Servicios y funcionalidades transversales
│           ├── features/   # Módulos funcionales de la aplicación
│           ├── layout/     # Estructura visual general
│           └── shared/     # Componentes y recursos compartidos
│
├── docs/                   # Documentación técnica
├── pruebas-manuales/       # Evidencias y pruebas manuales
├── .gitignore
└── README.md
```

El backend utiliza una arquitectura monolítica modular y agrupa las funcionalidades del sistema en módulos independientes dentro de una misma aplicación y despliegue.

## Autores

- **Leonel González** — Scrum Master + Developer
- **Esteban Toledo** — Developer
- **Jeremy Navarro** — Developer

**Sección:** PTY4614-002D  
**Docente guía:** Mallén González  
**Correo:** elia.gonzalezg@profesor.duoc.cl  
**Contacto alternativo:** mallen@mallen.cl

## Licencia

No aplica.

CSA Platform corresponde a un desarrollo realizado para CSA Salud Integral SpA. Su código fuente se mantiene en un repositorio privado administrado por la organización y no se publica bajo una licencia de código abierto.
