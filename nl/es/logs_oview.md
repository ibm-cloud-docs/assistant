---

copyright:
  years: 2015, 2018
lastupdated: "2018-01-25"

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

# La página Visión general

La página Visión general del panel **Mejorar** proporciona un resumen de las conversaciones entre los usuarios y su espacio de trabajo. Puede ver la cantidad de tráfico durante un periodo de tiempo determinado, así como las intenciones y entidades que se han reconocido con mayor frecuencia en las conversaciones de los usuarios.
{: shortdesc}

Las estadísticas que se muestran en la página Visión general abarcan un período de tiempo más largo que el período durante el cual se retienen los registros de conversaciones de usuarios.  Estas estadísticas representan tráfico externo (usuarios o llamadas de API) que ha interactuado con el espacio de trabajo; no se incluyen las interacciones desde el panel *Pruébelo* de la herramienta. 

Puede utilizar la página Visión general para responder a preguntas como:

* ¿En qué meses se han realizado el mayor número y el menor número de conversaciones durante el año pasado?
* ¿Cuál ha sido el promedio de conversaciones por semana durante el primer trimestre?
* ¿Qué intenciones han aparecido con mayor frecuencia durante la semana pasada?
* ¿Qué valores de entidad se han reconocido más veces durante el mes de septiembre?

Para abrir la página Visión general, seleccione **Visión general** en la barra de navegación. Si no ve **Visión general**, utilice el menú ![Menú](images/Menu_16.png) para abrir la página.

  ![Página Visión general](images/oview.png)

La parte superior de la página incluye los controles siguientes:

* *Renovar datos* - Le permite renovar las estadísticas de la página Visión general inmediatamente. La página Visión general muestra la última vez que se han actualizado los datos que se muestran. Puede seleccionar **Renovar datos** si cree que pueden estar disponibles datos nuevos.
* Control de periodo de tiempo - Utilice este control para elegir el periodo para el que se muestran datos.  Este control afecta a todos los datos que se muestran en la página: no sólo al número de conversaciones que se muestran en el gráfico, sino también a las estadísticas mostradas junto con el gráfico y a las listas de las principales intenciones y entidades.

  ![Control de periodo de tiempo](images/oview-time.png)

Puede elegir si desea ver datos correspondientes a un solo día, una semana, un mes, un trimestre o un año.  En cada caso, los puntos de datos del gráfico se ajustan a un periodo de medición adecuado.  Por ejemplo, cuando se ve un gráfico correspondiente a un día, los datos se presentan en valores por hora, pero cuando se ve un gráfico correspondiente a una semana, los datos se muestran por día.  Una semana siempre va de domingo a sábado.  No puede crear periodos de tiempo personalizados, como por ejemplo una semana que vaya de jueves a miércoles, o un mes que comience cualquier día que no sea el primero.

## Todas las conversaciones

Un gráfico muestra el número total de conversaciones correspondientes al rango de fechas seleccionado.

**Nota**: Se considera una 'conversación' a cualquier interacción con el espacio de trabajo, por lo tanto, si hay conversaciones en las que el servicio empieza diciendo por ejemplo `Hi, how can I help you?` (Hola, ¿cómo puede ayudarle) y, a continuación, el usuario cierra su navegador sin responder, también se considera una conversación que se incluye en el recuento de conversaciones. 

Puede seleccionar **Ver registros** para abrir la página [Conversaciones de usuario](logs_convo.html), con el rango de fechas filtrado de modo que se ajuste al periodo de tiempo que ha seleccionado para la página Visión general. La página [Conversaciones de usuario](logs_convo.html) muestra el número total de *expresiones*. Una expresión es un mensaje individual que el usuario envía al espacio de trabajo. Cada conversación puede estar formada por varias expresiones. Por lo tanto, el número de resultados de la página [Conversaciones de usuario](logs_convo.html) es distinto del número de conversaciones que se muestra en esta página Visión general.

**Nota**: Dependiendo del plan y del rango de fechas que ha seleccionado, es posible que no vea ningún dato. Por ejemplo, el [plan de servicio Estándar](logs_convo.html#log-limits) de {{site.data.keyword.conversationshort}} solo mantiene las conversaciones durante 30 días; si elige un rango de fechas anterior a los últimos 30 días, no verá ningún dato. 

Mientras visualiza el gráfico, puede pulsar en un punto de datos individual para ver el valor numérico, tal como se muestra a continuación:

![Un solo punto de datos](images/oview-point.png)

Debajo del gráfico se muestran estadísticas relacionadas con los datos visualizados:

* *Total de conversaciones* - El número total de conversaciones que han tenido lugar durante este periodo de tiempo
* *Número máximo de conversaciones* - El número máximo de conversaciones correspondientes a un solo punto de datos dentro del periodo de tiempo
* *Comprensión débil* - El número de expresiones con una comprensión débil. Estas expresiones no están clasificadas por una intención y no contienen ninguna entidad conocida. Pueden ser útiles para identificar posibles problemas del diálogo.

## Principales intenciones y Principales entidades

También puede ver las intenciones y entidades que se han reconocido con mayor frecuencia durante el periodo de tiempo especificado. De forma predeterminada, verá las tres primeras de cada una, pero puede cambiarlo por un número mayor, como 5 o 10.

* *Principales intenciones* - Las intenciones se muestran en una lista sencilla.  Además de ver el número de veces que se ha reconocido una intención, puede utilizar el enlace **Ver registros** para abrir la página Conversaciones de usuario con el rango de fechas filtrado para que coincida con los datos que está visualizando, y la intención filtrada para que coincida con la intención seleccionada.

* Las *principales entidades* se muestran en un diagrama de barras. Para cada entidad puede seleccionar la barra para ver el número que representa la barra.

  ![Globo de datos de entidad](images/oview-entity.png)

  Seleccione **Mostrar valores principales** para ver una lista de los valores más comunes que se han identificado para esta entidad durante el periodo de tiempo. Seleccione **Ver registros** para abrir la página [Conversaciones de usuario](logs_convo.html) con el rango de fechas para que coincida con los datos que está visualizando, y la entidad filtrada para que coincida con la entidad seleccionada.
