# AirBean API

AirBean API är ett REST-API för en fiktiv kaffekedja som levererar med drönare. Det hanterar meny, beställningar, kampanjrabatter och beräknad leveranstid. Vi byggde API:et i ett team på fyra personer.

**Teknik** · Node.js, Express och SQLite

[Visa källkoden för AirBean API](https://github.com/amir0321/airbean-api)

## Min roll

Jag ritade upp databasens tabeller och relationer så att gruppen hade en gemensam modell att arbeta utifrån. Jag bidrog också till att strukturera Express-koden och till arbetet med validering, orderflöde och kampanjlogik.

## Tekniska val

### Priser och rabatter beräknas på servern

När klienten anropar `POST /api/orders` skickar den produkt-ID och antal för varje rad. API:et validerar orderraderna, hämtar priserna från SQLite och tillämpar aktuella kampanjregler, till exempel rabatt när en kund beställer både bryggkaffe och en ostmacka. Servern returnerar grundbelopp, rabatt och slutpris tillsammans med de kampanjer som användes.

### Ett orderflöde för gäster och användare

Både gästbeställningar och beställningar kopplade till ett användar-ID går genom samma prisberäkning och får en beräknad leveranstid. Klienten kan skicka `x-user-id` för att koppla en order till ett användar-ID. Verifierad identitet skulle kräva ett separat autentiseringssteg.

### Validering och affärsregler i separata lager

Vi delade upp arbetet mellan Express-rutter, validering, controllers och tjänster. Det gav kampanjreglerna en egen plats i koden och gjorde det enklare för gruppen att arbeta parallellt med olika delar av API:et.

## Vad jag lärde mig

Jag fick omsätta kampanjerna i menyn till regler som API:et kan tillämpa konsekvent när det räknar ut en beställning. Databasdiagrammet blev en gemensam referens när vi delade upp arbetet med menyn, beställningarna och kampanjerna.

[Tillbaka till profilöversikten](../README.md)
