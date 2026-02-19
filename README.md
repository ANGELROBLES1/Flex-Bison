# Flex-Bison

# Requerimientos 
Requerimos los siguientes archivos ubicados en la misma carpeta:

*Calculadora con Bison y Flex*
- fb1-5.l
- fb1-5.y

*Contador de palabras en C*
- ContadorDePalabras.c

*Contador de palabras con Flex*
- fbPrueba.l
ademas para las pruebas de rendimiento se requiere un archivo de texto grande:
- textogrande.txt
en este .txt debe contener una gran cantidad de lineas en este caso (5,000,000)

# Dependencias
Debemos tener instalados los siguientes programas
- flex
- bison
- gcc
Todo esto lo instale en Ubuntu con el comando:
- sudo apt install flex bison gcc

# Abrir la terminal
# Navegar hasta la carpeta del programa
Usar el comando "cd" para cambiar al directorio donde se encuentran los archivos del proyecto

# Ejercicio 1 - Escaner con Flex
Se implementó un analizador léxico simple utilizando Flex que reconoce números y operadores básicos.

# Compilacion
Usamos los comandos:
- flex fb1-1.l
- gcc lex.yy.c -o fb1-1 -lfl

# Ejecucion
Usamos el comando:
- ./fb1-1

# Salida esperada del programa
Reconoce numeros y operadores basicos y muestra los tokens identificados

<img width="383" height="151" alt="image" src="https://github.com/user-attachments/assets/4d67caf1-efbf-4293-94a0-73265335e9de" />

# Ejercicio 2 - Conversion palabras
Se desarrolló un scanner en Flex que convierte automáticamente ciertas palabras del American English a su equivalente en British English.

# Compilacion
Usamos los comandos:
- flex fb1-2.l
- gcc lex.yy.c -o fb1-2 -lfl

# Ejecucion
./fb1-2

# Salida esperada del programa
Imprime el texto convertido

# Ejemplo
Entrada:
The color of the center is organized
Salida:
The colour of the centre is organised

<img width="921" height="254" alt="image" src="https://github.com/user-attachments/assets/c1365137-6ef7-4215-a907-ea6bf730a6e8" />


# Ejercicio 3 - Escaner simple con flex
Se implementó el scanner del Example 1-3, el cual reconoce tokens para una calculadora simple y los imprime en pantalla, reconoce:
+,-,*,/,|,Numeros,Saltos de linea y caracteres desconocidos

# Compilacion
Usamos los comandos:
- flex fb1-3.1.l
- gcc lex.yy.c -p fb1-3 -lfl

# Ejecucion
- ./fb1-3

# Ejemplo 
Entrada:
10/0
Salida:
NUMBER 10
DIVIDE
NUMBER 0
NEWLINE

Como caso de error si se ingrresa algun carcter desconocido no definido el programa mostrara:
- Mystery character XXXXXX

<img width="506" height="549" alt="image" src="https://github.com/user-attachments/assets/a860708d-f610-4415-a5c3-c85bee760d20" />



# Ejercicio 4 - Escaner de Calculadora
Este scanner no imprime texto como el anterior, sino que retorna tokens numéricos usando return, simulando la integración con un parser (como Bison) y se definen los siguientes tokens:

- NUMBER = 258
- ADD = 259
- SUB = 260
- MUL = 261
- DIV = 262
- ABS = 263
- EOL = 264
- Los números guardan su valor en "yylval"
- Los espacios y tabulaciones se ignoran
- Caracteres desconocidos muestran mensaje de error
El main() ejecuta yylex() y muestra el número del token devuelto

# Compilacion
- flex fb1-4.1.l
- gcc lex.yy.c o fb1-4 -lfl

# Ejecucion
- ./fb1-4

# Ejemplo
Entrada:
10+5
Salida:
258 = 10
259
258 = 5
264

258 corrresponde a NUMBER, 259 a ADD, etc.

<img width="921" height="458" alt="image" src="https://github.com/user-attachments/assets/71319cac-e955-4463-afc3-b5231d14ce77" />



# Ejercicio 5 - Calculadora Simple 
Implementación de una calculadora simple utilizando Bison donde soporta suma, resta, multiplicacion, division, valor absoluto, expresiones con multiples operaciones y fin de linea como final de expresion
El parser usa reglas gramaticales para evaluar expresiones y mostrar el resultado

# Compilacion
Primero genera el parser con Bison
- bison -d fb1-5.y
Luego de compilar junto con el escaner
- flex fb1-4.1.l
- gcc fb1-5.tab.c lex.yy.c -o fb1-5 -lfl

# Ejecucion
- ./fb1-5
Entrada:
3+4
Salida:
= 7

<img width="921" height="655" alt="image" src="https://github.com/user-attachments/assets/ebff3daa-8676-4df4-b047-2a486238e708" />


# Ejercicio 6 - Escaner Integrado
Le adicionamos esto al escaner:
"%{
#include "fb1-5.tab.h"
%}"
Esto lo hacemos para que el Flex use los tokens definidos por Bison, ya no se imprime nada en el escaner solo se retornan los tokens al parser

# Compilacion con Makefile
se usan unas reglas que son las siguientes:
"fb1-5: fb1-5.1 fb1-5.y
	bison -d fb1-5.y
	flex fb1-5.1
	gcc -o fb1-5 fb1-5.tab.c lex.yy.c -lfl"

luego solamente ejecutamos con un "make"

# Ejecucion
- ./fb1-5
<img width="747" height="653" alt="image" src="https://github.com/user-attachments/assets/7b5363f3-201f-4234-b81d-a30dd4e5fbe4" />





# Ejercicios
# 1.	Will the calculator accept a line that contains only a comment? Why not? Would it be easier to fix this in the scanner or in the parser?
# ¿La calculadora aceptará una línea que solo contenga un comentario? ¿Por qué no? ¿No sería más fácil solucionar esto en el escáner o en el analizador?

Respuesta:

Con el parser actual no aceptaría porque la gramática dice que:
<img width="651" height="270" alt="image" src="https://github.com/user-attachments/assets/24c91dcb-2c76-42a6-9844-bb1c59af459d" />

Dice “exp EOL” esto quiere decir que no aceptaría los comentarios y se generaría un error sintáctico. Esto ocurre si por ejemplo escribimos “// Angelito” El leer no tiene la regla para los comentarios y se reconocería como otra cosa y el otro “/” también y el texto seria un error

Seria mas fácil solucionarlo en el escáner porque ahí los comentarios serían un problema léxico, no sintáctico. Ahí se eliminarían en el leer y el parser no lo tomara


# 2.	Make the calculator into a hex calculator that accepts both hex and decimal numbers. In the scanner add a pattern such as 0x[a-f0-9]+ to match a hex number, and in the action code use strtol to convert the string to a number that you store in yylval; then return a NUMBER token. Adjust the output printf to print the result in both decimal and hex.
# Convierta la calculadora en una calculadora hexadecimal que acepte tanto números hexadecimales como decimales. En el scanner agregue un patrón como 0x[a-f0-9]+ para reconocer un número hexadecimal y, en el código de acción, use strtol para convertir la cadena en un número que se almacene en yylval; luego retorne el token NUMBER. Ajuste el printf de salida para imprimir el resultado tanto en decimal como en hexadecimal.

Respuesta:

Primero tendríamos que modificar el escaner
<img width="702" height="623" alt="image" src="https://github.com/user-attachments/assets/70d129b5-1e69-423b-b7e5-87b17843e2c9" />
Como siguiente paso deberíamos modificar el parser
<img width="921" height="164" alt="image" src="https://github.com/user-attachments/assets/070048b8-9b6b-43dc-89c9-73f0d9aaa956" />
Esto lo hacemos para que imprima el resultado decimal y el resultado hexadecimal, después compilamos con el comando “make”
<img width="716" height="159" alt="image" src="https://github.com/user-attachments/assets/81a3f9c2-5764-43fe-ab18-b6eade70cb44" />
Ya eso significa que está bien compilado, ahora necesitamos probar 
<img width="508" height="284" alt="image" src="https://github.com/user-attachments/assets/c51b49d8-4cd5-4ce8-bc3e-2a4411b7709c" />
Acá primero hicimos la operación “6+3” su resultado nos quedo bien. Hice una prueba con una letra y correctamente desconocido el número y en la terminal da el mensaje de que hay un carácter desconocido el cual es la letra que pusimos y es un error sintáctico


# 3.	Add bit operators such as AND and OR to the calculator. The obvious operator to use for OR is a vertical bar, but that’s already the unary absolute value operator. What happens if you also use it as a binary OR operator, for example, exp ABS factor?

# Agregue operadores de bits como AND y OR a la calculadora. El operador más obvio para OR es la barra vertical (|), pero está ya se utiliza como operador unario de valor absoluto. ¿Qué sucede si también se usa como operador OR binario, por ejemplo, exp ABS factor?

En lo que entendí es que tenemos que implementar “&” que sería el AND bit a bit y el “|” como OR bit a bit
Modificaríamos el leer que ya utilizamos en los ejemplos que hicimos antes. Modifique el archivbo flex para que reconozca números y operadores.
Luego en el archivo de bison definimos la gramática y las reglas para que las operaciones se evaluaran correctamente
Para terminar compile usando lo de siempre, hubieron varios errores y advertencias con las funciones de yylex y yyerror después de compilar, ejecutamos el programa y hice pruebas desde la terminal 
<img width="921" height="466" alt="image" src="https://github.com/user-attachments/assets/16e68e9c-09cf-470e-8800-916bd7ba60e6" />


# 4.	Does the handwritten version of the scanner from Example 1-4 recognize exactly the same tokens as the flex version?

# ¿La versión escrita a mano del scanner del Ejemplo 1-4 reconoce exactamente los mismos tokens que la versión hecha con flex?
Respuesta:
No necesariamente reconocen los mismos tokens. Se diseñaron para que se comporten de una manera similar pero no deben ser los mismos.
La del ejemplo 4 muestra como se debe reconocer cada token mediante un simple código en C y usa estructuras como el switch y el while. En el primero usa expresiones regulares para usar los patrones de los tokens lo que puede hacer mas preciso dependiendo las reglas.


# 5. Can you think of languages for which flex wouldn’t be a good tool to write a scanner?

# ¿Puedes pensar en lenguajes para los cuales flex no sería una buena herramienta para escribir un scanner?

Respuesta:
Flex no seria una buena herramienta para lenguajes cuyo reconocimiento de tokens o análisis léxico no puede describirse fácilmente mediante expresiones regulares. Porque tiene reglas que dependen del contexto o necesitan mantener información extra mediante el escaneo

# 6.	Rewrite the word count program in C. Run some large files through both versions. Is the C version noticeably faster? How much harder was it to debug?

# Reescribe el programa de conteo de palabras en C. Ejecuta algunos archivos grandes en ambas versiones. ¿Es la versión en C notablemente más rápida? ¿Qué tan difícil fue depurarla?

Hice un código simple en C sobre un contador de palabras, lo guarde, lo compile
<img width="921" height="94" alt="image" src="https://github.com/user-attachments/assets/29ff4720-355e-49ef-98a6-33be6491b1c4" />
Luego busque un código para que se genere un texto grande para que la prueba  a nivel de comparación se vea el código fue “yes “Esta es una prueba grande para medir el rendimiento del programa de contador de palabras” | head -n 5000000 > textogrande.txt”
<img width="921" height="518" alt="image" src="https://github.com/user-attachments/assets/af7271d1-9841-47d7-9e76-7c846535c213" />
Haciendo el análisis veo que la versión de C es más rápida y Flex tardo mas del doble y esa diferencia es muy notable. Esto pasa porque C lee carácter por carácter y tiene menos capas y la versión de flex tiene como una lógica automática que pesa más.
En la prueba realizada con un archivo de 5,000,000 de líneas, la versión en C tardó aproximadamente 2.46 segundos, mientras que la versión con Flex tardó alrededor de 5.15 segundos, es decir, más del doble de tiempo.
Sin embargo, aunque la versión en C mostró mejor rendimiento, su implementación requiere mayor control manual y puede ser más difícil de depurar. Por otro lado, Flex facilita el desarrollo al utilizar expresiones regulares y generar automáticamente el analizador léxico, lo que simplifica la programación y el mantenimiento del código.

































