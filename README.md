# P1 Energie Analyser

Een volledig client-side webapplicatie voor het analyseren van slimme meter data (P1-poort). Alle verwerking vindt lokaal in de browser plaats — er worden geen gegevens naar een server verstuurd.

---

## Functionaliteiten

- **Overzichtskaarten** — totale afname, teruglevering, netto verbruik, zelfvoorzienendheid en periode
- **Dagelijkse statistieken** — minimum, maximum, gemiddelde, P90 en piekinterval voor zowel afname als teruglevering
- **Grafieken per uur** — gemiddelde afname en teruglevering per uur van de dag
- **Grafieken per weekdag** — gemiddelde dagelijkse afname en teruglevering per weekdag
- **Grafieken per maand** — totale afname en teruglevering per kalendermaand
- **Heatmap** — uur van de dag × maand, schakelbaar tussen afname en teruglevering
- **Periodes** — configureerbare tijdvakken (bijv. avond/nacht, ochtend, dag) met statistieken per tijdvak; handig voor het dimensioneren van een thuisbatterij
- **Gegevenstabel** — eerste 1000 meetintervallen met tijdstip, afname en teruglevering in kWh en Watt

---

## Vereist CSV-formaat

Het bestand moet een komma-gescheiden CSV zijn met de volgende **exacte kolomnamen** in de eerste rij:

| Kolom | Omschrijving |
|---|---|
| `time` | Tijdstip van meting, bijv. `2025-01-01 00:15` |
| `Import T1 kWh` | Cumulatieve afname tarief 1 (dal) |
| `Import T2 kWh` | Cumulatieve afname tarief 2 (piek) |
| `Export T1 kWh` | Cumulatieve teruglevering tarief 1 |
| `Export T2 kWh` | Cumulatieve teruglevering tarief 2 |

Aanvullende kolommen (zoals `L1 max W`, `L2 max W`, `L3 max W`) worden genegeerd.

De waarden zijn **cumulatieve** kWh-standen (zoals de meter ze opslaat). De applicatie berekent zelf de delta's per interval.

### Voorbeeldregel

```
time,Import T1 kWh,Import T2 kWh,Export T1 kWh,Export T2 kWh
2025-01-01 00:00,23622.271,16316.212,6047.762,14135.324
2025-01-01 00:15,23622.398,16316.212,6047.762,14135.324
```

Het bestand [`voorbeeld-data.csv`](voorbeeld-data.csv) bevat 1000 rijen met gesimuleerde data (10 dagen, januari 2025) die het juiste formaat illustreren inclusief dag/nacht-ritme en zonnepanelen.

---

## Uploadvereisten

- Alleen `.csv` bestanden
- Maximale bestandsgrootte: **10 MB**
- Minimaal 2 gegevensrijen vereist

---

## Lokaal draaien

De applicatie bestaat uit één HTML-bestand zonder bouwstap. Je kunt het op twee manieren openen:

**Via een lokale server (aanbevolen — automatisch laden van CSV):**

```bash
npx serve .
```

Open daarna `http://localhost:3000` in je browser. Als er een CSV-bestand in dezelfde map staat met de naam `P1e-2025-1-1-2026-1-1.csv`, wordt dat automatisch geladen.

**Direct als bestand:**

Open `index.html` direct in je browser. Je kunt dan handmatig een CSV-bestand slepen of selecteren via de uploadknop.

---

## Hosten

De applicatie is één statisch HTML-bestand en draait op elk webhostingplatform zonder backend:

- **GitHub Pages** — push naar een repository en activeer Pages
- **Netlify / Vercel** — drag-and-drop de map in de dashboard
- **Eigen webserver** — kopieer `index.html` naar je `public_html` of `www` map

Omdat alle verwerking in de browser plaatsvindt, heb je geen server-side taal (PHP, Python, Node.js) nodig.

---

## Beveiliging

| Maatregel | Toelichting |
|---|---|
| Client-side verwerking | Geüploade bestanden verlaten nooit de browser |
| Bestandstypevalidatie | Extensiecheck vóór verwerking |
| Bestandsgrootte limiet | Maximaal 10 MB om misbruik te voorkomen |
| Kolomnaamvalidatie | Strikte headercheck met duidelijke foutmelding |
| HTML-escaping | Alle CSV-inhoud wordt gesaniteerd voor weergave |
| Geen externe opslag | Geen cookies, localStorage of externe API-aanroepen |

---

## Afhankelijkheden

| Pakket | Versie | Gebruik |
|---|---|---|
| [Chart.js](https://www.chartjs.org/) | 4.4.0 | Staafgrafieken |

Geladen via CDN. Geen `npm install` nodig.

---

## Bestandsstructuur

```
p1-analyser/
├── index.html          # Volledige applicatie (HTML + CSS + JS)
├── voorbeeld-data.csv  # Gesimuleerde voorbeelddata (1000 rijen)
├── .gitignore          # Sluit echte meetbestanden (P1e-*.csv) uit
└── README.md           # Deze documentatie
```

> Echte meetbestanden (`P1e-*.csv`) worden door `.gitignore` uitgesloten en komen nooit in versiebeheer terecht.
