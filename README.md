# Hej! Jag heter Ameer Shameel 👋

Jag studerar till webbutvecklare i Stockholm och gillar att bygga hela kedjan – från databasscheman och REST-API:er till responsiva gränssnitt i React och Angular.

Det här repot (`amir0321/amir0321`) fungerar både som min **GitHub-profil** och som en **samling av mina tekniska reflektioner, arkitekturval och lärdomar** från de projekt jag har byggt under utbildningen och på egen hand.

---

## 🧠 Hur jag jobbar och tänker som utvecklare

1. **Jag vill förstå *varför*, inte bara få det att fungera:**
   När jag bygger ett projekt stannar jag inte vid "det funkar på min dator". Jag funderar på vad som händer om två användare bokar samma plats samtidigt, vad som händer när en gratis-server på Render går i viloläge, eller hur man undviker att en kund i frontend kan manipulera ett pris.
2. **Lagspelare som gärna tar ansvar i grupparbeten:**
   I våra skolprojekt tar jag ofta en drivande roll i att rita upp databasstrukturen, sätta upp projektets grundarkitektur (routes, controllers, services, gemensamma komponenter) och hjälpa gruppen att sy ihop frontend och backend inför leverans.
3. **Hur jag använder AI som verktyg:**
   Jag använder moderna AI-verktyg som bollplank, för att snabba upp boilerplate, testa kantfall och strukturera dokumentation. Men **alla arkitekturval, datamodeller, säkerhetsavvägningar och affärsregler styr jag själv**. För att göra det tydligt vad som är mina egna tankar har jag här nedanför (och i varje projekts `README.md`) sammanfattat vilka beslut jag tog och vad jag lärde mig i varje projekt.

---

## 📂 Mina projekt – Vad jag själv bestämde, varför & vad jag lärde mig

Här är en genomgång av mina viktigaste projekt, sorterade efter komplexitet. Klicka gärna vidare till fördjupningsfilerna i [`projects/`](./projects/) eller direkt till respektive repo:

### 1. 🎬 [Filmvisarna Cinema](https://github.com/amir0321/filmvisarna-cinema)
*Fullstack biografbokningssystem (Grupparbete) — React 19, TypeScript, Express 5, PostgreSQL, SCSS*
- **Vad jag fokuserade på:** Helhetsarkitektur, databastransaktioner för stolbokning, interaktiv platskarta i React, gästavbokning via signerad mejllänk och designsystem i SCSS.
- **Viktigaste tekniska val:**
  - **PostgreSQL-transaktioner mot dubbelbokning:** Om två personer väljer samma biostol samtidigt rullas den ena bokningen tillbaka på databasnivå och ger `409 Conflict` istället för att skriva över varandra.
  - **Egenbyggd platskarta (Seat Map):** Vi valde bort tredjepartsbibliotek för att kunna rendera dynamiska salonger (1–26 rader, 1–50 stolar) från databasen med full tangentbordsnavigering.
  - **Filbaserad routing (`import.meta.glob`):** För att hela gruppen skulle kunna skapa nya sidor parallellt utan ständiga merge-konflikter i en central router-fil.
- 👉 **[Läs hela min tekniska reflektion om Filmvisarna](./projects/filmvisarna-cinema.md)**

---

### 2. 📚 [Marginalia](https://github.com/amir0321/Marginalia)
*Personligt bok- och citatbibliotek (Arbetsprov) — Angular 20, .NET 9 Web API, EF Core, PostgreSQL, Docker, GitHub Actions*
- **Vad jag fokuserade på:** Att bygga en produktionsredo SPA + .NET-lösning med fokus på säkerhet, testbarhet och bra UX trots begränsningarna i gratis-hosting.
- **Viktigaste tekniska val:**
  - **Reverse Proxy via Netlify istället för CORS:** Genom att skicka `/api/*` via Netlifys proxy ser webbläsaren frontend och backend som samma ursprung (`same-origin`), vilket eliminerar `OPTIONS`-preflight och CORS-problem.
  - **Hantering av kallstarter (`ApiWakeService`):** Eftersom Render Free Tier sover efter 15 minuters inaktivitet byggde jag en bakgrundstjänst som väcker API:et direkt vid sidladdning och visar en lugn informationsruta om det tar mer än 2 sekunder.
  - **Angular Signals istället för NgRx:** Höll koden lättläst och reaktiv med inbyggda `signal()` och `computed()`.
  - **Skydd mot IDOR (`404` istället för `403`):** Om någon försöker hämta en annan användares bok svarar API:et `404 Not Found` så att man inte kan gissa vilka ID:n som finns.
- 👉 **[Läs hela min tekniska reflektion om Marginalia](./projects/Marginalia.md)**

---

### 3. 🚗 [AutoEskil](https://github.com/amir0321/AutoEskil)
*Bilförmedling & automatisk Lead-matchning — React, Node.js, Express, SQLite → PostgreSQL*
- **Vad jag fokuserade på:** Att lösa ett verkligt affärsproblem för bilförmedling där kundens önskemål (budget, miltal, drivmedel, märke) automatiskt matchas mot anslutna bilhandlares lager.
- **Viktigaste tekniska val & lärdomar:**
  - **Migrering från SQLite till PostgreSQL:** Jag började med SQLite lokalt för att komma igång snabbt, men insåg inför deploy på Render att filsystemet nollställs vid omstart. Jag skrev därför om `db.js` till PostgreSQL (`pg.Pool`) och byggde en wrapper som översatte `?` till `$1, $2` så att befintliga queries fortsatte fungera.
  - **`HttpOnly`-cookies för admin:** Valde `HttpOnly`-cookies + `express-rate-limit` för adminpanelen för att skydda sessionen mot XSS.
- 👉 **[Läs hela min tekniska reflektion om AutoEskil](./projects/AutoEskil.md)**

---

### 4. ☕ [AirBean API](https://github.com/amir0321/airbean-api)
*Backend-API för kaffebeställning med drönarleverans (Grupparbete) — Node.js, Express, SQLite*
- **Vad jag fokuserade på:** Databasmodellering (ER-diagram), arkitektur i tre lager (`routes` → `controllers` → `services`) och kampanj-/rabattlogik på serversidan.
- **Viktigaste tekniska val:**
  - **Serverside-beräkning av rabatter:** Kombinationsrabatter (t.ex. *Bryggkaffe + Ostmacka = 15 %*) räknas alltid ut i backend utifrån produkt-ID:n så att ingen kan skicka in ett manipulerat pris från klienten.
- 👉 **[Läs hela min tekniska reflektion om AirBean API](./projects/airbean-api.md)**

---

### 5. 🛡️ [CodeForge](https://github.com/amir0321/codeforge)
*Webbyrå- & systemutvecklingssida — React 19, Vite, Tailwind CSS, Framer Motion, Firebase*
- **Viktigaste tekniska val:**
  - **Egen språkhantering (SV/EN) via React Context:** Istället för att installera ett tungt i18n-bibliotek för två språk byggde jag `LanguageContext` med en strukturerad översättningsfil.
  - **Anti-spam utan CAPTCHA:** Byggde in honeypot-fält och rate-limiting i kontaktformuläret för att skydda EmailJS-kvoten mot botar utan att försämra UX för riktiga besökare.
- 👉 **[Läs mer om CodeForge, NextWatch, Taskflow och mina tidiga projekt](./projects/other-projects.md)**

---

### 6. Övriga projekt (Frontend, API & Grupparbeten)
- 🍿 **[NextWatch](https://github.com/amir0321/Nextwatch)** *(React, Vite, Tailwind, Axios, TMDB API)* – Filmapp där jag övade på att bryta ut API-logik i ett eget `services/`-lager, dynamisk routing (`/movie/:id`) och att skilja på container-sidor och presentationskomponenter.
- ✅ **[TASKFLOW](https://github.com/amir0321/TASKFLOW)** *(React 19, Express 5, MongoDB/Mongoose, JWT)* – Fullstack task-manager där jag ville lära mig NoSQL-modellering med Mongoose som kontrast till mina SQL-projekt.
- 🪐 **[Solsystem (Solaris)](https://github.com/amir0321/Solsystem)** *(Vanilla JS, HTML, CSS)* – Byggt helt utan ramverk för att förstå DOM-manipulation och asynkrona API-anrop (tvåstegsanrop med dynamisk API-nyckel) på djupet.
- 📖 **[Pärmar & Poesi](https://github.com/amir0321/Parmar-Poesi)** *(HTML, SASS, Tailwind)* – Mitt första grupparbete där jag ansvarade för header, main, footer och att merga ihop alla fem gruppmedlemmars kod till en fungerande helhet.
- 🎨 **[Modern UX](https://github.com/amir0321/modern-ux)** & **[Portfolio](https://github.com/amir0321/Portfolio)** *(React, Vite, Tailwind)* – UI- och portfolioprojekt med fokus på mörkt tema, datadrivna komponenter och responsiv layout.

---

## 📫 Kontakt
- **GitHub:** [github.com/amir0321](https://github.com/amir0321)
- **Plats:** Stockholm / Eskilstuna med omnejd
