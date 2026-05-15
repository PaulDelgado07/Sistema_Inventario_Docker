# Proyecto: Sistema de Inventario Distribuido con Docker y Tailscale

## Modalidad

**Parejas (2 estudiantes)**

---

# Descripción General

En este proyecto se desarrollará un sistema de inventario de productos distribuido en contenedores Docker, donde cada componente se ejecutará en máquinas físicas diferentes y se comunicarán entre sí mediante una red privada virtual utilizando Tailscale.

El objetivo principal es aplicar de forma práctica conceptos como:

- Contenerización con Docker
- Comunicación entre servicios distribuidos
- Persistencia de datos
- Redes privadas virtuales
- Arquitectura distribuida

---

# Contexto del Sistema

Una empresa pequeña necesita un sistema básico para gestionar su inventario de productos.

El sistema estará compuesto por tres servicios independientes desplegados en contenedores:

1. Aplicación de gestión de inventario
2. Base de datos
3. Módulo de reportes

Cada servicio podrá ejecutarse en máquinas diferentes o en una misma máquina, dependiendo del rol asignado a cada estudiante. Todos los servicios se comunicarán mediante la red Tailscale.

---

# Arquitectura del Sistema

```text
[ Máquina — Estudiante 1 ]                 [ Máquina — Estudiante 2 ]

┌──────────────────────────┐               ┌──────────────────────────────────────┐
│  Contenedor              │               │  Contenedor A                        │
│  App Python - Inventario │◄─ Tailscale ─►│  Base de Datos                       │
│  (API REST con Flask     │               │  (PostgreSQL, MySQL o MongoDB)      │
│   o FastAPI)             │               ├──────────────────────────────────────┤
└──────────────────────────┘               │  Contenedor B                        │
                                           │  Mini Sistema de Reportes            │
                                           │  (Flask, FastAPI o script Python)    │
                                           └──────────────────────────────────────┘
                                                  ▲
                                           Red interna Docker
                                           (los dos contenedores
                                            se ven entre sí)
```

---

# Responsabilidades por Estudiante

## Estudiante 1 — Aplicación de Inventario (Python)

Desarrollará una API REST en Python usando Flask o FastAPI, ejecutada dentro de un contenedor Docker.

La aplicación deberá conectarse a la base de datos que corre en la máquina del Estudiante 2 utilizando la dirección IP proporcionada por Tailscale.

### Funcionalidades mínimas requeridas

- Registrar un nuevo producto:
  - nombre
  - descripción
  - cantidad
  - precio

- Actualizar el stock de un producto existente
- Eliminar un producto
- Listar todos los productos
- Consultar un producto por ID o nombre

### Requisitos técnicos

- La aplicación debe leer:
  - Dirección IP de la base de datos
  - Credenciales de acceso

  desde variables de entorno definidas en un archivo `.env`

- Debe incluir su propio `Dockerfile`
- El contenedor debe exponer un puerto accesible desde la red Tailscale

---

## Estudiante 2 — Base de Datos y Sistema de Reportes

Administrará dos contenedores Docker conectados mediante una red Docker interna personalizada.

---

# Contenedor A — Base de Datos

El motor de base de datos queda a elección del equipo:

- PostgreSQL
- MySQL
- MongoDB

La elección debe justificarse en el documento de entrega.

## Requisitos

- Inicializar la base de datos mediante un script que cree la tabla o colección de productos con los siguientes campos mínimos:

| Campo       | Descripción                |
| ----------- | -------------------------- |
| id          | Identificador del producto |
| nombre      | Nombre del producto        |
| descripción | Descripción del producto   |
| cantidad    | Cantidad en stock          |
| precio      | Precio unitario            |

- Los datos deben persistir utilizando un volumen Docker
- La información no debe perderse al reiniciar el contenedor
- El puerto de la base de datos debe estar accesible desde la red Tailscale

---

# Contenedor B — Mini Sistema de Reportes

Desarrollará un servicio independiente conectado a la base de datos mediante la red interna Docker utilizando el nombre del servicio y no la IP.

## Reportes mínimos requeridos

### 1. Productos con bajo stock

Listado de productos con menos de 5 unidades disponibles.

### 2. Productos con mayor valor en inventario

Mostrar los 5 productos con mayor valor total:

```text
cantidad × precio
```

### 3. Resumen general del inventario

Debe incluir:

- Total de productos registrados
- Valor total del inventario

---

# Formato de Presentación de Reportes.

Los reportes pueden mostrarse como:

- Interfaz web simple
- Endpoints JSON
- Archivos exportados en formato CSV

> Mientras mejor sea la presentación, mayor puntuación podrá obtener el proyecto.
MARLON OMAR SALAZAR ALVARADO
