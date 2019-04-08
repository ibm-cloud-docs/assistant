---

copyright:
  years: 2015, 2019
lastupdated: "2019-02-11"

subcollection: assistant

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:deprecated: .deprecated}
{:important: .important}
{:note: .note}
{:tip: .tip}
{:pre: .pre}
{:codeblock: .codeblock}
{:screen: .screen}
{:javascript: .ph data-hd-programlang='javascript'}
{:java: .ph data-hd-programlang='java'}
{:python: .ph data-hd-programlang='python'}
{:swift: .ph data-hd-programlang='swift'}
{:gif: data-image-type='gif'}

# Corrección de la entrada de usuario
{: #beta-spell-check}

Esta característica solo está disponible para los participantes en el programa beta. Para obtener información sobre cómo solicitar acceso, consulte [Participe en el programa beta](/docs/services/assistant?topic=assistant-feedback#feedback-beta).

![Beta](images/beta.png) IBM proporciona servicios, características y soporte de idioma nacional para su evaluación que se clasifican como versiones beta. Estas características pueden ser inestables, pueden cambiar con frecuencia y pueden dejar de recibir soporte tras un aviso con poca antelación. Las características beta también pueden no proporcionar el mismo nivel de rendimiento o compatibilidad que proporcionan las características con disponibilidad general, y no están pensadas para su uso en un entorno de producción.

Habilite la característica de *corrección ortográfica* para corregir las faltas de ortografía que los usuarios hacen en las expresiones que se envían como entrada de usuario. Si la corrección ortográfica está habilitada, las palabras con errores ortográficos se corrigen automáticamente. Y son las palabras corregidas las que se utilizan para evaluar la entrada. Cuando se proporciona una entrada más precisa, el servicio puede reconocer con mayor frecuencia las menciones de entidades y comprender la intención del usuario.

Actualmente, este valor solo se puede habilitar para los conocimientos de diálogo en inglés.
{: note}

Con la comprobación ortográfica habilitada, la entrada de usuario se corrige del siguiente modo:

- Entrada original: `letme applt for a memberdhip`
- Entrada corregida: `let me apply for a membership`

Cuando el servicio evalúa si se debe corregir la ortografía de una palabra, no se basa en un proceso de búsqueda de diccionario simple. Utiliza una combinación de proceso de lenguaje natural o modelos de probabilidades para evaluar si un término está realmente mal escrito y se debe corregir.

## Habilitación de la comprobación ortográfica
{: #beta-spell-check-enable}

Para habilitar la característica de comprobación ortográfica, siga estos pasos:

1.  En la página Conocimientos, busque su conocimiento.
1.  Pulse el icono ![abrir y cerrar lista de opciones](images/kabob-beta.png) y seleccione **Valores**.
1.  En la página de valores de *Comprobación ortográfica*, active **Corrección ortográfica automática**.

### Prueba de la corrección ortográfica
{: #beta-spell-check-test}

1.  En el panel "Pruébelo", envíe una expresión que incluya algunas palabras con errores ortográficos.

    Si las palabras de la entrada están mal escritas, se corrigen automáticamente y se muestra el icono ![corrección automática](images/auto-correct.png). Se subraya la expresión corregida.
1.  Mueva el puntero del ratón sobre la expresión subrayada para ver la redacción original.

Si hay términos con errores ortográficos que espera que el servicio corrija, pero no lo ha hecho, revise las reglas que utiliza el servicio para decidir si debe corregir una palabra para ver si la palabra está dentro de la categoría de palabras que el servicio no cambia intencionadamente.

Para evitar una corrección excesiva, el servicio no corrija la ortografía de los siguientes tipos de entrada:

- Palabras en mayúsculas
- Emojis
- Entidades de ubicación, como estados y nombres de calles
- Números y unidades de medida o de tiempo
- Nombres propios, como nombres de empresas o de particulares
- Texto entre comillas
- Palabras que contienen caracteres especiales como, como por ejemplo guiones (-), asteriscos (*) y ampersands (&)
- Palabras que *pertenecen* al conocimiento, lo que significa palabras que tienen un significado implícito porque aparecen en valores de entidad, sinónimos de entidad o ejemplos de intenciones de usuario.

Si no resulta evidente que la palabra que no se corrige pertenece a uno de estos tipos de entrada, es posible que valga la pena comprobar si la entidad tiene habilitada la coincidencia aproximada.

#### ¿Qué relación tiene la corrección ortográfica con la coincidencia aproximada?
{: #beta-spell-check-vs-fuzzy-matching}

La coincidencia aproximada ayuda al servicio a reconocer menciones de entidades basadas en diccionario en la entrada de usuario. Utiliza un enfoque de búsqueda de diccionario para comparar una palabra de la entrada de usuario con un valor de entidad o un sinónimo existente en los datos de entrenamiento del conocimiento. Por ejemplo, si el usuario escribe `books` y los datos de entrenamiento contienen el sinónimo de entidad `book`, la coincidencia aproximada reconoce que estos dos términos tienen el mismo significado.

Si habilita tanto la corrección ortográfica como la coincidencia aproximada, la función de coincidencia aproximada se ejecuta antes de que se active la corrección automática. Si encuentra un término que puede comparar con un valor de entidad de diccionario existente o con un sinónimo, añade el término a la lista de palabras que *pertenecen* al conocimiento y que, por tanto, no se corrigen. Paralelamente, si un usuario escribe una frase como `I want to buy a boook`, la coincidencia aproximada reconoce que el término `boook` significa lo mismo que el sinónimo de entidad `book` y lo añade a la lista de palabras protegidas. Como resultado, el servicio *no* corrige la ortografía de `boook`.

#### Cómo funciona
{: #beta-spell-check-how-it-works}

Normalmente, la entrada de usuario se guarda tal como está en el campo `text` del objeto `input` del mensaje. Si, y solo si, la entrada de usuario se corrige de alguna forma, se crea un nuevo campo en el objeto `input`, denominado `original_text`. Este campo almacena la entrada original del usuario que incluye las palabras con errores ortográficos. Y el texto corregido se añade al campo `input.text`.
