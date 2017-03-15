# Artículos

```json
{
	"id": 1,
	"title": "Chiquito ipsum",
	"body": "<p>Lorem fistrum a gramenawer qué dise usteer jarl torpedo amatomaa no te digo trigo por no llamarte Rodrigor al ataquerl no puedor.</p>"
}
```

Un artículo es un texto sobre el que se pueden crear anotaciones. Un artículo, por ejemplo, puede ser una entrada de un blog.

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
	"body": "<p>Lorem fistrum a gramenawer qué dise usteer jarl torpedo amatomaa no te digo trigo por no llamarte Rodrigor al ataquerl no puedor.</p>"
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
	"body": "<p>Lorem fistrum a gramenawer qué dise usteer jarl torpedo amatomaa no te digo trigo por no llamarte Rodrigor al ataquerl no puedor.</p>"
}'
```

> Cuerpo de la respuesta

```json
{
	"id": 1,
	"title": "Chiquito ipsum",
	"body": "<p>Lorem fistrum a gramenawer qué dise usteer jarl torpedo amatomaa no te digo trigo por no llamarte Rodrigor al ataquerl no puedor.</p>"
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