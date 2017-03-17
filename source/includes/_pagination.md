# Paginación

En todas las peticiones que devuelven listas de elementos, como `GET /annotations`, se utiliza el siguiente mecanismo de paginación.

Especificar dos parámetro extra:

- `limit` que indica el número de elementos que devolverá la petición. Por defecto, el valor es `10`
- `offset` que indica el desplazamiento del primer valor que devolverá la petición. Por defecto, el valor es `0`

Por ejemplo, para devolver una lista con los elementos del 0 al 9 (los 10 primeros), haríamos la petición `GET /annotations?offset=0&limit=10`

Para devolver una lista con los elementos del 10 al 19 (los 10 siguientes), haríamos la petición `GET /annotations?offset=10&limit=10`

Sin embargo, para pasar de una página a la siguiente o anterior no se recomienda calcular manualmente los parámetros que se deben enviar, sino utilizar la información que proveen la cabecera HTTP en la respuesta.

Concretamente la cabecera HTTP incluye los links a la página siguiente, anterior, primera y última:

```
Link: <https://blog.mwaysolutions.com/sample/api/v1/cars?offset=15&limit=5>; rel="next",
<https://blog.mwaysolutions.com/sample/api/v1/cars?offset=50&limit=3>; rel="last",
<https://blog.mwaysolutions.com/sample/api/v1/cars?offset=0&limit=5>; rel="first",
<https://blog.mwaysolutions.com/sample/api/v1/cars?offset=5&limit=5>; rel="prev",
```
