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

# Representación de un diálogo
{: #dialog-depiction}

![Un árbol de diálogo de muestra con contenido de ejemplo](images/dialog-depiction-full.png)

Este diagrama muestra una maqueta de un árbol de diálogo creado la herramienta de edición de diálogos de la interfaz gráfica de usuario. Contiene dos nodos de diálogo raíz. Un árbol de diálogo típico probablemente tenga muchos más nodos, pero esta representación proporciona una visión general del aspecto que puede tener un subconjunto de nodos.

- El primer nodo raíz está condicionado por un valor de intención. Tiene dos nodos hijo, cada uno de los cuales está condicionado por un valor de entidad.  El segundo nodo hijo define dos respuestas. La primera respuesta se devuelve al usuario si el valor de la variable de contexto coincide con el valor especificado en la condición. De lo contrario, se devuelve la segunda respuesta.

  Este tipo de nodo estándar resulta útil para capturar preguntas sobre un determinado tema y, a continuación, en la respuesta raíz, realizar una pregunta de seguimiento que gestionan los nodos hijo. Por ejemplo, puede reconocer una pregunta de usuario sobre descuentos y hacer una pregunta de seguimiento sobre si el usuario es miembro de una asociación con la que la empresa tiene acuerdos especiales de descuento. Y los nodos hijo proporcionan distintas respuestas en función de la respuesta del usuario a la pregunta sobre la pertenencia a la asociación.

- El segundo nodo raíz es un nodo con ranuras. También está condicionado por un valor de intención. Define un conjunto de ranuras, una para cada parte de la información que desea recopilar del usuario. Cada ranura realiza una pregunta para obtener la respuesta del usuario. Busca un valor de entidad específico en la respuesta del usuario a la solicitud, que luego guarda en una variable de contexto de ranura.

  Este tipo de nodo resulta útil para recopilar detalles que puede necesitar para realizar una transacción en nombre del usuario. Por ejemplo, si la intención del usuario es reservar un vuelo, las ranuras pueden recopilar la información sobre el lugar de origen y de destino, las fechas de viaje, etc.
