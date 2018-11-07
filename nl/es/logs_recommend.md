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

# Recomendaciones
Esta página muestra recomendaciones para mejorar el sistema.
{: shortdesc}

![Separador Recomendaciones](images/RecommendTop.png)

Esta característica es solo una versión Beta.
{: tip}

Esta característica sólo está disponible para usuarios de la versión Premium.
{: tip}

Al analizar las conversaciones que han tenido los usuarios con su espacio de trabajo, y teniendo en cuenta los datos de entrenamiento actuales del sistema y las respuestas correctas, verá las acciones que puede llevar a cabo para mejorar el espacio de trabajo de forma fácil y eficaz.

<iframe class="embed-responsive-item" id="youtubeplayer" type="text/html" width="640" height="390" src="https://www.youtube.com/embed/scMu66AvZtY" frameborder="0" webkitallowfullscreen mozallowfullscreen allowfullscreen> </iframe>

Las recomendaciones se generan por la noche y requieren un gran volumen de mensajes de usuario, por ejemplo más de 50.
{: tip}

## Mejorar las intenciones existentes
Esta recomendación implica tomar frases individuales especificadas por los usuarios, que el sistema no reconoce, y presentarlas para que seleccione una intención para cada frase. Esto ayudará al espacio de trabajo a comprender mejor lo que dicen los usuarios.

Pulse **Iniciar** para empezar a identificar intenciones. ![Página Mejorar intenciones existentes](images/rec_improve_intent.png)

Cuando entre o salga de **Mejorar intenciones existentes**, la barra de progreso mostrará el número de frases sobre las que ha actuado en la sesión actual, del total de frases que se han dejado para ese día. Tenga en cuenta que si sale y vuelve a entrar, la barra de progreso volverá a empezar `desde cero`, pero esto no significa que el trabajo anterior se haya perdido; simplemente no cuenta en el progreso de la sesión actual. 

Seleccione la mejor intención para una frase de la lista proporcionada, o seleccione *Marcar como irrelevante*. Las frases se añaden a las intenciones como ejemplos (se añaden como datos de entrenamiento) en cuanto pulsa **Guardar**.

El botón *Saltar al siguiente* le permite saltarse la frase actual y pasar a la siguiente. La frase que se ha saltado no se volverá a mostrar y sale y vuelve a entrar en **Mejorar intenciones existentes** durante el mismo día, pero puede volver a aparecer en días posteriores.

![Página de edición de Mejorar intenciones existentes](images/rec_improve_intent2.png)
