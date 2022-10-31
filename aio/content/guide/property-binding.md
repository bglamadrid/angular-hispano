
# Enlace de propiedades `[propiedad]`

Puedes usar los enlaces de propiedad para _asignar valores_ a datos dentro de elementos (_propiedades_) que posean decoradores de directiva `@Input()`.

<div class="alert is-helpful">

Echa un vistazo a <live-example></live-example>, un ejemplo en acción usando los fragmentos de código de esta guía.

</div>

## Enlace de una sola vía (One-way binding)

Este tipo de enlace permite que un dato fluya en una única dirección,
desde una propiedad origen de un componente hacia la propiedad destino de un elemento.

No puedes usar enlaces de propiedad
para leer ni recibir valores desde elemento destino. Asimismo, tampoco puedes utilizar
enlaces de propiedad para llamar a métodos del elemento destino.
Si el elemento en cuestión dispara eventos, puedes instalar un escucha utilizando [enlaces de eventos](guide/event-binding).

Si necesitas leer la propiedad de un elemento destino o llamar a alguno de sus métodos,
echa un vistazo a la referencia de la API para los decoradores [ViewChild](api/core/ViewChild) y
[ContentChild](api/core/ContentChild).

## Ejemplos

El enlace de propiedad más común establece la propiedad de un elemento
como valor de propiedad de un componente. Un ejemplo es
enlazar la propiedad `src` de un elemento de imagen desde la propiedad `itemImageUrl` (_urlDeImagen_) de un componente:

<code-example path="property-binding/src/app/app.component.html" region="property-binding" header="src/app/app.component.html"></code-example>

En otro ejemplo, enlazamos la propiedad `colSpan`. Ten cuidado de no confundirla con `colspan`,
la que es el atributo HTML escrito con una `s` minúscula.

<code-example path="property-binding/src/app/app.component.html" region="colSpan" header="src/app/app.component.html"></code-example>

Para más detalles, revisa la documentación de [HTMLTableCellElement en MDN](https://developer.mozilla.org/en-US/docs/Web/API/HTMLTableCellElement).

Para más información sobre este ejemplo de enlace con `colSpan` y `colspan`, echa un vistazo a la guía de [Enlaces de atributo](guide/attribute-binding#colspan).

Otro ejemplo es desactivar un botón cuando el componente posee una propiedad `isUnchanged` (_noPoseeCambios_):

<code-example path="property-binding/src/app/app.component.html" region="disabled-button" header="src/app/app.component.html"></code-example>

Otro ejemplo, asignar un valor a la propiedad de una directiva:

<code-example path="property-binding/src/app/app.component.html" region="class-binding" header="src/app/app.component.html"></code-example>

Otro más es asignar un objeto como valor a la propiedad de un componente&mdash;la que es una excelente
manera de comunicar componentes padres con hijos:

<code-example path="property-binding/src/app/app.component.html" region="model-property-binding" header="src/app/app.component.html"></code-example>

## Destinos de enlaces de propiedad

Una propiedad de elemento encerrada en corchetes (_square brackets_) se identifica
como propiedad de destino.
En el código a continuación, la propiedad destino es `src`, del elemento de imagen.

<code-example path="property-binding/src/app/app.component.html" region="property-binding" header="src/app/app.component.html"></code-example>

También es posible usar la nomenclatura alternativa, añadiendo el prefijo `bind-`:

<code-example path="property-binding/src/app/app.component.html" region="bind-prefix" header="src/app/app.component.html"></code-example>

En la mayoría de los casos, el nombre del destino es de una propiedad, incluso
si parece ser el nombre de un atributo.
En este caso, `src` es el nombre de una propiedad del elemento `<img>`.

Las propiedades de elemento pueden ser destinos comunes de enlace,
pero Angular siempre analiza antes si el nombre corresponde a la propiedad de una directiva conocida,
como en el caso del ejemplo que sigue:

<code-example path="property-binding/src/app/app.component.html" region="class-binding" header="src/app/app.component.html"></code-example>

Técnicamente, Angular está correspondiendo el nombre como si se tratara de una directiva `@Input()`,
o un nombre de propiedad incluido en el array `inputs` de la directiva,
o una propiedad decorada con `@Input()`.
En tales casos se realiza un mapeo hacia las propiedades de la directiva.

Si el nombre no se corresponde con una propiedad de una directiva o elemento conocido, Angular reporta un error de “directiva desconocida” (_unknown directive_).

<div class="alert is-helpful">

Aunque el nombre del destino usualmente es el nombre de una propiedad,
Angular posee un mapeo automático de attributo-a-propiedad para
varios atributos comunes. Entre ellos se incluyen `class`/`className`, `innerHtml`/`innerHTML`, y
`tabindex`/`tabIndex`.

</div>

## Evitar efectos secundarios

El evaluamiento de una expresión de plantilla no debería tener efectos secundarios visibles.
El propio lenguaje de expresión, o la manera en que escribes expresiones de plantilla,
favorece este aspecto hasta cierto punto;
no puedes simplemente asignar valores a cualquier cosa dentro de una expresión de enlace de propiedad
ni usar los operadores de incremento o reducción.

Pero por ejempo, podrías tener una expresión que invocase una propiedad o método el cual tuviera
efectos secundarios. La expresión internamente podría llamar a un método `getFoo()` y sólo tú sabrías
lo que `getFoo()` hace. Si `getFoo()` cambia algo más
y estás enlazando algún dato a ese algo más,
Angular podría no mostrar el dato correcto. O Angular podría detectar el
cambio y levantar una advertencia o un error.
Como consejo y buena práctica, es mejor atenerse a enlazar propiedades y métodos que devuelvan
valores para evitar efectos secundarios.

## Devolver el valor adecuado

Toda expresión de plantilla debiese ser evaluada con el tipo de valor
que la propiedad destino espera.
Deberías devolver un string si la propiedad destino espera un string, o un número si ésta
espera un número, un objeto si lo que espera es un objeto, y así consecutivamente.

En el siguiente ejemplo, la propiedad `childItem` (__itemHijo__) del `ItemDetailComponent` espera un string, exactamente lo mismo que verás que es enviado a través del enlace de propiedad:

<code-example path="property-binding/src/app/app.component.html" region="model-property-binding" header="src/app/app.component.html"></code-example>

Al revisar el `ItemDetailComponent` se puede verificar que la propiedad con el decorador `@Input` ha sido declarada de tipo string:
<code-example path="property-binding/src/app/item-detail/item-detail.component.ts" region="input-type" header="src/app/item-detail/item-detail.component.ts (setting the @Input() type)"></code-example>

Y como se aprecia aquí, el `parentItem` (__itemPadre__) del `AppComponent` es un string, justamente lo que `ItemDetailComponent` espera:
<code-example path="property-binding/src/app/app.component.ts" region="parent-data-type" header="src/app/app.component.ts"></code-example>

### Enlazar objetos

El sencillo ejemplo anterior demostró el enlazado de un string. No obstante, para enlazar un objeto,
se usa el mismo fundamento y sintaxis.

En el siguiente escenario, el `ItemListComponent` se anida dentro del `AppComponent` y la propiedad `items` espera recibir un array de objetos.

<code-example path="property-binding/src/app/app.component.html" region="pass-object" header="src/app/app.component.html"></code-example>

La propiedad `items` es declarada en el `ItemListComponent` como array de tipo `Item` y decorada con `@Input()`:

<code-example path="property-binding/src/app/item-list/item-list.component.ts" region="item-input" header="src/app/item-list.component.ts"></code-example>

En esta aplicación de muestra, un `Item` es un objeto con dos propiedades; `id` y `name`.

<code-example path="property-binding/src/app/item.ts" region="item-class" header="src/app/item.ts"></code-example>

Mientras una lista de items existe en otro archivo, `mock-items.ts`, puedes
indicar un item diferente en `app.component.ts` de forma que el nuevo item se renderizará:

<code-example path="property-binding/src/app/app.component.ts" region="pass-object" header="src/app.component.ts"></code-example>

Sólo tienes que asegurarte, en este caso, de que provees un array de objectos pues ese es el tipo de `item` y lo que el componente anidado, `ItemListComponent`, espera.

En este ejemplo, `AppComponent` declara un objeto `item` diferente
(`currentItems`) y lo enlaza al `ItemListComponent` anidado. `ItemListComponent` es capaz de recibir `currentItems` pues éste
corresponde a la definición de `Item` dada por `item.ts`. El archivo `item.ts` es aquél donde
`ItemListComponent` obtiene su definición de un `item`.

## Recordar los corchetes

Los corchetes (__square brackets__), `[]`, indican para Angular que debe evaluar la expresión de plantilla.
Si omites los corchetes, Angular trata al string como una constante
e *inicializa la propiedad de destino* con ese string:

<code-example path="property-binding/src/app/app.component.html" region="no-evaluation" header="src/app.component.html"></code-example>


Omitir los corchetes renderizará el texto
`parentItem`, no el valor de la propiedad `parentItem`.

## Inicialización aislada de string

*Sí deberías* omitir los corchetes cuando todas las condiciones siguientes se cumplen:

* La propiedad destino acepta un valor de tipo string.
* El string es un valor fijo que puedes ingresar directamente en la plantilla.
* Este valor inicial nunca cambia.

Esta es la manera en que inicializas atributos en la sintaxis estándar de HTML, y funciona
sin problemas para inicializar directivas y propiedades de componentes.
El ejemplo siguiente inicializa la propiedad `prefix` del `StringInitComponent` con un string fijo,
y no una expresión de plantilla. Angular le asigna el valor y se olvida de ella.

<code-example path="property-binding/src/app/app.component.html" region="string-init" header="src/app/app.component.html"></code-example>

El enlace de `[item]`, en cambio, se mantiene unido a la propiedad `currentItems` del componente.

## Enlace de propiedad vs. interpolación

A menudo tienes la posibilidad de elegir entre realizar uniones de datos con interpolación o con enlace de propiedad.
Cada uno de los pares de unión siguientes tienen el mismo efecto:

<code-example path="property-binding/src/app/app.component.html" region="property-binding-interpolation" header="src/app/app.component.html"></code-example>

La interpolación en muchos casos es una alternativa conveniente a la unión de propiedad.
Cuando se renderizan valores de datos como string, no existe motivo
técnico para preferir una opción sobre la otra, aunque la legibilidad
tiende a favorecer la interpolación.
Sin embargo, *debes usar el enlace de propiedad para asignar un valor que no sea string a la propiedad de un elemento*.

## Seguridad del contenido

Supongamos que tenemos el siguiente contenido malicioso.

<code-example path="property-binding/src/app/app.component.ts" region="malicious-content" header="src/app/app.component.ts"></code-example>

En la plantilla del componente, este contenido podría ser interpolado:

<code-example path="property-binding/src/app/app.component.html" region="malicious-interpolated" header="src/app/app.component.html"></code-example>

Afortunadamente, los enlaces de Angular están alertas ante la posibilidad de encontrar HTML peligroso. En el caso anterior,
el código HTML se muestra tal cual, y la porción Javascript no se ejecuta. Angular **no permite**
que se filtre código HTML con etiquetas script al navegador, ni por medio de interpolación
o enlaces de propiedad.

En el ejemplo siguiente, sin embargo, Angular [sanitiza](guide/security#sanitization-and-security-contexts)
los values antes de mostrarlos.

<code-example path="property-binding/src/app/app.component.html" region="malicious-content" header="src/app/app.component.html"></code-example>

La interpolación maneja las etiquetas `<script>` de forma distinta que
los enlaces de propiedad pero ambas metodologías renderizan el
contenido de forma segura. La salida representada por el navegador
de los ejemplos de `evilTitle` anteriores se muestra a continuación.

<code-example language="bash">
"Template <script>alert("evil never sleeps")</script> Syntax" is the interpolated evil title.
"Template alert("evil never sleeps")Syntax" is the property bound evil title.
</code-example>
