
Respuestas del resumen:

### 1. Identificar características de los Traductores y Compiladores

- Traductores:
	- Programas que convierten texto de un source language (lenguaje fuente) a un target language (lenguaje objetivo).
	- Ejemplos:
		- Un traductor chino-inglés convierte texto en chino a texto semánticamente equivalente en inglés (lenguajes naturales).
		- Un traductor de Java a C transforma programas en Java al lenguaje C (lenguajes de programación).
    

- Tipos principales:
	- Assembler: Traduce lenguajes de ensamblaje al código máquina correspondiente. Ejemplo: ensamblador x86.
	- Compiler: Traduce lenguajes de alto nivel a código máquina o un lenguaje más bajo. Ejemplo: Java a x86.
	- Disassembler: Convierte código máquina a lenguaje ensamblador.
	- Decompiler: Convierte código máquina a un lenguaje de alto nivel.
    

- Los traductores verifican:
	- Syntax (estructura del lenguaje fuente).
	- Semantics (significado contextual).
    
- Se enfocan en producir un programa objeto (object program) que sea semánticamente equivalente al programa fuente.
    

- Compiladores:
	- Subtipo de traductores que convierten programas de alto nivel a código de bajo nivel (generalmente código máquina).
	- Antes de generar un programa objeto, verifican si el programa fuente está bien formado según las reglas sintácticas y semánticas.
	- Utilizan herramientas como cross-compilers para generar código para máquinas diferentes de la máquina anfitriona (host machine). Ejemplo: un compilador que genera código PPC desde un sistema x86.
	- Son ideales para maximizar velocidad de ejecución en producción, aunque requieren más tiempo de traducción inicial.
    

---

### 2. Identificar características de los Intérpretes

- Definición:
	- Ejecutan programas directamente, sin generar un programa objeto intermedio.
	- Aceptan programas en el lenguaje fuente (source program) y ejecutan cada instrucción inmediatamente después de analizarla.
    
- Funcionamiento:
	- Recuperan, analizan y ejecutan instrucciones una por una, sin compilar el programa completo.
	- No convierten programas fuente en código máquina antes de la ejecución.
    
- Cuándo son útiles:
	- Cuando los programadores trabajan en modo interactivo y necesitan resultados inmediatos.
	- Para programas de uso único o "descartables", donde la velocidad de ejecución no es prioritaria.
	- Para ejecutar instrucciones simples o con estructuras uniformes que se analizan rápidamente.

- Limitaciones:
	- Son más lentos que los compiladores (hasta 100 veces más en lenguajes de alto nivel).
	- No son ideales para programas de producción donde se prioriza la velocidad.

- Ejemplos de intérpretes:
	- Basic Interpreter: Analiza y ejecuta expresiones como asignaciones. Se basa en estructuras de control simples.
	- Lisp Interpreter: Trabaja con estructuras de datos complejas como árboles. Es capaz de generar código nuevo en tiempo de ejecución.
	- UNIX Shell Interpreter: Procesa comandos del sistema operativo en tiempo real.
	- SQL Interpreter: Ejecuta consultas de bases de datos directamente tras analizarlas.

---

### 3. Diferencias entre Compiladores vs Intérpretes

- Compiladores:
	- Traducción completa del programa fuente al código máquina antes de su ejecución.
	- Produce un programa objeto independiente.
	- Ejemplo: Un compilador Java genera bytecode que puede ejecutarse en cualquier máquina virtual Java (JVM).
	- Adecuados para producción, ya que la velocidad de ejecución es crítica.
	- Limitación: No se obtiene retroalimentación inmediata durante la escritura del código.

- Intérpretes:
	- Traducción y ejecución línea por línea del programa fuente.
	- No genera código objeto; los resultados son inmediatos.
	- Ejemplo: Un intérprete SQL ejecuta consultas sobre bases de datos directamente.
	- Adecuados para entornos interactivos o debugging.
	- Limitación: Velocidad mucho más lenta que los compiladores.
    

- Comparación ilustrada:
	- Un compilador de Java genera código x86 que puede ejecutarse en una máquina x86 directamente.
	- Un intérprete Lisp analiza estructuras de datos y ejecuta las instrucciones en el momento.

---

### 4. Explicar los diagramas Tombstone (agregar diagramas)

- Propósito:
	- Representar gráficamente la relación entre programas, máquinas, traductores e intérpretes.
	- Facilitar la comprensión visual de cómo los elementos interactúan.
    
- Tipos:
	- Programas:
		- Representados con round tombstones. Ejemplo: "sort" en Java o en código máquina.
	- Máquinas:
		- Representadas con pentagon tombstones. Ejemplo: x86, PPC, SPARC.
	- Traductores:
		- Representados con T-shaped tombstones. Indican el lenguaje fuente (S), el objetivo (T) y la implementación (L). Ejemplo: un traductor Java a x86 expresado en C.
	- Intérpretes:
		- Representados con rectangular tombstones. Indican el lenguaje fuente y el lenguaje en el que está implementado el intérprete.
    

- Reglas clave:
	- El programa debe estar expresado en el lenguaje que la máquina o el intérprete puede entender.
- Ejemplo:
	- Un programa en Java necesita un traductor para convertirse en código máquina x86 antes de ejecutarse en una máquina x86.
	- Si un programa en Java se intenta ejecutar en una máquina x86 directamente, fallará porque no hay una coincidencia de lenguajes.
    
- Ejemplo de diagrama avanzado:
	- La compilación en dos etapas (two-stage translation):
	- Primero, un programa Java se traduce a C usando un Java-to-C translator.
	- Luego, el programa en C se compila a código x86 con un C-to-x86 compiler.

