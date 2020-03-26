# Operadores

Conocemos muchos operadores de la escuela. Son cosas como la adición `+`, multiplicación `*`, resta `-`, etc.

En este capítulo, nos concentraremos en aspectos de los operadores que no se cubren en la aritmética de la escuela.

## Vocablos: "unario", "binario", "operando"

Antes de continuar, comprendamos alguna terminología común.

- *Un operando* -- es aquello a lo que se aplican los operadores. Por ejemplo, en la multiplicación de `5 * 2` hay dos operandos: el operando izquierdo es `5` y el operando derecho es `2`. Algunas veces, son llamados "argumentos" en lugar de "operandos".
- Un operador es *unario* si tiene un único operando. Por ejemplo, la negación unaria `-` invierte el signo de un número:

    ```js run
    let x = 1;

    *!*
    x = -x;
    */!*
    alert( x ); // -1, se ha aplicado la negación unaria
    ```
- Un operador es *binario* si tiene dos operandos. El mismo operador menos '-' existe en forma binaria también:

    ```js run no-beautify
    let x = 1, y = 3;
    alert( y - x ); // 2, el menos binario sustrae valores
    ```

    Formalmente, aquí estamos hablando de dos operadores distintos: la negación unaria (operando simple: invierte el signo) y la sustracción binaria (dos operandos: sustrae).

## Concatenación de cadenas de texto, + binario

Ahora veamos características especiales de operadores de Javascript que van más allá de la aritmética escolar.

Habitualmente, el operador `+` suma números.

Sin embargo, si el operador `+` se aplica a cadenas de texto, las une (concatena):

```js
let s = "my" + "string";
alert(s); // mystring
```

Nota que si uno de los operandos es una cadena, el otro también se convierte a una cadena.

Por ejemplo:

```js run
alert( '1' + 2 ); // "12"
alert( 2 + '1' ); // "21"
```

Ves, no importa si el primer operando es un cadena o es el segundo. La regla es simple: si cualquiera de los dos es una cadena, el otro también se convierte a una cadena.

No obstante, note que las operaciones se ejecutan de izquierda a derecha. Si hay dos números seguidos de una cadena, los números serán sumados antes de ser convertidos a una cadena:


```js run
alert(2 + 2 + '1' ); // "41" y no "221"
```

La concatenación de cadenas y conversión es una característica especial del operador más binario `+`. Otros operadores aritméticos trabajan sólo con números y siempre convierten sus operandos a números.

Por ejemplo, la sustracción y la división:

```js run
alert( 2 - '1' ); // 1
alert( '6' / '2' ); // 3
```

## Conversión numérica, + unario

El más `+` existe en dos formas: la forma binaria que usamos más arriba y la forma unaria.

El más unario o, en otras palabras, el operador más `+` aplicado a un único valor, no hace nada a los números. Pero si el operador no es un número, el operador unario más lo convierte a un número.

Por ejemplo:

```js run
// No tiene efecto en números
let x = 1;
alert( +x ); // 1

let y = -2;
alert( +y ); // -2

*!*
// Convierte no-números
alert( +true ); // 1
alert( +"" );   // 0
*/!*
```

En realidad hace lo mismo que `Number(...)`, pero es más corto.

La necesidad de convertir cadenas a números surge frecuentemente. Por ejemplo, si obtenemos valores de campos de formularios HTML, habitualmente son cadenas de texto.

¿Y si queremos sumarlos?

El operador binario más los uniría como cadenas:

```js run
let apples = "2";
let oranges = "3";

alert( apples + oranges ); // "23", el operador binario más concatena cadenas
```

Si queremos tratarlos como números, necesitamos convertirlos primero y después sumarlos:

```js run
let apples = "2";
let oranges = "3";

*!*
// ambos valors convertidos a números antes del operador binario más
alert( +apples + +oranges ); // 5
*/!*

// la variante más larga
// alert( Number(apples) + Number(oranges) ); // 5
```

Desde el punto de vista de un matemático, la abundancia de mases puede resultar extraña. Pero desde el punto de vista de un programador, no hay nada especial: los mases unarios se aplican primero, convierten las cadenas a números, y luego el operador binario más los suma.

¿Por qué se aplican los mases unarios antes de los binarios? Como veremos, es a causa de su *mayor precedencia*.

## Precedencia de operadores

Si una expresión tiene más de un operador, el orden de ejecución se define por su *precedencia*, o, en otras palabras, el orden implícito de prioridad de los operadores.

Todos conocemos de la escuela que la multiplicación en la expresión `1 + 2 * 2` debe calcularse antes de la edición. Esto es exactamente la precedencia. Se dice de la multiplicación que tiene *una precedencia mayor* que la suma.

Los paréntesis anulan cualquier precedencia, así que si no estamos satisfechos con el orden implícito, podemos usarlos para cambiarlo. Por ejemplo: `(1 + 2) * 2`.

Hay muchos operadores en JavaScript. Cada operador tiene un correspondiente número de precedencia. El que tenga el mayor número se ejecuta primero. Si tienen la misma precedencia, el orden de ejecución es de izquierda a derecha.

Aquí hay un extracto de [tabla de precedencia](https://developer.mozilla.org/en/JavaScript/Reference/operators/operator_precedence) (no necesitas recordarlo por ahora, pero nota que los operadores unarios tienen mayor precedencia que sus correspondientes binarios):

| Precedencia | Nombre | Símbolo
|------------|------|------|
| ... | ... | ... |
| 16 | más unario | `+` |
| 16 | negación unaria | `-` |
| 14 | multiplicación | `*` |
| 14 | división | `/` |
| 13 | adición | `+` |
| 13 | sustracción | `-` |
| ... | ... | ... |
| 3 | asignación | `=` |
| ... | ... | ... |

Como podemos ver, el "más unario" tiene una prioridad de `16` que es mayor que el `13` de la "adición" (más binario). Este es el motivo por el que en la expresión `"+apples + +oranges"`, los mases unarios se aplican antes del más binario.

## Asignación

Nótese que una asignación `=` es también un operador. Se lista en la tabla de precedencia con una muy baja prioridad de `3`.

Por este motivo, cuando asignamos una variable, como `x = 2 * 2 + 1`, los cálculos se ejecutan primero y luego el `=` es evaluado, almacenando el resultado en `x`.

```js
let x = 2 * 2 + 1;

alert( x ); // 5
```

Es posible encadenar asignaciones:

```js run
let a, b, c;

*!*
a = b = c = 2 + 2;
*/!*

alert( a ); // 4
alert( b ); // 4
alert( c ); // 4
```

Las asignaciones encadenadas se evalúan de derecha a izquierda. Primero, la expresión más a la derecha `2 + 2` es evaluada y después es asignada a las variables de la izquierda : `c`, `b` y `a`. Al final, todas las variables comparten un único valor.

````smart header="The assignment operator `\"=\"` returns a value"
Un operador siempre retorna un valor. Esto es obvio para la mayoría de ellos como la adición `+` o la multiplicación `*`. Pero el operador asignación también sigue esta regla.

La llamada `x = value` escribe el `value` en`x` *y luego lo retorna*.

Aquí hay una demostración que utiliza una asignación como parte de una expresión más compleja:

```js run
let a = 1;
let b = 2;

*!*
let c = 3 - (a = b + 1);
*/!*

alert( a ); // 3
alert( c ); // 0
```

En el ejemplo de arriba, el resultado de `(a = b + 1)` es el valor que es asignado a `a` (esto es `3`). Es entonces usado para sustraerlo de `3`.

Qué código más divertido, ¿no? Debemos entender cómo funciona, porque algunas veces lo vemos en librerías de terceros, pero no debemos escribir nada semejante. Estos trucos definitivamente no hacen el código más claro ni más legible.
````

## Resto %

El operador resto `%`, a pesar de su apariencia, no está relacionado con porcentajes.

El resultado de `a % b` es el resto de la división entera de `a` por `b`.

Por ejemplo:

```js run
alert( 5 % 2 ); // 1 es el resto de 5 dividido por 2
alert( 8 % 3 ); // 2 es el resto de 8 dividido por 3
alert( 6 % 3 ); // 0 es el resto de 6 dividido por 3
```

## Exponenciación **

El operador exponenciación `**` es una adición reciente al lenguaje.

Para un número natural `b`, el resultado de `a ** b` es `a` multiplicado por sí mismo `b` veces.

Por ejemplo:

```js run
alert( 2 ** 2 ); // 4  (2 * 2)
alert( 2 ** 3 ); // 8  (2 * 2 * 2)
alert( 2 ** 4 ); // 16 (2 * 2 * 2 * 2)
```

El operador funciona también para números no enteros.

Por ejemplo:

```js run
alert( 4 ** (1/2) ); // 2 (la potencia de 1/2 es lo mismo que una raíz cuadrada, esto son mates)
alert( 8 ** (1/3) ); // 2 (la potencia de 1/3 es lo mismo que una raíz cúbica)
```

## Incremento/decremento

<!-- Can't use -- in title, because built-in parse turns it into – -->

Incrementar o decrementar un número por uno se encuentra entre las más comunes operaciones numéricas.

Así, hay operadores especiales para ello:

- **Incremento** `++` incrementa la variable en 1:

    ```js run no-beautify
    let counter = 2;
    counter++;      // funciona igual que counter = counter + 1, pero es más corto
    alert( counter ); // 3
    ```
- **Decremento** `--` decrementa la variable en 1:

    ```js run no-beautify
    let counter = 2;
    counter--;      // funciona igual que counter = counter - 1, pero es más corto
    alert( counter ); // 1
    ```

```warn
El incremento/decremento sólo puede aplicarse a variables. Intentar aplicarlo en un valor como `5++` producirá un error.
```

Los operadores `++` y `--` pueden situarse bien antes o bien después de una variable.

- Cuando el operador va después de la variable, está en su "forma sufijo": `counter++`.
- La "forma prefijo" es cuando el operador se sitúa antes de la variable: `++counter`.

Ambas expresiones hacen lo mismo: incrementan `counter` en `1`.

¿Hay alguna diferencia? Sí, pero sólo podemos verla si utilizamos el valor retornado por `++/--`.

Aclarémoslo. Como sabemos, todos los operadores retornan un valor. El incremento/decremento no es ninguna excepción. La forma prefijo retorna el nuevo valor mientras que la forma sufijo retorna el antiguo vaor (antes del incremento/decremento).

Para ver la diferencia, aquí hay un ejemplo:

```js run
let counter = 1;
let a = ++counter; // (*)

alert(a); // *!*2*/!*
```

En la línea `(*)`, la forma *prefijo* `++counter` incrementa `counter` y retorna el nuevo valor, `2`. Así, el `alert` muestra `2`.

Ahora usemos la forma sufijo:

```js run
let counter = 1;
let a = counter++; // (*) cambiamos ++counter por counter++

alert(a); // *!*1*/!*
```

En la línea `(*)`, la forma *sufijo* `counter++` también incrementa `counter` pero retorna el valor *antiguo* (antes del incremento). Así, el `alert` muestra `1`.

Para resumir:

- Si el resultado de un incremento / decremento no es utilizado, no hay diferencia en la forma que usemos:

    ```js run
    let counter = 0;
    counter++;
    ++counter;
    alert( counter ); // 2, las líneas de arriba hicieron lo mismo
    ```
- Si quisíeramos incrementar un valor *y* inmediatamente usar el resultado del operador, necesitamos la forma prefijo:

    ```js run
    let counter = 0;
    alert( ++counter ); // 1
    ```
- Si quisíeramos incrementar un valor pero usar su valor previo, necesitamos la forma sufijo:

    ```js run
    let counter = 0;
    alert( counter++ ); // 0
    ```

````smart header="Increment/decrement among other operators"
Los operadores `++/--` pueden usarse también dentro de expresiones. Su precedencia es mayor que la mayoría de los otros operadores aritméticos.

Por ejemplo:

```js run
let counter = 1;
alert( 2 * ++counter ); // 4
```

Compárenlo con:

```js run
let counter = 1;
alert( 2 * counter++ ); // 2, porque counter++ retorna el valor "antiguo"
```

Aunque técnicamente correcta, esta notación habitualmente hace el código menos legible. Una línea hace varias cosas -- no es bueno.

Mientras leemos el código, una lectura "vertical" rápida puede dejar fácilmente algo como `counter++` y no será obvio que la variable se habrá incrementado.

Recomendamos un estilo de "una línea -- una acción":

```js run
let counter = 1;
alert( 2 * counter );
counter++;
```
````

## Operadores bit a bit

Los operadores bit a bit tratan argumentos como números enteros de 32-bits y funcionan en el nivel de su representación binaria.

Estos operadores no son específicos de JavaScript. Están soportados en la mayoría de lenguajes de programación.

La lista de operadores:

- AND ( `&` )
- OR ( `|` )
- XOR ( `^` )
- NOT ( `~` )
- LEFT SHIFT ( `<<` )
- RIGHT SHIFT ( `>>` )
- ZERO-FILL RIGHT SHIFT ( `>>>` )

Estos operadores raramente se usan. Para comprenderlos, necesitamos profundizar en representación de bajo nivel de números y no sería óptimo hacerlo justo ahora, especialmente dado que por lo pronto no los vamos a necesitar. Si siente curiosidad, puede leer el artículo [Operadores bit a bit](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Operators/Bitwise_Operators) en MDN. Sería más práctico cuando surja una necesidad real.

## Modificar en el lugar

A menudo necesitamos aplicar un operador a una variable y almacenar el nuevo resultado en la misma variable.

Por ejemplo:

```js
let n = 2;
n = n + 5;
n = n * 2;
```

Esta notación puede reducirse usando los operadores `+=` y `*=`:

```js run
let n = 2;
n += 5; // ahora n = 7 (lo mismo que n = n + 5)
n *= 2; // ahora n = 14 (lo mismo que n = n * 2)

alert( n ); // 14
```

Los operadoress "modificar-y-asignar" cortos existen para todos los operadores aritméticos y bit a bit.: `/=`, `-=`, etc.

Este tipo de operadores tienen la misma precedencia que una asignación normal, por lo que se ejecutan después de la mayoría de otros cálculos:

```js run
let n = 2;

n *= 3 + 5;

alert( n ); // 16  (la parte de la derecha se evalúa primero, lo mismo que n *= 8)
```

## Coma

El operador coma `,` es uno de los operadores más raros e inusuales. Algunas veces se utiliza para escribir código más corto, por lo que necesitamos conocerlo para entender lo que está sucediendo.

El operador coma nos permite evaluar múltiples expresiones, dividiéndolas con una coma `,`. Cada una de ellas es evaluada pero sólo se retorna el resultado de la última.

Por ejemplo:

```js run
*!*
let a = (1 + 2, 3 + 4);
*/!*

alert( a ); // 7 (el resultado de 3 + 4)
```

Aquí, la primera expresión `1 + 2` es evaluada y su resultado descartado. Luego, `3 + 4` es evaluado y retornado como resultado.

```smart header="Comma has a very low precedence"
Nótese que el operador coma tiene muy baja precedencia, menor que `=`, por lo que los paréntesis son importantes en el ejemplo de arriba.

Sin ellos: `a = 1 + 2, 3 + 4` evalúa `+` primero, sumando los números `a = 3, 7`, luego el operador de asignación `=` asigna    `a = 3`, y finalmente el número después de la coma, `7`, no se procesa por lo que es ignorado.
```

¿Por qué necesitamos un operador que descarta todo excepto la última parte?

Algunas veces, la gente lo utiliza en construcciones más complejas para poner varias acciones en una línea.

Por ejemmplo:

```js
// tres operaciones en una línea
for (*!*a = 1, b = 3, c = a * b*/!*; a < 10; a++) {
 ...
}
```

Trucos semejantes se utilizan en muchos frameworks JavaScript. Por esto los mencionamos. Pero habitualmente no mejoran la legibilidad del código por lo que deberíamos pensárnoslo bien antes de usarlos.
