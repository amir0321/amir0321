# Övriga projekt

Här finns några mindre projekt där jag provade nya sätt att bygga gränssnitt och jobba med data.

[Tillbaka till profilöversikten](../README.md)

## CodeForge

En tvåspråkig webbplats för digitala tjänster inom webbutveckling och cybersäkerhet. Jag samlade svenska och engelska texter i en gemensam översättningsfil och lät React Context hantera språkbytet i hela gränssnittet.

Kontaktformuläret använder EmailJS. Jag lade till ett dolt fält för att fånga enkla botinlämningar och en minuts väntetid mellan lyckade inskick i samma webbläsare. Båda kontrollerna körs i klienten, där väntetiden hanteras med `localStorage`.

[Se källkoden för CodeForge](https://github.com/amir0321/codeforge)

## NextWatch

En filmapp i React som visar populära och kommande filmer från The Movie Database. Jag lade Axios-anropen i ett eget servicelager och återanvände samma detaljvy via `/movie/:id`. Komponenten `MoviesStatus` samlar återkopplingen för laddning, fel och tomma sökresultat, så att vyerna kan fokusera på att presentera filmerna.

[Se källkoden för NextWatch](https://github.com/amir0321/Nextwatch)

## Taskflow

En fullstackapp för att organisera uppgifter efter prioritet, förfallodatum och status. Efter att ha arbetat med relationsdatabaser ville jag prova dokumentmodellering med MongoDB. I Mongoose definierade jag ett schema där uppgiften har en ägarreferens och ett begränsat antal prioritetsnivåer. Jag övade också på hur ett Express-mellanlager verifierar JWT vid anrop till skyddade delar av API:et.

[Se källkoden för Taskflow](https://github.com/amir0321/TASKFLOW)

## Solaris

En interaktiv visualisering av solsystemet i HTML, CSS och JavaScript. Jag separerade API-anropen från koden som skapar planeterna i DOM:en och hanterar klick. Att först hämta en API-nyckel och sedan använda den för att läsa planetdata gav mig praktisk erfarenhet av asynkrona anrop som beror på varandra.

[Se källkoden för Solaris](https://github.com/amir0321/Solsystem)

## Pärmar & Poesi

En responsiv landningssida för en lokal bokhandel, byggd i en grupp om fem personer. Jag tog fram sidans huvudstruktur och ansvarade för att integrera gruppens sektioner. Det arbetet visade hur gemensamma klassnamn, Sass-variabler och mixins underlättar när flera personer bygger delar av samma gränssnitt.

[Se källkoden för Pärmar & Poesi](https://github.com/amir0321/Parmar-Poesi)

## Modern UX

En landningssida för en SaaS-produkt, byggd med React, Vite och Tailwind CSS. Jag delade upp innehållet i komponenter för bland annat funktioner, priser och kundomdömen. Det gav mig en konkret övning i visuell hierarki och responsiva layouter, från staplade sektioner på mobil till flera kolumner på större skärmar.

[Se källkoden för Modern UX](https://github.com/amir0321/modern-ux)

## Portfolio

Min portfoliowebbplats i React och Vite samlar en kort presentation, projekt och kontaktvägar. Jag gav varje sektion en egen komponent, vilket gör det enklare att uppdatera projektdelen i takt med utbildningen.

[Se källkoden för Portfolio](https://github.com/amir0321/Portfolio)
