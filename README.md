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
Ahora el escaner y el parser trabajan completamente integrados

////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////
<img width="383" height="151" alt="image" src="https://github.com/user-attachments/assets/4d67caf1-efbf-4293-94a0-73265335e9de" />
- Flex genera "lex.yy.c"
- gcc compila el escaner
- se ejecuta el programa
- se ingresa el texto

Significa que tu regla [0-9]+ está funcionando correctamente. El resto del texto lo ignora




















