# LiiS Tilskuddsagent

## Hva dette er
En AI-drevet tilskuddscrawler for LiiS og Parkdressen-verdikjeden. Du (Claude Code) er agenten som søker, vurderer og katalogiserer tilskuddsordninger.

## Kommandoer

### `/tilskudd` — Søk etter nye tilskuddsordninger
1. Les `config/kriterier.json` for verdikjedelag og matchkriterier
2. Les `config/seed_kilder.json` for kjente tilskuddsportaler
3. Les `funn/katalog.json` for eksisterende funn (unngå duplikater)
4. **Fase 1 — Seed-crawl**: Besøk hver kilde i seed-listen og let etter aktive tilskuddsordninger
5. **Fase 2 — Søkebasert**: Kjør web-søk med nøkkelord fra kriterielisten for å fange opp ordninger seed-listen ikke dekker
6. **Fase 3 — Matching**: For hvert funn, vurder relevans mot hvert verdikjedelag og aktørprofil
7. **Fase 4 — Katalogisering**: Legg nye funn til `funn/katalog.json`
8. **Fase 5 — Dashboard**: Regenerer `docs/index.html` fra katalogen

### `/tilskudd-nordisk` — Utvid søket til nordiske/EU-kilder
Samme prosess som over, men bruk også kildene i `config/seed_kilder.json` merket `scope: "nordisk"` og `scope: "eu"`.

### `/tilskudd-status` — Vis oversikt
Gi en oppsummering av antall ordninger per verdikjedelag, kommende frister, og nye funn siden sist.

### `/tilskudd-oppdater [id]` — Oppdater en spesifikk ordning
Sjekk om en ordning fortsatt er aktiv, oppdater frister og detaljer.

## Kontekst om LiiS

### Selskapet
LiiS.com driver Parkdressen — en abonnementsbasert utleiemodell for barns yttertøy levert til barnehager. Modellen er B2B2C: kommuner/barnehager er kunder, foreldre er sluttbrukere. LiiS driver også en B2B dataplattform som selger livssyklusdata til merkevarer.

### Verdikjeden
Parkdressen-verdikjeden omfatter flere lag som alle kan være relevante for ulike tilskudd:

1. **Design og produktutvikling** — Sirkulært design, holdbarhet, materialvalg
2. **Produksjon og leverandør** — Bærekraftig produksjon, EPR, merkevarepartnerskap
3. **Logistikk og redistribusjon** — Omvendt logistikk, RFID-sporing, Lundebygruppen
4. **Digital plattform og teknologi** — SaaS, DPP (Digital Product Passport), data/AI
5. **Kommunal implementering** — Offentlige anskaffelser, Klimasats, kommunale tjenester
6. **Forskning og dokumentasjon** — Sirkulær økonomi-forskning, atferdsendring, helseeffekter
7. **Forretningsutvikling og skalering** — Vekst, internasjonalisering, investering
8. **Reparasjon og levetidsforlengelse** — Reparasjonstjenester, vedlikehold, opplæring
9. **Kommunikasjon og atferdsendring** — Nudging, holdningskampanjer, forbrukerengasjement

### Aktørprofiler (hvem kan søke)
- **LiiS AS** — Plattformeier, sirkulær forretningsmodell, SMB
- **Lundebygruppen** — Logistikkpartner, reverse logistics, lagerhold
- **Kommuner** — Offentlige innkjøpere, Klimasats-berettiget
- **Merkevarer i barnesegmentet** — Produsenter av barnetøy, utstyr, møbler
- **Forskningsinstitusjoner** — Universiteter, SINTEF, SIFO, OsloMet
- **Konsortium/partnerskap** — Flere aktører sammen

## Vurderingskriterier for matching

Når du vurderer en tilskuddsordning, scor den mot disse:

### Tematisk relevans (vekting: 40%)
- Sirkulær økonomi / gjenbruk / deling
- Tekstil / barn / barnetøy
- Product-as-a-Service / abonnementsmodeller
- Kommunale tjenester / offentlig innovasjon
- Grønn logistikk / reverse logistics
- Digitalisering / plattformøkonomi
- Bærekraftig forbruk / atferdsendring

### Søkertype-match (vekting: 25%)
- Passer for SMB / oppstartsbedrift
- Passer for kommune
- Passer for konsortium
- Passer for forskningssamarbeid

### Praktisk gjennomførbarhet (vekting: 20%)
- Søknadskompleksitet (enkel/middels/kompleks)
- Krav til egenfinansiering
- Krav til partnere
- Tidshorisont for prosjekt

### Strategisk verdi (vekting: 15%)
- Størrelse på tilskudd
- Synlighet/prestisje
- Nettverkseffekt (åpner dører)
- Passer nåværende fase av LiiS

## Output-format for funn

Hvert funn i `funn/katalog.json` skal ha denne strukturen:
```json
{
  "id": "unik-id",
  "navn": "Navn på ordning",
  "kilde": "Organisasjon som gir tilskudd",
  "url": "https://...",
  "beskrivelse": "Kort beskrivelse",
  "beløp": "100 000 - 500 000 kr",
  "frist": "2026-06-01 eller løpende",
  "scope": "norsk|nordisk|eu",
  "verdikjedelag": ["4. Digital plattform og teknologi", "6. Forskning og dokumentasjon"],
  "aktører": ["LiiS AS", "Forskningsinstitusjoner"],
  "relevans_score": 0.85,
  "prioritet": "høy|middels|lav",
  "notater": "Spesifikke merknader om hvorfor denne er relevant",
  "status": "ny|vurdert|søkt|avslått|innvilget",
  "funnet_dato": "2026-03-20",
  "sist_oppdatert": "2026-03-20"
}
```

## Regler
- Ikke legg til ordninger som allerede finnes i katalogen (sjekk URL og navn)
- Sett alltid `status: "ny"` for nye funn
- Prioritet settes automatisk basert på relevans_score: høy ≥ 0.7, middels ≥ 0.4, lav < 0.4
- Vær konservativ med scoring — en ordning for "generell innovasjon" scorer lavere enn en for "sirkulær økonomi i tekstilbransjen"
- Inkluder alltid URL til den offisielle søknadssiden
- Hvis en frist er ukjent, sett "løpende" eller "ukjent"
