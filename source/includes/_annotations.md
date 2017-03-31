# Anotaciones

Una Anotación es una marca realizada en un artículo. Junto a la marca, se aporta información extra: una serie de *conceptos* y una *dimensión*.

## Objeto Anotación

> Ejemplo

```json
{
	"id": 1,
	"author": 5,
	"article_id": 1,
	"target": {},
	"tags": [],
	"dimension": ""
}
```

Atributos de una anotación:

Atributo    |Tipo   |Descripción
--------    |----   |-----------
`id`        |integer|Identificador de la anotación
`author`    |integer|Identificador del usuario que ha realizado la anotación
`article_id`|integer|Identificador del artículo marcado por la anotación
`tags`      |array  |Conceptos que se vinculan con el fragmento de texto. Es un array de enteros, en el que cada elemento corresponde con un identificador de tag.
`dimension` |string |Dimensión a la que se responde con el fragmento. Puede tener los siguientes valores: `what`, `who`, `when`, `where` o `why`.

Además, para *matizar* la marca, se utiliza el atributo `target`:

Atributo   |Tipo  |Descripción
--------   |----  |-----------
`target`   |object|Fragmento de texto del que se infiere una información.

## Obtener todas las anotaciones

```sh
curl -v https://liqen-core.herokuapp.com/annotations \
	-H "Content-Type: application/json"
```

> Respuesta

```json
[
	{
		"article_id": "1",
		"author": "1",
		"id": 1
	},
	{
		"title": "País",
		"author": "1",
		"id": 2
	}
]
```

### Petición HTTP

`GET https://liqen-core.herokuapp.com/annotations`

### Respuesta HTTP

En caso de éxito se retorna en el cuerpo un JSON, cuyo contenido es un array de objetos anotación.

El objeto anotación devuelto en esta petición solamente tiene los campos `id`, `author` y `article_id`.

## Obtener una anotación

```sh
curl -v https://liqen-core.herokuapp.com/annotations/1 \
	-H "Content-Type: application/json"
```

> Respuesta

```json
{
	"target": {
		"value": "/html/body/div/div[2]/article/p[35]/p",
		"type": "XPathSelector",
		"refinedBy": {
			"type": "TextQuoteSelector",
			"suffix": " antigua",
			"prefix": "lanza en astillero, ",
			"exact": "adarga"
		}
	},
	"tags": [],
	"id": 6,
	"author": 1,
	"article_id": 2
}
```

### Petición HTTP

`GET https://liqen-core.herokuapp.com/annotations/<ID>`

### Respuesta

En caso de éxito, se retorna un mensaje en cuyo cuerpo está el JSON con la anotación.

## Crear una anotación sin etiquetas

> Ejemplo de petición

```sh
curl -v https://liqen-core.herokuapp.com/annotations \
	-H "Content-Type: application/json" \
	-H "Authorization: Bearer EEwJ6tF9x5WCIZDYzyZGaz6Khbw7raYRIBV_WxVvgmsG" \
	-d '{
	"article_id": 1,
	"target": {
		"type": "XPathSelector",
		"value": "/html/body/div/div[2]/article/p[35]/p",
		"refinedBy": {
			"type": "TextQuoteSelector",
			"prefix": "lanza en astillero, ",
			"exact": "adarga",
			"suffix": " antigua"
		}
	},
	"dimension": "where"
}'
```

### Petición HTTP

`POST https://liqen-core.herokuapp.com/annotations`

Esta operación require autenticación. En la cabecera, se debe especificar la propiedad `Authorization` con el token de sesión.

En el cuerpo debe ser un JSON con los atributos:

Atributo    |Tipo   |Descripción
--------    |----   |-----------
`article_id`|integer|Identificador de artículo
`target`    |object |Explicado arriba
`dimension` |string |*(Opcional)*. Explicado arriba

### Respuesta

Si los parámetros indicados son correctos, se crea un nuevo objeto `Annotation` y se retorna información de dicho objeto.

En caso de éxito, se retorna un mensaje en el que la cabecera tiene los atributos:

Atributo  |Descripción
--------  |-----------
`Location`|URI del anotación creada

Y en el cuerpo un JSON con la anotación creada.

Para posteriormente editar o eliminar la anotación se puede usar la información de la cabecera `Location` o del `id` del cuerpo.

## Eliminar una anotación

```sh
curl -v -X DELETE https://liqen-core.herokuapp.com/annotations/1 \
	-H "Authorization: Bearer EEwJ6tF9x5WCIZDYzyZGaz6Khbw7raYRIBV_WxVvgmsG"
```

### Petición HTTP

`DELETE https://liqen-core.herokuapp.com/annotations/<ID>`

Esta operación require autenticación. En la cabecera, se debe especificar la propiedad `Authorization` con el token de sesión.

### Respuesta HTTP

En caso de éxito, se retorna un código `204` con el cuerpo y cabecera vacíos

## Añadir etiquetas a una anotación

> Ejemplo de petición

```sh
curl -v https://liqen-core.herokuapp.com/annotations/1/tags \
	-H "Content-Type: application/json" \
	-H "Authorization: Bearer EEwJ6tF9x5WCIZDYzyZGaz6Khbw7raYRIBV_WxVvgmsG" \
	-d '{
	"tag_id": 1
}'
```

### Petición HTTP

`POST https://liqen-core.herokuapp.com/annotations/<ID>/tags`

Esta operación require autenticación. En la cabecera, se debe especificar la propiedad `Authorization` con el token de sesión.

En el cuerpo debe ser un JSON con los atributos:

Atributo    |Tipo   |Descripción
--------    |----   |-----------
`tag_id`    |integer|Identificador de la etiqueta

### Respuesta

Si los parámetros indicados son correctos, se añade en la anotación indicada el tag correspondiente.


## Quitar etiquetas a una anotación

> Ejemplo de petición

```sh
curl -v -X DELETE https://liqen-core.herokuapp.com/annotations/1/tags/1 \
	-H "Content-Type: application/json" \
	-H "Authorization: Bearer EEwJ6tF9x5WCIZDYzyZGaz6Khbw7raYRIBV_WxVvgmsG"
```

### Petición HTTP

`DELETE https://liqen-core.herokuapp.com/annotations/<ID>/tags/<ID TAG>`

Esta operación require autenticación. En la cabecera, se debe especificar la propiedad `Authorization` con el token de sesión.

En caso de éxito, se retorna un código `204` con el cuerpo y cabecera vacíos