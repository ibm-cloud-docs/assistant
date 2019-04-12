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

# Programmgesteuerte Aufrufe über einen Dialogmodulknoten absetzen ![Betaversion](images/beta.png)
{: #dialog-actions}

Definieren Sie Aktionen, die programmgesteuerte Aufrufe an externe Anwendungen oder Services absetzen und Ergebnisse für die Verarbeitungsschritte in einer Dialogmodulrunde empfangen.
{: shortdesc}

Mit einem externen Service können Sie die folgenden Aktionen ausführen: 
- Informationen auswerten, die bei Benutzern erhoben wurden.
- Berechnungen oder Zeichenfolgebearbeitungen für Benutzereingaben durchführen, die für unterstützte SpEL-Ausdruchsmethoden zu komplex sind.
- Mit einem externen Web-Service interagieren, um Informationen abzurufen. Beispiele: Sie können die erwartete Ankunftszeit für einen bestimmten Flug einer Fluggesellschaft überprüfen oder eine Vorhersage bei einem Wetterdienst abrufen.
- Anfragen an eine externe Anwendung (z. B. eine Reservierungsplattform für Restaurants) senden, um eine einfache Transaktion für den Benutzer abzuwickeln. 

<iframe class="embed-responsive-item" id="youtubeplayer" title="IBM Cloud Functions aufrufen" type="text/html" width="640" height="390" src="https://www.youtube.com/embed/y0A6X-KNoB8?rel=0" frameborder="0" webkitallowfullscreen mozallowfullscreen allowfullscreen> </iframe>

Beim Definieren des programmgesteuerten Aufrufs wählen Sie einen der folgenden Typen aus:

- **client**: Definiert einen programmgesteuerten Aufruf in einem Standardformat, das von Ihrer externen Clientanwendung verstanden werden kann. Die bereitgestellten Informationen müssen von Ihrer Clientanwendung verwendet werden, um den programmgesteuerten Aufruf oder die programmgesteuerte Funktion auszuführen und das Ergebnis an das Dialogmodul zurückzugeben. Durch diesen Aufruftyp wird das Dialogmodul im Grunde angewiesen, die Verarbeitung an diesem Punkt zu unterbrechen und auf eine Aktion der Clientanwendung zu warten. Die Aktion der Clientanwendung kann das Ausführen eines beliebigen, von Ihnen ausgewählten Programms sein. Stellen Sie sicher, dass Sie den Namen des Aufrufs und die Parameterdetails angeben sowie den Variablennamen für Fehlernachrichten und beachten Sie dabei die JSON-Formatierungsregeln (diese Regeln werden später erläutert).

- **cloud_function**: Ruft eine {{site.data.keyword.openwhisk_short}}-Aktion direkt auf und gibt das Ergebnis an das Dialogmodul zurück. Mit dem Aufruf muss ein {{site.data.keyword.openwhisk_short}}-Authentifizierungsschlüssel angegeben werden. (Dieser Typ wurde bisher als **server** bezeichnet. Der Typ **server** wird weiter unterstützt.)

- **web_action**: Ruft eine {{site.data.keyword.openwhisk_short}}-Webaktion auf. Webaktionen sind annotierte {{site.data.keyword.openwhisk_short}}-Aktionen, die von Entwicklern zum Programmieren von  Back-End-Logik verwendet werden können, auf die eine Webanwendung anonym zugreifen kann (d. h. ohne einen {{site.data.keyword.openwhisk_short}}-Authentifizierungsschlüssel anzufordern). Obwohl die Authentifizierung nicht erforderlich ist, können Webaktionen wie folgt geschützt werden: 

  - Durch ein spezielles Authentifizierungstoken für die Webaktion, das vom Eigner der Aktion jederzeit widerrufen oder geändert werden kann
  - Durch Übergeben Ihrer {{site.data.keyword.openwhisk_short}}-Berechtigungsnachweise

Für alle {{site.data.keyword.openwhisk_short}}-Aktionstypen ('web_action', 'cloud_function' oder 'server') fallen Kosten an. Die Kosten für das Aktivieren der Aktion werden dem Eigner der im Aktionsaufruf angegebenen Berechtigungsnachweise in Rechnung gestellt. Weitere Details enthält der Abschnitt [Preisstruktur ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://cloud.ibm.com/openwhisk/learn/pricing){: new_window}. Der {{site.data.keyword.openwhisk_short}}-Service unterscheidet nicht zwischen Aufrufen über die Anzeige 'Ausprobieren' beim Testen und Aufrufen über eine Anwendung im Produktionsbetrieb. Daher können auch für Aufrufe während des Testbetriebs Kosten anfallen.
{: note}

## Vorgehensweise
{: #dialog-actions-call}

So setzen Sie einen programmgesteuerten Aufruf über einen Dialogmodulknoten ab:

1.  Öffnen Sie den JSON-Editor in dem Dialogmodulknoten, über den Sie den programmgesteuerten Aufruf absetzen möchten.

    - Um einen programmgesteuerten Aufruf abzusetzen, der nach dem Auswerten der Antwort für einen Knoten ausgeführt wird, öffnen Sie den JSON-Editor für die Antwort des Knotens. 

      ![Zeigt die Vorgehensweise zum Aufrufen des JSON-Editors für eine Standardknotenantwort.](images/contextvar-json-response.png)

      Wenn die Einstellung **Mehrere Antworten** für den Knoten auf **Ein** gesetzt ist, müssen Sie auf das Symbol **Antwort bearbeiten** klicken, damit das Symbol ![Antwort bearbeiten](images/edit-slot.png) für das Menü **Optionen** ![Erweiterte Antwort](images/kabob.png) angezeigt wird.

      ![Zeigt die Vorgehensweise zum Aufrufen des zugeordneten JSON-Editors für einen Standardknoten, für den mehrere bedingte Antworten aktiviert sind.](images/contextvar-json-multi-response.png)

    Wenn Sie die Antwort vom externen Service in derselben Dialogmodulrunde anzeigen oder weiter verarbeiten möchten, müssen Sie zu diesem Zweck einen zweiten Knoten hinzufügen und vom ersten Knoten zu diesem springen.
    {: tip}

    - Um einen Aufruf abzusetzen, der von einem einzelnen Slot verwendet werden kann, klicken Sie auf das Symbol **Slot bearbeiten** ![Symbol 'Slot bearbeiten'](images/edit-slot.png) für den Slot und führen Sie anschließend eine der folgenden Aktionen aus:

      - Um einen programmgesteuerten Aufruf abzusetzen, der ausgeführt wird, wenn die Slotbedingung mit 'true' ausgewertet wird, öffnen Sie den zugeordneten JSON-Editor für die Slotbedingung.

        ![Zeigt die Vorgehensweise zum Aufrufen des zugeordneten JSON-Editors für eine Slotbedingung.](images/contextvar-json-slot-condition.png)

      - Um einen programmgesteuerten Aufruf abzusetzen, der ausgeführt wird, nachdem der Slot erfolgreich gefüllt wurde, öffnen Sie den zugeordneten JSON-Editor für die Antwort der Bedingung für 'Gefunden'. Klicken Sie dazu im Menü **Optionen** ![Symbol 'Optionen'](images/kabob.png) für den Slot auf **Bedingte Antworten aktivieren**. Klicken Sie für die Antwort auf die Bedingung für 'Gefunden' auf das Symbol **Antwort bearbeiten** ![Antwort bearbeiten](images/edit-slot.png). Klicken Sie im Menü **Optionen** ![Symbol 'Optionen'](images/kabob.png) für die Antwort auf die Bedingung für 'Gefunden' auf **JSON-Editor öffnen**.

        ![Zeigt die Vorgehensweise zum Aufrufen des zugeordneten JSON-Editors für die bedingte Antwort für einen Slot.](images/contextvar-json-slot-multi-response.png)

1.  Verwenden Sie die folgende Syntax, um den programmgesteuerten Aufruf zu definieren.

    ```json
    {
      "context": {
        "variable_name" : "variable_value"
      },
      "actions": [
        {
          "name":"<actionName>",
          "type":"client | cloud_function | server | web_action",
          "parameters": {
            "<parameter_name>":"<parameter_value>",
            "<parameter_name>":"<parameter_value>"
          },
          "result_variable": "<result_variable_name>",
          "credentials": "<reference_to_credentials>"
        }
      ],
      "output": {
        "text": "response text"
      }
    }
    ```
    {: codeblock}

    Das Array `actions` gibt die programmgesteuerten Aufrufe an, die von dem Dialogmodul aus abgesetzt werden sollen. Das Array kann bis zu 5 separate programmgesteuerte Aufrufe definieren. Geben Sie in dem JSON-Array die folgenden Name/Wert-Paare an:

    - `<actionName>`: Erforderlich. Der Name der Aktion oder des Service, der aufgerufen werden soll. Der Name darf nicht länger als 256 Zeichen sein. 

       - Geben Sie für Clientaktionstypen einen Namen in der gewünschten Syntax an. Sie sollten einen Namen angeben, den Ihre Clientanwendung erkennen und verarbeiten kann.

          Beispiel: `calculateRate`.

       - Verwenden Sie für die Aktionstypen 'cloud_function' (oder 'server') und für 'web_action' die folgende Syntax, um den vollständig qualifizierten Namen der Aktion anzugeben: `/<namespace>/[<package-name>]/<action name>`

         - Wenn die Standardaktion Teil eines Pakets ist, ist die Angabe `<package-name>` erforderlich. Andernfalls ist kein Paketname erforderlich. 
         - Wenn die Webaktion Teil eines Pakets ist, ist die Angabe `<package-name>` erforderlich. Andernfalls ist der Paketname `default` erforderlich, der auf Webaktionen angewendet wird, die nicht über einen Paketnamen verfügen.
         - Wenn Sie eine Folge von Aktionen aufrufen, geben Sie `<sequence name>` anstelle von `<action name>`.
         - Der Namensbereich einer benutzerdefinierten Aktion weist normalerweise die folgende Syntax auf: `<myIBMCloudOrganizationID>_<myIBMCloudSpace>`. Beispiel: `/jdoeorg_prod10/search flights`.
         - Die mit {{site.data.keyword.openwhisk_short}} bereitgestellten Aktionen verwenden häufig den Namensbereich `whisk.system`. Der Namensbereich sollte jedoch zur Sicherheit immer überprüft werden. Beispiel: `/whisk.system/weather/forecast`.

           Weitere Details finden Sie unter [IBM Cloud Functions - Benennungsrichtlinien ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://cloud.ibm.com/docs/openwhisk/openwhisk_reference#openwhisk_entities).

    - `<type>`: Gibt den Typ des Aufrufs an, der ausgeführt werden soll. Wählen Sie einen der folgenden Typen aus:

      - **client**: Sendet eine Antwortnachricht mit programmgesteuerten Aufrufinformationen in einem standardisierten Format, das von Ihrer externen Clientanwendung verstanden werden kann. Die bereitgestellten Informationen müssen von Ihrer Clientanwendung verwendet werden, um den programmgesteuerten Aufruf oder die programmgesteuerte Funktion auszuführen und das Ergebnis an das Dialogmodul zurückzugeben. Das JSON-Objekt im Antworthauptteil gibt an, welcher Service bzw. welche Funktion aufgerufen werden soll, welche zugehörigen Parameter mit dem Aufruf übergeben werden sollen und in welchem Format das Ergebnis zurückgegeben werden soll. 

      - **cloud_function**: Ruft mindestens eine {{site.data.keyword.openwhisk_short}}-Aktion direkt auf. Sie müssen die Aktion mit {{site.data.keyword.openwhisk}} separat definieren. Weitere Informationen enthält der Abschnitt [Aktion erstellen](#dialog-actions-create). (Dieser Typ wurde bisher als **server** bezeichnet. Der Typ **server** wird weiter unterstützt.)

      - **web_action**: Ruft mindestens eine {{site.data.keyword.openwhisk_short}}-Webaktion direkt auf. Sie müssen die Webaktion mit {{site.data.keyword.openwhisk}} separat definieren. Weitere Informationen enthält der Abschnitt [Aktion erstellen](#dialog-actions-create). 

      Der Typ kann optional angegeben werden. Der Standardwert ist `client`.

    - `<action_parameters>`: Alle Parameter, die das externe Programm erwartet, angegeben als JSON-Objekt. Parameter sind nur erforderlich, wenn sie für das externe Programm benötigt werden.

    - `<result_variable_name>`: Dieser Name soll auf das JSON-Objekt verweisen, das von dem externen Service oder Programm zurückgegeben wird. Das Ergebnis wird zum Kontextabschnitt von '/message response' hinzugefügt. Anders ausgedrückt: Das Ergebnis wird als Kontextvariable gespeichert, damit es in der Antwort des Knotens angezeigt oder von später ausgelösten Dialogmodulknoten aufgerufen werden kann. Ein möglicherweise vorhandener Wert für die Kontextvariable wird durch den von der Aktion zurückgegebenen Wert überschrieben. Sie können `result_variable_name` mit der folgenden Syntax angeben:

      - `my_result`
      - `$my_result`

      Der Name darf nicht länger als 64 Zeichen sein. Der Variablenname darf die folgenden Zeichen nicht enthalten: runde Klammern `()`, eckige Klammern (`[]`), einfaches Anführungszeichen (`'`), Anführungszeichen (`"`) und umgekehrter Schrägstrich (`\`).

      Wenn das Ergebnis im Ein- oder Ausgabeabschnitt von '/message response' gespeichert werden soll, können Sie eines der folgenden Positionsschlüsselwörter als Präfix für `result_variable_name` hinzufügen:

       - `output.`: Fügt das Ergebnis zum Ausgabeabschnitt von '/message response' hinzu. Beispiel: `output.my_result`.
       - `input.`: Fügt das Ergebnis zum Eingabeabschnitt von '/message response' hinzu. Beispiel: `input.my_result`.

      Sie können auch einen Kontext (`context.`) als Positionsschlüsselwort voranstellen. Beispiel: `context.my_result`. Dies ist jedoch nicht erforderlich, da das Ergebnis standardmäßig zum Kontext hinzugefügt wird.

      Im Variablennamen können Sie Punkte einfügen, um ein verschachteltes JSON-Objekt zu erstellen. Sie können diese Variablen beispielsweise definieren, um Ergebnisse von zwei separaten Anfragen nach Vorhersagen für heute und morgen bei einem Wetterdienst zu erfassen:

      - `context.weather.today`
      - `context.weather.tomorrow`

      Die Ergebnisse (Parameterwerte `temp` und `rain`) werden mit der folgenden Struktur im Kontext gespeichert:

      ```json
      {
        "weather": {
          "today": {
            "temp": "20",
            "rain": "30"
          },
          "tomorrow": {
            "temp": "23",
            "rain": "80"
          }
        }
      }
      ```
      {: codeblock}

      Wenn die Ergebnisse der programmgesteuerten Aufrufe von mehreren Aktionen in einem einziges JSON-Aktionsarray zu derselben Kontextvariablen hinzugefügt werden, ist die Reihenfolge der Kontextaktualisierung von Bedeutung:

      1.  Wenn das Array eine Kombination aus Server- und Clientaktionen enthält, verarbeitet der Service die Aktionen des Typs 'server' (cloud_function oder web_action) zuerst. Dies führt dazu, dass der von der letzten Arrayaktion des Typs 'client' für die Kontextvariable errechnete Wert den Wert überschreibt, der von den Aktionen des Typs 'server' berechnet wurde.

      1.  Je nach Aktionstyp bestimmt die Reihenfolge, in der die Aktionen im Array definiert sind, in welcher Reihenfolge der Wert der Kontextvariablen festgelegt wird. Der von der letzten Aktion im Array zurückgegebene Kontextvariablenwert überschreibt die von allen anderen Aktionen berechneten Werte.

    - `<reference_to_credentials>`: Der Name des Objekts, in dem {{site.data.keyword.openwhisk_short}}-Berechtigungsnachweise gespeichert werden. Diese Angabe ist nur für Aktionen des Typs 'server' oder 'cloud_function' erforderlich. 

      Die angegebenen Berechtigungen werden für den Zugang zu der {{site.data.keyword.openwhisk_short}}-Instanz verwendet, auf der die Aktion ausgeführt wird. Dies sind nicht Ihre {{site.data.keyword.Bluemix_notm}}-Berechtigungsnachweise.

      So ermitteln Sie die Berechtigungsnachweise:
      1.  Rufen Sie die Seite für [{{site.data.keyword.openwhisk_short}} API-Schlüssel ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://cloud.ibm.com/openwhisk/learn/api-key){: new_window} auf.

          - Wenn Sie noch nicht über ein Konto verfügen, erstellen Sie ein Konto.
          - Wenn Sie noch nicht angemeldet sind, melden Sie sich jetzt an.

      1.  Klicken Sie auf das Symbol **Autorisierungsschlüssel anzeigen** ![Autorisierungsschlüssel anzeigen](images/show-auth-icon.png), um die Berechtigungsnachweise anzuzeigen. Das Segment vor dem Doppelpunkt (:) enthält Ihre Benutzer-ID. Das Segment nach dem Doppelpunkt enthält Ihr Kennwort.

      Alle beim Ausführen der Aktion anfallenden Kosten werden dem Eigentümer dieser Berechtigungsnachweise in Rechnung gestellt.
      {: note}

      Wenn Sie Standardintegrationen zum Bereitstellen des Assistenten verwenden, gibt es keine Möglichkeit, die Berechtigungsnachweise an das Dialogmodul zu übergeben. Speichern Sie sie als Kontextvariablenwerte in einem Dialogmdoulknoten, der ausgelöst wird, bevor der programmgesteuerte Aufruf abgesetzt wird. Dies führt dazu, dass die Berechtigungsnachweise in der JSON-Datei sichtbar sind, die den Skill darstellt. Diese Datei kann von jedem Benutzer heruntergeladen werden, der Zugriff auf Ihre Skills hat.
      {: important}

      Sie können verhindern, dass diese Informationen in Watson-Protokollen gespeichert werden, indem Sie Ihre Kontextvariable in den Abschnitt '$private' des Nachrichtenkontexts einbetten. Beispiel: `$private.my_credentials`. Durch das Speichern in dem privaten Objekt werden die Berechtigungsnachweise jedoch nur vor den Protokollen verborgen. Die Informationen werden trotzdem im zugrunde liegenden JSON-Objekt gespeichert. Machen Sie diese Informationen nicht für die Clientanwendung zugänglich. 

      Ziehen Sie einen der folgenden Ansätze zum Schützen von Berechtigungsnachweisen in Betracht: 

      - Wenn Sie eine angepasste Clientanwendung verwenden, implementieren Sie eine Architektur, die direkte API-Aufrufe der Clientanwendung verhindert. Verwenden Sie beispielsweise einen App-Server, der den REST-Endpunkt der {{site.data.keyword.conversationshort}}-API aufruft und nur das JSON-Ausgabeobjekt von Ihrem App-Server an Ihre Clientanwendung übergibt.
      - Verwenden Sie Webaktionen ohne Authentifizierung. 
      - Begrenzen Sie die Offenlegung, indem Sie den Aufruf durch ein Token authentifizieren, das nur für die Webaktion gilt. 

      Das von Ihnen definierte Berechtigungsnachweisobjekt muss gültige {{site.data.keyword.openwhisk_short}}-Berechtigungsnachweise enthalten. Wie diese angegeben werden, variiert entsprechend dem Standort, an dem der Service gehostet wird, und der Authentifizierungsmethode, die von Instanzen an diesem Standort verwendet wird. Zu den Methoden gehören die folgenden: 

      ```json
      {
        "user":"5tj3b41j-bf3j-5d92-24g9-4a7769ab12af",
        "password":"y65gqSTSRzqE..."
      }
      ```
      {: codeblock}

      oder

      ```json
      {
        "api_key":"user:password"
      }
      ```
      {: codeblock}

      Beim Testen des Dialogmoduls können Sie in der Kontextvariablen `$private.my_credentials` temporär Ihre tatsächlichen Werte für {{site.data.keyword.openwhisk_short}}-Benutzername und -Kennwort angeben, indem Sie auf **Kontext verwalten** in der Anzeige 'Ausprobieren' des Tools klicken. 

      ![Zeigt, wie die Kontextvariable '$private.my_credentials' in der Schnittstelle für Kontextmanagement der Anzeige 'Ausprobieren' definiert wird](images/testing-creds.png)

      {{site.data.keyword.openwhisk_short}} unterscheidet nicht zwischen Aufrufen über die Anzeige 'Ausprobieren' beim Testen und Aufrufen über eine Anwendung im Produktionsbetrieb. Daher können auch für Aufrufe während des Testbetriebs Kosten anfallen.
      {: note}

## Aktion erstellen
{: #dialog-actions-create}

Wenn Sie einen programmgesteuerten Aufruf des Typs 'action' oder 'web action' definieren, müssen Sie den Aufruf zuerst in {{site.data.keyword.openwhisk}} erstellen, bevor er in einem Dialogmodul ausgeführt werden kann. Wenn Sie einen programmgesteuerten Aufruf des Typs 'client' erstellen, können Sie diesen Schritt überspringen.

**Beschränkungen für den Standort**: Gegenwärtig können Sie eine {{site.data.keyword.openwhisk_short}}-Aktion nur über {{site.data.keyword.conversationshort}}-Serviceinstanzen aufrufen, die in Rechenzentren an den Standorten Dallas, Frankfurt, London und Washington DC gehostet werden. Der {{site.data.keyword.conversationshort}}-Service verwendet nur die {{site.data.keyword.openwhisk_short}}-Instanz, die am selben Standort gehostet wird. Der Service überprüft keine {{site.data.keyword.openwhisk_short}}-Instanzen, die an anderen Standorten gehostet werden. Aus diesem Grund sollten Sie über eine in Dallas gehostete {{site.data.keyword.conversationshort}}-Serviceinstanz beispielsweise keine Aktion aufrufen, die in einem definieren, die in einer in Washington DC gehosteten {{site.data.keyword.openwhisk_short}}-Instanz definiert ist. Beachten Sie, dass {{site.data.keyword.conversationshort}}-Serviceinstanzen, die im Rechenzentrum London vor dem 13. Dezember 13 2018 erstellt wurden, in das Rechenzentrum Dallas syndiziert wurden. Solche Instanzen können nur {{site.data.keyword.openwhisk_short}}-Aktionen finden, die ebenfalls in Dallas gehostet werden.
{: important}

**Zeitlimits**: Verwenden Sie die Typen **cloud_function**, **server** und **web_action** nur, um einen Aufruf abzusetzen, der in **weniger als 5 Sekunden** zurückgegeben werden kann. Das Zeitlimit der Anforderung an {{site.data.keyword.openwhisk_short}} wird überschritten, wenn ein einzelner Serviceaufruf länger dauert. Wenn Ihr Dialogmodul mehr als einen Aufruf an einen externen Service absetzt, müssen diese Aufrufe innerhalb von 7 Sekunden abgeschlossen werden. Wenn die ersten 3 Aufrufe jeweils in 2 Sekunden abgeschlossen werden und der vierte Aufruf in mehr als 1 Sekunde, dann wird der vierte Aufruf gestoppt und die Fehlernachricht für diesen Aufruf gibt an, dass der Aufruf nicht abgeschlossen wurde. Verwalten Sie den Aufruf für weniger effiziente Services, die Sie aufrufen müssen, über Ihre Clientanwendung und geben Sie die zugehörigen Informationen in einem separaten Schritt an das Dialogmodul weiter.

So erstellen Sie eine {{site.data.keyword.openwhisk_short}}-Aktion:

1.  Rufen Sie den [{{site.data.keyword.openwhisk_short}}-Online-Editor ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://cloud.ibm.com/openwhisk/create){: new_window} auf, mit dem Sie den Code direkt in Ihrem Browser schreiben.

    Sie können auch eine [Befehlszeilenschnittstelle ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://cloud.ibm.com/openwhisk/learn/cli){: new_window} installieren, mit der Sie lokalen Code schreiben können, um eine Aktion zu definieren.

1.  Erstellen Sie einen der folgenden Aktionstypen:

    - **{{site.data.keyword.openwhisk_short}}-Typ 'action'**: Details enthält der Abschnitt [Aktionen erstellen und aufrufen ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](/docs/openwhisk?topic=cloud-functions-openwhisk_actions){: new_window}.
    - **{{site.data.keyword.openwhisk_short}}-Typ 'web_action'**: Details enthält der Abschnitt [Webaktionen erstellen ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](/docs/openwhisk?topic=cloud-functions-openwhisk_webactions){: new_window}.

    Beachten Sie die folgenden Tipps:

    - Eine Beschreibung der Vorgehensweise zum Aufrufen eines externen Service enthält das [Beispiel für eine {{site.data.keyword.openwhisk_short}}-Aktion ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://cloud.ibm.com/docs/openwhisk/openwhisk_actions#openwhisk_apicall_action){: new_window}.
    - Verwenden Sie zum Aufrufen eines Watson-Service die Komponente [Watson Developer Cloud SDK ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://github.com/watson-developer-cloud){: new_window} für die Sprache, die Sie verwenden möchten.
    - Stellen Sie sicher, dass Ihre {{site.data.keyword.openwhisk_short}}-Aktion die Eingabeparameter als JSON-Objekte akzeptiert und Ausgaben als JSON-Objekte zurückgibt.
    - Wenn Sie Ihre {{site.data.keyword.openwhisk_short}}-Aktion mit Node.js schreiben, stellen Sie sicher, dass `Promise` für die asynchrone Verarbeitung verwendet wird. Stellen Sie außerdem sicher, dass das Endergebnis aus der Funktion `main` zurückgegeben wird.

    Alternativ können Sie eine Folge von Aktionen erstellen.
    {: tip}

## Fehlerbehandlung
{: #dialog-actions-handle-errors}

Wenn die {{site.data.keyword.openwhisk_short}}-Aktion einen Fehler feststellt, wird die Fehlernachricht an das Dialogmodul zurückgegeben und als Eigenschaft der Antwortvariablen `cloud_functions_call_error` gespeichert. Ein Fehler kann beispielsweise auftreten, wenn Ihre {{site.data.keyword.openwhisk_short}}-Aktion keine Antwort von einem externen Service empfängt, oder wenn die Cloud Function-Aktion fehlschlägt. Wenn keine oder fehlerhafte Cloud Function-Berechtigungsnachweise angegeben werden, wird ein Fehler zurückgegeben. Diese Kontextvariable wird nur für Aktionen des Typs 'server' verwendet. Sie sollten in Betracht ziehen, in Ihrer Clientanwendung ein ähnliches Objekt zu erstellen, das Fehlerinformationen aufzeichnet und als Kontextvariable an das Dialogmodul zurückgibt.

Sie können die Antwort des Dialogmodulknotens so konfigurieren, dass zuerst eine Fehlerprüfung erfolgt. Sie können beispielsweise sicherstellen, dass eine Antwort, die auf das Ergebnis einer {{site.data.keyword.openwhisk_short}}-Aktion verweist, nur angezeigt wird, wenn keine Fehler festgestellt wurden. Fügen Sie dazu den folgenden Ausdruck zur Antwortbedingung hinzu:

```bash
  $forecast_result.cloud_functions_call_error == null
```
{: codeblock}

Für einen programmgesteuerten Aufruf des Typs 'client' können Sie Informationen zur Fehlerbehandlung übergeben, indem Sie eine Kontextvariable wie `action_error` definieren. Sie können diese Informationen als Teil der Ergebnisvariable an den Service zurückgeben. Damit nur eine Antwort angezeigt wird, wenn keine Fehler festgestellt wurden, können Sie eine Antwortbedingung wie diese definieren:

```bash
  $forecast_result.action_error == null
```
{: codeblock}

## Beispiel für den Aufruftyp 'client'
{: #dialog-actions-client-example}

Das folgende Beispiel zeigt, wie ein Aufruf an einen externen Wetterdienst aussehen kann. Der Aufruf wird im zugeordneten JSON-Editor der Knotenantwort hinzugefügt. Zu dem Zeitpunkt, an dem die Antwort auf Knotenebene ausgelöst wird, sind die Datums- und Positionsinformationen des Benutzers bereits in Slots erfasst und gespeichert. In diesem Beispiel wird vorausgesetzt, dass der Service, der aufgerufen wird, über einen Endpunkt namens `/weather` verfügt, die Parameter `location` und `date` entgegennimmt und ein JSON-Objekt `{"forecast": "<value>"}`.

 ``` json
{
  "actions": [
    {
      "name": "MyWeatherFunction",
      "type": "client",
      "parameters": {
        "date": "$date",
        "location": "$location"
      },
      "result_variable": "context.my_forecast"
    }
  ]
}
```
{: codeblock}

Normalerweise kehrt der Service von einer Anforderung 'POST /message' nur zum Client zurück, wenn eine neue Benutzereingabe erforderlich ist (z. B. nach dem Ausführen eines übergeordneten Knotens und vor dem Ausführen eines zugehörigen untergeordneten Knotens). Wenn Sie jedoch eine Clientaktion zu einem Knoten hinzufügen, kehrt der Service nach der Auswertung immer zum Client zurück, damit das Ergebnis des Aktionsaufrufs zurückgegeben werden kann. Um unnötiges Warten auf Benutzereingaben zu vermeiden (z. B. wenn ein Knoten so konfiguriert ist, dass er direkt zu einem untergeordneten Knoten springt), fügt der Service den folgenden Wert zum Nachrichtenkontext hinzu:

```json
  {
    "context": {
      "skip_user_input": true
    }
  }
```
{: codeblock}

Wenn der Client eine Aktion ausführen soll, ohne Benutzereingaben abzurufen, können Sie in ähnlicher Weise die Kontextvariable 'skip_user_input' zum übergeordneten Knoten hinzufügen, um die Clientanwendung entsprechend zu instruieren.

Ihre Clientanwendung sollte stets die Kontextvariable 'skip_user_input' prüfen. Wenn sie vorhanden ist, wird keine neue Eingabe vom Benutzer angefordert. Stattdessen wird die Aktion ausgeführt, das Aktionsergebnis zur Nachricht hinzugefügt und die Nachricht an den Service zurückgegeben. Die neue POST-Nachrichtenanforderung sollte die von der vorherigen POST-Nachrichtenantwort zurückgegebene Nachricht (insbesondere Kontext, Eingabe, Entitäten und optional den Ausgabeabschnitt) enthalten. Und sie sollte anstelle des JSON-Objekts, das den programmgesteuerten Aufruf definiert, das Ergebnis enthalten, das von dem programmgesteuerten Aufruf zurückgegeben wurden.

Fügen Sie in einem untergeordneten Knoten, der nach diesem Knoten verarbeitet wird, die Antwort hinzu, die für den Benutzer angezeigt werden soll:

``` json
{
  "output":{
    "text": {
      "values": [
        "Die Wetteraussichten sind $my_forecast $date.literal in $location.literal."
      ]
    }
  }
```
{: codeblock}

Das folgende Diagramm zeigt, wie mit einem Aufruf des Typs 'client' Wetteraussichten abgerufen und an den Benutzer zurückgegeben werden können.

![Zeigt die Anfrage eines Benutzers nach einer Wettervorhersage und das Dialogmodul zum Senden einer Anfrage an eine Client-App, die die Anfrage an den externen Service sendet](images/forecast.png)

## Beispiel für das Aufrufen einer IBM Cloud Functions-Aktion
{: #dialog-actions-server-example}

Das folgende Beispiel zeigt, wie eine {{site.data.keyword.openwhisk_short}}-Aktion aufgerufen werden kann. Das Beispiel veranschaulicht die Verwendung der {{site.data.keyword.openwhisk_short}}-Aktion 'echo' aus dem [Paket 'Utilities' ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://cloud.ibm.com/docs/openwhisk/openwhisk_actions#openwhisk_create_action_sequence){: new_window}, das mit dem Service bereitgestellt wird. Die Aktion nimmt eine Textzeichenfolge entgegen und gibt sie zurück.

``` json
{
  "actions": [
    {
      "name": "/whisk.system/utils/echo",
      "type":"cloud_function",
      "parameters": {
        "message": "<?input.text?>"
      },
      "result_variable": "context.my_input_returned",
      "credentials":"$private.my_credentials"
    }
  ]
}
```
{: codeblock}

Die Ausgabe der {{site.data.keyword.openwhisk_short}}-Aktion wird in der Variable 'context.my_input_returned' gespeichert und kann von den nachfolgenden Dialogmodulknoten aufgerufen werden.

``` json
 {
  "output":{
    "text": {
      "values": [
        "Ihre Eingabe lautete: $my_input_returned."
      ]
    }
  }
}
```
{: codeblock}

Das folgende Diagramm veranschaulicht das Aufrufen einer {{site.data.keyword.openwhisk_short}}-Aktion durch einen einfachen Beispielaufruf des integrierten Echoservice von {{site.data.keyword.openwhisk_short}}. Dabei wird eine Eingabe vom Benutzer angefordert und an den Echoservice zurückgegeben. Der Echoservice gibt diesen Text zurück, der für den Benutzer angezeigt wird.

![Zeigt, wie eine Person Text eingibt und das Dialogmodul die Anforderung an den Service sendet. Anschließend wird der Text an das Dialogmodul zurückgegeben.](images/echo-via-cf.png)

### Beispiel für die Aktion 'Echo'
{: #dialog-actions-echo-example}

So zeigen Sie einen Dialogskill mit einem bereits eingerichteten Dialogmodul zum Aufrufen der integrierten {{site.data.keyword.openwhisk_short}}-Aktion 'Echo' an:

1.  Laden Sie die Datei [CloudFunctionsEcho.json ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://raw.githubusercontent.com/watson-developer-cloud/community/master/watson-assistant/cloud-functions-echo.json){: new_window}.
1.  Importieren Sie die JSON-Datei als neuen Dialogskill.
1.  Überprüfen Sie, wie der Aufruf der Aktion 'Echo' im Dialogmodul angegeben wird.
1.  Klicken Sie in der Anzeige 'Ausprobieren' auf **Kontext verwalten**  und geben Sie in den Kontextvariablen vorübergehend Ihren {{site.data.keyword.openwhisk_short}}-Benutzernamen und das zugehörige Kennwort an.

    ```json
    $private.my_credentials
    {
      "user":"<ihr_benutzername_für_die_CF-instanz>",
      "password":"<ihr_kennwort_für_die_CF-insatnz>"
    }
    ```
    {: codeblock}

    oder

    ```json
    $private.my_credentials
    {
      "api_key":"user:password"
    }
    ```
    {: codeblock}

1.  Testen Sie das Dialogmodul mit einer Beispieleingabe.

    Der Service verwendet die {{site.data.keyword.openwhisk_short}}-Aktion 'Echo', um die Eingabe an Sie zurückzugeben.

## Beispielaufruf für IBM Cloud Functions-Webaktion
{: #dialog-actions-web-action-example}

Das folgende Beispiel zeigt, wie ein Aufruf an eine Webaktion aussehen kann. Dieses Beispiel zeigt, wie ein Benutzername an einen einfachen Begrüßungsservice übergeben wird. Der Service gibt eine Begrüßung zurück, in der der Benutzer namentlich angesprochen wird. 

  ``` json
  {
    "actions": [
        {
        "name": "jdoe@example.com_sales/default/hello",
        "type":"web_action",
        "parameters": {
          "name": "<? context['name'] ?>"
        },
        "result_variable": "context.greet_user",
        "credentials": "<Siehe unten>"
      }
    ]
  }
  ```
  {: codeblock}

  Dabei wird `"credentials"` auf eine der folgenden Arten angegeben:

  - Ohne Authentifizierung:

    ```json
    "credentials": null
    ```
    {: screen}

  - Authentifizierung mit Ihren {{site.data.keyword.openwhisk_short}}-Berechtigungsnachweisen

    ```json
    "credentials": "$private.my_credentials"
    ```
    {: screen}

    Dabei ist `$private.my_credentials` wie folgt definiert:

    ```json
    $private.my_credentials
    {
      "user":"<ihr_benutzername_für_die_CF-instanz>",
      "password":"<ihr_kennwort_für_die_CF-insatnz>"
    }
    ```
    {: screen}

    oder 

    ```json
    $private.my_credentials
    {
      "api_key":"username:password"
    }
    ```
    {: screen}
  
  - Authentifizierung mit einem spezifischen Token für die Webaktion

    ```json
    "credentials": "$private.my_credentials"
    ```
    {: screen}

    Dabei ist `$private.my_credentials` wie folgt definiert:

    ```json
    $private.my_credentials
    {
      "web_action_key":"<geheimer_schlüssel_für_aktion>"
    }
    ```
    {: screen}

Die Ausgabe der Webaktion, die in der Variablen `context.greet_user` gespeichert ist, kann jetzt von nachfolgenden Dialogmodulknoten aufgerufen werden.

``` json
 {
  "output":{
    "text": {
      "values": [
        "$greet_user"
      ]
    }
  }
}
```
{: codeblock}

## Beispielaufruf für erweiterte IBM Cloud Functions-Aktion
{: #dialog-actions-advanced-example}

Sie können in einem einzigen Dialogmodulablauf mehrere Aktionen aufrufen. Bis zu fünf Aktionen können innerhalb eines JSON-Objekts `actions` in einem einzigen Dialogmodulknoten aufgerufen werden. Jedoch werden alle Aktionen des Typs 'server', die in einem JSON-Array `actions` definiert sind, parallel verarbeitet. Es ist daher nicht möglich, eine Aktion des Typs 'server' aufzurufen und das Ergebnis dieser Aktion an eine zweite Aktion des Typs 'server' im selben Block `actions` zu übergeben. Die beste Methode zum Aufrufen von Serveraktionen in einer bestimmten Reihenfolge ist eine {{site.data.keyword.openwhisk_short}}-Sequenz. Diese Methode benötigt weniger Zeit, da das Dialogmodul mit nur einem externen Aufruf mehrere Aktionen ausführen kann. Um eine Sequenz zu verwenden, verweisen Sie in der Definition des Blocks `actions` einfach auf den Namen einer Sequenz anstatt auf den Namen einer Aktion. Alternativ können Sie die erste Aktion des Typs 'server' in einem Knoten aufrufen und zu einem untergeordneten Knoten springen, der die nächste Aktion des Typs 'server' aufruft.

Wenn Sie ein Array `actions` mit den Aktionstypen 'client' und 'server' definieren, werden die Aktionen des Typs 'client' beim Ausführen des Dialogmodulknotens erst an den Client gesendet, nachdem alle Aktionen des Typs 'server' verarbeitet wurden.
{: tip}

Das folgende Beispiel zeigt, wie ein Aufruf für eine {{site.data.keyword.openwhisk_short}}-Aktion aussehen kann.

Dieses Beispiel zeigt, wie mit einer {{site.data.keyword.openwhisk_short}}-Aktion ein externer Service aufgerufen wird, der einen Ortsnamen entgegennimmt und die Koordinaten der Breiten- und Längengrade der entsprechenden Position zurückgibt.

``` json
{
  "actions": [
        {
      "name": "/jdoeorg_prod/get coordinates",
      "type":"cloud_function",
      "parameters": {
        "location": "$location"
      },
      "result_variable": "context.my_coordinates",
      "credentials":"$private.my_credentials"
    }
  ]
}
```
{: codeblock}

Die Kontextvariable `$my_coordinates` speichert die beiden Werte, die vom Service `get coordinates` zurückgegeben werden, wie folgt:

```json
{
 "lat":"42.3611",
 "long":"-71.0571"
}
```
{: codeblock}

Dieses Beispiel veranschaulicht die Verwendung der {{site.data.keyword.openwhisk_short}}-Aktion `forecast` aus dem [Paket 'Weather' ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://cloud.ibm.com/docs/openwhisk/openwhisk_weather#openwhisk_catalog_weather){: new_window}, das mit dem {{site.data.keyword.openwhisk_short}}-Service bereitgestellt wird. Die Aktion erwartet Koordinaten der Breiten- und Längengrade und einen Zeitraum. Sie gibt ein JSON-Objekt mit den Wetteraussichten für die angegebene Position und den angegebenen Zeitraum zurück. Die von der vorherigen Aktion zurückgegebenen Koordinaten werden als `$my_coordinates.lat` und `$my_coordinates.long` angegeben.

``` json
{
  "actions": [
    {
      "name": "/whisk.system/weather/forecast",
      "type":"cloud_function",
      "parameters": {
        "latitude": "$my_coordinates.lat",
        "longitude": "$my_coordinates.long",
        "timePeriod": "$period",
        "username": "$private.my_weather_service.username",
        "password": "$private.my_weather_service.password"
      },
      "result_variable": "context.forecasts",
      "credentials":"$private.my_credentials"
    }
  ]
}
```
{: codeblock}

Ein Benutzername und ein Kennwort werden als Parameter aufgelistet. Sie sind nur vorhanden, weil sie für die Aktion erforderlich sind. Sie ergeben zusammen die erforderlichen Berechtigungsnachweise für den externen Wetterservice der als Back-End von der bereitgestellten Aktion aufgerufen wird. Dies sind nicht die Berechtigungnachweise des IBM Cloud Function-Kontos. Sorgen Sie dafür, dass auch diese Berechtigungsnachweise vertraulich bleiben.
{: note}

Die Ausgabe der {{site.data.keyword.openwhisk_short}}-Aktion, die in der Variablen `context.forecasts` gespeichert ist, kann jetzt von nachfolgenden Dialogmodulknoten aufgerufen werden.

``` json
 {
  "output":{
    "text": {
      "values": [
        "Für den Zeitraum $period können Sie in $location mit $forecasts rechnen."
      ]
    }
  }
}
```
{: codeblock}

Das folgende Diagramm veranschaulicht eine komplexe Interaktion. Ein Benutzer fragt nach einer Wettervorhersage. Die Slots im Dialogmodulknoten fordern den Benutzer auf, den gewünschten Ort und Zeitraum anzugeben. Beim Aufrufen des {{site.data.keyword.openwhisk_short}}-Service für Wettervorhersagen müssen Sie geografische Koordinaten angeben. Daher ermittelt das Dialogmodul zuerst den Längen- und Breitengrad für den vom Benutzer angegebenen Ort, indem eine Anforderung an den {{site.data.keyword.openwhisk_short}}-Service gesendet und von dort an einen externen Service übermittelt wird, der die entsprechenden Koordinaten zurückgibt. In einem nachfolgenden Knoten sendet das Dialogmodul die empfangenen Koordinaten zusammen mit dem vom Benutzer angegebenen Zeitraum an den integrierten {{site.data.keyword.openwhisk_short}}-Vorhersageservice, um die Vorhersagedetails abzurufen. Anschließend beantwortet das Dialogmodul die Frage des Benutzers durch das Anzeigen des Vorhersageergebnisses, das vom {{site.data.keyword.openwhisk_short}}-Vorhersageservice bereitgestellt wurde.

![Zeigt die Frage eines Benutzers nach einer Wettervorhersage, die vom Dialogmodul an den Cloud Function-Service gesendet und von dort an den externen Service weitergeleitet wird.](images/forecast-via-cf.png)
