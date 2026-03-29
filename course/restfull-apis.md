# RESTFULL APIs

## 1\. Wat is een API?

Een **API** (Application Programming Interface) is een set van regels en protocollen waarmee verschillende softwaretoepassingen met elkaar kunnen communiceren. Denk aan een API als een soort "tolk" die de communicatie tussen verschillende applicaties of systemen mogelijk maakt. Met API's kunnen ontwikkelaars functies van een externe applicatie of dienst gebruiken zonder dat ze de onderliggende code hoeven te kennen.

### Voorbeelden van API's in het dagelijks leven

-   **Weerapps op smartphones**: Wanneer je de weersvoorspelling op je telefoon bekijkt, haalt de weerapp real-time gegevens op via een API van een weerdienst. Dit stelt de app in staat om actuele temperaturen, voorspellingen en weersomstandigheden te tonen.
    
-   **Inloggen met sociale media**: Veel websites en apps bieden de optie om in te loggen met je Facebook-, Google- of Twitter-account. Dit gebeurt via de API's van deze sociale mediaplatforms, waardoor de website toegang krijgt tot je basisprofielinformatie (met jouw toestemming) voor een eenvoudige registratie.
    
-   **Kaarten en navigatie**: Navigatie-apps zoals Google Maps, Waze of Apple Kaarten gebruiken API's om kaartgegevens, verkeersinformatie en locatieservices te leveren. Wanneer je een route plant, communiceren deze apps met verschillende API's om je de beste route en actuele verkeersupdates te geven.
    
-   **Online betalingen en bankieren**: Bij het gebruik van betaalapps zoals PayPal, Bancontact of je mobiele bankapp maken deze diensten gebruik van API's om veilig transacties uit te voeren. Ze verbinden met banken en betalingsnetwerken om saldo’s te controleren, overboekingen te doen en transacties te verifiëren.
    
-   **Slimme thuisapparaten en spraakassistenten**: Apparaten zoals slimme thermostaten, verlichting en spraakassistenten (zoals Amazon Alexa of Google Assistant) communiceren via API's. Bijvoorbeeld, wanneer je je spraakassistent vraagt om muziek af te spelen, gebruikt deze API's om verbinding te maken met muziekstreamingdiensten zoals Spotify.
    

![](https://youtu.be/-MTSQjw5DrM)

## 2\. Web-API's en REST: Een Overzicht

Een **Web-API** is een API die communiceert via internet, waarbij gegevens worden opgevraagd of verzonden tussen een client (bijvoorbeeld een webbrowser) en een server. Veel van deze web-API's gebruiken de **REST**\-architectuur, die bekend staat om zijn eenvoud en efficiëntie.

### Wat is REST?

REST staat voor **Representational State Transfer** en is een reeks principes voor het ontwerpen van netwerktoepassingen. RESTful web-API's werken met een client-server model waarin de client verzoeken stuurt naar de server en de server antwoorden terugstuurt. REST is populair omdat het een standaard, schaalbare en goed leesbare manier biedt om API's te ontwerpen.

De vijf belangrijkste principes van REST zijn:

1.  **_Uniform interface_**: Resources van de server worden op een eenduidige manier aangesproken.
2.  **_Client–server_**: Client en server leven apart van elkaar zonder enige afhankelijkheid. De client hoeft enkel op de hoogte te zijn van de beschikbare resource URLs, de server is verantwoordelijk voor de verdere invulling.
3.  **_Stateless_**: De server bewaart geen informatie over voorgaande interacties met de client. Elke aanvraag wordt dus als een volledig nieuw behandeld.
4.  **_Cacheable_**: Een API moet in staat zijn om bepaalde gegevens (vb. frequent geraadpleegde data) tijdelijk op te slaan op een sneller dan gebruikelijk medium en zo de prestaties voor de client aanzienlijk te verbeteren.
5.  **_Layered system_**: De architectuur van de API kan opgedeeld worden in verschillende logische lagen met elk hun eigen verantwoordelijkheid. In sommige situaties is het bijvoorbeeld mogelijk om een API te voorzien vanuit één server, de resources te laden vanuit een andere en gebruikers te laten authenticeren door een derde server.

## 1\. Uniform Interface

**Wat betekent dit?** Alle communicatie met de API gebeurt via dezelfde, voorspelbare regels. Je gebruikt standaard HTTP-methoden en consistente URL-structuren.

**Concrete voorbeelden:**

```
# GOED - Uniform interface
GET    /api/devices/123           # Ophalen van device
POST   /api/devices               # Nieuw device aanmaken
PUT    /api/devices/123           # Device updaten
DELETE /api/devices/123           # Device verwijderen

# SLECHT - Niet uniform
GET    /api/getDevice?id=123
POST   /api/createNewDevice
POST   /api/updateDevice123
GET    /api/removeDevice/123
```

**IoT voorbeeld - Slim gebouw:**

```
# Temperatuursensoren beheren
GET    /api/sensors/temperature/floor-2-room-5
PUT    /api/sensors/temperature/floor-2-room-5
DELETE /api/sensors/temperature/floor-2-room-5

# Verlichting beheren
GET    /api/actuators/lights/floor-2-room-5
POST   /api/actuators/lights/floor-2-room-5/actions
```

Elke resource heeft **één duidelijke URL** en je weet meteen wat elke HTTP-methode doet.

* * *

## 2\. Client-Server

**Wat betekent dit?** Client en server zijn volledig gescheiden. De server weet niet wie de client is (mobiele app, website, Node-RED, ...) en de client hoeft niet te weten hoe de server zijn data opslaat of verwerkt.

**Concrete voorbeelden:**

**Server kant:**

```
// De server geeft alleen data terug, geen HTML of UI-code
app.get('/api/sensors/:id', (req, res) => {
  const sensor = database.getSensor(req.params.id);
  res.json(sensor);  // Alleen data!
});
```

**Verschillende clients kunnen dezelfde API gebruiken:**

```
// Node-RED flow
[inject] → [http request: GET /api/sensors/temp1] → [debug]

// Python script
response = requests.get('https://api.example.com/sensors/temp1')

// React webapp
fetch('/api/sensors/temp1').then(res => res.json())
```

**IoT voorbeeld - Samsonite koffer:**

-   De **server** bewaart locatiedata, batterijniveau, lock-status
-   **Client 1**: Smartphone app toont koffer op kaart
-   **Client 2**: Desktop website toont reishistoriek
-   **Client 3**: Node-RED dashboard toont real-time status

Alle drie gebruiken dezelfde API, maar presenteren de data anders!

* * *

## 3\. Stateless

**Wat betekent dit?** De server "vergeet" je na elk verzoek. Elke request moet **alle nodige informatie** bevatten.

**Concrete voorbeelden:**

**FOUT - Stateful (server onthoudt dingen):**

```
// Request 1: Gebruiker logt in
POST /api/login
{ "username": "vinnie", "password": "xxx" }

// Request 2: Server "weet" nog wie je bent
GET /api/sensors  // ❌ Server gebruikt sessie van vorige request
```

**GOED - Stateless:**

```
// Request 1: Login en krijg token
POST /api/login
{ "username": "vinnie", "password": "xxx" }
→ Response: { "token": "eyJhbGc..." }

// Request 2: Stuur token mee in ELKE request
GET /api/sensors
Headers: { "Authorization": "Bearer eyJhbGc..." }  // ✅ Volledig zelfstandige request
```

**Waarom is dit belangrijk?**

-   Load balancers kunnen requests naar verschillende servers sturen
-   Server crashes = geen verloren sessies
-   Simpeler om te debuggen

* * *

## 4\. Cacheable

**Wat betekent dit?** De API vertelt de client: "Deze data mag je X tijd bewaren voordat je opnieuw moet vragen."

**Concrete voorbeelden:**

**Server stuurt cache-instructies mee:**

```
GET /api/sensors/temperature/room-5

Response:
HTTP/1.1 200 OK
Cache-Control: max-age=60  ← "Deze data blijft 60 seconden geldig"
Content-Type: application/json

{
  "temperature": 22.5,
  "timestamp": "2025-11-17T10:30:00Z"
}
```

**IoT voorbeeld - Slim gebouw:**

```
// Ruimte-informatie verandert zelden
GET /api/rooms/floor-2-room-5
Cache-Control: max-age=3600  // 1 uur

// Temperatuur verandert vaak
GET /api/sensors/temperature/floor-2-room-5
Cache-Control: max-age=30  // 30 seconden

// Live sensor data → GEEN cache
GET /api/sensors/temperature/floor-2-room-5/live
Cache-Control: no-cache
```

**Voordelen:**

-   Minder belasting op server
-   Snellere response voor client
-   Lagere datakosten (belangrijk voor IoT!)

* * *

## 5\. Layered System

**Wat betekent dit?** De API kan uit meerdere lagen bestaan. De client weet niet of hij rechtstreeks met de server praat of via tussenliggende systemen.

**Concrete voorbeelden:**

**Architectuur voorbeeld:**

```
[Client: Node-RED]
      ↓
[Layer 1: Load Balancer] ← Verdeelt verkeer
      ↓
[Layer 2: API Gateway] ← Authenticatie, rate limiting
      ↓
[Layer 3: Backend Server] ← Business logic
      ↓
[Layer 4: Database Server] ← Data opslag
```

**IoT voorbeeld - Smart building met beveiliging:**

```
// Client doet simpel request
GET /api/sensors/temperature/room-5

// Maar achter de schermen:
1. Request komt aan bij Reverse Proxy (nginx)
   → Controleert SSL certificaat

2. Doorgestuurd naar API Gateway
   → Valideert API key
   → Controleert rate limits (max 100 req/min)

3. Doorgestuurd naar Authentication Server
   → Verifieert JWT token
   → Checkt gebruikersrechten

4. Doorgestuurd naar Application Server
   → Haalt sensor data op

5. Data komt van Caching Layer
   → Of rechtstreeks van IoT Database
```

**Voordelen:**

-   Beveiliging op meerdere niveaus
-   Makkelijk schaalbaar (extra servers toevoegen)
-   Onderhoudswerken zonder downtime (één laag offline, rest werkt door)[](https://restfulapi.net/rest-architectural-constraints/)

Bronnen:

-   [https://nl.wikipedia.org/wiki/Application\_programming\_interface](https://nl.wikipedia.org/wiki/Application_programming_interface)
-   [https://microservices.io/](https://microservices.io/)
-   [https://en.wikipedia.org/wiki/Web\_API](https://en.wikipedia.org/wiki/Web_API)
-   [https://restfulapi.net/rest-architectural-constraints/](https://restfulapi.net/rest-architectural-constraints/)