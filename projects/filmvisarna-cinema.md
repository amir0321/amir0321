# Filmvisarna Cinema

Filmvisarna är ett grupprojekt där vi byggde ett bokningssystem för en biograf. Besökare kan hitta en visning, välja platser på en interaktiv salongskarta, förbeställa snacks och avboka biljetter. Personalen har en separat vy för filmer, salonger, visningar och bokningar.

**Teknik** · React, TypeScript, Vite, React Router, Bootstrap, SCSS, Express, PostgreSQL och Zod

[Visa källkoden för Filmvisarna Cinema](https://github.com/amir0321/filmvisarna-cinema)

## Min roll

Jag tog ansvar för delar av arkitekturen mellan frontend och backend och bidrog till att definiera vilka data de utbyter. Jag omsatte också Figma-designen i återanvändbara React-komponenter och gemensamma designvärden i SCSS. Mitt arbete omfattade platskartan och delar av boknings- och avbokningsflödena.

## Tekniska val

### En bokning per plats och visning

Två besökare kan ha samma lediga plats framför sig på skärmen. Vi lät därför databasen avgöra vem som faktiskt får platsen. Bokningen skapas i en PostgreSQL-transaktion, och biljettabellens sammansatta primärnyckel på `screening_id` och `seat_id` gör kombinationen av visning och plats unik. Om en annan bokning hinner ta platsen rullas transaktionen tillbaka och API:et svarar med `409 Conflict`.

### Platskarta från salongens data

Vi byggde platskartan i React och SCSS. Kartan grupperar platserna efter rad och skiljer mellan lediga, valda och bokade platser. Samma modell kan representera salonger med olika antal rader och platser.

### Separata rutter för besökare och personal

Frontend deklarerar sina rutter uttryckligen i React Router och samlar publikens sidor och personalens sidor under varsin layout. Det gör det lättare att se vilka sidor som hör till respektive del av applikationen.

### Avbokning via bekräftelsemejlet

Gäster får en länk till `/avboka` med en signerad token. Sidan visar först uppgifter om bokningen och ber sedan gästen bekräfta avbokningen. API:et verifierar token och kontrollerar tidsgränsen. Med standardinställningen går det att avboka fram till två timmar före visningen. Direkt uppslag på bokningsreferens kräver administratörsbehörighet.

## Vad jag lärde mig

Bokningsflödet gav mig ett konkret exempel på hur frontend, API och databas delar på ansvaret. Gränssnittet visar aktuell tillgänglighet, databasen upprätthåller regeln vid samtidiga bokningar och API:et beskriver konflikten. Jag fick också arbeta med hur visningstider och datumfilter tolkas konsekvent mellan databasen och gränssnittet.

[Tillbaka till profilöversikten](../README.md)
