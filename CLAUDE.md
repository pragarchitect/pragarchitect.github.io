# Pragmatic Architect — pragarchitect.github.io

Persoonlijke blog van Marcel Schutte over pragmatische software-architectuur.
Motto (ondertitel): *"Yes, it depends. Let's find out on what."*

## Techniek
- Jekyll-site op GitHub Pages (gem `github-pages`), thema gebaseerd op Reverie / Jekyll Now.
- Default branch: `master`. Alles wat op `master` komt staat direct live.
- `master` is beschermd met een GitHub-ruleset: wijzigingen alleen via een pull request,
  de check `build` (Jekyll-build + linkcheck) moet groen zijn, en force-push/verwijderen is geblokkeerd.
  Alleen Marcel (repo-admin) kan dit omzeilen.
- Permalink: `/:title/` → de URL is de slug uit de bestandsnaam (zonder datum).
- Plugins: jekyll-sitemap, jekyll-feed, jekyll-seo-tag, jekyll-paginate (6 per pagina).
- Ruby 3.3 via rbenv (`.ruby-version`); gems staan buiten de repo (niet in `vendor/`, ivm OneDrive-sync).
- **Na elke wijziging aan posts/layouts: `bundle exec jekyll build` draaien** en fouten oplossen vóór committen.
- Links checken: `htmlproofer ./_site --disable-external --no-enforce-https --allow-missing-href`
  (ook in de GitHub Action `build-check`; een kapotte interne link of ontbrekende alt-tekst maakt de build rood).
- Lokaal bekijken: `bundle exec jekyll serve` → http://localhost:4000
- `bundle install` faalt op `'iostream' file not found` (kapotte Command Line Tools)? Draai dan:
  `CPLUS_INCLUDE_PATH=$(xcrun --show-sdk-path)/usr/include/c++/v1 bundle install`

## Structuur
- `_posts/` — artikelen, `YYYY-MM-DD-slug.md` (slug in kleine letters, woorden gescheiden door `-`).
- `_pages/` — vaste pagina's (about, archive, categories, search, talks).
- `_archief/` — demo-posts van het originele thema; niet aanpassen, niet als voorbeeld gebruiken.
- `images/` — afbeeldingen. Per artikel: `images/YYYYMMDD-naam.jpg` of een map `images/YYYYMMDD/`.
- `_includes/pullquote.html` — pullquote-component.
- `_config.yml` — **niet wijzigen zonder expliciet te vragen.**

## Opbouw van een post
```markdown
---
layout: post
title: "Titel in zinsvorm"
categories: [architecture, teams]
---

Openingsalinea: de haak. Dit wordt de excerpt op de homepage.

---

Rest van het artikel...
```
- De **eerste alinea** is de excerpt op de homepage → kort, concreet, maakt nieuwsgierig.
- Direct daarna een `---` als visuele scheiding.
- Pullquote: `{% include pullquote.html quote="Eén pakkende zin." %}`
- Afbeelding: `<img src="{{ site.baseurl }}/images/<bestand>" style="width: 700px;"/>`
- Interne link: `[tekst]({{ site.baseurl }}/<slug>/)`
- Categorieën: gebruik `categories: [...]` (niet `category:`), kleine letters, bij voorkeur bestaande:
  `architecture`, `teams`, `agile`, `security`, `adr`, `java`, `archunit`, `java magazine`,
  `business`, `lessonslearned`, `conference`, `misc`. Nederlandstalige posts: `architectuur`.
  Een nieuwe categorie alleen als geen bestaande past.

## Schrijfstijl (belangrijk)
- Standaard **Engels**; Nederlands alleen als daarom gevraagd wordt (bijv. bij een NL-congres).
- Eerste persoon, persoonlijk, vanuit de praktijk. Begin vaak met een concrete situatie uit een team.
- **Eenvoudig, direct Engels**: korte zinnen, gewone woorden, geen jargon-opstapeling, geen marketingtaal. Engels is niet de moedertaal van de schrijver.
  Het mag klinken als een Nederlandse architect die helder Engels schrijft — niet als een copywriter.
- Kort: meestal **300–600 woorden**. Langere stukken (conferentieverslag, tutorial) alleen op verzoek.
- Vaste ingrediënten: herkenbare anekdote → één kerninzicht (pullquote) → praktische tips (lijstje) → nuchtere, soms licht humoristische afsluiter.
- Pragmatisch en genuanceerd: noem ook wanneer de regel níet geldt.
- Vermijd: em-dashes in overvloed, "In today's fast-paced world", "delve", "game-changer", opsommingen van buzzwords, afsluitende samenvatting-alinea's ("In conclusion…").
- Verwijs naar eerdere posts als dat logisch is (interne links).

## Werkwijze voor agents
- **Nooit direct op `master` committen of pushen.** Werk op een branch (`artikel/<slug>` of `fix/<onderwerp>`).
  Een push naar `master` wordt door GitHub geweigerd; probeer dat niet te omzeilen.
- Pushen en PR's aanmaken alleen na akkoord van Marcel. Mergen doet Marcel zelf — nooit zelf mergen,
  ook niet als de checks groen zijn.
- Faalt de `build`-check op een PR? Lees de fout, los hem op dezelfde branch op en push opnieuw.
- Voor je een branch maakt: eerst `git pull` op `master`, er kan vanaf een andere plek (telefoon/cloud) gewerkt zijn.
- Feiten, citaten en bronnen niet verzinnen. Persoonlijke anekdotes die je niet kent: markeer als `<!-- TODO Marcel: ... -->` in plaats van ze te fabriceren.
- Nieuwe artikelen: gebruik de skill `/nieuw-artikel`.
