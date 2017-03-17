# Preguntas

```json
{
	"id": 1,
	"title": "Esto es una pregunta de ejemplo",
	"tags": []
}
```

## Obtener todas las preguntas

```sh
curl -v https://liqen-core.herokuapp.com/questions \
	-H "Content-Type: application/json"
```

> Respuesta

```json
[
	{
		"id": 1,
		"title": "¿Quién ganó la Liga en la temporada 2015-16?"
	}
]
```

### Petición HTTP

`GET https://liqen-core.herokuapp.com/questions`

## Obtener una anotación

```sh
curl -v https://liqen-core.herokuapp.com/questions/1 \
	-H "Content-Type: application/json"
```

> Respuesta

```json
{
	"id": 1,
	"title": "¿Quién ganó la Liga en la temporada 2015-16?",
	"tags": []
}
```

### Petición HTTP

`GET https://liqen-core.herokuapp.com/questions/<ID>`

### Respuesta

En caso de éxito, se retorna un mensaje en cuyo cuerpo está el JSON con la pregunta.

## Crear una pregunta sin etiquetas

> Ejemplo de petición

```sh
curl -v https://liqen-core.herokuapp.com/questions \
	-H "Content-Type: application/json" \
	-H "Authorization: Bearer EEwJ6tF9x5WCIZDYzyZGaz6Khbw7raYRIBV_WxVvgmsG" \
	-d '{
	"title": "¿Dónde nació Cristobal Colón?"
}'
```

### Petición HTTP

`POST https://liqen-core.herokuapp.com/questions`

Esta operación require autenticación. En la cabecera, se debe especificar la propiedad `Authorization` con el token de sesión.

En el cuerpo debe ser un JSON con los atributos:

Atributo|Tipo  |Descripción
--------|----  |-----------
`title` |string|Pregunta

### Respuesta

Si los parámetros indicados son correctos, se crea un nuevo objeto `Question` y se retorna información de dicho objeto.

En caso de éxito, se retorna un mensaje en el que la cabecera tiene los atributos:

Atributo  |Descripción
--------  |-----------
`Location`|URI de la pregunta creada

Y en el cuerpo un JSON con la pregunta creada.

Para posteriormente editar o eliminar la pregunta se puede usar la información de la cabecera `Location` o del `id` del cuerpo.

## Eliminar una anotación

```sh
curl -v -X DELETE https://liqen-core.herokuapp.com/questions/1 \
	-H "Authorization: Bearer EEwJ6tF9x5WCIZDYzyZGaz6Khbw7raYRIBV_WxVvgmsG"
```

### Petición HTTP

`DELETE https://liqen-core.herokuapp.com/questions/<ID>`

Esta operación require autenticación. En la cabecera, se debe especificar la propiedad `Authorization` con el token de sesión.

### Respuesta HTTP

En caso de éxito, se retorna un código `204` con el cuerpo y cabecera vacíos

## Añadir etiquetas a una pregunta

> Ejemplo de petición

```sh
curl -v https://liqen-core.herokuapp.com/questions/1/tags \
	-H "Content-Type: application/json" \
	-H "Authorization: Bearer EEwJ6tF9x5WCIZDYzyZGaz6Khbw7raYRIBV_WxVvgmsG" \
	-d '{
	"tag_id": 1
}'
```

### Petición HTTP

`POST https://liqen-core.herokuapp.com/questions/<ID>/tags`

Esta operación require autenticación. En la cabecera, se debe especificar la propiedad `Authorization` con el token de sesión.

En el cuerpo debe ser un JSON con los atributos:

Atributo    |Tipo   |Descripción
--------    |----   |-----------
`tag_id`    |integer|Identificador de la etiqueta

### Respuesta

Si los parámetros indicados son correctos, se añade en la pregunta indicada el tag correspondiente.


## Quitar etiquetas a una pregunta

> Ejemplo de petición

```sh
curl -v -X DELETE https://liqen-core.herokuapp.com/questions/1/tags/1 \
	-H "Content-Type: application/json" \
	-H "Authorization: Bearer EEwJ6tF9x5WCIZDYzyZGaz6Khbw7raYRIBV_WxVvgmsG"
```

### Petición HTTP

`DELETE https://liqen-core.herokuapp.com/questions/<ID>/tags/<ID TAG>`

Esta operación require autenticación. En la cabecera, se debe especificar la propiedad `Authorization` con el token de sesión.

En caso de éxito, se retorna un código `204` con el cuerpo y cabecera vacíos