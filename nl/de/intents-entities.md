---

copyright:
  years: 2015, 2018
lastupdated: "2018-01-09"

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

# Absichten und Entitäten planen
{: #planning-your-entities}

Um die *Absichten* für Ihre Anwendung zu planen, müssen Sie sich damit beschäftigen, was Ihre Kunden möglicherweise tun wollen und zu welcher Verarbeitung Ihre Anwendung in der Lage sein soll. Der erste Schritt zur Bereitstellung einer sinnvollen Antwort ist die Wahl der richtigen Absicht für eine Benutzereingabe. Die Absichten, die Sie für Ihre Anwendung angeben, bestimmen die Dialogmodulabläufe, die Sie erstellen müssen; außerdem legen sie unter Umständen fest, mit welchen Back-End-Systemen Ihre Anwendung interagieren muss, um die Kundenanfragen erfüllen zu können (z. B. Kundendatenbanken oder Zahlungsverarbeitungssysteme).

Eine *Entität* stellt einen Term oder ein Objekt in der Benutzereingabe dar, der/das eine Erläuterung oder einen speziellen Kontext für eine bestimmte Absicht bietet. Während Absichten Verben darstellen (also etwas, das ein Benutzer tun möchte), stehen Entitäten für Nomen (beispielsweise für das Objekt, also den Kontext einer Aktion). Durch Entitäten kann eine einzelne Absicht mehrere spezielle Aktionen darstellen. Eine Entität definiert eine Klasse von Objekten mit bestimmten Werten, die mögliche Objekte in dieser Klasse repräsentieren.

Im Grunde verwenden Sie eine Absicht, wenn Sie eine Anforderung erfassen oder eine Aktion ausführen wollen. Falls Sie Informationen erfassen wollen, die sich darauf auswirken können, wie auf eine Anforderung oder eine Aktion reagiert wird, verwenden Sie eine Entität.

Beispiel: Sie wollen eine Dashboardanwendung für kognitive Automobile erstellen, über die Benutzer Zusatzeinrichtungen ein- oder ausschalten können:

1.  Sammeln Sie so viele tatsächliche Kundenfragen, -befehle oder andere Eingaben wie möglich. Wenn Sie Eingaben von echten Kunden verwenden, erhalten Sie eine viel bessere Vorstellung von der zu erwartenden Eingabe, als wenn Sie Listen von möglichen verbalen Äußerungen durch Fachleute erstellen lassen. Denken Sie immer daran, dass verschiedene Kunden dieselbe Anforderung ganz unterschiedlich formulieren können. Alle folgenden Beispiele für verbale Äußerungen sind beispielsweise Anforderungen, etwas auszuschalten:

    - `Licht aus`
    - `Mach das Radio aus`
    - `Klimaanlage ausschalten`

1.  Sobald eine Liste von Beispielen vorliegt, sortieren Sie diese auf Grundlage der Funktionen, die Ihre Anwendung unterstützen soll, in Kategorien. Diese Kategorien stellen die Absichten dar, die Sie definieren werden, während die Beispiele Ihrer Anwendung dabei helfen, diese Absichten in einer neuen Eingabe zu erkennen.

    Gestalten Sie Ihre Absichten nicht zu ähnlich. Ähnliche Absichten sind für den Service '{{site.data.keyword.conversationshort}}' möglicherweise schwer voneinander zu unterscheiden. Wenn Sie feststellen, dass mehrere Absichten in ihrer Bedeutung verwandt sind, kann es sinnvoll sein, sie in einer einzigen Absicht zu kombinieren und dann Entitäten zu nutzen, um mehrere mögliche Antworten für diese Absicht bereitzustellen.
    {: tip}

    Da alle obigen Beispiele dieselbe Absicht (etwas ausschalten) darstellen, können Sie diese Kategorie `#ausschalten` nennen.
    Denken Sie daran, dass die Eingabe eines Kunden nicht exakt mit einem der Beispiele übereinstimmen muss; Absichten werden mittels Verarbeitung natürlicher Sprache erkannt.
    {: tip}

1.  Als Nächstes stellen Sie mithilfe einer Entität dar, was der Benutzer ausschalten will. Hierzu könnten Sie eine Entität namens `@zubehör` erstellen und ihr die folgenden möglichen Werte zuweisen:

    - `Scheinwerfer`
    - `Radio`
    - `Klimaanlage`

    Für jeden Wert können Sie auch Synonyme angeben. Beispielsweise könnten Sie `KA` und `Kühlung` als Synonyme für `Klimaanlage` angeben. Alle drei Zeichenfolgen werden als derselbe Wert behandelt. Die Auflistung mehrerer Formulierungen, die ein Benutzer für denselben Wert verwenden könnte, verbessert die Genauigkeit Ihrer Anwendung.
1.  Sobald die Benutzereingabe empfangen wird, erkennt der Dialog sowohl Absichten als auch Entitäten. Ihr Dialogmodulablauf kann sie anschließend verwenden, um die beste Antwort zu geben. Eine Entität sollten Sie nur für Dinge erstellen, die im Hinblick darauf relevant sind, wie die Anwendung eine Absicht beantwortet.
    Neben den von Ihnen definierten Entitäten stellt der Service eine Reihe von vordefinierten Systementitäten bereit, die Sie sofort einsetzen können. Weitere Informationen finden Sie unter [Systementitäten aktivieren](entities.html#enable_system_entities).
    {: tip}

1.  Präzisieren Sie Ihre Absichten, Entitäten und Beispiele bei Bedarf weiter. Betrachten Sie Ihre Gruppe von Absichten und Entitäten nicht endgültig. Beim [Erstellen der Dialogmodule](dialog-build.html) werden Sie wahrscheinlich weitere Absichten und Entitäten feststellen, die Sie hinzufügen müssen. Außerdem können Sie kontiuierlich die Eingabe von neuen Kunden erfassen und zum Hinzufügen neuer Beispiele nutzen. Dieser iterative Prozess verbessert die Fähigkeit der Anwendung, Absichten und Entitäten korrekt zu erkennen.
