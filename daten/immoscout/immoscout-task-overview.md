# Praxisbericht – Aufgaben & Tasks @ ImmoScout24 / Team Honey Badgers

> **Hinweis:** Tickets mit `Relevant: FALSE` sind zur Vollständigkeit aufgeführt, aber für den Bericht nur am Rande erwähnenswert (z.B. als Beleg für Unterstützungsaufgaben / Pair Programming).


## Produkt- & Tool-Kontext

### ImmoScout24
- Deutschlands größte Immobilienplattform (Kauf, Miete, Verkauf)
- Kernprodukt: Immobiliensuche für Mieter, Käufer und Vermieter/Makler
- **Consumer-Subscriptions (MeinPlus-Bereich):**
  - **MieterPlus / Mieter+** – für Mietende: erhöhte Sichtbarkeit in Bewerbungen, inkl. SCHUFA-Bonitätscheck, Schlüsseldienst-Notfall (AXA), Lagerraum-Service u.a.
  - **WohnenPlus / Wohnen+** – ähnlich MieterPlus, jedoch für bestehende Mietverhältnisse (kein Fokus auf Wohnungssuche)
  - **KäuferPlus / Käufer+** – für Kaufinteressierte: Immobilienbewertungen, Marktdaten, SCHUFA-Check
- **MeinPlus-Bereich** (`/meinkonto/meinplus/`): Portal, in dem Nutzer ihre aktive Subscription und enthaltene Services als Kacheln sehen und verwalten können
- **Bonitätscheck / SCHUFA-Auskunft:** Digitale Bonitätsprüfung für Wohnungsbewerbungen – instantanes SCHUFA-Zertifikat, 3 Monate gültig, nur vermieterrelevante Daten
- **IS24-Solvency:** ImmoScouts eigene Bonitätslösung (Alternative zur klassischen SCHUFA)
- **Merkzettel:** Gespeicherte Immobilien-Merkliste des Nutzers
- **Exposé:** Detailseite einer Immobilienanzeige

### Tools & Third-Party Services

#### Optimizely
- A/B-Testing- und Feature-Management-Plattform
- Experimente laufen über **Feature Flags**: Varianten werden per ExperimentId + VariantId im Code referenziert
- Ablauf: Data/Product Team legt Experiment an → Engineer speichert IDs im Frontend → Hook liefert aktive Variante → Komponente rendert entsprechend
- Ermöglicht Traffic-Aufteilung ohne Code-Deployment, mit eingebautem Stats Engine für Signifikanz

#### Iterable
- Marketing-Automation- und Customer-Engagement-Plattform
- Ermöglicht **Embedded Messages**: Inline-Banner/Cards in der Web-App, konfiguriert vom Marketing-Team
- Engineer setzt Platzhalter (Slots) im Frontend; Marketing füllt Inhalt eigenständig ohne Code-Änderung
- Wird u.a. für SCHUFA-Upsell-Banner im Exposé genutzt (Property Hub)

---

## Immoscout Relevant Kontext

### repositories
- `is24-mein-plus-benefits-frontend` 
	- Link: `meinplus/...` (/mieterplus /wohnenplus /kaeuferplus)
	- Card: Meinplus
	- Reac Web App
- `is24-consumerbenefits`
	- hält daten für `is24-mein-plus-benefits-frontend`
	- hält daten für `is24-consumerbenefits-admin-frontned`
	- hält feature switches
	- Java Spring Boot Projekt
- `is24-rent-profile-frontend`
	- Card: Profil & Dokumente

### Tech Stack & Tools
Jira - für issues
Storybook - für immoscouts Design System Components
Figma - Design System & Product Initiatives
Confluence - Documentation (Sprint notes, tech docs)
Slack - day-to-day communication
Microsoft, Outlook & Zoom - Meetings, E-Mails, Calender
Optimizely - A/B Testing
Jenkins: CI/CD run unit and e2e tests, build pipelines and build artifacts
Iterable 3rd Party Service - add placeholders in frontend repo, which Marketing team can decide to fill how they like

### Mein Aufgabenbereich
- Ich habe hauptsächlich an den übersicht für die 3 plus subscriptions gearbeitet 

#### Immoscout Subscrippions - Meinplus Benefits
- backend repo: is24-consumerbenefits
- frontend repo: mein-plus-benefits-frontend
- ImmoScout bietet 3 subscriptions mit verschiedenen services an:
	- käuferplus - KP
	- mieterplus - MP
	- wohnenplus - WP
- mieterplus und wohnenplus haben teilweise die gleichen services im paket enthalten
- im portal hat jede subscription einen tab. die nutzer können dort ihre aktive subscription und enthaltenen services als cards sehen. klick auf ein card öffnet ein modal mit informationen und ggf. aktionen die der nutzer machen kann

#### Dashboard - `meinkonto/dashboard/`
- für mich relevante Bereiche
	- Meinplus Card (click leitet weiter zu plus subscriptions oder zu warenkorb wenn noch keine subscription vorhanden)
	- Suchaufträge - Iterable issue
	- Profile & Dokumente
		- Mein Profil
		- Meine Bewerbermappe
		- IS24 solvency (Immoscouts eigene Bonitätsversicherung)
![[Pasted image 20260602184608.png]]

#### Admin Dashbaord (für meinplus benefits backend daten)
- frontend-repo: is24-consumerbenefits-admin-frontend
- backend-repo: is24-consumerbenefits


---

## Task 1: Erste Feature-Arbeiten & WohnenPlus Copy-Fixes ~ WohnenPlus Copy & UX
**Started:** 2023-11-15 | **Completed:** 2023-12-18
**Storypoints:** –

### Kurzbeschreibung
Erste eigenständige Tickets kurz nach dem Onboarding: Bugfixes und Copy-Anpassungen im WohnenPlus-Bereich. Nutzer mit reinem WP-Abo sahen teilweise falsche Links und Texte (MP statt WP). Außerdem Styling-Fix für die Lagerraum-Benefit-Kachel.

### Tickets
* HB-57: `Bug` WP users: change copy from "MP" to "WP" in storage + key service benefit IF user only has WP
	* Der fehler war, dass unabhängig von der subscription im schlüsseldienst modal text immer steht: "in mieterplus enthalten" auch wenn der nutzer nur WP subscription hat
	* Fix: check for current subscription. Based on subscription show different text
* HB-58: `Bug` WP users: change from "MP LP link" to "WP LP link"
	* Ich glaube hier war der fehler, dass der falsche link in den wohnenplus cards enthalten war. wenn ein nutzer noch keine subscription hat soll in den wohnenplus cards auch zu WP weitergeleitet werden. nicht zu MP
* HB-77: `Bug` MP+KP users: Fix styling and copy for storage room / Lagerraum benefit
	* Product Team hat einen neuen text für laggerraum model bereitgestellt und ich habe es einfach in den code übernommen

### Reflexion
- **Ausgangssituation:** Was war der Status quo, was hat nicht funktioniert?
- **Technische Umsetzung:** Welche Komponenten/Layer hast du angefasst?
	- das meinplus frontend verwendet i18n für translations. Es gibt eine de.ts und en.ts die die Text Blöcke für die meisten inhalte der website enthält
	- Reine Änderungen für Text muss man nur hier machen. einfach über strg-f in der de oder en datei suchen und ersetzen
	- In der Component kann man den text beziehen indem man einen translations hook aufruft und eine contentId übergibt
- **Herausforderung:** Was war nicht trivial?
	- Manchmal enthalten die textblöcke z.b. eine variable. 
		- Dann funktionierte die suche nach dem ganzen textblock evtl nicht -> muss man nur ausschnitte vom text suchen. 
		- Und man muss nachgucken wie eine variable übergeben wird. 
		- Es gab einen custom hook der übergebene tags oder sonderzeichen formatiert. z.b. `<b>` für bold etc.
- **Ergebnis/Lerneffekt:** Was hat sich verändert, was hast du mitgenommen?
	- wie websites multi language anbieten
	- wenn man an einem neuen ticket arbeitet: wie finde ich die dateien an denen ich arbeiten muss. D.h. verschiedene methoden ein großes softwareprojekt zu suchen: Ordner-struktur, Dateien suche, global search mit regex, Code lesen
- **Kontext im größeren Bild:** Warum war das Feature aus Produktsicht wichtig?
	- es waren nur bisschen angezeigter text der nicht ganz der business logik entspricht. keine große sache
	- diente vor allem um mich in das projekt einzuarbeiten. 
		- projekt kennenlernen (ordner strutkur, lokal ausführen)
		- CI/CD flow entdecken (Jira, Git, Test in sandbox, release)

---

## Task 2: UI/Modal Komponenten & Header-Refactoring ~ UI/Modal Komponenten
**Started:** 2023-12-06 | **Completed:** 2024-02-05
**Storypoints:** –

### Kurzbeschreibung
Arbeiten rund um die MeinPlus-Frontend-Architektur: Sticky Footer/CTA im Benefit Modal, Übernahme neuer Header & Footer-Komponenten aus dem Design-System, sowie Modal-Verhalten (Back-Button Exit). Erster Kontakt mit dem Component-getriebenen Aufbau des Frontends.

### Tickets
* HB-118: `Feature` FE: Make Benefit Modal footer and CTA sticky
	* Vorher: Meinplus Benefits Modal footer und CTA Button war am ende von content
	* Problem: Abhängig von Bildschirmgröße kann es sein, dass der CTA Text und Button bei Modal Öffnen nicht von Anfang an sichtbar sind und man erst scrollen muss um es zu sehen
	* Fix: simple CSS changes
* HB-148: `Feature` FE: Adopt new header & footer in mein-plus-benefits-frontend
	* Ich weiß nicht mehr genau was das für änderungen waren
* HB-174: `Bug` Mein-plus-fe: Allow for modal to be exited when clicking on back browser button
	* Vorher: Browser back button navigiert zur vorherigen seite
	* Erwartetes verhalten: 
		* Modal Open pushed einen neuen browser history eintrag. 
		* Modal Close oder browser back löscht den eintrag wieder
	* Fix: browser history eintrag pushen on Modal Open

### Reflexion
- **Ausgangssituation:** Was war der Status quo, was hat nicht funktioniert?
- **Technische Umsetzung:** Welche Komponenten/Layer hast du angefasst?
	- touched components: reuseable benefit modal component. changed only the css 
	- page component: add routerhistory entry on modal open
- **Herausforderung:** Was war nicht trivial?
	- 
- **Ergebnis/Lerneffekt:** Was hat sich verändert, was hast du mitgenommen?
	- Ich wusste vorher nicht wie der Browser seine history speichert bzw. wie man darauf zugreifen kann
	- bei guter component struktur und wiederverwendbaren components muss man änderungen ggf. nur an einer stelle machen
- **Kontext im größeren Bild:** Warum war das Feature aus Produktsicht wichtig?
	- User Experience
	- Uniform Website Layout (Header & Footer)
	- Den Weg zum CTA für den User Verkürzen -> User leichter zu Subscription konvertieren

---

## Task 3: A/B Testing & Experimente (Merkzettel WP Placement) ~ A/B Testing & Experimente
**Started:** 2024-01-31 | **Completed:** 2024-02-12
**Storypoints:** –

### Kurzbeschreibung
Eigenständige Implementierung eines A/B-Tests zur Platzierung eines WohnenPlus-Upsell-Elements im Merkzettel. Inklusive technischer Anforderungsanalyse und Implementierung hinter Feature Flag / Experiment-Setup.

### Tickets
* HB-167: `Experiment` a/b test for placement test in Merkzettel for WP
* HB-169: `Experiment` A/B Test Requirements (Merkzettel | TP WP for MP users | Web)
	* Merkzettel: Liste von Immobilien die der User gespeichert hat
	* In merkzettel sollte glaube ich ein wohnenplus upsell banner angezeigt werden

### Stichpunkte
* Ich erinnere mich hier nicht mehr an genaue details
* Wir verwenden Optimizely für A/B Testing. Ablauf:
	* Product Team mit Data Team denken sich einen Test aus
	* Data Team setzt einen A/B Test in Optimizely auf
	* Engineer speichert ExperimentId und experiment variants von Optimizely im Frontend Projekt
	* experimentId an experiment hook übergeben -> hook returnd active variant
	* show the component/ text/ ... for this variant
	* Via User tracking we know the difference in user behaviour for the experiments

### Reflexion
- **Ausgangssituation:** Was war der Status quo, was hat nicht funktioniert?
- **Technische Umsetzung:** Welche Komponenten/Layer hast du angefasst?
	- ich habe zugriff bekommen zur optimizely plattform
	- implementierung siehe stichpunkte
- **Herausforderung:** Was war nicht trivial?
- **Ergebnis/Lerneffekt:** Was hat sich verändert, was hast du mitgenommen?
	- Wie experiments funktionieren. woher weiß das frontend für welchen user er welche variante anzeigen soll
- **Kontext im größeren Bild:** Warum war das Feature aus Produktsicht wichtig?
	- Ziel von Produktseite: Wohnenplus subscription anbieten für mieterplus user


---

## Task 4: Solvency & SCHUFA Integration ~ Solvency & SCHUFA Integration
**Started:** 2024-05-22 | **Completed:** 2024-07-08
**Storypoints:** ~15+

### Kurzbeschreibung
Eines der größten eigenständig umgesetzten Features: Integration der SCHUFA-/Solvency-Prüfung ins MeinPlus-Bereich. Sowohl Frontend (SolvencyChoiceModal, Identity-Sektion im Rent-Profile, Proxy-Endpoints) als auch Backend (API-Anbindung an BoniCheck `/solvency/info`, is24solvency-Endpunkt). Spätere Erweiterung: SCHUFA statt ISBA anzeigen (HB-793).

### Tickets
* HB-372: `Feature` Monetize SCHUFA in myPlus area tile *(Parent Issue)*
	* myplus area tile is in the dashboard where all the tiles are
	* wenn man auf das tile klickt wird man weitergeleitet zu warenkorb für mieterplus
	* ich weiß nicht mehr genau aber entweder im warenkorb, also modal oder direkt im tile soll auch schufa angeboten werden
* HB-373: `Feature` Remove 3.) in "So funktioniert's" for all users
	* just remove a bullet point in "How it works" for one of the plus benefits, because this point is not up to date with business logik
* HB-385: `Feature` Create SolvencyChoiceModal in FE
	* create a new component that holds the business logik for whether to show the schufa solvency or is24 solvency
	* very simple component that just displays either either of the 2 modals
* HB-392: `Feature` Add bonicheck's /solvency/info API to BE
	* add a new rest api endpoint to backend
	* get the data from service and send it as a response
* HB-425: `Feature` find/create endpoint to get latest is24solvency
	* check if endpoint exists
	* or create endpoint to get altest is24solvency
	* goal: display latest is24solvency in Bonitätscheck modal
* HB-426: `Feature` update logic of displaying modals according to which solvency documents the user has already
	* if user has schufa -> show schufa modal
	* if user has is24solvency -> show is24 solvency modal
	* if user has both -> show is24 solvency modal, because it shows both solvencies
* HB-429: `Feature` add getIs24solvency proxy endpoint to BE
	* mp-frontend kommuniziert mit cb-backend
	* Endpoint für is24-solvency kommt von einem anderen backend
	* wegen authentication muss anfrage von cb-be kommen
	* deshalb proxy endpoint in cb-be
* HB-431: `Feature` add getIs24solvency endpoint to FE and consume it in solvencychoice.tsx
	* preparation for other solvency tickets
* HB-453: `Feature` FE: Verify identity section – Rent-profile Page
	* HB-454: `Feature` FE: Modal that shows the options to get verified
	* HB-471: `Feature` FE: Add warning message before deleting Identitätsnachweis
	* Das feature sich über immoscout verifizieren zu lassen, damit vermieter usw. wissen dass die daten (adresse, handynummer usw.) echt sind
	* dafür gibt es ein checkmark beim profil icon
	* und für das feature wurd ein neues modal erstellt um sich verifizieren zu lassen
* HB-793: `Feature` [MyPlus | Solvency Check] Show SCHUFA instead of ISBA
	* show schufa modal instead of IS24 bonitätsauskunft modal 

### Zugehörige PRs
* `is24-mein-plus-benefits-frontend/pull/1336` – HB-793: show Schufa instead of ISBA

### Stichpunkte
* Stand vorher
	* in Rent profile repository gab es bereits ein modal für is24-solvency
	* in meinplus benefits hat man 1 schufa im jahr included in mieterplus und wohnenplus
	* wenn man keine subscription hat oder frei-schufa aufgebrahct -> kann man kaufen
* Ziele dieses Milestones
	* Phase 1: monetize schufa - als upsell modal
	* für die nutzer die noch die "alte version" von mieter/wohnenplus haben: beibehalten, dass sie ihre inkludierte 1 kostenlose schufa bekommen können. (Logik in SolvencyChoice Modal)
	* migrieren dazu, dass die is24-solvency anzubieten

### Reflexion
- **Ausgangssituation:** Was war der Status quo, was hat nicht funktioniert?
- **Technische Umsetzung:** Welche Komponenten/Layer hast du angefasst?
	- BE solvency Rest controller, solvencyservice
	- FE: solvencychoice modal
- **Herausforderung:** Was war nicht trivial?
- **Ergebnis/Lerneffekt:** Was hat sich verändert, was hast du mitgenommen?
	- das war die erste aufgabe an der ich gearbeitet habe, wo änderungen im BE und FE nötigen waren. also quasi eine vollumfängliche aufgabe. ich fand es sehr gut weil man so ein besseres gefühl dafür bekommt, wie alles zusamenhängt
	- kennenlernen des backendend projekts. aufsetzen für lokale entwicklung
- **Kontext im größeren Bild:** Warum war das Feature aus Produktsicht wichtig?
	- keine produkt erweiterung, sondern einfach visibility für die produkte erhöhen. produkte an verschiedenen stellen anbieten. konvertierungszahlen erhöhen

---

## Task 5: Admin Panel / Dashboard ~ Admin Dashboard
**Started:** 2024-04-24 | **Completed:** 2025-08-05
**Storypoints:** ~13+

### Kurzbeschreibung
Entwicklung und iterative Erweiterung des internen Admin Panels (`is24-consumerbenefits-admin-frontend`). Feature-Switch-Verwaltung, und zuletzt KPI-Kacheln für MagentaTV-Voucher und digitale Umzugsmeldungen. Nutzung von shadcn/ui Komponenten und automatisch generiertem API-Interface (Swagger/openapi.yaml).

### Tickets
* HB-922: `Feature` CB: expose client endpoint for feature switches
* HB-670: `Feature` Admin panel FE: add feature switch
	* add a new page to manage feature switches coming from is24-consumerbenefits
	* these feature switches are used e.g. to release features in different projects at the same time
	* and allows easy rollback if feature is not working
* HB-965: `Feature` [Admin Panel] Add digital change of address numbers in the dashboard
	* add a new endpoint in consumerbenefits
	* retrieve the data and display using shadcn/ui graphs
* HB-966: `Feature` [Admin Panel] Add MagentaTV vouchers number to dashboard
	* add a new endpoint in consumerbenefits
	* retrieve the data and display using shadcn/ui graphs

### Zugehörige PRs
* `is24-consumerbenefits-admin-frontend/pull/39` – HB-670: add feature switch page (sfmc)
* `is24-mein-plus-benefits-frontend/pull/1452` – HB-790: remove success step from billcheck flow
* `is24-consumerbenefits-admin-frontend/pull/48` – HB-965: add address change tracking
* `is24-consumerbenefits/pull/805` – HB-965: add address change tracking (BE)
* `is24-consumerbenefits-admin-frontend/pull/49` – HB-966: add magenta dashboard
* `is24-consumerbenefits/pull/849` – HB-966: add magenta dashboard (BE)

### Reflexion
- **Ausgangssituation:** Was war der Status quo, was hat nicht funktioniert?
- **Technische Umsetzung:** Welche Komponenten/Layer hast du angefasst?
	- consumerbenefits api endpoints und types sind definiert in openapi.yml
	- wir haben eine library benutzt, aus openapi.yml api interfaces generiert für alle endpoints. 
	- diese interfaces muss man dann nur noch implementieren und für die response die daten beziehen aus service bzw. aus datenbank
	- programmiersprache java, framework springboot
- **Herausforderung:** Was war nicht trivial?
	- 
- **Ergebnis/Lerneffekt:** Was hat sich verändert, was hast du mitgenommen?
	- mir hat die einheitliche definition in einer openapi.yml gefallen
	- daraus kann dann der nötige code und java klassen/ records generiert werden
	- man kann damit einfach swagger ui documentation bauen
	- cleaner ansatz, aber man muss bisschen gucken wie man die endpoints definieren muss und den code generieren lassen mit build command
- **Kontext im größeren Bild:** Warum war das Feature aus Produktsicht wichtig?
	- einfaches feature management: rollout (zeitgleich und projektübergreifend) und rollback
		- wurde später auch verwendet z.b. für salutation removal in mehreren projekten gleichzeitung und plus rebranding
	- admin dashboard mit übersicht der wichtigsten performance zahlen
		- wurden regelmäßig von produktteam angefragt -> value

---

## Task 6: Feature Flags & Rollout-Strategien ~ Feature Flags & Rollout
**Started:** 2024-09-03 | **Completed:** 2025-05-28
**Storypoints:** ~12+

### Kurzbeschreibung
Wiederkehrendes Thema über viele Features hinweg: Implementierung von Features hinter Feature Flags, koordinierter Rollout über mehrere Repositories, Cleanup nach Rollout. Beispiele: ARAG/AXA Tile-Logik, WP Upsell Rollout, MagentaTV Rollout, Feature-Switch-API für den Client. Architektur-Vergleich: FE-seitige Flags (LocalStorage) vs. BE-seitige Flags (DB-Tabelle) vs. Third-Party (Optimizely).

### Tickets
* HB-545: `Feature` FE: add tile logic based on contract/entitlement (if AXA or ARAG)
	* AXA and ARAG are two different insurance services. based on your subscription you may have access to either or both of those services.
	* based on your entitlements -> show the correct tiles in meinplus area
	* components already exist. entitlements endpoint already exists. just need to add the logik to show only the correct tiles
		* add entitlements hook in tile-display-configuration. add condition when to display each tile
* HB-572: `Rollout/Cleanup` MeinPlus FE: Clean up old WohnenPlus upsell modal code
	* delete the upsell component
	* delete related subcomponents/ hooks/ translation texts that become unused
	* check if the app and tests still run
* HB-675: `Feature` CB: hide ARAG from KP
	* hide ARAG insurance tile for KP users
* HB-676: `Feature` CB: hide Dekra from KP
	* hide Dekra tile for KP users
* HB-704: `Rollout/Cleanup` Roll out + clean up WP upsell in Profile experiment
	* repository: rent-profile
	* context: we did an optimizely experiment as A/B test to see if showing WP Upsell in user profile page would increase WP conversions. Experiment was successfull, so we want to clean up the experiment code and keep the upsell in profile.
* HB-828: `Rollout/Cleanup` Uncomment and adjust e2e tests – WP V2 Release Cleanup
	* during the Wohnenplus V2 migration some e2e tests stopped working and therefore were commented out temporarily
	* we want to bring back the e2e tests and adjust to the new functionality to assure the functionality
* HB-894: `Rollout/Cleanup` [MeinPlus FE] Rollout + Cleanup Feature Switch - for MagentaTV Discount Codes
	* context: 
		* immoscout did a collaboration with telekom magentaTV: when users buys mieterplus they get discount code for magentaTV
		* we had a featureswitch coming from is24-consumerbenefits-backend for safe rollout the magentaTV feature
		* after the featureswitch is enabled and no problems after 2 days monitoring -> feature can be rolled out and remove featureswitch
* HB-921: `Bug` CB: reset WP upsell when user cancels subscription
	* before: users which have no WP subscription see the WP upsell modal regularly
	* bug: when you buy WP and then cancel subscription you don't see the WP Upsell anymore
	* change: need to adjust the business logik. When you cancel reset the show upsell counter. change in subscription service in consumberbenefits backend 

### Zugehörige PRs
* `is24-mein-plus-benefits-frontend/pull/1316` – HB-676: hide dekra services
* `is24-rent-profile-frontend/pull/1296` – HB-704: Roll out WP upsell experiment
* `is24-mein-plus-benefits-frontend/pull/1338` – HB-828: cleanup e2e tests
* `is24-mein-plus-benefits-frontend/pull/1387` – HB-894: rollout magentatv
* `is24-consumerbenefits/pull/740` – HB-921: bugfix reset living offer when user buys MP
* `is24-consumerbenefits/pull/747` – HB-921: add transactional annotation
* `is24-consumerbenefits/pull/738` – HB-922: expose feature switches to client

### Reflexion
- **Ausgangssituation:** Was war der Status quo, was hat nicht funktioniert?
	- Es gab verschiedene Aufgaben die mithilfe von eines feature switches sicher ausgerollt werden sollten.
- **Technische Umsetzung:** Welche Komponenten/Layer hast du angefasst?
	- consumerbenefits backend subscription service
	- meinplus benefits frontend: tile display configuration logik
- **Herausforderung:** Was war nicht trivial?
	- what is a feature switch. Where does the feature switch value come from? For Experiments we use optimizely + hook. For everything else we use feature switch managed in consumerbenefits-backend
- **Ergebnis/Lerneffekt:** Was hat sich verändert, was hast du mitgenommen?
	* working with playwright, e2e test setup, run & write e2e tests
- **Kontext im größeren Bild:** Warum war das Feature aus Produktsicht wichtig?

---

## Task 7: WohnenPlus CVR & Upsell-Experimente ~ A/B Testing & Experimente / WohnenPlus CVR
**Started:** 2024-09-11 | **Completed:** 2025-07-28
**Storypoints:** ~10+

### Kurzbeschreibung
Mehrere Iterationen des WohnenPlus-Upsell-Modals, zunächst als Feature (Upsell Modal für MP-Nutzer), dann als A/B-Test (Experiment Setup, Rent Profile Upsell, WP Upsell V2). Inklusive End-to-End-Tests für das Modal. Zeigt den kompletten Lifecycle eines Experiments: Setup → Launch → Auswertung → Cleanup.

### Tickets
* HB-548: `Feature` FE: Mein-plus | WP Upsell Modal for MP
* HB-726: `Experiment` FE: [PR1] Experiment setup
* HB-759: `Experiment` [Rent Profile] Experiment to Upsell Modal for Basic Users to MieterPlus Users
* HB-792: `Bug` [WP Upsell] Adjust Typo in Upsell Modal
* HB-817: `Bug` [WP Upsell] Adjust Plus Signs in Mobile View
* HB-829: `Bug` [MieterPlus] Success Modal and Upsell Modal Overlap
* HB-856: `Feature` FE e2e Tests for WP Upsell Modal
* HB-1016: `Experiment` [Experiment] Adjustments to WP Upsell – V2

### Zugehörige PRs
* `is24-rent-profile-frontend/pull/1312` – HB-726: [PR1] setup MP Upsell experiment
* `is24-rent-profile-frontend/pull/1348` – HB-759: Mieter Upsell Experiment
* `is24-mein-plus-benefits-frontend/pull/1332` – HB-792: fix price in WP Upsell Modal
* `is24-mein-plus-benefits-frontend/pull/1433` – HB-817: Fix WP upsell plus icon positioning
* `is24-mein-plus-benefits-frontend/pull/1437` – HB-829: Bugfix welcome modals
* `is24-mein-plus-benefits-frontend/pull/1340` – HB-856: Add e2e tests for WP Upsell
* `is24-mein-plus-benefits-frontend/pull/1499` – HB-1016: Feat new wp upsell iteration 2
* `is24-mein-plus-benefits-frontend/pull/1504` – HB-1016: remove description in original WP Upsell

### Reflexion
- **Ausgangssituation:** Was war der Status quo, was hat nicht funktioniert?
- **Technische Umsetzung:** Welche Komponenten/Layer hast du angefasst?
- **Herausforderung:** Was war nicht trivial?
- **Ergebnis/Lerneffekt:** Was hat sich verändert, was hast du mitgenommen?
- **Kontext im größeren Bild:** Warum war das Feature aus Produktsicht wichtig?

---

## Task 8: MagentaTV Integration ~ MagentaTV Integration
**Started:** 2025-02-26 | **Completed:** 2025-08-05
**Storypoints:** ~8+

### Kurzbeschreibung
End-to-end Integration der MagentaTV-Partnerschaft ins MeinPlus-Ökosystem: FE-Tile im MyPlus-Bereich, Modal mit Rabattcode-Anzeige, Backend-API zur Discount-Code-Abfrage, Dashboard-Erweiterung. Rollout über Feature Switch, inklusive Tracking und Bugfixes. Gutes Beispiel für eine vollständige Feature-Entwicklung über FE + BE + Admin-Panel.

context: immoscout did a collaboration with telekom magentaTV: when users buys mieterplus they get discount code for magentaTV
### Tickets
* HB-774: `Feature` [MyPlus Area] Include MagentaTV Tile in MyPlus Area
	* Add a new tile in myplus area for magentaTV collaboration
* HB-822: `Feature` [MeinPlus FE] MagentaTV Modal
	* create a new component with the reusable modal component
	* add text blocks do translations files and add for magenta TV banner to images folder
	* modal logic: if you buy mieterplus ->  you will see your discount code else -> show CTA to buy mieterplus
* HB-820: `Feature` [CB] Discount Code APIs
	* add endpoint to get discount code for user
* HB-894: `Rollout/Cleanup` [MeinPlus FE] Rollout + Cleanup Feature Switch *(auch Task 6)*
	* context: 
		* we had a featureswitch coming from is24-consumerbenefits-backend for safe rollout the magentaTV feature
		* after the featureswitch is enabled and no problems after 2 days monitoring -> feature can be rolled out and remove featureswitch
* HB-898: `Feature` [MagentaTV] Include Starred Info Message
	* very small change: just add a small info message für the magentaTV Modal. just one textblock
* HB-903: `Bug` [MagentaTV] Make "Übersicht" more visible in Mobile View
	* small bug: in mobile one of the tabs in magentaTV modal was a bit to big and therfore overlapping the modal container
* HB-966: `Feature` [Admin Panel] Add MagentaTV vouchers number to admin dashboard *(auch Task 5)*
	* For Product team it was valuable to know how much of the vouchers were already used
	* ->  add a bar chart to show the status of magenta discount codes (how much are assigned to users)
	* requires new endpoint in consumerbenefits backend and a new page/ graph to dispaly in admin-frontend

### Zugehörige PRs
* `is24-consumerbenefits/pull/697` – HB-820: add magenta discount api
* `is24-consumerbenefits/pull/702` – HB-820: get magenta discount only for plus user
* `is24-mein-plus-benefits-frontend/pull/1348` – HB-822: Add Magenta TV Modal
* `is24-mein-plus-benefits-frontend/pull/1361` – HB-822: magenta tv ui fixes
* `is24-mein-plus-benefits-frontend/pull/1390` – HB-881: Magenta TV add tracking *(kein Jira-Ticket exportiert)*
* `is24-mein-plus-benefits-frontend/pull/1395` – HB-881: fix magenta tracking terms value
* `is24-mein-plus-benefits-frontend/pull/1396` – HB-898: add starred info message
* `is24-mein-plus-benefits-frontend/pull/1407` – HB-903: Fix Magenta TV Übersicht

### Reflexion
- **Ausgangssituation:** Was war der Status quo, was hat nicht funktioniert?
	- context: immoscout did a collaboration with telekom magentaTV: when users buys mieterplus they get discount code for magentaTV
- **Technische Umsetzung:** Welche Komponenten/Layer hast du angefasst?
- **Herausforderung:** Was war nicht trivial?
- **Ergebnis/Lerneffekt:** Was hat sich verändert, was hast du mitgenommen?
- **Kontext im größeren Bild:** Warum war das Feature aus Produktsicht wichtig?

---

## Task 9: Plus Rebranding (WohnenPlus → Wohnen+) ~ Plus Rebranding
**Started:** 2025-04-17 | **Completed:** 2025-06-27
**Storypoints:** ~7

### Kurzbeschreibung
Koordinierter Rollout des "Plus Rebranding" über mehrere Repositories gleichzeitig: MyScout Dashboard, Quick-Cart-Frontend, MeinPlus-Frontend, ConsumerBenefits-Backend. Zusätzlich: Anrede/Salutation als optionales Feld in MeinPlus-FE und CB-Backend – Feature Flag-basierter Rollout, inklusive Cleanup in mehreren Repos.

### Tickets
* HB-871: `Feature` [MeinPlus FE] Remove Salutation Fields under FS
* HB-872: `Feature` [CB BE] Make salutation optional in API calls
* HB-880: `Feature` [Mein Profil] Renaming of Plus Products in MyScout Dashboard
* HB-958: `Feature` [QC/Checkout] Rebrand WP in QC and Checkout

### Zugehörige PRs
* `is24-consumerbenefits/pull/711` – HB-872: Make salutation optional in API calls
* `is24-consumerbenefits/pull/778` – HB-871: add remove-salutation-feature (BE)
* `is24-mein-plus-benefits-frontend/pull/1464` – HB-871: Make Salutation Optional in Meinplus (FE)
* `myscout-ui-dashboard/pull/106` – HB-880: Rebrand Plus Products
* `myscout-ui-dashboard/pull/107` – HB-880: Rebrand WP to Wohnen+
* `myscout-ui-dashboard/pull/108` – HB-880: remove plus indicator in Wohnen+ Upsell
* `is24-quick-cart-frontend/pull/2746` – HB-958: rebrand WP to Wohnen+

### Reflexion
- **Ausgangssituation:** Was war der Status quo, was hat nicht funktioniert?
	- salutation
		- Vorher: salutation war ein pflichtfeld in forms. man konnte herr/frau oder divers angeben
		- änderung: man soll es garnicht mehr angeben müssen
	- plus rebranding: statt wohnenplus soll stehen wohnen+ und dasselbe für die anderen (statt plus als wort als + icon)
- **Technische Umsetzung:** Welche Komponenten/Layer hast du angefasst?
	- ein feature switch für plus rebranding und consumed in mehreren repositories
	- ich abe änderung implementiert in rent-profile projekt
		- based on feature switch show wohnen+, mieter+, käufer+
- **Herausforderung:** Was war nicht trivial?
- **Ergebnis/Lerneffekt:** Was hat sich verändert, was hast du mitgenommen?
- **Kontext im größeren Bild:** Warum war das Feature aus Produktsicht wichtig?

---

## Task 10: Ainavio / AI Chatbot Integration ~ Ainavio / AI Chatbot Integration
**Started:** 2025-04-24 | **Completed:** 2025-07-23
**Storypoints:** ~7

### Kurzbeschreibung
Integration des Ainavio-KI-Chatbots (HeyImmo) in den MeinPlus-Bereich: Frontend-Tracking, iFrame-Event-Listener, Backend-Validierung von Lizenz- und Gesprächslimits vor iFrame-URL-Generierung. Späterer Bugfix bei überlappenden UI-Elementen auf Mobile.

### Tickets
* HB-892: `Feature` [Ainavio] Frontend Tracking of Ainavio Chatbot – MeinPlus
* HB-899: `Feature` BE > check conversation and license limits before creating iframe URL
	* this should avoid opening an empty chat window, when the user already used up his conversation limit for this month
* HB-900: `Feature` FE > add iframe event listener to modal
* HB-1018: `Bug` Ainavio Button Overlapping Success Modal on Mobile After Plus Unlock

### Zugehörige PRs
* `is24-mein-plus-benefits-frontend/pull/1375` – HB-878: rollout ainavio feature *(kein Jira-Ticket exportiert)*
* `is24-mein-plus-benefits-frontend/pull/1391` – HB-892: Ainavio Add Tracking
* `is24-consumerbenefits/pull/720` – HB-899: ainavio usage tracking (BE)
* `is24-mein-plus-benefits-frontend/pull/1409` – HB-900: add event listener for valid iframe license
* `is24-mein-plus-benefits-frontend/pull/1496` – HB-1018: Bugfix Fix overlapping ainavio chat button

### Reflexion
- **Ausgangssituation:** Was war der Status quo, was hat nicht funktioniert?
- **Technische Umsetzung:** Welche Komponenten/Layer hast du angefasst?
- **Herausforderung:** Was war nicht trivial?
- **Ergebnis/Lerneffekt:** Was hat sich verändert, was hast du mitgenommen?
	- Was ist ein Iframe
	- wie kann man ein chatbot in eine app einbinden
	- wie funktioniert frontend user tracking
- **Kontext im größeren Bild:** Warum war das Feature aus Produktsicht wichtig?

---

## Task 11: Iterable / SCHUFA Embedded Message ~ Iterable Integration
**Started:** 2025-02-03 | **Completed:** 2025-02-18
**Storypoints:** ~3+

### Kurzbeschreibung
Integration eines eingebetteten SCHUFA-Banners im Property-Hub (Exposé-Seite) als Upsell-Einstiegspunkt. Dazu: Auth-Token-Endpunkt für den Iterable-Email-Service im Property-Hub-Backend. Arbeit über mehrere Repositories (property-hub-app, property-hub-service).

### Tickets
* HB-739: `Investigation` Investigate Iterable*(Sub-task zu HB-714)*
	* Research Ticket. 
		* Was ist Iterable, Wie funktioniert es.
		* Hat ein anderes Team das schonmal benutzt
		* Welche Daten/ Informationen muss ich dem Marketing Team geben und was müssen sie für mich bereitstellen
		* How was iterable used in a different project by a different team - see as a example
* HB-736: `Investigation` Investigate how it (iterable) was ipmlemented in Messenger app
* HB-745: `Feature` BE: Implement auth token endpoint *(Sub-task zu HB-732)*
	* necessary to authenticate towards iterable plattform

### Zugehörige PRs
* `is24-property-hub-service/pull/1017` – HB-745: create auth endpoint for iterable (closed/replaced)
* `is24-property-hub-app/pull/1945` – HB-732: Add Schufa Embedded Message

### Reflexion
- **Ausgangssituation:** Was war der Status quo, was hat nicht funktioniert?
- **Technische Umsetzung:** Welche Komponenten/Layer hast du angefasst?
- **Herausforderung:** Was war nicht trivial?
- **Ergebnis/Lerneffekt:** Was hat sich verändert, was hast du mitgenommen?
	- Bei dieser Aufgabe habe ich zum ersten mal mit dem Marketing Team zusammengearbeitet. Und dadurch dass sie weiter entfernt von den Entwickler Themen sind muss ich hier klar kommunizieren was ich vom Marketing Team für Informationen brauch um so ein Placeholder zu implementieren.
	- Also kurz: Kommunikation mit nicht-technischen Kollegen. Eigene Recherche im Internet und Austausch mit anderen Teams
- **Kontext im größeren Bild:** Warum war das Feature aus Produktsicht wichtig?
	- Marketing Team wollte einen Placeholder in der Favoriten Seite haben für Wohnungen die Mann sich speichert. Das Marketing Team kann dann über die Iterable Plattform beliebigen Content in diesen Placeholder einsetzen ohne Codeänderungen.

---

## Task 12: Salutation Removal – Bonicheck & Checkout ~ Salutation Removal Extended
**Started:** 2025-06-24 | **Completed:** 2025-07-30
**Storypoints:** ~3+

### Kurzbeschreibung
Erweiterung des Salutation-Rollouts (siehe Task 9) auf den eigentlichen SCHUFA-Checkout-Flow: Entfernung des Anrede-Pflichtfelds in bonicheck-frontend, IS24-media-BoniCheck-Web und Quick-Cart. Koordination über mehrere Repos und eine eigene Library (`is24-quick-cart-bonicheck-library`).

### Tickets
* HB-946: `Feature` *(Salutation aus SCHUFA-Formular entfernen – Ticket nicht im CSV-Export enthalten)*

### Zugehörige PRs
* `IS24-media-BoniCheck-Web/pull/2337` – HB-946: Remove Salutation
* `is24-bonicheck-frontend/pull/554` – Feat 946: Remove Salutation from Schufa Form
* `is24-quick-cart-bonicheck-library/pull/3` – no validation for salutation field
* `is24-quick-cart-bonicheck-library/pull/4` – update version package.json
* `is24-quick-cart-frontend/pull/2827` – HB-946: no validation for salutation field

### Reflexion
- **Ausgangssituation:** Was war der Status quo, was hat nicht funktioniert?
- **Technische Umsetzung:** Welche Komponenten/Layer hast du angefasst?
- **Herausforderung:** Was war nicht trivial?
- **Ergebnis/Lerneffekt:** Was hat sich verändert, was hast du mitgenommen?
- **Kontext im größeren Bild:** Warum war das Feature aus Produktsicht wichtig?

---

## Task 13: Deposit Cancellation ~ Deposit & Payments
**Started:** 2025-03-12 | **Completed:** 2025-04-15
**Storypoints:** ~3

### Kurzbeschreibung
Backend-Task: Automatisches Stornieren von Zahlungen und Kautionen für alle Nutzer über zwei iterative PRs (zunächst "pending/ending tomorrow", dann Erweiterung auf 3 Tage im Voraus).

### Tickets
* HB-787: `Task` [Deposit] Cancel Payments and Deposits for All Users

### Zugehörige PRs
* `is24-consumerbenefits/pull/694` – HB-787: cancel deposits that end tomorrow or are pending
* `is24-consumerbenefits/pull/699` – HB-787: cancel deposit 3 days in advance instead of 1

### Reflexion
- **Ausgangssituation:** Was war der Status quo, was hat nicht funktioniert?
- **Technische Umsetzung:** Welche Komponenten/Layer hast du angefasst?
- **Herausforderung:** Was war nicht trivial?
- **Ergebnis/Lerneffekt:** Was hat sich verändert, was hast du mitgenommen?
- **Kontext im größeren Bild:** Warum war das Feature aus Produktsicht wichtig?

---

## Allgemein: Pair Programming / Support & kleinere Aufgaben
**Zeitraum:** 2024-01-08 – 2025-03-27

### Kurzbeschreibung
Neben den oben beschriebenen Hauptaufgaben wurden regelmäßig Kolleginnen und Kollegen bei ihren Tickets unterstützt (Pair Programming, Code Reviews, Investigations). Diese Tickets sind in der Tabelle als `Support` oder `Investigation` gekennzeichnet.

### Beispiel-Tickets
* HB-138: `Support` support (Sub-task)
* HB-170: `Support` Implementation (Sub-task, im Kontext einer fremden Task)
* HB-171: `Support` Support (Sub-task)
* HB-197: `Investigation` Investigate
* HB-300: `Infrastructure` MeinPlus > Implement Datadog RUM
* HB-757: `Investigation` Setup project and understand codebase

### Reflexion
- **Ausgangssituation:** Was war der Status quo, was hat nicht funktioniert?
- **Technische Umsetzung:** Welche Komponenten/Layer hast du angefasst?
- **Herausforderung:** Was war nicht trivial?
- **Ergebnis/Lerneffekt:** Was hat sich verändert, was hast du mitgenommen?
- **Kontext im größeren Bild:** Warum war das Feature aus Produktsicht wichtig?

---

*Generiert aus: Team Honey Badgers – Felix's Tickets (Jira-Export) + felix_prs_sanitized.json*
