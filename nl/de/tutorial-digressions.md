---

copyright:
  years: 2015, 2019
lastupdated: "2019-02-12"

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

# Lernprogramm: Abschweifungen verstehen
{: #tutorial-digressions}

In diesem Lernprogram lernen Sie die Funktionsweise von Abschweifungen kennen.
{: shortdesc}

## Lernziele
{: #tutorial-digressions-objectives}

Nach Abschluss dieses Lernprogramms sind Sie mit den folgenden Inhalten vertraut:

- Aufbau und Funktionsweise von Abschweifungen
- Auswirkung der Abschweifungseinstellungen auf den Ablauf des Dialogmoduls
- Abschweifungseinstellungen für ein Dialogmodul testen

### Dauer
{: #tutorial-digressions-duration}

Für dieses Lernprogramm benötigen Sie ungefähr 20 Minuten.

### Voraussetzung
{: #tutorial-digressions-prereqs}

Wenn Sie nicht über eine {{site.data.keyword.conversationshort}}-Instanz verfügen, führen Sie den unter **Vorbereitende Schritte** im [Lernprogramm 'Einführung'](/docs/services/assistant?topic=assistant-getting-started#getting-started-prerequisites) beschriebenen Schritt aus, um eine Instanz zu erstellen.

## Schritt 1: Beispieldialogskill 'Digression showcase' importieren
{: #tutorial-digressions-import-json}

Zuerst müssen Sie den Dialogskill *Digression showcase* in Ihre {{site.data.keyword.conversationshort}}-Instanz importieren.

1.  Laden Sie die Datei [digression-showcase.json](https://github.com/watson-developer-cloud/community/raw/master/watson-assistant/digression-showcase.json) herunter.
1.  Klicken Sie in Ihrer {{site.data.keyword.conversationshort}}-Instanz auf das Symbol ![Importieren](images/workspace_import.png).
1.  Klicken Sie auf **Datei auswählen**u und wählen Sie dann die Datei **digression-showcase.json** aus, die Sie zuvor heruntergeladen haben.
1.  Klicken Sie auf **Importieren**, um das Importieren des Dialogskills abzuschließen.

## Schritt 2: Temporäre Abschweifung vom Dialogmodul
{: #tutorial-digressions-temporarily-digress-away}

Abschweifungen ermöglichen dem Benutzer, eine Dialogmodulverzweigung zu verlassen, um vorübergehend das Thema zu wechseln und anschließend zum ursprünglichen Dialogablauf zurückzukehren. Im vorliegenden Schritt beginnen Sie eine Tischreservierung in einem Restaurant und schweifen davon ab, um nach den Öffnungszeiten des Restaurants zu fragen. Nachdem die Öffnungszeiten zur Verfügung gestellt wurden, kehrt der Service zum Dialogablauf der Tischreservierung zurück.

1.  Klicken Sie auf **Dialogmodul**, um von der Seite mit Absichten aus eine Ansicht der Baumstruktur des Dialogmoduls aufzurufen.

1.  Klicken Sie auf das Symbol ![Ausprobieren](images/ask_watson.png), um die Anzeige 'Ausprobieren' zu öffnen.
1.  Geben Sie `Reserviere mir einen Tisch im Restaurant` in das Textfeld ein.

    Der Service antwortet mit der Nachfrage nach dem gewünschten Tag für die Reservierung: `Wann möchten Sie essen gehen?`

1.  Klicken Sie auf das Symbol **Standort** ![Standort](images/location.png) neben der Antwort, um den Knoten in der Baumstruktur des Dialogmoduls hervorzuheben, von dem die Antwort ausgelöst wurde (der Knoten **Restaurantbuchung**.

    ![Zeigt den hervorgehobenen Knoten 'Restaurantbuchung' und den aktiven Dialog im Fenster 'Ausprobieren'.](images/tut-dig-location.png)
1.  Geben Sie `Morgen` ein.

    Der Service antwortet mit der Nachfrage nach der gewünschten Uhrzeit für die Reservierung: `Um welche Uhrzeit möchten Sie essen gehen?`

1.  Sie wissen nicht, wann das Restaurant schließt und fragen daher: `Wann schließen Sie?`

    Der Bot schweift vom Knoten für die Restaurantbuchung ab und wechselt zum Knoten **Öffnungszeiten des Restaurants**. Die Antwort lautet: `Das Restaurant ist von 8:00 bis 22:00 Uhr geöffnet.` Anschließend kehrt der Service zum Knoten für die Restaurantbuchung zurück und fragt erneut nach der Uhrzeit für die Reservierung.

    ![Zeigt die aktive Abschweifung im Fenster 'Ausprobieren'.](images/tut-dig-digression.png)
1.  **Optional**: Um den Dialogablauf abzuschließen, geben Sie `20:00 Uhr` als Reservierungszeit ein und `2` für die Anzahl der Gäste.

Glückwunsch! Sie haben erfolgreich eine Abschweifung vom Dialogablauf durchgeführt und sind anschließend zum ursprünglichen Dialogablauf zurückgekehrt.

## Schritt 3: Slotabschweifungen inaktivieren
{: #tutorial-digressions-disable-slot}

In diesem Schritt bearbeiten Sie die Abschweifungseinstellungen des Knotens für die Restaurantbuchung, um das Abschweifen der Benutzer zu verhindern und erfahren, wie sich die geänderten Einstellungen auf den Dialogablauf auswirken.

1.  Betrachten wir die aktuellen Abschweifungseinstellungen für den Knoten **Restaurantbuchung**. Klicken Sie auf den Knoten, um ihn in der Bearbeitungsansicht zu öffnen.

1.  Klicken Sie auf **Anpassen** und dann auf die Registerkarte **Abschweifungen**.

    ![Zeigt die Abschweifungseinstellungen für den Knoten 'Restaurantbuchung'.](images/tut-dig-resto-settings.png)

1.  Ändern Sie die Einstellung des Umschaltsteuerelements **Abgehende Abschweifungen zulassen** von 'Ein' in '**Aus**' und klicken Sie anschließend auf **Anwenden**.

1.  Klicken Sie auf ![Schließen](images/close.png), um die Bearbeitungsansicht für den Knoten zu schließen.

1.  Klicken Sie im Fenster 'Ausprobieren' auf **Inhalt löschen**, um das Dialogmodul zurückzusetzen.

1.  Geben Sie `Reserviere mir einen Tisch im Restaurant` ein.

    Der Service antwortet mit der Nachfrage nach dem gewünschten Tag für die Reservierung: `Wann möchten Sie essen gehen?`

1.  Geben Sie `Morgen` ein.

    Der Service antwortet mit der Nachfrage nach der gewünschten Uhrzeit für die Reservierung: `Um welche Uhrzeit möchten Sie essen gehen?`

1.  Stellen Sie die folgende Frage: `Um welche Zeit schließen Sie?`

    Der Service erkennt zwar, dass diese Frage die Absicht '#restaurant_opening_hours' auslöst, ignoriert jedoch diese Absicht und zeigt stattdessen die Nachfrage für den Slot '@sys-time' erneut an.

Sie haben erfolgreich verhindert, dass der Benutzer vom Prozess für Restaurantbuchung abschweift.

## Schritt 4: Abschweifung zu einem Knoten ohne Rückführung
{: #tutorial-digressions-digress-without-return}

Sie können einen Dialogmodulknoten so konfigurieren, dass nach einer Abschweifung nicht die Verarbeitung des vorherigen Knotens fortgesetzt wird. Zur Veranschaulichung der Vorgehensweise ändern Sie die Abschweifungseinstellung des Knotens für die Öffnungszeiten eines Restaurants. In Schritt 2 wurde dargestellt, wie nach einer Abschweifung von dem Knoten für Restaurantbuchung zum Knoten für die Öffnungszeiten des Restaurants der Reservierungsprozess fortgesetzt wurde. In der vorliegenden Übung schweifen Sie nach dem Ändern der Einstellung vom Dialogmodul **Stellenangebote** ab, um nach den Öffnungszeiten eines Restaurants zu fragen, ohne danach zum vorherigen Dialogknoten zurückzukehren.

1.  Klicken Sie auf den Knoten **Öffnungszeiten des Restaurants**, um ihn zu öffnen.

1.  Klicken Sie auf **Anpassen** und dann auf die Registerkarte **Abschweifungen**.

1.  Erweitern Sie den Abschnitt **Abschweifungen zu diesem Knoten zulassen** und wählen Sie das Kontrollkästchen **Nach Abschweifung zurückkehren** ab. Klicken Sie auf **Anwenden** und danach auf ![Schließen](images/close.png), um die Bearbeitungsansicht für den Knoten zu schließen.

1.  Klicken Sie im Fenster 'Ausprobieren' auf **Inhalt löschen**, um das Dialogmodul zurückzusetzen.

1.  Um den Dialogmodulknoten **Stellenangebote** aufzurufen, geben Sie `Ich suche einen Job` ein.

    Der Service gibt die folgende Antwort zurück: `Wir suchen ständig nach qualifizierten Mitarbeitern für unser Team. Für welche Art von Job interessieren Sie sich?`

1.  Anstatt diese Frage zu beantworten, stellen Sie dem Bot eine ganz andere Frage. Geben Sie Folgendes ein: `Um welche Uhrzeit öffnen Sie?`

    Der Service schweift vom Knoten für Stellenangebote zum Knoten für Öffnungszeiten des Restaurants ab, um Ihre Frage zu beantworten. Der Service antwortet wie folgt: `Das Restaurant ist von 8:00 bis 22:00 Uhr geöffnet.`

    Im Unterschied zum vorherigen Test wird der Dialog danach nicht mit dem vorherigen Knoten **Stellenangebote** fortgesetzt. Der Service kehrt nicht zu dem vorherigen aktiven Dialogmodulknoten zurück, da der Knoten **Öffnungszeiten des Restaurants** so konfiguriert wurde, dass keine Rückkehr zum vorherigen Knoten erfolgt.

    ![Zeigt einen Dialog ohne Rückkehr zum vorherigen Knoten nach einer Abschweifung.](images/tut-dig-noreturn.png)

Glückwunsch! Sie haben erfolgreich eine Abschweifung vom Dialogablauf ohne Rückkehr zum vorherigen Knoten erstellt.

## Zusammenfassung
{: #tutorial-digressions-summary}

In diesem Lernprogramm haben Sie die Funktionsweise von Abschweifungen kennengelernt und erfahren, wie die Einstellungen einzelner Dialogmodulknoten das Verhalten bei Abschweifungen beeinflussen können.

## Nächste Schritte
{: #tutorial-digressions-next-steps}

Hilfe zum Konfigurieren von Abweichungen für Ihr angepasstes Dialogmodul finden Sie im Abschnitt [Abweichungen](/docs/services/assistant?topic=assistant-dialog-runtime#dialog-runtime-digressions).
