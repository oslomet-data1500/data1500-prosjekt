# Brukerscenarioer (kapittel 4 fra fra OPPGAVE.md)

## 4. Brukerscenarioer (NS 4102)

Følgende scenarioer illustrerer hvordan en liten konsulentbedrift, **DATA1500 Konsult AS**, tar i bruk dobbelt bokholderi fra oppstart, med kontoer fra Norsk Standard Kontoplan (NS 4102). Alle kontonumre er i henhold til NS 4102, og alle beløp er i norske kroner (NOK) med mindre annet er angitt. Scenarioene er utformet slik at de dekker alle åtte kontoklassene og de viktigste funksjonene i den norske datamodellen.

---

### Scenario 1: Stiftelse av selskapet — innskudd av aksjekapital

**Hendelse:** Eieren skyter inn 200 000 kr i aksjekapital ved stiftelse av DATA1500 Konsult AS. Pengene settes inn på bedriftens bankkonto.

| Kontonr | Kontonavn | Kontoklasse | Debet | Kredit |
|---|---|---|---|---|
| 1920 | Bankinnskudd | 1 — Eiendeler | 200 000 kr | — |
| 2000 | Aksjekapital | 2 — Egenkapital og gjeld | — | 200 000 kr |

**Forklaring:** Bedriften får en eiendel (penger i banken), og eieren har en tilsvarende fordring på selskapet i form av aksjekapital. Balansen er i likevekt: Eiendeler (200 000) = Gjeld (0) + Egenkapital (200 000). Merk at dette er en ren balansetransaksjon — ingen resultatkontoer berøres.

**NS 4102-kobling:** Konto 1920 tilhører klasse 1 (Eiendeler), konto 2000 tilhører klasse 2 (Egenkapital og gjeld). Begge er balansekontoer.

---

### Scenario 2: Kjøp av kontorrekvisita på kreditt (med inngående MVA)

**Hendelse:** DATA1500 Konsult AS kjøper kontorrekvisita for 4 375 kr inkl. 25% MVA fra leverandøren Kontormateriell AS. Fakturaen betales om 30 dager (netto 30).

| Kontonr | Kontonavn | Kontoklasse | Debet | Kredit |
|---|---|---|---|---|
| 6560 | Rekvisita | 6 — Annen driftskostnad | 3 500 kr | — |
| 2710 | Inngående MVA, høy sats | 2 — Egenkapital og gjeld | 875 kr | — |
| 2400 | Leverandørgjeld | 2 — Egenkapital og gjeld | — | 4 375 kr |

**Forklaring:** En driftskostnad på 3 500 kr (eks. MVA) oppstår og reduserer egenkapitalen indirekte. Bedriften har en fordring på staten for inngående MVA på 875 kr (som vil bli avregnet mot utgående MVA). Leverandøren har en fordring på bedriften for hele beløpet inkl. MVA (4 375 kr). Summen balanserer: 3 500 + 875 = 4 375.

**NS 4102-kobling:** Konto 6560 tilhører klasse 6, konto 2710 og 2400 tilhører klasse 2. `MVA-linjer`-tabellen skal inneholde én rad for denne transaksjonen med grunnlag = 3 500 og MVA-beløp = 875.

---

### Scenario 3: Fakturering av en kunde (med utgående MVA)

**Hendelse:** Bedriften fullfører et konsulentoppdrag for kunden TechNord AS og sender en faktura på 62 500 kr inkl. 25% MVA. Betalingsbetingelse: 30 dager netto.

| Kontonr | Kontonavn | Kontoklasse | Debet | Kredit |
|---|---|---|---|---|
| 1500 | Kundefordringer | 1 — Eiendeler | 62 500 kr | — |
| 3100 | Salgsinntekt, tjenester | 3 — Salgsinntekter | — | 50 000 kr |
| 2700 | Utgående MVA, høy sats | 2 — Egenkapital og gjeld | — | 12 500 kr |

**Forklaring:** Bedriften har nå en fordring på kunden (en eiendel på 62 500 kr), har opptjent en inntekt på 50 000 kr (som øker egenkapitalen), og skylder staten 12 500 kr i utgående MVA. Summen balanserer: 62 500 = 50 000 + 12 500.

**NS 4102-kobling:** Konto 1500 tilhører klasse 1, konto 3100 tilhører klasse 3, konto 2700 tilhører klasse 2. Dette scenariet involverer `Fakturaer`- og `Fakturalinjer`-tabellene i tillegg til kjerne-transaksjonene.

---

### Scenario 4: Innbetaling fra kunde

**Hendelse:** TechNord AS betaler fakturaen fra Scenario 3 ved forfall.

| Kontonr | Kontonavn | Kontoklasse | Debet | Kredit |
|---|---|---|---|---|
| 1920 | Bankinnskudd | 1 — Eiendeler | 62 500 kr | — |
| 1500 | Kundefordringer | 1 — Eiendeler | — | 62 500 kr |

**Forklaring:** Fordringen er innfridd. Bankkontoen øker, mens kundefordringen nulles ut. Begge kontoer er eiendeler (klasse 1), så egenkapitalen og resultatet påvirkes ikke — kun sammensetningen av eiendelene endres. Fakturaens status oppdateres til `BETALT`.

---

### Scenario 5: Lønnsutbetaling med forskuddstrekk og arbeidsgiveravgift

**Hendelse:** Bedriften utbetaler lønn for mars 2026. Bruttolønn er 45 000 kr. Forskuddstrekk er 12 000 kr. Arbeidsgiveravgift (14,1%) er 6 345 kr.

**Del A — Lønnsutbetaling:**

| Kontonr | Kontonavn | Kontoklasse | Debet | Kredit |
|---|---|---|---|---|
| 5000 | Lønn til ansatte | 5 — Lønnskostnad | 45 000 kr | — |
| 1920 | Bankinnskudd | 1 — Eiendeler | — | 33 000 kr |
| 2600 | Forskuddstrekk | 2 — Egenkapital og gjeld | — | 12 000 kr |

**Del B — Arbeidsgiveravgift (periodisering):**

| Kontonr | Kontonavn | Kontoklasse | Debet | Kredit |
|---|---|---|---|---|
| 5400 | Arbeidsgiveravgift | 5 — Lønnskostnad | 6 345 kr | — |
| 2780 | Skyldig arbeidsgiveravgift | 2 — Egenkapital og gjeld | — | 6 345 kr |

**Forklaring:** Lønnskostnaden er 45 000 kr (bruttolønn). Nettolønnen som utbetales er 33 000 kr. Skattetrekket på 12 000 kr er en kortsiktig gjeld til kemneren. Arbeidsgiveravgiften er en separat kostnad og gjeld. Kontoklasse 5 (Lønnskostnad) er her representert av to ulike kontoer.

---

### Scenario 6: Kjøp av utenlandsk verdipapir (flervaluta)

**Hendelse:** Bedriften kjøper 10 aksjer i Apple Inc. (AAPL) for 175 USD per aksje. Valutakursen er 10,50 NOK/USD. Handelen koster totalt 1 750 USD = 18 375 NOK.

| Kontonr | Kontonavn | Kontoklasse | Debet | Kredit |
|---|---|---|---|---|
| 1350 | Aksjer i utenlandske selskaper | 1 — Eiendeler | 18 375 kr | — |
| 1920 | Bankinnskudd | 1 — Eiendeler | — | 18 375 kr |

**Forklaring:** Dette er en sammensatt flervalutatransaksjon. Posteringen på konto 1350 har `antall_teller = 10` (antall AAPL-aksjer) og `belop_teller = 175000` / `belop_nevner = 100` (1 750 USD). Posteringen på konto 1920 har `belop_teller = 1837500` / `belop_nevner = 100` (18 375 NOK). `Valutakurser`-tabellen inneholder kursen 10,50 NOK/USD for denne datoen. Et `Lot` opprettes for å spore kostprisen for fremtidig gevinstberegning (FIFO).

---

### Scenario 7: Innbetaling av MVA til staten (avregning)

**Hendelse:** Bedriften sender inn MVA-oppgave for 1. termin og betaler netto MVA til Skatteetaten. Utgående MVA er 12 500 kr (fra Scenario 3), inngående MVA er 875 kr (fra Scenario 2). Netto å betale: 11 625 kr.

| Kontonr | Kontonavn | Kontoklasse | Debet | Kredit |
|---|---|---|---|---|
| 2700 | Utgående MVA, høy sats | 2 — Egenkapital og gjeld | 12 500 kr | — |
| 2710 | Inngående MVA, høy sats | 2 — Egenkapital og gjeld | — | 875 kr |
| 2740 | Oppgjørskonto MVA | 2 — Egenkapital og gjeld | — | 11 625 kr |
| 1920 | Bankinnskudd | 1 — Eiendeler | — | 11 625 kr |

**Forklaring:** MVA-gjelden nulles ut. Netto MVA-beløp betales fra bankkontoen. Dette scenariet demonstrerer verdien av `Regnskapsperioder`-tabellen: perioden for 1. termin kan nå låses (`LAAST`) for å forhindre etterpostering.

---

### Scenario 8: Prosjektfakturering med delvis betaling og valutatap

**Hendelse:** DATA1500 Konsult AS fullfører et prosjekt for den svenske kunden Göteborg Tech AB. Faktura sendes på 50 000 SEK. Kursen ved fakturering er 1,02 NOK/SEK (faktura = 51 000 NOK). Kunden betaler 30 dager senere, men kursen har da falt til 0,98 NOK/SEK (innbetaling = 49 000 NOK).

**Del A — Fakturering:**

| Kontonr | Kontonavn | Kontoklasse | Debet | Kredit |
|---|---|---|---|---|
| 1500 | Kundefordringer (SEK) | 1 — Eiendeler | 51 000 kr | — |
| 3100 | Salgsinntekt, tjenester | 3 — Salgsinntekter | — | 51 000 kr |

**Del B — Innbetaling (med valutatap):**

| Kontonr | Kontonavn | Kontoklasse | Debet | Kredit |
|---|---|---|---|---|
| 1920 | Bankinnskudd | 1 — Eiendeler | 49 000 kr | — |
| 8160 | Valutatap (disagio) | 8 — Finansposter | 2 000 kr | — |
| 1500 | Kundefordringer (SEK) | 1 — Eiendeler | — | 51 000 kr |

**Forklaring:** Valutatapet på 2 000 kr (51 000 − 49 000) posteres på konto 8160 (klasse 8 — Finansposter). Dette er det eneste scenariet der klasse 8 brukes, og det illustrerer hvordan valutasvingninger påvirker resultatet. `Valutakurser`-tabellen inneholder begge kursene (ved fakturering og ved betaling).

---