# Artículos

Un artículo es una referencia a una porción de un documento HTML sobre el que se pueden crear anotaciones.

Para referenciar dicho elemento, se utiliza el campo `source` que tiene dos atributos: `uri` y `target`.

## Objeto artículo

> Ejemplo de Artículo.

```json
{
	"title": "My first article",
	"source": {
		"uri": "https://es.wikiquote.org/wiki/Trainspotting",
		"target": {
			"type": "XPathSelector",
			"value": "//*[@id=\"bodyContent\"]"
		}
	}
}
```

Un artículo tiene los atributos siguientes:

Atributo | Descripción
:------- | :----------
`id`     | Identificador del artículo (interno de Liqen)
`title`  | Título del artículo
`source` | Referencia al artículo

A su vez, el `source` tiene los atributos:

Atributo | Descripción
:------- | :----------
`uri`    | URI del artículo.
`target` | Selector. Delimita qué parte del contenido generado por la URI es el artículo.

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

### Respuesta

En caso de éxito, el servidor responde con una lista de artículos, de los cuales solo se incluyen los atributos `title` e `id`

## Obtener un artículo

```sh
curl -v https://liqen-core.herokuapp.com/articles/1 \
	-H "Content-Type: application/json"
```

> Respuesta

```json
{
	"id": 1,
	"title": "My first article",
	"source": {
		"uri": "https://es.wikiquote.org/wiki/Trainspotting",
		"target": {
			"type": "XPathSelector",
			"value": "//*[@id=\"bodyContent\"]"
		}
	}
}
```

### Petición HTTP

`GET https://liqen-core.herokuapp.com/articles/<ID>`

### Respuesta

En caso de éxito se retorna un objeto Artículo completo.
