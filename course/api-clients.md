# API clients

Een API kan op verschillende manieren aangeroepen worden. Het spreekt dan ook voor zich dat meerdere soorten clients zo'n aanroep kunnen uitvoeren. We bespreken hieronder de drie meest gebruikte clients in onze sector.

Hier zijn drie voorbeelden met eenvoudige API's om cURL, Postman en Python te demonstreren. Voor elke methode gebruiken we een andere API, zodat je verschillende soorten data en interacties kunt zien.

* * *

### 1\. cURL

cURL, wat staat voor _client URL_, is een command line tool voor het doorsturen van data met behulp van verschillende soorten netwerkprotocollen. Op de meeste machines wordt cURL reeds voorgeïnstalleerd, dus je hoeft geen extra stappen te doorlopen om er gebruik van te kunnen maken.

De Random User API genereert willekeurige gebruikersinformatie. Dit is handig om te oefenen met eenvoudige data-opvragingen.

#### Commando:

```
curl https://randomuser.me/api/
```

Wanneer je dit cURL-commando uitvoert, ontvang je een JSON-respons met gegevens over een willekeurige gebruiker, zoals naam, locatie en e-mailadres.

#### Verwachte output:

De JSON-respons zal informatie bevatten zoals:

```
{
  "results": [
    {
      "name": {
        "title": "Mr",
        "first": "John",
        "last": "Doe"
      },
      "email": "john.doe@example.com",
      "location": {
        "city": "New York",
        "state": "New York"
      },
      ...
    }
  ]
}
```

Met deze gegevens kun je oefenen om specifieke informatie zoals naam en locatie uit de JSON te halen.

* * *

### 2\. POSTMAN

Een iets gebruiksvriendelijkere manier om HTTP requests manueel te versturen is via de Postman GUI (_Graphical User Interface_). Postman profileert zichzelf als een platform dat specifiek ontwikkeld werd om APIs te bouwen en te gebruiken. Wij zullen er daarom gretig gebruik van maken om wegwijs te worden in het API landschap. Op [deze webpagina](https://www.postman.com/downloads/) kan je de desktop applicatie downloade

De Random Facts API geeft een willekeurig feit terug, ideaal voor het testen van eenvoudige GET-verzoeken in Postman en het bekijken van HTTP-headers en statuscodes.

1.  Open Postman en maak een nieuw verzoek aan.
2.  Kies het `GET`\-verzoekstype.
3.  Vul de volgende URL in:
    
    ```
    https://uselessfacts.jsph.pl/random.json?language=en
    ```
    
4.  Klik op “Send” om het verzoek te verzenden.

#### Verwachte respons:

De JSON-respons bevat een willekeurig feit, bijvoorbeeld:

```
{
  "id": "98c8b13",
  "text": "A bolt of lightning is 5 times hotter than the surface of the sun.",
  "source": "internet",
  "source_url": "https://uselessfacts.jsph.pl/",
  "language": "en"
}
```

Hiermee kun je in Postman verschillende informatie zoals de statuscode (bijv. 200) bekijken en het `text`\-veld vinden dat het feit bevat. Je kunt in Postman experimenteren met de console om de headers en timinginformatie te zien.

* * *

### 3\. Python code

Als laatste kunnen we ervoor kiezen om HTTP requests vanuit programmacode te sturen. In Python wordt daar meestal de [requests](https://requests.readthedocs.io/en/latest/) bibliotheek voor gebruikt. Wanneer het antwoord van een API bijvoorbeeld in JSON formaat teruggestuurd wordt naar de client, zou die laatste ervoor kunnen kiezen om automatisch bepaalde informatie uit dat antwoord te halen.

De Joke API levert een willekeurige grap terug in JSON-formaat. Dit is handig voor Python, omdat we de JSON kunnen verwerken en de grap op een leuke manier kunnen weergeven.

#### Voorbeeldcode in Python:

```
import requests

# Stuur een GET-verzoek naar de Joke API
response = requests.get('https://official-joke-api.appspot.com/random_joke')
json_response = response.json()

# Haal de grap en de punchline op uit de JSON-respons
setup = json_response['setup']
punchline = json_response['punchline']

# Print de grap
print(f'Joke: {setup}')
input("Press Enter to see the punchline...")
print(f'Punchline: {punchline}')
```

#### Verwachte output:

De JSON-respons bevat de grap in twee delen, zoals:

```
{
  "id": 1,
  "type": "general",
  "setup": "Why did the scarecrow win an award?",
  "punchline": "Because he was outstanding in his field!"
}
```

De Python-code haalt de `setup` en `punchline` op en wacht tot je op Enter drukt om de punchline te tonen. Dit maakt het interactief en geeft je de kans om te oefenen met het verwerken van JSON-data in Python.

Bronnen:

-   [https://en.wikipedia.org/wiki/CURL](https://en.wikipedia.org/wiki/CURL)
-   [https://curl.se/](https://curl.se/)