
# Enlace de propiedades `[propiedad]`

Puedes usar los enlaces de propiedad para _asignar valores_ a datos dentro de elementos (_propiedades_) que posean decoradores de directiva `@Input()`.

<div class="alert is-helpful">

Echa un vistazo al <live-example></live-example>, un ejemplo en acción usando los bloques de código de esta guía.

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

## Return the proper type

The template expression should evaluate to the type of value
that the target property expects.
Return a string if the target property expects a string, a number if it
expects a number, an object if it expects an object, and so on.

In the following example, the `childItem` property of the `ItemDetailComponent` expects a string, which is exactly what you're sending in the property binding:

<code-example path="property-binding/src/app/app.component.html" region="model-property-binding" header="src/app/app.component.html"></code-example>

You can confirm this by looking in the `ItemDetailComponent` where the `@Input` type is set to a string:
<code-example path="property-binding/src/app/item-detail/item-detail.component.ts" region="input-type" header="src/app/item-detail/item-detail.component.ts (setting the @Input() type)"></code-example>

As you can see here, the `parentItem` in `AppComponent` is a string, which the `ItemDetailComponent` expects:
<code-example path="property-binding/src/app/app.component.ts" region="parent-data-type" header="src/app/app.component.ts"></code-example>

### Passing in an object

The previous simple example showed passing in a string. To pass in an object,
the syntax and thinking are the same.

In this scenario, `ItemListComponent` is nested within `AppComponent` and the `items` property expects an array of objects.

<code-example path="property-binding/src/app/app.component.html" region="pass-object" header="src/app/app.component.html"></code-example>

The `items` property is declared in the `ItemListComponent` with a type of `Item` and decorated with `@Input()`:

<code-example path="property-binding/src/app/item-list/item-list.component.ts" region="item-input" header="src/app/item-list.component.ts"></code-example>

In this sample app, an `Item` is an object that has two properties; an `id` and a `name`.

<code-example path="property-binding/src/app/item.ts" region="item-class" header="src/app/item.ts"></code-example>

While a list of items exists in another file, `mock-items.ts`, you can
specify a different item in `app.component.ts` so that the new item will render:

<code-example path="property-binding/src/app/app.component.ts" region="pass-object" header="src/app.component.ts"></code-example>

You just have to make sure, in this case, that you're supplying an array of objects because that's the type of `Item` and is what the nested component, `ItemListComponent`, expects.

In this example, `AppComponent` specifies a different `item` object
(`currentItems`) and passes it to the nested `ItemListComponent`. `ItemListComponent` was able to use `currentItems` because it matches what an `Item` object is according to `item.ts`. The `item.ts` file is where
`ItemListComponent` gets its definition of an `item`.

## Remember the brackets

The brackets, `[]`, tell Angular to evaluate the template expression.
If you omit the brackets, Angular treats the string as a constant
and *initializes the target property* with that string:

<code-example path="property-binding/src/app/app.component.html" region="no-evaluation" header="src/app.component.html"></code-example>


Omitting the brackets will render the string
`parentItem`, not the value of `parentItem`.

## One-time string initialization

You *should* omit the brackets when all of the following are true:

* The target property accepts a string value.
* The string is a fixed value that you can put directly into the template.
* This initial value never changes.

You routinely initialize attributes this way in standard HTML, and it works
just as well for directive and component property initialization.
The following example initializes the `prefix` property of the `StringInitComponent` to a fixed string,
not a template expression. Angular sets it and forgets about it.

<code-example path="property-binding/src/app/app.component.html" region="string-init" header="src/app/app.component.html"></code-example>

The `[item]` binding, on the other hand, remains a live binding to the component's `currentItems` property.

## Property binding vs. interpolation

You often have a choice between interpolation and property binding.
The following binding pairs do the same thing:

<code-example path="property-binding/src/app/app.component.html" region="property-binding-interpolation" header="src/app/app.component.html"></code-example>

Interpolation is a convenient alternative to property binding in
many cases. When rendering data values as strings, there is no
technical reason to prefer one form to the other, though readability
tends to favor interpolation. However, *when setting an element
property to a non-string data value, you must use property binding*.

## Content security

Imagine the following malicious content.

<code-example path="property-binding/src/app/app.component.ts" region="malicious-content" header="src/app/app.component.ts"></code-example>

In the component template, the content might be used with interpolation:

<code-example path="property-binding/src/app/app.component.html" region="malicious-interpolated" header="src/app/app.component.html"></code-example>

Fortunately, Angular data binding is on alert for dangerous HTML. In the above case,
the HTML displays as is, and the Javascript does not execute. Angular **does not**
allow HTML with script tags to leak into the browser, neither with interpolation
nor property binding.

In the following example, however, Angular [sanitizes](guide/security#sanitization-and-security-contexts)
the values before displaying them.

<code-example path="property-binding/src/app/app.component.html" region="malicious-content" header="src/app/app.component.html"></code-example>

Interpolation handles the `<script>` tags differently than
property binding but both approaches render the
content harmlessly. The following is the browser output
of the `evilTitle` examples.

<code-example language="bash">
"Template <script>alert("evil never sleeps")</script> Syntax" is the interpolated evil title.
"Template alert("evil never sleeps")Syntax" is the property bound evil title.
</code-example>
