A continuación encontrarás **un análisis detallado de todos los puntos semánticos que faltan** y una **propuesta** de cómo implementarlos en el proyecto para que el análisis semántico quede mucho más completo.

> **Nota importante**: Muchos de estos chequeos requieren **almacenar tipos y estructura de las expresiones** en tiempo de parsing, idealmente con una representación intermedia (p.e., AST o atributos de gramática). En el ejemplo que te muestro, se ilustra **una versión expandida** del `SemanticAnalyzer` que cubre cada punto del checklist.
> 
> Sin embargo, si **no cuentas aún con un AST ni con atributos de tipos en tus acciones semánticas**, será necesario **extender tus reglas de CUP** para ir capturando esa información (por ejemplo, guardando el tipo resultante de cada `expresion` y pasando dichos tipos a los métodos de chequeo).
> 
> El código que verás es **una guía** para ilustrar cómo realizar cada validación. Dependiendo de tu implementación de gramática, podrías adaptarlo o inyectar parte de esta lógica en tu `SymbolTableManager` o directamente en las acciones semánticas del parser.

---

## 1. Checklist de verificación semántica faltante

La lista de la Fase 3 menciona varios puntos importantes que aún **no** están cubiertos en el proyecto. Los agrupo según el orden de tu propio checklist, añadiendo **cómo** podemos implementarlos:

### A) Declaraciones y Tipos de Datos

1. **Validar que todas las variables sean declaradas antes de ser usadas**
    
    - _Estado actual_: Parcialmente cubierto en `checkDeclaracionesYTipos()` al recorrer las asignaciones simples.
    - _Mejora_: Extender la verificación a _todos_ los usos de variables (no solo en asignaciones). Para ello, necesitaríamos un árbol de expresiones o bien recolectar todos los usos de IDs.
2. **Verificar que no existan variables con el mismo nombre dentro del mismo scope**
    
    - _Estado actual_: Ya se hace al llamar `addSimbolo`, que detecta duplicados y lanza `addSemanticError`.
3. **Asociar el tipo de dato a cada identificador en la tabla de símbolos**
    
    - _Estado actual_: Sí, se hace al declarar.
    - _Mejora_: Asegurarnos de actualizarlo correctamente en caso de arrays y funciones.
4. **Comprobar que los arreglos declarados tengan dimensiones válidas (≥ 1) y que estas sean `int`**
    
    - _Falta_: En el mid-rule action donde marcamos el array, convendría chequear si `idx` es entero y `> 0`.
5. **Confirmar que el programa tenga exactamente una función `main`**
    
    - _Falta_: Necesitamos un contador de `main` detectados. Si hay 0 o >1, error semántico.
6. **Asegurar que los parámetros de las funciones no se repitan dentro de su definición**
    
    - _Estado actual_: `addParamToCurrentFunction` hace una pequeña verificación, pero no 100% estricta.
    - _Mejora_: Verificar no solo `paramTypes` repetidas, sino que no repita el **nombre** del parámetro.
7. **Registrar correctamente los parámetros en la tabla de símbolos**
    
    - _Estado actual_: Sí, se añade el símbolo con el scope de la función.
8. **Comprobar que los valores asignados sean del tipo esperado por la variable receptora**
    
    - _Falta_*: Se revisa de forma muy básica si es `int` y la expresión contiene `.` => error. Falta un chequeo más riguroso (veremos debajo en “Expresiones”).
9. **Validar que los arreglos solo reciban elementos compatibles con su tipo**
    
    - _Falta_: Deberíamos, al hacer `assignArr(...)`, chequear tipo del array y tipo del `rhs`.

### B) Tipos de Expresiones

10. **Verificar que operadores aritméticos binarios (suma, resta, mul, div, etc.) operen sobre `int` o `float`**
    
    - _Falta_: Para ello, cada `expresion` en CUP debe devolver un `type` (p.e., `"int"`, `"float"`, `"bool"`, ...). Luego, al ver `expr -> expr + expr`, si `type(e1)` es `bool` y `type(e2)` es `int`, error.
11. **Validar que el operador unario negativo opere solo sobre valores `int` o `float`**
    
    - _Falta_: Mismo mecanismo: si hacemos `- exp` y el tipo no es `int` o `float`, error.
12. **Detectar divisiones con denominador cero**
    
    - _Falta_: Requiere evaluar si la segunda expresión es un literal `0` o si en tiempo de análisis podemos determinarlo. Se puede hacer un chequeo parcial si la expresión es una constante literal `0`.
    - _En casos más avanzados_, se evalúan constantes o se advierte si no se puede saber en tiempo de compilación.
13. **Asegurar que operadores relacionales `<`, `>`, `==`, etc. sean usados entre tipos compatibles**
    
    - _Falta_: Por lo general, `<` y `>` aplican a `int` o `float`. `==` y `!=` pueden aplicar a varios tipos, siempre que sean _idénticos_ entre sí o compatibles.
14. **Confirmar que los operadores lógicos `&&`, `||` solo trabajen con `bool`**
    
    - _Falta_: Verificar que `e1` y `e2` sean de tipo `bool`.
15. **Verificar que el operador lógico `!` opere únicamente sobre valores `bool`**
    
    - _Falta_.
16. **Validar que los índices de los arreglos sean de tipo `int`**
    
    - _Falta_: Al usar `arreglo[exp]`, asegurarnos de que `exp` sea `int`.
17. **Comprobar que los accesos a arreglos no excedan los límites**
    
    - _Falta_: Esto en compiladores se hace generalmente en _runtime_ (inserción de checks). Se puede dar un _warning_ si detectas un literal que excede el tamaño.

### C) Control de Tipos Compuestos

18. **Verificar que las condiciones de `if`, `while`, `for` sean expresiones de tipo `bool`**
    
    - _Falta_: Requiere la info de tipo de cada condición.
19. **Asegurar que las expresiones en `case` de un `switch` sean del mismo tipo que la expresión evaluada**
    
    - _Falta_: Requiere saber el tipo del `switch(expr)` y compararlo con cada `case`.
20. **Confirmar que el tipo de retorno declarado en una función sea consistente con los `return`**
    
    - _Falta_: Podemos llevar un _flag_ en el `SymbolData` de la función con su `returnType`, e ir revisando los `return expr`.
    - _Si es una función no `void`_: cada `return` debe concordar con el tipo.
21. **Validar que todas las rutas de ejecución retornen un valor cuando la función no sea de tipo `void`**
    
    - _Falta_: Requiere un análisis de flujos. Al menos, un check simplificado de que la función contenga un `return` en su bloque.
22. **Asegurar que el número y tipo de parámetros en una llamada a función coincidan con la definición**
    
    - _Falta_: Al ver `expresion -> IDENTIFICADOR(args)`, se revisa si ese ID es función, cuántos parámetros y sus tipos.

### D) Alcance y Contexto

23. **Verificar que las variables y funciones se usen solo dentro de ámbitos válidos**
    
    - _Estado actual_: Ya se buscan recursivamente en scopes.
    - _Mejora_: Podríamos generar error si se quiere acceder a una función/variable _privada_ (en caso de un lenguaje con modificadores) o algo similar.
24. **Comprobar que las variables globales no sean sobrescritas en ámbitos locales sin justificación**
    
    - _Falta_: Se puede advertir si el mismo nombre existe global y local.
25. **Validar que sentencias `return` solo aparezcan dentro de funciones**
    
    - _Falta_: Si se detecta `return` en scope `global`, error.
26. **Confirmar que `break` y `continue` solo se usen dentro de estructuras de control**
    
    - _Falta_: Se hace un _stack_ de bucles y switch para verificar si un `break/continue` está dentro de uno.

---

## 2. Ejemplo de Implementación Ampliada

### 2.1. Extender el **parser** para capturar tipos de expresiones

La gramática actual no está devolviendo el **tipo** de cada producción de `expresion`. Se usa `RESULT = ...` para la cadena, pero no un atributo semántico. **Necesitamos** (idealmente) algo así:

```java
expresion ::= 
    expresion:e1 SUMA expresion:e2
    {: 
       // ...
       // Chequeo de tipos: e1.type y e2.type deben ser int o float
       if ((!e1.type.equals("int") && !e1.type.equals("float"))
            || (!e2.type.equals("int") && !e2.type.equals("float"))) {
           manager.addSemanticError("Operador '+' solo aplica a int o float");
           RESULT.type = "error"; // Propagar error
       } else {
           // Si uno de los dos es float, el resultado es float; sino int
           if (e1.type.equals("float") || e2.type.equals("float")) 
               RESULT.type = "float";
           else 
               RESULT.type = "int";
       }
       // ...
       RESULT.valueString = "(" + e1.valueString + " + " + e2.valueString + ")";
    :}
    ;
```

> **OJO**: Esto implica cambiar la definición de símbolos en `.cup` para que `expresion` tenga un **objeto** conteniendo el `type`, `valueString`, etc.  
> Sin embargo, si no haces esta atribución en CUP, no podrás hacer chequeos exhaustivos de tipos en tiempo de parsing. **Es la forma más limpia** para un compilador real.

### 2.2. Nuevo `SemanticAnalyzer` con chequeos extra

Suponiendo que **hemos logrado** (o _simulamos_) obtener el tipo de cada variable y de cada expresión (aunque sea parcial), podemos añadir métodos que realicen los chequeos. Por ejemplo:

```java
package ParserLexer;

import java.util.List;
import java.util.Set;
import java.util.HashSet;

public class SemanticAnalyzer {
    private SymbolTableManager symbolTable;
    private List<String> asignacionesSimples;

    // Para contar cuántas funciones main hay
    private int mainFunctionsCount = 0;

    // Para detectar si estamos dentro de un bucle o switch (para break/continue)
    private int loopOrSwitchNesting = 0; 

    public SemanticAnalyzer(SymbolTableManager stm, List<String> assigns) {
        this.symbolTable = stm;
        this.asignacionesSimples = assigns;
    }

    public void runSemanticChecks() {
        System.out.println("\n[SEMANTIC] Iniciando chequeos semánticos...");

        // Chequeos existentes:
        checkDeclaracionesYTipos();
        checkTiposExpresiones();

        // --- Chequeos nuevos o ampliados ---
        checkUniqueMain();
        checkArrayDimensions();
        checkFunctionParams();
        // etc.

        System.out.println("[SEMANTIC] Análisis semántico terminado.\n");
    }

    /**
     * Verificar que haya exactamente 1 función main.
     * Se pueden buscar en la tabla de símbolos las funciones
     * cuyo lexema sea "main".
     */
    private void checkUniqueMain() {
        int foundMains = 0;
        // Recorremos todos los scopes
        for (var entry : symbolTable.getTablaSimbolos().entrySet()) {
            for (var sd : entry.getValue()) {
                // Chequear si es función y se llama main
                if (sd.isFunction && sd.lexeme.equals("main")) {
                    foundMains++;
                }
            }
        }
        if (foundMains == 0) {
            symbolTable.addSemanticError("No se declaró la función 'main'. Debe existir exactamente una.");
        }
        else if (foundMains > 1) {
            symbolTable.addSemanticError("Existen múltiples funciones 'main' ("+foundMains+"). Debe haber solo una.");
        }
    }

    /**
     * Chequear que los arreglos tengan dimensiones válidas.
     * Si en la tabla un símbolo es array, arraySize debe ser >= 1.
     */
    private void checkArrayDimensions() {
        for (var entry : symbolTable.getTablaSimbolos().entrySet()) {
            for (var sd : entry.getValue()) {
                if (sd.isArray && sd.arraySize < 1) {
                    symbolTable.addSemanticError("Array '"+sd.lexeme+"' con dimensión inválida: "+sd.arraySize);
                }
            }
        }
    }

    /**
     * Verificar que los parámetros de cada función no se repitan 
     * en nombre y sean consistentes en tipos.
     */
    private void checkFunctionParams() {
        // Recorremos la tabla de símbolos
        for (var entry : symbolTable.getTablaSimbolos().entrySet()) {
            for (var sd : entry.getValue()) {
                // Si es función, revisamos sus parámetros
                if (sd.isFunction) {
                    Set<String> seenParamNames = new HashSet<>();
                    for (String pt : sd.paramTypes) {
                        // pt vendría en formato "tipo:paramName"
                        String[] parts = pt.split(":");
                        if (parts.length == 2) {
                            String pType = parts[0];
                            String pName = parts[1];
                            if (seenParamNames.contains(pName)) {
                                symbolTable.addSemanticError("Parámetro repetido '" + pName
                                        + "' en la función '" + sd.lexeme + "'");
                            } else {
                                seenParamNames.add(pName);
                            }
                            // Chequear si pType es válido (int, float, bool, etc.)
                            if (!isValidType(pType)) {
                                symbolTable.addSemanticError("Tipo de parámetro inválido '"+pType
                                        +"' en la función '"+sd.lexeme+"'");
                            }
                        }
                    }
                }
            }
        }
    }

    private boolean isValidType(String t) {
        // Asumiendo un set de tipos básicos
        return t.equals("int") || t.equals("float") 
            || t.equals("bool") || t.equals("char") 
            || t.equals("string");
    }

    // -----------------------------------------------------------------
    // Métodos existentes (ejemplo con mejoras)
    // -----------------------------------------------------------------

    private void checkDeclaracionesYTipos() {
        // Tal como ya existía
        for (String asig : asignacionesSimples) {
            String varName = parseVarFromAssign(asig);
            SymbolTableManager.SymbolData sd = symbolTable.findSymbolRecursive(varName);
            if (sd == null) {
                symbolTable.addSemanticError("Variable '" + varName
                        + "' usada sin declarar. En: " + asig);
            }
        }
    }

    private void checkTiposExpresiones() {
        // Ejemplo: si es int y la expresión contiene '.', da error
        // Este método es muy básico, habría que ampliar con un 
        // procesamiento de AST para un chequeo real de tipos.
        for (String asig : asignacionesSimples) {
            String var = parseVarFromAssign(asig);
            String expr = parseExprFromAssign(asig);

            SymbolTableManager.SymbolData sd = symbolTable.findSymbolRecursive(var);
            if (sd == null) continue; // ya reportado

            // Check muy simplón:
            if (sd.type.equals("int") && expr.contains(".")) {
                symbolTable.addSemanticError("Asignando float a variable int ("+ var + ") en " + asig);
            }

            // Podrías añadir un mini parse de la expresión
            // para detectar divisiones por 0, etc.
            if (expr.contains("/0")) {
                symbolTable.addSemanticError("División por cero en la expresión: " + expr);
            }
        }
    }

    // -----------------------------------------------------------------
    // Utilidades
    // -----------------------------------------------------------------
    private String parseVarFromAssign(String asigStr) {
        int openPar = asigStr.indexOf("(");
        int eq = asigStr.indexOf("=");
        if (openPar < 0 || eq < openPar) return "???";
        return asigStr.substring(openPar + 1, eq).trim();
    }

    private String parseExprFromAssign(String asigStr) {
        int eq = asigStr.indexOf("=") + 1;
        int closePar = asigStr.lastIndexOf(")");
        if (eq < 1 || closePar < eq) return "";
        return asigStr.substring(eq, closePar).trim();
    }

}
```

#### Comentarios sobre esta ampliación:

- El método `checkUniqueMain()` localiza todas las funciones llamadas `main` y **lanza error** si no hay exactamente una.
- El método `checkArrayDimensions()` revisa que `sd.arraySize >= 1`.
- El método `checkFunctionParams()` verifica que no se repitan nombres de parámetros y que los tipos sean válidos.
- Podrías añadir métodos equivalentes para:
    - `checkReturnTypesInFunctions()`
    - `checkSwitchCaseTypes()`
    - `checkBooleanConditions()` para `if`, `while`, `for`.
    - `checkBreakContinueScope()` usando un _stack_ que incremente en `while_estructura`, `for_estructura`, `switch_estructura` y decremente al salir.

> **Reitero**: Para la mayoría de chequeos de tipos (operadores binarios, unarios, etc.), necesitas un árbol de expresiones o atributos que devuelvan el tipo de cada subexpresión, pues en la implementación actual solo estás guardando la _cadena_ que representa la expresión (`RESULT = "e1 + e2"`), pero no su tipo. Sin esa información **no** se puede saber (por ejemplo) si `e1` es `int` y `e2` es `bool`.

---

## 3. Resumen de lo que “falta” para un análisis semántico completo

27. **Atributos de tipos en la gramática**  
    Para cada regla de `expresion`, hay que computar y devolver su tipo (y no solo una cadena).
28. **Chequeos de compatibilidad de operadores** (suma, resta, comparaciones, lógicos, etc.).
29. **Verificación de tipos en arrays** (índice y contenido).
30. **Chequeos extra de control** (que `break`/`continue` estén dentro de bucles o switch).
31. **Control del retorno de funciones** (tipo esperado, si existen rutas sin `return`, etc.).
32. **Control estricto de `main`** (exactamente uno).
33. **Control de duplicación de variables y de parámetros**.
34. **Validación de usos de funciones** (cantidad y tipo de parámetros en llamadas).

Con estas incorporaciones, tu análisis semántico se acercaría mucho a la **lista completa** de requisitos de la Fase 3.

---

## 4. Conclusión

En resumen, **lo que falta** es:

- **Extender las reglas de la gramática** para llevar un tipo en cada `expresion`.
- **Crear/Agregar métodos** en el `SemanticAnalyzer` (o en acciones semánticas) para cada uno de los puntos del checklist:
    1. Declaraciones y tipos (arrays, duplicados, existencia de `main`, etc.).
    2. Operaciones entre tipos (aritméticos, relacionales, lógicos).
    3. Control de flujo (tipo de condición, `break`, `continue`, retorno de funciones).
    4. Chequeos de parámetros en definición y uso de funciones.

La sección de código anterior es un **ejemplo de cómo** podrías acomodar estos chequeos en tu `SemanticAnalyzer`. Adicionalmente, deberás **adaptar** tu parser para que, **en tiempo de construcción de las expresiones**, conserves la información de tipos y así puedas hacer validaciones mucho más finas.

---

¡Con esto tendrías **un análisis semántico mucho más completo** y alineado con la lista de la Fase 3!