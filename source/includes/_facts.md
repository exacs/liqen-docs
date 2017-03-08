# Hechos

```json
{
	"id": "1",
	"answers_to": "1",
	"annotations": [
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
		},
		{
			"id": "2",
			"article_id": "1",
			"body": {
				"id": "amount"
			},
			"target": {
				"type": "FragmentSelector",
				"value": "p1",
				"refinedBy": {
					"type": "TextQuoteSelector",
					"prefix": "1.- Turquía es el país que más refugiados alberga --",
					"exact": "2.541.352",
					"suffix": "--, con una población de unos 80 millones de personas."
				}
			},
			"dimension": "what"
		}
	]
}
```

Un hecho se define como un conjunto de Anotaciones que componen una única unidad informativa.

Por ejemplo, el texto siguiente:

"1.- Turquía es el país que más refugiados alberga --2.541.352--, con una población de unos 80 millones de personas."

Puede tener dos anotaciones:

- "Turquía" con la etiqueta *país* y la dimensión *where*
- "2.541.352" con la etiqueta *amount* y la dimensión *what*

## Atributos

Atributo   |Tipo  |Descripción
--------   |----  |-----------
id         |string|Identificador del propio recurso (el hecho)
answers_to |object|*(opcional)* ID de la pregunta a la que responde el hecho

## Obtener una lista de hechos

`GET /facts/`

TODO

## Obtener un hecho

`GET /facts/<ID>`

TODO

## Crear un hecho y anotaciones

> Ejemplo de cuerpo de la petición en la que se crean, junto con el hecho, dos anotaciones

```json
{
	"answers_to": "1",
	"annotations": [
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
			},
			"dimension": "where"
		},
		{
			"article_id": "1",
			"body": {
				"id": "number"
			},
			"target": {
				"type": "FragmentSelector",
				"value": "p1",
				"refinedBy": {
					"type": "TextQuoteSelector",
					"prefix": "1.- Turquía es el país que más refugiados alberga --",
					"exact": "2.541.352",
					"suffix": "--, con una población de unos 80 millones de personas."
				}
			},
			"dimension": "what"
		}
	]
}
```

`POST /facts`

Crea un hecho nuevo. Este hecho se puede crear a partir de anotaciones existentes o crear nuevas anotaciones para incluirlas en este nuevo hecho.

### Body Parameters

Parámetro  |Tipo  |Descripción
---------  |----  |-----------
answers_to |string|Identificador de la pregunta a la que responderá el hecho.
annotations|array |Lista de anotaciones

os elementos del array `annotations` son los objetos que se pasarían al método "Crear una anotación".

### Success response

Si la petición tiene éxito, retorna un código 201 (Created) con las cabeceras

Atributo|Descripción
--------|-----------
Location|URI del nuevo hecho creado

En el cuerpo de la petición se devuelve el objeto completo del hecho recién creado, que incluye también los IDs asignados a las anotaciones creadas.

## Crear un hecho con anotaciones existentes

> Ejemplo en el que se crea un hecho que engloba las anotaciones 1 y 2

```json
{
	"answers_to": "1",
	"annotations": ["1", "2"]
}
```

### Body Parameters

Parámetro  |Tipo  |Descripción
---------  |----  |-----------
answers_to |string|Identificador de la pregunta a la que responderá el hecho.
annotations|array |Lista de anotaciones

Los elementos del array `annotations` son strings, siendo cada uno el ID de la anotación correspondiente.

### Success response

Si la petición tiene éxito, retorna un código 201 (Created) con las cabeceras

Atributo|Descripción
--------|-----------
Location|URI del nuevo hecho creado
