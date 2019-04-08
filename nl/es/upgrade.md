---

copyright:
  years: 2015, 2019
lastupdated: "2019-02-21"

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

# Actualización
{: #upgrade}

Aprenda a actualizar su plan de servicio.
{: shortdesc}

## Actualización del plan
{: #upgrade-plan}

Puede explorar las {{site.data.keyword.conversationshort}} [opciones del plan de servicio ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](https://www.ibm.com/cloud/watson-assistant/pricing/){: new_window} para decidir el plan que mejor se adapta a sus requisitos.

No puede actualizar una instancia basada en Cloud Foundry a un plan Plus. Debe migrar la instancia de modo que utilice un grupo de recursos para poder actualizarla. Consulte [Migración](/docs/services/assistant?topic=assistant-migrate) para obtener más detalles.
{: note}

Para actualizar su plan, siga estos pasos:

1.  En el menú {{site.data.keyword.Bluemix_notm}}, seleccione **Plan de actualización**.
    Desde aquí puede ver el plan actual y otras opciones del plan disponibles, así como realizar cambios.

Para ver respuestas a preguntas comunes sobre suscripciones, consulte [Gestión de facturación y uso ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](/docs/billing-usage?topic=billing-usage-charges){: new_window}.

¿Le queda alguna duda? Póngase en contacto con el equipo de [ventas de IBM ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](https://www-01.ibm.com/marketing/iwm/dre/signup?source=urx-20970){: new_window}.

## Actualización de un conocimiento de diálogo
{: #upgrade-skill}

El servicio {{site.data.keyword.conversationshort}} añade y actualiza periódicamente sus características. Aunque algunos de estos cambios se aplican automáticamente a sus conocimientos de diálogo, las actualizaciones que tienen un gran impacto requieren una actualización manual del modelo de aprendizaje automático que utiliza el conocimiento.

Únicamente se visualiza el icono (![Icono actualizar](images/upgrade.png)) si hay disponible una actualización para su conocimiento de diálogo.

Para actualizar su conocimiento de diálogo, siga los siguientes pasos:

1.  [Descargue una copia de su conocimiento de diálogo](/docs/services/assistant?topic=assistant-skill-add#skill-add-download-skill) y luego impórtelo como un nuevo conocimiento.
2.  Actualice la nueva copia de su conocimiento de diálogo.

    Cuando actualiza el conocimiento, la última versión de la API se habilita en la herramienta y el panel "Pruébelo" empieza a utilizar las últimas características.
3.  Pruebe el conocimiento actualizado.
4.  Después de evaluar el conocimiento actualizado para entender la forma en la que la actualización afectará a su aplicación, aplique la actualización a su conocimiento de diálogo primario.
5.  Actualice la aplicación. Para ello, cambie las llamadas de API de mensaje que utiliza para especificar la última versión de la API. Para ver detalles sobre la versión de la API, consulte las [notas del release](/docs/services/assistant?topic=assistant-release-notes).
