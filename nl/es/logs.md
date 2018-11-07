---

copyright:
  years: 2015, 2018
lastupdated: "2018-02-13"

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

# Acerca del componente Mejorar

El componente Mejorar de {{site.data.keyword.conversationshort}} proporciona un historial de interacciones con usuarios del espacio de trabajo. Puede utilizar este historial para mejorar la comprensión por parte del espacio de trabajo de las entradas de los usuarios.
{: shortdesc}

Secciones del panel **Mejorar**:

* [Visión general](logs_oview.html): Un resumen de las interacciones de los usuarios con el espacio de trabajo. 
* [Conversaciones de usuario](logs_convo.html): Una lista de expresiones de usuario. Puede actualizar las intenciones y entidades mientras visualiza expresiones de usuarios individuales. **Nota**: Una conversación de usuario individual puede estar formada por varias expresiones. 
* [Recomendaciones](logs_recommend.html): Formas de mejorar el sistema. Solo está disponible para los usuarios de la versión Premium.

## Mejoras a través de espacios de trabajo
{: #deploy_id}

Para comprender cómo utilizar los datos de las expresiones para realizar mejoras en los espacios de trabajo, es útil revisar las siguientes definiciones asociadas con el servicio {{site.data.keyword.conversationshort}}:  

* ***Instancia***: Su despliegue de {{site.data.keyword.conversationshort}}, accesible a través de credenciales exclusivas. Una instancia de {{site.data.keyword.conversationshort}} puede estar formada por varios espacios de trabajo. 
* ***Espacio de trabajo***: Un espacio de trabajo es un modelo de su contenido de {{site.data.keyword.conversationshort}}; a menudo, corresponde a un bot. 
* ***ID de espacio de trabajo***: Identificador exclusivo de un espacio de trabajo. 
* ***ID de despliegue***: Los identificadores de despliegue corresponden a etiquetas exclusivas que se pasan con las expresiones de usuario y que ayudan a identificar el entorno de despliegue del que provienen las expresiones. 
* ***Expresión***: Una expresión es un mensaje individual que un usuario envía al espacio de trabajo. 

Crear un espacio de trabajo es un proceso muy iterativo. Cuando desarrolle el espacio de trabajo, utilice el panel *Pruébelo* para verificar que el espacio de trabajo reconoce las intenciones y entidades correctas en entradas de prueba y para realizar las correcciones necesarias. 

En el panel **Mejorar**, puede ver información sobre las iteraciones reales con los usuarios y realizar correcciones similares para mejorar la precisión con la que el espacio de trabajo reconoce las intenciones y entidades. Es difícil saber exactamente *cómo* los usuarios responderán a las preguntas, o qué expresiones aleatorias pueden realizar, por ello es importante visitar con frecuencia el panel **Mejorar** con el propósito de mejorar los espacios de trabajo. 

Para una instancia de {{site.data.keyword.conversationshort}} que incluya varios espacios de trabajo, puede haber ocasiones en las que es útil utilizar datos de expresiones de un espacio de trabajo para mejorar otro espacio de trabajo dentro de esa misma instancia.
**Nota**: Si es un usuario con una cuenta {{site.data.keyword.conversationshort}} Premium, sus instancias Premium se pueden configurar de forma opcional para permitir el acceso a datos de registro desde espacios de trabajo a través de distintas instancias Premium. 

Como ejemplo, supongamos que tiene una instancia de {{site.data.keyword.conversationshort}} denominada *HelpDesk*. 
En su instancia HelpDesk tiene dos espacios de trabajo: Production y Development. Al trabajar con expresiones en el espacio de trabajo Development, es posible filtrar las expresiones según el `ID de despliegue` para el espacio de trabajo Production, de forma que estará utilizando expresiones del espacio de trabajo Production para mejorar el espacio de trabajo Development. 

![Enlace de origen de datos](images/data_source_1.png)

Todos los cambios que realice dentro del espacio de trabajo Development únicamente afectarán al espacio de trabajo Development, incluso si está utilizando expresiones del espacio de trabajo Production. Consulte [Selección de un origen de datos](logs_convo.html#select-source) para obtener información adicional. 

Para especificar un ID de despliegue para une expresión enviada utilizando la API `/message`, incluya la propiedad de despliegue dentro del objeto de metadatos en su [contexto ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](https://www.ibm.com/watson/developercloud/conversation/api/v1/#send_message){: new_window}, como en este ejemplo: 

```
"context" : {
            "username" : "jane_doe@myemail.com",
  "member_type" : "gold",
  "metadata" : {
       "deployment": "HelpDesk-Production"
  }
}
```
{: #codeblock}
