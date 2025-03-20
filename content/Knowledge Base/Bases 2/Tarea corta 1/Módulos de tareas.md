### **1️⃣ Autenticación y Gestión de Usuarios (Módulo de Seguridad)**

📌 **Objetivo:** Implementar el servicio de autenticación como un sistema separado, basado en JWT.

🔹 **Tareas:**

- [ ]  Configurar **Keycloak/Auth0/Authelia/Dex** en un contenedor Docker.
- [ ]  Implementar **registro** y **login** de usuarios con JWT.
- [ ]  Crear middleware para validar y proteger rutas según permisos.
- [ ]  Definir modelos de usuario: **clientes y administradores**.
- [ ]  Probar autenticación de manera independiente con Postman.

💡 **Dependencias:** Ninguna (se puede desarrollar primero).

---

### **2️⃣ API REST - Core de Reservas**

📌 **Objetivo:** Implementar la API principal con la lógica de negocio.

🔹 **Tareas:**

- [ ]  Crear estructura del proyecto con **Express.js (Node.js)**.
- [ ]  Implementar los modelos de datos para:
    - Usuarios
    - Restaurantes
    - Menús
    - Reservas
    - Pedidos
- [ ]  CRUD de **restaurantes** (solo administradores).
- [ ]  CRUD de **menús y platos** por restaurante.
- [ ]  CRUD de **reservas** y verificación de disponibilidad.
- [ ]  CRUD de **pedidos** con opción de recogida.

💡 **Dependencias:**

- Necesita que el módulo de autenticación esté listo para proteger rutas.

---

### **3️⃣ Base de Datos y Conexión con PostgreSQL**

📌 **Objetivo:** Configurar la base de datos PostgreSQL con Docker.

🔹 **Tareas:**

- [ ]  Crear esquema en **PostgreSQL** con las tablas necesarias.
- [ ]  Definir relaciones entre entidades (Usuarios, Reservas, etc.).
- [ ]  Escribir **migraciones** con Sequelize/TypeORM.
- [ ]  Implementar conexión entre la API y la base de datos.

💡 **Dependencias:**

- Puede desarrollarse en paralelo con la API REST.
- Es fundamental para probar CRUDs en la API.

---

### **4️⃣ Contenedorización con Docker**

📌 **Objetivo:** Configurar Docker para la API, autenticación y base de datos.

🔹 **Tareas:**

- [ ]  Crear **Dockerfile** para la API.
- [ ]  Crear un contenedor para PostgreSQL.
- [ ]  Configurar **servicio de autenticación en su propio contenedor**.
- [ ]  Definir un archivo **docker-compose.yml** para levantar todos los servicios.

💡 **Dependencias:**

- La API debe estar implementada para poder contenerizarla.
- La base de datos también debe estar lista.

---

### **5️⃣ Pruebas Unitarias e Integración**

📌 **Objetivo:** Implementar pruebas con cobertura del 90% mínimo.

🔹 **Tareas:**

- [ ]  Escribir pruebas unitarias para cada endpoint clave.
- [ ]  Implementar pruebas de integración entre API y base de datos.
- [ ]  Verificar autenticación y seguridad con JWT.
- [ ]  Medir cobertura de código (**Jest/Mocha**).

💡 **Dependencias:**

- La API REST debe estar funcional.
- La base de datos y autenticación deben estar configuradas.

---

### **6️⃣ Documentación y Entrega Final**

📌 **Objetivo:** Crear documentación clara y un video explicativo.

🔹 **Tareas:**

- [ ]  Documentar la API con **Swagger**.
- [ ]  Escribir un **README.md** con instrucciones de uso.
- [ ]  Explicar cómo correr el sistema en **Docker**.
- [ ]  Grabar un **video demostrativo** del funcionamiento.

💡 **Dependencias:**

- Todo el sistema debe estar terminado y funcional.

---

### **🛠 Integración Final**

1. Conectar el módulo de autenticación con la API.
2. Probar el sistema completo en **Docker Compose**.
3. Ejecutar todas las pruebas y validar cobertura.
4. Preparar entrega con README y video.

---

💡 **En resumen, los bloques independientes serían:** 
1️⃣ **Autenticación y JWT**  
2️⃣ **API REST (CRUDs y lógica de negocio)**  
3️⃣ **Base de Datos PostgreSQL**  
4️⃣ **Dockerización y Orquestación**  
5️⃣ **Pruebas Unitarias e Integración**  
6️⃣ **Documentación y Video**

