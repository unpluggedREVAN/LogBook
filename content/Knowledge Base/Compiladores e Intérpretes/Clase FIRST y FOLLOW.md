A continuación encontrarás **todo** el contenido en un formato plano/Markdown (sin caracteres “raros”) para que puedas copiar y pegar sin problemas en un archivo de texto. Incluye:

1. **Explicación y cálculo de FIRST y FOLLOW** (misma gramática que antes).
2. **Tablas** de FIRST y FOLLOW.
3. **Desarrollo del algoritmo de Análisis Descendente (Desplazamiento–Reducción)** para **dos** cadenas de ejemplo:
    - `-13 + 7 * 23`
    - `-(100*((1000)))`

---

# PARTE 1: Cálculo de FIRST y FOLLOW

Usaremos la gramática (tal como se presentó antes), que maneja expresiones, términos y factores. Sin embargo, aquí añadimos una producción para manejar el **menos unario** (opcional). Si no necesitas el menos unario, omite esa producción.

**Gramática base** (sin unario):

```
1) E -> E + T
2) E -> T
3) T -> T * F
4) T -> F
5) F -> ( E )
6) F -> id
```

**Producción adicional (para manejar menos unario), si se requiere:**

```
7) F -> -F
```

En muchos lenguajes se introduce esta producción para poder reconocer cadenas como `-13` o `-(100)`, etc. Aquí asumimos que la variable `id` también representa literales numéricos (como `13`, `100`, etc.).

---

## 1.1 FIRST de cada no terminal

**Definiciones informales de FIRST:**

- `FIRST(X)` es el conjunto de símbolos terminales que pueden aparecer como primer símbolo derivado de `X`.
- Si `X` es terminal, `FIRST(X) = { X }`.
- Si `X` es no terminal y `X -> Y1 Y2 ... Yk`, los terminales en `FIRST(Y1)` pasan a `FIRST(X)`, excepto si `Y1` puede derivar epsilon, en cuyo caso también consideramos `FIRST(Y2)`, etc.

Para la **gramática base**:

### FIRST(F)

```
F -> ( E )  
F -> id
( si se usa el menos unario: F -> -F )
```

- De `F -> ( E )`: el primer símbolo terminal posible es `(`.
- De `F -> id`: el primer símbolo terminal posible es `id`.
- (Opcional) De `F -> -F`: el primer símbolo terminal posible es `-`.

Por tanto, si **no** tenemos menos unario:

```
FIRST(F) = { (, id }
```

Si **sí** tenemos menos unario, entonces

```
FIRST(F) = { (, id, - }
```

### FIRST(T)

```
T -> T * F
T -> F
```

- La producción `T -> F` nos dice que `FIRST(T)` es al menos `FIRST(F)`.
- En `T -> T * F` hay recursión por la izquierda, pero `T` no deriva epsilon, así que no agrega nuevos terminales.

Por tanto,

```
FIRST(T) = FIRST(F) = { (, id }   (o { (, id, - } si hay unario)
```

### FIRST(E)

```
E -> E + T
E -> T
```

- `E -> T` implica `FIRST(E)` incluye `FIRST(T)`.
- `E -> E + T` es recursión por la izquierda; `E` no deriva epsilon.

Con ello,

```
FIRST(E) = FIRST(T) = { (, id }   (o { (, id, - } si hay unario)
```

#### Resumen FIRST (sin unario):

- FIRST(E) = { ( , id }
- FIRST(T) = { ( , id }
- FIRST(F) = { ( , id }

#### Resumen FIRST (con unario):

- FIRST(E) = { ( , id, - }
- FIRST(T) = { ( , id, - }
- FIRST(F) = { ( , id, - }

---

## 1.2 FOLLOW de cada no terminal

**Definiciones informales de FOLLOW(A) para un no terminal A:**

1. Si A es el símbolo inicial, entonces `$` (fin de cadena) está en FOLLOW(A).
2. Si en alguna producción aparece Aα, entonces todo `FIRST(α)` (excepto epsilon) está en FOLLOW(A).
3. Si en alguna producción aparece Aα y α ⇒* ε (alpha puede derivar epsilon), entonces todo lo que esté en FOLLOW(del no terminal del lado izquierdo) pasa también a FOLLOW(A).

Asumimos que el **símbolo inicial** es `E`.

### FOLLOW(E)

- Por ser símbolo inicial: `$` ∈ FOLLOW(E).
    
- Revisamos producciones donde E aparezca a la derecha:
    
    1. `F -> ( E )`: después de `E` hay `)`, por lo que `)` ∈ FOLLOW(E).
    2. `E -> E + T`: después del primer `E` hay `+`, por lo que `+` ∈ FOLLOW(E). (En `E -> T` no aparece `E` a la derecha).

Por tanto,

```
FOLLOW(E) = { $, ), + }
```

### FOLLOW(T)

- Dónde aparece T:
    
    1. `E -> E + T`: T está al final, por lo que todo lo de FOLLOW(E) pasa a FOLLOW(T).  
        Entonces añadimos { $, ), + } a FOLLOW(T).
    2. `T -> T * F`: T aparece antes de `* F`.  
        Entonces `*` ∈ FOLLOW(T) (porque `*` es terminal y aparece inmediatamente después de T).
    3. `E -> T`: T está al final, entonces lo de FOLLOW(E) va a FOLLOW(T).  
        (ya está contemplado el mismo conjunto).

Así,

```
FOLLOW(T) = { $, ), +, * }
```

### FOLLOW(F)

- Dónde aparece F:
    
    1. `T -> T * F`: después de F no hay nada, entonces FOLLOW(T) ⊆ FOLLOW(F).  
        (y conocemos FOLLOW(T) = { $, ), +, * }).
    2. `T -> F`: F está al final, entonces FOLLOW(T) ⊆ FOLLOW(F) también.

En consecuencia,

```
FOLLOW(F) = { $, ), +, * }
```

(No cambia si agregamos `F -> - F` respecto a FOLLOW, porque F sigue siendo no terminal y la producción no introduce símbolos tras F.)

#### Resumen FOLLOW

- FOLLOW(E) = { $, ), + }
- FOLLOW(T) = { $, ), +, * }
- FOLLOW(F) = { $, ), +, * }

---

## 1.3 Tablas FIRST y FOLLOW

Aquí tienes las tablas en **formato de texto**. Primero cada una por separado y luego unidas en una.

```
TABLA FIRST (sin unario)
---------------------------------
NoTerminal   FIRST
---------------------------------
E            { ( , id }
T            { ( , id }
F            { ( , id }

TABLA FOLLOW
-----------------------------------------
NoTerminal   FOLLOW
-----------------------------------------
E            { $ , ) , + }
T            { $ , ) , + , * }
F            { $ , ) , + , * }
```

Si usas el **menos unario**, simplemente agregas `-` a cada FIRST:

```
TABLA FIRST (con unario)
------------------------------------
NoTerminal   FIRST
------------------------------------
E            { ( , id , - }
T            { ( , id , - }
F            { ( , id , - }
```

Y la FOLLOW no cambia.

### Tabla conjunta (sin unario)

```
NoTerminal | FIRST         | FOLLOW
-----------------------------------------
E          | { ( , id }    | { $ , ) , + }
T          | { ( , id }    | { $ , ) , + , * }
F          | { ( , id }    | { $ , ) , + , * }
```

### Tabla conjunta (con unario)

```
NoTerminal | FIRST              | FOLLOW
-----------------------------------------
E          | { ( , id , - }    | { $ , ) , + }
T          | { ( , id , - }    | { $ , ) , + , * }
F          | { ( , id , - }    | { $ , ) , + , * }
```

---

# PARTE 2: Análisis Desplazamiento–Reducción (Shift–Reduce) para dos cadenas

Vamos a mostrar, **a grandes rasgos**, cómo se realiza un análisis tipo Desplazamiento–Reducción (Shift–Reduce) con la misma gramática (o la versión extendida con `F -> -F`). Supondremos que el lexer nos da tokens tales como `id = -13`, `id = 7`, `id = 23`, etc., de modo que el **signo menos** va “pegado” al número si se trata de un literal negativo. Si prefieres manejarlo como `-` seguido de `id`, entonces necesitas la producción `F -> -F`.

## 2.1 Cadena 1: `-13 + 7 * 23`

**Suposición**: El lexer produce los tokens (id = -13), `+`, (id = 7), `*`, (id = 23), `$`.  
En ese caso, el análisis (versión resumida) se ve así:

1. **Shift** el token `id(-13)`.
    
    - Pila: `id(-13)`
    - Entrada: `+ id(7) * id(23) $`
    - Acción: SHIFT
2. **Reduce** por `F -> id`.
    
    - Pila: `F`
    - Entrada: `+ id(7) * id(23) $`
    - Acción: REDUCE (F -> id)
3. **Reduce** por `T -> F`.
    
    - Pila: `T`
    - Entrada: `+ id(7) * id(23) $`
    - Acción: REDUCE (T -> F)
4. **Reduce** por `E -> T`.
    
    - Pila: `E`
    - Entrada: `+ id(7) * id(23) $`
    - Acción: REDUCE (E -> T)
5. **Shift** el token `+`.
    
    - Pila: `E +`
    - Entrada: `id(7) * id(23) $`
    - Acción: SHIFT
6. **Shift** el token `id(7)`.
    
    - Pila: `E + id(7)`
    - Entrada: `* id(23) $`
    - Acción: SHIFT
7. **Reduce** por `F -> id`.
    
    - Pila: `E + F`
    - Entrada: `* id(23) $`
    - Acción: REDUCE (F -> id)
8. **Reduce** por `T -> F`.
    
    - Pila: `E + T`
    - Entrada: `* id(23) $`
    - Acción: REDUCE (T -> F)
9. **Shift** el token `*`.
    
    - Pila: `E + T *`
    - Entrada: `id(23) $`
    - Acción: SHIFT
10. **Shift** el token `id(23)`.
    
    - Pila: `E + T * id(23)`
    - Entrada: `$`
    - Acción: SHIFT
11. **Reduce** por `F -> id`.
    
    - Pila: `E + T * F`
    - Entrada: `$`
    - Acción: REDUCE (F -> id)
12. **Reduce** por `T -> T * F`.
    
    - Pila: `E + T`
    - Entrada: `$`
    - Acción: REDUCE (T -> T * F)
13. **Reduce** por `E -> E + T`.
    
    - Pila: `E`
    - Entrada: `$`
    - Acción: REDUCE (E -> E + T)
14. **Shift** `$` (o ver directamente que se acepta).
    
    - Pila: `E $`
    - Entrada: _(vacía)_
    - Acción: Aceptar

La cadena se reconoce exitosamente.

---

## 2.2 Cadena 2: `-(100*((1000)))`

Dependiendo de cómo tratemos el `-`, hay **dos** maneras principales:

### Opción A: Tratar `-(` como un **token `id`** con valor `-(100`, algo poco usual

Lo normal es NO hacer esto, pero podríamos forzar si no queremos la producción para menos unario.  
(Esta forma es muy poco convencional, así que no se recomienda.)

### Opción B: Usar la producción unaria `F -> - F`

En este caso, nuestro lexer produce:

```
-   (   100   *   (   (   1000   )   )   )   $
```

Luego, el parser:

1. Shift `-`
    
    - Pila: `-`
    - Entrada: `( 100 * ( ( 1000 ) ) ) $`
    - Acción: SHIFT
2. Verificamos si podemos reducir por `F -> -F` de inmediato.
    
    - No, porque aun no tenemos un `F` a la derecha del `-`, así que **shift** el `(`.
3. Shift `(`
    
    - Pila: `- (`
    - Entrada: `100 * ( ( 1000 ) ) ) $`
    - Acción: SHIFT
4. Shift `100` (que sería `id`)
    
    - Pila: `- ( id`
    - Entrada: `* ( ( 1000 ) ) ) $`
    - Acción: SHIFT
5. Reduce `id` -> `F`
    
    - Pila: `- ( F`
    - Entrada: `* ( ( 1000 ) ) ) $`
    - Acción: REDUCE
6. Reduce `F` -> `T`
    
    - Pila: `- ( T`
    - Entrada: `* ( ( 1000 ) ) ) $`
    - Acción: REDUCE
7. Reduce `T` -> `E`
    
    - Pila: `- ( E`
    - Entrada: `* ( ( 1000 ) ) ) $`
    - Acción: REDUCE
8. Shift `*`
    
    - Pila: `- ( E *`
    - Entrada: `( ( 1000 ) ) ) $`
    - Acción: SHIFT
9. Shift `(`
    
    - Pila: `- ( E * (`
    - Entrada: `( 1000 ) ) ) $`
    - Acción: SHIFT
10. Shift `(`
    
    - Pila: `- ( E * ( (`
    - Entrada: `1000 ) ) ) $`
    - Acción: SHIFT
11. Shift `1000` (que es `id`)
    
    - Pila: `- ( E * ( ( id`
    - Entrada: `) ) ) $`
    - Acción: SHIFT
12. Reduce `id -> F`, luego `F -> T`, luego `T -> E` … (de manera análoga a lo anterior).
    
    - Pila tras esas reducciones: `- ( E * ( ( E`
    - Entrada: `) ) ) $`
13. Shift `)`
    
    - Pila: `- ( E * ( ( E )`
    - Entrada: `) ) $`
    - Acción: SHIFT
14. Reduce `( E ) -> F`
    
    - Pila: `- ( E * ( F`
    - Entrada: `) ) $`
15. Reduce `F -> T` y luego `T -> E` (similares pasos).
    
    - Pila: `- ( E * ( E`
    - Entrada: `) ) $`
16. Shift `)`
    
    - Pila: `- ( E * ( E )`
    - Entrada: `) $`
    - Acción: SHIFT
17. Reduce `( E ) -> F`
    
    - Pila: `- ( E * F`
    - Entrada: `) $`
18. Reduce `T -> T * F` o si todavía no se ha armado `T`, se hace la secuencia `F -> T`, `T -> T * F`.
    
    - Pila: `- ( T`
    - Entrada: `) $`
19. Reduce `T -> E`
    
    - Pila: `- ( E`
    - Entrada: `) $`
20. Shift `)`
    
    - Pila: `- ( E )`
    - Entrada: `$`
    - Acción: SHIFT
21. Reduce `( E ) -> F`
    
    - Pila: `- F`
    - Entrada: `$`
    - Acción: REDUCE
22. Ahora tenemos `- F` en pila, reducimos por `F -> - F`
    
    - Pila: `F`
    - Entrada: `$`
    - Acción: REDUCE (F -> -F)
23. Reduce `F -> T`, luego `T -> E` (o directamente, según las acciones).
    
    - Pila: `E`
    - Entrada: `$`
24. Shift `$` o se **acepta** la cadena.
    

Esta secuencia es larga, pero la idea es la misma: **desplazar** (shift) hasta que podamos **reducir**, aplicando las producciones.

---

## Resumen final

1. **Hemos recalculado** FIRST y FOLLOW para la gramática estándar de expresiones `E, T, F`.
2. **Mostramos** las tablas FIRST y FOLLOW en **texto plano**.
3. **Desarrollamos** el análisis Desplazamiento–Reducción para dos expresiones de ejemplo que involucran números (posibles negativos) y paréntesis anidados, aplicando los pasos de **shift** y **reduce** de manera típica.

Con esto tienes todo el **contenido copiable** en un formato (Markdown o texto plano) fácil de pegar en cualquier editor. ¡Éxitos!


---

Aquí tienes el **análisis desplazamiento-reducción** para ambas cadenas en un formato de tabla fácilmente copiable:

---

### **Cadena 1: `-13 + 7 * 23`**

**Tokens:** `(id = -13)`, `+`, `(id = 7)`, `*`, `(id = 23)`, `$`.

|**Pila**|**Entrada**|**Acción**|
|---|---|---|
|`$`|`id(-13) + id(7) * id(23) $`|Shift|
|`$id(-13)`|`+ id(7) * id(23) $`|Reduce F→idF \to id|
|`$F`|`+ id(7) * id(23) $`|Reduce T→FT \to F|
|`$T`|`+ id(7) * id(23) $`|Reduce E→TE \to T|
|`$E`|`+ id(7) * id(23) $`|Shift|
|`$E +`|`id(7) * id(23) $`|Shift|
|`$E + id(7)`|`* id(23) $`|Reduce F→idF \to id|
|`$E + F`|`* id(23) $`|Reduce T→FT \to F|
|`$E + T`|`* id(23) $`|Shift|
|`$E + T *`|`id(23) $`|Shift|
|`$E + T * id(23)`|`$`|Reduce F→idF \to id|
|`$E + T * F`|`$`|Reduce T→T∗FT \to T * F|
|`$E + T`|`$`|Reduce E→E+TE \to E + T|
|`$E`|`$`|Aceptar|

---

### **Cadena 2: `-(100*((1000)))`**

**Tokens:** `-`, `(`, `id(100)`, `*`, `(`, `(`, `id(1000)`, `)`, `)`, `)`, `$`.

|**Pila**|**Entrada**|**Acción**|
|---|---|---|
|`$`|`- ( id(100) * ( ( id(1000) ) ) ) $`|Shift|
|`$ -`|`( id(100) * ( ( id(1000) ) ) ) $`|Shift|
|`$ - (`|`id(100) * ( ( id(1000) ) ) ) $`|Shift|
|`$ - ( id(100)`|`* ( ( id(1000) ) ) ) $`|Reduce F→idF \to id|
|`$ - ( F`|`* ( ( id(1000) ) ) ) $`|Reduce T→FT \to F|
|`$ - ( T`|`* ( ( id(1000) ) ) ) $`|Reduce E→TE \to T|
|`$ - ( E`|`* ( ( id(1000) ) ) ) $`|Shift|
|`$ - ( E *`|`( ( id(1000) ) ) ) $`|Shift|
|`$ - ( E * (`|`( id(1000) ) ) ) $`|Shift|
|`$ - ( E * ( (`|`id(1000) ) ) ) $`|Shift|
|`$ - ( E * ( ( id(1000)`|`) ) ) $`|Reduce F→idF \to id|
|`$ - ( E * ( ( F`|`) ) ) $`|Reduce T→FT \to F|
|`$ - ( E * ( ( T`|`) ) ) $`|Reduce E→TE \to T|
|`$ - ( E * ( ( E`|`) ) ) $`|Shift|
|`$ - ( E * ( ( E )`|`) ) $`|Reduce F→(E)F \to (E)|
|`$ - ( E * ( F`|`) ) $`|Reduce T→FT \to F|
|`$ - ( E * T`|`) ) $`|Reduce T→T∗FT \to T * F|
|`$ - ( E`|`) ) $`|Shift|
|`$ - ( E )`|`) $`|Reduce F→(E)F \to (E)|
|`$ - F`|`$`|Reduce F→−FF \to -F|
|`$ F`|`$`|Reduce T→FT \to F|
|`$ T`|`$`|Reduce E→TE \to T|
|`$ E`|`$`|Aceptar|

---

### **Notas:**

1. **Cadena 1:** La cadena `-13 + 7 * 23` se acepta después de las reducciones correspondientes.
2. **Cadena 2:** La cadena `-(100*((1000)))` también se acepta después de incluir la producción F→−FF \to -F.

¿Te gustaría un desglose adicional o implementación en código? 😊