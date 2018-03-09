## Varios bugs

Respondo las preguntas para el caso del archivo add_array_segfault.c

- Correr el programa con un debugger, sin agregar flags de
debug. ¿Tienen toda la información que requerían?
Rta.: El gdb tira un error relativo a arena.c. No es una buena pista sobre dónde
radica el error.

- ¿Qué pasa si ponen el flag de debug? ¿Qué flag de optimización es el
mejor para debuggear?
Rta.: Con el flag de debug (-Og) el programa no corre, pero el gdb tira que el error
es una direccion de memoria en el main, que nos da algo más de ayuda que en otros casos.
El flag de optimización que me dio la mejor pista para arreglar el bug fue -O1, ya 
que corriendo gdb decia que estaba asignando una dirección de memoria al arreglo.
Con las optimizaciones O2 y O3, el programa corre, aunque el resultado está mal. 

- Agreguen algún flag para que informe todos los warnings en la
compilación, como `-Wall`. ¿Alguno les da alguna pista de por qué el
programa se rompe?
Rta.: -Wall agrega que existe algún tipo de error con la inicialización de los
arreglos a[i] y b[i], que es algo más, pero no del todo útil.



## Floating point exception

- ¿Qué función requiere agregar `-DTRAPFPE`? ¿Cómo pueden hacer que el
programa *linkee* adecuadamente?
Rta.:

- Para cada uno de los ejecutables, ¿qué hace agregar la opción
`-DTRAPFPE` al compilar? ¿En qué se diferencian los mensajes de salida
de los programas con y sin esa opción cuando tratan de hacer una
operación matemática prohbida, como dividir por 0 o calcular la raíz
cuadrada de un número negativo?
Rta.:


## Segmentation Fault

- ¿Devuelven el mismo error que antes?
Rta.:

- Averigüen qué hicieron al ejecutar la sentencia `ulimit -s
unlimited`. Algunas pistas son: abran otra terminal distinta y fíjense
si vuelve al mismo error, fíjense la diferencia entre `ulimit -a`
antes y después de ejecutar `ulimit -s unlimited`, googleen, etcétera.
Rta.:
Cuando se declara una variable en un programa, el kernel asigna un 
espacio para sus datos en el stack. Sin embargo, es posible decirle
al kernel cuánta cantidad de espacio en el stack puede utilizar un programa
determinado. El error que nace de superar el espacio asignado en
stack se denomina "stack overflow".
Poner ulimit -s unlimited` hace que el stack size pase de tener 8192 Kb 
asignados a tener el flag 'unlimited'

- La "solución" anterior, ¿es una solución en el sentido de debugging?
Rta.: No, puesto que si el programa está mal hecho pq, por ejemplo, tiene
un bucle infinito, entonces este programa generará una falla del sistema
en lugar de una falla del programa.

- ¿Cómo harían una solución más robusta que la anterior, que no
requiera tocar los `ulimits`?
Rta.: Una mejor solución sería, primero que nada, revisar el código para
controlar que no haya errores de recursividad infinita. También se debe
revisar que el código sea eficiente. Otra opción es utilizar Valgrind.


## Funny

- En la carpeta `funny/` hay un código de C. Describan las diferencias
de los ejecutables al compilar con y sin el flag `-DDEBUG`. ¿De dónde
vienen esas diferencias?
Rta.: `-DDEBUG` es un flag que sirve para implementar lo que se denomina
"Instrumentation techniques". Esta técnica simplemente consiste en ir agregando
código al programa con el fin de obtener más información acerca del comportamiento
del mismo cuando corre. Lo más común es agregar printf para ir chequeando los
valores de las variables en diferentes etapas de la ejecución del programa.
La desventajade utilizar los printf es que luego de haber encontrado el error,
hay que eliminarlos a todos. 
Para evitar esto podemos utilizar el preprocesador para incluir selectivamente 
el código de instrumentación de manera que sólo se necesita recompilar el programa
para incluir o no el código de debugging. Esto se realiza mediante el uso del flag
`-DDEBUG` al compilar y agregando en nuestro código #ifdef DEBUG cada vez que se
quiera que el programa imprima algo en pantalla.
