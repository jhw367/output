# H - Warm 2502 — investeringsroute en cashflow-update

Laatst bijgewerkt: 2026-05-24

## Kernadvies

De optimale volgorde is nu niet: direct warmtepomp kiezen. De betere route is gefaseerd:

1. **Nu / aanstaand:** SolarEdge hub-omvormer, thuisbatterij(en), SolarEdge laadunit en ERE-inboekroute combineren.  
2. **Vanaf volgend jaar:** dynamisch contract/EMS pas activeren op basis van meetdata en batterijgedrag.  
3. **Daarna:** warmtepomp als generieke route uitwerken, niet vastpinnen op één merk/type voordat Htr, afgifte en praktijkdata rond zijn.  
4. **Daarna finetunen:** lage-temperatuurafgifte, kierdichting, regeling, ventilatie/comfort.

## Waarom deze volgorde

- De woning heeft al veel PV en EV-verbruik; opslag + slim laden geeft direct effect zonder installatierisico van een te vroeg gekozen warmtepomp.
- De nieuwe hub-omvormer en batterij vormen de centrale energielaag: PV, batterij, EV-laden en later dynamische sturing.
- De SolarEdge laadunit kan operationeel logisch zijn als hij goed integreert met dezelfde energielaag; ERE-opbrengst maakt thuisladen extra aantrekkelijk, mits meting/data/inboekdienstverlener schriftelijk klopt.
- Een warmtepomp blijft strategisch belangrijk, maar typekeuze moet later volgen uit meetdata, comfortwens, afgifte en geluids/ruimte-eisen.

## ERE-vergoeding in deze analyse

Werkhypothese:

- thuisladen EV: **±3.500 kWh/jaar**;
- netto ERE-opbrengst: **€0,10/kWh**;
- indicatieve opbrengst: **±€350/jaar = ±€29/maand**.

Controlepunten vóór opdracht:

1. Exacte SolarEdge-laadunit en MID-meter/meetpad vastleggen.
2. Schriftelijke bevestiging inboekdienstverlener: model ondersteund, data-uitwisseling werkbaar, terugwerkende kracht/instapdatum duidelijk.
3. ERE niet als subsidie behandelen maar als marktmechanisme via inboekdienstverlener.

## Indicatieve investeringen

- SolarEdge hub-omvormer + batterij + SolarEdge laadunit + installatie/EMS: **±€11.300**.
- Dynamisch/EMS-inrichting volgend jaar: **±€350**.
- Generieke warmtepomproute later: **±€9.500 netto** na eventuele subsidie/keuzes.
- LT-afgifte/kierdichting/regeling: **±€2.500**.
- Ventilatie/comfort/koelstrategie: **±€1.800**.

Totale indicatieve investeringsroute: **±€25.450**.

## Maandkosten en cashflow

Referentie zonder nieuwe maatregelen: **±€342/maand**.

Na alle fases: **±€95/maand**.

Structurele maandelijkse verbetering na alle fases: **±€247/maand**.

Cumulatieve cashflow na 5 jaar inclusief investeringen: **±€-13,875**.  
Let op: dit is een investeringsroute, dus de cashflow blijft in de eerste jaren negatief; het doel is TCO, comfort, risicoverlaging en voorbereiding op salderingsafbouw/gasafbouw.

## Maandelijkse eventpunten

- **2026-06** — investering €11,300; SolarEdge hub-omvormer + thuisbatterij + SolarEdge laadunit + ERE-route; nieuwe maandkosten ±€250; netto maandcashflow incl. investering €-11,208; cumulatief €-11,208.
- **2027-06** — investering €350; Dynamisch contract/EMS optimalisatie na meetjaar; nieuwe maandkosten ±€222; netto maandcashflow incl. investering €-230; cumulatief €-10,426.
- **2028-06** — investering €9,500; Generieke warmtepomp-route (typekeuze later, op basis van meetdata); nieuwe maandkosten ±€104; netto maandcashflow incl. investering €-9,262; cumulatief €-18,368.
- **2029-06** — investering €2,500; LT-afgifte/kierdichting/regeling finetuning; nieuwe maandkosten ±€95; netto maandcashflow incl. investering €-2,253; cumulatief €-18,003.
- **2030-06** — investering €1,800; Ventilatie/comfort/koelstrategie optimalisatie; nieuwe maandkosten ±€95; netto maandcashflow incl. investering €-1,553; cumulatief €-16,839.

## Onzekerheden

- Exacte offertes voor batterijcapaciteit, hub-omvormer, laadunit, installatie en warmtepomp ontbreken nog.
- ERE-opbrengst per kWh is markt- en contractafhankelijk.
- Dynamisch contract kan positief of negatief uitpakken zonder goede sturing; pas doen als batterij/EV-sturing dat aankan.
- Warmtepompbesparing hangt af van werkelijke SCOP, gasprijs, elektra, tapwaterstrategie, afgifte en gedrag.

## Bestanden

- HTML-dashboard: `/home/hermes/energie/H - Warm 2502 - investeringsroute.html`
- JSON-model: `/home/hermes/energie/h-warm-2502-cashflow-model.json`
- Publieke kopie: `/home/hermes/work/output/h-warm/2026/h-warm-2502-investeringsroute-20260524.html`
