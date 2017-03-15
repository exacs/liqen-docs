# Autenticación

La API de Liqen, para determinadas acciones, exige que el cliente esté correctamente autenticado.

Para ello, primero se debe solicitar un *token de sesión*, el cual se usará en todas las peticiones posteriores.

## Iniciar sesión

Para solicitar el token de sesión, se realiza una petición POST a `/sessions`.

> Ejemplo de petición

```sh
curl -v https://liqen-core.herokuapp.com/sessions \ 
	-H "Content-Type: application/json" \
	-d '{
		"email": "john@example.com",
		"password": "secret"
	}'
```

> Ejemplo de respuesta

```json
{
	"access_token": "EEwJ6tF9x5WCIZDYzyZGaz6Khbw7raYRIBV_WxVvgmsG",
	"expires": 1489358571,
	"user": {
		"id": 5
	}
}
```

### HTTP Request

`POST /session`

### Body Parameters

Parámetro |Tipo  |Descripción
--------- |----  |-----------
`email`   |string|E-mail de usuario
`password`|string|Contraseña

### Success Response

Si la petición tiene éxito, retorna un código 200 (OK) con un cuerpo en formato JSON con los campos:

Atributo      |Tipo   |Descripción
--------      |----   |-----------
`access_token`|string |Token de sesión
`expires`     |integer|Fecha/hora de caducidad del token en segundos UNIX
`user`        |object |Representación del usuario que inicia sesión

Además, la respuesta incluye las siguientes cabeceras:

Cabecera|Descripción
--------|-----------
`Authorization`|Token de sesión.
`X-Espires`    |Fecha/hora de caducidad del token en segundos UNIX

Tanto el valor `Authorization` de la cabecera como el valor `access_token` del cuerpo tienen el mismo valor

## Peticiones que requiren autenticación

> **Ejemplo**, crear una anotación:

```sh
curl -v https://liqen-core.herokuapp.com/annotations \
	-H "Content-Type: application/json" \
	-H "Authorization: Bearer EEwJ6tF9x5WCIZDYzyZGaz6Khbw7raYRIBV_WxVvgmsG" \
	-d '{
	"article_id": 1,
		"tags": [],
		"target": {
			...
		}
  	}'
```

Una vez se posee el token de sesión, se debe pasar en la cabecera de cualquier petición que requiera autenticación.

El token debe pasarse como valor del atributo `Authorization`.

Atributo       |Valor|Descripción
--------       |-----|-----------
`Authorization`|`Bearer <Token de Acceso>`|Token de acceso
