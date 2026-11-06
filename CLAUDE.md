# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project

Narrowcasting-menubord voor dorpshuis Baaiermerstee in Bierum (Groningen). Één enkel HTML-bestand (`index.html`) dat op een iiyama LH4375UHS in **portrait** draait en via GitHub Pages wordt gehost.

- Live: `https://tiemenafman.github.io/Baaiermerstee/`
- Branch: `main` (GitHub Pages deployt vanaf `main`, map `/`)
- Na elke wijziging: `git add index.html && git commit -m "..." && git push`
- De pagina controleert elke `CHECK_MINUTES` minuten (nu 1) stil op de achtergrond of `index.html` op de server is veranderd en herlaadt alleen dan, dus geen geflits; `0` schakelt de controle uit. Pas na twee opeenvolgende checks met dezelfde nieuwe inhoud wordt er herladen (voorkomt een herlaad op een eenmalige CDN-hik)
- Voor een Raspberry Pi met FullPageOS als alternatief scherm: zie `wifi-setup.md`

## Scherm & technische eisen

- Ontwerpresolutie: **2160 × 3840 px** (4K portrait), vaste `#stage` die met `transform: scale()` op elk scherm past en zichzelf 90° draait als het beeldvlak liggend is
- Geen flex `gap` gebruiken (oudere Android-WebViews op signage-SoCs kennen dat niet); marges in plaats daarvan
- Kiosk: `overflow: hidden`, `cursor: none`, `<meta name="google" content="notranslate">`
- Geen frameworks of build-stappen. Enige externe bron: Google Fonts (Young Serif + Atkinson Hyperlegible, plus Creepster voor de Halloween-titel; een font wordt pas opgehaald als het gebruikt wordt) met systeem-fallbacks
- Lettergroottes zijn afgestemd op leesbaarheid vanaf ~3 meter — **niet verkleinen** (items 56 px, prijzen 58 px)

## URL-parameters (voor testen en installatie)

| Parameter | Werking |
|---|---|
| `?t=21:30` of `?t=2026-12-21T16:00` | Vaste tijd (zon, maan, lucht en klok) |
| `?demo` | Een etmaal in 96 seconden, om de luchtovergangen te bekijken |
| `?rotate=auto` (standaard) | Staand beeldvlak → rechtop; liggend beeldvlak → 90° gedraaid zodat het scherm vult (op Windows/Mac blijft de pagina rechtop) |
| `?rotate=0` / `90` / `180` / `270` | Vaste draaiing, bijv. `270` als het beeld ondersteboven staat |
| `?debug` | Toont viewport, schaal en user-agent op het scherm, handig bij het instellen van het display |
| `?theme=halloween` / `sinterklaas` / `kerst` / `oudennieuw` / `none` | Vast thema, of `none` voor het gewone bord, los van de datum (combineer met `?t=` voor dag of nacht) |

Het thema volgt verder vanzelf de datum uit `?t=` of `?demo`, dus `?t=2026-10-20T21:00` toont Halloween bij nacht.

## Architectuur van index.html

1. **Lucht** (`.sky`, 0–380 px): volgt de echte zonnestand boven Bierum (53.4167 N, 6.8667 E) met een SunCalc-achtige berekening in de pagina.
   - Zon en maan staan op hun echte azimut/hoogte (azimut 40°–320° → volle breedte, hoogte 0°–62° → horizon tot boven)
   - Luchtkleuren, bordkleur en zonkleur worden geïnterpoleerd uit de tabel `SKY` (per zonshoogte in graden)
   - Sterren verschijnen onder −3°, maanfase wordt getekend met een SVG-pad
   - Silhouet van Bierum (kerk met steunbeer, boerderijen, bomen, dijk, turbines Eemshaven) als SVG die de bordkleur (`currentColor` = `--ground-top`) overneemt, zodat het naadloos in het bord overloopt
   - Klok en datum rechtsboven
2. **Titelband** (`.band`, 380–600 px): "Baaiermerstee" in Young Serif + "Menukaart"
3. **Menubord** (`.board`, 600–3660 px): twee kolommen met kaarten, opgebouwd uit `MENU` in het script
   - Kaarten groeien naar rato van hun aantal regels (`flex-grow`), zodat beide kolommen gelijk uitkomen
   - `fitRows()` verkleint te lange namen tot ze op één regel passen
   - `SNACK_VAN_DE_MAAND` = optionele kaart bovenin de linkerkolom, `EVENT` = de Noaberschap-kaart onderin de linkerkolom (`show: false` verbergt elk apart)
4. **Voet** (`.foot`, 3660–3840 px): "Waar jong en oud elkaar ontmoeten!"
5. **Thema's**: een seizoenslaag over het gewone bord, die op datum aangaat (zie hieronder)

## Thema's (seizoensversiering op datum)

Er is één `index.html` op `main` en dus één `MENU` als prijsbron: thema's zijn geen aparte branches meer. Ze gaan vanzelf aan en uit.

- **Periodetabel**: `THEMES` bovenin `<script>`, vlak na `EVENT`. Per thema `{ id, van, tot }` als `'dd-mm'`, lokale tijd. `van` is de eerste dag mét thema, `tot` de eerste dag zónder (exclusief). Een periode mag over de jaargrens lopen (`oudennieuw`: 27-12 tot 02-01). Bij overlap wint de bovenste regel.
  - `halloween` 01-10 tot 01-11, `sinterklaas` 01-11 tot 06-12, `kerst` 06-12 tot 27-12, `oudennieuw` 27-12 tot 02-01
- **Werking**: `render()` bepaalt elke seconde via `themeFor(now())` het thema en zet bij een wissel de klasse `theme-<id>` op `#stage`. Zo wisselt het om middernacht vanzelf (het scherm herlaadt alleen als `index.html` verandert) en werken `?t=`, `?demo` en `?theme=` mee.
- **Zichtbaarheid**: elementen die bij één thema horen krijgen de klasse `t-<id>` en staan zonder dat thema op `display: none`, dus ze tekenen niet en hun animaties draaien niet (prestaties op de signage-SoC/Pi). Gewone elementen pas je aan met CSS onder `.theme-<id>` (bijv. `--accent`, `--accent-rgb`, kaartkleuren, titel).
- **Lucht en bord**: een thema kan een eigen luchttabel hebben in `SKY_THEMES[<id>]` (zelfde opbouw als `SKY`, inclusief de bordkleuren). JS zet `--ground-top`/`--ground-bottom` daaruit, dus nooit hardcoden.
- **Algemene haken** (door `render()` gezet op `#stage`):
  - klasse `after-sunset` zodra de zon officieel onder is (−0,833°) tot zonsopgang; bijv. de skeletten van Halloween en de Sint op zijn schimmel
  - `--night` (0 overdag … 1 's nachts) en `--glow` (0,3 … 1) voor verlichte ramen en kaarsjes
  - `.land .lit`: ramen in het silhouet die 's avonds oplichten (aan bij alle vier de thema's)
  - `.no-kerst`: de twee bolle bomen (naast de kerk en de derde links bij boerderij 1) die met kerst plaatsmaken voor kerstbomen
- **Halloween**: oranje accent, paarse lucht, Creepster-titel, kale bomen en kerkhof bij de Sebastiaankerk, vleermuizen, heks langs de maan, grondmist, pompoenen (ook in de voet), spinnenwebben en spin op de kaarten, en na zonsondergang dansende skeletten.
- **Sinterklaas**: goud accent, wijnrood bord, blauwe winterlucht, stoomboot die achter de dijk aanmeert (met rook, vlaggetjes en 's avonds verlichte patrijspoorten), pakjes op de grondlijn en in de voet, mijter in de titelband, gouden staf-krul op de bovenste kaarten, en na zonsondergang de Sint op zijn schimmel die over de daken rijdt en pepernoten strooit.
- **Kerst**: kaarsgoud accent, donker dennengroen bord met hulstrode kaartranden, heldere winterlucht met lichte sneeuwval (twee getegelde lagen, vier bewegende elementen), sneeuw op daken, dijk, wierde en boomkruinen, fonkelende lichtjes in de bomen, drie groene kerstbomen met slingers lampjes, ballen en een piek (`KERSTBOMEN` in `buildKerst`: slank bij boerderij 1, breed naast de Sebastiaankerk, klein bij boerderij 2 op de dijk), een ster boven de toren, hulst in de titelband, een dennentak op de bovenste kaarten met een schommelende kerstbal aan de Patat-kaart, kerstboompjes in de voet, en na zonsondergang de arreslee met rendieren langs de maan.
  - De lampjes zijn dekkende, verzadigde kernen (goud, rood, wit, blauw; in de kerstbomen met een donkere rand), zodat ze ook overdag tegen de lichte lucht en de sneeuw te zien zijn. 's Avonds krijgen ze een halo (radiale gradiënt per kleur, opacity `--night`) en zakt het groen van de kerstbomen weg in het silhouet (`.kdim`). Alle lampjes zitten in drie groepen `.xl > .a/.b/.c`, dus drie fonkelanimaties; overdag fonkelen ze nauwelijks.
- **Oud en nieuw**: champagne-accent, middernachtsblauw bord, serpentines op de bovenste kaarten, klinkende glazen in de titelband, oliebollen en een glas champagne (met bubbels) in de voet. Overdag carbidschieten: melkbussen op het erf en op de dijk, met steekvlam en rookwolk. Na zonsondergang vuurwerk achter het silhouet (4 pijlen). Daarvoor zet JS op `#stage`:
  - `nye` op 31 december na zonsondergang en in de nacht naar 1 januari (tot 12:00): 6 extra pijlen (`.fw.extra`)
  - `new-year` op 1 januari: "Gelukkig nieuwjaar!" met "Gelukkig nijjoar · <jaar>" linksboven in de lucht

**Een thema toevoegen** (bijv. `kerst`):
1. Regel staat al in `THEMES` (anders toevoegen, met `van`/`tot`)
2. CSS onder `.theme-kerst` voor bestaande elementen, en eigen elementen in de HTML met klasse `t-kerst`
3. Voeg in de CSS `#stage:not(.theme-kerst) .t-kerst` toe aan de regel met `display: none !important` bovenaan het themablok
4. Eventueel `SKY_THEMES.kerst` voor eigen lucht- en bordkleuren, en JS-opbouw van versiering (zoals `buildPumpkins`) die alleen klaarzet; de CSS bepaalt de zichtbaarheid
5. Animaties en `prefers-reduced-motion` toevoegen aan het `@media`-blok; houd het licht (alleen `transform`/`opacity`, geen blur)
6. Testen met `?theme=kerst&t=…T12:00` en `T21:00`, en de grensdagen met `?t=`

Let op: een CSS-animatie op een element binnen een verborgen SVG-groep (`<g class="t-…">` in het silhouet) blijft in Chrome doortikken. Hang zulke animaties daarom aan `.theme-<id>` (zoals `.theme-kerst .land .xl g`).

## Menu aanpassen

Alles staat in het `MENU`-object bovenin `<script>`. Een item is `[naam, prijs]` of `[naam, prijs, toelichting]`, bijvoorbeeld `['Kipnuggets', '2,50', '6 stuks']`. Nieuwe secties: een object `{ title, items }` toevoegen aan `left` of `right`; de kolomhoogte verdeelt zichzelf. `SNACK_VAN_DE_MAAND` en `EVENT` (vlak erna in het script) zijn losse kaarten met dezelfde `show`-vlag.

## Ontwerprichtlijnen

- Accentkleur: geel `#f9a825` (prijzen, koppen, zon, kaartranden); in CSS altijd via `var(--accent)` of `rgba(var(--accent-rgb), …)`, zodat een thema hem kan vervangen
- Tekst: warm gebroken wit `#f6f0e6`
- De bordkleur (`--ground-top` / `--ground-bottom`) wordt door JS gezet en wordt donkerder naarmate het buiten donkerder is — niet hardcoden
- Geen zware effecten (`backdrop-filter`, grote blur-filters): de speler is een signage-SoC of Raspberry Pi
