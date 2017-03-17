# Hechos

```json
{
	"id": "1",
	"question_id": "1",
	"annotations": []
}
```

Un hecho se define como un conjunto de Anotaciones que componen una única unidad informativa.

Por ejemplo, el texto siguiente:

"1.- Turquía es el país que más refugiados alberga --2.541.352--, con una población de unos 80 millones de personas."

Puede tener dos anotaciones:

- "Turquía" con la etiqueta *país* y la dimensión *where*
- "2.541.352" con la etiqueta *amount* y la dimensión *what*

## Atributos

Atributo     |Tipo  |Descripción
--------     |----  |-----------
`id`         |string|Identificador del propio recurso (el hecho)
`question_id`|object|*(opcional)* ID de la pregunta a la que responde el hecho
`annotations`|array |Lista de objetos que representan las anotaciones que pertenecen al hecho

## Obtener una lista de hechos

```sh
curl -v https://liqen-core.herokuapp.com/articles \
	-H "Content-Type: application/json"
```

> Respuesta

```json
[
	{
		"id": 3,
		"question_id": 1
	},
	{
		"id": 3,
		"question_id": 1
	}
	{
		"id": 3,
		"question_id": 1
	}
]
```

### Petición HTTP

`GET /facts/`

## Obtener un hecho

```sh
curl -v https://liqen-core.herokuapp.com/articles/1 \
	-H "Content-Type: application/json"
```

> Respuesta

```json
{
	"id": 1,
	"title": "Chiquito ipsum",
	"annotations": [
		{
			"id": "3"
		}
	]
}
```

### Petición HTTP

`GET /facts/<ID>`

## Crear un hecho sin anotaciones

```sh
curl -v -X POST https://liqen-core.herokuapp.com/articles \
	-H "Content-Type: application/json" \
	-H "Authorization: Bearer EEwJ6tF9x5WCIZDYzyZGaz6Khbw7raYRIBV_WxVvgmsG" \
	-d '{
	"question_id": "3"
}'
```

### Petición HTTP

`POST /facts`

Esta operación require autenticación. En la cabecera, se debe especificar la propiedad `Authorization` con el token de sesión.

En el cuerpo debe ser un JSON con los atributos:

Atributo     |Tipo   |Descripción
--------     |----   |-----------
`question_id`|string |ID de la pregunta a la que responde el hecho que se desea crear

### Respuesta HTTP

En caso de éxito, se retorna un mensaje en el que la cabecera tiene los atributos:

Atributo  |Descripción
--------  |-----------
`Location`|URI del artículo creado

Y en el cuerpo un JSON con el artículo creado.

Para posteriormente editar o eliminar el artículo se puede usar la información de la cabecera `Location` o del `id` del cuerpo.

## Eliminar un hecho

```sh
curl -v -X DELETE https://liqen-core.herokuapp.com/facts/1 \
	-H "Authorization: Bearer EEwJ6tF9x5WCIZDYzyZGaz6Khbw7raYRIBV_WxVvgmsG"
```

### Petición HTTP

`DELETE https://liqen-core.herokuapp.com/facts/<ID>`

Esta operación require autenticación. En la cabecera, se debe especificar la propiedad `Authorization` con el token de sesión.

### Respuesta HTTP

En caso de éxito, se retorna un código `204` con el cuerpo y cabecera vacíos

## Añadir anotaciones a un hecho

> Ejemplo de petición

```sh
curl -v https://liqen-core.herokuapp.com/facts/1/annotations \
	-H "Content-Type: application/json" \
	-H "Authorization: Bearer EEwJ6tF9x5WCIZDYzyZGaz6Khbw7raYRIBV_WxVvgmsG" \
	-d '{
	"tag_id": 1
}'
```

### Petición HTTP

`POST https://liqen-core.herokuapp.com/facts/<ID>/annotations`

Esta operación require autenticación. En la cabecera, se debe especificar la propiedad `Authorization` con el token de sesión.

En el cuerpo debe ser un JSON con los atributos:

Atributo       |Tipo   |Descripción
--------       |----   |-----------
`annotation_id`|integer|Identificador de la anotación

### Respuesta

Si los parámetros indicados son correctos, se añade en la anotación indicada el tag correspondiente.


## Quitar anotaciones a un hecho

> Ejemplo de petición

```sh
curl -v -X DELETE https://liqen-core.herokuapp.com/facts/1/annotations/1 \
	-H "Content-Type: application/json" \
	-H "Authorization: Bearer EEwJ6tF9x5WCIZDYzyZGaz6Khbw7raYRIBV_WxVvgmsG"
```

### Petición HTTP

`DELETE https://liqen-core.herokuapp.com/facts/<ID>/annotations/<ID ANNOTATION>`

Esta operación require autenticación. En la cabecera, se debe especificar la propiedad `Authorization` con el token de sesión.

En caso de éxito, se retorna un código `204` con el cuerpo y cabecera vacíos