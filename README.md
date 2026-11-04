# CSA Platform

CSA Platform es una aplicación web desarrollada para CSA Salud Integral SpA con el objetivo de centralizar y conectar sus procesos clínicos, ópticos y comerciales. La plataforma permite gestionar información de pacientes bajo una identidad única, mantener trazabilidad entre módulos y reducir la duplicación y retranscripción manual de información.

El proyecto corresponde al Producto Mínimo Viable (MVP) desarrollado en el contexto del proyecto Capstone y de la práctica profesional realizada en CSA Salud Integral SpA.

> **Estado actual:** CSA Platform se ejecuta actualmente en un entorno local de desarrollo. No dispone de un entorno de producción ni de una URL pública documentada.

## Tecnologías y prerrequisitos

Para ejecutar el proyecto se utilizan las siguientes tecnologías:

### Frontend

- Node.js 22.22.3
- npm 11.17.0
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
- Motor relacional MySQL/InnoDB
- Codificación `utf8mb4`

### Otras herramientas y componentes

- Git
- JWT/Bearer para autenticación
- RBAC para autorización basada en roles y permisos
- OpenAPI/Swagger para documentación de la API REST

Docker y Docker Compose no forman parte actualmente del entorno de ejecución del proyecto.

## Instalación

El código fuente principal de CSA Platform se mantiene en un repositorio privado administrado por CSA Salud Integral SpA. Por lo tanto, su acceso requiere autorización de la empresa.

Una vez obtenida una copia autorizada del proyecto, ubicarse en la raíz de `CSA-Platform`.

### 1. Crear el entorno virtual de Python

Desde PowerShell:

```powershell
python -m venv .venv
```

Activar el entorno virtual:

```powershell
.\.venv\Scripts\Activate.ps1
```

### 2. Instalar las dependencias del backend

Desde la raíz del proyecto:

```powershell
python -m pip install -r .\backend\requirements.txt
```

Las dependencias Python y sus versiones se encuentran definidas en:

```text
backend/requirements.txt
```

### 3. Instalar las dependencias del frontend

Ingresar al directorio del frontend:

```powershell
cd frontend
```

Instalar las dependencias declaradas en `package-lock.json`:

```powershell
npm ci
```

Volver posteriormente a la raíz cuando sea necesario:

```powershell
cd ..
```

## Configuración

CSA Platform obtiene la configuración sensible del backend mediante variables de entorno.

Actualmente el proyecto no utiliza un archivo `.env.example`, por lo que las variables necesarias para el entorno local deben establecerse en la misma sesión de PowerShell desde la que se ejecutará Django.

### Variables principales

Ejemplo de configuración local:

```powershell
$env:DJANGO_SECRET_KEY="TU_CLAVE_LOCAL_DE_DESARROLLO"

$env:DB_NAME="csa_platform_dev"
$env:DB_USER="csa_dev"
$env:DB_PASSWORD="TU_PASSWORD_LOCAL_DE_MYSQL"
$env:DB_HOST="127.0.0.1"
$env:DB_PORT="3308"
```

No se deben almacenar contraseñas, claves secretas, tokens ni credenciales reales en el repositorio.

Las variables principales utilizadas para la conexión y ejecución del backend son:

```text
DJANGO_SECRET_KEY
DB_NAME
DB_USER
DB_PASSWORD
DB_HOST
DB_PORT
```

Determinadas funcionalidades de integración, como servicios de correo, Gmail o almacenamiento, utilizan variables de entorno adicionales. Estas configuraciones dependen del entorno y de los servicios autorizados por CSA Salud Integral SpA.

### Base de datos

La configuración local utilizada durante el desarrollo es:

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
```

## Uso

El frontend y el backend deben ejecutarse de forma independiente, idealmente utilizando dos terminales.

### Backend

Desde la raíz del proyecto:

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

En otra terminal:

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

### Flujo básico de funcionamiento

Una vez que backend y frontend se encuentran ejecutándose, el usuario puede acceder a la aplicación web e iniciar sesión según los roles y permisos configurados.

Entre los principales flujos del sistema se encuentran:

- gestión unificada de pacientes mediante RUT;
- Programas de Salud y atención clínica;
- registro de información y documentos clínicos;
- Salud Visual y generación de receta óptica;
- continuidad del proceso hacia Óptica;
- productos, inventario y Órdenes de Trabajo;
- ventas y pagos;
- Servicio al Paciente;
- CRM Comercial;
- dashboards e indicadores operacionales.

## Pruebas

El proyecto incorpora pruebas y validaciones técnicas tanto para backend como para frontend.

### Backend

Desde el directorio `backend`:

```powershell
python manage.py check
python manage.py makemigrations --check --dry-run
python manage.py test
```

Estos comandos permiten verificar la configuración de Django, detectar migraciones pendientes y ejecutar la suite de pruebas del backend.

### Frontend

Desde el directorio `frontend`:

```powershell
npm run build
npx tsc --noEmit
npx ng test --watch=false
```

Estos comandos permiten validar la compilación del proyecto Angular, comprobar TypeScript y ejecutar la suite de pruebas del frontend.

Las pruebas forman parte del proceso de validación de los incrementos desarrollados y se ejecutan antes de integrar cambios relevantes al proyecto.

## Enfoque de trabajo en equipo

El proyecto utiliza **Scrum** como marco de trabajo, mediante Sprints semanales y entregas funcionales incrementales.

**Jira** se utiliza como herramienta de gestión para administrar el Product Backlog, Sprint Backlog, prioridades, Sprint Goals y seguimiento de las historias de usuario.

## Estructura del proyecto

La estructura principal del proyecto es la siguiente:

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

El backend se encuentra organizado mediante una arquitectura monolítica modular. Entre los módulos principales se encuentran Clínica, Pacientes, Óptica, Inventario, CRM, Operaciones, Servicio al Paciente, Ventas/Pagos, Reporting, Documentos, Auditoría e Integraciones.

## Autores

- **Leonel González** - Scrum Master + Developer
- **Esteban Toledo** - Developer
- **Jeremy Navarro** - Developer

**Sección:** PTY4614-002D  
**Docente guía:** Mallén González  
**Correo:** elia.gonzalezg@profesor.duoc.cl  
**Contacto alternativo:** mallen@mallen.cl

## Licencia

No aplica.

CSA Platform corresponde a un desarrollo realizado para CSA Salud Integral SpA y su código fuente principal se mantiene en un repositorio privado administrado por la empresa. No se declara una licencia de código abierto para su distribución pública.
