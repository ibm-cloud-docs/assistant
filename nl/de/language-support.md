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

# Unterstützte Sprachen
{: #language-support}

Der Service '{{site.data.keyword.conversationshort}}' unterstützt die hier aufgeführten Sprachen. Einzelne Features des Service werden bei jeder Sprache in größerem oder geringerem Umfang unterstützt.
{: shortdesc}

In den nachstehenden Tabellen wird die Stufe der Unterstützung für Sprachen und Features durch die folgenden Codes angegeben:

- **GA** - Das Feature ist allgemein verfügbar und wird für diese Sprache unterstützt. Bitte beachten Sie, dass Features nach der allgemeinen Verfügbarkeit weiter aktualisiert werden können.
- **Beta** - Das Feature wird nur als Betaversion unterstützt und wird noch getestet, bevor es in dieser Sprache allgemein verfügbar gemacht wird.
- **Keine Angabe** - Gibt an, dass ein Feature in dieser Sprache nicht verfügbar ist.

In der ersten Tabelle ist die Unterstützungsstufe für alle Features angegeben (ausgenommen die Features für Absichten und Entitäten; sie sind in der zweiten und dritten Tabelle enthalten).

**Tabelle 1. Details zur Feature-Unterstützung**

| Sprache | **[Absichten](/docs/services/assistant?topic=assistant-intents)**, **[Entitäten](/docs/services/assistant?topic=assistant-entities)** und **[Dialogmodule](/docs/services/assistant?topic=assistant-dialog-build)** definieren | **Suche** |
|:---:|:---:|:---:|
| **Englisch (en)**                   | GA | GA |
| **Arabisch (ar)**                    | GA | Keine Angabe |
| **Chinesisch (vereinfacht) (zh-cn)**   | GA | Beta |
| **Chinesisch (traditionell) (zh-tw)**  | Beta | Beta |
| **Tschechisch (cs)**                     | GA | Beta |
| **Niederländisch (nl)**                     | GA | Beta |
| **Französisch (fr)**                    | GA | Beta |
| **Deutsch (de)**                    | GA | Beta |
| **Italienisch (it)**                   | GA | Beta |
| **Japanisch (ja)**                  | GA | Beta |
| **Koreanisch (ko)**                    | GA | Beta |
| **Portugiesisch (Brasilien) (pt-br)** | GA | Beta |
| **Spanisch (es)**                   | GA | Beta |
{: caption="Details zur Feature-Unterstützung" caption-side="top"}

**Tabelle 2. Details zur Feature-Unterstützung für Absichten**

| Sprache | **[Absolute Bewertung und 'Als irrelevant markieren'](/docs/services/assistant?topic=assistant-intents#intents-mark-irrelevant)** | **[Inhaltskatalog](/docs/services/assistant?topic=assistant-catalog)** | **[Empfehlungen für Benutzerbeispiele](/docs/services/assistant?topic=assistant-intent-recommendations#intent-recommendations-get-example-recommendations)** |
|:---:|:---:|:---:|:---:|
| **Englisch (en)**                   | GA | GA | Beta |
| **Arabisch (ar)**                    | Beta | GA | Keine Angabe |
| **Chinesisch (vereinfacht) (zh-cn)**   | GA | Keine Angabe | Keine Angabe |
| **Chinesisch (traditionell) (zh-tw)**  | Beta | Keine Angabe | Keine Angabe |
| **Tschechisch (cs)**                     | GA | Keine Angabe | Keine Angabe |
| **Niederländisch (nl)**                     | GA | Keine Angabe | Keine Angabe |
| **Französisch (fr)**                    | GA | GA | Keine Angabe |
| **Deutsch (de)**                    | GA | GA | Keine Angabe |
| **Italienisch (it)**                   | GA | GA | Keine Angabe |
| **Japanisch (ja)**                  | GA | GA | Keine Angabe |
| **Koreanisch (ko)**                    | GA | Keine Angabe | Keine Angabe |
| **Portugiesisch (Brasilien) (pt-br)** | GA | GA | Keine Angabe |
| **Spanisch (es)**                   | GA | GA | Keine Angabe |
{: caption="Details zur Feature-Unterstützung für Absichten" caption-side="top"}

**Tabelle 3. Details zur Feature-Unterstützung für Entitäten**

| Sprache | **Systementitäten ([Zahl](/docs/services/assistant?topic=assistant-system-entities#system-entities-sys-number), [Währung](/docs/services/assistant?topic=assistant-system-entities#system-entities-sys-currency), [Prozentsatz](/docs/services/assistant?topic=assistant-system-entities#system-entities-sys-percentage), [Datum, Zeit](/docs/services/assistant?topic=assistant-system-entities#system-entities-sys-date-time))** | **[Unscharfe Suche nach Entität](/docs/services/assistant?topic=assistant-entities#entities-fuzzy-matching)** | **[Kontextbasierte Entitäten](/docs/services/assistant?topic=assistant-entities#entities-create-annotation-based)** | **[Synonymempfehlungen](/docs/services/assistant?topic=assistant-entities#entities-synonyms)**
|:---|:---:|:---:|:---:|:---:|
| **Englisch (en)**                   | GA, Beta ([Position](/docs/services/assistant?topic=assistant-system-entities#system-entities-sys-location), [Person](/docs/services/assistant?topic=assistant-system-entities#system-entities-sys-person)) | GA | Beta | GA |
| **Arabisch (ar)**                    | Beta | GA (nur Rechtschreibfehler) | Keine Angabe | Keine Angabe |
| **Chinesisch (vereinfacht) (zh-cn)**   | GA | Keine Angabe | Keine Angabe | Keine Angabe |
| **Chinesisch (traditionell) (zh-tw)**  | Beta | Keine Angabe | Keine Angabe | Keine Angabe |
| **Tschechisch (cs)**                     | GA | GA (nur Rechtschreibfehler) | Keine Angabe | Keine Angabe |
| **Niederländisch (nl)**                     | GA | GA (nur Rechtschreibfehler) | Keine Angabe | Keine Angabe |
| **Französisch (fr)**                    | GA | GA (nur Rechtschreibfehler) | Keine Angabe | GA |
| **Deutsch (de)**                    | GA | GA (nur Rechtschreibfehler) | Keine Angabe | Keine Angabe |
| **Italienisch (it)**                   | GA | GA (nur Rechtschreibfehler) | Keine Angabe | Keine Angabe |
| **Japanisch (ja)**                  | GA | GA (nur Rechtschreibfehler) | Keine Angabe | GA |
| **Koreanisch (ko)**                    | GA | GA (nur Rechtschreibfehler) | Keine Angabe | Keine Angabe |
| **Portugiesisch (Brasilien) (pt-br)** | GA | GA (nur Rechtschreibfehler) | Keine Angabe | Keine Angabe |
| **Spanisch (es)**                   | GA | GA (nur Rechtschreibfehler) | Keine Angabe | GA |
{: caption="Details zur Feature-Unterstützung für Entitäten" caption-side="top"}

**Hinweis:** Der {{site.data.keyword.conversationshort}}-Service unterstützt mehrere Sprachen wie hier angegeben. Die Toolschnittstelle (Beschreibungen, Beschriftungen usw.) verwendet die englische Sprache. Alle unterstützten Sprachen können über die in Englisch angezeigte Schnittstelle eingegeben und trainiert werden.

**Konformität mit GB18030**: GB18030 ist eine chinesische Norm, die eine erweiterte Codepage für die Verwendung im chinesischen Markt spezifiziert. Diese normierte Codepage ist für die Softwarebranche von großer Bedeutung, da vom China National Information Technology Standardization Technical Committee verfügt wurde, dass jede Softwareanwendung, die nach dem 1. September 2001 im chinesischen Markt freigegeben wird, für GB18030 aktiviert sein muss. Der Service '{{site.data.keyword.conversationshort}}' unterstützt diese Codierung und ist für die Einhaltung von GB18030 zertifiziert.

## Sprache für einen Skill ändern
{: #language-support-change-language}

Nachdem ein Skill erstellt wurde, kann die zugehörige Sprache nicht mehr geändert werden. Falls die unterstützte Sprache für einen Skill geändert werden muss, sollte der Benutzer den Skill herunterladen. Anschließend kann die resultierende JSON-Datei in einem Texteditor bearbeitet werden. Suchen Sie nach einer JSON-Eigenschaft namens `language`.

Die Eigenschaft `language` sollte auf die ursprüngliche Sprache des Skills gesetzt sein (die Einstellung für Englisch wäre beispielsweise `en`). Ändern Sie den Wert dieser Eigenschaft in die gewünschte Sprache (`fr` für Französisch, `de` für Deutsch usw.). Speichern Sie die Änderungen an der JSON-Datei und importieren Sie die geänderte Datei in Ihre {{site.data.keyword.conversationshort}}-Serviceinstanz.

## Bidirektionale Sprachen konfigurieren
{: #language-support-configure-bi-directional}

Für bidirektionale Sprachen (z. B. Arabisch) können Sie Ihre Skillvorgabe entsprechend ändern. Wählen Sie auf der Kachel für Ihren Skill das Dropdown-Menü *Aktionen* aus und wählen Sie dann die Option **Vorgaben für bidirektionale Sprache** aus (diese Option ist nur für Skills verfügbar, für die eine bidirektionale Sprache festgelegt wurde):

![Vorgaben für bidirektionale Sprache](images/bidi_prefs.png)

Wählen Sie eine der folgenden Optionen für Ihren Skill aus:

- **GUI-Richtung**: Gibt die Layoutrichtung von Elementen wie Schaltflächen oder Menüs in der grafischen Benutzerschnittstelle an. Wählen Sie `LTR` (von links nach rechts) oder `RTL` (von rechts nach links) aus. Wenn Sie für diese Option nichts angeben, verwendet das Tool die Einstellung des Web-Browsers für die GUI-Richtung.
- **Textrichtung**: Gibt die Richtung von eingegebenem Text an. Wählen Sie `LTR` (von links nach rechts) bzw. `RTL` (von rechts nach links) oder `Auto` aus. Bei der letzten Option wird die Textrichtung automatisch auf Grundlage der Systemeinstellungen ausgewählt. Die Option `Keine` zeigt Text von links nach rechts an.
- **Numerische Umsetzung**: Gibt an, welches Ziffernformat zur Darstellung von normalen Ziffern verwendet wird. Wählen Sie `Nominal`, `Arabisch-Indisch` oder `Arabisch-Europäisch` aus. Die Option `Keine` zeigt Numerale in westlicher Schreibweise an.
- **Kalendertyp**: Gibt an, wie Sie die Filterung von Datumsangaben in der Benutzerschnittstelle für den Skill auswählen. Wählen Sie `Islamisch (zivil)`, `Islamisch (tabellarisch)`, `Islamisch (Umm al-Qura)` oder `Gregorianisch` aus.

  Diese Einstellung wird nicht auf die Anzeige 'Ausprobieren' angewendet.
  {: note}

![Optionen für bidirektionale Sprachen](images/bidi_opts.png)

Nachdem Sie alle gewünschten Einstellungen ausgewählt haben, klicken Sie auf **Aktualisieren**, um sie zu speichern und zur Kachel für den Skill zurückzukehren.

## Mit Akzentzeichen arbeiten
{: #language-support-accents}

In einer dialogorientierten Einstellung können Benutzer bei der Interaktion mit dem Service '{{site.data.keyword.conversationshort}}' Akzente verwenden oder nicht. Insofern können Fassungen von Wörtern mit Akzent und ohne Akzent bei der Absichts- und Entitätserkennung identisch behandelt werden.

Bei einigen Sprachen (z. B. Spanisch) können manche Akzente jedoch die Bedeutung der Entität ändern. Obwohl die Originalentität implizit einen Akzent besitzt, kann der Service daher bei der Entitätserkennung auch eine Übereinstimmung mit der Version derselben Entität ohne Akzent erzielen, allerdings mit einer etwas geringeren Konfidenzbewertung.

Beispiel: Für das Wort 'barrió' mit Akzent, das der Vergangenheitsform des Verbs 'barrer' (fegen) entspricht, kann der Service auch eine Übereinstimmung mit dem Wort 'barrio' (Nachbarschaft) ermitteln, jedoch mit einer etwas geringeren Konfidenzbewertung.

Das System stellt die höchsten Konfidenzbewertungen in Entitäten mit exakten Übereinstimmungen bereit. Beispielsweise wird `barrio` nicht erkannt, wenn `barrió` im Trainingsset angegeben ist, und `barrió` wird nicht erkannt, wenn `barrio` im Trainingsset enthalten ist.

Sie müssen dafür sorgen, dass das System mit den richtigen Zeichen und Akzenten trainiert wird. Falls Sie beispielsweise `barrió` als Antwort erwarten, müssen Sie `barrió` in das Trainingsset aufnehmen.

Obwohl es sich streng genommen bei der Tilde nicht um ein Akzentzeichen handelt, gilt dasselbe für Wörter, die z. B. den spanischen Buchstaben `ñ` im Gegensatz zum Buchstaben `n` verwenden (z. B. 'uña' und 'una'). In diesem Fall ist der Buchstabe `ñ` nicht einfach ein `n` mit einem Akzent, sondern ein eindeutiger besonderer Buchstabe des Spanischen.

Sie können die unscharfe Suche aktivieren, wenn Sie vermuten, dass Ihre Kunden möglicherweise nicht die richtigen Akzente verwenden oder Wörter falsch schreiben (beispielsweise `n` statt `ñ` verwenden). Sie können die Varianten aber auch explizit in die Trainingsbeispiele aufnehmen.
