# **Resumen del Proceso de Compilación**

A continuación, se detallan las fases del proceso de compilación teniendo en cuenta explicaciones adicionales sobre coerciones y código de tres direcciones.

---

## **1. Análisis Léxico (Lexical Analysis)**

- **Función**:
  - Es la primera fase del compilador.
  - Convierte el flujo de caracteres del programa fuente en secuencias significativas llamadas *lexemes*.
  - Produce *tokens* para ser usados en la fase de análisis sintáctico.

- **Salida**:
  - Los tokens se representan como `(token-name, attribute-value)`:
    - *token-name*: Nombre abstracto del tipo de lexema (por ejemplo, `id` para identificadores, `number` para literales numéricos).
    - *attribute-value*: Referencia a la tabla de símbolos donde se almacena información sobre el lexema (como nombre, tipo, valor, etc.).

- **Ejemplo**:
  - Código fuente: `position = initial + rate * 60`.
  - Lexemas: `position`, `=`, `initial`, `+`, `rate`, `*`, `60`.
  - Tokens generados: `(id, 1) (=) (id, 2) (+) (id, 3) (*) (60)`.

- **Notas**:
  - Los espacios en blanco y comentarios son ignorados.
  - Se actualiza la tabla de símbolos con detalles como nombres y tipos de identificadores.

---

## **2. Análisis Sintáctico (Syntax Analysis)**

- **Función**:
  - Construye un árbol sintáctico (*syntax tree*) que representa la estructura gramatical del programa.
  - Garantiza que el código cumple con las reglas gramaticales del lenguaje.

- **Salida**:
  - Árbol sintáctico donde:
    - Los nodos interiores representan operadores (como `+`, `*`, `=`).
    - Las hojas representan identificadores o valores literales.

- **Ejemplo**:
  - Para `position = initial + rate * 60`, el árbol sintáctico asegura que:
    1. La multiplicación (`rate * 60`) tiene prioridad sobre la suma (`+`).
    2. La asignación (`=`) es la operación principal.
  - Árbol resultante:
    ```plaintext
                     =
                  /     \
           position      +
                       /   \
                initial     *
                           /   \
                      rate      60
    ```

---

## **3. Análisis Semántico (Semantic Analysis)**

- **Función**:
  - Verifica la coherencia semántica del programa:
    - Compatibilidad de tipos.
    - Reglas semánticas específicas del lenguaje (ej. un índice de array debe ser un entero).
  - Introduce conversiones automáticas de tipos (*coercions*) cuando es necesario.

- **Salida**:
  - Árbol sintáctico decorado con información de tipos y conversiones necesarias.
  - Se actualiza la tabla de símbolos con tipos y conversiones realizadas.

- **Ejemplo de Coerción**:
  - Si `rate` es un *float* y `60` es un *int*, se inserta un nodo `inttofloat` en el árbol:
    ```plaintext
                     =
                  /     \
           position      +
                       /   \
                initial     *
                           /   \
                      rate      inttofloat
                                    |
                                   60
    ```

- **Notas**:
  - Este paso incluye verificación de tipos, coerciones y anotaciones para facilitar la generación de código intermedio.

---

## **4. Generación de Código Intermedio (Intermediate Code Generation)**

- **Función**:
  - Convierte el árbol sintáctico en una representación intermedia de bajo nivel (como *three-address code*).
  - Divide expresiones complejas en operaciones simples.

- **Propiedades del Código de Tres Direcciones**:
  - Cada instrucción tiene un máximo de tres operandos.
  - Utiliza variables temporales para almacenar resultados intermedios.
  - Es independiente de máquina.

- **Ejemplo**:
  - Para `position = initial + rate * 60`, el código sería:
    ```plaintext
    t1 = inttofloat(60)  ; Convierte 60 de entero a flotante.
    t2 = rate * t1       ; Multiplica rate por el flotante 60.
    t3 = initial + t2    ; Suma initial con el resultado de la multiplicación.
    position = t3        ; Asigna el resultado final a position.
    ```

---

## **5. Optimización de Código (Code Optimization)**

- **Función**:
  - Mejora el código intermedio para generar código máquina más eficiente.
  - Puede reducir el tiempo de ejecución, el consumo de energía o el tamaño del programa.

- **Tipos**:
  - **Independiente de Máquina**:
    - Ejemplo: Elimina operaciones redundantes, como `inttofloat(60)` reemplazado por `60.0`.
  - **Dependiente de Máquina**:
    - Ajusta el código para aprovechar características específicas del hardware, como registros.

- **Ejemplo**:
  - Optimización del ejemplo anterior:
    ```plaintext
    t1 = rate * 60.0
    position = initial + t1
    ```

---

## **6. Generación de Código (Code Generation)**

- **Función**:
  - Traduce el código intermedio al lenguaje objetivo, generalmente código máquina.
  - Selecciona registros y direcciones de memoria para almacenar variables.

- **Ejemplo**:
  - Código máquina para el ejemplo:
    ```plaintext
    LDF R2, rate          ; Carga rate en el registro R2.
    MULF R2, R2, #60.0    ; Multiplica R2 por 60.0.
    LDF R1, initial       ; Carga initial en el registro R1.
    ADDF R1, R1, R2       ; Suma R1 con R2.
    STF position, R1      ; Almacena el resultado en position.
    ```

---

## **7. Administración de la Tabla de Símbolos (Symbol-Table Management)**

- **Función**:
  - Registra información sobre los identificadores (nombres, tipos, alcance, etc.).
  - Es accesible para todas las fases del compilador.

- **Ejemplo**:
  - Para `position = initial + rate * 60`, la tabla incluiría:
    ```
    Identificador   Tipo       Dirección   Alcance
    position        float      0x0010      global
    initial         float      0x0014      global
    rate            float      0x0018      global
    ```

---

## **Conclusión**

El proceso de compilación es un flujo estructurado de fases que convierten el código fuente en un programa ejecutable eficiente. Cada etapa (léxico, sintáctico, semántico, intermedio, optimización, generación de código) aporta una transformación crucial, mientras que herramientas como las coerciones y el código de tres direcciones permiten gestionar la complejidad y mejorar la eficiencia del compilador.

## Imágenes a tener en cuenta

![[Pasted image 20241210113825.png]]

![[Pasted image 20241210113846.png]]

![[Pasted image 20241210114203.png]]