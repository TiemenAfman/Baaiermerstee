# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project

Narrowcasting-menubord voor dorpshuis Baaiermerstee in Bierum (Groningen). Één enkel HTML-bestand (`index.html`) dat op een iiyama LH4375UHS in **portrait** draait en via GitHub Pages wordt gehost.

- Live: `https://tiemenafman.github.io/Baaiermerstee/`
- Branch: `main` (GitHub Pages deployt vanaf `main`, map `/`)
- Na elke wijziging: `git add index.html && git commit -m "..." && git push`
- De pagina herlaadt zichzelf elke 20 minuten, zodat nieuwe prijzen vanzelf op het scherm komen

## Scherm & technische eisen

- Ontwerpresolutie: **2160 × 3840 px** (4K portrait), vaste `#stage` die met `transform: scale()` op elk scherm past
- Kiosk: `overflow: hidden`, `cursor: none`, `<meta name="google" content="notranslate">`
- Geen frameworks of build-stappen. Enige externe bron: Google Fonts (Young Serif + Atkinson Hyperlegible) met systeem-fallbacks
- Lettergroottes zijn afgestemd op leesbaarheid vanaf ~3 meter — **niet verkleinen** (items 52 px, prijzen 54 px)

## URL-parameters (voor testen en installatie)

| Parameter | Werking |
|---|---|
| `?t=21:30` of `?t=2026-12-21T16:00` | Vaste tijd (zon, maan, lucht en klok) |
| `?demo` | Een etmaal in 96 seconden, om de luchtovergangen te bekijken |
| `?rotate=90` / `?rotate=270` | Pagina zelf draaien als de speler geen portrait kan uitsturen |

## Architectuur van index.html

1. **Lucht** (`.sky`, 0–760 px): volgt de echte zonnestand boven Bierum (53.4167 N, 6.8667 E) met een SunCalc-achtige berekening in de pagina.
   - Zon en maan staan op hun echte azimut/hoogte (azimut 40°–320° → volle breedte, hoogte 0°–62° → horizon tot boven)
   - Luchtkleuren, bordkleur en zonkleur worden geïnterpoleerd uit de tabel `SKY` (per zonshoogte in graden)
   - Sterren verschijnen onder −3°, maanfase wordt getekend met een SVG-pad
   - Silhouet van Bierum (kerk met steunbeer, boerderijen, bomen, dijk, turbines Eemshaven) als SVG met `fill: var(--ground-top)`, zodat het naadloos in het bord overloopt
   - Klok, datum en zon-op/zon-onder rechtsboven
2. **Titelband** (`.band`, 760–980 px): "Baaiermerstee" in Young Serif + "Afhaalmenu"
3. **Menubord** (`.board`, 980–3660 px): twee kolommen met kaarten, opgebouwd uit `MENU` in het script
   - Kaarten groeien naar rato van hun aantal regels (`flex-grow`), zodat beide kolommen gelijk uitkomen
   - `fitRows()` verkleint te lange namen tot ze op één regel passen
   - `EVENT` = de Noaberschap-kaart onder in de linkerkolom (`show: false` verbergt hem)
4. **Voet** (`.foot`): "Waar jong en oud elkaar ontmoeten!"

## Menu aanpassen

Alles staat in het `MENU`-object bovenin `<script>`. Een item is `[naam, prijs]` of `[naam, prijs, toelichting]`, bijvoorbeeld `['Kipnuggets', '2,50', '6 stuks']`. Nieuwe secties: een object `{ title, items }` toevoegen aan `left` of `right`; de kolomhoogte verdeelt zichzelf.

## Ontwerprichtlijnen

- Accentkleur: geel `#f9a825` (prijzen, koppen, zon, kaartranden)
- Tekst: warm gebroken wit `#f6f0e6`
- De bordkleur (`--ground-top` / `--ground-bottom`) wordt door JS gezet en wordt donkerder naarmate het buiten donkerder is — niet hardcoden
- Geen zware effecten (`backdrop-filter`, grote blur-filters): de speler is een signage-SoC of Raspberry Pi
