## 1. Visión general de un sistema operativo

Un **sistema operativo** (SO) es el software que provee el entorno en el cual se ejecutan los programas. Su diseño y estructura pueden variar significativamente, pero comparten metas esenciales: coordinar y administrar recursos (CPU, memoria, dispositivos, archivos, etc.), así como brindar servicios a usuarios y aplicaciones. [citeturn0file0]

- **Servicios al usuario**. El SO ofrece funciones como interfaz de usuario, ejecución de programas, operaciones de E/S, manipulación de archivos, comunicación entre procesos y detección de errores, entre otras.
- **Operación interna**. Internamente, se encarga de asignar recursos, llevar contabilidad del uso del sistema (logging), y proteger y asegurar la información. 

---

## 2. Servicios que ofrece un sistema operativo

En un nivel general, los SO suelen ofrecer servicios para que el usuario y los programas trabajen de forma más sencilla y segura. Destacan los siguientes: [citeturn0file0]

1. **Interfaz de usuario (UI)**
    
    - **CLI (Command-Line Interface)**: Se basa en introducir comandos de texto (por ejemplo, shells como bash en UNIX/Linux).
    - **GUI (Graphical User Interface)**: Emplea ventanas, iconos y menús, manejables por ratón o pantalla táctil.
    - **Interfaz táctil**: Propia de dispositivos móviles y tabletas, con gestos táctiles para interacción.
2. **Ejecución de programas**
    
    - El SO debe poder cargar un programa en memoria y ejecutarlo, así como permitir la finalización normal o anormal (por error).
3. **Operaciones de E/S**
    
    - Lectura y escritura en dispositivos de almacenamiento, pantallas, redes, etc.
    - Por razones de protección y eficiencia, muchas veces no se permite acceso directo al hardware, por lo que el SO brinda llamadas especiales.
4. **Manipulación de archivos**
    
    - Lectura, escritura, creación y eliminación de archivos y directorios.
    - Gestión de atributos de archivos (permisos, propiedades).
    - Búsqueda de archivos y organización en directorios.
5. **Comunicaciones**
    
    - **Entre procesos**: intercambio de información si se ejecutan en la misma máquina (usando memoria compartida o paso de mensajes).
    - **Entre sistemas en red**: conectividad vía sockets, uso de protocolo IP, etc.
6. **Detección y manejo de errores**
    
    - El SO debe vigilar errores de hardware (por ejemplo, fallas de memoria, dispositivos) o de software (overflow, accesos ilegales, etc.) y definir acciones correctivas.
7. **Asignación de recursos**
    
    - El SO decide qué procesos usan la CPU, cuánto tiempo; asigna y libera memoria, dispositivos, etc., en sistemas con multitarea.
8. **Registro (logging) o contabilidad**
    
    - Lleva registros del uso de recursos para estadísticas, facturación o auditoría.
9. **Protección y seguridad**
    
    - Se asegura de que usuarios y procesos no interfieran maliciosamente en la memoria o en los dispositivos de otros.
    - Control de acceso, autenticación, cifrado, etc.

---

## 3. Interfaz entre usuario y sistema operativo

Las formas principales de interactuar con el SO son: [citeturn0file0]

- **Command Interpreter (Shell)**: Interpreta y ejecuta comandos de texto.
- **Interfaces gráficas (GUI)**: Proporcionan metáforas de escritorio, iconos, barras de menú y más.
- **Interfaz táctil**: Presente en tablets y smartphones, orientada al uso de gestos y toques.

Cada enfoque responde a necesidades diferentes. Los usuarios avanzados suelen preferir shells de línea de comandos, mientras que la mayoría de usuarios domésticos están más habituados a las GUIs o a las interfaces táctiles.

---

## 4. Llamadas al sistema (System Calls)

Los **system calls** son el mecanismo que permite que un programa solicite servicios o acceso a recursos del SO. [citeturn0file0]

### 4.1 Uso de llamadas al sistema

Imagina un programa que copia el contenido de un archivo a otro. Requerirá llamadas al sistema para:

1. Obtener los nombres de archivo (por CLI o GUI).
2. Abrir el archivo origen (open()) y crear el archivo destino.
3. Leer (read()) y escribir (write()) repetidamente.
4. Cerrar ambos archivos (close()).
5. Manejar condiciones de error (archivo inexistente, falta de permisos, etc.).

### 4.2 API (Application Programming Interface)

Normalmente, el programador no llama directamente a las funciones internas del SO. En su lugar, usa APIs (por ejemplo, la API de Windows, POSIX en sistemas tipo UNIX, o la API de Java). Estas APIs encapsulan las llamadas al sistema y suelen ofrecer portabilidad a las aplicaciones, de modo que puedan compilarse en diferentes SO con cambios mínimos.

### 4.3 Tipos de llamadas al sistema

Los system calls se agrupan en grandes categorías: [citeturn0file0]

- **Control de procesos**: Crear y terminar procesos (create process, terminate process), cargar y ejecutar programas (load, exec), esperar eventos (wait), manejar bloqueos, etc.
- **Gestión de archivos**: Crear, eliminar, abrir, cerrar, leer, escribir, reposicionar, cambiar atributos de archivos y directorios.
- **Gestión de dispositivos**: Peticiones para uso exclusivo de un dispositivo (request, release), leer/escribir, etc.
- **Mantenimiento de información**: Obtener y establecer hora/fecha, estado del sistema, atributos de proceso/archivo.
- **Comunicaciones**: Creación y uso de canales de comunicación (sockets, pipes, memoria compartida, paso de mensajes).
- **Protección**: Control de permisos y acceso a recursos, verificación de usuarios, etc.

---

## 5. Servicios del sistema y utilidades

Además de las llamadas al sistema, muchos SO proporcionan **servicios de sistema** o **utilities**, que facilitan el desarrollo y la ejecución de programas: [citeturn0file0]

- **Gestión de archivos**: Copiar, renombrar, listar, imprimir, etc.
- **Información del sistema**: Estado, uso de disco, procesos activos, tiempo de arranque, entre otros.
- **Modificación de archivos**: Editores de texto, herramientas de búsqueda y filtrado.
- **Soporte de lenguajes de programación**: Compiladores, ensambladores, intérpretes, depuradores, etc.
- **Comunicación**: Comandos y utilidades para redes, mensajería, conexiones remotas.
- **Servicios en segundo plano (background services o daemons)**: Procesos que se inician al arrancar el sistema y permanecen ejecutándose para manejar eventos o peticiones (por ejemplo, demonios de impresión, servidores web, etc.).

---

## 6. Proceso de compilación y carga: Linkers y Loaders

Cuando un programa en C (u otro lenguaje) se compila, se producen **archivos objeto** que contienen código en un formato “relocatable”. Un **linker** combina esos archivos y las bibliotecas necesarias (estáticas o dinámicas) para generar un **ejecutable**. El **loader**, al invocarse (por ejemplo, al ejecutar el nombre del programa en la consola), carga ese binario en memoria y establece las direcciones finales que usará el programa. [citeturn0file0]

- **ELF (Executable and Linkable Format)** en UNIX/Linux.
- **PE (Portable Executable)** en Windows.
- **Mach-O** en macOS.

Algunos sistemas utilizan **librerías dinámicas** (DLL en Windows, .so en Linux) que se vinculan solo si se necesitan durante la ejecución, ahorrando memoria y reduciendo el tamaño del binario final.

---

## 7. Por qué las aplicaciones dependen del sistema operativo

El **conjunto específico de llamadas al sistema** que ofrece un SO hace que un binario compilado para un sistema no sea directamente ejecutable en otro. Hay varias estrategias para compartir aplicaciones entre distintos sistemas: [citeturn0file0]

1. **Lenguaje interpretado** (Python, Ruby): requiere un intérprete compatible en cada SO.
2. **Máquinas virtuales de lenguaje** (Java, .NET): código bytecode ejecutado en una VM, portable a varias plataformas.
3. **Uso de APIs estándar** (POSIX, por ejemplo) o reescritura y recompilación para cada SO.

---

## 8. Diseño e implementación de sistemas operativos

El diseño de un SO puede describirse en distintos niveles. Es crucial separar la **política** de la **mecánica**:

- **Política**: qué se hace (por ejemplo, cuánto tiempo de CPU se asigna a un proceso).
- **Mecánica**: cómo se implementa esa política (uso de interrupciones, temporizadores, etc.). [citeturn0file0]

### 8.1 Implementación

Los SO suelen escribirse mayoritariamente en C/C++. En piezas muy específicas (controladores, manejo de hardware) puede usarse ensamblador. [citeturn0file0]

### 8.2 Estructuras del SO

Distintos modelos de organización del kernel:

1. **Monolítico**
    
    - Todo el kernel corre en un único espacio de direcciones en modo kernel.
    - Ejemplo: **Linux** es monolítico, aunque modular.
2. **En capas (layered)**
    
    - Se divide el SO en niveles, donde el más bajo gestiona hardware y capas superiores ofrecen más abstracción.
    - Cada capa solo interactúa con las capas inmediatamente inferior.
3. **Microkernel**
    
    - Mínimo de funcionalidades en el kernel (comunicación básica, manejo de interrupciones).
    - El resto (drivers, sistemas de archivos, protocolos de red) reside en espacios de usuario como servidores.
    - Ventaja: sistema más seguro y extensible, aunque puede tener sobrecarga de comunicación.
4. **Modular**
    
    - Núcleo básico al que se pueden **cargar “módulos”** en tiempo de ejecución (drivers, extensiones).
    - Flexibilidad similar al microkernel, pero manteniendo estructura monolítica.
5. **Híbrido**
    
    - Combina enfoques de microkernel y monolítico, buscando un equilibrio entre rendimiento y modularidad.

---

## 9. Arranque del sistema (System Boot)

Cuando encendemos un ordenador o dispositivo:

1. El hardware inicializa componentes y busca un **boot loader** (por ejemplo, en BIOS/UEFI).
2. El boot loader carga el kernel del SO en memoria.
3. El kernel se inicializa y lanza procesos o demonios de arranque, dejando el sistema listo para el uso. [citeturn0file0]

En sistemas pequeños (por ejemplo, microcontroladores como Arduino), no hay un SO completo, sino un **boot loader** minimalista que carga el programa a ejecutar (el “sketch”). [citeturn0file0]

---

## 10. Depuración y generación de sistemas

- **Depuración**: Uso de **logs**, dumps de memoria, y herramientas de rastreo (por ejemplo, `strace` en Linux) para aislar y corregir errores.
- **Generación de sistemas**: Ajuste del núcleo (kernel) a hardware específico, enlazando solo los módulos y controladores necesarios.

---

## 11. Comentarios finales y puntos clave para el quiz

1. **Conceptos básicos**:
    - Diferencia entre **servicios del SO** (vista externa: ejecución de programas, E/S, archivos) y **objetivos internos** (asignación de recursos, protección, contabilidad).
2. **Interfaces de usuario**: CLI vs GUI vs táctil: ventajas y desventajas.
3. **Llamadas al sistema**:
    - Por qué son la “puerta de entrada” para que programas soliciten acciones al SO.
    - Diferentes categorías y ejemplos de cada una.
4. **Herramientas y servicios**:
    - Importancia de **compiladores**, **linkers**, **loaders**, **debuggers**.
    - Uso de **scripting** y **shell** para automatizar tareas.
5. **Organización interna del kernel**:
    - Modelos (monolítico, microkernel, modular, híbrido).
    - Ventajas y desventajas de cada enfoque.
6. **Arranque (boot)** y **estructuras de carga** (BIOS, UEFI, boot loaders).
7. **Protección y seguridad**: cómo se controla el acceso a recursos y se evita la interferencia entre procesos.

