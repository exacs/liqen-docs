---
title: Liqen Core

search: true
includes:
  - authentication
  - articles
  - tags
  - annotations
  - questions
  - facts
  - errors
  - targets
---

# Liqen

Liqen es un sistema de preguntas, respuestas a esas preguntas y anotaciones sobre artículos que forman parte de las respuestas.

El modelo de datos del Core se compone de 5 tipos de objetos principales: **Articles**, **Annotations**, **Facts**, **Questions** y **Tags**.

![](http://www.plantuml.com/plantuml/png/VL9DQyCm3BtdLvWUCr9sBhjiWvvNsi5knJXgQd3jOCjhojR_FdyaZfEmNepyzFmiFOa9QWnvrSYP0F9J4FB4QtyYHm4-CCfg1aUhUNP3gXl0ubwm-5vAXHIvaha4ZQf1ZJOcG1RFIaSaABY8QQ08zP66csthuNOlUlb3u4PflBL1KSE9IwZVRYFjwuFYUGy262eTsTzKM4XblXlpABtLjBc0n4US0tIuimfXIcfzEPsFeACiD6BioKEfkgt39-uapw8pqbn1coEgpAVqU6V1pErD4mhcPcOrYPN0JmCwVmiNoaKq_zPwkrl7kYeTdWpRK5QQDsUiyl5ko33L35mzhzVbQkI7LXqpwp10bO0JLkNOZUUajlifbvLjPr_skp8bz4Lko7GbatPLgwjqd_N5ULkyahCgq_wVD64vHd3GrHi74zjPm4E9FjE76p9kwf1--Ot_JZTsaStMTP6Rx-g2i5ZKFm00)

## Características de la API

- En las peticiones con métodos GET, los parámetros se envían mediante *query params* en la propia URL.
- En las peticiones con métodos POST, DELETE, PUT y PATCH, los parámetros se envían como *body params*.

## Limitaciones de la API

- La API solamente devuelve valores JSON. Es necesario indicar en la cabecera el atributo `content-type` con el valor `application/json`.

## Endpoints

El Core de Liqen se puede acceder mediante una API REST con los siguientes endpoints.

- `/articles`
- `/annotations`
- `/facts`
- `/questions`
- `/tags`
- `/users`
- `/sessions`
