# Errores

Todos los recursos Liqen, pueden devolver los siguientes errores

Error Code | Meaning
---------- | -------
400 | Bad Request – Request incorrecto
401 | Unauthorized – El token indicado no es válido
404 | Not Found – El recurso al que se intenta acceder no existe
422 | Unprocessable Entity – Error en el cuerpo de la petición
500 | Error del servidor

Además del código de error, la mayoría de las veces se indica, en el cuerpo de la respuesta, información extra detallando el error.
