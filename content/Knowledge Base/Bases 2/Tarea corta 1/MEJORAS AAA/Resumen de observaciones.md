### **Problemas Clave a Resolver para la Autenticación Funcional**

#### **1. Integración Incompleta de Keycloak**
- **Descripción**:  
  El middleware de Keycloak está deshabilitado (`app.use(keycloak.middleware())` está comentado), lo que impide la protección de rutas. Además, no se validan roles ni tokens JWT en los endpoints críticos (ej: `POST /restaurants`).
- **Solución**:  
  - Descomentar y configurar el middleware de Keycloak en `src/index.js`.  
  - Proteger rutas con roles usando `keycloak.protect('realm:administrador')`.  
  - Asegurar que el archivo `realm-export.json` defina correctamente el cliente y roles en Keycloak.

---

#### **2. Falta de Sincronización de Usuarios**
- **Descripción**:  
  Los usuarios registrados en Keycloak no se reflejan en la tabla `usuarios` de PostgreSQL, rompiendo la integridad de reservas y pedidos.
- **Solución**:  
  - Modificar el endpoint `/auth/register` para:  
    1. Crear usuarios en Keycloak usando su API Admin.  
    2. Insertar el mismo usuario en PostgreSQL con el `id` generado por Keycloak.  
  - Ejemplo de código para `auth.controller.js`:
    ```javascript
    import KeycloakAdminClient from 'keycloak-admin';
    const kcAdmin = new KeycloakAdminClient();

    export const register = async (req, res) => {
      const { nombre, correo, password, tipo_usuario } = req.body;

      // Crear en Keycloak
      await kcAdmin.auth({ clientId: 'admin-cli', username: 'admin', password: 'admin' });
      const userId = await kcAdmin.users.create({ username: nombre, email: correo, enabled: true });
      await kcAdmin.users.resetPassword({ id: userId, credential: { value: password } });

      // Crear en PostgreSQL
      await pool.query(
        'INSERT INTO usuarios (id, nombre, correo, tipo_usuario) VALUES ($1, $2, $3, $4)',
        [userId, nombre, correo, tipo_usuario]
      );

      res.status(201).json({ message: 'Usuario registrado' });
    };
    ```

---

#### **3. Integridad Referencial en la Base de Datos**
- **Descripción**:  
  Las tablas `reservas` y `pedidos` no tienen claves foráneas hacia `usuarios.id`, permitiendo operaciones con usuarios inexistentes.
- **Solución**:  
  Ejecutar migraciones SQL en `init.sql`:
  ```sql
  ALTER TABLE reservas ADD CONSTRAINT fk_reservas_cliente FOREIGN KEY (id_cliente) REFERENCES usuarios(id);
  ALTER TABLE pedidos ADD CONSTRAINT fk_pedidos_cliente FOREIGN KEY (id_cliente) REFERENCES usuarios(id);
  ```

---

#### **4. Configuración de Red en Docker Compose**
- **Descripción**:  
  Los servicios `app` y `auth` no pueden comunicarse porque no comparten una red definida.
- **Solución**:  
  Agregar una red en `docker-compose.yml`:
  ```yaml
  networks:
    restaurante-net:
      driver: bridge

  services:
    app:
      networks:
        - restaurante-net
    auth:
      networks:
        - restaurante-net
  ```

---

#### **5. Pruebas Unitarias sin Cobertura de Autenticación**
- **Descripción**:  
  Las pruebas no validan escenarios con JWT ni roles, y la cobertura no alcanza el 90%.
- **Solución**:  
  - Incluir tokens JWT válidos en las pruebas usando `supertest`.  
  - Ejemplo en `auth.test.js`:
    ```javascript
    it('Obtener detalles del usuario autenticado', async () => {
      const token = 'token-generado-por-keycloak'; // Usar token real
      const res = await request(app)
        .get('/auth/me')
        .set('Authorization', `Bearer ${token}`);
      expect(res.statusCode).toBe(200);
    });
    ```

---

### **Resumen de Acciones Críticas**
1. **Habilitar Middleware de Keycloak**:  
   Asegurar que todas las rutas sensibles estén protegidas con `keycloak.protect()`.
2. **Sincronizar Usuarios**:  
   Usar la API Admin de Keycloak para reflejar usuarios en PostgreSQL.
3. **Corregir la Base de Datos**:  
   Añadir claves foráneas para garantizar integridad.
4. **Configurar Red en Docker**:  
   Permitir comunicación entre `app` y `auth`.
5. **Actualizar Pruebas**:  
   Cubrir autenticación y alcanzar el 90% de cobertura.

### **Resultado Esperado**
- ✅ **Autenticación Funcional**: Usuarios registrados en Keycloak y PostgreSQL.  
- 🔒 **Rutas Protegidas**: Solo administradores pueden crear restaurantes.  
- 🛡️ **Integridad de Datos**: Reservas y pedidos vinculados a usuarios válidos.  
- 🧪 **Pruebas Robustas**: Cobertura del 90% con validación de JWT.  

Con estos ajustes, el sistema cumplirá con todos los requerimientos del enunciado. 🚀