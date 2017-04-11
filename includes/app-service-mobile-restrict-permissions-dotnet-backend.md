
Prema zadanim postavkama API-ji u mobilnu aplikaciju pozadinskog možete pozvati anonimno. Ćete morati ograničiti pristup samo čija je autentičnost provjerena klijente.  

+ **Node.js pozadinskog (putem portala)** :  
    
    U mobilnu aplikaciju **Postavke**, kliknite **Jednostavno tablice** i odaberite tablicu. Kliknite **Promijeni dozvole**, odaberite **potvrda autentičnosti pristup samo** za sve dozvole, a zatim kliknite **Spremi**. 

+ **.NET pozadinskog (C#)**:  

    U programu project server dođite do **kontrolera** > **TodoItemController.cs**. Dodavanje u `[Authorize]` atribut klase **TodoItemController** na sljedeći način. Da biste ograničili pristup samo određenim metode, možete primijeniti taj atribut samo za ove metode umjesto klasu. Ponovno objaviti server project.


        [Authorize]
        public class TodoItemController : TableController<TodoItem>

+ **Node.js pozadinskog (putem Node.js kod)** :  
    
    Da biste zahtijeva provjeru autentičnosti za pristup tablicu, dodajte sljedeći redak poslužiteljsku skriptu Node.js:


        table.access = 'authenticated';

    Dodatne informacije potražite u priručniku [Kako: Traži provjeru autentičnosti za pristup tablice](../articles/app-service-mobile/app-service-mobile-node-backend-how-to-use-server-sdk.md#howto-tables-auth). Da biste saznali kako preuzeti projekt koda za brzi početak rada s web-mjesta, u odjeljku [Kako: preuzimanje Node.js pozadinskog brzi početak rada s kodom projekta brojka](../articles/app-service-mobile/app-service-mobile-node-backend-how-to-use-server-sdk.md#download-quickstart).

