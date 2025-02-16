# Observaciones Finales y Recomendaciones

1. **Arreglos solo de `rodolfo` y `cupido`**  
    El punto más importante a corregir para que sea “milimétrico” con la especificación es **restringir** la producción de arreglos a `rodolfo` o `cupido`. Actualmente tu gramática permitiría `bromista[10]`, `trueno[5]` o `cometa[2]`, que no están en la especificación.
    
    > **Solución**:
    > 
    > bnf
    > 
    > Copiar código
    > 
    > `declaracion ::=     tipo identificador finregalo   | tipo identificador entrega expresion finregalo   | (rodolfo | cupido) identificador abreempaque INTEGER_LITERAL cierraempaque finregalo`
    
    Esto hace que solo se permitan arreglos de `rodolfo` (int) y `cupido` (char).
    
2. **Retorno de funciones**
    
    - El enunciado dice que pueden retornar `entero, flotante, char o booleanos`. Sin mencionar string. Tu gramática deja que el `tipo` sea también `cometa (string)`. Decide si realmente quieres permitir funciones que retornen string.
    - Si quieres restringirlo, en `funcionDeclaracion` puedes hacer:
        
        bnf
        
        Copiar código
        
        `funcionDeclaracion ::=     (rodolfo | bromista | trueno | cupido) identificador parametros bloque`
        
    - O si te da igual y deseas permitir `string`, no hay problema.
3. **Sintaxis del for**
    
    - El enunciado no define explícitamente la sintaxis, pero tradicionalmente es `for (asignación; expresión; expresión)`. Tú has optado por comas:
        
        go
        
        Copiar código
        
        `estructuraFor ::= duende abreregalo asignacion COMA expresion COMA expresion cierraregalo bloque`
        
    - Es absolutamente válido si así lo quiere el lenguaje. Solo **confirma** que es tu intención.
4. **Verificación semántica de tipos**
    
    - La gramática no prohíbe, por ejemplo, usar `baltazar (expresionAritmetica)` donde la expresión no sea booleana, etc. Pero eso se suele chequear en semántica (fase 3). La BNF está bien mientras reconozca la estructura.
5. **Uso de `finregalo`**
    
    - Funciona como “punto y coma”; todo está consistente. Verifica que no haya lugar donde quieras forzar su presencia y no lo tengas, o viceversa.
6. **Posible Conflicto en Orden**
    
    - Si deseas que las funciones globales puedan ir antes o después de `_verano_`, tendrías que modificar `programa`. Tal como está escrito, todo lo que se llame `declaracionesGlobales` debe aparecer antes de `_verano_`.
7. **Identificadores**
    
    - Tu regla es coherente con “empiezan y terminan en `_`”. Solo verificar que no quieras permitir `_algo_1_`. De hecho, tu regla **sí** lo permite, pues `alfanumerico` se puede iterar.
    - Está correcto según el enunciado.