<properties 
    pageTitle="Strojnog učenja preporuke: Integracija JavaScript | Microsoft Azure" 
    description="Azure strojnog učenja preporuke - JavaScript Integracija - dokumentacija" 
    services="machine-learning" 
    documentationCenter="" 
    authors="LuisCabrer" 
    manager="jhubbard" 
    editor="cgronlun"/>

<tags 
    ms.service="machine-learning" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="javascript" 
    ms.topic="article" 
    ms.date="09/08/2016" 
    ms.author="luisca"/>

# <a name="azure-machine-learning-recommendations---javascript-integration"></a>Azure strojnog učenja preporuke - Integracija JavaScript

>[AZURE.NOTE]Trebali biste početi koristiti Kognitivne servisa za preporuke API umjesto ovu verziju. Servis za preporuke za Kognitivne će zamjene taj servis, a sve nove značajke će razvio postoji. Ima nove mogućnosti kao što su grupnog slanja promjena podršku, bolje Explorer API-JA, izvlačenje, dosljedan prijava/naplata sučelje za čišćenje API, itd.
> Dodatne informacije o [migraciji nove Kognitivne usluge](http://aka.ms/recomigrate)


Ovaj dokument opisuju kako integrirati web-mjesta pomoću JavaScript. JavaScript omogućuje slanje podataka Acquisition događaja i zauzeti preporuke nakon stvaranja preporuke modela. Sve operacije gotovo putem JS također možete učiniti na strani poslužitelja.

[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

##<a name="1-general-overview"></a>1. pregled Općenito
Integriranje web-mjesta s Azure ML preporuke sastojati na 2 faze:

1.  Pošaljite događaje u Azure ML preporuke. To će omogućiti za izradu modela za preporuke.
2.  Korištenje preporuke. Kada se model temelji mogu zauzeti preporuke. (Ovaj dokument objašnjavaju kako stvoriti model, pročitajte vodič za brzi početak rada da biste saznali više o tome kako).


<ins>Faza li</ins>

U prvom fazi umetnete u html stranica small JavaScript biblioteke koja omogućuje stranice da biste poslali događaje kako nastaju na stranici html u Azure ML preporuke poslužitelja (putem Data Market):

![Drawing1][1]

<ins>Faza II</ins>

U drugom fazi kada želite da biste prikazali preporuke na stranici odaberite jednu od sljedećih mogućnosti:

1. poslužitelja (na faza prikaza stranice) poziva Azure ML preporuke Server (putem Data Market) da biste dobili preporuke. Rezultati obuhvatiti popis id stavke. Poslužitelj mora obogaćivanje rezultate sa stavkama Meta podataka (primjerice slike, opis) i poslali stvorene stranice u web-pregledniku.

![Drawing2][2]

2. na drugu mogućnost jest korištenje small JavaScript datoteku iz faza nešto dobit ćete jednostavan popis preporučenih stavki. Primljeni ovdje podaci fleksibilan od onog u prvu mogućnost.

![Drawing3][3]

##<a name="2-prerequisites"></a>2. preduvjeti za

1. Stvaranje nove korištenja API-ji modela. Pogledajte vodič za brzi početak rada na kako to učiniti.
2. Kodiranje vaše &lt;dataMarketUser&gt;:&lt;dataMarketKey&gt; s base64. (Ovo će se koristiti za osnovnu provjeru autentičnosti da biste omogućili JS kod da biste uputili poziv API-ji).


##<a name="3-send-data-acquisition-events-using-javascript"></a>3. slanje podataka Acquisition događaja pomoću JavaScript
Sljedeći koraci olakšavaju slanje događaja:

1.  U kodu uvrstiti JQuery biblioteke. Preuzmite ga iz nuget u sljedeći URL.

        http://www.nuget.org/packages/jQuery/1.8.2
2.  Uključite preporuke Java Script biblioteke s sljedeći URL: http://aka.ms/RecoJSLib1

3.  Pokretanje Azure ML preporuke biblioteka s odgovarajućim parametre.

        <script>
            AzureMLRecommendationsStart("<base64encoding of username:key>",
            "<model_id>");
        </script>

4.  Pošaljite odgovarajuće događaj. U odjeljku detaljne ispod sve vrste događaja (primjer kliknite događaj)

        <script>
            if (typeof AzureMLRecommendationsEvent=="undefined") {      
                        AzureMLRecommendationsEvent = [];
                    }
            AzureMLRecommendationsEvent.push({ event: "click", item: "18321116" });
        </script>


###<a name="31-limitations-and-browser-support"></a>3.1. Ograničenja i podrške za preglednike
Ovo je vodič implementacije i izražen je kao što je. Trebali biste ga podržavati sve važnije preglednike.

###<a name="32-type-of-events"></a>3,2. Vrsta događaja
Postoje 5 vrste događaja koji podržava biblioteku: kliknite, preporuke, dodavanje košaricu pogon uklonili s web-mjesto pogon košaricu i u okvir za kupnju. Postoji dodatni događaja koji se koristi za postavljanje konteksta korisnika koji se naziva prijava.

####<a name="321-click-event"></a>3.2.1. Kliknite događaj
Ovaj događaj treba koristiti bilo kada korisnik klikne na stavci. Obično kad korisnik klikne na stavci nove stranice je otvorena s detalje o stavci; na ovoj stranici moraju se pokrenuti ovaj događaj.

Parametri:
- događaj (niz znakova, obavezno) – "kliknite"
- stavke (niz znakova, obavezno) – Jedinstveni identifikator stavke
- itemName (niz znakova, nije obavezno) – naziv stavke
- itemDescription (niz znakova, nije obavezno) – opis artikla
- itemCategory (niz znakova, nije obavezno) – kategorije stavke
        
        <script>
            if (typeof AzureMLRecommendationsEvent == "undefined") { AzureMLRecommendationsEvent = []; }
            AzureMLRecommendationsEvent.push({ event: "click", item: "3111718" });
        </script>

Ili neobavezno podacima:

        <script>
            if (typeof AzureMLRecommendationsEvent === "undefined") { AzureMLRecommendationsEvent = []; }
            AzureMLRecommendationsEvent.push({event: "click", item: "3111718", itemName: "Plane", itemDescription: "It is a big plane", itemCategory: "Aviation"});
        </script>


####<a name="322-recommendation-click-event"></a>3.2.2. Preporuka kliknite događaj
Ovaj događaj treba koristiti bilo kojem trenutku korisnik nakon klika na stavci koja je poslao Azure ML preporuke kao preporučene stavke. Obično kad korisnik klikne na stavci nove stranice je otvorena s detalje o stavci; na ovoj stranici moraju se pokrenuti ovaj događaj.

Parametri:
- događaj (niz znakova, obavezno) – "recommendationclick"
- stavke (niz znakova, obavezno) – Jedinstveni identifikator stavke
- itemName (niz znakova, nije obavezno) – naziv stavke
- itemDescription (niz znakova, nije obavezno) – opis artikla
- itemCategory (niz znakova, nije obavezno) – kategorije stavke
- seeds (niz polje, nije obavezno) – sjemenke koji je generirao upit za preporuke.
- recoList (niz polje, nije obavezno) – rezultat zahtjeva za preporuke koji je generirao stavku koju ste kliknuli.
        
        <script>
            if (typeof AzureMLRecommendationsEvent=="undefined") { AzureMLRecommendationsEvent = []; }
            AzureMLRecommendationsEvent.push({event: "recommendationclick", item: "18899918" });
        </script>

Ili neobavezno podacima:

        <script>
            if (typeof AzureMLRecommendationsEvent == "undefined") { AzureMLRecommendationsEvent = []; }
            AzureMLRecommendationsEvent.push({ event: eventName, item: "198", itemName: "Plane2", itemDescription: "It is a big plane2", itemCategory: "Default2", seeds: ["Seed1", "Seed2"], recoList: ["199", "198", "197"]               });
        </script>


####<a name="323-add-shopping-cart-event"></a>3.2.3. Dodavanje kupovinu košaricu događaja
Ovaj događaj treba koristiti kada korisnik dodavanje stavke u košaricu.
Parametri:
* događaj (niz znakova, obavezno) – "addshopcart"
* stavke (niz znakova, obavezno) – Jedinstveni identifikator stavke
* itemName (niz znakova, nije obavezno) – naziv stavke
* itemDescription (niz znakova, nije obavezno) – opis artikla
* itemCategory (niz znakova, nije obavezno) – kategorije stavke
        
        <script>
            if (typeof AzureMLRecommendationsEvent == "undefined") { AzureMLRecommendationsEvent = []; }
            AzureMLRecommendationsEvent.push({event: "addshopcart", item: "13221118" });
        </script>

####<a name="324-remove-shopping-cart-event"></a>3.2.4. Uklanjanje košarice košaricu događaja
Ovaj događaj treba koristiti kada korisnik uklanja stavke u košarici.

Parametri:
* događaj (niz znakova, obavezno) – "removeshopcart"
* stavke (niz znakova, obavezno) – Jedinstveni identifikator stavke
* itemName (niz znakova, nije obavezno) – naziv stavke
* itemDescription (niz znakova, nije obavezno) – opis artikla
* itemCategory (niz znakova, nije obavezno) – kategorije stavke
        
        <script>
            if (typeof AzureMLRecommendationsEvent=="undefined") { AzureMLRecommendationsEvent = []; }
            AzureMLRecommendationsEvent.push({ event: "removeshopcart", item: "111118" });
        </script>

####<a name="325-purchase-event"></a>3.2.5. Za kupnju događaja
Ovaj događaj treba koristiti kada korisnik kupili njegovog košaricu.

Parametri:
* događaj (niz) – "kupiti"
* stavki (kupili []) - polja držite unos za svaku stavku kupili.<br><br>
Kupljeni oblik:
    * Stavka (niz) – Jedinstveni identifikator stavke.
    * Count (int ili niz) – broj stavki koje ste kupili.
    * cijena (plutati ili niz) – neobavezna polja – cijenu stavke.

U sljedećem primjeru pokazuje kupnju od 3 stavke (33 34, 35), dva s sva polja popunjeno (stavke, broj, cijena) i jedan (stavke 34) bez cijena.

        <script>
            if ( typeof AzureMLRecommendationsEvent == "undefined"){ AzureMLRecommendationsEvent = []; }
            AzureMLRecommendationsEvent.push({ event: "purchase", items: [{ item: "33", count: "1", price: "10" }, { item: "34", count: "2" }, { item: "35", count: "1", price: "210" }] });
        </script>

####<a name="326-user-login-event"></a>3.2.6. Događaj Prijava korisnika
Azure biblioteke ML preporuke događaja stvara i korištenje kolačić prepoznavanje događaje koje ste dobili s istom pregledniku. Da biste poboljšali rezultate modela Azure ML preporuke omogućuje da biste postavili korisnika jedinstveni identifikacijski koji će nadjačati korištenje kolačića.

Ovaj događaj treba koristiti nakon Prijava korisnika na web-mjesto.

Parametri:
* događaj (niz) – "userlogin"
* korisnik (niz) – jedinstveni identifikacijski korisnika.
        <script>
  Ako (typeof AzureMLRecommendationsEvent == "Nedefinirana") {AzureMLRecommendationsEvent = [];} AzureMLRecommendationsEvent.push ({događaja: "userlogin" korisnika: "ABCD10AA"});      </script>

##<a name="4-consume-recommendations-via-javascript"></a>4. zauzeti preporuke putem JavaScript
Kod koji se koristi za preporuke aktivira neki događaj JavaScript klijenta web-stranice. Preporuka odgovor sadrži preporučene stavke ID-a, njihova imena i njihove ocjene. Preporučuje se da biste koristili tu mogućnost samo za prikaz popisa preporučene stavke – složenije rukovanje (kao što su dodavanje metapodataka stavke) računajući o integraciji strani poslužitelja.

###<a name="41-consume-recommendations"></a>4.1 zauzeti preporuke
Za preporuke morate obuhvaćaju potrebna JavaScript biblioteke na stranici i da biste nazvali AzureMLRecommendationsStart. U odjeljku 2.

Za preporuke za jednu ili više stavki koje morate nazvati metode pod nazivom: AzureMLRecommendationsGetI2IRecommendation.

Parametri:
* stavke (polje nizova) – da biste dobili preporuke za jednu ili više stavki. Ako je Fbt Sastavi zauzeti, a zatim možete postaviti ovdje samo jednu stavku.
* numberOfResults (int) – broj potrebnih rezultata.
* includeMetadata (Booleove vrijednosti, nije obavezno) – ako je postavljeno na "true" označava polja metapodataka moraju biti ispunjena u rezultatu.
* Obrada funkcija - funkciju koja će se rukovati preporuke vraća. Podaci se vraćaju kao polja:
    * Stavka – stavka jedinstveni id
    * Naziv - stavku name (Ako postoji u katalogu)
    * Ocjena – ocjena preporuka
    * Metapodaci - niz koji predstavlja metapodataka stavke

Primjer: Sljedeći kod zahtjeva 8 preporuke za stavku "64f6eb0d-947a-4c18-a16c-888da9e228ba" (i tako da ne navedete includeMetadata - implicitno piše da je potrebno nema metapodataka), on zatim concatenate rezultate u međuspremnika.

        <script>
            var reco = AzureMLRecommendationsGetI2IRecommendation(["64f6eb0d-947a-4c18-a16c-888da9e228ba"], 8, false, function (reco) {
                var buff = "";
                for (var ii = 0; ii < reco.length; ii++) {
                    buff += reco[ii].item + "," + reco[ii].name + "," + reco[ii].rating + "\n";
                }
                alert(buff);
            });
        </script>


[1]: ./media/machine-learning-recommendation-api-javascript-integration/Drawing1.png
[2]: ./media/machine-learning-recommendation-api-javascript-integration/Drawing2.png
[3]: ./media/machine-learning-recommendation-api-javascript-integration/Drawing3.png
 
