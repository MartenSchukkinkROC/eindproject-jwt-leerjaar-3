# Stappenplan Web API met Identity en JWT
Voordat je toevoegingen gaat doen aan je eigen project, is het goed om eerst 
het voorbeeldproject “WebApiWithJSONWebToken” aan de praat te krijgen en 
te bekijken. Je zult straks deze stappen ook uitvoeren op je eigen project.

## Het voorbeeldproject 

### Opstarten
Om het voorbeeldproject te kunnen gebruiken moet je de volgende stappen uitvoeren:

1. Controleer de ConnectionString ApplicationDbContextConnection in appsettings.json. Pas deze eventueel aan.
1. Voer database migrations uit (Update-Database in Package Manager Console)
1. Start het project: Swagger wordt getoond. Hier zie je een aantal endpoints.
1. Klik /Weatherforcast open
1. Klik “Try it out” en vervolgens “Execute”
1. Je ziet dat de server reageert met een 401 Unauthorized response. Dit komt omdat je geauthentiseerd moet zijn met een gebruiker die de rol “Admin” heeft om deze methode uit te voeren.

### Admin gebruiker aanmaken
We gaan nu de eerste Admin gebruiker aanmaken in de database. Dit doen we als volgt:

1. Comment out de regel ```[Authorize(Roles = UserRoles.Admin)]``` boven de RegisterAdmin methode in de AuthenticateController class. Dit zorgt ervoor dat we deze methode nu even zonder authenticatie uit kunnen voeren.
1. Start het project opnieuw op
1. Klik /Authenticate/register-admin open
1. Klik “Try it out”
1. Vul een gebruikersnaam (“admin”), emailadres en (sterk!) wachtwoord in
1. Klik op “Execute”. Krijg je een foutmelding "User creation failed! Please check user details and try again", dan heb je waarschijnlijk een niet sterk genoeg wachtwoord. Pas dit dan aan.
1. Haal het comment voor de regel [Authorize(Roles = UserRoles.Admin)] boven de RegisterAdmin methode in de AuthenticateController nu weer weg class weg, zodat niet zomaar iemand een admin-gebruiker aan kan maken, maar alleen een andere admin.

### Inloggen als admin
We gaan nu inloggen op de applicatie met de aangemaakte “admin” gebruiker:

1. Start het project op. Swagger verschijnt nu
1. Klik /Authenticate/login open
1. Klik “Try it out”
1. Voer de gebruikersnaam en wachtwoord in die je eerder hebt gebruikt bij het aanmaken
1. Klik op “Execute”. Als het inloggen goed is gegaan, heb je nu een token ontvangen. Knip dit token (zonder aanhalingstekens).
1. Klik “Authorize” boven in het scherm
1. Vul as value in: ```bearer[spatie]<token>```
1. Klik op “Authorize” en vervolgens op “Close”
1. Wanneer je nu naar /Weatherforecast gaat en deze uitvoert, dan zul je zien dat de server nu reageert met een 200 Ok en de weer-data.
 

Wanneer je nu een endpoint aan wil roepen vanuit de frontend, dan is het belangrijk dat je de gebruiker eerst laat inloggen en vervolgens het token onthoudt. Dit token geef je bij elke andere request mee, door een “Authorization” header toe te voegen aan je request:
 

## Jullie eigen project aanpassen
1. We gaan ervanuit dat jullie al een ASP.NET Core Web API project hebben aangemaakt met de standaard instellingen. Zo niet, doe dit dan.
1. Gebruik scaffolding om Identity toe te voegen aan je project
   - Voeg geen layouts toe
   - Stel als DbContext class aan in: ApplicationDbContext
   - Stel als User class in: ApplicationUser
1. Maak een database-migration "Init" aan (Add-Migration) en voer deze door op de database (Update-Database)
1. Controleer de ConnectionString ApplicationDbContextConnection in appsettings.json. Pas deze eventueel aan.
1. Installeer de package Microsoft.AspNetCore.Authentication.JwtBearer, versie 8.0.15 (!)
1. Kopieer de volgende bestanden uit het voorbeeldproject naar jullie eigen project en pas de namespaces en usings in de bestanden aan naar de namespace van jullie project (tip: gebruik de Quick Action "Change namespace to match folder structure"):
   - Authentication\AuthResponse.cs
   - Authentication\LoginModel.cs
   - Authentication\RegisterModel.cs
   - Authentication\UserRoles.cs
   - Controllers\AuthenticateController.cs
   - Controllers\WeatherForecastController.cs (vervang de bestaande)
   - Program.cs (vervang de bestaande)
1. Voeg het volgende toe aan appsettings.json:
   ```json
   "JWT": {
     "ValidAudience": "http://localhost:4200",
     "ValidIssuer": "http://localhost:61955",
     "Secret": " lHisyRAbtzNDQRC7uQTZbA7SRr5bzxUGSqXQ19GhQSOfV"
   }
   ```

    Let op, het secret zou je eigenlijk moeten wijzigen naar een willekeurige andere string om echt veilig te zijn!

1. Build het project. Build het project niet? Controleer dan of je bovenstaande stappen goed hebt doorlopen (let ook vooral op het aanpassen van de namespaces!)
1. Voer de stappen van "Admin gebruiker aanmaken" van het voorbeeldproject uit in je eigen project
1. Voer de stappen van "Inloggen als admin" van het voorbeeldproject uit in je eigen project
1. PROFIT! Je bent nu klaar om je project verder uit te breiden met extra functionaliteiten!


> Mist er een stap in dit stappenplan? Laat het me gerust weten via e-mail, dan pas ik het an!