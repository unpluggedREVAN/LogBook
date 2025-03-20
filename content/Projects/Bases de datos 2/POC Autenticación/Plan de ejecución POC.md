# ✅ Planificación Integral Extendida para POC de Autenticación con JWT, Node.js, SQLite y Docker

---

## 🎯 **Objetivo del POC**

Implementar una **API RESTful** para autenticación segura utilizando **JSON Web Tokens (JWT)** como método de autenticación sin estado (**stateless**), persistiendo los usuarios en **SQLite**, y contenerizada completamente con **Docker** para garantizar portabilidad, replicabilidad y facilidad de despliegue.

---

## 🔹 ¿Por qué esta arquitectura?

- **Node.js + Express:** Framework minimalista, rápido, fácil de comprender e ideal para prototipos.
- **SQLite:** Base de datos ligera y embebida. No requiere configuración de servidor y es perfecta para POC.
- **JWT:** Permite autenticación **sin necesidad de gestionar sesiones** en el servidor, con tokens firmados y autocontenidos.
- **Docker:** Garantiza que cualquier persona pueda ejecutar el proyecto de manera idéntica sin importar su sistema operativo o configuración local.
- **dotenv:** Permite la gestión de claves y configuraciones sensibles sin hardcodear datos críticos.

---

## 🔹 Explicación técnica de cada componente

|Componente|Explicación detallada|
|---|---|
|**Dockerfile**|Define el entorno donde correrá la aplicación, instalando dependencias y exponiendo puertos necesarios.|
|**docker-compose.yml**|Orquesta los servicios, exponiendo el puerto 3000 y cargando variables de entorno.|
|**SQLite**|Proporciona almacenamiento persistente para usuarios sin necesidad de un SGBD externo ni configuración previa.|
|**JWT**|Genera tokens firmados (con expiración) que representan sesiones, sin almacenar estado en el servidor.|
|**bcryptjs**|Hashea contraseñas antes de almacenarlas para evitar que queden expuestas en caso de una filtración.|
|**dotenv**|Maneja claves sensibles como el secreto del JWT sin dejarlas expuestas en el código fuente.|
|**Postman**|Herramienta para probar fácilmente las rutas de la API, visualizar respuestas y simular ataques básicos.|

---

## 🔹 Arquitectura general del flujo

```plaintext
[Cliente (Postman)] ---> POST /register ---> [Servidor Node.js]
                                          |
                                          ---> Hashea contraseña y guarda en SQLite
                                          
[Cliente] ---> POST /login ---> Verifica credenciales
                          |
                          ---> Genera JWT firmado y lo retorna

[Cliente] ---> GET /protected ---> Verifica JWT en headers
                             |
                             ---> Responde con contenido protegido si JWT válido
```

---

## 🔹 Estructura extendida del proyecto

```
/poc-jwt/
│
├── src/
│   ├── controllers/
│   │   └── authController.js   # Lógica de registro y login
│   ├── models/
│   │   └── userModel.js        # Interacción con la base SQLite
│   ├── database/
│   │   └── db.js               # Configuración de la conexión a SQLite
│   ├── middlewares/
│   │   └── authMiddleware.js   # Verificación del JWT en rutas protegidas
│   ├── app.js                  # Definición de rutas
│   └── server.js               # Arranque de la aplicación
├── Dockerfile                  # Configuración de la imagen
├── docker-compose.yml          # Orquestación y entorno
├── .env                        # Claves sensibles
├── package.json                # Dependencias
├── package-lock.json
├── README.md                   # Instrucciones detalladas
└── video_demostrativo.mp4
```

---

## 🔹 Configuración detallada

### 🔹 Dockerfile explicado

```dockerfile
FROM node:20-alpine
WORKDIR /app
COPY package.json package-lock.json ./
RUN npm install
COPY . .
EXPOSE 3000
CMD ["node", "src/server.js"]
```

- **node:20-alpine:** Imagen ligera de Node.js.
- **WORKDIR /app:** Define la carpeta de trabajo dentro del contenedor.
- **npm install:** Instala dependencias solo si cambian.
- **COPY . .:** Copia el código al contenedor.
- **EXPOSE 3000:** Puerto donde escucha la app.
- **CMD:** Comando final para iniciar el servidor.

---

### 🔹 docker-compose.yml explicado

```yaml
version: '3.8'
services:
  app:
    build: .
    ports:
      - "3000:3000"
    volumes:
      - .:/app
    environment:
      - JWT_SECRET=supersecretoseguro
      - DB_FILE=./src/database/users.db
    restart: unless-stopped
```

- **build:** Usa el Dockerfile.
- **ports:** Mapea el puerto 3000.
- **volumes:** Sincroniza cambios locales automáticamente.
- **environment:** Variables de entorno internas del contenedor.

---

### 🔹 .env (usado con dotenv)

```
JWT_SECRET=supersecretoseguro
DB_FILE=./src/database/users.db
```

> Nota: En producción deberías generar un secreto seguro y único.

---

## 🔹 Flujo de autenticación JWT

1. **Login exitoso →** Se genera un JWT con:
    
    - `sub`: ID del usuario.
    - `iat`: Fecha de emisión.
    - `exp`: Fecha de expiración (ej. 30 minutos).
    - Firma con **HS256** usando la clave secreta del `.env`.
2. **En cada solicitud protegida →**
    
    - El token viaja en el header:
        
        ```
        Authorization: Bearer <token>
        ```
        
    - El middleware verifica:
        - Validez de la firma.
        - Que no esté expirado.
        - Que no haya sido alterado.

---

## 🔹 Cronograma detallado

|Semana|Actividad|
|---|---|
|1|Definir stack y crear el entorno Docker + estructura de carpetas y dependencias|
|1|Configurar SQLite y el modelo de usuarios|
|2|Implementar `/register` con bcrypt|
|2|Implementar `/login` y generación de JWT|
|2|Crear middleware de validación de JWT y ruta `/protected`|
|3|Pruebas de flujo completo y manejo de errores (credenciales incorrectas, tokens expirados)|
|3|Mejoras de seguridad y detalles (timeouts, validación de entradas)|
|3|Grabación del video explicativo|
|4|Empaquetado y entrega|

---

## 🔹 Seguridad mínima garantizada (detalles clave)

|Seguridad|Implementación|
|---|---|
|Hash de contraseñas|bcryptjs con 10 rondas|
|Expiración de tokens|JWT con expiración de 30 minutos (`exp`)|
|Claves sensibles|dotenv (.env)|
|Verificación estricta|Middleware personalizado de validación JWT|
|SQLite segura|Archivo local cifrado (opcional si se desea reforzar)|

---

## 🔹 Video demostrativo (detallado)

Duración: **6-8 minutos**. Contenido:

- Introducción breve sobre autenticación con JWT.
- Explicación del flujo de autenticación y arquitectura.
- Registro y login con Postman.
- Demostración de acceso autorizado y rechazo por token inválido o expirado.
- Resumen de ventajas y limitaciones.

---

## 🔹 Entrega final

Archivo `.zip` con:

- Código completo.
- Dockerfile y docker-compose.yml.
- `.env.example`.
- Video demostrativo.
- README detallado con instrucciones de instalación y uso.
