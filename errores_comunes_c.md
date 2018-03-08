## Cuando las cuentas no cierran

Entre los primeros errores que surgen cuando nos iniciamos en C (y otros lenguajes como C++ o Python 2) están los relacionados con operaciones aritméticas.   

### ¿Y los decimales?

``` javascript
int suma = 18;
int cantidad = 10;
double promedio;

promedio = suma / cantidad;

printf("%f\n", promedio);
```

El resultado **es 1 obviamente**

En la expresión de arriba, le decimos a C: ___"Hacé la división entre 18 (entero) y 10 (entero) y mostrala en la salida estándar como un número en punto flotante"___

Y como 18 y 10 son enteros, el compilador entiende -afortunadamente- que estamos trabajando con enteros y hace la cuenta con enteros; por eso, el resultado con decimales **truncados**.

Para obtener el resultado "con decimales", es necesario decirle al compilador que trate a divisor y/o dividendo como números representados como punto flotante; de esta manera lo forzamos a hacer las cuentas con este sistema.

Para lograr esto hacemos **cast** de al menos un operando:

```promedio = (double)suma / cantidad; ``` o similares.

**No** hacer ```promedio = (double)(suma / cantidad);```

Una situación similar:

``` javascript
double promedio;

promedio = 18 / 10;

printf("%f\n", promedio);
```

18 y 10 son literales **enteros** por lo que el resultado también será 1.

Para expresar literales para que sean representados con punto flotante hay que escribir **.0** como parte decimal (si no tienen una) o preceder el número con ```(float)``` o ```(double)```

Para el ejemplo ```promedio = 18.0 / 10;``` o ```promedio = (double)18 / 10;```

### Se complica...

La característica de C que vimos arriba puede resultar aun más confusa con expresiones apenas más complejas, como mostrar cuánto es tres cuartas partes de 80:

``` javascript
double f = 3 / 4 * 80;
printf("%f\n", f);
```

Que claramente muestra 0.000000 por el asunto que vimos arriba.

Cambiando por ```double f = 3.0 / 4 * 80;``` (o similares) muestra el resultado esperado.


### Qué número más extraño...

```javascript
uint8_t num = 200;
num += 100;
printf("%d\n", num);
```

Muestra un número muy distinto a 300... ¡44!

¿Qué sucede? Los bits que usa la computadora (8 en este caso) no alcanzan para representar el 300.

300 en binario es  100101100 => el bit más significativo no se puede representar en 8 bits, por lo que se decarta, quedando en num: 00101100 que es 44 expresado en binario.

En C hay que prestar más atención que en otros lenguajes al tipo de datos que usamos para definir una variable... Hay que asegurarse que sea suficientemente "grande" para el uso que se le piensa dar.

Otro problemita que suele aparecer, y que tiene que ver con el tamaño insuficiente de las variables:

```javascript
int8_t p = 100;

p += 60;
printf("%d\n", p);
```

100 + 60 y muestra -96 (¡Negativo!)

160 se almacena, como todo, en binario: **1** 0100000

Si declaramos una variable como signed, los números se representan en "complemento a 2" En este sistema el bit más significativo (el de más a la izquierda) se utiliza para indicar el signo... En este caso 1, negativo.


### ¡Limpialo vos!

```javascript
int d;
d = d + 9;
printf("%d\n", d);
```

Si recién empezas, esperas ver 9... Pero no... Vas a ver un número cualquiera, aleatorio.

En C, al declarar una variable sólo se reserva memoria para el tipo especificado y se le asigna un nombre. Pero no un valor inicial. Este valor puede ser cualquiera...

Por eso, hay que acostumbrarse a **inicializar las variables**

Dependiendo la configuración, si una variable no fue inicializada el compilador nos va a avisar con un **warning**, a los que muchas veces lamentablemmente pasamos por alto. (Ej. si compilamos con ```gcc uno.c -o uno``` no se generan warnings por variables sin inicializar. Si usamos ```gcc uno.c -o uno -Wall``` sí)

Es **recomendable prestar atención a los warnings** y entender por qué aparecen.

### Un cero a la izquierda

La trampa del sistema octal...

```javascript
int t = 015;
printf("%d\n", t);
```

¡Muestra 13!

¿Qué sucede? C entiende que un literal numérico que comienza en 0 está expresado en **octal**: los números se representan con dígitos entre 0 y 7

En el ejemplo entonces 015 es 15 en base 8, por lo que el valor en decimal será 1 * 8 + 5 = 13

### Nada de esto fue un error...

Las situaciones que mencionamos quedan en evidencia (a veces demasiado tarde, lamentablemente...) porque los resultados que se obtienen no son los esperados; **el compilador no nos avisa** porque todo el código de arriba es correcto.
