---

copyright:
  years: 2015, 2019
lastupdated: "2019-02-11"

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
{:gif: data-image-type='gif'}

# Benutzereingabe korrigieren
{: #beta-spell-check}

Diese Feature ist nur für Benutzer verfügbar, die am Betaprogramm teilnehmen. Informationen zum Anfordern des Zugriffs enthält der Abschnitt [Am Betaprogramm teilnehmen](/docs/services/assistant?topic=assistant-feedback#feedback-beta).

![Beta](images/beta.png) IBM gibt Services, Features und Sprachunterstützung für Sie zum Bewerten frei, die als Betaversion klassifiziert sind. Diese Services können unter Umständen instabil sein, häufig geändert werden und nach Ankündigung kurzfristig eingestellt werden. Außerdem bieten Beta-Features möglicherweise nicht dieselben Leistungswerte oder dieselbe Kompatibilität wie allgemein verfügbare Features und sind nicht für die Verwendung in einer Produktionsumgebung bestimmt.

Aktivieren Sie die *Rechtschreibprüfung*, um Rechtschreibfehler in den Äußerungen der Benutzer zu korrigieren, die als Benutzereingaben übermittelt werden. Wenn die Rechtschreibprüfung aktiviert ist, werden Rechtschreibfehler automatisch korrigiert. Außerdem werden beim Auswerten der Eingabe die korrigierten Wörter verwendet. Je höher die Genauigkeit der Eingabedaten, umso zuverlässiger kann der Service Entitätserwähnungen und die Absicht des Benutzers erkennen. 

Diese Einstellung kann derzeit nur für Dialogskills in englischer Sprache aktiviert werden.
{: note}

Bei aktivierter Rechtschreibprüfung wird die Benutzereingabe wie folgt korrigiert: 

- Ursprüngliche Eingabe: `letme applt for a memberdhip`
- Korrigierte Eingabe: `let me apply for a membership`

Beim Prüfen der Rechtschreibung eines Wortes verwendet der Service mehr als nur einen einfachen Suchvorgang im Wörterverzeichnis. Vielmehr wird durch eine Kombination aus Verarbeitung natürlicher Sprache und Wahrscheinlichkeitsmodellen ermittelt, ob ein Rechtschreibfehler vorliegt, der korrigiert werden sollte. 

## Rechtschreibprüfung aktivieren
{: #beta-spell-check-enable}

So aktivieren Sie die Rechtschreibprüfung: 

1.  Lokalisieren Sie den gewünschten Skill auf der Seite 'Skills'.
1.  Klicken Sie auf das Symbol ![Liste der Optionen öffnen bzw. schließen](images/kabob-beta.png) und wählen Sie anschließend **Einstellungen** aus.
1.  Aktivieren Sie auf der Seite mit Einstellungen für die *Rechtschreibprüfung* die Einstellung **Automatische Rechtschreibkorrektur**.

### Rechtschreibkorrektur testen
{: #beta-spell-check-test}

1.  Geben Sie auf der Seite 'Ausprobieren' eine Äußerung an, die Rechtschreibfehler enthält. 

    Wenn Wörter in der Eingabe Rechtschreibfehler enthalten, werden sie automatisch korrigiert und ein Symbol ![Automatische Korrektur](images/auto-correct.png) wird angezeigt. Die korrigierte Äußerung wird unterstrichen.
1.  Bewegen Sie den Mauszeiger über die unterstrichene Äußerung, damit die ursprüngliche Schreibung angezeigt wird. 

Wenn der Service nicht alle Rechtschreibfehler korrigiert, überprüfen Sie in den vom Service angewendeten Korrekturregeln, ob das betreffende Wort einer Kategorie angehört, die vom Service absichtlich nicht korrigiert wird.

Um übermäßigen Korrekturaufwand zu vermeiden, verzichtet der Service auf Korrekturen in den folgenden Eingabedatentypen:

- Wörter mit durchgängiger Großschreibung
- Emojis
- Ortsangaben wie Staaten oder Straßenadressen
- Zahlen, Maßeinheiten und Zeitangaben
- Eigennamen wie Vornamen oder Firmennamen
- Text in Anführungszeichen
- Wörter, die Sonderzeichen wie Bindestriche (-), Sterne (*) oder Et-Zeichen (&) enthalten
- Wörter, die diesem Skill *angehören*, d. h. Wörter mit einer impliziten Bedeutung durch ihr Vorkommen in Entitätswerten, in Entitätssynonymen oder in Beispielen für Benutzerabsichten

Wenn das Wort, das nicht korrigiert wurde, keinem dieser Eingabedatentypen angehört, kann es hilfreich sein zu prüfen, ob in der Entität die unscharfe Suche aktiviert ist.

#### In welchem Zusammenhang stehen Rechtschreibkorrektur und unscharfe Suche?
{: #beta-spell-check-vs-fuzzy-matching}

Die unscharfe Suche erleichtert die wörterverzeichnisbasierte Erkennung von Entitätserwähnungen in der Benutzereingabe. Dabei wird ein Wörterverzeichnis durchsucht, um Übereinstimmungen zwischen einem Wort in der Benutzereingabe und einem vorhandenen Entitätswert oder -synonym in den Trainingsdaten zu erkennen. Angenommen, der Benutzer gibt `Bücher` ein und die Trainingsdaten enthalten das Entitätssynonym `Buch`. In diesem Fall erkennt die unscharfe Suche, dass beide Begriffe dasselbe bedeuten.

Wenn Sie sowohl die Rechtschreibprüfung als auch die unscharfe Suche aktivieren, wird die Funktion für unscharfe Suche ausgeführt, bevor die Rechtschreibprüfung gestartet wird. Wird ein Term gefunden, der mit einem Entitätswert oder -synonym aus dem Wörterverzeichnis übereinstimmt, dann wird dieser Term in die Liste der Wörter eingetragen, die dem Skill *angehören* und daher nicht korrigiert werden sollen. Ebenso erkennt die unscharfe Suche in dem vom Benutzer eingegebenen Satz `I want to buy a boook` den Term `boook` als gleichbedeutend mit dem Entitätssynonym `book` und fügt ihn zur Liste der geschützten Wörter hinzu. Dies hat zur Folge, dass der Rechtschreibfehler in dem Wort `boook` vom Service *nicht* korrigiert wird. 

#### Funktionsweise
{: #beta-spell-check-how-it-works}

Normalerweise werden Benutzereingaben im Feld `text` des Objekts `input` der Nachricht unverändert gespeichert. Nur wenn die Benutzereingabe in irgendeiner Weise korrigiert wird, wird im Objekt `input` ein neues Feld mit dem Namen `original_text` erstellt. In diesem Feld wird die ursprüngliche Eingabe des Benutzers (mit allen Rechtschreibfehlern) gespeichert. Der korrigierte Text wird im Feld `input.text` hinzugefügt.
