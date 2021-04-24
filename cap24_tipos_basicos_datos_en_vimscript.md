# Capítulo 24: Tipos de datos básicos de Vimscript 

En los próximos capítulos, aprenderás sobre Vimscript, el lenguaje de programación propio de Vim.

Cuando aprendes un nuevo lenguaje, hay tres elementos básicos para buscar:
- Primitivos
- Medios de combinación
- Medios de abstracción

En este capítulo, aprenderás los tipos de datos primitivos de Vim.

## Tipos de datos

Vim tiene 10 tipos de datos diferentes:
- Number (Números)
- Float (Flotante)
- String (Cadenas)
- List (Lista)
- Dictionary (Diccionario)
- Specia (Especial)
- Funcref
- Job (Trabajos)
- Channel (Canal)
- Blob

Veremos los seis primeros tipos de datos en este capítulo. En el capítulo 27 aprenderás sobre Funcref. Para aprender más sobre los tipos de datos, echa un vistazo a `:h variables`.

## Continuando con el modo Ex

Vim técnicamente no tiene un [REPL](https://es.wikipedia.org/wiki/REPL) propio, pero tiene un modo, el modo Ex que puede utilizarse de esa manera. Puedes entrar en el modo Ex con `Q` o `gQ`. El modo Ex es como un modo extendido del modo línea de comandos (es como escribir en el modo línea de comandos sin fin). Para salir del modo Ex, escribe `:visual`.

Puedes utilizar tanto `:echo` o `:echom` en este capítulo y los siguientes capítulos sobre Vimscript para continuar escribiendo código. Son similares a `console.log` en JS o `print` en Python. El comando `:echo` muestra las expresiones evaluadas que le das. El comando `:echom` hace lo mismo, pero además, este almacena el resultado en el historial de mensajes.

```viml
:echom "hola mensaje echo"
```

Puedes ver el historial de mensajes con:

```
:messages
```

Para borrar el historial de mensajes, ejecuta:

```
:messages clear
```

## Number (Números)

Vim tiene 4 tipos diferentes de números: decimales, hexadecimales, binarios y octales. Por cierto cuando menciono tipos de datos de números, a menudo esto significa un dato de tipo entero. En esta guía, usaré los términos números y enteros de manera indistinta.

### Decimal

Deberías estar familiarizado con el sistema decimal. Vim acepta decimales positivos y negativos. 1, -1, 10, etc. En programación con Vimscript, probablemente usarás el tipo decimal la mayor parte del tiempo.

### Hexadecimal

El tipo hexadecimal comienzo con `0x` o `0X`. Puedes asociarlo con He**x**adecimal.

### Binario

El tipo binario comienza con `0b` o `0B`. Puedes asociarlo con : **B**inario.

### Octal

El tipo octal comienza con `0`, `0o` y `0O`. Puedes asociarlo con **O**ctal.

### Mostrando números

Si ejecutas el comando `echo` ya sea con números de tipo hexadecimal, binario, u octal, Vim automáticamente los convierte a decimales.

```viml
:echo 42
" devuelve 42

:echo 052
" devuelve 42

:echo 0b101010
" devuelve 42

:echo 0x2A
" devuelve 42
```

### Verdadero y falso

En Vim, un valor 0 es similar a falso y todos los valores que no son 0 son tomados como verdadero.

El siguiente ejemplo no mostrará nada:

```viml
:if 0
:  echo "Nope"
:endif
```

Sin embargo, esto sí lo hará:

```viml
:if 1
:  echo "Sí"
:endif
```

Cualquier otro valor que no sea 0 es tomado como verdadero, incluyendo los números negativos. 100 es verdadero. -1 es verdadero.

### Números aritméticos

Los números pueden ser utilizados para ejecutar expresiones aritméticas:

```viml
:echo 3 + 1
" devuelve 4

: echo 5 - 3
" devuelve 2

:echo 2 * 2
" devuelve 4

:echo 4 / 2
" devuelve 2
```

Cuando divides un número y la división produce un resto, Vim elimina ese resto.

```viml
:echo 5 / 2
" devuelve 2 en vez de 2.5
```

Para obtener un resultado más acertado, necesitas utilizar un número flotante.

## Float (Flotante)

Los números flotantes son números con decimales finales. Hay dos maneras de representar los números flotantes: utilizar la notación con un punto (como 31.4) o con exponente (3.14e01). De manera similar a los números, puedes tener signo positivo o negativo:

```viml
:echo +123.4
" devuelve 123.4

:echo -1.234e2
" devuelve -123.4

:echo 0.25
" devuelve 0.25

:echo 2.5e-1
" devuelve 0.25
```

Necesitas darle el número entero un punto y los dígitos finales. `25e-2` (sin punto) y `1234.` (tiene un punto, pero no tiene decimales detrás) son ambos números flotantes inválidos.

### Aritmética con números flotantes

Cuando realizas expresiones aritméticas entre un número y un número flotante, Vim fuerza el resultado a un número flotante.

```viml
:echo 5 / 2.0
" devuelve 2.5
```

Una operación aritmética entre dos numeros flotantes te devuelve otro número flotante.

```
:echo 1.0 + 1.0
" devuelve 2.0
```

## String (Cadenas)

Las *strings* o cadenas son caracteres rodeados ya sea con comillas dobles (`""`) o comillas simples (`''`). "Hola", "123" y '123.4' son ejemplos de cadenas.

### Concatenación de cadenas

Para concatenar una cadena o *string* en Vim, utiliza el operador `.`.

```viml
:echo "Hola" . " mundo"
" devuelve "Hola mundo"
```

### Operaciones aritméticas con cadenas

Cuando ejecutas operaciones aritméticas (`+ - * /`) con un número y una cadena, Vim fuerza la cadena a convertirse a número.

```viml
:echo "12 donuts" + 3
" devuelve 15
```

Cuando Vim ve "12 donuts", extrar el 12 de la cadena y lo convierte al número 12. Después realiza la suma, devolviendo 15. Para que funcione este forzado de cadena a número, los caracteres de los números necesitan ser *los primeros caracteres* de la cadena.

Lo siguiente no funcionará porque 12 no está al comienzo de la cadena:

```viml
:echo "donuts 12" + 3
" devuelve 3
```

Esto tampoco funcionará porque hay un espacio en blanco al comienzo de la cadena:

```viml
:echo " 12 donuts" + 3
" devuelve 3
```

Ese forzao funciona incluso con dos cadenas:

```
:echo "12 donuts" + "6 rosquillas"
" devuelve 18
```

Esto funciona con cualquier operador aritmético, no solo `+`:

```viml
:echo "12 donuts" * "5 cajas"
" devuelve 60

:echo "12 donuts" - 5
" devuelve 7

:echo "12 donuts" / "3 personas"
" devuelve 4
```

Un buen truco para forzar una conversión de cadena a número es añadir 0 o multiplicar por 1:

```viml
:echo "12" + 0
" devuelve 12

:echo "12" * 1
" devuelve 12
```

Cuando se realiza una operación aritmética de un flotante con una cadena, Vim lo trata como un entero, no como un número con coma flotante.

```
:echo "12.0 donuts" + 12
" devuelve 24, no 24.0
```

### Concatenación de número con una cadena

Puedes forzar un número en una cadena con el operador del punto (`.`):

```viml
:echo 12 . "donuts"
" devuelve "12donuts"
```

Este forzado solo funciona con tipo de datos de números, no flotantes. Esto no funcionará:

```
:echo 12.0 . "donuts"
" Esto no devolverá "12.0donuts" si no que lanzará un error
```

### Condicionales de cadenas

Recuerda que 0 es tratado como falso y todos los demás números son verdadero. Esto también se cumple cuando estamos utilizando cadenas como condicionales. This is also true when using string as conditionals.

En la siguiente declaración de un comando *if*, Vim fuerza "12donuts" en 12, lo que se considera verdadero:

```viml
:if "12donuts"
:  echo "Yum"
:endif
" devuelve "Yum"
```

Por otra parte, esto es tratado como falso:

```viml
:if "donuts12"
:  echo "Nope"
:endif
" no devuelve nada
```

Vim fuerza "donuts12" a 0, porque el primer caracter no es un número.

### Comillas dobles vs simples

Las comillas dobles se comportan de manera diferente que las comillas simples. Las comillas simples muestran los caracteres literalmente, mientras que las comillas dobles aceptan caracteres especiales.

¿Qué son caracteres especiales? Por ejemplo una línea nueva y unas comillas dobles mostrarán:

```viml
:echo "hola\nmundo"
" devuelve
" hola
" mundo

:echo "hola \"mundo\""
" devuelve "hola "mundo""
```

Compara esto con las comillas simples:

```
:echo 'hola\nmundo'
" devuelve 'hola\nmundo'

:echo 'hola \"mundo\"'
" devuelve 'hola \"mundo\"'
```

Los caracteres especiales son caracteres de cadena especiales que cuando son escapados, se comportan de manera diferente. `\n` actúa como una línea nueva. `\"` se comporta como un literal `"`. Para consultar una lista de caracteres especiales, echa un vistazo a `:h expr-quote`.

### Procedimientos con cadenas

Vamos a echar un vistazo a los procedimientos internos con cadenas.

Puedes obtener la longitud de una cadena con `strlen()`.

```
:echo strlen("choco")
" devuelve 5
```

Puedes convertir una cadena a un número con `str2nr()`:

```
:echo str2nr("12donuts")
" devuelve 12

:echo str2nr("donuts12")
" devuelve 0
```

De manera similar al forzado anterior de cadena a número, si el número no es el primer caracter, Vim no funcionará.

La buena noticia es que Vim tiene un método para transformar una cadena a flotante, `str2float()`:

```
:echo str2float("12.5donuts")
" devuelve 12.5
```

Puedes sustituir un patrón en una cadena con el método `substitute()`:

```
:echo substitute("sweet", "e", "o", "g")
" devuelve "swoot"
```

El último parámetro "g", es la opción global. Con este, Vim sustituirá todas las ocurrencias que coincidan. Sin este, Vim solo sustituirá la primera ocurrencia que encuentre.

```
:echo substitute("sweet", "e", "o", "")
" devuelve "swoet"
```

El comando de sustitución puede ser combinado con `getline()`. Recuerda que la función `getline()` obtiene el texto de un número de línea dado. Supón ue tienes el texto "chocolate donut" en la línea 5. Puedes utilizar el siguiente procedimiento:

```
:echo substitute(getline(5), "chocolate", "glaseado", "g")
" devuelve glaseado donut
```

Hay muchos otros procedimientos con cadenas. Echa un vistazo a `:h string-functions`.

## List (Listas)

Una lista en Vimscript es como un Array en Javascript o List en Python. Es una secuencia de elementos *ordenados*. Puedes mezclar y combinar el contenido con diferentes tipos de datos:

```
[1,2,3]
['a', 'b', 'c']
[1,'a', 3.14]
[1,2,[3,4]]
```

### Sublistas

Las listas de Vim comienza en el elemento cero. Puedes acceder a un elemento en particular en una lista con `[n]`, donde n es el índice.

```
:echo ["a", "dulce", "postre"][0]
" devuelve "a"

:echo ["a", "dulce", "postre"][2]
" devuelve "postre"
```

Si sobrepasas el número máximo del índice, Vim mostrará un error que dice que el índice está fuera de rango:

```
:echo ["a", "dulce", "postre"][999]
" devuelve un error
```

Cuando estás por debajo de cero, Vim comenzará el índice desde el último elemento. Sobrepasar el número de índice mínimo también mostrará un error:

```
:echo ["a", "dulce", "postre"][-1]
" devuelve "dulce"

:echo ["a", "dulce", "postre"][-3]
" devuelve "a"

:echo ["a", "dulce", "postre"][-999]
" devuelve un error
```

Puedes seleccionar varios elementos de una lista con `[n:m]`, donde `n` es el índice inicial y `m` es el índice final.

```
:echo ["chocolate", "glazed", "plain", "strawberry", "lemon", "sugar", "cream"][2:4]
" devuelve ["plain", "strawberry", "lemon"]
```

Si no pasas el índice final `m` (`[n:]`), Vim devolverá el resto de elementos comenzando desde el elemento n. Sin no pasa el índice `n` (`[:m]`), Vim devolverá desde el primer elemento hasta el elemento m.

```
:echo ["chocolate", "glazed", "plain", "strawberry", "lemon", "sugar", "cream"][2:]
" devuelve ['plain', 'strawberry', 'lemon', 'sugar', 'cream']

:echo ["chocolate", "glazed", "plain", "strawberry", "lemon", "sugar", "cream"][:4]
" devuelve ['chocolate', 'glazed', 'plain', 'strawberry', 'lemon']
```

Puedes pasarle un índice que exceda el máximo de elementos cuando selecciones varios elementos de un array.

```viml
:echo ["chocolate", "glazed", "plain", "strawberry", "lemon", "sugar", "cream"][2:999]
" devuelve ['plain', 'strawberry', 'lemon', 'sugar', 'cream']
```

### Diseccionando una cadena

Puedes dividir una cadena igual que con las listas:

```viml
:echo "choco"[0]
" devuelve "c"

:echo "choco"[1:3]
" devuelve "hoc"

:echo "choco"[:3]
" devuelve choc

:echo "choco"[1:]
" devuelve hoco
```

### Lista aritmética

Puedes utilizar `+` para concatenar y mutar una lista:

```viml
:let sweetList = ["chocolate", "fresa"]
:let sweetList += ["azúcar"]
:echo sweetList
" devuelve ["chocolate", "fresa", "azúcar"]
```

### Funciones de listas

Vamos a explorar las funciones de listas propias de Vim.

Para obtener la longitud de una lisya, utiliza `len()`:

```
:echo len(["chocolate", "fresa"])
" devuelve 2
```

Para anteponer un nuevo elemento en una lista, puedes utilizar `insert()`:

```
:let sweetList = ["chocolate", "fresa"]
:call insert(sweetList, "azucarado")

:echo sweetList
" devuelve ["azucarado", "chocolate", "fresa"]
```

También puedes pasar a `insert()` el índice donde quieres anteponer el elemento. Si quieres añadir un elemento antes del segundo elemento (índice 1):

```
:let sweeterList = ["azucarado", "chocolate", "fresa"]
:call insert(sweeterList, "crema", 1)

:echo sweeterList
" devuelve ['azucarado', 'crema', 'chocolate', 'fresa']
```

Para eliminar un elemento de una lista, utiliza `remove()`. Esto acepta una lista y el índice del elemento que quieres eliminar.

```
:let sweeterList = ["azucarado", "chocolate", "fresa"]
:call remove(sweeterList, 1)

:echo sweeterList
" devuelve ['azucarado', 'fresa']
```

Puedes utilizar `map()` o `filter()` en una lista para filtrar un element en una frase. Por ejemplo el elemento que contiene la palabra "choco":

```
:let sweeterList = ["azucarado", "chocolate", "fresa"]
:call filter(sweeterList, 'v:val !~ "choco"')
:echo sweeterList
" devuelve ["azucarado", "fresa"]

:let sweetestList = ["chocolate", "glaseado", "azucarado"]
:call map(sweetestList, 'v:val . " donut"')
:echo sweetestList
" devuelve ['chocolate donut', 'glaseado donut', 'azucarado donut']
```

La variable `v:val` es una variable especial de Vim. Está disponible cuando interactuas con una lista o un diccionario utilizando `map()` o `filter()`. Representa cada elemento repetido.

Para más información, echa un vistazo a `:h list-functions`.

### Desgranar una lista

Puedes desempaquetar, desgranar o separar una lista y asignar variables a los elementos de la lista:

```
:let favoriteFlavor = ["chocolate", "glaseado", "fresa"]
:let [flavor1, flavor2, flavor3] = favoriteFlavor

:echo flavor1
" devuelve "chocolate"

:echo flavor2
" devuelve "glaseado"
```

Para asignar el resto de elementos de la lista, puedes utilizar `;` seguido con el nombre de una variable:

```
:let favoriteFruits = ["manzana", "banana", "limón", "arándano", "frambuesa"]
:let [fruit1, fruit2; restFruits] = favoriteFruits

:echo fruit1
" devuelve "manzana"

:echo restFruits
" devuelve ['limón', 'arándano', 'frambuesa']
```

### Modificar una lista

Puedes modificar directamente el elemento de una lista:

```
:let favoriteFlavor = ["chocolate", "glaseado", "fresa"]
:let favoriteFlavor[0] = "azucarado"
:echo favoriteFlavor
" devuelve ['azucarado', 'glaseado', 'fresa']
```

Puedes mutar directamente múltiples elementos de la lista:

```
:let favoriteFlavor = ["chocolate", "glaseado", "azucarado"]
:let favoriteFlavor[2:] = ["fresa", "chocolate"]
:echo favoriteFlavor
devuelve ['chocolate', 'glaseado', 'fresa', 'chocolate']
```

## Diccionario

Un diccionario en Vimscript es una lista desordenada y asociada. Un diccionario no vacío consiste en al menos un par de valores clave.

```
{"desayuno": "gofres", "almuerzo": "tortitas"}
{"comida": ["desayuno", "segundo desayuno", "tercer desayuno"]}
{"cena": 1, "postre": 2}
```

Un dato de un objeto de un diccionario utiliza una cadena como clave. Si tratas de utilizar un número, Vim lo forzará como cadena.

```
:let breakfastNo = {1: "7am", 2: "9am", "11ses": "11am"}

:echo breakfastNo
" devuelve {'1': '7am', '2': '9am', '11ses': '11am'}
```

Si no quieres poner comillas en cada clave, también puedes utilizar la notación `#{}`:

```
:let mealPlans = #{breakfast: "waffles", lunch: "pancakes", dinner: "donuts"}

:echo mealPlans
" devuelve {'lunch': 'pancakes', 'breakfast': 'waffles', 'dinner': 'donuts'}
```

El único requisito para utilizar `#{}` es que cada clave debe ser al menos:

- Un caracter ASCII.
- Digito.
- Una barra baja (`_`).
- Un guión (`-`).

Igual que en la lista, puedes utilizar cualquier tipo de dato como valor.

```
:let mealPlan = {"breakfast": ["pancake", "waffle", "hash brown"], "lunch": WhatsForLunch(), "dinner": {"appetizer": "gruel", "entree": "more gruel"}}
```

### Acceder al diccionario

Para acceder a un valor de un diccionario, puedes llamar a la clave usando tanto la notación de los corchetes (`['key']`) o la notación del punto (`.key`).

```
:let comida = {"desayuno": "huevos revueltos", "almuerzo": "bocadillo", "cena": "sopa"}

:let breakfast = comida['desayuno']
:let lunch = comida.almuerzo

:echo breakfast
" devuelve "huevos revueltos"

:echo lunch
" devuelve "bocadillo"
```

### Modificar un diccionario

Puedes modificar o añadir contenido en un diccionario:

```
:let comida = {"desayuno": "huevos revueltos", "almuerzo": "bocadillo"}

:let comida.desayuno = "tortitas"
:let comida["almuerzo"] = "tacos al pastor"
:let comida["cena"] = "quesadillas"

:echo meal
" devuelve {'almuerzo': 'tacos al pastor', 'desayuno': 'tortitas', 'cena': 'quesadillas'}
```

### Funciones con el diccionario

Vamos a explorar algunas de las funciones propias de Vim que podemos hacer para manejar diccionarios.

Para comprobar la longitud de un diccionario, utiliza `len()`.

```
:let mealPlans = #{desayuni: "gofres", almuerzo: "tortitas", cena: "donuts"}

:echo len(meaPlans)
" devuelve 3
```

Para ver si un diccionario contiene una clave específica, utiliza `has_key()`

```
:let mealPlans = #{desayuno: "gofres", almuerzo: "tortitas", cena: "donuts"}

:echo has_key(mealPlans, "desayuno")
" devuelve 1

:echo has_key(mealPlans, "postre")
" devuelve 0
```

Para ver si un diccionario tiene cualquier elemento, utiliza `empty()`. El procedimiento `empty()` funciona con todo tipo de datos: listas, diccionario, cadenas, números, flotante, etc.

```
:let mealPlans = #{desayuno: "gofres", almuerzo: "tortitas", cena: "donuts"}
:let noMealPlan = {}

:echo empty(noMealPlan)
" devuelve 1

:echo empty(mealPlans)
" devuelve 0
```

Para eliminar una entrada de un diccionario, utiliza `remove()`.

```
:let mealPlans = #{desayuno: "gofres", almuerzo: "tortitas", cena: "donuts"}

:echo "eliminando el desayuno: " . remove(mealPlans, "desayuno")
" devuelve "eliminando el desayuno: 'gofres'""

:echo mealPlans
" devuelve {'almuerzo': 'tortitas', 'cena': 'donuts'}
```

Para convertir un diccionario a una lista de listas, utiliza `items()`:

```
:let mealPlans = #{desayuno: "gofres", almuerzo: "tortitas", cena: "donuts"}

:echo items(mealPlans)
" devuelve [['almuerzo', 'tortitas'], ['desayuno', 'gofres'], ['cena', 'donuts']]
```

`filter()` y `map()` también están disponibles.

```
:let breakfastNo = {1: "7am", 2: "9am", "11ses": "11am"}
:call filter(breakfastNo, 'v:key > 1')

:echo breakfastNo
" devuelve {'2': '9am', '11ses': '11am'}
```

Como un diccionario contiene un par de valores clace, Vim dispone de la variable especial `v:key` que funciona de manera similar a `v:val`. Cuando recorremos un diccionario, `v:key` tendrá el valor de la clave del elemento actual.

Si tienes un diccionario llamado `mealPlans`, puedes mapearlo utilizando `v:key`.

```
:let mealPlans = #{desayuno: "gofres", almuerzo: "tortitas", cena: "donuts"}
:call map(mealPlans, 'v:key . " y leche"')

:echo mealPlans
" devuelve {'almuerzo': 'almuerzo y leche', 'desayuno': 'desayuno y leche', 'cena': 'cena y leche'}
```

De manera similar, puedes mapear el diccionario utilizando `v:val`:

```
:let mealPlans = #{desayuno: "gofres", almuerzo: "tortitas", cena: "donuts"}
:call map(mealPlans, 'v:val . " y leche"')

:echo mealPlans
" devuelve {'almuerzo': 'tortitas y leche', 'desayuno': 'gofres y leche', 'cena': 'donuts y leche'}
```

Para saber más sobre funciones con diccionarios, echa un vistazo a `:h dict-functions`.

## Primitivos especiales (Special Primitives)

Vim tiene unos primitivos especiales:

- `v:false`
- `v:true`
- `v:none`
- `v:null`

Por cierto, `v:` es una variable interna de Vim. Trataremos más en detalle sobre esas variables en un capítulo posterior.

En mi experiencia, no vas a utilizar muy a menudi estos primitivos especiales. Si necesitas un valor verdadero o falso, puedes utilizar simplemente 0 (falso) y un valor distinto de 0 (verdadero). Si necesitas una cadena vacía, simplemente utiliza `""`. Pero es buena idea conocer que existen por si quieres utilizarlos, así que vamos a darles un repaso rápido.

### True (Verdadero)

Esto es equivalente a `true`. Es equivalente a un número con un valor distinto de 0. Cuando codificamos con json mediante `json_encode()`, esto es interpretado como "verdadero".

```
:echo json_encode({"test": v:true})
" devuelve {"test": true}
```

### False (Falso)

Esto es equivalente a `false`. Es equivalente a un número con un valor 0. Cuando codificamos con json mediante `json_encode()`, esto es interpretado como "falso".

```
:echo json_encode({"test": v:false})
" devuelve {"test": false}
```

### None (Vacío)

Es equivalente a una cadena vacía. Cuando codificamos con json mediante `json_encode()`, esto es interpretado como un elemento vacío (`null`).

```
:echo json_encode({"test": v:none})
" devuelve {"test": null}
```

### Null (Vacío)

Similar a `v:none`.

```
:echo json_encode({"test": v:null})
" devuelve {"test": null}
```

## Aprendiendo los tipos de datos de la manera más inteligente

En este capítulo, has aprendido sobre los tipos básicos de datos de Vimscript: números, flotantes, cadenas, listas, diccionarios y especiales. Aprender esto es el primer paso para comenzar a programar en Vimscript.

En el siguiente capítulo, aprenderás cómo combinar estos tipos para escribir expresiones como igualdades, condicionales o bucles.