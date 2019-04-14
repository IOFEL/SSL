# TP N°1

**1**-Escribir hello2.c , que es una variante de hello.c :


	#include <stdio.h>

	int/*medio*/main(void){
		int i=42;
		prontf("La respuesta es %d\n");

**2**-Preprocesar hello2.c , no compilar, y generar hello2.i . Analizar su
contenido.

**comando ejecutado**

	gcc -std=c11 -E hello2.c -o hello2.i

**resultado**
El  "hello2.i" contiene los structs, typedef, prototipos de las funciones y directivas del archivo "stdio.h" . Al final de "hello2.i", el codigo del archivo "hello2.c"  escrito sin los comentarios.

**3**- Escribir hello3.c , una nueva variante:

	int printf(const char *s, ...);

	int main(void){
		int i=42;
	prontf("La respuesta es %d\n");
**4** - Investigar la semantica de la primera línea.

La primera linea `int printf(const char *s, ...)` define el prototipo de la funcion printf. La declaración `...` nos dice que la cantidad y tipo de esos argumentos puede variar.
La declaración `...` solo puede aparecer al final de la lista de argumentos.

**5**- Preprocesar hello3.c , no compilar, y generar hello3.i . Buscar diferencias
entre hello3.c y hello3.i .

**comando ejecutado**

	gcc -std=c11 -E hello3.c -o hello3.i
**resultado**

	# 1 "hello3.c"
	# 1 "<built-in>"
	# 1 "<command-line>"
	# 1 "/usr/include/stdc-predef.h" 1 3 4
	# 1 "<command-line>" 2
	# 1 "hello3.c"
	int printf(const char *s, ...);

	int main(void){
	 int i=42;
	 prontf("La respuesta es %d\n");

Las primeras 5 lineas son la diferencia. Luego son iguales.

**6**- Compilar el resultado y generar hello3.s , no ensamblar.

**comando ejecutado**

	gcc -std=c11 -S hello3.c
**resultado**

	hello3.c: In function ‘main’:
	hello3.c:5:2: warning: implicit declaration of function ‘prontf’ [-Wimplicit-function-declaration]
	  prontf("La respuesta es %d\n");
	  ^
	hello3.c:5:2: error: expected declaration or statement at end of input

Nos tira una ==advertencia== porque no esta declarada `prontf`.
Nos tira un ==error== .El mismo indica que hay un error de sintaxis (falta un simbolo '}').
Nos crea el archivo hello3.s pero no hay nada en su interior.

	.file	"hello3.c"

**7**- Corregir en el nuevo archivo hello4.c y empezar de nuevo, generar
hello4.s , no ensamblar.

**comando ejecutado**

	gcc -std=c11 -S hello4.c
**resultado**

	hello4.c: In function ‘main’:
	hello4.c:5:2: warning: implicit declaration of function ‘prontf’ [-Wimplicit-function-declaration]
	  prontf("La respuesta es %d\n");
	  ^
Nos tira una ==advertencia== porque no esta declarada `prontf`. Esta vez, nos genero el archivo hello4.s con codigo assembler.

**8**- Investigar hello4.s .
**resultado**

		.file	"hello4.c"
		.section	.rodata
	.LC0:
		.string	"La respuesta es %d\n"
		.text
		.globl	main
		.type	main, @function
	main:
	.LFB0:
		.cfi_startproc
		pushq	%rbp
		.cfi_def_cfa_offset 16
		.cfi_offset 6, -16
		movq	%rsp, %rbp
		.cfi_def_cfa_register 6
		subq	$16, %rsp
		movl	$42, -4(%rbp)
		movl	$.LC0, %edi
		movl	$0, %eax
		call	prontf
		movl	$0, %eax
		leave
		.cfi_def_cfa 7, 8
		ret
		.cfi_endproc
	.LFE0:
		.size	main, .-main
		.ident	"GCC: (Ubuntu 5.4.0-6ubuntu1~16.04.9) 5.4.0 20160609"
		.section	.note.GNU-stack,"",@progbits

Define la organización de un programa en assembler.  Tiene seccion donde declara los datos y otra donde declara los pasos a ejecutar en assembler.

**9**- Ensamblar hello4.s en hello4.o , no vincular.

**comando ejecutado**

	as -o hello4.o hello4.s

**resultado**
Se crea un archivo binario llamado hello4.o que contiene código objeto.

**10**- Vincular hello4.o con la biblioteca estándar y generar el ejecutable

**comando ejecutado**

	gcc -std=c11 hello4.o -o hello4
**resultado**

	hello4.o: En la función `main':
	hello4.c:(.text+0x1a): referencia a `prontf' sin definir
	collect2: error: ld returned 1 exit status
No encuentra la definición de `prontf`. No se crea el ejecutable.

**11**- Corregir en hello5.c y generar el ejecutable.

**comando ejecutado**

	gcc -std=c11 hello5.c -o hello5

**resultado**

	hello5.c: In function ‘main’:
	hello5.c:5:9: warning: format ‘%d’ expects a matching ‘int’ argument [-Wformat=]
	  printf("La respuesta es %d\n");
		 ^

Nos tira una ==advertencia== sobre el formato `%d` que espera coincidir con un `int`. Nos crea el ejecutable.

**12**- Ejecutar y analizar el resultado.

**comando ejecutado**

	./hello5

**resultado**

	La respuesta es -252899560

Basicamente no le pasamos el `int` que esperaba entonces completo con  cualquier cosa.

**13**- Corregir en hello6.c y empezar de nuevo.

	int printf(const char *s, ...);

	int main(void){
		int i=42;
		printf("La respuesta es %d\n",i);
	}

- Creación del ejecutable hello6:
	**comando ejecutado**

		gcc -std=c11 hello6.c -o hello6

	**resultado**
	Nos crea el ejecutable hello6 sin advertencia ni errores.

- Ejecutando hello6:
	**comando ejecutado**

		./hello6

	**resultado**

		La respuesta es 42

	Se le paso el dato `int i = 42` y lo imprimio.

**14**- Escribir hello7.c , una nueva variante:

	int main(void){
	int i=42;
	printf("La respuesta es %d\n", i);
	}
- Creación del ejecutable hello7:
	**comando ejecutado**

		gcc -std=c11 hello7.c -o hello7

	**resultado**

		hello7.c: In function ‘main’:
		hello7.c:3:2: warning: implicit declaration of function ‘printf’ [-Wimplicit-function-declaration]
		  printf("La respuesta es %d\n",i);
		  ^
		hello7.c:3:2: warning: incompatible implicit declaration of built-in function ‘printf’
		hello7.c:3:2: note: include ‘<stdio.h>’ or provide a declaration of ‘printf’


- Ejecutando hello6:
	**comando ejecutado**

		./hello7

	**resultado**

		La respuesta es 42

**15**- Explicar porqué funciona.

No hay errores sintacticos. El programa esta bien escrito.
Las dos advertencias que nos tira son del compilador. Nos dice que no esta definida `printf`.
Si bien no definimos la funcion esta se encuentra en la libreria de c. El linker se encarga enlazar la definicion de `printf`.
