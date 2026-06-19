---
layout: post
title: "Architectuur in een Agile Wereld"
category: architectuur
---

Hoe je architectuurbeslissingen neemt zonder het team te vertragen — een terugblik op mijn sessie bij het Agile Congres 2026 in Groningen.

---

Sprint 4. Het team heeft een oplossing voor een kritiek performance-probleem. Technisch goed onderbouwd, iedereen enthousiast. Dan de mededeling: "Dit moet eerst door de Architecture Review Board." Volgende meeting: acht weken. Het ticket gaat op wacht. De ARB keurt het goed — met kleine wijzigingen die de urgentie allang niet meer wegnemen. Het probleem is er nog steeds.

Dit verhaal herkennen de meeste mensen in de zaal direct. En het is precies de spanning waar deze sessie over gaat: **architectuur en Agile lijken elkaar in de weg te zitten**. Maar dat hoeft niet zo te zijn.

## De klassieke tegenstelling

Aan de ene kant: de waterval-architect met Big Design Up Front, maanden documentatie, en review cycles zonder einde. Aan de andere kant: het Agile team dat snel wil opleveren, iteratief werkt, en niet wil wachten op goedkeuring.

Geen wonder dat ze botsen.

Maar de tegenstelling is grotendeels vals. Het probleem is niet architectuur *an sich* — het is hoe architectuur traditioneel georganiseerd wordt. Architectuur hoeft geen rem te zijn. Het is een keuze hoe je het inricht.

## De architect als elevator

Gregor Hohpe beschrijft in *The Architect Elevator* drie posities die een architect kan innemen:

- **Penthouse** — ver van de code, beslist zonder context, verliest de realiteit
- **Engine Room** — diep in de code, ziet het grote plaatje niet, verliest de richting
- **Elevator** — reist tussen beide werelden, brengt context omhoog en omlaag, stelt vragen en geeft niet altijd antwoorden

De elevator-architect maakt het team eigenaar. Niet door beslissingen te nemen, maar door de juiste omstandigheden te scheppen zodat het team zelf goede beslissingen kan nemen.

> *"The architect's value is not in making decisions, but in creating the conditions for others to make good ones."*

## JIT-JEA: Just in Time / Just Enough Architecture

Het alternatief voor Big Design Up Front is niet géén architectuur — het is *slimmere* architectuur. JIT-JEA staat voor:

**Just Enough…**
- **Architecture** — alleen wat het team nu nodig heeft
- **Documentation** — vastleggen wat ertoe doet
- **Governance** — sturen zonder te remmen

**Just in Time…**
- Geleverd op het moment dat het team het nodig heeft, niet maanden vooruit
- In iteratie-formaat — kleine brokken, niet alles in één keer

Het idee is simpel: architectuurbeslissingen neem je op het moment dat je genoeg context hebt, en je legt precies zoveel vast als nodig is om de volgende stap te kunnen zetten.

## Architectuur in de praktijk: drie concrete tools

JIT-JEA klinkt als een principe. Maar hoe ziet het er in de praktijk uit? Ik werk met drie concrete instrumenten.

### C4 Model

Het C4 Model structureert architectuurdiagrammen in vier abstractielagen: Context, Containers, Components en Code. Je zoomt in waar nodig, en past het diagram aan per doelgroep. Via Structurizr DSL beheer je de diagrammen als code — ze leven in je repository, naast je code. Geen verouderde Visio-bestanden meer op een sharepoint.

### Architecture Decision Records (ADRs)

Een ADR is een klein Markdown-bestand waarin je één architectuurbeslissing vastlegt: de context, de overwogen opties, de beslissing, de reden, en de status. Ze leven in de repository. Iedereen kan bijdragen. Geen zware reviews nodig.

Het kernprincipe: **leg de WHY vast, niet alleen de WHAT**.

Architectuurbeslissingen die niemand meer weet te verklaren? Met ADRs weet iedereen waarom iets zo is — ook over twee jaar nog.

### ArchUnit — Fitness Functions

Architectuurafspraken slijten. Mensen vergeten regels. Reviews komen te laat. Technische schuld groeit stilletjes.

ArchUnit lost dit op door architectuurregels te formuleren als geautomatiseerde tests. Ze draaien in je CI-pipeline, en de build faalt bij een overtreding. Directe feedback — geen verrassingen bij de code review.

```java
layeredArchitecture()
  .layer("Presentation").definedBy("..controller..")
  .layer("Service").definedBy("..service..")
  .layer("Persistence").definedBy("..persistence..")
  .whereLayer("Service").mayOnlyBeAccessedByLayers("Presentation")
  .check(jc);
```

## Architectuur als AI-input

Een onverwacht voordeel van deze aanpak: doordat alles als platte tekst in de repository leeft, wordt het machineleesbaar — en daarmee geschikt als input voor AI agents.

- **ADRs zijn Markdown** — AI kan ze lezen, schrijven en samenvatten
- **C4/Structurizr DSL is platte tekst** — AI kan diagrammen genereren en interpreteren
- **ArchUnit-regels zijn code** — AI herkent patronen en stelt verbeteringen voor

Concreet: een AI agent kan een ADR genereren op basis van een pull request, controleren of nieuwe code consistent is met bestaande beslissingen, of een architectuurvraag beantwoorden met context uit je eigen repository.

Architectuur-als-code is niet alleen beter voor mensen. Het is ook beter voor machines.

## Gedeeld eigenaarschap

De tools helpen alleen als het team ze ook gebruikt. En dat gebeurt pas als het team eigenaarschap voelt over de architectuur.

Wat misgaat zonder gedeeld eigenaarschap:
- De architect schrijft, het team negeert
- Beslissingen worden niet nageleefd
- Technische schuld groeit onzichtbaar
- "Dat heeft de architect bedacht" — niet "ons systeem"

Hoe je het wél organiseert:
- ADRs schrijft het team **samen** met de architect
- C4-diagrammen worden besproken in de refinement
- ArchUnit-tests zijn onderdeel van de definition of done
- De architect stelt vragen — geeft niet altijd antwoorden

### Architectuur in het sprintritme

Geen aparte architectuurvergadering. Architectuur zit in het ritme van de sprint:

| Moment | Wat je doet |
|---|---|
| **Refinement** | C4-diagram bespreken — past de nieuwe feature nog in het plaatje? |
| **Tijdens de sprint** | ADR schrijven bij een beslissing — ArchUnit-tests in de pipeline |
| **Sprint review** | Architectuurpunten benoemen — wat veranderde er? |
| **Retrospective** | Wat verwaterde er? Welke afspraken werden niet nageleefd? |

## Architectuurkeuzes zijn businesskeuzes

Een misverstand dat ik vaak tegenkom: architectuurbeslissingen zijn technisch, de PO en business hoeven er niet bij betrokken te zijn. Maar elke architectuurkeuze heeft een prijs en een risico — en die zijn altijd ook zakelijk.

| De business zegt... | De architect hoort... |
|---|---|
| "Het duurt veel te lang" | Schaalbaarheid — we groeien sneller dan de architectuur aankan |
| "Het kost steeds meer geld" | Technische schuld — de prijs van eerdere keuzes |
| "We zijn afhankelijk van die ene leverancier" | Integratiekeuze — gemak van toen, risico van nu |

De architect vertaalt. Zonder die vertaling praten business en techniek langs elkaar.

## Uit de praktijk

Wat werkte:
- ADRs schrijven **tijdens** de sprint, niet erna
- C4 als gespreksstart met het team
- ArchUnit vroeg in het project inzetten
- Architect als teamlid, niet als reviewer

Wat niet werkte:
- Architectuurdocumentatie los van de code houden
- Wachten op "het juiste moment" voor beslissingen
- Architect die pas gevraagd wordt als het al fout is
- Te veel upfront — niemand leest het

## De kernboodschap

Architectuur hoeft het agile proces niet te vertragen.

**Goede architectuurbeslissingen neem je niet ondanks Agile, maar dankzij Agile.**

Architectuur is geen fase. Het is een gedeelde verantwoordelijkheid.

