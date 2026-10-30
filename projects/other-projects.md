# Övriga projekt – Tankar, val & lärdomar

Här är en kort sammanfattning av vad jag ville lära mig och vilka val jag gjorde i mina övriga projekt på GitHub.

---

## 1. [CodeForge](https://github.com/amir0321/codeforge) *(React 19, Vite, Tailwind CSS, Framer Motion, EmailJS)*
- **Vad det är:** En webbplats för en digital byrå med fokus på cybersäkerhet, prestanda och flerspråkighet.
- **Mina egna val:**
  - **Språkbyte (SV/EN) med egen Context:** Istället för att ta in `react-i18next` för en mindre sajt skapade jag `LanguageContext` och en ren objektstruktur i `translations.js`. Det gav mig full kontroll och noll extra paketberoenden.
  - **Honeypot & rate-limiting i kontaktformuläret:** Eftersom sidan är statiskt hostad på Firebase och använder EmailJS ville jag skydda formuläret mot spambotar utan att tvinga användaren att klicka i trafikljus (reCAPTCHA). Ett osynligt honeypot-fält + tidsgräns löste det snyggt.

---

## 2. [NextWatch](https://github.com/amir0321/Nextwatch) *(React, Vite, Tailwind CSS, React Router, Axios, TMDB API)*
- **Vad det är:** En filmapp som hämtar populära och kommande filmer från The Movie Database (TMDB).
- **Mina egna val:**
  - **Separera API-anrop från UI (`services/`):** Jag bröt ut alla Axios-anrop till en egen `services/`-mapp så att sidkomponenterna slipper innehålla URL:er och fetch-logik.
  - **Enkel state-hantering (`useState` / `useEffect`) framför Redux:** Jag funderade på om Context eller Redux behövdes, men kom fram till att vanliga props och lokal state gjorde koden mycket enklare att följa för den här storleken på app.
  - **Egen statuskomponent (`MoviesStatus`):** Istället för att upprepa laddningssnurror och felmeddelanden på varje sida samlade jag det i en återanvändbar komponent.

---

## 3. [TASKFLOW](https://github.com/amir0321/TASKFLOW) *(React 19, Express 5, MongoDB/Mongoose, JWT, Tailwind v4)*
- **Vad det är:** En fullstack todo-/projekthanterare med inloggning, prioriteringar och statusflöden (*To Do → In Progress → Done*).
- **Mina egna val & lärdomar:**
  - **Varför MongoDB här?** I mina andra projekt har jag använt SQL (PostgreSQL och SQLite). Med Taskflow ville jag lära mig hur man modellerar dokument och relationer i en NoSQL-databas med Mongoose, samt hur man bygger JWT-autentisering och lösenordshashning (`bcrypt`) i Express 5.

---

## 4. [Solsystem – Solaris](https://github.com/amir0321/Solsystem) *(Vanilla JavaScript, HTML5, CSS3)*
- **Vad det är:** En interaktiv visualisering av solsystemet byggd helt utan ramverk.
- **Mina lärdomar:**
  - Att hantera asynkrona beroenden i ren JavaScript (`async/await`): först hämta en API-nyckel från en endpoint och sedan använda nyckeln för att hämta planeterna i `service.js`.
  - Hur mycket React faktiskt hjälper en i vardagen – när man bygger allt med `document.createElement` och event-lyssnare i `script.js` får man en mycket djupare förståelse för hur DOM:en fungerar under huven.

---

## 5. [Pärmar & Poesi](https://github.com/amir0321/Parmar-Poesi) *(HTML5, SASS, Tailwind CSS – Grupparbete)*
- **Vad det är:** Landningssida för en lokal bokhandel, byggt i en grupp om fem personer.
- **Min roll & lärdom:**
  - Utöver att bygga `header`, `main` och `footer` tog jag på mig uppgiften att **koppla ihop allas kod på slutet** och städa upp alla smådetaljer. Det lärde mig hur viktigt det är att tidigt komma överens om namngivning av CSS-klasser och variabler så att inte allas stilar krockar vid merge.
