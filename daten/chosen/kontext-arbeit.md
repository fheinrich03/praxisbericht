# Arbeitskontext für Praxisbericht

---

## Rahmendaten

| | |
|---|---|
| **Unternehmen** | Immobilien Scout GmbH (ImmoScout24) |
| **Adresse** | Invalidenstraße 65, 10557 Berlin |
| **Beschäftigungsform** | Werkstudent |
| **Team** | Builders Organisation / Team Honey Badgers |
| **Beginn** | 15.11.2023 |
| **Ende (vertraglich)** | 14.11.2024 |
| **Arbeitszeit** | max. 20 Std./Woche (Vorlesungszeit), max. 40 Std./Woche (Semesterferien) |
| **Vergütung** | 15,00 € brutto/Stunde |
| **Sprache** | Englisch (Arbeitssprache im Team) |

---

## Unternehmen

ImmoScout24 ist Deutschlands größte Immobilienplattform für Kauf, Miete und Verkauf von Immobilien. Das Kernprodukt ist die Immobiliensuche für Mieter, Käufer, Vermieter und Makler.

**Consumer-Subscriptions (MeinPlus-Bereich):**
- **MieterPlus / Mieter+** – für Wohnungssuchende: erhöhte Sichtbarkeit in Bewerbungen, SCHUFA-Bonitätscheck, Schlüsseldienst (AXA), Lagerraum-Service u.a.
- **WohnenPlus / Wohnen+** – für bestehende Mietverhältnisse: ähnliche Leistungen wie MieterPlus
- **KäuferPlus / Käufer+** – für Kaufinteressierte: Immobilienbewertungen, Marktdaten, SCHUFA-Check

---

## Aufgabenbereich & Rolle

Schwerpunkt: Fullstack-Entwicklung im MeinPlus-Ökosystem (Consumer Subscriptions). Hauptsächlich gearbeitet an der Übersicht der drei Plus-Subscriptions (MieterPlus, WohnenPlus, KäuferPlus) sowie dem dazugehörigen Admin Panel.

**Verantwortete Repositories:**
- `is24-mein-plus-benefits-frontend` – React Web App für den `/meinkonto/meinplus/` Bereich
- `is24-consumerbenefits` – Java Spring Boot Backend (hält Daten, Feature Switches, APIs)
- `is24-consumerbenefits-admin-frontend` – internes Admin Panel
- `is24-rent-profile-frontend` – Profil & Dokumente
- Weitere: `is24-property-hub-app`, `myscout-ui-dashboard`, `is24-quick-cart-frontend`

---

## Tech Stack

| Bereich | Technologien |
|---|---|
| **Frontend** | React, TypeScript, i18n |
| **Backend** | Java, Spring Boot, OpenAPI/Swagger |
| **Testing** | Playwright (E2E), Unit Tests |
| **CI/CD** | Jenkins |
| **A/B Testing** | Optimizely |
| **Marketing Automation** | Iterable (Embedded Messages) |
| **Design System** | Storybook, Figma |
| **Monitoring** | Datadog RUM |
| **Projektmanagement** | Jira, Confluence |
| **Kommunikation** | Slack, Zoom, Outlook |

---

## Tätigkeiten & Projekte

### Task 1: Onboarding & erste Bugfixes (Nov–Dez 2023)
Erste eigenständige Tickets nach dem Onboarding. Copy-Fixes und Bugfixes im WohnenPlus-Bereich: Nutzer sahen falsche Texte und Links (MP statt WP). Einstieg in Codebase, CI/CD-Flow (Jira → Git → Sandbox → Release), i18n-System.

**Lerneffekte:** Navigation in großen Codebases, CI/CD-Prozess, i18n/Mehrsprachigkeit

---

### Task 2: UI/Modal-Komponenten & Header-Refactoring (Dez 2023–Feb 2024)
Sticky Footer/CTA im Benefit Modal (CSS), Übernahme neuer Header & Footer-Komponenten aus dem Design-System, Browser-History-Handling beim Modal-Schließen.

**Lerneffekte:** Komponentengetriebene Architektur, Browser History API, Design-System-Integration

---

### Task 3: A/B Testing mit Optimizely (Jan–Feb 2024)
Eigenständige Implementierung eines A/B-Tests zur Platzierung eines WohnenPlus-Upsell-Elements im Merkzettel. Ablauf: Product/Data Team → Optimizely-Setup → Engineer implementiert ExperimentId im Frontend → Hook liefert aktive Variante.

**Lerneffekte:** A/B-Testing-Konzepte, Feature-Flag-Architektur, Optimizely

---

### Task 4: Solvency & SCHUFA-Integration (Mai–Jul 2024) — Größtes Feature
Fullstack-Feature: Integration der SCHUFA-/IS24-Solvency-Prüfung in den MeinPlus-Bereich.
- **Frontend:** SolvencyChoiceModal (Business-Logik: welche Solvency anzeigen), Identity-Sektion im Rent-Profile
- **Backend:** Neue REST-API-Endpoints (`/solvency/info`), Proxy-Endpoint für IS24-Solvency (wegen Auth)
- Logik: Hat User SCHUFA → SCHUFA-Modal; hat User IS24-Solvency → IS24-Modal; hat User beides → IS24-Modal

**Lerneffekte:** Erstes vollumfängliches FE+BE-Feature, Backend-Setup für lokale Entwicklung, Zusammenhänge zwischen Schichten

---

### Task 5: Admin Panel / Dashboard (Apr 2024–Aug 2025)
Entwicklung und iterative Erweiterung des internen Admin Panels. Feature-Switch-Verwaltung, KPI-Kacheln (MagentaTV-Voucher-Status, digitale Umzugsmeldungen). Einsatz von shadcn/ui-Komponenten und automatisch generiertem API-Interface aus `openapi.yaml`.

**Lerneffekte:** OpenAPI-Spec-Driven Development, Swagger UI, shadcn/ui

---

### Task 6: Feature Flags & Rollout-Strategien (Sep 2024–Mai 2025)
Wiederkehrendes Thema über viele Features: Features hinter Feature Flags umsetzen, koordinierter Rollout über mehrere Repositories, Cleanup nach Rollout.

**Architektur-Vergleich:**
- FE Feature Flags: `LocalStorage`-Overrides für Sandbox-Tests
- BE Feature Flags: Dedizierte Tabelle in `is24-consumerbenefits`-Datenbank
- Third-Party: Optimizely (für A/B-Tests)

**Beispiele:** ARAG/AXA Tile-Logik, WP Upsell Rollout, MagentaTV Rollout, Salutation Removal

**Lerneffekte:** Playwright E2E-Tests (Setup, Schreiben, Ausführen), Rollout-Strategien, Feature-Flag-Architektur

---

### Task 7: WohnenPlus Upsell-Experimente (Sep 2024–Jul 2025)
Mehrere Iterationen des WohnenPlus-Upsell-Modals als Feature und A/B-Test. Vollständiger Experiment-Lifecycle: Setup → Launch → Auswertung → Cleanup (Experiment-Code entfernen, Tests wieder aktivieren).

**Lerneffekte:** Vollständiger A/B-Test-Lifecycle, E2E-Tests für Experimente

---

### Task 8: MagentaTV-Partnerschaft (Feb–Aug 2025)
End-to-End-Integration einer Telekom-MagentaTV-Kooperation: Nutzer die MieterPlus kaufen erhalten einen MagentaTV-Rabattcode.
- **Frontend:** Neues Tile im MyPlus-Bereich, Modal mit Rabattcode-Anzeige (conditional: Subscription vorhanden oder nicht)
- **Backend:** Discount-Code-API-Endpoint
- **Admin Panel:** Bar Chart zur Voucher-Nutzung
- Rollout über Feature Switch, inkl. Tracking und Bugfixes

**Lerneffekte:** Vollständige Feature-Entwicklung FE + BE + Admin-Panel

---

### Task 9: Plus Rebranding (Apr–Jun 2025)
Koordinierter Rollout von „WohnenPlus → Wohnen+" (und analoge Umbenennung der anderen Produkte) über mehrere Repositories gleichzeitig (Dashboard, Quick-Cart, MeinPlus-FE, CB-Backend). Parallel: Salutation-Feld als optionales Feld.

**Lerneffekte:** Cross-Repository-Rollout, Feature-Switch-Koordination

---

### Task 10: KI-Chatbot-Integration (Ainavio/HeyImmo) (Apr–Jul 2025)
Integration des Ainavio-KI-Chatbots in den MeinPlus-Bereich:
- **Frontend:** Tracking, iFrame-Event-Listener
- **Backend:** Validierung von Lizenz- und Gesprächslimits vor iFrame-URL-Generierung

**Lerneffekte:** iFrame-Integration, Frontend User Tracking, KI-Chatbot-Einbindung

---

### Task 11: Iterable / Embedded Messages (Feb 2025)
Integration eines SCHUFA-Upsell-Banners im Property-Hub (Exposé-Seite) über Iterable (Marketing-Automation-Plattform). Engineer setzt Platzhalter im Frontend; Marketing-Team füllt Inhalt eigenständig.

**Lerneffekte:** Zusammenarbeit mit nicht-technischen Kollegen (Marketing), technische Recherche, Iterable-Plattform

---

### Task 12–13: Weitere Aufgaben
- **Salutation Removal Extended:** Entfernung des Anrede-Pflichtfelds im SCHUFA-Checkout über mehrere Repos und eine eigene Library (`is24-quick-cart-bonicheck-library`)
- **Deposit Cancellation:** Backend-Task: automatisches Stornieren von Zahlungen/Kautionen
- **Pair Programming / Code Reviews:** Regelmäßige Unterstützung von Teammates
- **Datadog RUM:** Monitoring-Integration

---

## Querschnittsthemen

| Thema | Beschreibung |
|---|---|
| **Feature Flags** | Drei Ebenen: LocalStorage (FE-Sandbox), DB-Tabelle (BE), Optimizely (A/B) |
| **A/B Testing** | Vollständiger Lifecycle: Konzept → Implementierung → Auswertung → Cleanup |
| **Fullstack** | FE (React/TypeScript) + BE (Java/Spring Boot) + Admin Panel |
| **CI/CD** | Jenkins, automatisierte Tests (Unit + E2E/Playwright) |
| **OpenAPI-Driven Development** | API-Spec in `openapi.yaml`, Code-Generierung für Interfaces |
| **Design System** | Storybook, shadcn/ui, Figma |
