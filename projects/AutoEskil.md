# AutoEskil

AutoEskil är en webbapplikation för bilförmedling i Eskilstuna. En köpare kan ange önskemål som budget, miltal och drivmedel och få förslag från anslutna bilhandlares lager. Administratören kan följa upp förfrågningarna och se vilka bilar som matchade.

**Teknik** · React, Vite, React Router, Node.js, Express och PostgreSQL

[Visa källkoden för AutoEskil](https://github.com/amir0321/AutoEskil)

## Min roll

Jag byggde flödet från kundens intresseanmälan till matchningen och uppföljningen i adminpanelen. Det innebar att översätta köparens önskemål till sökvillkor, modellera relationerna mellan handlare, bilar och förfrågningar samt anpassa databasen för drift.

## Tekniska val

### Från SQLite till PostgreSQL inför driftsättning

SQLite gav mig en enkel start under lokal utveckling. När jag driftsatte applikationen på Render behövde datan överleva omstarter även när serverns filsystem återställdes. Jag flyttade därför datalagret till PostgreSQL. Ett kompatibilitetslager i `db.js` behöll gränssnittet för `db.all()`, `db.get()` och `db.run()` och översatte frågornas platshållare från `?` till `$1`, `$2` och så vidare. Det gjorde att befintliga anrop kunde behållas under övergången.

### Direkt återkoppling och sparade matchningar

När en förfrågan kommer in via `POST /api/leads` sparar servern den och söker efter bilar som passar önskemålen. Köparen får eventuella träffar direkt och administratören kan se samma matchningar vid uppföljningen. Främmande nycklar med `ON DELETE CASCADE` rensar kopplingarna när en bil eller en förfrågan tas bort.

### Separat inloggning för administration

Besökare kan skicka in en förfrågan utan konto. Vid admininloggning sparar servern sessionstoken i en `HttpOnly`-cookie, vilket hindrar skript i webbläsaren från att läsa token direkt. Inloggningsrutten begränsar antalet försök med `express-rate-limit`.

## Vad jag lärde mig

SQLite passade bra när jag ville få igång projektet lokalt. Flytten till PostgreSQL lärde mig att tänka på driften tidigare, särskilt på var datan finns efter en omstart. I matchningen fick jag också väga två behov mot varandra. Köparen vill ha ett snabbt svar, medan administratören behöver kunna följa upp förfrågan senare.

[Tillbaka till profilöversikten](../README.md)
