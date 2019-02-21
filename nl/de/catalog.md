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

# Kataloge verwenden
{: #catalog}

***Kataloge*** bieten eine einfache Möglichkeit zum Hinzufügen gängiger Absichten im Arbeitsbereich Ihres Service '{{site.data.keyword.conversationshort}}'.
{: shortdesc}

## Katalog zum Arbeitsbereich hinzufügen
{: #add-catalog}

Zum Hinzufügen von Katalogen verwenden Sie das {{site.data.keyword.conversationshort}}-Tool.

1.  Öffnen Sie Ihren Arbeitsbereich im {{site.data.keyword.conversationshort}}-Tool und wählen Sie
dann die Registerkarte **Katalog** in der Navigationsleiste aus. Falls die Registerkarte **Katalog**
nicht angezeigt wird, öffnen Sie die Seite über das Menü ![Menü](images/Menu_16.png).

1.  Wählen Sie einen Katalog (z. B. *Abrechnung*) aus, um die darin bereitgestellten Absichten anzuzeigen.

    ![Screenshot mit den verfügbaren Katalogen](images/catalog_overview.png)

    Hier finden Sie Informationen zu den Absichten, die in der Kategorie *Abrechnung* definiert sind.

    ![Screenshot mit Absichten aus der Kategorie 'Abrechnung'](images/catalog_open.png)

    Aus einem Katalog hinzugefügte Absichten können anhand der Namenskonvention (im vorliegenden Beispiel (`#Billing_) von Absichten aus anderen Quellen unterschieden werden. . .`

1.  Klicken Sie auf den ![Pfeil 'Schließen'](images/close_arrow.png), um zur Registerkarte **Katalog** zurückzukehren.

1.  Fügen Sie als Nächstes den Katalog *Abrechnung* zu Ihrem Arbeitsbereich hinzu, indem Sie auf die Schaltfläche `Zu Bot hinzufügen` klicken. Eine Nachricht weist darauf hin, dass die Absichten aus der Kategorie *Abrechnung* zu Ihrem Arbeitsbereich hinzugefügt wurden.

    ![Screenshot der Schaltfläche 'Zu Bot hinzufügen'](images/catalog_addtobot.png)

1.  Wählen Sie nun die Registerkarte **Absichten** aus und überprüfen Sie, dass die Absichten des Katalogs *Abrechnung* zu Ihrem Arbeitsbereich hinzugefügt wurden.

    ![Screenshot der Registerkarte 'Absichten' mit aufgelisteten Absichten des Katalogs 'Abrechnung'](images/catalog_intents.png)

### Ergebnisse

Die Absichten aus dem Katalog *Abrechnung* wurden zur Registerkarte **Absichten** in Ihrem Arbeitsbereich hinzugefügt. Das System beginnt nun, sich selbst mit den neuen Daten zu trainieren.

## Beispiele aus Katalogen bearbeiten

Sie können an den hinzugefügten Absichten aus dem Katalog *Abrechnung* (wie an anderen Absichten) nun folgende Änderungen vornehmen:

- Sie können die Absicht umbenennen.
- Sie können die Absicht löschen.
- Sie können Beispiele hinzufügen, bearbeiten oder löschen.
- Sie können ein Beispiel in eine andere Absicht verschieben.

Mit der Tabulatortaste können Sie vom Namen der Absicht zu den einzelnen Beispielen springen und die Beispiele bei Bedarf bearbeiten.

Um ein Beispiel zu verschieben oder zu löschen, wählen Sie es mithilfe des Kontrollkästchens aus und wählen Sie dann **Verschieben** oder **Löschen** aus.

  ![Screenshot, der das Verschieben oder Löschen eines Beispiels zeigt](images/catalog_edit.png)
