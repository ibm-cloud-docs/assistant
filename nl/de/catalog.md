---

copyright:
  years: 2015, 2019
lastupdated: "2019-02-28"

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

# Inhaltskataloge verwenden
{: #catalog}

***Inhaltskataloge*** sind eine einfache Methode, häufig vorkommende Absichten zu Ihrem {{site.data.keyword.conversationshort}}-Dialogskill hinzuzufügen.
{: shortdesc}

Aus dem Katalog hinzugefügte Absichten sind als Ausgangspunkt für angepasste Ansichten gedacht. Bearbeiten Sie die hinzugefügten Katalogabsichten, um sie an Ihren Anwendungsfall anzupassen. 

## Inhaltskatalog zu Ihrem Dialogskill hinzufügen
{: #catalog-add}

Zum Hinzufügen von Inhaltskatalogen verwenden Sie das {{site.data.keyword.conversationshort}}-Tool. 

1.  Öffnen Sie Ihren Dialogskill im {{site.data.keyword.conversationshort}}-Tool und klicken Sie anschließend auf die Registerkarte **Inhaltskatalog**.

1.  Wählen Sie einen Inhaltskatalog aus (z. B. *Banking*), um die darin bereitgestellten Absichten anzuzeigen.

    ![Screenshot mit den verfügbaren Katalogen](images/catalog_overview.png)

    Hier finden Sie Informationen zu den Absichten, die in dem Katalog enthalten sind. 

    ![Screenshot mit Absichten der Kategorie 'Banking'](images/catalog_open.png)

    Aus einem Inhaltskatalog hinzugefügte Absichten können anhand der Namenskonvention von Absichten aus anderen Quellen unterschieden werden. Jedem Absichtsnamen ist der Name des Inhaltskatalogs als Präfix vorangestellt. 

1.  Klicken Sie auf den ![Pfeil 'Schließen'](images/close_arrow.png), um zur Registerkarte **Inhaltskatalog** zurückzukehren.

1.  Fügen Sie als nächstes einen Inhaltskatalog zu Ihrem Dialogskill hinzu, indem Sie auf die Schaltfläche `Zu Skill hinzufügen` klicken.

1.  Wählen Sie nun die Registerkarte **Absichten** aus und überprüfen Sie, dass die Absichten aus dem Katalog hinzugefügt wurden und verfügbar sind. 

    ![Screenshot mit einer Auflistung von Absichten der Kategorie 'Banking' auf der Registerkarte 'Absichten'](images/catalog_intents.png)

Das System beginnt damit, sich selbst mit den neuen Daten zu trainieren. 

Durch das Hinzufügen eines Katalogs zu Ihrem Skill werden die zugehörigen Absichten ein Bestandteil Ihrer Trainingsdaten. Wenn ein Inhaltskatalog später von IBM geändert wird, werden die Änderungen nicht automatisch auf Absichten angewendet, Sie aus dem Katalog hinzugefügt haben.
{: note}

## Beispiele aus Inhaltskatalog bearbeiten
{: #catalog-edit-content}

Wie jede andere Absicht können Sie auch die aus Inhaltskatalogen zu einem Skill hinzugefügten Absichten wie folgt bearbeiten:

- Sie können die Absicht umbenennen.
- Sie können die Absicht löschen.
- Sie können Beispiele hinzufügen, bearbeiten oder löschen.
- Sie können ein Beispiel in eine andere Absicht verschieben.
