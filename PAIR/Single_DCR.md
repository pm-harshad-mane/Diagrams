```mermaid
sequenceDiagram

participant Adv as Advertiser
participant DCR
participant Pub as Publisher
participant DSP

DCR->>DCR: 1.a Generate Ks for Advertiser-Puublisher pair
DCR->>DCR: 1.b Generate Ka
DCR->>DCR: 1.c Generate Kp

Adv->>DCR: 2.a Upload raw Advertiser ID list (PII used as join keys) using TLS/SSL
DCR->>DCR: 2.b Canonicalizes Advertiser's PII, encrypts using Ks and Ka, and adds a field AdvPubID* to each row
Pub->>DCR: 2.c Upload raw Publisher ID list (PII used as join keys) using TLS/SSL
DCR->>DCR: 2.d Canonicalizes Publisher's PII, encrypts using Ks and Kp, and adds a field AdvPubID* to each row
DCR->>DCR: 2.e Encrypt Publisher list using Ka. Generating Publisher PAIR IDs.
DCR->>DCR: 2.f Encrypt Advertiser list using Kp. Generating Publisher PAIR IDs.

DCR->>DCR: 3.a Match PAIR IDs from the Advertiser and Publisher lists
DCR->>Pub: 3.b.1 Tabular list with two columns, (1) raw PII (e.g. email) and (2) KsKp encrypted IDs (Publisher identifiers).<br/>This list contains all the Publisher PII, not just matches.
DCR->>DSP:

```

AC->>PC: 2.c AC shares KsKa-encrypted Advertiser list w/ AdvPubID with PC
PC->>AC: 2.d PC shares KsKp-encrypted Publisher list w/ AdvPubID with AC
AC->>AC: 2.e AC encrypts list from 2.d using Ka
PC->>PC: 2.f PC encrypts list from 2.c using Kp
PC->>AC: 2.g PC shares a list that contains all AdvPubIDs and PAIR iDs with AC
AC->>PC: 2.h AC shares the whole PAIR ID list with PC ( i.e. list created in 2.e)

PC->>PC: 3.a PC matches PAIR IDs from step 2.f and 2.h
AC->>AC: 3.b AC matches PAIR IDs from step 2.e and 2.g
PC->>Pub: 3.c Tabular list with two columns, (1) raw PII (e.g. email and (2) KsKp encrypted IDs. This list contains all the Publisher PII not just matches.
AC->>AC: 3.d Decrypt PAIR IDs matched in 3.b by removing the Ka key to get KsKp iDs.
AC->>DSP: 3.e List of matches encrypted by KsKp (no PII)
