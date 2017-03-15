# Etiquetas

```json
{
	"id": 1,
	"title": "Persona"
}
```

Una etiqueta es un concepto que, eventualmente, está vinculado a una ontología.

## Obtener todas las etiquetas

```sh
curl -v https://liqen-core.herokuapp.com/tags \
	-H "Content-Type: application/json"
```

> Respuesta

```json
[
	{
		"title": "Persona",
		"id": 1
	},
	{
		"title": "País",
		"id": 2
	}
]
```

### Petición HTTP

`GET https://liqen-core.herokuapp.com/tags`

## Obtener una etiqueta

```sh
curl -v https://liqen-core.herokuapp.com/tags/1 \
	-H "Content-Type: application/json"
```

> Respuesta

```json
{
	"id": 1,
	"title": "Persona"
}
```

### Petición HTTP

`GET https://liqen-core.herokuapp.com/tags/<ID>`

### Respuesta

En caso de éxito, se retorna un mensaje en cuyo cuerpo está el JSON con la etiqueta.

## Crear una etiqueta

```sh
curl -v -X POST https://liqen-core.herokuapp.com/tags \
	-H "Content-Type: application/json" \
	-H "Authorization: Bearer EEwJ6tF9x5WCIZDYzyZGaz6Khbw7raYRIBV_WxVvgmsG" \
	-d '{
	"title": "Continente"
}'
```

> Cuerpo de la respuesta

```json
{
	"id": 3,
	"title": "Continente"
}
```

### Petición HTTP

`POST https://liqen-core.herokuapp.com/tags`

Esta operación require autenticación. En la cabecera, se debe especificar la propiedad `Authorization` con el token de sesión.

En el cuerpo debe ser un JSON con los atributos:

Atributo    |Tipo   |Descripción
--------    |----   |-----------
`title`     |string |Nombre descriptivo de la etiqueta

### Respuesta HTTP

En caso de éxito, se retorna un mensaje en el que la cabecera tiene los atributos:

Atributo  |Descripción
--------  |-----------
`Location`|URI la etiqueta creado

Y en el cuerpo un JSON con la etiqueta creado.

Para posteriormente editar o eliminar la etiqueta se puede usar la información de la cabecera `Location` o del `id` del cuerpo.

## Editar una etiqueta

```sh
curl -v -X PATCH https://liqen-core.herokuapp.com/tags/3 \
	-H "Content-Type: application/json" \
	-H "Authorization: Bearer EEwJ6tF9x5WCIZDYzyZGaz6Khbw7raYRIBV_WxVvgmsG" \
	-d '{
	"title": "País"
}'
```

### Petición HTTP

`PATCH https://liqen-core.herokuapp.com/tags/<ID>`

Esta operación require autenticación. En la cabecera, se debe especificar la propiedad `Authorization` con el token de sesión.

En el cuerpo debe ser un JSON con los atributos siguientes. Todos son opcionales.

Atributo    |Tipo   |Descripción
--------    |----   |-----------
`title`     |string |Nombre descriptivo de la etiqueta

### Respuesta HTTP

En caso de éxito, se retorna un mensaje en cuyo cuerpo está el JSON con la etiqueta modificada


## Eliminar una etiqueta

```sh
curl -v -X DELETE https://liqen-core.herokuapp.com/tags/1 \
	-H "Authorization: Bearer EEwJ6tF9x5WCIZDYzyZGaz6Khbw7raYRIBV_WxVvgmsG"
```

### Petición HTTP

`DELETE https://liqen-core.herokuapp.com/tags/<ID>`

Esta operación require autenticación. En la cabecera, se debe especificar la propiedad `Authorization` con el token de sesión.

### Respuesta HTTP

En caso de éxito, se retorna un código `204` con el cuerpo y cabecera vacíos