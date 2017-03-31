# Target

Un target es un objeto que sirve para seleccionar **algo** dentro de un **contenedor**.

Por simplicidad, vamos a poner que vamos a seleccionar partiendo de un **documento HTML**.

- El único atributo fijo de Target es el atributo `type`, que indica el tipo de selección.
- Dependiendo del valor de `type`, se deben indicar obligatoriamente otros atributos. [Ver tipos de selectores](#selector-type)
- Opcionalmente se puede incluir un atributo `refinedBy`. [Más información](#refinedBy)

## <a id="selector-type"></a> Tipos de selectores

Existen 4 maneras de seleccionar contenido.

1. `XPathSelector`
2. `CssSelector`
3. `FragmentSelector`
4. `TextQuoteSelector`

El atributo `type` de Target debe indicar el tipo de selección y tener uno de los cuatro valores anteriores.

Dependiendo del **tipo de contenedor** y dependiendo del **tipo de contenido** usaremos uno u otro. A grandes rasgos:

Selector  | Tipo de contenedor     | Qué se quiere seleccionar
:-------  | :--------------------- | :------------------------
XPath     | Elemento HTML          | Elemento HTML o texto
Css       | Elemento HTML          | Elemento HTML o texto
Fragment  | Elemento HTML          | Elemento HTML o texto
TextQuote | Elemento HTML o texto  | Texto

Como puede verse, se pueden usar `XPathSelector`, `CssSelector` y `FragmentSelector` para el mismo fin.

En cada selector se debe especificar información diferente.

## Selector `TextQuote`

> Ejemplo. Señalar la palabra **adarga** en el siguiente texto:

```html
En un lugar de la Mancha, de cuyo nombre no quiero acordarme,
no ha mucho tiempo que vivía un hidalgo de los de lanza en astillero, adarga
antigua, rocín flaco y galgo corredor. Una olla de algo más vaca que carnero,
salpicón...
```

```json
{
	"type": "TextQuoteSelector",
	"prefix": "lanza en astillero, ",
	"exact": "adarga",
	"suffix": " antigua"
}
```

Es el selector más básico. Permite seleccionar un fragmento de tipo texto. Atributos que deben acompañarlo:

Atributos | Descripción
:-------- | :----------
`exact`   | Fragmento seleccionado
`prefix`  | Texto que precede a la selección
`suffix`  | Texto que va después de la selección

Los campos `prefix` y `suffix` sirven para buscar el fragmento de texto en un texto en el que dicho fragmento se puede repetir.

## Selectores `XPath`, `Css` y `Fragment`

Permiten seleccionar un elemento HTML dentro de un documento HTML o de otro elemento HTML. Para ello, se utiliza la sintaxis `XPath`, `Css` y `Fragment`.

Existen numerosas maneras de realizar selecciones usando los tres métodos. Incluso de cada método, existen numerosas formas de seleccionar el mismo elemento. Los ejemplos muestran cómo seleccionar el mismo elemento usando diversas maneras.

En todos ellos se debe usar el atributo `value`.

> Ejemplo. Señalar la etiqueta `article` del siguiente documento:

```html
<html>
  <body>
    <article id="main-content">
      <h1>Hola Mundo!!</h1>
    </article>
  </body>
</html>
```

> Usando `XPathSelector`

```json
{
	"type": "XPathSelector",
	"value": "/html/body/article"
}
```

> Otra manera

```json
{
	"type": "XPathSelector",
	"value": "//*[@id=\"main-content\"]"
}
```

> Usando `CssSelector`

```json
{
	"type": "CssSelector",
	"value": "#main-content"
}
```

> Usando `FragmentSelector`

```json
{
	"type": "FragmentSelector",
	"value": "main-content"
}
```

## <a id="refinedBy"></a> Atributo `refinedBy`

En ocasiones, es necesario ser muy preciso en la selección. Para ello, se pueden anidar selectores.

El ejemplo ilustra el uso de `refinedBy`. Naturalmente, en este ejemplo concreto, solamente con el `TextQuoteSelector` hubiese sido suficiente. Hay otros casos en los que sí es recomendable usar varios selectores anidados.

> Ejemplo, dado el siguiente documento HTML del que queremos seleccionar **adarga** que está dentro de la etiqueta `<p>`.

```html
<html>
  <body>
    <aside>
    	<blockquote>En un lugar de la Mancha, de cuyo nombre no
		quiero acordarme, no ha mucho tiempo que vivía un hidalgo
		de los de lanza en astillero, adarga antigua, rocín flaco
		y galgo corredor. Una olla de algo más vaca que carnero,
		salpicón...
		</blockquote>
    </aside>
    <article id="main-content">
      <h1>Hola Mundo!!</h1>
		<p id="p1">En un lugar de la Mancha, de cuyo nombre no
		quiero acordarme, no ha mucho tiempo que vivía un hidalgo
		de los de lanza en astillero, adarga antigua, rocín flaco
		y galgo corredor. Una olla de algo más vaca que carnero,
		salpicón...</p>
    </article>
  </body>
</html>
```

Podríamos usar un `TextQuoteSelector`, pero sería insuficiente para señalar de manera **unívoca** el texto **adarga**. Por ello, vamos a usar selectores anidados.

Un selector anidado es un selector dentro de un selector. Usaremos dos selectores:

1. Un primer selector para seleccionar solamente a etiqueta `<article id="main-content">`.
2. Una vez hemos "delimitado" un elemento, podemos usar un `TextQuoteSelector` anidado para seleccionar texto dentro.

> Con un `XPathSelector` podríamos seleccionar el `article`

```json
{
	"type": "XPathSelector",
	"value": "//*[@id=\"main-content\"]"
}
```

> Y una vez hemos seleccionado el párrafo podemos seleccionar el texto **adarga** usando un `TextQuoteSelector`

```json
{
	"type": "XPathSelector",
	"value": "//*[@id=\"main-content\"]",
	"refinedBy": {
		"type": "TextQuoteSelector",
		"prefix": "lanza en astillero, ",
		"exact": "adarga",
		"suffix": " antigua"
	}
}
```
