### 📑 **Plan de Ejecución: Fase 2 - Análisis Sintáctico**

La **Fase 2 del proyecto Xmas-Compiler** se centra en la implementación del **Análisis Sintáctico** mediante **CUP (Constructor de Parsers para Java)**. Esta fase validará la estructura gramatical de los programas escritos en el lenguaje navideño definido en la Fase 1.

El éxito de esta fase depende de una planificación detallada para evitar errores críticos como conflictos **Shift-Reduce** o **Reduce-Reduce**, los cuales pueden impedir la correcta generación del parser.

---

## **1. Objetivos Generales de la Fase 2**

1. **Implementar el Analizador Sintáctico** utilizando **CUP** para validar la estructura gramatical de los archivos fuente.
2. **Gestionar y recuperar errores sintácticos** mediante la técnica de **Recuperación en Modo Pánico**.
3. **Validar la Tabla de Símbolos**, indicando a cuál tabla pertenece cada token y qué información almacena.
4. **Generar reportes claros** sobre los errores encontrados durante el análisis sintáctico.
5. **Garantizar compatibilidad total** entre el **Lexer (Fase 1)** y el **Parser (Fase 2)**.

---

## **2. Análisis de Requerimientos**

### **2.1 Gramática del Lenguaje**

- Utilizar la gramática definida en el **Anexo I** del enunciado.
- Adaptar la gramática al formato adecuado para **CUP** (BNF extendido soportado por CUP).
- Validar reglas de producción para evitar **conflictos Shift-Reduce** y **Reduce-Reduce**.

### **2.2 Tabla de Símbolos**

- Desarrollar una estructura de datos para manejar tablas de símbolos.
- Cada token identificado por el lexer debe ser registrado con su información relevante (tipo, lexema, posición, valor, etc.).

### **2.3 Gestión de Errores**

- Implementar **Recuperación en Modo Pánico**, asegurando que el parser pueda continuar el análisis después de un error.
- Reportar los errores con información detallada (línea, columna, tipo de error).

### **2.4 Integración con el Lexer**

- El parser debe consumir tokens generados por el lexer sin errores de compatibilidad.
- Garantizar que los tokens estén correctamente mapeados con los terminales en el archivo `.cup`.

---

## **3. Fases de Desarrollo**

### **Fase 1: Preparación del Proyecto**

- **Revisión de la Gramática:** Adaptar la gramática definida en la Fase 1 al formato compatible con CUP.
- **Actualización del Lexer:** Validar que todos los tokens definidos en la gramática estén correctamente reconocidos por el lexer.
- **Preparación de Tablas de Símbolos:** Crear la estructura de la tabla de símbolos para el almacenamiento de tokens.

**Productos Entregables:**

- Gramática final validada.
- Lexer funcional con tokens completos.
- Tabla de símbolos inicial definida.

---

### **Fase 2: Implementación del Parser**

- **Definición de Terminales y No Terminales:** En el archivo `parser.cup`, definir todos los terminales y no terminales correspondientes a la gramática.
- **Reglas de Producción:** Escribir las reglas de producción en CUP, asegurando que estén ordenadas y estructuradas correctamente.
- **Manejo de Errores Sintácticos:** Implementar mecanismos de recuperación mediante bloques como:
    
    ```cup
    error ::= <manejo específico>
    ```
    
- **Pruebas Unitarias:** Probar cada sección de la gramática de forma aislada para detectar conflictos.

**Productos Entregables:**

- Archivo `parser.cup` funcional y libre de errores.
- Generación exitosa de los archivos `Parser.java` y `sym.java`.

---

### **Fase 3: Integración con el Sistema Principal**

- **Conexión entre Lexer y Parser:** Validar que el parser recibe correctamente los tokens desde el lexer.
- **Implementación del Test:** Actualizar la clase `Test.java` para incluir la funcionalidad de validación sintáctica.
- **Pruebas Funcionales:** Probar el sistema con archivos fuente válidos e inválidos.

**Productos Entregables:**

- Clase `Test.java` actualizada.
- Validación sintáctica integrada al menú del programa principal.

---

### **Fase 4: Generación de Reportes**

- **Reporte de Tokens:** Validar que el archivo `output_tokens.txt` sigue generándose correctamente.
- **Reporte de Errores:** Generar un archivo adicional que almacene errores sintácticos con información detallada.

**Productos Entregables:**

- Archivo `output_tokens.txt` con los tokens generados.
- Archivo `errors.txt` con los errores sintácticos encontrados.

---

## **4. Plan de Pruebas**

### **Pruebas Unitarias**

- Probar individualmente cada regla de producción en la gramática.
- Validar reglas específicas con casos simples (e.g., asignaciones, estructuras de control).

### **Pruebas de Integración**

- Probar la interacción entre el lexer y el parser, asegurando que los tokens sean correctamente interpretados.

### **Pruebas Funcionales**

- Crear archivos fuente válidos e inválidos para evaluar el desempeño del parser.

**Casos de Prueba:**

1. Declaraciones válidas e inválidas.
2. Estructuras de control anidadas.
3. Funciones con múltiples parámetros.
4. Errores sintácticos específicos (e.g., llaves sin cerrar, tokens inesperados).

---

## **5. Documentación**

- **Manual de Usuario:** Instrucciones claras sobre cómo ejecutar el programa y probar el análisis sintáctico.
- **Diseño del Programa:** Decisiones técnicas clave y algoritmos utilizados.
- **Pruebas de Funcionalidad:** Resultados y capturas de pantalla de los casos de prueba.
- **Análisis de Resultados:** Evaluación de los objetivos alcanzados.
- **Bitácora de Cambios:** Historial detallado de commits.

---

## **6. Entregables Finales**

1. **Archivos fuente:**
    
    - Lexer.jflex
    - Parser.cup
    - Test.java
    - Main.java
2. **Documentación Externa:**
    
    - Informe PDF con las secciones requeridas.
3. **Archivos de Salida:**
    
    - output_tokens.txt
    - errors.txt
4. **Repositorio GitHub:**
    
    - Repositorio actualizado y accesible para revisión.

---

## **7. Riesgos y Estrategias de Mitigación**

|**Riesgo**|**Impacto**|**Estrategia de Mitigación**|
|---|---|---|
|Conflictos Shift-Reduce|Alto|Revisar la gramática en cada fase para evitar ambigüedades.|
|Errores en la integración Lexer-Parser|Medio|Realizar pruebas unitarias constantes.|
|Mala gestión de errores sintácticos|Alto|Implementar correctamente la recuperación en modo pánico.|
