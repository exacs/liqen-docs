# Anotaciones

> Ejemplo de anotación sin concepto ni dimensión. Se destaca el texto "Turquía" de un texto.

```json
{
	"id": "1",
	"article_id": "1",
	"target": {
		"type": "FragmentSelector",
		"value": "p1",
		"refinedBy": {
			"type": "TextQuoteSelector",
			"prefix": "1.- ",
			"exact": "Turquía",
			"suffix": " es el país que más refugiados alberga --2.541.352--, con una población de unos 80 millones de personas."
		}
	}
}
```

> Ejemplo de anotación completo. Se destaca el texto "Turquía" y se vincula con el concepto "País" ([Country](https://schema.org/Country)) y la dimensión "where"

```json
{
	"id": "1",
	"article_id": "1",
	"body": {
		"id": "country"
	},
	"target": {
		"type": "FragmentSelector",
		"value": "p1",
		"refinedBy": {
			"type": "TextQuoteSelector",
			"prefix": "1.- ",
			"exact": "Turquía",
			"suffix": " es el país que más refugiados alberga --2.541.352--, con una población de unos 80 millones de personas."
		}
	},
	"dimension": "where"
}
```

El recurso **Annotation** describe una Anotación, un fragmento de texto con valor semántico. Se compone de tres elementos fundamentales:

1. Un fragmento de texto del que se infiere una información.
2. Opcionalmente, un concepto al que se refiere el fragmento de texto.
3. Opcionalmente, una dimensión a la que se responde con el fragmento.

Se debe entender que el recurso Anotación es una *extensión* o subclase del objeto [Web Annotation][w3-annotation-model].

## Atributos

Estos son los atributos del recurso Annotation. La mayoría de atributos pertenecen al modelo de datos descrito en la [especificación W3C de Web Annotation][w3-annotation-model].

En tanto que la Anotación Liqen es una extensión de *Web Annotation*, todos los atributos que tiene dicho objeto, también pueden llegar a pertenecer a la Anotación de Liqen.

Los siguientes atributos son obligatorios en el objeto Anotación de Liqen pero no pertenecen a Web Annotation:

Atributo     |Tipo  |Descripción
--------     |----  |-----------
article_id   |string|ID del artículo que lleva la anotación
dimension    |string|Dimensión a la que se responde con el fragmento de texto. Valores admitidos: `what`, `who`, `where`, `when`, `why`.
id           |string|ID de la propia anotación (a sí mismo).

Los siguientes atributos, que pertenecen al modelo de Web Annotation, son obligatorios en el objeto Anotación de Liqen:

Atributo|Tipo  |Descripción
--------|----  |-----------
body    |object|Concepto al que se refiere el fragmento de texto.
body.id |string|ID del concepto
target  |object|Fragmento del texto que tiene el valor del concepto. Explicado a continuación.

## Atributo `target`. Seleccionar un fragmento de texto

> **Ejemplo**. Supongamos que encontramos un extracto de texto cuyo HTML es:

```html
<p id="p1">En un lugar de la Mancha, de cuyo nombre no quiero acordarme, no ha mucho tiempo que vivía un hidalgo de los de lanza en astillero, adarga antigua, rocín flaco y galgo corredor. Una olla de algo más vaca que carnero, salpicón...</p>
```

> Y queremos resaltar el texto "adarga". El objeto que representa esta selección (y por tanto el valor de la propiedad `target` de la anotación) es:

```json
{
	"selector": {
		"type": "FragmentSelector",
		"value": "p1",
		"refinedBy": {
			"type": "TextQuoteSelector",
			"prefix": "lanza en astillero, ",
			"exact": "adarga",
			"suffix": " antigua"
		}
	}
}
```

El **fragmento de texto** se debe definir de manera inequívoca mediante la propiedad `target` del objeto Anotación en dos niveles:


1. El fragmento. Se debe seleccionar un párrafo o sección del documento con un selector de tipo [Fragment](https://www.w3.org/TR/annotation-model/#fragment-selector), [XPath](https://www.w3.org/TR/annotation-model/#xpath-selector) o [CSS](https://www.w3.org/TR/annotation-model/#css-selector) según se indica en las secciones correspondientes de la especificación. Se deben usar las propiedades `type` y `value`.
2. Texto dentro del fragmento. Se debe seleccionar una frase dentro del fragmento utilizando la propiedad `refinedBy`. Esta propiedad a su vez debe ser un selector de tipo [Text Quote](https://www.w3.org/TR/annotation-model/#text-quote-selector).

## Obtener un listado de anotaciones

### HTTP Request

`GET /annotations`

### Response

Array de anotaciones. Devuelve un máximo de 10 elementos.

```json
[
	{
		"id": "1"
	}
]
```

## Obtener una anotación

### HTTP Request

`GET /annotations/<ID>`

### URL Parameters

Atributo|Descripción
--------|-----------
ID      |ID de la anotación

## Crear una anotación

> Ejemplo de cuerpo de la petición

```json
{
	"article_id": "1",
	"body": {
		"id": "country"
	},
	"target": {
		"type": "FragmentSelector",
		"value": "p1",
		"refinedBy": {
			"type": "TextQuoteSelector",
			"prefix": "1.- ",
			"exact": "Turquía",
			"suffix": " es el país que más refugiados alberga --2.541.352--, con una población de unos 80 millones de personas."
		}
	}
}
```

`POST /annotations`

Crea una nueva anotación

### Body Parameters

Parámetro |Tipo  |Descripción
--------- |----  |-----------
article_id|string|Identificador de artículo al que se añade la anotación
dimension |string|Dimensión a la que se responde con el fragmento de texto. Valores admitidos: `what`, `who`, `where`, `when`, `why`.
body.id   |string|ID del concepto, normalmente referencia a un concepto de una ontología
target    |object|Fragmento del texto que tiene el valor del concepto. A diferencia de la especificación del modelo propuesto por la W3C, solo se tienen en cuenta las propiedades `type`, `value` y `refinedBy` de este atributo. La propiedad `source` se calcula con el parámetro `article_id`.

### Success response

Si la petición tiene éxito, retorna un código 201 (Created) con las cabeceras:

Atributo|Descripción
--------|-----------
Location|URI de la nueva anotación creada

### Errores

TODO

## Modificar una anotación

> Ejemplo de cuerpo de la petición

```json
{
	"body": {
		"id": "country"
	}
}
```

`PATCH /annotations/<ID>`

Realiza una modificación parcial de la anotación. El servidor internamente, utiliza un algoritmo de *merge* para mezclar el objeto anotación existente y el objeto anotación del cuerpo de la petición PATCH.

- Si se indica en el cuerpo una propiedad que está presente en el objeto anotación almacenado, el valor se superpone.
- Si se indica en el cuerpo una propiedad que no existe en el objeto anotación almacenado, se crea la nueva propiedad con el valor asignado.

No es posible eliminar propiedades de objetos utilizando este método.

### Path Parameters

Atributo|Descripción
--------|-----------
ID      |ID de la anotación

### Body Parameters

Los mismos que los indicados en la sección "Crear una anotación"


[w3-annotation-model]: https://www.w3.org/TR/annotation-model
