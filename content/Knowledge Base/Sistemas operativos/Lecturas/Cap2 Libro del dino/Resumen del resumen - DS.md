### **Resumen Detallado del Capítulo 2: Fundamentos de Sistemas Operativos**  
**(Basado en "Libro del dinosaurio - Cap 2")**  

---

#### **1. Introducción a los Sistemas Operativos**  
- **Definición**: Un sistema operativo (SO) es un software que actúa como intermediario entre el hardware y los programas de usuario, proporcionando un entorno para la ejecución de aplicaciones.  
- **Diseño**: El diseño de un SO es complejo y requiere objetivos claros (ej: eficiencia, seguridad, usabilidad) para guiar la selección de algoritmos y estrategias.  
- **Perspectivas de análisis**:  
  - **Servicios**: Funcionalidades que ofrece el SO (ej: ejecución de programas, E/S, comunicación).  
  - **Interfaces**: Modos de interacción (CLI, GUI, APIs).  
  - **Componentes**: Partes internas y su interconexión (núcleo, gestores de recursos, etc.).  

---

#### **2. Funciones Principales del Sistema Operativo**  
1. **Ejecución de Programas**:  
   - Carga programas en memoria, los ejecuta y finaliza su ejecución (normal o por error).  
   - Ejemplo: El *boot loader* en Arduino carga "sketches" en memoria específica.  

2. **Operaciones de E/S**:  
   - Gestiona dispositivos y archivos. En UNIX, dispositivos y archivos se tratan de forma unificada (ej: `read()`, `write()`).  
   - Los dispositivos pueden identificarse mediante nombres especiales (ej: `/dev` en Linux).  

3. **Manipulación del Sistema de Archivos**:  
   - Operaciones: crear, eliminar, leer, escribir, buscar y gestionar permisos.  
   - Permisos: Control de acceso basado en propiedad (ej: `chmod` en UNIX).  

4. **Comunicación entre Procesos**:  
   - **Mecanismos**:  
     - **Memoria compartida**: `shared_memory_create()`, `shared_memory_attach()`.  
     - **Redes**: Identificación por hostname/IP y procesos por PID.  
   - Problemas: Sincronización y protección en memoria compartida.  

5. **Detección y Corrección de Errores**:  
   - Errores en hardware (ej: fallos de memoria) o software (ej: división por cero).  
   - Dumps de memoria y logs para depuración (ej: uso de *debuggers*).  

6. **Asignación de Recursos**:  
   - Gestión de CPU, memoria, almacenamiento y dispositivos.  
   - **CPU Scheduling**: Algoritmos para asignar tiempo de CPU a procesos (ej: Round Robin).  

7. **Logging (Registro)**:  
   - Auditoría y estadísticas de uso de recursos (ej: facturación o análisis de rendimiento).  

8. **Protección y Seguridad**:  
   - Aislamiento de procesos para evitar interferencias.  
   - Control de acceso a información en sistemas multiusuario o en red.  

---

#### **3. Interfaces del Sistema Operativo**  
1. **Interfaz de Usuario (UI)**:  
   - **CLI (Shell)**:  
     - Intérprete de comandos (ej: Bash en UNIX).  
     - Funciones: Ejecutar comandos, manipular archivos (`cp`, `rm`, `ls`).  
   - **GUI**:  
     - Basada en ventanas, íconos y puntero (ratón o pantalla táctil).  
     - Origen: Investigación en Xerox PARC (1973, Xerox Alto).  

2. **APIs y Llamadas al Sistema**:  
   - **API**: Conjunto de funciones para desarrolladores (ej: POSIX, Win32).  
     - Ventaja: Portabilidad entre sistemas compatibles (ej: programas en C usando `libc`).  
   - **Llamadas al Sistema**:  
     - Invocadas por APIs para acceder a servicios del kernel.  
     - Categorías: Control de procesos, gestión de archivos, dispositivos, comunicaciones, etc.  
     - Ejemplo: `open()`, `close()`, `fork()`.  

3. **Ejecución de Programas**:  
   - Proceso: Compilación → Enlazado (linker) → Carga en memoria (loader).  
   - **Bibliotecas Dinámicas (DLLs)**:  
     - Cargadas en tiempo de ejecución para optimizar memoria (ej: `.dll` en Windows).  

---

#### **4. Diseño y Estructura del Sistema Operativo**  
1. **Principios de Diseño**:  
   - **Separación de Política y Mecanismo**:  
     - Mecanismo: *Cómo* se hace algo (ej: temporizador para protección de CPU).  
     - Política: *Qué* se hace (ej: tiempo asignado al temporizador).  

2. **Arquitecturas**:  
   - **Monolítica**:  
     - Todo el kernel en un único espacio de direcciones (ej: Linux tradicional).  
     - Ventaja: Eficiencia. Desventaja: Complejidad en mantenimiento.  
   - **Microkernel**:  
     - Funciones esenciales en kernel, servicios en modo usuario (ej: Mach, QNX).  
     - Ventaja: Estabilidad. Desventaja: Overhead por comunicación entre módulos.  
   - **Híbrida**:  
     - Combina monolítico con módulos cargables (ej: Linux moderno, Windows).  
     - Módulos del kernel (LKMs): Agregar drivers o funciones sin reiniciar.  

3. **Ejemplos de Sistemas**:  
   - **macOS vs. iOS**:  
     - macOS: Intel/AMD, APIs abiertas (POSIX), para PCs.  
     - iOS: ARM, gestión agresiva de memoria y energía, APIs restringidas.  
   - **Android**:  
     - Basado en Linux, pero con bibliotecas personalizadas (Bionic libc).  
     - Bootloader: LK ("Little Kernel").  

---

#### **5. Proceso de Arranque (Booting)**  
1. **BIOS vs. UEFI**:  
   - **BIOS**: Firmware antiguo, proceso de arranque en etapas (carga del boot block).  
   - **UEFI**: Más rápido, soporta discos grandes y 64 bits, unificado.  

2. **Boot Loaders**:  
   - **GRUB**: Usado en Linux/UNIX, permite seleccionar kernels y ajustar parámetros.  
   - **LK**: En Android, carga el kernel y el sistema de archivos RAM inicial.  

3. **Modos de Recuperación**:  
   - Single-user mode (Linux), Recovery Mode (Windows/iOS): Para reparar sistemas o reinstalar SO.  

---

#### **6. Herramientas de Depuración y Rendimiento**  
- **BCC (BPF Compiler Collection)**:  
  - Kit de herramientas para tracing en Linux, basado en eBPF.  
  - Usos: Diagnóstico de rendimiento, detección de cuellos de botella.  

---

#### **7. Retos en Desarrollo de Aplicaciones**  
- **Portabilidad**:  
  - **Interpretados**: Python, Ruby (lentos pero multiplataforma).  
  - **Máquinas Virtuales**: Java (JVM), .NET (CLR).  
  - **APIs Estándar**: POSIX para compatibilidad entre UNIX-like.  
- **Desafíos Técnicos**:  
  - Formatos binarios distintos por SO.  
  - Llamadas al sistema variables (ej: `open()` en UNIX vs. `CreateFile()` en Windows).  

---

#### **8. Conceptos Clave para el Quiz**  
- Diferencias entre CLI y GUI.  
- Proceso de ejecución de un programa (compilación, enlazado, carga).  
- Roles de APIs vs. llamadas al sistema.  
- Ventajas/desventajas de arquitecturas (monolítica, microkernel).  
- Ejemplos de sistemas operativos y sus características (Linux, macOS, iOS, Android).  
- Proceso de arranque (UEFI vs. BIOS, GRUB, LK).  
- Herramientas de depuración (BCC, eBPF).  
