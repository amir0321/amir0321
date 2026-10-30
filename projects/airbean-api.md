# AirBean API – Tekniska val, min roll & lärdomar

**Repo:** [github.com/amir0321/airbean-api](https://github.com/amir0321/airbean-api)  
**Stack:** Node.js, Express, SQLite

---

## Bakgrund & min roll i gruppen

AirBean API var ett grupparbete (4 personer) där vi byggde ett REST-API för en kaffekedja som levererar kaffe med drönare. Vi arbetade agilt med Trello, dagliga standups och en strikt Git-regel: ingen pushar direkt till `main`, och minst två gruppmedlemmar måste granska varje Pull Request.

- **Min roll i starten:** Innan vi skrev en enda rad kod ritade jag upp databasdiagrammet så att alla i gruppen var överens om tabellerna och relationerna.
- **Under projektet:** Jag var med och byggde upp arkitekturen (`routes`, `controllers`, `services`, `middleware`), valideringen och order-/kampanjflödet.

## Viktiga beslut vi tog

### 1. Automatisk rabattberäkning på serversidan
I kraven fanns flera kombinationskampanjer (t.ex. *Bryggkaffe + Ostmacka ger 15 % rabatt*, *3 ostmackor ger en på köpet*).
- **Varför vi lade allt i backend:** Vi bestämde att klienten bara ska skicka in en lista med `{ productId, quantity }` till `POST /api/orders`. Backend slår själv upp priserna i databasen, räknar ut vilka kampanjer som matchar och returnerar `baseTotal`, `discountTotal`, `totalPrice` och `appliedDiscounts`. På så sätt kan en användare aldrig manipulera totalpriset i webbläsaren.

### 2. Både gäst- och medlemsbeställningar i samma flöde
- **Varför:** Vi ville att det skulle vara lika enkelt att beställa som gäst som inloggad. Om headern `x-user-id` skickas med kopplas ordern till användarens historik (`GET /api/orders/user/:userId`), annars registreras den som en gästorder men får exakt samma ETA- och statusspårning (`brygger` → `Drönare på väg` → `Levererad`).
