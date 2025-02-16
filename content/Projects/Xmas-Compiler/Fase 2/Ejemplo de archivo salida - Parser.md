
### **Ejemplo de Archivo de Salida para el Análisis Sintáctico**

```plaintext
Análisis Sintáctico - Resumen

1. Declaraciones (Tabla de Símbolos)
----------------------------------------------
Lexema         Tipo        Línea    Columna    Scope
------------------------------------------------------
_x_            int         2        5          global
_str_          string      3        5          global
_arr_          char[]      7        5          global
_mibool_       bool        12       1          global
_fl1_          float       6        5          global
_i_            int         16       19         loop

2. Asignaciones (Tabla de Símbolos Actualizada)
----------------------------------------------
Lexema         Tipo        Línea    Columna    Scope         Valor
---------------------------------------------------------------
_x_            int         5        3          global        10
_str_          string      5        5          global        "Hello World"
_arr_[2]       char        8        9          global        'c'
_mibool_       bool        12       17         global        true
_i_            int         16       19         loop          0

3. Estructuras de Control
----------------------------------------------
- If detectado: Condición: _x_ == 0 (Línea: 10, Columna: 1)
  - Bloque:
    - Return: 1 (Línea: 11, Columna: 5)
- For detectado: Inicialización: _i_ = 0, Condición: _i_ < 10, Incremento: _i_++ (Línea: 15, Columna: 1)

4. Errores Sintácticos
----------------------------------------------
- Error: Se esperaba ';' después de la declaración (Línea: 18, Columna: 12)
- Error: Se esperaba ')' en la condición del while (Línea: 20, Columna: 7)

5. Resumen General
----------------------------------------------
Estado del archivo: Inválido
Errores encontrados: 2
```

---

### **Estructura Detallada**

1. **Declaraciones:**
    
    - Cada declaración detectada se agrega a la tabla de símbolos.
    - Contiene el **lexema**, **tipo**, **línea**, **columna** y el **scope** (alcance del identificador).
2. **Asignaciones:**
    
    - Cada asignación detectada actualiza la tabla de símbolos con el valor asignado al identificador.
    - Contiene los mismos campos que las declaraciones, pero también incluye el **valor** asignado.
3. **Estructuras de Control:**
    
    - Detalla las estructuras como `if`, `for`, y `while` detectadas, con sus condiciones y bloques de ejecución.
4. **Errores Sintácticos:**
    
    - Lista los errores encontrados, incluyendo línea, columna y descripción.
5. **Resumen General:**
    
    - Indica si el archivo fuente es válido o inválido según la gramática y muestra un recuento de los errores encontrados.
