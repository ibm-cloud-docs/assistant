---

copyright:
  years: 2015, 2018
lastupdated: "2018-02-07"

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

# Unterstützte Sprachen
Der Service '{{site.data.keyword.conversationshort}}' unterstützt die hier aufgeführten Sprachen. Einzelne Features des Service werden bei jeder Sprache in größerem oder geringerem Umfang unterstützt.
{: shortdesc}

Die Stufe der Unterstützung für Sprachen und Features ist in der nachstehenden Tabelle durch die folgenden Codes wiedergegeben:

- **GA** - Das Feature ist allgemein verfügbar und wird für diese Sprache unterstützt. Bitte beachten Sie, dass Features nach der allgemeinen Verfügbarkeit weiter aktualisiert werden können.
- **Beta** - Das Feature wird nur als Betaversion unterstützt und wird noch getestet, bevor es in dieser Sprache allgemein verfügbar gemacht wird.
- **Keine Angabe** - Falls kein Code angegeben ist, bedeutet dies, dass ein Feature in dieser Sprache nicht verfügbar ist.

|                  | **[Absichten](intents.html)**, **[Entitäten](entities.html)** und **[Dialogmodule definieren](dialog-build.html)** | **[Absolute Bewertung und Option 'Als irrelevant markieren'](intents.html#mark-irrelevant)** | **Systementitäten (für [Zahlen](system-entities.html#sys-number), [Währung](system-entities.html#sys-currency), [Prozentsatz](system-entities.html#sys-percentage), [Datum/Uhrzeit](system-entities.html#sys-datetime))** | **[Unscharfe Suche für Entitäten](entities.html#fuzzy-matching)** |
|:---|:---:|:---:|:---:|:---:|
| **Englisch (en)**                   | GA | GA | GA </br> Beta ([location](system-entities.html#sys-location), [person](system-entities.html#sys-person)) | Beta (Normalformenreduktion, Rechtschreibfehler und Suche mit teilweiser Übereinstimmung) |
| **Arabisch (ar)**                    | GA | Beta | Beta | Beta (nur Rechtschreibfehler) |
| **Chinesisch (vereinfacht) (zh-cn)**   | Beta | Beta | Beta |  |
| **Chinesisch (traditionell) (zh-tw)**  | Beta | Beta |  |  |
| **Tschechisch (cs)**                     | Beta | Beta | Beta | Beta (nur Rechtschreibfehler) |
| **Niederländisch (nl)**                     | Beta | Beta | Beta |  |
| **Französisch (fr)**                    | GA | GA | GA | Beta (nur Rechtschreibfehler) |
| **Deutsch (de)**                    | GA | GA | GA | Beta (nur Rechtschreibfehler) |
| **Italienisch (it)**                   | GA | GA | GA | Beta (nur Rechtschreibfehler) |
| **Japanisch (ja)**                  | GA | GA | GA | Beta (nur Rechtschreibfehler) |
| **Koreanisch (ko)**                    | GA | GA | GA | Beta (nur Rechtschreibfehler) |
| **Portugiesisch (Brasilien) (pt-br)** | GA | GA | GA | Beta (nur Rechtschreibfehler) |
| **Spanisch (es)**                   | GA | GA | GA | Beta (nur Rechtschreibfehler) ||

**Hinweis:** Der Service '{{site.data.keyword.conversationshort}}' unterstützt mehrere Sprachen wie hier angegeben. Die Toolschnittstelle (Beschreibungen, Bezeichnungen usw.) verwendet Englisch. Alle unterstützten Sprachen können über die in Englisch angezeigte Schnittstelle eingegeben und trainiert werden.

**Konformität mit GB18030**: GB18030 ist eine chinesische Norm, die eine erweiterte Codepage für die Verwendung im chinesischen Markt spezifiziert. Diese normierte Codepage ist für die Softwarebranche von großer Bedeutung, da vom China National Information Technology Standardization Technical Committee verfügt wurde, dass jede Softwareanwendung, die nach dem 1. September 2001 im chinesischen Markt freigegeben wird, für GB18030 aktiviert sein muss. Der Service '{{site.data.keyword.conversationshort}}' unterstützt diese Codierung und ist für die Einhaltung von GB18030 zertifiziert.

## Sprache für einen Arbeitsbereich ändern

Nachdem ein Arbeitsbereich erstellt wurde, kann seine Sprache nicht geändert werden. Falls die unterstützte Sprache eines Arbeitsbereichs geändert werden muss, sollte der Benutzer den Arbeitsbereich herunterladen. Anschließend kann die resultierende JSON-Datei in einem Texteditor bearbeitet werden. Suchen Sie nach einer JSON-Eigenschaft namens `language`.

Die Eigenschaft `language` sollte auf die ursprüngliche Sprache des Arbeitsbereichs gesetzt sein; Englisch wäre beispielsweise `en`. Ändern Sie den Wert dieser Eigenschaft in die gewünschte Sprache (`fr` für Französisch, `de` für Deutsch usw.). Speichern Sie die Änderungen an der JSON-Datei und importieren Sie die geänderte Datei in Ihre {{site.data.keyword.conversationshort}}-Serviceinstanz.

## Bidirektionale Sprachen konfigurieren
{: #configuring-bi-directional}

Für bidirektionale Sprachen (z. B. Arabisch) können Sie Ihre Arbeitsbereichsvorgabe entsprechend ändern. Wählen Sie auf der Registerkarte für Ihren Arbeitsbereich das Dropdown-Menü *Aktionen* aus und wählen Sie dann die Option **Vorgaben für bidirektionale Sprache** aus (diese Option ist nur für Arbeitsbereiche verfügbar, für die eine bidirektionale Sprache festgelegt wurde):

![Vorgaben für bidirektionale Sprache](images/bidi_prefs.png)

Wählen Sie eine der folgenden Optionen für Ihren Arbeitsbereich aus:

- **GUI-Richtung**: Gibt die Layoutrichtung von Elementen wie Schaltflächen oder Menüs in der grafischen Benutzerschnittstelle an. Wählen Sie `LTR` (von links nach rechts) oder `RTL` (von rechts nach links) aus. Wenn Sie für diese Option nichts angeben, verwendet das Tool die Einstellung des Web-Browsers für die GUI-Richtung.
- **Textrichtung**: Gibt die Richtung von eingegebenem Text an. Wählen Sie `LTR` (von links nach rechts) bzw. `RTL` (von rechts nach links) oder `Auto` aus. Bei der letzten Option wird die Textrichtung automatisch auf Grundlage der Systemeinstellungen ausgewählt. Die Option `Keine` zeigt Text von links nach rechts an.
- **Numerische Umsetzung**: Gibt an, welches Ziffernformat zur Darstellung von normalen Ziffern verwendet wird. Wählen Sie `Nominal`, `Arabisch-Indisch` oder `Arabisch-Europäisch` aus. Die Option `Keine` zeigt Numerale in westlicher Schreibweise an.
- **Kalendertyp**: Gibt an, wie Sie die Filterung von Datumsangaben in der Benutzerschnittstelle für den Arbeitsbereich auswählen. Wählen Sie `Islamisch (zivil)`, `Islamisch (tabellarisch)`, `Islamisch (Umm al-Qura)` oder `Gregorianisch` aus. **Hinweis**: Diese Einstellung wird nicht auf die Anzeige 'Ausprobieren' angewendet.

![Optionen für bidirektionale Sprachen](images/bidi_opts.png)

Nachdem Sie alle gewünschten Einstellungen ausgewählt haben, klicken Sie auf **Aktualisieren**, um sie zu speichern und zur Registerkarte für den Arbeitsbereich zurückzukehren.

## Mit Akzentzeichen arbeiten
{: #working-with-accents}

In einer dialogorientierten Einstellung können Benutzer bei der Interaktion mit dem Service '{{site.data.keyword.conversationshort}}' Akzente verwenden oder nicht. Insofern können Fassungen von Wörtern mit Akzent und ohne Akzent bei der Absichts- und Entitätserkennung identisch behandelt werden.

Bei einigen Sprachen (z. B. Spanisch) können manche Akzente jedoch die Bedeutung der Entität ändern. Obwohl die Originalentität implizit einen Akzent besitzt, kann der Service daher bei der Entitätserkennung auch eine Übereinstimmung mit der Version derselben Entität ohne Akzent erzielen, allerdings mit einer etwas geringeren Konfidenzbewertung.

Beispiel: Für das Wort 'barrió' mit Akzent, das der Vergangenheitsform des Verbs 'barrer' (fegen) entspricht, kann der Service auch eine Übereinstimmung mit dem Wort 'barrio' (Nachbarschaft) ermitteln, jedoch mit einer etwas geringeren Konfidenzbewertung.

Das System stellt die höchsten Konfidenzbewertungen in Entitäten mit exakten Übereinstimmungen bereit. Beispielsweise wird `barrio` nicht erkannt, wenn `barrió` im Trainingsset angegeben ist, und `barrió` wird nicht erkannt, wenn `barrio` im Trainingsset enthalten ist.

Sie müssen dafür sorgen, dass das System mit den richtigen Zeichen und Akzenten trainiert wird. Falls Sie beispielsweise `barrió` als Antwort erwarten, müssen Sie `barrió` in das Trainingsset aufnehmen.

Obwohl es sich streng genommen bei der Tilde nicht um ein Akzentzeichen handelt, gilt dasselbe für Wörter, die z. B. den spanischen Buchstaben `ñ` im Gegensatz zum Buchstaben `n` verwenden (z. B. 'uña' und 'una'). In diesem Fall ist der Buchstabe `ñ` nicht einfach ein `n` mit einem Akzent, sondern ein eindeutiger besonderer Buchstabe des Spanischen.

Sie können die unscharfe Suche aktivieren, wenn Sie vermuten, dass Ihre Kunden möglicherweise nicht die richtigen Akzente verwenden oder Wörter falsch schreiben (beispielsweise `n` statt `ñ` verwenden). Sie können die Varianten aber auch explizit in die Trainingsbeispiele aufnehmen.
