# Praxisbericht – Felix Heinrich
**Studiengang:** B.Sc. Angewandte Informatik | HTW Berlin, FB 4
**Matrikelnummer:** –
**Praktikumsbetrieb:** Immobilien Scout GmbH (ImmoScout24), Berlin
**Praktikumszeitraum:** 15. November 2023 – 31. Oktober 2025

---

## 1. Zeitraum und Praktikumsstelle

### Unternehmen

ImmoScout24 ist Deutschlands größtes Online-Immobilienportal und gehört zur Scout24 SE, einem börsennotierten Digitalunternehmen im DAX. Das Kernprodukt der Plattform ist die Immobiliensuche für Mieter, Käufer, Vermieter und Makler mit monatlich über 20 Millionen Nutzern. Neben der klassischen Immobiliensuche betreibt ImmoScout24 ein wachsendes Ökosystem rund um den gesamten Immobilienlebenszyklus: von der Bonitätsprüfung über Finanzierungsvermittlung bis hin zu Umzugsservices. Strategisch setzt Scout24 dabei stark auf moderne Technologien wie Datenanalytik, A/B-Testing und künstliche Intelligenz, um Nutzererlebnisse zu personalisieren und Conversion-Raten zu optimieren. Der Hauptsitz befindet sich in München, der Großteil der Produktentwicklung findet jedoch am Berliner Standort statt, wo das Unternehmen rund 1.100 Mitarbeiterinnen und Mitarbeiter aus über 60 Nationen beschäftigt.

### Rolle und Team

Ich war als Werkstudent im Bereich Fullstack-Entwicklung tätig und gehörte dem **Team Honey Badgers** an, das zur sogenannten *Builders Organisation* innerhalb von ImmoScout24 zählt. Das Team verantwortet den **MeinPlus-Produktbereich** (`/meinkonto/meinplus/`), ein Portal in dem Nutzer ihre Consumer-Subscriptions und die enthaltenen Mehrwertleistungen verwalten können. ImmoScout24 bietet drei solcher Subscriptions an: **MieterPlus** (für aktiv Wohnungssuchende, mit erhöhter Bewerbungssichtbarkeit, SCHUFA-Bonitätscheck und weiteren Services), **WohnenPlus** (für Nutzer im bestehenden Mietverhältnis mit ähnlichem Leistungsumfang) sowie **KäuferPlus** (für Kaufinteressierte mit Markdaten und Immobilienbewertungen). Meine Arbeitszeit betrug während der Vorlesungszeit maximal 20 Stunden pro Woche, in den Semesterferien bis zu 40 Stunden.

---

## 2. Aufgabenbereich & Technologieüberblick

### Produktkontext: MeinPlus

Der MeinPlus-Bereich ist das zentrale Portal, in dem Nutzerinnen und Nutzer ihre aktive Subscription einsehen und die enthaltenen Services nutzen können. Jede Subscription wird als eigener Tab dargestellt; die einzelnen enthaltenen Leistungen (z.B. SCHUFA-Auskunft, Schlüsselnotdienst) erscheinen als anklickbare Kacheln. Ein Klick auf eine Kachel öffnet ein Modal mit weiteren Informationen und nutzerrelevanten Aktionen. Ich habe sowohl an der Benutzeroberfläche dieses Portals als auch am zugehörigen Backend und an einem internen Admin Panel gearbeitet.

### Repositories

| Repository | Beschreibung |
|---|---|
| `is24-mein-plus-benefits-frontend` | React Web App für den MeinPlus-Bereich (`/meinkonto/meinplus/`) |
| `is24-consumerbenefits` | Java Spring Boot Backend – hält Daten, Feature Switches und APIs für den MeinPlus-Bereich |
| `is24-consumerbenefits-admin-frontend` | Internes Admin Panel zur Verwaltung von Feature Switches und KPIs |
| `is24-rent-profile-frontend` | Frontend für Profil & Dokumente (Identitätsnachweis, Solvency) |
| `myscout-ui-dashboard` | Zentrales Nutzer-Dashboard mit MeinPlus-Einstiegskachel |
| `is24-quick-cart-frontend` | Checkout-Frontend für Subscription-Käufe |

### Tech Stack & Tools

| Bereich | Technologie |
|---|---|
| ⚛️ Frontend | React, TypeScript, i18n, TanStack Query |
| ☕ Backend | Java, Spring Boot, OpenAPI / Swagger |
| 🧪 Testing | Playwright (E2E), Unit Tests (Jest) |
| 🚀 CI/CD | Jenkins |
| 🔬 A/B-Testing | Optimizely |
| 📣 Marketing Automation | Iterable (Embedded Messages) |
| 🎨 Design System | Storybook, Figma, shadcn/ui |
| 📊 Monitoring | Datadog RUM |
| 📋 Projektmanagement | Jira, Confluence |
| 💬 Kommunikation | Slack, Zoom, Outlook |

---

## 3. Eigene Aufgaben und Tätigkeiten

### 3.1 Onboarding & erste Feature-Arbeiten

**Art und Umfang**

Meine ersten Wochen bei ImmoScout24 waren geprägt vom Onboarding in eine sehr große, gewachsene Codebase. Das Team arbeitete nach dem agilen Scrum-Prinzip mit zweiwöchigen Sprints, und ich wurde schrittweise in den Entwicklungsworkflow eingeführt: von der Ticket-Management in Jira über die lokale Entwicklung bis hin zum Deployment-Prozess über Jenkins in eine Sandbox-Umgebung und schließlich in die Produktion.

Meine ersten eigenständigen Tickets nach dem Onboarding waren Copy-Fixes und Bugfixes im WohnenPlus-Bereich. Konkret hatten Nutzer, die ausschließlich eine WohnenPlus-Subscription besaßen, in verschiedenen Modals und Kacheln falsche Texte und Links angezeigt bekommen – etwa den Hinweis „In MieterPlus enthalten", obwohl sie eine WohnenPlus-Subscription hatten, oder einen Link zur MieterPlus-Landingpage statt zur WohnenPlus-Seite. Außerdem umfassten die ersten Aufgaben UI-Verbesserungen: Der Footer-Bereich mit dem Call-to-Action-Button in den Benefit-Modals war nicht sticky – bei kleinen Bildschirmen war der CTA erst nach dem Scrollen sichtbar, was die Conversion negativ beeinflusste. Ein weiteres Ticket befasste sich mit dem Verhalten des Browser-Zurück-Buttons beim Schließen von Modals.

**Technische Umsetzung und Methoden**

Das MeinPlus-Frontend nutzt ein zentrales i18n-System (Internationalisierung) zur Verwaltung aller angezeigten Texte für multi-language-support. Die Übersetzungen sind in separaten TypeScript-Dateien (`de.ts`, `en.ts`) hinterlegt und werden über einen custom Hook mit einer Content-ID in die jeweiligen Komponenten eingebunden. Für die Copy-Fixes war es daher ausreichend, den falschen Text in der Übersetzungsdatei anzupassen und abhängig von der Subscription des Nutzers die korrekte Variante anzuzeigen. Etwas komplizierter wurde es, wenn Textblöcke Variablen oder spezielle Formatierungen (z.B. `<b>` für Fettschrift) enthielten, da dann der entsprechende Formatierungs-Hook beachtet werden musste.

Für den sticky Footer reichten einfache CSS-Anpassungen. Das Browser-History-Problem hingegen erforderte ein Grundverständnis der Browser History API: Beim Öffnen eines Modals muss ein neuer History-Eintrag gepusht werden, sodass der Zurück-Button das Modal schließt statt zur vorherigen Seite zu navigieren – ein Verhaltensmuster, das Nutzer von nativen Apps gewohnt sind.

**Lerneffekte**

Diese ersten Aufgaben lehrten mich vor allem, wie man sich in einer großen, unbekannten Codebase orientiert: Ordnerstruktur lesen, gezielt nach Komponentennamen suchen, globale Regex-Suche einsetzen und den Zusammenhang zwischen Komponenten verstehen. Darüber hinaus lernte ich den vollständigen CI/CD-Flow des Teams kennen und verstand, wie komponentengetriebene Architekturen dabei helfen, Änderungen an einer zentralen Stelle vorzunehmen, ohne viele Dateien anfassen zu müssen.

---

### 3.2 A/B-Testing mit Optimizely

**Art und Umfang**

A/B-Testing war ein wiederkehrendes Thema während meiner gesamten Zeit bei ImmoScout24. Für die Planung und Durchführung von Experimenten nutzte das Team **Optimizely**, eine spezialisierte Plattform für Feature-Management und kontrollierte Nutzertests. Ich habe mehrere Experimente implementiert, darunter einen Platzierungstest für ein WohnenPlus-Upsell-Element im Merkzettel (der Immobilien-Merkliste des Nutzers) sowie eine Versuchsreihe rund um das WohnenPlus-Upsell-Modal mit mehreren Iterationen.

**Technische Umsetzung und Methoden**

Der Ablauf eines Experiments folgte einem klaren Prozess: Das Product- und Data-Team definiert die Hypothese und legt das Experiment in Optimizely an. Dabei werden mehrere Varianten erstellt (z.B. Variante A: kein Upsell-Banner, Variante B: Banner an Position X). Der Engineer hinterlegt die Experiment-ID und Varianten-IDs im Frontend-Projekt und ruft über einen custom Hook die jeweils aktive Variante für den eingeloggten Nutzer ab. Optimizely übernimmt dabei die Traffic-Aufteilung und stellt sicher, dass ein Nutzer konsistent dieselbe Variante sieht. Die Komponente rendert dann je nach zurückgegebener Variante unterschiedliche Inhalte.

Mindestens ebenso wichtig wie die Implementierung ist der **vollständige Experiment-Lifecycle**: Nach dem Launch wird das Experiment über einen definierten Zeitraum laufen gelassen, bis das Data-Team statistische Signifikanz feststellt. Fällt das Ergebnis positiv aus, wird die siegreiche Variante als Standard eingeführt und der Experiment-Code bereinigt (Cleanup). Bei einem der WohnenPlus-Upsell-Experimente war genau dieser Cleanup meine Aufgabe: Das Experiment hatte sich als erfolgreich erwiesen, also wurde die Upsell-Komponente fest in die Produktion übernommen, die Experiment-IDs entfernt und ggf. unit oder E2E-Tests und auf die neue Funktionalität angepasst.
Mindestens ebenso wichtig wie die Implementierung ist der **vollständige Experiment-Lifecycle**: Nach dem Launch wird das Experiment über einen definierten Zeitraum laufen gelassen, bis das Data-Team statistische Signifikanz feststellt. Fällt das Ergebnis positiv aus, wird die siegreiche Variante als Standard eingeführt und der Experiment-Code bereinigt (Cleanup). Bei einem der WohnenPlus-Upsell-Experimente war genau dieser Cleanup meine Aufgabe: Das Experiment hatte sich als erfolgreich erwiesen, also wurde die Upsell-Komponente fest in die Produktion übernommen, die Experiment-IDs entfernt und ggf. Unit- oder E2E-Tests 
an die neue Funktionalität angepasst.

**Lerneffekte**

A/B-Testing war für mich ein völlig neues Konzept. Ich habe verstanden, warum Unternehmen dieser Größe kaum noch Features ohne vorherige Validierung durch Daten einführen – jede Änderung an conversion-kritischen Stellen kann erhebliche Auswirkungen auf den Umsatz haben. Technisch habe ich gelernt, wie Feature-Flag-Architekturen funktionieren und wie man den gesamten Lifecycle eines Experiments – von der Implementierung bis zum Cleanup – sauber durchführt.

---

### 3.3 SCHUFA & Solvency-Integration

**Art und Umfang**

Die Integration des SCHUFA- und Bonitätsprüfungs-Features in den MeinPlus-Bereich war das umfangreichste Feature, das ich bei ImmoScout24 eigenständig umgesetzt habe. Es erstreckte sich über mehrere Wochen und war das erste Mal, dass ich gleichzeitig Änderungen im Frontend und im Backend vorgenommen habe – was mir ein viel tieferes Verständnis dafür gegeben hat, wie beide Schichten zusammenhängen.

Zur Einordnung: ImmoScout24 bietet Wohnungssuchenden zwei Arten von Bonitätsnachweisen an. Zum einen die klassische **SCHUFA-Auskunft** (über den Drittanbieter BoniCheck), die ein sofortiges digitales Zertifikat mit vermieterrelevanten Daten liefert. Zum anderen eine eigene Lösung namens **IS24-Solvency**, die eine Bonitätsauskunft von ImmoScout selbst ist. MieterPlus- und WohnenPlus-Abonnenten hatten bereits Anspruch auf eine kostenlose SCHUFA-Auskunft pro Jahr. Ziel des Features war es den Nutzern, basierend auf ihrem Paket SCHUFA oder IS24-Solvency anzubieten, SCHUFA zu monetarisieren und im MeinPlus Bereich die Solvency-Dokumente, welche ein Nutzer bereits besitzt, im passenden Modal entsprechend anzuzeigen.

**Technische Umsetzung und Methoden**

Die Implementierung umfasste sowohl Frontend- als auch Backend-Anteile:

Auf der **Frontend-Seite** habe ich eine neue `SolvencyChoiceModal`-Komponente entwickelt, die die zentrale Business-Logik enthält: Hat der Nutzer eine SCHUFA-Auskunft, wird das SCHUFA-Modal angezeigt. Besitzt er eine IS24-Solvency, erscheint das IS24-Modal. Hat er beides, wird das IS24-Modal bevorzugt, da es beide Nachweise in einer Übersicht zusammenführt. Zusätzlich habe ich eine Identity-Sektion in der Rent-Profile-Seite implementiert, die anzeigt ob ein Nutzer sich bei ImmoScout verifiziert hat.

Auf der **Backend-Seite** habe ich im `is24-consumerbenefits`-Projekt mehrere neue REST-API-Endpoints implementiert. Den `/solvency/info`-Endpoint, der Daten von BoniCheck abfragt und an das Frontend weiterreicht, sowie einen Proxy-Endpoint für die IS24-Solvency. Der Proxy-Endpoint war notwendig, da das MeinPlus-Frontend aus Authentifizierungsgründen nicht direkt mit dem IS24-Solvency-Backend kommunizieren durfte – alle Anfragen mussten über das `is24-consumerbenefits`-Backend als Zwischenschicht laufen.

**Herausforderungen**

Eine der zentralen Herausforderungen war das Konzept des Proxy-Endpoints: Ich hatte zuvor noch nicht damit gearbeitet und musste zunächst verstehen, warum ein solcher Umweg überhaupt notwendig ist – nämlich weil das Frontend aus Authentifizierungsgründen nicht direkt mit dem IS24-Solvency-Backend kommunizieren durfte. Im `is24-consumerbenefits`-Backend gab es bereits ein bestehendes Beispiel, das Spring's `RestTemplate` für ausgehende HTTP-Anfragen verwendete. Ich habe dieses Muster analysiert, die Funktionsweise recherchiert und es anschließend auf meinen Anwendungsfall übertragen. Diese Vorgehensweise – vorhandene Lösungen im Codebase als Referenz zu nutzen – hat sich bei dem meisten Aufgaben als sehr effektiv erwiesen.

**Lerneffekte**

Dieses Feature war der Wendepunkt in meiner Zeit bei ImmoScout24. Zum ersten Mal habe ich ein vollständiges Feature von der Datenbankabfrage im Backend über die API-Schicht bis zur Darstellung im Frontend umgesetzt. Ich habe verstanden, wie Frontend und Backend über REST-APIs kommunizieren, warum Proxy-Endpoints in verteilten Systemen sinnvoll sind und wie man Spring Boot lokal für die Entwicklung einrichtet.

---

### 3.4 Feature Flags & koordinierter Multi-Repository-Rollout

**Art und Umfang**

Feature Flags waren ein zentrales Werkzeug im Entwicklungsalltag des Teams und waren deshalb of Bestandteil meiner Aufgaben. Die Idee dahinter ist einfach: Neue Funktionen werden nicht direkt für alle Nutzer aktiviert, sondern zunächst hinter einem Flag versteckt. Das erlaubt es, Code bereits in die Produktion einzuspielen, ohne die Funktion sichtbar zu machen – und im Fehlerfall mit einem einzigen Schalter zurückzurufen, ohne einen erneuten Deployment-Vorgang anstoßen zu müssen.

**Technische Umsetzung und Methoden**

Bei ImmoScout24 existierten drei verschiedene Mechanismen für Feature Flags, je nach Anwendungsfall:

1. **LocalStorage-Overrides (Frontend):** Für die lokale Entwicklung und Tests in der Sandbox-Umgebung konnten Entwickler über den Browser-LocalStorage bestimmte Features manuell aktivieren, ohne dass Endnutzer davon betroffen waren.

2. **Datenbankbasierte Feature Switches (Backend):** Der `is24-consumerbenefits`-Backend hält eine dedizierte Datenbanktabelle mit Feature Switches. Diese können über das Admin Panel verwaltet werden und wirken sich auf alle verbundenen Repositories aus. Diesen Mechanismus habe ich für das Admin Panel-Feature (Task 3.5) und mehrere Rollouts implementiert.

3. **Optimizely (A/B-Tests):** Für kontrollierte Experimente mit Nutzer-Traffic-Aufteilung, wie in Abschnitt 3.2 beschrieben.

Ein besonders lehrreiches Beispiel für den koordinierten Rollout war das **Plus-Rebranding**: ImmoScout24 hat seine Subscription-Produkte umbenannt – aus „WohnenPlus" wurde „Wohnen+", aus „MieterPlus" wurde „Mieter+" und so weiter. Was nach einer einfachen Text-Änderung klingt, war technisch eine koordinierte Aktion über vier Repositories gleichzeitig: das MeinPlus-Frontend, das MyScout-Dashboard, das Quick-Cart-Checkout-Frontend und das ConsumerBenefits-Backend. Alle Änderungen wurden hinter einem einzigen Feature Switch implementiert, sodass der Rollout über alle Repositories hinweg synchron erfolgen konnte. Ich habe diese Änderungen in mehreren der genannten Repositories umgesetzt.

Zum Thema Feature Flags gehörte auch das regelmäßige **Cleanup nach einem Rollout**: Sobald ein Feature stabil in der Produktion lief und kein Rollback mehr nötig war, wurden der Feature-Flag-Code, bedingte Abfragen und temporäre Testkonfigurationen entfernt. Dies ist wichtig, um die Codebase lesbar und wartbar zu halten.

**Lerneffekte**

Das Arbeiten mit Feature Flags hat mir ein ganz anderes Verständnis von Software-Deployment vermittelt. In großen Systemen mit Millionen von Nutzern ist ein sofortiger, unkontrollierter Rollout oft nicht die beste Lösung. Die Möglichkeiten für Feature-Flags (LocalStorage, Datenbank, Optimizely) zeigten mir, wie verschiedene Anforderungen (Entwicklertest, Produktions-Rollout, A/B-Test) zu unterschiedlichen technischen Lösungen führen. Der koordinierte Multi-Repository-Rollout verdeutlichte außerdem, wie wichtig klare Absprachen und eine saubere Architekturdokumentation in verteilten Systemen sind.

---

### 3.5 Admin Panel & OpenAPI-Driven Development

**Art und Umfang**

Parallel zu den feature-orientierten Aufgaben habe ich über einen längeren Zeitraum an der Entwicklung und Erweiterung des internen Admin Panels gearbeitet. Das Admin Panel (`is24-consumerbenefits-admin-frontend`) ist ein internes Tool für das Team selbst: Es ermöglicht die einfache Verwaltung von Feature Switches und gibt einen Überblick über relevante KPIs. Das Product-Team nutzte es außerdem, um aggregierte Produktdaten einzusehen. Das Panel ist mit dem `is24-consumerbenefits`-Backend verbunden und bezieht seine Daten direkt aus dessen Datenbank.

Konkret habe ich folgende Funktionalitäten implementiert: eine **Feature-Switch-Verwaltungsseite**, über die Feature Switches direkt im Browser aktiviert und deaktiviert werden können, sowie zwei **KPI-Dashboard-Kacheln** – eine für den Status der MagentaTV-Rabattcodes (wie viele der verfügbaren Codes bereits an Nutzer verteilt wurden) und eine für die Anzahl der digitalen Adressänderungen.

**Technische Umsetzung und Methoden**

Besonders interessant an diesem Projekt war der Einsatz von **OpenAPI-Driven Development**: Alle API-Endpoints des `is24-consumerbenefits`-Backends sind in einer zentralen `openapi.yaml`-Datei spezifiziert. Mithilfe des **`openapi-generator-maven-plugin`** wird daraus automatisch ein Java-Interface generiert, das der Entwickler dann nur noch implementieren muss – Methodensignaturen, Request- und Response-Typen werden vollständig vom Generator übernommen. Für das Frontend wird entsprechend ein typsicherer TypeScript-Client generiert, sodass API-Interfaces nicht manuell gepflegt werden müssen.

Für die Visualisierung der KPI-Daten habe ich **shadcn/ui** eingesetzt, eine Komponentenbibliothek für React, die vorgefertigte, gut anpassbare UI-Elemente wie Diagramme und Tabellen bereitstellt.

**Lerneffekte**

OpenAPI-Driven Development war ein komplett neues Konzept für mich. Ich fand den Ansatz sehr überzeugend: Die API-Spezifikation in `openapi.yaml` dient gleichzeitig als Dokumentation (über Swagger UI), als Vertrag zwischen Frontend und Backend und als Grundlage für die Code-Generierung. Änderungen an der API müssen nur an einer Stelle vorgenommen werden, und beide Seiten werden automatisch konsistent gehalten. Dies spart Entwicklungszeit und reduziert Fehler durch manuelle Synchronisierung.

---

### 3.6 MagentaTV-Partnerschaft

**Art und Umfang**

Eine weitere Aufgabe, die ich bei ImmoScout24 zu großen Teilen allein umgesetzt habe, war die End-to-End-Integration einer Kooperation mit der Deutschen Telekom: Nutzer, die MieterPlus kaufen, erhalten einen Rabattcode für MagentaTV – Telekomsm Streaming-Dienst. Das Feature musste über alle drei Schichten implementiert werden: Frontend, Backend und Admin Panel.

**Technische Umsetzung und Methoden**

Auf der **Frontend-Seite** habe ich im MeinPlus-Bereich eine neue MagentaTV-Kachel hinzugefügt sowie ein dazugehöriges Modal entwickelt. Das Modal enthält eine bedingte Logik: Hat der Nutzer MieterPlus gekauft, wird ihm sein persönlicher Rabattcode angezeigt. Hat er noch kein MieterPlus, sieht er stattdessen eine Handlungsaufforderung zum Kauf. Für das Modal wurden neue Textblöcke in den i18n-Übersetzungsdateien angelegt und ein MagentaTV-Banner als Bild eingebunden.

Auf der **Backend-Seite** habe ich einen neuen API-Endpoint zur Abfrage des nutzerspezifischen Rabattcodes im `is24-consumerbenefits`-Projekt implementiert. Der Rollout erfolgte über einen Feature Switch, der es erlaubte, die Integration zunächst intern zu testen und dann schrittweise für Nutzer freizuschalten. Nach einem Monitoring-Zeitraum von zwei Tagen ohne Probleme wurde der Switch aktiviert und schließlich im Rahmen eines Cleanup-Tickets der Flag-Code entfernt.

Für das **Admin Panel** habe ich zusätzlich eine neue Dashboard-Seite mit einem Balkendiagramm implementiert, das den Verbrauchsstatus der MagentaTV-Rabattcodes visualisiert – wie viele Codes insgesamt verfügbar sind und wie viele bereits an Nutzer vergeben wurden. Diese Information war für das Product-Team regelmäßig relevant.

**Lerneffekte**

Das MagentaTV-Feature war ein gutes Beispiel dafür, wie ein Feature im Unternehmenskontext selten isoliert in einem einzigen Repository umgesetzt werden kann. Die Notwendigkeit, Frontend, Backend und Admin Panel kohärent zu entwickeln und abzustimmen, verdeutlichte den Wert einer klaren Architektur und gut definierter API-Contracts. Außerdem habe ich gelernt, wie ein Feature Switch für einen kontrollierten Rollout in einer Produktionsumgebung mit echten Nutzern eingesetzt wird.

---

### 3.7 Weitere Tätigkeiten

**Iterable / Embedded Messages**

Eine kürzere, aber interessante Aufgabe war die Integration eines eingebetteten SCHUFA-Upsell-Banners in den Property-Hub – also die Exposé-Seite einer Immobilienanzeige – über die Marketing-Automation-Plattform **Iterable**. Die Idee hinter dem Feature ist, dass das Entwicklungsteam lediglich einen Platzhalter (Slot) im Frontend implementiert, während das Marketing-Team über die Iterable-Plattform eigenständig und ohne Code-Änderungen den Inhalt einsetzen und anpassen kann. Meine Aufgabe bestand darin, diesen Platzhalter zu implementieren sowie einen Authentifizierungs-Endpoint im Backend bereitzustellen, der für die Kommunikation mit Iterable benötigt wurde.

Neu für mich war hier die Zusammenarbeit mit nicht-technischen Kollegen aus dem Marketing-Team. Ich musste klar kommunizieren, welche Informationen ich von ihnen benötigte (z.B. Slot-Konfigurationen, Content-Einschränkungen) und ihnen verständlich machen, was ich als Entwickler liefern würde. Diese Form der Schnittstellenarbeit zwischen Entwicklung und Marketing war eine wertvolle Erfahrung.

**KI-Chatbot-Integration (Ainavio)**

Im Rahmen der Integration des KI-Chatbots *HeyImmo* (bereitgestellt durch Ainavio) in den MeinPlus-Bereich habe ich Frontend-Tracking implementiert sowie einen iFrame-Event-Listener entwickelt, der auf Ereignisse des Chatbot-iFrames reagiert. Im Backend habe ich eine Validierungslogik implementiert, die vor der Generierung der iFrame-URL prüft, ob der Nutzer sein monatliches Gesprächslimit bereits ausgeschöpft hat. Dies sollte verhindern, dass Nutzer ein leeres Chat-Fenster angezeigt bekommen.

**Deposit Cancellation & Pair Programming**

Ergänzend habe ich eine Backend-Aufgabe zur automatischen Stornierung auslaufender Zahlungen und Kautionen umgesetzt sowie regelmäßig Kolleginnen und Kollegen beim Pair Programming unterstützt und Code Reviews durchgeführt.

---

### 3.8 Betreuung und Grad der Selbstständigkeit

Die Zusammenarbeit im Team war von Beginn an von einem hohen Grad an Vertrauen und Eigenverantwortung geprägt. Nach einer initialen Einarbeitungsphase, in der ich die Codebase, die wichtigsten Prozesse und den CI/CD-Flow kennenlernte, arbeitete ich überwiegend selbstständig. Neue Aufgaben wurden in der Regel als Jira-Tickets beschrieben und umfassten ein breites Spektrum: von kleinen Fixes über Bugs, die zunächst eine eigenständige Untersuchung erforderten, bis hin zu neuen Features und technischen Recherchen. Auch beim reinen Abarbeiten von Tickets blieb mir Spielraum für eigene technische Entscheidungen. Darüber hinaus gab es alle zwei Wochen Sprint Plannings sowie anlassbezogene technische Diskussionen, wenn größere Themen oder teamübergreifende Vorhaben geplant wurden. In beiden Formaten konnte ich meine Ideen und Einschätzungen einbringen. Ergänzend dazu bot meine direkte Vorgesetzte, die Engineering Managerin, regelmäßige 1-on-1-Meetings an, in denen ich Lernfortschritte, persönliche Themen und Wünsche zur eigenen Weiterentwicklung besprechen konnte.

Das Team kommunizierte primär über Slack; für konkrete Fragen oder schnelle Abstimmungen waren kurze Zoom-Calls jederzeit möglich. Tägliche Stand-ups (Dailies) boten eine regelmäßige Gelegenheit, Blockaden anzusprechen und Feedback einzuholen. Bei manchen Aufgaben, habe Kollegen und ich die Methode Pair Programming verwendet um gemeinsam Entscheidungen zu treffen oder voneinander zu lernen. Vor dem Mergen von Code in den Hauptbranch war ein Code Review und ein User Acceptance Test durch ein Teammitglied zwingend erforderlich. Durch Code Reviews vor dem Mergen konnte ich viel konstruktives und lehrreiches Feedback erhalten, aus meinen Fehlern lernen und mir gute Praktiken aneignen.

---

## 4. Bezüge zwischen Studium und Praktikum

Da mein Praktikum im November 2023 begann und mein Studium an der HTW erst ab dem WS24/25 lief, verliefen Studium und Praxistätigkeit über weite Strecken parallel. Die meisten relevanten Module habe ich also nicht vor, sondern während des Praktikums absolviert. Das bedeutet, dass ich viele theoretischen Inhalte zeitgleich mit konkreten Anwendungsfällen in der Praxis erlebt habe – was den Lerneffekt in beide Richtungen verstärkt hat.

**Web Application Development** (B41, SS25) war das Modul mit dem direktesten Bezug. Das im Kurs vermittelte Verständnis von React, HTTP und dem Request-Response-Modell deckte sich mit meiner täglichen Arbeit im MeinPlus-Frontend und gab mir eine strukturierte theoretische Einordnung für Dinge, die ich bereits praktisch umgesetzt hatte. **Software Engineering 1 & 2** (B32 im WS24/25, B42 im SS25) haben mich mit Konzepten wie komponentenbasierter Architektur, Entwurfsmustern und Qualitätssicherung durch Tests vertraut gemacht – Prinzipien, die im Projektalltag an jeder Ecke sichtbar waren, etwa beim Aufbau der wiederverwendbaren Modal-Komponenten im MeinPlus-Frontend. Aus **Verteilte Systeme** (B43, SS25) kannte ich die theoretischen Grundlagen von REST-APIs und Client-Server-Architekturen, die mir beim Verständnis der Kommunikation zwischen Frontend und Backend geholfen haben.

**Programmierung 3** (B31, SS25) hatte Java als Sprache behandelt, was mir den Einstieg in das Spring-Boot-Backend erleichterte. Das Modul **Betriebssysteme und Netzwerke** (B23, WS24/25) gab mir ein Grundverständnis für HTTP und Netzwerkkommunikation, das beim Verständnis von Proxy-Endpoints und API-Authentication hilfreich war. Das Modul **Projektmanagement** (B46, SS25) vermittelte theoretische Konzepte wie Aufwandsschätzung und Projektphasen, die ich in der Praxis im Kontext von Jira und Sprint-Planung wiedererkannt habe – wenngleich die praktische Umsetzung im agilen Kontext deutlich stärker prozessgetrieben ist als in der Theorie dargestellt.

Umgekehrt hat die Praxis mein Studium bereichert: Ich habe konkrete Anwendungsfälle für theoretische Konzepte gesehen und ein Verständnis für Themen entwickelt, die im Studium wenig Raum finden – darunter A/B-Testing, Feature Flags, CI/CD-Pipelines, OpenAPI-Driven Development und die Realität agiler Softwareentwicklung in großen Teams.

---

## 5. Zusammenfassung und Ausblick

Die Zeit bei ImmoScout24 war für mich eine außerordentlich lehrreiche Erfahrung. Ich habe von einer verhältnismäßig einfachen Bugfix-Aufgabe am ersten Tag bis hin zur Implementierung von vollumfänglichen Aufgaben im Frontend und Backend. Damit habe ich eine deutliche Entwicklung in meiner technischen Kompetenz und meinem Selbstverständnis als Entwickler gemacht.

Besonders wertvoll war die Breite der Tätigkeiten: Ich habe nicht nur Frontend-Code geschrieben, sondern auch Backend-Endpoints entwickelt, mit Datenbanken gearbeitet, ein Admin Panel aufgebaut, A/B-Tests implementiert und coordinated Rollouts über mehrere Repositories hinweg durchgeführt. Diese Breite hat mir ein ganzheitliches Verständnis dafür gegeben, wie professionelle Softwareentwicklung in einem großen Unternehmen funktioniert – von der Ticket-Erfassung bis zum produktiven Deployment.

Das Team hat mich von Anfang an als vollwertiges Mitglied behandelt: Ich habe eigene technische Entscheidungen getroffen, meine Ergebnisse präsentiert und war für meine Aufgaben vollständig verantwortlich. Diese Eigenverantwortung hat mich schneller wachsen lassen, als es in einer stärker betreuten Umgebung möglich gewesen wäre.

Konkrete Kritikpunkte fallen mir ehrlich gesagt kaum ein – die Stelle bei ImmoScout24 war für mich als Werkstudenten eine nahezu optimale Lernumgebung. Wenn ich einen Wunsch formulieren müsste, wäre es ein gelegentlicher Einblick in andere Teams: Ein Sprint oder eine gemeinsame Session mit einem benachbarten Team hätte geholfen, das Gesamtprodukt und die übergreifende Systemarchitektur besser zu verstehen.

Meine Werkstudentenfähigkeit hat mir gezeigt, dass Fullstack-Webentwicklung in einem produktorientierten Umfeld eine Tätigkeit ist, auf der ich meine längerfristige Karriere weiter aufbauen kann. Die Kombination aus technischer Tiefe, produktstrategischem Denken und der direkten Sichtbarkeit des eigenen Beitrags im Produkt empfinde ich als sehr motivierend.
