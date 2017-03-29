# Artículos

```json
{
	"title": "My first article",
	"body": [
		{
			"name": "p",
			"id": "diane",
			"children": [
				"¿Encuentras que este sistema funciona bien? ",
				{
					"name": "strong",
					"id": "",
					"children": [
						"O déjame adivinarlo: nunca lo has probado antes."
					]
				},
				" En realidad tú no le entras a las chicas normalmente"
			]
		},
		{
			"name": "footer",
			"id": "",
			"children": [
				"– Trainspotting"
			]
		}
	]
}
```

Un artículo es un texto sobre el que se pueden crear anotaciones. Un artículo, por ejemplo, puede ser una entrada de un blog.

## Cuerpo

> Ejemplo 1. Un único elemento.

```html
<blockquote id="trainspotting">Elige la vida. Elige un empleo.</p>
```

```json
[
	{
		"name": "blockquote",
		"id": "trainspotting",
		"children": [
			"Elige la vida. Elige un empleo."
		]
	}
]
```

El cuerpo es un objeto JS que representa el HTML del artículo. Es un array de `Element`.

`Element = {name, id, children}; children = Array<string | Element>`

Element es un objeto con tres atributos: `name` (nombre de la etiqueta), `id` (atributo ID del elemento) y `children` (hijos del elemento).

Los atributos `name` e `id` son strings. `children` es un array, cuyos elementos son `string` o `Element`.

Ver los ejemplos para más información

> Ejemplo 2. Varios elementos

```html
<blockquote id="renton">Me lo he justificado a mí mismo de todas las maneras.</p>
<p id="sickboy">Ursula Andress, la chica Bond por excelencia. Todo el mundo lo dice.</p>
```

```json
[
	{
		"name": "blockquote",
		"id": "renton",
		"children": [
			"Me lo he justificado a mí mismo de todas las maneras"
		]
	},
	{
		"name": "p",
		"id": "sickboy",
		"children": [
			"Ursula Andress, la chica Bond por excelencia. Todo el mundo lo dice."
		]
	}
]
```

> Ejemplo 3. Elementos hijo

```html
<h1>Tabla</h1>
<table>
	<tr>
		<td>Elemento 1</td>
		<td>Elemento 2</td>
		<td>Elemento 3</td>
	</tr>
</table>
```

```json
[
	{
		"name": "h1",
		"children": ["Tabla"]
	},
	{
		"name": "table",
		"children": [
			{
				"name": "tr",
				"children": [
					{
						"name": "td",
						"children": ["Elemento 1"]
					},
					{
						"name": "td",
						"children": ["Elemento 2"]
					},
					{
						"name": "td",
						"children": ["Elemento 3"]
					}
				]
			}
		]
	}
]
```

> Ejemplo 4. Elementos hijo in-line.

```html
<p id="diane">¿Encuentras que este sistema funciona bien? <strong>O déjame adivinarlo: nunca lo has probado antes.</strong> En realidad tú no le entras a las chicas normalmente</p>
<footer>– Trainspotting</footer>
```

```json
[
	{
		"name": "p",
		"id": "diane",
		"children": [
			"¿Encuentras que este sistema funciona bien? ",
			{
				"name": "strong",
				"children": ["O déjame adivinarlo: nunca lo has probado antes."]
			},
			" En realidad tú no le entras a las chicas normalmente"
		]
	},
	{
		"name": "footer",
		"children": ["– Trainspotting"]
	}
]
```

## Obtener todos los artículos

```sh
curl -v https://liqen-core.herokuapp.com/articles \
	-H "Content-Type: application/json"
```

> Respuesta

```json
[
	{
		"title": "Chiquito ipsum",
		"id": 1
	},
	{
		"title": "Trainspotting",
		"id": 2
	}
]
```

### Petición HTTP

`GET https://liqen-core.herokuapp.com/articles`

## Obtener un artículo

```sh
curl -v https://liqen-core.herokuapp.com/articles/1 \
	-H "Content-Type: application/json"
```

> Respuesta

```json
{
	"id": 1,
	"title": "Chiquito ipsum",
	"body": [
		{
			"name": "p",
			"children": [
				"Lorem fistrum a gramenawer qué dise usteer jarl torpedo amatomaa no te digo trigo por no llamarte Rodrigor al ataquerl no puedor."
			]
		}
	]
}
```

### Petición HTTP

`GET https://liqen-core.herokuapp.com/articles/<ID>`

### Respuesta

En caso de éxito, se retorna un mensaje en cuyo cuerpo está el JSON con el artículo

## Crear un artículo

```sh
curl -v -X POST https://liqen-core.herokuapp.com/articles \
	-H "Content-Type: application/json" \
	-H "Authorization: Bearer EEwJ6tF9x5WCIZDYzyZGaz6Khbw7raYRIBV_WxVvgmsG" \
	-d '{
	"title": "Chiquito ipsum",
	"body": []
}'
```

> Cuerpo de la respuesta

```json
{
	"id": 1,
	"title": "Chiquito ipsum",
	"body": []
}
```

### Petición HTTP

`POST https://liqen-core.herokuapp.com/articles`

Esta operación require autenticación. En la cabecera, se debe especificar la propiedad `Authorization` con el token de sesión.

En el cuerpo debe ser un JSON con los atributos:

Atributo    |Tipo   |Descripción
--------    |----   |-----------
`title`     |string |Título del artículo
`body`      |string |Contenido del artículo

### Respuesta HTTP

En caso de éxito, se retorna un mensaje en el que la cabecera tiene los atributos:

Atributo  |Descripción
--------  |-----------
`Location`|URI del artículo creado

Y en el cuerpo un JSON con el artículo creado.

Para posteriormente editar o eliminar el artículo se puede usar la información de la cabecera `Location` o del `id` del cuerpo.

## Editar un artículo

```sh
curl -v -X PATCH https://liqen-core.herokuapp.com/articles/1 \
	-H "Content-Type: application/json" \
	-H "Authorization: Bearer EEwJ6tF9x5WCIZDYzyZGaz6Khbw7raYRIBV_WxVvgmsG" \
	-d '{
	"title": "Chiquito!!!"
}'
```

### Petición HTTP

`PATCH https://liqen-core.herokuapp.com/articles/<ID>`

Esta operación require autenticación. En la cabecera, se debe especificar la propiedad `Authorization` con el token de sesión.

En el cuerpo debe ser un JSON con los atributos siguientes. Todos son opcionales.

Atributo    |Tipo   |Descripción
--------    |----   |-----------
`title`     |string |Título del artículo
`body`      |string |Contenido del artículo

### Respuesta HTTP

En caso de éxito, se retorna un mensaje en cuyo cuerpo está el JSON con el artículo modificado


## Eliminar un artículo

```sh
curl -v -X DELETE https://liqen-core.herokuapp.com/articles/1 \
	-H "Authorization: Bearer EEwJ6tF9x5WCIZDYzyZGaz6Khbw7raYRIBV_WxVvgmsG"
```

### Petición HTTP

`DELETE https://liqen-core.herokuapp.com/articles/<ID>`

Esta operación require autenticación. En la cabecera, se debe especificar la propiedad `Authorization` con el token de sesión.

### Respuesta HTTP

En caso de éxito, se retorna un código `204` con el cuerpo y cabecera vacíos