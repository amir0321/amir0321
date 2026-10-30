# Marginalia – Tekniska val & lärdomar

**Repo:** [github.com/amir0321/Marginalia](https://github.com/amir0321/Marginalia)  
**Stack:** Angular 20 (Signals, Standalone Components), .NET 9 Web API, EF Core 9, PostgreSQL 17, Docker, GitHub Actions CI, Playwright

---

## Bakgrund

Marginalia byggdes som ett arbetsprov: en responsiv CRUD-applikation med JWT-autentisering där en användare kan hantera sina lästa böcker och upp till 5 favoritcitat. Uppgiften lämnade medvetet flera design- och arkitekturval öppna för egen tolkning.

## Vad jag själv bestämde och varför

### 1. Reverse Proxy (Same-Origin) istället för CORS
När frontend ligger på Netlify och .NET-API:et ligger på Render är standardlösningen att slå på CORS i backend. Jag valde istället att konfigurera Netlifys `_redirects` (och Angulars `proxy.conf.json` lokalt) som en omvänd proxy för `/api/*`.
- **Varför:** Webbläsaren upplever att både Angular-appen och API:et ligger på exakt samma domän. Det tar bort alla `OPTIONS`-preflight-anrop, gör anropen snabbare och minskar attackytan.

### 2. Lösa Render Free Tier-kallstarter i användargränssnittet (`ApiWakeService`)
Gratisinstanser på Render somnar efter 15 minuters inaktivitet och tar 50–60 sekunder att starta upp vid första anropet.
- **Varför:** Jag insåg att när en rekryterare öppnar live-länken för att testa inloggningen kommer servern sannolikt att sova. Om ingenting händer på en minut tror man att appen är trasig. Därför byggde jag `ApiWakeService` som direkt vid sidladdning skickar ett tyst anrop till `GET /api/health` och visar en lugn toast-notis om svaret dröjer mer än 2 sekunder.

### 3. Angular Signals istället för NgRx
- **Varför:** För en applikation med några få vyer (böcker, citat, autentisering, tema) ger ett stort state-bibliotek som NgRx bara onödig boilerplate. Genom att använda Angular 20:s inbyggda `signal()`, `computed()` och `OnPush` change detection blev koden kort, lättläst och enkel att enhetstesta.

### 4. Returnera `404 Not Found` istället för `403 Forbidden` på andras resurser
- **Varför:** Om användare A försöker hämta eller ta bort användare B:s bok via `/api/books/42`, och API:et svarar `403 Forbidden`, avslöjar servern att bok #42 existerar (IDOR-läcka). Genom att alltid filtrera frågan på `UserId` från JWT-token direkt i databasfrågan returneras konsekvent `404 Not Found`.

## Vad jag lärde mig
- Hur man sätter upp en komplett CI/CD-kedja i GitHub Actions som testar både .NET (`dotnet test`), Angular (`Karma/ChromeHeadless`) och Docker-bygget innan deploy.
- Hur man undviker layout-flimmer vid sidomladdning i Angular genom att kontrollera initial rutt innan shell-komponenten renderar menyn.
