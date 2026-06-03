---
type: doc
tags:
  - archived
  - immoscout
status: archived
source: notion-import
---
# General

## Phone Number

**Case:** Any billcheck, any state

- **Phone Number:** `+49 30 40365750`

# Billcheck v3

## Billchecklist Description

**Case:** User `!canStartNewBillcheck && isEntitled`

**Text:** "Als WohnenPlus-Mitglied kannst du einmal im Jahr kostenlos einen Nebenkosten-Check durchführen. Am **01.01.YYYY** kannst du deine Nebenkosten erneut prüfen."

**Requirements:**

- The date should be bold
- Adjust the year depending on when the utility bill check was created
- Show the adjusted info message in completed state as well

## Ticket Number Info Label (Bold)

**Case:** `state = BILLCHECK_V3_STATUS.PROCESSING`

- "Deine Auftragsnummer" is bold

![[image 4.png]]

---

# Legacy Billcheck

## Status Box Text - Billcheck State: Submitted

**Case:** `lastBillCheck?.status === LEGACY_BILLCHECK_STATUS.SUBMITTED && !!lastBillCheck?.minekoTicketId`

- **Text:** "Innerhalb von 5 Werktagen erhältst du das Ergebnis per E-Mail."
- "Deine Auftragsnummer" is bold
- Ticket ID displayed as a link

![[image 5.png]]

## Ticket Number Info (Bold)

**Case:** `lastBillCheck.status === LEGACY_BILLCHECK_STATUS.DONE`

- "Deine Auftragsnummer" is bold
- Ticket ID displayed as text (NOT as a link)

![[image 6.png]]