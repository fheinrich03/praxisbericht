---
type: doc
tags:
  - archived
  - immoscout
status: archived
source: notion-import
---
### Upload/ Create Document

- `DocumentsApiClient`
- normal Flow
- FS on → save in DH Hub → save returned ID in Rent Profile (if MyScout-ID exists. same entry)


### Comments

1. Get Documents
    - why check is FS is on? 
        - Are All Documents in Myscout DB (→ only portion of Documents saved in Document Hub as a copy atm?)
        - What if a Document only in Documents Hub DB but Feature Switch is off?
        - In other words: would we ever turn off the feature switch or just delete the old code, as soon as it is on?
    - Why is there a different flow for Get documents than Download Document?
    - 

### Idea

2. Add and enable Feature Switch
3. Copy all documents in MyScout DB to Document Hub DB (that are not already there)
4. turn off and clean up Feature Switch
