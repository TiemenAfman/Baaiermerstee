# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project

Narrowcasting display pagina voor dorpshuis Baaiermerstee. Één enkel HTML-bestand (`index.html`) dat op een kiosk-scherm draait.

## Deployment

- Bestand: `index.html` (+ eventuele afbeeldingen in dezelfde map)
- GitHub repo: `https://github.com/TiemenAfman/Baaiermerstee` — branch `main`
- Na elke wijziging: `git add index.html && git commit -m "..." && git push`
- De pagina ververst zichzelf elke 60 seconden via `<meta http-equiv="refresh">`

## Scherm & technische eisen

- Resolutie: **2560×1440px** (QHD), vaste afmeting op `body`
- `overflow: hidden` en `cursor: none` — kiosk-omgeving, geen scrollbalk, geen cursor
- `<meta name="google" content="notranslate">` — geen automatische vertaling
- Geen externe dependencies, frameworks of build-stappen — puur HTML/CSS/JS

## Architectuur van index.html

Het bestand bevat een **slide-systeem** met twee slides die via JavaScript om de 20 seconden wisselen met een 1-seconde CSS fade (`transition: opacity 1s ease`):

- **Slide 1** (`#slide1`): Afhaal menu in 4 kolommen (Snacks, Borrelhapjes, Patat+Sauzen, Dranken)
- **Slide 2** (`#slide2`): Evenement-slide (Noaberschap etentje) met tekst links en `sameneten.png` rechts

Beide slides delen dezelfde `.header` en `.footer` HTML-structuur en CSS-klassen.

### CSS-klassen menu (Slide 1)

| Klasse | Gebruik |
|---|---|
| `.menu-item` + `.item-name` / `.item-price` | Reguliere items (Borrelhapjes, Patat, Sauzen) |
| `.menu-item-sm` + `.item-name-sm` / `.item-price-sm` | Compacte items voor kolommen met veel regels (Snacks, Dranken) |
| `.section-title` | Categorie-koptekst in geel (`#f9a825`) |
| `.section-gap` | Verticale ruimte tussen twee secties in één kolom |
| `.col-snacks / .col-borrel / .col-patat / .col-dranken` | Kolombreedtes via `flex` |

## Ontwerprichtlijnen

- Achtergrond: donker (`#1a2a3a → #2d4356`)
- Accentkleur: geel/oranje `#f9a825` (prijzen, titels, borders)
- Lettergroottes zijn afgestemd op leesbaarheid vanaf ~3 meter afstand — **niet verkleinen**
- Afbeeldingen opslaan in dezelfde map als `index.html` en refereren via relatief pad

## Nieuwe slides toevoegen

1. Voeg een `<div id="slideN" class="slide">` toe met dezelfde `.header` / `.footer` structuur
2. Het JavaScript-rotatiescript pikt nieuwe slides automatisch op via `querySelectorAll('.slide')`
