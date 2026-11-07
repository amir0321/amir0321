# Marginalia

Marginalia är ett personligt bibliotek för lästa böcker och upp till fem favoritcitat. Jag byggde den responsiva webbapplikationen som arbetsprov, med ett Angular-gränssnitt och ett ASP.NET Core-API.

**Teknik** · Angular 20, .NET 9, EF Core, PostgreSQL, Docker och GitHub Actions

[Visa källkoden för Marginalia](https://github.com/amir0321/Marginalia)

## Min roll

Jag tog fram gränssnittet, modellerade böcker och citat och byggde API:et och driftsättningen. Jag lade särskild vikt vid att varje användare bara kommer åt sina egna böcker och att väntan på API:et blir begriplig i gränssnittet.

## Tekniska val

### Åtkomstkontroll i databasfrågan

Bok-API:et kräver autentisering. Vid läsning, ändring och borttagning av en enskild bok filtrerar API:et på både bokens ID och användarens ID från den verifierade JWT-token. Ett bok-ID som tillhör någon annan ger därmed samma `404 Not Found` som ett ID utan träff. Åtkomstkontrollen ligger i ägarfiltret. Det gemensamma svaret begränsar vilken information som lämnas om andra användares böcker.

### Samma ursprung för gränssnitt och API

Jag låter frontend göra anrop till den relativa sökvägen `/api`. Netlify vidarebefordrar anropen till API:et på Render, medan Angulars utvecklingsproxy hanterar samma sökväg lokalt. Det ger frontend en gemensam anropsväg i båda miljöerna och håller webbläsarens anrop till samma ursprung som gränssnittet.

### Tydlig återkoppling vid kallstart

API:et kan behöva starta upp efter en tids inaktivitet. Därför byggde jag `ApiWakeService`, som anropar `GET /api/health` i bakgrunden när appen laddas. Om det dröjer mer än två sekunder att få kontakt får användaren ett meddelande om att servern startar.

### Lokal tillståndshantering i Angular

Jag använde Angular Signals för bland annat tema, inloggningsstatus och filtrering. `signal()` håller reda på aktuellt tillstånd och `computed()` härleder exempelvis vilka böcker som ska visas för det valda filtret.

## Vad jag lärde mig

Jag fick praktisk erfarenhet av att kombinera autentisering med ägarbaserad behörighet och av att anpassa ett gränssnitt till driftsmiljön. Jag satte också upp ett arbetsflöde i GitHub Actions som bygger och testar .NET-API:et och Angular-appen samt kontrollerar att API:ets Docker-avbildning går att bygga.

[Tillbaka till profilöversikten](../README.md)
