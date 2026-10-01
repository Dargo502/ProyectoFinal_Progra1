curl -sSL "https://gist.githubusercontent.com/raw/d8e13f412499d63f7db201a43a054db0" -o README.md 2>/dev/null || cat << 'EOF' > README.md
# 🏨 Hotel MVC — Sistema de Gestión Hotelera

Sistema de gestión hotelera desarrollado con arquitectura de dos capas: una API REST construida en ASP.NET Core (.NET 10) como backend, y un frontend MVC separado que consume dicha API. El proyecto permite administrar reservaciones, habitaciones, clientes, empleados y más, a través de endpoints RESTful documentados con Swagger.

---

## 🛠️ Tecnologías Utilizadas

### Backend (API REST)
| Tecnología | Versión | Descripción |
|---|---|---|
| .NET / ASP.NET Core | 10.0 | Framework principal del servidor |
| Entity Framework Core | 10.0.5 | ORM para acceso a base de datos |
| EF Core SqlServer | 10.0.5 | Proveedor para SQL Server |
| Swashbuckle / Swagger | 10.1.7 | Documentación interactiva de la API |
| Microsoft.AspNetCore.OpenApi | 10.0.4 | Soporte OpenAPI |

### Base de Datos
| Tecnología | Descripción |
|---|---|
| SQL Server / SQL Server Express | Motor de base de datos relacional |
| T-SQL | Definición del esquema de tablas y consultas |

---

## 🖥️ Frontend — Servidor de Aplicaciones

Este repositorio incluye dos implementaciones del frontend, ambas consumen la misma API REST y reflejan la lógica del modelo relacional de la base de datos.

### 🌐 Frontend SPA (Single Page Application)
> Ubicación: API REST CONFIGURACION/wwwroot/

Interfaz de usuario construida con HTML5, CSS3 y JavaScript puro, servida directamente desde el proyecto de la API mediante UseStaticFiles().

Características:
- Inicio de sesión con control de roles (Administrador, Supervisor, Recepcionista)
- Sidebar dinámico con navegación entre módulos sin recarga de página
- CRUD completo para: Hoteles, Habitaciones, Tipos de Habitación, Clientes, Empleados, Reservaciones y Usuarios
- Listas desplegables que resuelven las llaves foráneas (FK) del modelo relacional en tiempo real
- Estado de sesión gestionado en sessionStorage

---

### 🏗️ Frontend MVC (Model–View–Controller)
> Ubicación: HotelMVC/

Proyecto independiente de ASP.NET Core 8.0 con arquitectura MVC clásica usando Razor Pages.

Características:
- Autenticación con sesiones HTTP de ASP.NET (ISession)
- Vistas Razor (.cshtml) con Tag Helpers para formularios tipados
- CRUD completo para los 7 módulos del sistema
- Redirección automática al login si no hay sesión activa

---

## ⚖️ SPA vs MVC — Diferencias Clave

| Característica | SPA | MVC |
|---|---|---|
| HTML generado en | Navegador (JS) | Servidor (C# + Razor) |
| Navegación | Sin recarga de página | Recarga completa |
| Estado del usuario | sessionStorage | Sesión HTTP de ASP.NET |
| Vistas | index.html + app.js | Archivos .cshtml por módulo |
| Comunicación con API | fetch() desde el navegador | HttpClient desde C# |

---

## 🗄️ Modelo de Base de Datos

El sistema maneja las siguientes entidades relacionales:

Hotel ------< Habitacion >------ TipoHabitacion
  │                 │
  │                 └──────< DetalleReservacion
  │                                   │
  └──────< Reservacion >──────────┘
              │
        ┌─────┴─────┐
      Cliente     Empleado
                    │
                 Usuario

Tablas principales:
- Hotel — Información del establecimiento (nombre, dirección, teléfono)
- TipoHabitacion — Categorías de habitación con precio por noche
- Habitacion — Habitaciones con estado (disponible/ocupada) vinculadas a hotel y tipo
- Cliente — Datos personales (nombre, apellido, DPI, correo, teléfono)
- Empleado — Personal del hotel (nombre, puesto, teléfono)
- Usuario — Credenciales de acceso vinculadas a empleado, incluye campo rol
- Reservacion — Registro de reservas con fechas, total y relaciones a cliente/empleado/hotel
- DetalleReservacion — Desglose por habitación (días, subtotal)

---

## 🔌 Endpoints de la API

Todos los endpoints siguen la convención REST /api/[entidad] y soportan operaciones CRUD completas:

| Método | Ruta | Descripción |
|--------|------|-------------|
| GET | /api/Hotel | Listar hoteles |
| GET | /api/Cliente | Listar clientes |
| GET | /api/Habitacion | Listar habitaciones |
| GET | /api/TipoHabitacion | Listar tipos de habitación |
| GET | /api/Empleado | Listar empleados |
| GET | /api/Usuario | Listar usuarios |
| GET | /api/Reservacion | Listar reservaciones |
| GET | /api/DetalleReservacion | Listar detalles de reservación |
| POST | /api/[entidad] | Crear nuevo registro |
| PUT | /api/[entidad]/{id} | Actualizar registro existente |
| DELETE | /api/[entidad]/{id} | Eliminar registro |

La documentación interactiva completa está disponible en /swagger al ejecutar el proyecto.

---

## ⚙️ Configuración e Instalación

### 1. Clonar el repositorio
git clone https://github.com/Dargo502/ProyectoFinal_Progra1.git
cd ProyectoFinal_Progra1

### 2. Configurar la base de datos
Aplica las migraciones de Entity Framework Core para crear la base de datos HotelDB:

cd "API_HOTEL_MAIN/API REST CONFIGURACION"
dotnet ef database update

### 3. Configurar la cadena de conexión
Edita el archivo appsettings.json en el proyecto de la API con tu instancia local de SQL Server:

{
  "ConnectionStrings": {
    "DefaultConnection": "Server=localhost\\SQLEXPRESS;Database=HotelDB;Trusted_Connection=True;TrustServerCertificate=True;"
  }
}

### 4. Ejecutar la API
dotnet run

La API estará disponible en https://localhost:{puerto} y Swagger UI en /swagger.

---

## 👥 Autores y Créditos Académicos

Este proyecto fue desarrollado como parte del curso de Programación I en la Universidad Mariano Gálvez de Guatemala (UMG).

* Darío Alfredo Rabé Godoy — Carné: 5190-25-23683
* Libbny Dayana Medrano Arévalo — Carné: 5190-25-24096
* Richard Esteev Pernillo Macario — Carné: 5190-25-21234
* Cristian Daniel Emiliano Cano Estrada — Carné: 5190-25-24608
* Diego José Quevedo Vega — Carné: 5190-24-21422
EOF
