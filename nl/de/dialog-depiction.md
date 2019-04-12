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

# Dialogmodul darstellen
{: #dialog-depiction}

![Beispiel für die Baumstruktur eines Dialogmoduls mit Beispielinhalt](images/dialog-depiction-full.png)

Dieses Diagramm zeigt das Modell der Baumstruktur eines Dialogmoduls, das mit der grafischen Benutzerschnittstelle des Dialogmoduleditors erstellt wird. Das Modell enthält zwei Stammknoten des Dialogmoduls. Eine typische Dialogmodulbaumstruktur umfasst eine größere Anzahl von Knoten, aber diese Darstellung vermittelt einen Eindruck davon, wie eine Teilmenge der Knoten aussehen kann.

- Der erste Stammknoten bezieht sich auf einen Absichtswert. Der Stammknoten enthält zwei untergeordnete Knoten, die sich jeweils auf einen Entitätswert beziehen. Der zweite untergeordnete Knoten definiert zwei Antworten. Die erste Antwort wird an den Benutzer zurückgegeben, wenn der Wert der Kontextvariablen mit dem in der Bedingung angegebenen Wert übereinstimmt. Andernfalls wird die zweite Antwort zurückgegeben. 

  Diese standardmäßige Knotentyp ist hilfreich, um Fragen zu einem bestimmten Thema zu erfassen und anschließend in der Antwort eine Nachfrage zu stellen, die in den untergeordneten Knoten verarbeitet wird. Wenn beispielsweise eine Benutzerfrage zu Preisnachlässen erkannt wurde, kann durch eine Nachfrage geklärt werden, ob der Benutzer einer Organisation angehört, der das Unternehmen Sonderrabatte gewährt. Die untergeordneten Knoten können je nach Antwort des Kunden auf die Frage nach der Organisationszugehörigkeit verschiedene Antworten ausgeben. 

- Der zweite Stammknoten ist ein Knoten mit Slots. Er bezieht sich auf einen Absichtswert und definiert je einen Slot für jede Einzelinformation, die Sie beim Benutzer abfragen möchten. Jeder Slot stellt eine Frage, um die entsprechende Antwort vom Benutzer zu erhalten. Dabei wird in der Benutzerantwort ein bestimmter Entitätswert gesucht und in einer Slotkontextvariablen gespeichert. 

  Dieser Knotentyp ist hilfreich zum Erfassen von Einzelangaben, die zum Ausführen einer Transaktion im Auftrag des Benutzers erforderlich sind. Wenn der Benutzer beispielsweise einen Flug buchen möchte, können die einzelnen Slots Angaben zu Start und Ziel, Reisetermin usw. erfassen.
