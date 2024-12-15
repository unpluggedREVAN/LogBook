# Comandos hasta ahora

## **1. Generar el Lexer (JFlex)**

Se toma el archivo `Lexer.jflex` de la carpeta `src/main/jflex` y genera el archivo `Lexer.java` en `src/main/java/ParserLexer`.

```bash
java -jar lib/jflex-full-1.9.1.jar -d src/main/java/ParserLexer src/main/jflex/Lexer.jflex
```

## **2. Generar el Parser (CUP)**

Se toma el archivo `parser.cup` de la carpeta `src/main/cup` y genera los archivos `Parser.java` y `sym.java` en `src/main/java/ParserLexer`.

```bash
java -jar lib/java-cup-11b.jar -destdir src/main/java/ParserLexer -parser Parser src/main/cup/parser.cup
```

## **3. Compilar los archivos Java**

Se compilan los archivos `.java` ubicados en `src/main/java/ParserLexer` y genera los archivos `.class` en `src/main/generated`.

```bash
javac -cp "lib/*" -d src/main/generated src/main/java/ParserLexer/*.java
```

## **4. Ejecutar el Programa de Prueba**

Ejecuta la clase principal `Test` ubicada en el paquete `ParserLexer`. Los archivos `.class` están en `src/main/generated`.

```bash
java -cp "lib/*;src/main/generated" ParserLexer.Test
```
