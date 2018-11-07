---

copyright:
  years: 2015, 2018
lastupdated: "2018-02-09"

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

# Actualización

Aprenda a actualizar su plan de servicio.
{: shortdesc}

## Actualización del plan
{: #plan-upgrade}

Puede explorar el servicio {{site.data.keyword.conversationshort}} ampliamente con el plan Lite. Sin embargo, existen límites en lo referente a lo que puede hacer.


### Límites por artefacto
Para obtener información sobre los límites de artefacto por plan, consulte estos temas:

- [Espacios de trabajo](configure-workspace.html#workspace-limits)
- [Nodos del diálogo](dialog-build.html#dialog-node-limits)
- [Intenciones](intents.html#intent-limits)
- [Entidades](entities.html#entity-limits)
- [Registros](logs_convo.html#log-limits)

### Límites en las llamadas de API
El número de llamadas de API permitidas por instancia depende de su plan de servicio. Consulte la descripción de su plan para obtener detalles.

Si tiene un plan Lite y alcanza el límite de llamadas de API, pero los registros muestran que ha realizado menos llamadas de lo esperado, recuerde que el plan Lite almacena la información de registro solo durante 7 días.

Para actualizar su plan, siga estos pasos:

1.  En el menú de {{site.data.keyword.Bluemix_notm}}, seleccione **Servicios** > **Panel de control**.
1.  Seleccione la instancia de servicio que desea actualizar para abrirla.
1.  Pulse **Plan** en el panel de navegación.
   Desde aquí puede ver el plan actual y otras opciones del plan disponibles, así como realizar cambios.

Para ver respuestas a preguntas comunes sobre suscripciones, consulte [Gestión de facturación y uso![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](/docs/billing-usage/how_charged.html){: new_window}.

Puede obtener más información sobre los servicios alojados por IBM Cloud desde los siguientes enlaces:

- [Términos de los servicios de Cloud](http://www.ibm.com/software/sla/sladb.nsf/sla/saas)
- [Seguridad y privacidad en los servicios Cloud](http://www.ibm.com/software/sla/sladb.nsf/sla/csdsp)

## Actualización del espacio de trabajo
{: #upgrade-workspace}

El servicio {{site.data.keyword.conversationshort}} añade y actualiza periódicamente sus características. Aunque algunos de estos cambios se aplican automáticamente a su espacio de trabajo, las actualizaciones que tienen un gran impacto requieren una actualización manual.


Únicamente se visualiza el icono (![Icono actualizar](images/upgrade.png)) si hay disponible una actualización para su espacio de trabajo de trabajo. 

**Nota**: Después de actualizar un espacio de trabajo, no puede revertir el espacio de trabajo a una versión anterior.

Para actualizar su espacio de trabajo, siga estos pasos:
1.  [Duplique el espacio de trabajo](configure-workspace.html#exporting-and-copying-workspaces).
2.  Actualice el espacio de trabajo duplicado.

Cuando actualiza el espacio de trabajo, la última versión de la API se habilita en la herramienta y el panel "Pruébelo" empieza a utilizar las últimas características.
3.  Pruebe el espacio de trabajo actualizado.
4.  Después de evaluar el espacio de trabajo duplicado, para entender la forma en la que la actualización afectará a su aplicación, aplique la actualización a su espacio de trabajo primario. 
5.  Actualice la aplicación cambiando la llamada de API de mensajes para utilizar la última versión API. 
