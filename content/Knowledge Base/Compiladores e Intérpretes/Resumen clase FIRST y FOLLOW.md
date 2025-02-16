**Resumen Completo: Teoría y Práctica de Análisis Léxico y Sintáctico**

1. **Conceptos Fundamentales**  
    El análisis léxico y sintáctico son fases importantes en la construcción de un compilador o intérprete:

- Análisis Léxico: Convierte el código fuente en una secuencia de tokens.
- Análisis Sintáctico: Verifica la estructura del código fuente para asegurarse de que cumpla con la gramática del lenguaje.

---

2. **Gramáticas Libres de Contexto (CFG)**  
    Las CFG (Context-Free Grammar) definen los lenguajes utilizados por los compiladores.

- Producciones: Reglas de la forma A -> alfa, donde A es un no terminal y alfa es una combinación de terminales y no terminales.
- Símbolo inicial: Es el no terminal a partir del cual se generan las cadenas válidas.
- Terminales: Los "literales" del lenguaje.
- No terminales: Representan constructos abstractos como expresiones o factores.

Ejemplo de gramática:

```
E -> E + T | T  
T -> T * F | F  
F -> (E) | id
```

---

3. **Análisis Descendente**  
    Este tipo de análisis construye el árbol sintáctico de arriba hacia abajo, comenzando con el símbolo inicial.

3.1 Descenso Recursivo:

- Utiliza funciones recursivas para cada no terminal.
- Puede requerir backtracking para probar diferentes producciones.
- Fácil de implementar, pero ineficiente con gramáticas ambiguas o recursión izquierda.

3.2 Análisis Predictivo (LL(1)):

- No requiere backtracking.
- Utiliza los conjuntos FIRST y FOLLOW para construir una tabla LL(1).
- Una gramática es LL(1) si, para dos producciones A -> alfa y A -> beta, se cumple:  
    a. FIRST(alfa) y FIRST(beta) no tienen intersección.  
    b. Si epsilon está en FIRST(beta), entonces FIRST(alfa) y FOLLOW(A) no tienen intersección.

---

4. **Cálculo de FIRST y FOLLOW**  
    Los conjuntos FIRST y FOLLOW son esenciales para construir analizadores predictivos.

FIRST: Contiene los terminales que pueden aparecer al principio de las cadenas derivadas de un no terminal.

- Si X es un terminal, FIRST(X) = {X}.
- Si X -> epsilon, entonces epsilon pertenece a FIRST(X).
- Si X -> Y1 Y2...Yk, se incluyen los elementos de FIRST(Y1), y si epsilon está en FIRST(Y1), se agrega FIRST(Y2), y así sucesivamente.

FOLLOW: Contiene los terminales que pueden aparecer inmediatamente después de un no terminal en una derivación.

- Si A es el símbolo inicial, FOLLOW(A) incluye $.
- Si A -> alfa B beta, entonces los elementos de FIRST(beta) se agregan a FOLLOW(B), excepto epsilon.
- Si epsilon está en FIRST(beta), entonces FOLLOW(A) se agrega a FOLLOW(B).

---

5. **Gramáticas LL(1)**

- Son un subconjunto de gramáticas libres de contexto que pueden analizarse sin backtracking.
- Utilizan una tabla de análisis predictivo para decidir qué producción usar.

Ejemplo de tabla para la gramática:

```
E -> E + T | T  
T -> T * F | F  
F -> (E) | id
```

|No Terminal|Token Entrada|Producción|
|---|---|---|
|E|id|E -> T|
|E|(|E -> T|
|T|id|T -> F|
|T|(|T -> F|
|F|id|F -> id|
|F|(|F -> (E)|

---

6. **Análisis Ascendente (Shift-Reduce)**  
    El análisis ascendente construye el árbol sintáctico desde las hojas hasta la raíz.

- Shift-Reduce: Utiliza una pila para realizar desplazamientos y reducciones.
- Pasos básicos:
    1. Desplazar el token actual a la pila.
    2. Reducir si los símbolos en la cima de la pila coinciden con el lado derecho de una producción.
    3. Repetir hasta aceptar la cadena o detectar un error.

Ejemplo de acciones:

|Estado|Token Entrada|Acción|
|---|---|---|
|0|id|Shift, ir a estado 5|
|5|+|Reduce F -> id|

---

7. **Tipos de Análisis LR**  
    LR (Left-to-right, Rightmost derivation) analiza de izquierda a derecha produciendo derivaciones por la derecha.

Tipos principales:

- LR(1): Usa un símbolo de lookahead para decidir las acciones. Es poderoso pero genera tablas grandes.
- SLR (Simple LR): Usa los conjuntos FOLLOW para decidir reducciones. Es más simple pero menos potente.
- LALR (Lookahead LR): Combina estados equivalentes de LR(1) para reducir el tamaño de las tablas. Es el tipo más usado.

---

8. **Ejemplo Práctico: Shift-Reduce**

Cadena 1: `-13 + 7 * 23`  
Tokens: id(-13), +, id(7), *, id(23), $

|Pila|Entrada|Acción|
|---|---|---|
|$|id(-13) + id(7) * id(23) $|Shift|
|$id(-13)|+ id(7) * id(23) $|Reduce F -> id|
|$F|+ id(7) * id(23) $|Reduce T -> F|
|$T|+ id(7) * id(23) $|Reduce E -> T|
|$E|+ id(7) * id(23) $|Shift|
|$E +|id(7) * id(23) $|Shift|
|$E + id(7)|* id(23) $|Reduce F -> id|
|$E + F|* id(23) $|Reduce T -> F|
|$E + T|* id(23) $|Shift|
|$E + T *|id(23) $|Shift|
|$E + T * id(23)|$|Reduce F -> id|
|$E + T * F|$|Reduce T -> T * F|
|$E + T|$|Reduce E -> E + T|
|$E|$|Aceptar|

Cadena 2: `-(100*((1000)))`  
Tokens: -, (, id(100), *, (, (, id(1000), ), ), ), $

|Pila|Entrada|Acción|
|---|---|---|
|$|- ( id(100) * ( ( id(1000) ) ) ) $|Shift|
|$ -|( id(100) * ( ( id(1000) ) ) ) $|Shift|
|$ - (|id(100) * ( ( id(1000) ) ) ) $|Shift|
|$ - ( id(100)|* ( ( id(1000) ) ) ) $|Reduce F -> id|
|$ - ( F|* ( ( id(1000) ) ) ) $|Reduce T -> F|
|$ - ( T|* ( ( id(1000) ) ) ) $|Reduce E -> T|

(Continúa hasta aceptar la cadena con pasos similares al ejemplo anterior).

---

9. **CUP y Analizadores LALR(1)**  
    CUP (Constructor of Useful Parsers) es una herramienta que genera analizadores sintácticos basados en gramáticas LALR(1).

Pasos básicos:

1. Escribir la gramática en un archivo .cup.
2. Definir tokens y reglas de producción.
3. Generar el analizador e integrarlo con un lexer como JFlex.

---

