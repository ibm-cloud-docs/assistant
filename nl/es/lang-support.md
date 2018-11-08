---

copyright:
  years: 2015, 2018
lastupdated: "2018-02-07"

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:tip: .tip}
{:pre: .pre}
{:codeblock: .codeblock}
{:screen: .screen}
{:javascript: .ph data-hd-programlang='javascript'}
{:java: .ph data-hd-programlang='java'}
{:python: .ph data-hd-programlang='python'}
{:swift: .ph data-hd-programlang='swift'}

# Idiomas soportados
El servicio {{site.data.keyword.conversationshort}} da soporte a los idiomas que aparecen en la lista. Las características individuales del servicio reciben soporte en mayor o menor medida según el idioma.
{: shortdesc}

En la tabla siguiente, el nivel de soporte del idioma y de las características se indica con estos códigos:

- **GA** - La característica está disponible a nivel general y recibe soporte para este idioma. Tenga en cuenta que las características pueden seguir actualizándose incluso después de que estén disponibles a nivel general.
- **Beta** - La característica solo recibe soporte como release Beta y continúa en proceso de prueba antes de que esté disponible a nivel general en este idioma.
- **En blanco** - La ausencia de código indica que una característica no está disponible en este idioma.

|                  | **[Definición de intenciones](intents.html)**, **[entidades](entities.html)** y **[diálogo](dialog-build.html)** | **[Puntuación absoluta y 'Marcar como irrelevante'](intents.html#mark-irrelevant)** | **Entidades del sistema ([number](system-entities.html#sys-number) (número), [currency](system-entities.html#sys-currency) (moneda), [percentage](system-entities.html#sys-percentage) (porcentaje), [date, time](system-entities.html#sys-datetime) (fecha, hora))** | **[Coincidencia aproximada de entidades](entities.html#fuzzy-matching)** |
|:---|:---:|:---:|:---:|:---:|
| **Inglés (en)**                   | GA | GA | GA </br> Beta ([ubicación](system-entities.html#sys-location), [persona](system-entities.html#sys-person)) | Beta (lematización, errores ortográficos y coincidencia parcial) |
| **Árabe (ar)**                    | GA | Beta | Beta | Beta (solo errores ortográficos) |
| **Chino (simplificado) (zh-cn)**   | Beta | Beta | Beta |  |
| **Chino (tradicional) (zh-tw)**  | Beta | Beta |  |  |
| **Checo (cs)**                     | Beta | Beta | Beta | Beta (solo errores ortográficos) |
| **Neerlandés (nl)**                     | Beta | Beta | Beta |  |
| **Francés (fr)**                    | GA | GA | GA | Beta (solo errores ortográficos) |
| **Alemán (de)**                    | GA | GA | GA | Beta (solo errores ortográficos) |
| **Italiano (it)**                   | GA | GA | GA | Beta (solo errores ortográficos) |
| **Japonés (ja)**                  | GA | GA | GA | Beta (solo errores ortográficos) |
| **Coreano (ko)**                    | GA | GA | GA | Beta (solo errores ortográficos) |
| **Portugués (de Brasil) (pt-br)** | GA | GA | GA | Beta (solo errores ortográficos) |
| **Español (es)**                   | GA | GA | GA | Beta (solo errores ortográficos) ||

**Nota:** El servicio {{site.data.keyword.conversationshort}} da soporte a varios idiomas, tal como se indica, pero la interfaz de herramientas propiamente dicha (descripciones, etiquetas, etc.) está en inglés. Todos los idiomas soportados se pueden especificar y formar con la interfaz en inglés.

**Conformidad del estándar GB18030**: GB18030 es un estándar chino que especifica una página de códigos ampliada que se utiliza en el mercado chino. Este estándar de página de códigos es importante para la industria del software porque el Comité técnico de estandarización de tecnología de la información nacional de China obliga desde el 1 de septiembre de 2001 a que todas las aplicaciones de software para el mercado de China estén habilitadas para el estándar GB18030. El servicio {{site.data.keyword.conversationshort}} da soporte a esta codificación, y se ha certificado el cumplimiento del estándar GB18030. 

## Cambio del idioma del espacio de trabajo

Una vez que se ha creado el espacio de trabajo, su idioma no se puede modificar. Si es necesario cambiar el idioma soportado de un espacio de trabajo, debe descargar el espacio de trabajo. Luego debe editar el archivo JSON resultante en un editor de texto y buscar una propiedad de JSON denominada `language`.

La propiedad `language` debe estar establecida en el idioma original del espacio de trabajo; por ejemplo, inglés sería `en`. Modifique el valor de esta propiedad para cambiarla por idioma deseado (`fr` para francés, `de` para alemán, etc.). Guarde los cambios en el archivo JSON e importe el archivo modificado en la instancia del servicio {{site.data.keyword.conversationshort}}.

## Configuración de idiomas bidireccionales
{: #configuring-bi-directional}

Para idiomas bidireccionales, como por ejemplo el árabe, puede cambiar las preferencias del espacio de trabajo. En el separador del espacio de trabajo, seleccione el menú desplegable *Acciones* y seleccione **Preferencias de Bidi** (esta opción solo está disponible para los espacios definidos en un idioma bidireccional):

![Preferencias de Bidi](images/bidi_prefs.png)

Seleccione entre las siguientes opciones para el espacio de trabajo:

- **Dirección de GUI**: Especifica la dirección de los elementos, como botones o menús, en la interfaz gráfica de usuario. Seleccione `LTR` (izquierda a derecha, left-to-right) o `RTL` (derecha a izquierda, right-to-left). Si no lo especifica, la herramienta sigue el valor de dirección de GUI del navegador web.
- **Dirección de texto**: Especifica la dirección del texto que se escribe. Seleccione `LTR` (izquierda a derecha, left-to-right) o `RTL` (derecha a izquierda, right-to-left) o bien seleccione `Auto`, que elegirá automáticamente la dirección del texto en función de los valores del sistema. La opción `Ninguno` mostrará el texto de izquierda a derecha.
- **Forma numérica**: Especifica la forma de numerales que se utilizará cuando se representen dígitos regulares. Elija entre `Nominal`, `Árabe-Índico` o `Árabe-Europeo`. La opción `Ninguno` mostrará numerales occidentales.
- **Tipo de calendario**: Especifica cómo se eligen las fechas filtradas en la IU del espacio de trabajo. Seleccione `Islámico-Civil`, `Islámico-Tabular`, `Islámico-Umm al-Qura` o `Gregoriano`. **Nota**: Este valor no se aplica al panel "Pruébelo".

![Opciones de Bidi](images/bidi_opts.png)

Cuando termine de realizar selecciones, pulse **Actualizar** para guardar y volver al separador del espacio de trabajo.

## Cómo trabajar con caracteres acentuados
{: #working-with-accents}

En un entorno conversacional, puede que los usuarios utilicen o no acentos cuando interactúan con el servicio {{site.data.keyword.conversationshort}}. Por lo tanto, las versiones acentuadas y no acentuadas de las palabras se pueden tratar de la misma manera para la detección de intenciones y el reconocimiento de entidades.

Sin embargo, para algunos idiomas, como el español, algunos acentos puede alterar el significado de la entidad. Por lo tanto, para la detección de entidades, aunque la entidad original pueda tener implícitamente un acento, el servicio también puede considerar como coincidencia la versión no acentuada de la misma entidad, pero con una puntuación de confianza ligeramente inferior.

Por ejemplo, para la palabra "barrió", que lleva acento y corresponde al pretérito indefinido del verbo "barrer", el servicio también puede considerar como coincidencia la palabra "barrio", pero con un nivel de confianza ligeramente inferior.

El sistema proporcionará la mayor puntuación de confianza en entidades con coincidencias exactas. Por ejemplo, `barrio` no se detectará si `barrió` está en el conjunto de datos de entrenamiento; y `barrió` no se detectará si `barrio` está en el conjunto de datos de entrenamiento.

Se espera que forme el sistema con los caracteres y acentos adecuados. Por ejemplo, si espera `barrió` como respuesta, debe colocar `barrió` en el conjunto de datos de entrenamiento.

Aunque no lleven acento, lo mismo se aplica a las palabras que utilizan, por ejemplo, la letra `ñ` frente a la letra `n`, como "uña" frente a "una". En este caso la letra `ñ` no es simplemente una `n` con virgulilla, sino que es una letra específica del español.

Puede habilitar la coincidencia aproximada si cree que los clientes no utilizarán los acentos correctamente o cometerán errores ortográficos en algunas palabras (por ejemplo, colocando una `n` en lugar de una `ñ`), o bien puede excluirlas de forma explícita de los ejemplos de entrenamiento.
