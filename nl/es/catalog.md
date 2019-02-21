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

# Utilización de catálogos
{: #catalog}

Los ***catálogos*** proporcionan una manera fácil de agregar intenciones comunes al espacio de trabajo del servicio {{site.data.keyword.conversationshort}}.
{: shortdesc}

## Adición de un catálogo al espacio de trabajo
{: #add-catalog}

Utilice la herramienta de {{site.data.keyword.conversationshort}} para añadir catálogos.

1.  En la herramienta de {{site.data.keyword.conversationshort}}, abra el espacio de trabajo y seleccione el separador **Catálogo** en la barra de navegación. Si no ve **Catálogo**, utilice el menú ![Menú](images/Menu_16.png) para abrir la página. 

1.  Seleccione un catálogo como, por ejemplo, *Billing*, para ver las intenciones que se proporcionan con dicho catálogo. 

    ![Captura de pantalla mostrando los catálogos disponibles](images/catalog_overview.png)

    Verá información sobre las intenciones definidas en la categoría *Billing*. 

    ![Captura de pantalla mostrando las intenciones de la categoría Billing.](images/catalog_open.png)

    Es posible distinguir las intenciones que se añaden desde un catálogo de otras intenciones gracias a la convención de denominación, en este caso, `#Billing_ . . .`

1.  Seleccione ![Flecha de cierre](images/close_arrow.png) para volver al separador **Catálogo**. 

1.  A continuación, añada el catálogo *Billing* a su espacio de trabajo pulsando el botón `Añadir a bot`. Verá un mensaje indicando que las intenciones de *Billing* se han añadido a su espacio de trabajo. 

    ![Captura de pantalla que muestra el botón Añadir a bot.](images/catalog_addtobot.png)

1.  Ahora, seleccione el separador **Intenciones**, y verifique que las intenciones de *Billing* se han añadido a su espacio de trabajo. 

    ![Captura de pantalla que muestra las intenciones de Billing listadas en el separador Intenciones. ](images/catalog_intents.png)

### Resultados

Las intenciones del catálogo *Billing* se habrán añadido al separador **Intenciones** para su espacio de trabajo. El sistema empieza a entrenar por sí mismo con los nuevos datos. 

## Edición de ejemplos de catálogo

Como con cualquier otra intención, una vez se hayan añadido las intenciones del catálogo *Billing* al espacio de trabajo, podría realizar los siguientes cambios: 

- Cambiar el nombre de la intención.
- Suprimir la intención.
- Añadir, editar o suprimir ejemplos.
- Mover un ejemplo a otra intención.

Puede tabular desde el nombre de la intención a cada ejemplo, editando los ejemplos si así lo desea.

Para mover o suprimir un ejemplo, seleccione el ejemplo seleccionando el recuadro de selección y, a continuación, seleccione **Mover** o **Suprimir**. 

  ![Captura de pantalla que muestra cómo mover o suprimir un ejemplo](images/catalog_edit.png)
