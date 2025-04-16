# 🧠 Análisis de Requisitos: Punto por Punto

## 🔍 Interpretación clave del enunciado

- El **failover debe ejecutarse** ante la falla de la instancia principal.
    
- **Debe haber una réplica funcional y preparada**, lista para asumir el control.
    
- No se permite usar **sistemas preconstruidos de failover automático** (como los de Azure, AlwaysOn configurado con failover automático, Kubernetes, etc.).
    
- **Sí puedes implementar scripts propios**, creados por ti, que automaticen la sincronización de datos o incluso la activación de la réplica.
    
- **Debes hacer todo dentro de Docker**.
    
- El entorno debe demostrar:
    
    - Sincronización de datos
        
    - Conmutación de roles
        
    - Integridad de datos
        
    - Pruebas reales
        

---

# 🧭 Planificación Completa para Obtener un 100%

---

## 🗂️ I. **Estructura del Proyecto**

```
/mssql-failover-project/
│
├── docker/
│   ├── docker-compose.yml
│   ├── mssql_primary/
│   │   └── Dockerfile
│   ├── mssql_secondary/
│   │   └── Dockerfile
│   └── init_scripts/
│       ├── setup_primary.sql
│       └── setup_secondary.sql
│
├── scripts/
│   ├── trigger_failover.sh
│   ├── restore_secondary.sql
│   └── check_health.sh
│
├── docs/
│   ├── informe_final.pdf
│   └── diagramas/
│       └── arquitectura.png
│
├── video/
│   └── demostracion.mp4
│
└── README.md
```

---

## 🧱 II. **Configuración del Entorno (35%)**

### 🔹1. Crear el archivo `docker-compose.yml`

- Define al menos dos servicios:
    
    - `mssql_primary`
        
    - `mssql_secondary`
        
- Utiliza la imagen oficial de `mcr.microsoft.com/mssql/server:2019-latest`
    
- Establece `ACCEPT_EULA`, `SA_PASSWORD`
    
- Expón puertos diferentes (`1433` y `1434`)
    
- Configura volúmenes persistentes
    

### 🔹2. Dockerfiles (si decides modificar la imagen base)

- Pueden incluir scripts de inicialización usando:
    
    ```Dockerfile
    COPY ./init_scripts /docker-entrypoint-initdb.d
    ```
    

### 🔹3. Scripts de inicialización

- `setup_primary.sql`:
    
    - Crear base de datos `AdventureWorks` (puede cargarse como `.bak`)
        
    - Configurar permisos y acceso
        
- `setup_secondary.sql`:
    
    - Crear la misma base en modo _restore only_ o _standby_
        
    - Configurar para recibir los logs
        

---

## 🔁 III. **Implementación de la Replicación Failover (20%)**

### 🔹1. Escoge una técnica **controlable** por vos:

La más viable en este caso:

> **Log Shipping + Failover manual con script personalizado**

### 🔹2. Implementación:

#### En el primario:

- Crea la base de datos
    
- Crea un job (script) que:
    
    - Realice backups del log
        
    - Los copie a un volumen compartido
        

#### En el secundario:

- Crea un script (job) que:
    
    - Lea los logs del volumen compartido
        
    - Aplique automáticamente esos logs (`RESTORE LOG ... WITH NORECOVERY`)
        

#### Script de failover (`trigger_failover.sh`):

- Detecta que el primario no responde (ping o `sqlcmd`)
    
- Ejecuta:
    
    ```sql
    RESTORE DATABASE AdventureWorks WITH RECOVERY;
    ```
    
- Informa por consola: "FAILOVER EJECUTADO: La base ha sido promovida"
    

> Nota: Este script debe ser **hecho por vos**, no copiado de soluciones automáticas.

---

## 🧪 IV. **Pruebas de Funcionamiento (20%)**

### ✔️ Pruebas obligatorias:

1. **Simulación de fallo:**
    
    - Usa `docker stop mssql_primary`
        
2. **Ejecución de failover con tu script**
    
3. **Acceso exitoso a la base en la instancia secundaria**
    
4. **Verificación de datos previos y posteriores**
    
5. **Restauración o recreación del primario (opcional)**
    

> **Todo esto debe grabarse en un video con narración**.

---

## 📹 V. **Video de Pruebas (10%)**

- Debe contener:
    
    - Breve presentación del proyecto
        
    - Cómo se inicia el sistema (`docker compose up`)
        
    - Ejecución del `trigger_failover.sh`
        
    - Consulta a la base en la nueva instancia activa
        
    - Narración explicando cada paso
        

> Recomendación: usa OBS Studio o ScreenRec para grabar.

---

## 📑 VI. **Documentación Técnica (25%)**

### ✔️ Incluir:

- Detalle de arquitectura y herramientas usadas
    
- Instrucciones de despliegue paso a paso
    
- Comandos y scripts (comentados)
    
- Capturas de consola y base de datos
    
- Diagrama arquitectónico (puede ser hecho en draw.io o Miro)
    
- Problemas encontrados y soluciones
    
- Cómo funciona tu sistema de failover manual
    

---

## 🧠 VII. **Análisis Crítico (10%)**

### ✅ Incluir:

- **Ventajas y desventajas del método usado (log shipping + failover manual)**
    
- **Escenarios reales donde esto es útil**
    
- **Riesgos** (ej. si el backup falla o el script no corre)
    
- Comparativa con:
    
    - Always On Availability Groups
        
    - Replicación transaccional
        
    - Clustering (WSFC)
        

---

## 📦 VIII. **Entrega (Formato y Penalización)**

### ✔️ Estructura de entrega:

- `reporte_final.pdf`
    
- `scripts.zip` con:
    
    - `docker-compose.yml`
        
    - Scripts SQL y Shell
        
- `video.mp4`
    

### ⚠️ Entrega en TecDigital antes de las **10:00pm del 10 de abril**.

- Cada 24h de atraso → -5%
    

---

## 📝 ¿Querés que te prepare...?

- Un `docker-compose.yml` inicial
    
- Los scripts de backup/log shipping
    
- Un esquema de `README.md` para guiarte en tu informe
    

Puedo ayudarte a desarrollar todo paso a paso. ¿Cuál de esas partes querés comenzar a construir?