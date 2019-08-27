---

copyright:
  years: 2015, 2019
lastupdated: "2019-07-19"

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

# Integración con Intercom ![Solo plan Plus o Premium](images/plus.png)
{: #deploy-intercom}

Intercom es una plataforma de mensajería de cliente que ayuda a impulsar el crecimiento de las empresas mediante la mejora de las relaciones en todo el ciclo de vida del cliente.
{: shortdesc}

Intercom se ha asociado con IBM para añadir un nuevo agente al equipo de soporte al cliente, un asistente virtual de Watson. Puede integrar su asistente con una aplicación Intercom para permitir que la app pase sin problemas conversaciones de usuario entre el asistente y el equipo de agentes de soporte al usuario. Lea esta [publicación del blog de Watson ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](https://medium.com/@blakemcgregor/contact-center-post-394dff427c8) para obtener más información sobre la integración.

Esta integración solo está disponible para los usuarios del plan Plus o Premium. Si quiere intentarlo, puede registrarse en un plan de prueba gratuito (Plus Trial). [Obtener la licencia Plus Trial](https://cloud.ibm.com/registration?target=%2Fdeveloper%2Fwatson%2Flaunch-tool%2Fconversation%3Fplan%3Dplus-trial&cm_mmc=OSocial_Voicestorm-_-Watson+AI_Watson+Core+-+Conversation-_-WW_WW-_-Intercom+Trial+Registration+Link&cm_mmca1=000027BD&cm_mmca2=10004432).
{: note}

Si integra el asistente con Intercom, la aplicación Intercom se convierte en la aplicación de cara al cliente para su conocimiento. Todas las interacciones con los usuarios se inician y gestionan mediante Intercom.

Actualmente no hay manera de pasar la conversación en curso entre un canal de integración y otro.

## Creación única de agente
{: #deploy-intercom-account-prereq}

Usted o alguien en su organización debe seguir, una sola vez, estos pasos de requisito previo para poder añadir la integración de Intercom a su asistente.

1.  Cree una cuenta de correo electrónico funcional para el asistente.

    Cada asistente debe tener una dirección de correo electrónico exclusiva válida para que se pueda añadir a un equipo en Intercom.
1.  Desde el espacio de trabajo de Intercom, añada el asistente a su equipo como un nuevo agente.

    Vaya a la página de valores de compañero de equipo en el espacio de trabajo de Intercom e invite al asistente como nuevo agente añadiendo el correo electrónico que ha creado en el paso anterior al campo de invitación.

    Si todavía no ha configurado tiene un espacio de trabajo de Intercom, cree uno en [www.intercom.com ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](https://www.intercom.com). Como mínimo, necesita una suscripción al producto *Inbox* de Intercom para poder crear un espacio de trabajo.

1.  Desde la cuenta de correo electrónico del asistente que ha creado anteriormente, busque la invitación de Intercom. Pulse el enlace en el correo electrónico para unirse al equipo. Regístrese utilizando la dirección de correo electrónico funcional del asistente y luego únase al equipo.

1.  **Opcional**: actualice el perfil de su asistente.

    Puede editar el nombre y la imagen de perfil de su asistente. Este perfil representa el asistente en las comunicaciones de agente privado dentro de su espacio de trabajo y en las interacciones públicas con clientes a través de las apps Intercom. Cree un perfil que refleje su marca.

    Pulse el icono de perfil de Intercom en la barra de navegación para acceder a los valores del perfil y del espacio de trabajo.

    ![Captura de pantalla de la página de valores de Intercom.](images/intercom-settings.png)

## Preparación del diálogo
{: #deploy-intercom-dialog-prereq}

Si no tiene un conocimiento de diálogo asociado a su asistente, cree uno o añádalo ahora. Para obtener más detalles, consulte [Creación de un diálogo](/docs/services/assistant?topic=assistant-dialog-build).

Actualmente no hay soporte para la activación de una búsqueda mediante un conocimiento de búsqueda desde una integración Intercom.
{: note}

Siga estos pasos en su conocimiento de diálogo para que el asistente pueda manejar las solicitudes de usuario y pueda pasar la conversación a un agente humano cuando un cliente solicite uno.

1.  Añada una intención a su conocimiento que pueda reconocer la solicitud de un usuario de hablar con un humano.

    Puede crear su propia intención o puede añadir la intención predefinida denominada `#General_Connect_to_Agent` que se proporciona con el catálogo de contenido **General** desarrollado por IBM.

1.  Añada un nodo raíz al diálogo que esté condicionado por la intención que ha creado en el paso anterior. Seleccione **Conectar con un agente humano** como tipo de respuesta.

1.  Prepare cada rama del diálogo para que la active el asistente desde la app Intercom.

    El asistente puede procesar cada nodo del diálogo raíz mientras esté funcionando como compañero de equipo de Intercom, incluidos los nodos raíz de las carpetas. Especificará la acción que desea que emprenda el asistente para cada rama del diálogo más tarde cuando configure la integración de Intercom. Por lo tanto, aunque no puede ocultar un nodo de Intercom, puede configurar el asistente de modo que no haga nada cuando se desencadena el nodo.

    Rellene los campos siguientes del nodo raíz de cada rama de diálogo:

    - **Nombre de nodo**: asigne un nombre al nodo. El nodo se identificará por este nombre cuando configure las interacciones para el mismo más tarde. Si no añade un nombre, tendrá que elegir el nodo en función de su ID de nodo.
    - **Nombre de nodo externo**: añada un resumen de la finalidad de la rama de diálogo. Por ejemplo, *Encontrar una tienda*.

      Esta información se muestra a los otros agentes del equipo del asistente cuando el asistente ofrece la respuesta a una consulta de usuario. Si hay más de un nodo de diálogo que puede dar respuesta a la consulta, el asistente comparte una lista de opciones de respuesta con los compañeros de equipo que son agentes humanos para recibir su consejo sobre la respuesta que se debe utilizar.

      ![Captura de pantalla del campo de la vista de edición de nodo donde puede añadir el resumen de la finalidad del nodo.](images/disambig-node-purpose.png)

      **No** añada un nombre de nodo externo al nodo raíz que ha creado en el paso 2. Cuando se realiza un escalado, su asistente examina el nombre de nodo externo del último nodo procesado para saber qué objetivo de usuario no se ha cumplido correctamente. Si incluye un nombre de nodo externo en el nodo con conexión con la intención del agente humano, impedirá que su asistente sepa cuál es el último nodo real orientado al objetivo con el que ha interactuado el usuario antes de escalar el problema.
      {: tip}

1.  Si un nodo hijo está condicionado por una solicitud de seguimiento o por una pregunta que no desea que el asistente maneje, añada el tipo de respuesta **Conectar con un agente humano** al nodo.

    Por ejemplo, es posible que desee añadir este tipo de respuesta a los nodos que cubren problemas confidenciales que solo un humano debe manejar o que realizan el seguimiento cuando un asistente no entiende al usuario de forma repetida.

    En tiempo de ejecución, si la conversación alcanza este nodo hijo, el diálogo se pasa a un agente humano en ese punto. Más tarde, cuando configure la integración de Intercom, puede elegir un agente humano como respaldo para cada rama.

Su diálogo ya está listo para dar soporte a su asistente en Intercom.

### Consideraciones sobre el diálogo
{: #deploy-intercom-dialog}

Algunas respuestas completas que añade a un diálogo se muestran en el panel "Pruébelo" de forma distinta a cómo se muestran a los usuarios de Intercom. En la tabla siguiente se describe cómo trata Intercom los distintos tipos de respuesta.

| Tipo de respuesta | Cómo se muestra a los usuarios de Intercom  |
|---------------|---------------------------|
| **Opción**    | Las opciones se muestran como una lista numerada. En el campo de **título** o de **descripción**, especifique instrucciones que indiquen al usuario cómo elegir una opción de la lista. |
| **Imagen**     | Se muestra el **título**, la **descripción** y la propia imagen. |
| **Pausa**     | Independientemente de si se habilita o no, no se muestra un indicador de escritura durante la pausa. |

Consulte [Respuestas completas](/docs/services/assistant?topic=assistant-dialog-overview#dialog-overview-multimedia) para obtener más información sobre los tipos de respuesta.

## Adición de una integración de Intercom
{: #deploy-intercom-add-intercom}

1.  En el separador Asistentes, pulse para abrir el mosaico del asistente que desea desplegar.

1.  En la sección Integraciones, pulse **Añadir integración**.

1.  Pulse **Intercom**.

    Siga las instrucciones que se proporcionan en la pantalla. Las secciones siguientes le ayudan con los pasos a seguir.

El siguiente vídeo de 4 minutos muestra los pasos.

<iframe class="embed-responsive-item" id="youtubeplayer" title="Configuración rápida" type="text/html" width="640" height="390" src="https://www.youtube.com/embed/SkbFWNScueU" frameborder="0" webkitallowfullscreen mozallowfullscreen allowfullscreen> </iframe>

## Conexión del asistente a Intercom
{: #deploy-intercom-connect}

En cuanto de a Intercom permiso para utilizar el asistente, este se convertirá en un miembro viable del equipo de Intercom.

Los agentes (personas) pueden asignar mensajes al asistente utilizando las reglas de asignación de Intercom. Los mensajes se pueden asignar al asistente de las formas siguientes:

- Asignación automática de conversaciones de entrada a un compañero de equipo o bandeja de entrada de equipo en función de algunos criterios

  El siguiente vídeo de un minuto y medio muestra los pasos.

  <iframe class="embed-responsive-item" id="youtubeplayer2" title="Asignación automática" type="text/html" width="640" height="390" src="https://www.youtube.com/embed/4M9wu8NHxcY" frameborder="0" webkitallowfullscreen mozallowfullscreen allowfullscreen> </iframe>

- Reasignación manual realizada por un agente humano en tiempo de ejecución.

  El siguiente vídeo de menos de tres minutos muestra los pasos.

  <iframe class="embed-responsive-item" id="youtubeplayer3" title="Asignación manual" type="text/html" width="640" height="390" src="https://www.youtube.com/embed/jAnolyUJAIA" frameborder="0" webkitallowfullscreen mozallowfullscreen allowfullscreen> </iframe>

Consulte la [documentación de Intercom ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](https://www.intercom.com/help/support-and-retain-customers/work-as-a-team/assign-conversations-to-teammates-and-teams) para obtener más detalles.

1.  Cuando el diálogo esté listo, pulse **Conectar ahora**.
1.  Pulse **Acceder a Intercom** para redirigirse al sitio de Intercom.

     Inicie una sesión en Intercom utilizando la dirección de correo electrónico funcional y la contraseña del asistente, no la suya propia. Desea establecer la conexión con un ID funcional que se comparta entre más de una persona de su organización.
     {: important}

1.  Pulse **Autorizar acceso**.
1.  Pulse **Volver a visión general**.

Ahora el asistente está disponible para recibir asignaciones de compañeros de equipo de Intercom. Si aún no lo ha hecho, [configure su diálogo](#deploy-intercom-dialog-prereq).
{: important}

## Configuración del direccionamiento de mensajes
{: #deploy-intercom-config-backup}

Asigne compañeros de equipo humanos como respaldo del asistente en caso de que el asistente necesite transferir una conversación en curso a un ser humano. Puede elegir otro equipo u otro compañero de equipo como contacto para cada rama del diálogo.

Para configurar las asignaciones de direccionamiento para escalados desde el asistente a un ser humano, siga estos pasos:

1.  En la página de integración de Intercom, pulse **Mi conocimiento de diálogo está listo** para confirmar que ha preparado el diálogo.

    Pulse este botón únicamente si ha completado el procedimiento de [Preparación del diálogo](#deploy-intercom-dialog-prereq).
    {: important}

1.  En la sección *Valores*, pulse **Reglas de gestión**.

    Si no realiza ningún cambio, el contacto de soporte para todos los nodos seguirá sin asignar.

1.  Pulse **Nueva regla**.

1.  En la lista desplegable *Elegir nodo*, elija el nodo correspondiente a la rama de diálogo que desea configurar.

    Recuerde que las ramas se identifican por su nombre de nodo. Si no ha especificado un nombre de nodo, se visualiza en su lugar el ID de nodo.

1.  Elija el equipo o el compañero de equipo humano que será el contacto de soporte para esta rama del diálogo. La consulta del usuario se escalará a esta persona si el asistente no puede responder la consulta o llega a un nodo hijo con el tipo de respuesta *Conectar con un agente humano*, lo que indica que solo debe responder un humano.

1.  Para definir reglas de direccionamiento para otras ramas del diálogo, vuelva a pulsar **Nueva regla** y repita los pasos anteriores.

    No olvide configurar una asignación para los nodos raíz que tengan el tipo de respuesta *Conectar con un agente humano* en un nodo hijo en su rama. Si no transfiere el nodo raíz asociado a una persona o un equipo específico, se podría transferir un asunto confidencial a la bandeja de entrada *Sin asignar*.

1.  Después de añadir reglas, pulse **Volver a visión general** para salir de la página.

El siguiente vídeo de 3 minutos muestra los pasos.

<iframe class="embed-responsive-item" id="youtubeplayer0" title="Direccionamiento de escalamiento basado en tema" type="text/html" width="640" height="390" src="https://www.youtube.com/embed/dTwJZOqdzII" frameborder="0" webkitallowfullscreen mozallowfullscreen allowfullscreen> </iframe>

## Otorgue al asistente permiso para supervisar y responder consultas de usuario
{: #deploy-intercom-config-action}

Cuando desee que el asistente inicie la supervisión de una bandeja de entrada de Intercom y responda a los mensajes por su cuenta, active la supervisión.

Su asistente observa las consultas de usuario a medida que se registran en Intercom. Si el asistente está seguro de que conoce la respuesta a una consulta de usuario, responde directamente al usuario. (El asistente está seguro cuando la intención principal identificada por su asistente tiene una puntuación de confianza de 0,75 o superior).

Si no desea que el asistente responda a determinados tipos de consultas de usuario, puede añadir reglas para especificar otras acciones que el asistente debe realizar por rama de diálogo. Por ejemplo, es posible que desee empezar a incorporar al asistente en el equipo Intercom de forma más conservadora, permitiendo que el asistente solo sugiera respuestas a medida que transfiere mensajes de usuario a otros compañeros del equipo para que respondan. Con el tiempo, cuando el asistente haya demostrado su efectividad, puede asignarle más responsabilidad.

Para configurar el modo en que desea que el asistente maneje determinadas ramas de diálogo, defina reglas.

1.  En la página de integración de Intercom, en la sección *Permitir que el asistente supervise una bandeja de entrada*, **active** la supervisión.

1.  En *Valores*, pulse **Gestionar reglas**.

    Si no define reglas, el asistente se configura de modo que supervisa la bandeja de entrada *Sin asignar* y responde automáticamente a las consultas de usuario cuya respuesta conoce con seguridad.

1.  En el campo *Supervisar bandeja de entrada de Intercom*, elija la bandeja de entrada de Intercom que desea que el asistente supervise.

1.  Pulse **Nueva regla** para definir un patrón de interacción exclusivo para una determinada rama de diálogo.

1.  En la lista desplegable *Elegir nodo*, elija el nodo correspondiente a la rama que desea configurar.

    Recuerde que las ramas se identifican por su nombre de nodo. Si no ha especificado un nombre de nodo, se visualiza en su lugar el ID de nodo.

1.  Elija el tipo de acción que desea que el asistente realice cuando se active este nodo de diálogo. Las opciones de tipo de acción son las siguientes:

    - **No hacer nada**: el asistente no interviene en la respuesta; el mensaje del usuario permanece en la bandeja de entrada para que alguien pueda responder.
    - **Enviar a equipo o a compañero de equipo**: el asistente evalúa la entrada de usuario para determinar su objetivo y la transfiere al compañero de equipo adecuado.
    - **Sugerir a equipo o a compañero de equipo**: el asistente proporciona a su compañero de equipo sugerencias sobre cómo responder compartiendo notas con el agente humano a través de la app interna de Intercom.

      - Si la entrada de usuario activa una rama de diálogo, lo que significa un nodo de diálogo raíz con nodos hijo que representa una interacción completa, el asistente indica que es capaz de responder a la solicitud y se ofrece a hacerlo.
      - Si la entrada de usuario activa un nodo raíz sin hijos, el asistente simplemente comparte la respuesta programada del nodo con el agente humano, pero no responde directamente al usuario.
      - Si la entrada activa más de un nodo de diálogo con un alto grado de fiabilidad, el asistente muestra al compañero de equipo humano una lista de posibles respuestas y le pide a su compañero de equipo que elija la mejor.

      En cada uno de los casos, el agente humano decide si deja que el asistente asuma el control.

    - **Responder**: el asistente responde directamente al usuario, sin comunicarse con ningún otro compañero de equipo.

    Se requiere la participación de los compañeros de equipo para los nodos a los que asigna el tipo de acción *Enviar a equipo o a compañero de equipo* o *Sugerir a equipo o a compañero de equipo*. Asegúrese de volver y añadir una regla que asigne la persona o el equipo adecuados como soporte para este nodo de diálogo en particular.

1.  Para definir valores de interacción exclusivos para otras ramas del diálogo, vuelva a pulsar **Nueva regla** y repita los pasos anteriores.

1.  Después de añadir reglas, pulse **Volver a visión general** para salir de la página.

A medida que cambia el diálogo, es probable que vuelva a la página de integración de Intercom para realizar cambios incrementales en estas reglas.

El siguiente vídeo de 3 minutos muestra los pasos.

<iframe class="embed-responsive-item" id="youtubeplayer1" title="Supervisión de bandeja de entrada" type="text/html" width="640" height="390" src="https://www.youtube.com/embed/fFKjWUfIftw" frameborder="0" webkitallowfullscreen mozallowfullscreen allowfullscreen> </iframe>

## Prueba de la integración
{: #deploy-intercom-try}

Para probar de forma efectiva la integración de Intercom desde el extremo a extremo, debe tener acceso a una aplicación de usuario final de Intercom. Ya ha creado o editado un espacio de trabajo de Intercom. El espacio de trabajo debe tener un cliente de interfaz de usuario asociado. Si no es así, consulte [Apps en Intercom ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](https://www.intercom.com/help/apps-in-intercom){: new_window} para obtener ayuda con la configuración de uno.

Envíe las consultas de usuario de prueba a través de una aplicación cliente que esté asociada con el espacio de trabajo de Intercom para ver cómo maneja los mensajes Intercom. Verifique que los mensajes que están configurados a ser respondidos por el asistente están generando las respuestas adecuadas y que el asistente no está respondiendo a mensajes que no debe responder según la configuración.
