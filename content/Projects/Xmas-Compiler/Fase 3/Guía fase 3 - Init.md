
# **Guía Completa para la Fase 3: Análisis Semántico y Generación de CIPS MIPS**  

---

## **1. Preparación del Entorno**  
### **1.1. Estructura del Proyecto**  
Organiza tu proyecto para separar responsabilidades y facilitar la integración:  

src/  
├── ParserLexer/              # Archivos JFlex/CUP (parser.cup, lexer.jflex)  
├── Semantic/                 # Análisis semántico  
│   ├── SymbolTable.java      # Tabla de símbolos ampliada (scopes, tipos, funciones)  
│   ├── TypeChecker.java      # Validaciones de tipos y compatibilidad  
│   └── SemanticAnalyzer.java # Lógica central de verificaciones semánticas  
├── MIPS/                     # Generación de código  
│   ├── MIPSGenerator.java    # Emisión de instrucciones MIPS  
│   └── MIPSUtils.java        # Funciones auxiliares (etiquetas, temporales)  
├── Errors/                   # Manejo de errores  
│   └── ErrorHandler.java     # Reporte unificado de errores  
└── Main.java                 # Punto de entrada (lectura de archivos, ejecución)  


### **1.2. Archivos Nuevos vs. Existentes**  
- **Nuevos archivos**:  
  - `SemanticAnalyzer.java`, `TypeChecker.java`, `MIPSGenerator.java`: Contendrán toda la lógica nueva.  
  - **No modifiques `parser.cup` directamente**: Usa inyección de dependencias para conectar estas clases al parser.  
- **Archivos existentes**:  
  - `SymbolTable.java`: Amplíala para soportar funciones y scopes anidados.  
  - `Main.java`: Modifícalo para inicializar los nuevos componentes.  

---

## **2. Análisis Semántico**  
### **2.1. Ampliación de la Tabla de Símbolos**  
En `SymbolTable.java`:  
- **Campos nuevos**:  
  - `currentScope`: Pila para rastrear scopes (global → función → bloque).  
  - `functions`: Mapa para almacenar funciones (nombre, tipo de retorno, parámetros).  
- **Métodos clave**:  
  ```java  
  void enterScope(String scope);      // Entra a un nuevo ámbito (ej: función "main")  
  void exitScope();                   // Vuelve al ámbito anterior  
  void addFunction(String name, String returnType, List<String> paramTypes);  
  Symbol getSymbol(String name);      // Busca en todos los scopes accesibles  
  ```  

### **2.2. Validaciones Semánticas (Checklist)**  
#### **2.2.1. Declaraciones y Tipos**  
- **Variables no declaradas**:  
  - En cada uso de un identificador (asignación, expresión), verifica si existe en `SymbolTable`.  
  - **Ejemplo**:  
    ```java  
    // En SemanticAnalyzer.java  
    if (!symbolTable.exists(currentScope, variableName)) {  
      ErrorHandler.reportError("Variable '" + variableName + "' no declarada");  
    }  
    ```  

- **Variables duplicadas**:  
  - Al declarar una variable, verifica que no exista en el scope actual.  

- **Tipos incompatibles**:  
  - Implementa en `TypeChecker.java` reglas como:  
    - `int` puede asignarse a `float`, pero no viceversa.  
    - Operadores lógicos (`&&`, `||`) solo con `bool`.  

#### **2.2.2. Funciones**  
- **Función `main` única**:  
  - Al final del análisis, verifica que haya exactamente una función `main`.  

- **Parámetros y retorno**:  
  - Registra los parámetros de una función en `SymbolTable` al declararla.  
  - En llamadas a funciones, compara el número y tipo de argumentos con los parámetros declarados.  

#### **2.2.3. Estructuras de Control**  
- **Condiciones booleanas**:  
  - En `if`, `while`, `for`, valida que la expresión sea de tipo `bool`.  
  ```java  
  // En TypeChecker.java  
  public static void validateCondition(String type) {  
    if (!type.equals("bool")) {  
      ErrorHandler.reportError("La condición debe ser booleana");  
    }  
  }  
  ```  

- **Switch y Default**:  
  - Asegura que `default` esté solo al final del `switch`.  
  - Valida que los `case` sean del mismo tipo que la expresión del `switch`.  

---

## **3. Generación de Código MIPS**  
### **3.1. Diseño de `MIPSGenerator.java`**  
- **Atributos clave**:  
  ```java  
  private StringBuilder code = new StringBuilder();  // Código MIPS acumulado  
  private int labelCounter = 0;                       // Etiquetas únicas (ej: LOOP_1)  
  private int tempCounter = 0;                        // Registros temporales ($t0, $t1...)  
  ```  

- **Métodos esenciales**:  
  ```java  
  public String getLabel(String prefix) { return prefix + "_" + (labelCounter++); }  
  public String getTemp() { return "$t" + (tempCounter++); }  
  public void emit(String instruction) { code.append(instruction + "\n"); }  
  ```  

### **3.2. Estrategia de Generación**  
#### **3.2.1. Variables y Asignaciones**  
- **Variables globales**:  
  ```mips  
  .data  
  var1: .word 0     # int  
  var2: .float 0.0  # float  
  ```  
  - Emite esto al procesar declaraciones en el scope global.  

- **Asignaciones**:  
  - Traduce `a = b + c` a:  
    ```mips  
    lw $t0, b  
    lw $t1, c  
    add $t2, $t0, $t1  
    sw $t2, a  
    ```  

#### **3.2.2. Estructuras de Control**  
- **If-Else**:  
  ```mips  
  # Evaluar condición  
  beq $t0, $zero, ELSE_LABEL  
  # Bloque if  
  j END_IF  
  ELSE_LABEL:  
  # Bloque else  
  END_IF:  
  ```  

- **While Loop**:  
  ```mips  
  WHILE_START:  
  # Evaluar condición  
  beq $t0, $zero, WHILE_END  
  # Bloque while  
  j WHILE_START  
  WHILE_END:  
  ```  

#### **3.2.3. Funciones**  
- **Llamadas a función**:  
  ```mips  
  jal funcion_name  
  ```  
- **Manejo de parámetros**:  
  - Usa la pila (`$sp`) para pasar argumentos y almacenar registros.  

### **3.3. Syscalls para Entrada/Salida**  
- **Imprimir un entero**:  
  ```mips  
  li $v0, 1  
  lw $a0, <variable>  
  syscall  
  ```  
- **Leer un entero**:  
  ```mips  
  li $v0, 5  
  syscall  
  sw $v0, <variable>  
  ```  

---

## **4. Integración con el Parser Existente**  
### **4.1. Conectar Componentes**  
- **En `Main.java`**:  
  ```java  
  SymbolTable symbolTable = new SymbolTable();  
  SemanticAnalyzer semanticAnalyzer = new SemanticAnalyzer(symbolTable);  
  MIPSGenerator mipsGenerator = new MIPSGenerator();  
  Parser parser = new Parser(lexer, symbolTable, semanticAnalyzer, mipsGenerator);  
  ```  

- **Modificaciones en `parser.cup`**:  
  - Inyecta las clases en el constructor del parser.  
  - En cada regla, llama a los métodos de validación/generación.  
  ```cup  
  // Ejemplo en regla de asignación  
  asignacion ::= IDENTIFICADOR:id ASIGNA ... {  
    semanticAnalyzer.validateAssignment(id, expresionType);  
    mipsGenerator.emitAssignmentCode(id, expresionType);  
  }  
  ```  

### **4.2. Manejo de Errores**  
- **Centraliza los errores en `ErrorHandler.java`**:  
  ```java  
  public class ErrorHandler {  
    public static void reportError(String type, String message, int line, int column) {  
      System.err.printf("[%s] Línea %d, Col %d: %s\n", type, line, column, message);  
    }  
  }  
  ```  

---

## **5. Correcciones de la Fase 2**  
### **5.1. Errores en Expresiones Unarias**  
- **Problema**: No se reconocían negativos (ej: `-5`).  
- **Solución**:  
  - Modifica la regla de `expresion` en `parser.cup` para aceptar `-` como operador unario.  
  - En `TypeChecker.java`, valida que el operando sea `int` o `float`.  

### **5.2. Switch y Default**  
- **Problema**: `default` no estaba restringido al final.  
- **Solución**:  
  - En la regla de `switch_estructura`, fuerza a que `default` sea la última opción.  

---

## **6. Pruebas y Validación**  
### **6.1. Casos de Prueba Prioritarios**  
1. **Declaración de Función sin Retorno**:  
   ```java  
   void funcion() { ... }  
   // Debe generar error si se usa en una expresión.  
   ```  
2. **Acceso a Variable Fuera de Scope**:  
   ```java  
   { int a; }  
   a = 5;  // Error: variable no declarada  
   ```  
3. **Generación de Código para If-Else**:  
   - Verifica que las etiquetas en MIPS se generen correctamente.  

### **6.2. Herramientas**  
- **QtSpim**: Para ejecutar el código MIPS generado.  
- **MARS**: Alternativa si hay problemas con QtSpim.  

---

## **7. Entrega Final**  
### **7.1. Archivos a Incluir**  
- **Código Fuente**:  
  - Todos los archivos nuevos (`Semantic/`, `MIPS/`, `Errors/`).  
  - `parser.cup` y `lexer.jflex` actualizados.  
- **Documentación**:  
  - Manual de usuario actualizado (cómo compilar, ejecutar, y probar).  
  - Evidencia de pruebas (screenshots de QtSpim con salidas correctas).  

### **7.2. Recomendaciones Clave**  
- **Commits Atómicos**: Usa Git para guardar cambios pequeños y frecuentes.  
- **Pruebas Incrementales**:  
  - Primero valida el análisis semántico, luego la generación de MIPS.  
  - Verifica cada estructura (if, while, funciones) por separado.  
