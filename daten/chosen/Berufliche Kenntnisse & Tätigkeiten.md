### 🏢 Station 1: ImmoScout
**Fokus: Feature Management, A/B-Testing & Frontend-Architektur**

* **A/B-Testing (Optimizely):**
  * Eigenständige Implementierung mehrerer Features in Form von A/B-Tests.
* **Feature Flags & Rollout-Strategien:**
  * Implementierung von Feature-Teilstücken hinter Feature Flags.
  * Management komplexer, gleichzeitiger Feature-Rollouts über mehrere Repositories hinweg (Beispiel: "Plus Rebranding").
  * Architektur-Vergleich: Frontend Feature Flags (Nutzung von `LocalStorage`) vs. Backend Feature Flags (dedizierte Backend-Tabelle) vs. Third-Party-Lösungen (Optimizely).
  * Durchführung von unsichtbaren Tests in Sandbox- und Produktionsumgebungen (via `LocalStorage`-Overrides), ohne dass Endnutzer dies bemerken.
* **Admin Panel Project (Dashboard-Implementierung):**
  * Entwicklung und Integration einer neuen Dashboard-Seite im internen Admin Panel.
  * Nutzung der *shadcn-ui* Component Registry zur Implementierung von Diagrammen und KPI-Komponenten.
  * Automatisierte API-Anbindung: Nutzung einer Swagger UI Library zur Generierung des API-Interfaces basierend auf der `openapi.yaml`.

---

### 🏢 Station 2: SRP
**Fokus: Internal Tooling, Package Management & Infrastruktur**

* **Angular Libraries & Private NPM Registry:**
  * Konzeption und Erstellung eines eigenen Templates für eine Angular Library (inkl. standardisierter Ordnerstruktur).
  * Bereitstellung der Libraries als NPM-Packages in einer privaten Registry.
* **Infrastruktur & Gitea:**
  * Eigenständige Installation und Setup von *Gitea*.
  * Konfiguration der privaten Package Registry direkt in Gitea.
* **Tooling & Workflows:**
  * Integration von *Storybook* zur Komponenten-Dokumentation.
  * Definition und Umsetzung der elementaren Entwicklungs-Workflows (`run dev`, `build`, `deploy`).

---

### 🏢 Station 3: ITONICS (Aktuelle Werkstudentenstelle)
**Fokus: Design Systems, Monorepo-Architektur & Quality Assurance**

Onboarding
* **Design System & Frontend:**
  * Aufbau, Pflege und Weiterentwicklung eines unternehmensweiten Design Systems.
  * Einsatz von *Headless UI Libraries* für flexible, ungestylte Basis-Komponenten.
  * Enge Verknüpfung von Design und Entwicklung durch *Storybook* und *Figma*.
* **Architektur & Infrastruktur:**
  * Arbeit in einer modernen Monorepo-Struktur.
  * Nutzung von *Angular Nx* für das Workspace-Management.
* **Release Management & Quality Assurance (CI/CD):**
  * Arbeit mit unterschiedlichen Release-Strategien (Continuous Delivery vs. geplante, reguläre Releases).
  * Anwendung diverser QA-Maßnahmen (Unit Tests, User Testing etc.).

* **Konkrete Aufgaben bisher (Onboarding & Operations):**
  * Setup eines neuen Macs für die lokale Entwicklung inkl. technischem Onboarding.
  * Bug-Investigation im Cloud-Umfeld: Analyse von Verbindungsabbrüchen bei AWS Webhooks (Connection closes nach 2 Stunden).
  * Konkrete Arbeit am Design System: Technisches Refactoring der Alert-Component. *(Hinweis: Tätigkeit läuft aktuell noch).*

---