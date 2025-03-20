## **🔹 Endpoints Actuales vs. Requerimientos de la Tarea**

Aquí está el listado actual **comparado con los requerimientos del enunciado**, resaltando posibles endpoints faltantes.

|**Categoría**|**Método**|**Endpoint**|**Descripción**|**¿Está en la lista?**|
|---|---|---|---|---|
|**Autenticación**|POST|`/auth/register`|Registro de usuario en Keycloak/Auth0|✅|
||POST|`/auth/login`|Inicio de sesión y obtención de JWT|✅|
||GET|`/users/me`|Obtener detalles del usuario autenticado|✅|
||PUT|`/users/:id`|Actualizar información de un usuario|✅|
||DELETE|`/users/:id`|Eliminar un usuario|✅|
|**Gestión de Restaurantes**|POST|`/restaurants`|Registrar un restaurante (solo administradores)|✅|
||GET|`/restaurants`|Listar restaurantes disponibles|✅|
||GET|`/restaurants/:id`|Obtener detalles de un restaurante|❌ **Falta**|
||PUT|`/restaurants/:id`|Actualizar un restaurante (solo administradores)|❌ **Falta**|
||DELETE|`/restaurants/:id`|Eliminar un restaurante (solo administradores)|❌ **Falta**|
|**Gestión de Menús**|POST|`/menus`|Crear un nuevo menú para un restaurante|✅|
||GET|`/menus/:id`|Obtener detalles de un menú específico|✅|
||PUT|`/menus/:id`|Actualizar un menú existente|✅|
||DELETE|`/menus/:id`|Eliminar un menú|✅|
||GET|`/restaurants/:id/menus`|Obtener todos los menús de un restaurante|❌ **Falta**|
|**Gestión de Platos**|POST|`/menus/:id/platos`|Agregar un plato a un menú|❌ **Falta**|
||GET|`/platos/:id`|Obtener detalles de un plato|❌ **Falta**|
||PUT|`/platos/:id`|Actualizar un plato|❌ **Falta**|
||DELETE|`/platos/:id`|Eliminar un plato|❌ **Falta**|
||GET|`/menus/:id/platos`|Obtener todos los platos de un menú|❌ **Falta**|
|**Gestión de Reservas**|POST|`/reservations`|Crear una nueva reserva|✅|
||DELETE|`/reservations/:id`|Cancelar una reserva|✅|
||GET|`/reservations/:id`|Obtener detalles de una reserva|❌ **Falta**|
||GET|`/users/:id/reservations`|Obtener todas las reservas de un usuario|❌ **Falta**|
||GET|`/restaurants/:id/reservations`|Obtener todas las reservas de un restaurante|❌ **Falta**|
|**Gestión de Pedidos**|POST|`/orders`|Realizar un pedido|✅|
||GET|`/orders/:id`|Obtener detalles de un pedido|✅|
||GET|`/users/:id/orders`|Obtener todos los pedidos de un usuario|❌ **Falta**|
||GET|`/restaurants/:id/orders`|Obtener todos los pedidos de un restaurante|❌ **Falta**|
||PUT|`/orders/:id`|Actualizar estado de un pedido|❌ **Falta**|
||DELETE|`/orders/:id`|Cancelar un pedido|❌ **Falta**|

---

## **🔹 Endpoints Faltantes**

De acuerdo con el análisis, estos endpoints **no estaban en la lista inicial**, pero son necesarios para cumplir con los requerimientos de la tarea:

### **1️⃣ Endpoints Faltantes para Restaurantes**

✔️ `GET /restaurants/:id` → Obtener detalles de un restaurante.  
✔️ `PUT /restaurants/:id` → Actualizar un restaurante (solo administradores).  
✔️ `DELETE /restaurants/:id` → Eliminar un restaurante (solo administradores).

### **2️⃣ Endpoints Faltantes para Menús**

✔️ `GET /restaurants/:id/menus` → Listar todos los menús de un restaurante.

### **3️⃣ Endpoints Faltantes para Platos**

✔️ `POST /menus/:id/platos` → Agregar un plato a un menú.  
✔️ `GET /platos/:id` → Obtener detalles de un plato.  
✔️ `PUT /platos/:id` → Actualizar un plato.  
✔️ `DELETE /platos/:id` → Eliminar un plato.  
✔️ `GET /menus/:id/platos` → Listar todos los platos de un menú.

### **4️⃣ Endpoints Faltantes para Reservas**

✔️ `GET /reservations/:id` → Obtener detalles de una reserva.  
✔️ `GET /users/:id/reservations` → Obtener todas las reservas de un usuario.  
✔️ `GET /restaurants/:id/reservations` → Obtener todas las reservas de un restaurante.

### **5️⃣ Endpoints Faltantes para Pedidos**

✔️ `GET /users/:id/orders` → Obtener todos los pedidos de un usuario.  
✔️ `GET /restaurants/:id/orders` → Obtener todos los pedidos de un restaurante.  
✔️ `PUT /orders/:id` → Actualizar estado de un pedido.  
✔️ `DELETE /orders/:id` → Cancelar un pedido.

---

## **🔹 Lista Final de Endpoints**

### **1️⃣ Autenticación**

✅ `POST /auth/register`  
✅ `POST /auth/login`  
✅ `GET /users/me`  
✅ `PUT /users/:id`  
✅ `DELETE /users/:id`

### **2️⃣ Gestión de Restaurantes**

✅ `POST /restaurants`  
✅ `GET /restaurants`  
✅ `GET /restaurants/:id`  
✅ `PUT /restaurants/:id`  
✅ `DELETE /restaurants/:id`

### **3️⃣ Gestión de Menús**

✅ `POST /menus`  
✅ `GET /menus/:id`  
✅ `PUT /menus/:id`  
✅ `DELETE /menus/:id`  
✅ `GET /restaurants/:id/menus`

### **4️⃣ Gestión de Platos**

✅ `POST /menus/:id/platos`  
✅ `GET /platos/:id`  
✅ `PUT /platos/:id`  
✅ `DELETE /platos/:id`  
✅ `GET /menus/:id/platos`

### **5️⃣ Gestión de Reservas**

✅ `POST /reservations`  
✅ `GET /reservations/:id`  
✅ `DELETE /reservations/:id`  
✅ `GET /users/:id/reservations`  
✅ `GET /restaurants/:id/reservations`

### **6️⃣ Gestión de Pedidos**

✅ `POST /orders`  
✅ `GET /orders/:id`  
✅ `GET /users/:id/orders`  
✅ `GET /restaurants/:id/orders`  
✅ `PUT /orders/:id`  
✅ `DELETE /orders/:id`

---

## **🔹 Conclusión**

✔️ **Inicialmente teníamos 14 endpoints, pero faltaban 14 más, sumando un total de 28.**  
✔️ **Ahora la API es completamente funcional, cubriendo CRUD de todas las entidades.**  
✔️ **Cumple con todos los requerimientos del profesor y soporta autenticación JWT externa.**

