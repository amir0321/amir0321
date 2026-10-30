# Filmvisarna Cinema – Tekniska val, min roll & lärdomar

**Repo:** [github.com/amir0321/filmvisarna-cinema](https://github.com/amir0321/filmvisarna-cinema)  
**Stack:** React 19, TypeScript, Vite, Bootstrap 5 + SCSS, Node.js, Express 5, PostgreSQL, Zod, JWT

---

## Bakgrund & min roll i gruppen

Filmvisarna är ett fullstack-bokningssystem för en biograf med både en publik del (bläddra bland filmer, filtrera visningar, välja stolar på en interaktiv salongskarta, förbeställa snacks från kiosken och avboka biljetter) samt ett komplett admin-backoffice (hantera salonger, filmer, visningsschema, bokningar och statistik).

I gruppen tog jag ett stort ansvar för:
- Att sätta upp arkitekturen och typkontrakten mellan frontend (`Frontend/src/types`) och backend (`Backend/src`).
- Att bryta ner Figma-designen till ett konsekvent designsystem med SCSS-tokens (`_variables.scss`, `_components.scss`) och återanvändbara komponenter.
- Att säkra kritiska flöden som stolbokning, gäst-avbokning med token och validering.

## Viktiga beslut jag/vi fattade och varför

### 1. PostgreSQL-transaktioner mot dubbelbokning
**Problemet:** Vad händer om två besökare sitter på varsin dator och klickar på "Boka" för rad D stol 7 i samma sekund?  
**Mitt/vårt val:** Vi valde att aldrig lita enbart på vad UI:t visar. Hela bokningsflödet i `bookings.service.ts` körs inuti en databastransaktion (`BEGIN` ... `COMMIT`/`ROLLBACK`). Innan bokningen sparas kontrolleras att ingen av stolarna redan är bokad på visningen. Om en krock upptäcks rullas hela transaktionen tillbaka och backend returnerar `409 Conflict`, vilket frontend fångar upp och ber användaren välja nya stolar.

### 2. Egenbyggd salongskarta (Seat Map) utan externa bibliotek
**Problemet:** Många färdiga seat-map-bibliotek är svåra att anpassa för både mobil (horisontell scroll), mörkt tema och tangentbordsnavigering.  
**Mitt/vårt val:** Vi byggde komponenten själva i React och SCSS. Eftersom backend returnerar salongens rader och stolar dynamiskt (en salong kan skapas med 1–26 rader och 1–50 stolar per rad) ritar vår komponent upp rutnätet helt dynamiskt, visar en böjd `BIODUK` med projektionssken och stödjer olika lägen (publik bokning, mini-karta på Salonger-sidan och live-karta i admin).

### 3. Filbaserad routing (`import.meta.glob`) för att undvika merge-konflikter
**Problemet:** När flera utvecklare i en grupp lägger till nya sidor samtidigt uppstår nästan alltid merge-konflikter i `App.tsx` eller router-filen.  
**Mitt/vårt val:** Vi byggde `routes.tsx` med Vites `import.meta.glob` som automatiskt läser in alla komponenter i `src/pages/`. Varje sida exporterar själv `navTitle`, `navOrder` eller `hideFromNav`, och filer som innehåller `detail` i namnet får automatiskt `:id` som URL-parameter.

### 4. Gästavbokning via signerad länk istället för kontotvång
**Problemet:** En gäst som bokar utan konto kan inte logga in för att se eller avboka sina biljetter, och att låta vem som helst slå upp en bokningsreferens publikt hade varit en säkerhetsrisk (man hade kunnat gissa andras referenser).  
**Mitt/vårt val:** `lookup`-endpointen låstes till endast Admin. För gäster skapade vi istället en signerad token-länk i bekräftelsemejlet som går till `/cancel-booking?token=...`. Sidan hämtar först en förhandsgranskning (`GET /api/bookings/cancel-preview`) och låter kunden bekräfta avbokningen (`POST /api/bookings/cancel-by-token`) så länge det är minst 2 timmar kvar till visningen startar.

## Vad jag lärde mig
- Hur mycket enklare samarbetet i en grupp blir när man har ett strikt API-kontrakt (Zod i backend + TypeScript-typer i frontend) från dag ett.
- Hur man hanterar tidzoner (`Europe/Stockholm`) och datumfilter konsekvent mellan databas och UI.
- Hur man använder AI effektivt som bollplank för delmoment utan att tappa kontrollen över helhetsarkitekturen.
