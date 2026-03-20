# 🎯 LiiS Tilskuddradar

AI-drevet tilskuddscrawler for LiiS og Parkdressen-verdikjeden.

## Slik bruker du det

### Forutsetninger
- [Claude Code](https://docs.claude.com) installert
- Tilgang til dette repoet

### Kjør et søk
```bash
cd liis-tilskudd-agent
claude
```

I Claude Code, skriv:
```
/tilskudd
```

Claude leser konfigurasjonene, crawler kjente tilskuddsportaler, kjører web-søk, matcher mot verdikjedelaget, og oppdaterer katalogen + dashboardet.

### Andre kommandoer
| Kommando | Beskrivelse |
|---|---|
| `/tilskudd` | Søk etter norske tilskuddsordninger |
| `/tilskudd-nordisk` | Inkluder nordiske og EU-kilder |
| `/tilskudd-status` | Vis oppsummering av katalogen |
| `/tilskudd-oppdater [id]` | Oppdater en spesifikk ordning |

### Dashboard
Dashboardet publiseres automatisk via GitHub Pages fra `docs/`-mappen.

Etter push: `https://<bruker>.github.io/liis-tilskudd-agent/`

## Filstruktur
```
├── CLAUDE.md                  # Agent-instruksjoner for Claude Code
├── config/
│   ├── kriterier.json         # Verdikjedelag, aktørprofiler, scoring
│   └── seed_kilder.json       # Kjente tilskuddsportaler og søkeord
├── funn/
│   └── katalog.json           # Alle funn (oppdateres av agenten)
├── docs/
│   └── index.html             # Dashboard (GitHub Pages)
└── README.md
```

## Verdikjedelag
| Lag | Ikon | Eksempel-aktører |
|---|---|---|
| Design og produktutvikling | ✏️ | LiiS, merkevarer |
| Produksjon og leverandør | 🏭 | Merkevarer |
| Logistikk og redistribusjon | 🚛 | Lundebygruppen, LiiS |
| Digital plattform og teknologi | 💻 | LiiS |
| Kommunal implementering | 🏛️ | Kommuner, LiiS |
| Forskning og dokumentasjon | 🔬 | Universiteter, SINTEF |
| Forretningsutvikling og skalering | 📈 | LiiS |
| Reparasjon og levetidsforlengelse | 🔧 | LiiS, Lundebygruppen |
| Kommunikasjon og atferdsendring | 📣 | LiiS, kommuner |
