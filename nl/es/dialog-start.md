---

copyright:
  years: 2015, 2019
lastupdated: "2019-05-28"

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

# Inicio del diálogo
{: #dialog-start}

No puede utilizar el nodo de bienvenida incorporado para iniciar un diálogo del mismo modo para todas las integraciones. Utilice en su lugar este método alternativo.
{: shortdesc}

La respuesta que defina para el nodo de bienvenida en el diálogo se visualiza para iniciar una conversación desde el panel "Pruébelo" y desde el widget de conversación en la integración de enlace de vista previa. Sin embargo, no se visualiza en muchas de las otras integraciones de canal porque los nodos con la condición especial `welcome` se omiten en los flujos de diálogo que inician los usuarios. Y los asistentes desplegados suelen esperar a que los usuarios inicien conversaciones con ellos, no a la inversa.

A diferencia de la condición especial `welcome`, la condición especial `conversation_start` siempre se activa al principio de un diálogo. Puede utilizar una combinación de nodos con estas dos condiciones especiales (`welcome` y `conversation_start`) para gestionar el inicio de su diálogo de forma coherente.

Siga los pasos siguientes para gestionar el inicio del diálogo:

1.  Añada un nodo de diálogo sobre el nodo de bienvenida que se añade automáticamente a la parte superior del árbol de diálogo cuando se crea el diálogo.

1.  Establezca la condición de nodo para este nodo recién añadido en `conversation_start`, que es una condición especial, tal como se ha descrito anteriormente.

1.  Defina las variables de contexto que desee establecer con valores predeterminados para el diálogo en el nodo `conversation_start`.

1.  No defina una respuesta de texto para este nodo.

1.  Configure este nodo de modo que salte directamente al nodo `Welcome` bajo el mismo en el árbol de diálogo y seleccione **Si bot lo reconoce(condición)**.

![Captura de pantalla del árbol de diálogo con el nodo conversation_start que salta a un nodo welcome bajo el mismo.](images/dialog-start.png)

Este diseño da lugar a un diálogo que funciona de esta manera:

- Independientemente del tipo de integración, se procesa el nodo `conversation_start`, lo que significa que se inicializan las variables de contexto que defina en el mismo.
- En las integraciones donde el asistente inicia el flujo de diálogo, se activa el nodo `welcome` y se visualiza su respuesta de texto.
- En las integraciones en las que el usuario inicia el flujo de diálogo, se evalúa la primera entrada del usuario y luego la procesa el nodo que puede proporcionar la mejor respuesta.
