# Autenticación

La API de Liqen, para determinadas acciones, exige que el cliente esté correctamente autenticado.

Para ello, primero se debe solicitar un *token de sesión*, el cual se usará en todas las peticiones posteriores.

## Iniciar sesión

> Ejemplo de petición

```json
{
	"email": "john@example.com",
	"password": "secret"
}
```

> Ejemplo de respuesta

```json
{
	"access_token": "EEwJ6tF9x5WCIZDYzyZGaz6Khbw7raYRIBV_WxVvgmsG",
	"expires": 1489358571,
	"user": {
		"id": "5"
	}
}
```

Autentica un usuario en el sistema. Obtiene un token de sesión que se podrá usar en sucesivas peticiones a la API.

### HTTP Request

`POST /session`

### Body Parameters

Parámetro|Tipo  |Descripción
---------|----  |-----------
email    |string|E-mail de usuario
password |string|Contraseña

### Success Response

Si la petición tiene éxito, retorna un código 200 (OK) con un cuerpo en formato JSON con los campos:

Atributo    |Tipo   |Descripción
--------    |----   |-----------
access_token|string |Token de sesión
expires     |integer|Fecha/hora de caducidad del token en segundos UNIX
user        |object |Representación del usuario que inicia sesión

## Peticiones que requiren autenticación

> **Ejemplo**, crear una anotación, que requiere autenticación

```sh
curl -v http://<LIQEN_API_ROOT>/annotations \
	-H "Content-Type: application/json" \
	-H "Authorization: Bearer EEwJ6tF9x5WCIZDYzyZGaz6Khbw7raYRIBV_WxVvgmsG" \
	-d '{
	"article_id": 1,
		"tags": [
			{
				"id": "country"
			}
		],
		"target": {
			...
		}
  	}'
```

Una vez se posee el token de sesión, se puede utilizar la API REST para realizar las peticiones oportunas.

### Headers

Atributo     |Valor|Descripción
--------     |-----|-----------
Authorization|Bearer `<Token de Acceso>`|Token de acceso
