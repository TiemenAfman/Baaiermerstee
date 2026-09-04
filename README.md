# Baaiermerstee · afhaalmenu

Menubord voor dorpshuis [Baaiermerstee](https://baaiermerstee.nl/) in Bierum, gemaakt voor een iiyama LH4375UHS in portrait (2160 × 3840).

**Live:** https://tiemenafman.github.io/Baaiermerstee/

De lucht boven het silhouet van Bierum volgt de echte zonnestand: de zon en maan staan op hun werkelijke plek, en het bord wordt donkerder naarmate het buiten donkerder wordt.

## Prijzen aanpassen

Open `index.html`, zoek `const MENU` en pas de regels aan, bijvoorbeeld:

```js
['Frikandel', '2,25'],
['Kipnuggets', '2,50', '6 stuks'],
```

Commit en push naar `main`; het scherm controleert elke minuut stil of het bestand is veranderd en herlaadt alleen dan (`CHECK_MINUTES` bovenin het script, `0` = uit).

## Testen

- `index.html?t=21:30` — vaste tijd
- `index.html?demo` — een etmaal in anderhalve minuut
- `index.html?rotate=270` — vaste draaiing als het beeld ondersteboven staat (standaard herkent de pagina de oriëntatie zelf)
- `index.html?debug` — toont viewport en schaal op het scherm bij het instellen van het display

Zie `CLAUDE.md` voor de opbouw van het bestand en `wifi-setup.md` voor de Raspberry Pi-variant.
