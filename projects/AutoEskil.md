# AutoEskil – Tekniska val & lärdomar

**Repo:** [github.com/amir0321/AutoEskil](https://github.com/amir0321/AutoEskil)  
**Stack:** React, Vite, React Router, Node.js, Express, SQLite → PostgreSQL (`pg`)

---

## Bakgrund

AutoEskil är en webbapplikation för bilförmedling i Eskilstuna. Grundidén är att bilköpare inte bara ska behöva leta själva i ett lager, utan kan skicka in en intresseanmälan (*Lead*) med sina krav (märke, modell, drivmedel, årsmodell, miltal och maxbudget). Systemet matchar då automatiskt kundens önskemål mot bilar som finns inne hos anslutna bilhandlare (*Dealers*).

## Vad jag själv bestämde och varför

### 1. Migreringen från SQLite till PostgreSQL inför produktion
Jag byggde först hela backend med SQLite (`sqlite3`) eftersom det är smidigt under utveckling – databasen är bara en lokal fil (`bilformedling_etuna.db`).
- **Problemet jag stötte på:** När jag skulle driftsätta projektet på Render insåg jag att Render använder ett temporärt filsystem i sin gratisversion. Varje gång servern startade om raderades SQLite-filen och alla bilar och leads försvann.
- **Hur jag löste det:** Jag bytte ut SQLite mot en riktig PostgreSQL-databas (`pg.Pool`). För att slippa skriva om all logik i varje fil på en gång byggde jag en kompatibilitets-wrapper i `db.js` för `db.all()`, `db.get()` och `db.run()` som automatiskt översatte mina `?`-platshållare till PostgreSQL:s `$1, $2`-format.

### 2. Direkt matchning i `POST /api/leads` + `ON DELETE CASCADE`
- **Varför:** När en kund skickar in sin förfrågan ville jag både ge kunden ett direkt svar om det redan finns en matchande bil i lagret, och samtidigt spara matchningen för administratören i adminpanelen. Genom att använda `ON DELETE CASCADE` i databasschemat rensas gamla matchningsrader automatiskt bort när en bil eller bilhandlare raderas.

### 3. Cookie-baserad adminsession (`HttpOnly`)
- **Varför:** Eftersom vanliga besökare inte behöver logga in för att skicka in en intresseanmälan är det bara administratören som har ett konto. Jag valde då att sätta admin-token i en `HttpOnly`-cookie vid `POST /api/login` och skydda servern med `helmet` och `express-rate-limit`.
