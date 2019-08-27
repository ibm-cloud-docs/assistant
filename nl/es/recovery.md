---

copyright:
  years: 2015, 2019
lastupdated: "2019-04-12"

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

# Alta disponibilidad y recuperación tras desastre
{: #recovery}

{{site.data.keyword.conversationfull}} está altamente disponible en varias ubicaciones {{site.data.keyword.cloud}} (por ejemplo, Dallas y Washington, DC). No obstante, la recuperación de desastres potenciales que afecten a toda una ubicación requiere planificación y preparación.
{: shortdesc}

Usted es responsable de comprender la configuración, la personalización y el uso de {{site.data.keyword.conversationshort}}. También es responsable de estar preparado para volver a crear una instancia del servicio en una nueva ubicación y de restaurar los datos en cualquier ubicación. Para obtener más información, consulte [¿Cómo garantizo un tiempo cero de inactividad? ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](/docs/overview?topic=overview-zero-downtime#zero-downtime){: new_window}.

## Alta disponibilidad
{: #recovery-ha}

{{site.data.keyword.conversationshort}} da soporte a una alta disponibilidad sin siquiera un punto de anomalía. El servicio logra alta disponibilidad de forma automática y transparente, utilizando la característica de región multizona proporcionada por {{site.data.keyword.cloud_notm}}.

{{site.data.keyword.cloud_notm}} habilita varias zonas que no comparten un único punto de anomalía dentro de una única ubicación. También proporciona equilibrio de carga automático entre las zonas dentro de una región.

## Recuperación tras desastre
{: #recovery-dr}

La recuperación tras desastre puede convertirse en un problema si una ubicación de {{site.data.keyword.cloud_notm}} sufre un fallo significativo que incluye la pérdida potencial de datos. Puesto que la característica de región de varias zonas no está disponible en todas las ubicaciones, debe esperar a que IBM vuelva a poner una ubicación en línea si no está disponible. Si los servicios de datos subyacentes se ven comprometidos por la anomalía, también debe esperar a que IBM restaure dichos servicios de datos.

Si se produce una anomalía grave, es posible que IBM no pueda recuperar los datos de las copias de seguridad de la base de datos. En tal caso, deberá restaurar los datos para devolver la instancia de servicio a su estado más reciente. Puede restaurar los datos en la misma ubicación o en una ubicación distinta.

El plan de recuperación tras desastre implica conocimiento, conservación y preparación para restaurar todos los datos que se mantienen en {{site.data.keyword.cloud_notm}}. Consulte [Copia de seguridad y restauración de datos](/docs/services/assistant?topic=assistant-backup) para obtener información acerca de cómo realizar una copia de seguridad de las instancias de servicio.
