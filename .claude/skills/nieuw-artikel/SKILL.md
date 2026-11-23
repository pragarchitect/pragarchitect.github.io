---
name: nieuw-artikel
description: Nieuw blogartikel schrijven voor Pragmatic Architect (Jekyll-post in _posts/) op een eigen branch. Gebruik bij "nieuw artikel", "schrijf een post over...", "blog over...".
argument-hint: <onderwerp of idee> [nl] [lang]
---

# /nieuw-artikel

Schrijf een nieuw artikel voor de Pragmatic Architect-blog op basis van: **$ARGUMENTS**

Volg de conventies en schrijfstijl uit `CLAUDE.md` strikt.

## Stappen

1. **Context ophalen**
   - Lees 2–3 recente posts in `_posts/` (de nieuwste eerst) om toon en lengte te kalibreren.
     Sla `_archief/` over.
   - Check of het onderwerp al eerder behandeld is (`grep -ril` op kernwoorden in `_posts/`).
     Zo ja: meld dat en stel een andere invalshoek voor, of link ernaar.
   - Is het idee te vaag (alleen een trefwoord)? Stel dan maximaal 2 korte vragen:
     wat is de kern/boodschap, en is er een eigen ervaring of anekdote bij? Anders: direct doorgaan.

2. **Opties bepalen**
   - Taal: Engels, tenzij `nl` in de argumenten staat of het onderwerp duidelijk Nederlands is.
   - Lengte: 300–600 woorden, tenzij `lang` in de argumenten staat (dan max ~1200).
   - Datum: vandaag (`date +%F`).
   - Slug: kort, Engels (of NL bij NL-post), kleine letters, koppeltekens, max ~6 woorden.

3. **Branch maken**
   ```bash
   git checkout master && git pull --ff-only
   git checkout -b artikel/<slug>
   ```
   Lukt `git pull` niet (bijv. offline), meld het en ga verder vanaf de lokale `master`.

4. **Artikel schrijven** → `_posts/<YYYY-MM-DD>-<slug>.md`
   - Front matter: `layout: post`, `title` (zinsvorm, tussen quotes als er een `:` in staat),
     `categories: [...]` met 1–3 bestaande categorieën uit `CLAUDE.md`.
   - Eerste alinea = excerpt/haak, gevolgd door `---`.
   - Precies één pullquote via `{% include pullquote.html quote="..." %}`.
   - Een praktisch lijstje (tips, vragen of stappen) waar het past.
   - Nuchtere afsluiter, geen samenvattende "In conclusion".
   - Waar relevant 1 interne link naar een eerdere post: `[tekst]({{ site.baseurl }}/<slug>/)`.
     **Link alleen naar posts die echt in `_posts/` staan** (slug = bestandsnaam zonder datum).
     Wil je verwijzen naar een post die nog niet bestaat? Gebruik dan
     `<!-- TODO: link naar /<slug>/ zodra die post bestaat -->` in plaats van een link.
   - Afbeeldingen altijd met een korte, beschrijvende `alt="..."`.
   - **Niets verzinnen**: geen nep-anekdotes, cijfers of citaten. Waar een persoonlijke ervaring nodig is
     die je niet kent, zet je `<!-- TODO Marcel: korte anekdote over ... -->`.
   - Externe bronnen alleen als je ze zeker kent; anders weglaten.

5. **Zelfreview** (stil, vóór je het toont)
   - Klinkt het als de bestaande posts? Korte zinnen, gewone woorden, geen AI-clichés
     (delve, game-changer, landscape, "In today's...", overmatig em-dashes)?
   - Woordenaantal binnen de grens (`wc -w`)?
   - Front matter valide YAML, bestandsnaam en slug kloppen?
   - Als `bundle exec jekyll build` beschikbaar is: draaien en fouten oplossen. Anders overslaan en melden.
   - Als `htmlproofer` beschikbaar is (anders `gem install html-proofer`):
     `htmlproofer ./_site --disable-external --no-enforce-https --allow-missing-href`
     en kapotte links of ontbrekende alt-teksten oplossen. Lukt installeren niet: melden en doorgaan,
     de GitHub Action controleert het ook.

6. **Committen (lokaal)**
   ```bash
   git add _posts/<bestand>
   git commit -m "Concept: <titel>"
   ```

7. **Opleveren**
   Toon Marcel:
   - pad van het bestand en de branch
   - titel, categorieën, woordenaantal
   - de excerpt (eerste alinea) en de pullquote
   - eventuele `TODO Marcel`-punten
   - suggestie voor een afbeelding (onderwerp/zoekterm voor Unsplash), niet zelf downloaden

   Vraag daarna wat de volgende stap is: aanpassen, pushen + PR openen, of later verder.
   **Push en open geen PR zonder expliciet akkoord. Merge nooit naar `master`.**
