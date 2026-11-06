# Onderhoud menubord Baaiermerstee

Bijgewerkt op 6 november 2026

Wie het menubord wil bijwerken, heeft alleen een GitHub-account met schrijfrechten nodig. Aanpassen en publiceren gaat volledig in de browser, en het scherm in het dorpshuis neemt een wijziging binnen een paar minuten vanzelf over.

## Hoe het werkt

Een wijziging is binnen twee à drie minuten op het scherm te zien; je doet alleen de eerste stap zelf.

1. Je past het bestand `index.html` aan op GitHub, in de repository [TiemenAfman/Baaiermerstee](https://github.com/TiemenAfman/Baaiermerstee), en slaat op met "Commit changes".
2. GitHub Pages zet de nieuwe versie binnen ongeveer een minuut online op [tiemenafman.github.io/Baaiermerstee](https://tiemenafman.github.io/Baaiermerstee/).
3. Het scherm in het dorpshuis kijkt elke minuut of de pagina veranderd is. Ziet het twee keer achter elkaar dezelfde nieuwe versie, dan herlaadt het.

Alles staat in dat ene bestand. Er is geen server, geen andere software en geen abonnement: GitHub en GitHub Pages zijn gratis voor een openbare repository. Openbaar betekent ook dat iedereen de inhoud kan lezen, dus zet er nooit wachtwoorden of persoonsgegevens in.

## Wat je nodig hebt

Voor prijzen en het menu volstaan een gratis GitHub-account met schrijfrechten en een browser.

| Wat | Waarvoor | Hoe regel je het |
| --- | --- | --- |
| GitHub-account | Inloggen om te bewerken | Gratis aanmaken op [github.com](https://github.com/signup) |
| Schrijfrechten op de repository | Zonder kun je meekijken, maar niet opslaan | De eigenaar nodigt je uit via Settings → Collaborators → Add people; jij accepteert de uitnodiging uit de e-mail |
| Computer met een browser | Bewerken en controleren | Niets te installeren |
| Toegang tot het scherm in het dorpshuis | Alleen als het scherm vastloopt | Zie "Het scherm in het dorpshuis" |

Voor grotere wijzigingen, zoals een nieuw thema of een ander uiterlijk, is wat programmeerkennis nodig, of Claude Code. De technische uitleg daarvoor staat in `CLAUDE.md` in dezelfde repository.

## Prijzen of het menu aanpassen

Alle prijzen staan bovenin `index.html`, onder `const MENU`. Je past ze aan in de browser:

1. Ga naar [github.com/TiemenAfman/Baaiermerstee](https://github.com/TiemenAfman/Baaiermerstee) en log in.
2. Klik op `index.html` en dan rechtsboven op het potloodje (Edit this file).
3. Zoek met Ctrl+F naar `const MENU`. Daaronder staan alle secties met hun items.
4. Pas de tekst aan, bijvoorbeeld `['Frikandel', '2,25'],` wordt `['Frikandel', '2,50'],`.
5. Klik op "Commit changes…", schrijf een korte omschrijving ("Frikandel naar 2,50"), laat "Commit directly to the main branch" aan staan en klik op "Commit changes".

De regels voor het menu:

- Een item is `['naam', 'prijs'],`, of met een toelichting erbij `['Kipnuggets', '2,50', '6 stuks'],`.
- Een prijs schrijf je met een komma en zonder euroteken; dat zet de pagina er zelf voor.
- Elke regel eindigt op `],`. Een vergeten komma of aanhalingsteken maakt het hele bord kapot (zie hieronder).
- Een apostrof in een naam (zoals in pinda's) breekt de tekst af. Schrijf dan `\'`, dus `'Pinda\'s'`.
- Een nieuwe sectie is een blok `{ title: 'IJs', items: [ ... ] },` in `left` (linkerkolom) of `right` (rechterkolom). De kolommen verdelen de ruimte zelf.
- De kaart "Samen eten" staat iets lager onder `const EVENT`; met `show: false` verdwijnt hij. Onder `const SNACK_VAN_DE_MAAND` staat een kaart die je met `show: true` bovenin de linkerkolom zet.
- Alles onder het kopje "Locatie voor de zonnestand" is programmacode: daar niets aanpassen.

## Controleren, en terugdraaien als het misgaat

Controleer na elke wijziging binnen vijf minuten de live pagina, want één tikfout laat het hele bord verdwijnen.

1. Open op GitHub het tabblad Actions en wacht tot "pages build and deployment" een groen vinkje heeft (ongeveer een minuut).
2. Open [tiemenafman.github.io/Baaiermerstee](https://tiemenafman.github.io/Baaiermerstee/) en ververs met Ctrl+F5.
3. Kijk of het menu, de lucht en de klok er staan en of je wijziging klopt.

Is het bord leeg, zwart of mist het menu, dan zit er bijna altijd een vergeten komma, haakje of aanhalingsteken in je wijziging. Open `index.html` opnieuw en herstel het. Onder History (het klokje bij het bestand) zie je precies wat je veranderd hebt, in rood en groen.

**Belangrijk:** het scherm in het dorpshuis laadt een kapotte versie wel, maar herlaadt daarna niet meer vanzelf, ook niet als jij de fout hebt hersteld. Iemand moet dan in het dorpshuis de pagina op het scherm handmatig verversen of het scherm uit- en aanzetten.

Vooraf bekijken hoe het bord er op een ander moment uitziet, kan met een toevoeging aan de link:

- `?t=2026-12-15T21:00` — doet alsof het die datum en tijd is (lucht, zon, maan en thema)
- `?theme=kerst` of `?theme=none` — toont een thema, of juist geen
- `?demo` — speelt een heel etmaal af in anderhalve minuut

## Werken met Git op je eigen computer (technisch, optioneel)

Wie liever op de eigen computer werkt dan in de browser, gebruikt Git: `git pull` haalt de nieuwste versie op, `git push` zet jouw wijziging online. Het resultaat is hetzelfde als bewerken in de browser.

Eenmalig instellen:

1. Installeer Git van [git-scm.com](https://git-scm.com/downloads). Op Windows zit er een inlogvenster bij: de eerste keer dat je pusht, log je in de browser in bij GitHub. Op een Mac of Linux log je in met de [GitHub CLI](https://cli.github.com/): `gh auth login`.
2. Vertel Git wie je bent (dit komt bij elke wijziging te staan):

    ```
    git config --global user.name "Voornaam Achternaam"
    git config --global user.email "jij@voorbeeld.nl"
    ```

3. Haal de repository op en ga de map in:

    ```
    git clone https://github.com/TiemenAfman/Baaiermerstee.git
    cd Baaiermerstee
    ```

Elke keer dat je iets aanpast:

1. `git pull` — haal eerst de nieuwste versie op, want iemand anders kan intussen in de browser iets veranderd hebben.
2. Pas `index.html` aan in een teksteditor (Kladblok werkt, [Visual Studio Code](https://code.visualstudio.com/) is prettiger). Dezelfde regels als hierboven gelden.
3. Bekijk het resultaat door `index.html` in de browser te openen (dubbelklikken). Ook hier werken `?t=` en `?theme=` achter het adres. Of klik in Visual Studio Code met de rechtermuisknop op `index.html` en kies "Open with Live Server" (daarvoor installeer je eenmalig de extensie Live Server).
4. `git diff` — laat precies zien wat je veranderd hebt.
5. `git add index.html`
6. `git commit -m "Frikandel naar 2,50"`
7. `git push` — binnen een paar minuten staat het online; controleer het zoals hierboven beschreven.

Als het niet meteen lukt:

- **Push geweigerd** ("rejected", "fetch first"): iemand anders heeft intussen iets veranderd. Doe `git pull` en daarna opnieuw `git push`.
- **Conflict na `git pull`**: jij en iemand anders hebben dezelfde regel aangepast. Open `index.html` en zoek naar `<<<<<<<`. Tussen die markering en `=======` staat jouw versie, tussen `=======` en `>>>>>>>` de andere. Laat de juiste versie staan, haal de drie markeringsregels weg, en doe `git add index.html`, `git commit` en `git push`.
- **Je laatste wijziging ongedaan maken**: `git revert HEAD` en daarna `git push`. Dit maakt een nieuwe wijziging die de vorige terugdraait; er gaat niets verloren.
- **Waar sta ik?** `git status` laat zien wat er veranderd is en of je achterloopt; `git log --oneline` toont de geschiedenis.

Gebruik nooit `git push --force`: dat kan wijzigingen van anderen overschrijven. Werk altijd op de branch `main`, want alleen die staat online.

## Thema's en hun data

De thema's gaan om middernacht vanzelf aan en uit, en de prijzen blijven in elk thema dezelfde.

| Thema | Van | Tot en met |
| --- | --- | --- |
| Halloween | 1 oktober | 31 oktober |
| Sinterklaas | 1 november | 5 december |
| Kerst | 6 december | 26 december |
| Oud en nieuw | 27 december | 1 januari |
| Nieuwjaarsgroet (gewoon bord met "Gelukkig nieuwjaar!") | 2 januari | 14 januari |

De data staan onder `const THEMES`, direct onder het menu. `van` is de eerste dag mét thema en `tot` de eerste dag zónder, geschreven als dag-maand: `{ id: 'kerst', van: '06-12', tot: '27-12' },`. Een thema overslaan kan door `//` voor zijn regel te zetten. Een nieuw thema maken of het uiterlijk veranderen is programmeerwerk; zie `CLAUDE.md`.

## Het scherm in het dorpshuis

Het scherm is een iiyama LH4375UHS (43 inch), staand opgehangen, en toont de live link. Valt het internet weg, dan blijft de laatst geladen versie gewoon staan.

Toevoegingen aan de link op het scherm zelf:

- `?rotate=270` — als het beeld ondersteboven staat (standaard draait de pagina zichzelf goed)
- `?debug` — toont schermmaat en schaal, handig bij het instellen

Nog in te vullen door de huidige beheerder:

- Hoe en waar is de link op het scherm ingesteld (menu van het scherm of een app), en is daar een pincode voor nodig?
- Hoe ververs je de pagina op het scherm, en waar liggen de afstandsbediening en de aan/uit-knop?
- Op welk wifi-netwerk zit het scherm, en wie beheert dat?

## Overdracht regelen

De repository hangt nu aan het persoonlijke GitHub-account van Tiemen; met deze stappen kan iemand anders het overnemen.

- [ ] De nieuwe beheerder maakt een GitHub-account aan.
- [ ] Tiemen nodigt die uit als collaborator op TiemenAfman/Baaiermerstee, en de nieuwe beheerder accepteert.
- [ ] Tiemen vult de open vragen over het scherm in.
- [ ] Samen één keer een prijs aanpassen, controleren en zo nodig herstellen.
- [ ] Besluiten of de repository naar een eigen GitHub-organisatie van het dorpshuis gaat, zodat hij niet aan één persoon hangt. De link verandert dan, dus die moet ook op het scherm worden aangepast.
