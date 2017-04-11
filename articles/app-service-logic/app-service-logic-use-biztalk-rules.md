<properties
   pageTitle="Dodatne informacije o i stvaranje aplikacije za BizTalk pravila API-JA u aplikaciji logike | Microsoft Azure"
   description="U ovoj se temi pokriva značajke pravila BizTalk poveznika i uputama njegova korištenja"
   services="logic-apps"
   documentationCenter=".net,nodejs,java"
   authors="anuragdalmia"
   manager="erikre"
   editor=""/>

<tags
   ms.service="logic-apps"
   ms.devlang="multiple"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="integration"
   ms.date="04/20/2016"
   ms.author="andalmia"/>

#<a name="biztalk-rules"></a>BizTalk pravila

[AZURE.INCLUDE [app-service-logic-version-message](../../includes/app-service-logic-version-message.md)]

Poslovna pravila encapsulates pravila i odluke koje kontroliraju poslovnih procesa. Ta pravila možda formally definirane u priručnike postupak, ugovore ili ugovore ili možda postoji kao znanja i stručna znanja embodied u zaposlenika. Ta pravila dinamične i mijenjati iznad vremena krajnji promjene u poslovne planove, propisa ili drugih razloga.

Implementacijom ta pravila u tradicionalni programskog jezika zahtijeva znatno vrijeme i koordinaciji i omogućite koje nisu-programere da biste sudjelovali u stvaranja i održavanje pravila za tvrtke. BizTalk poslovna pravila omogućuje brzo implementirati ta pravila i decouple ostatak poslovni proces. Time se omogućuje za podnošenje potrebne promjene pravila tvrtke bez utjecaja ostatak poslovni proces.

##<a name="why-rules"></a>Zašto pravila

Postoje 3 ključa razloga za korištenje BizTalk poslovna pravila u poslovnog procesa:

* Decouple poslovne logike iz aplikacije koda
- Dopusti poslovni analitičar analitičar podataka da bi veću kontrolu nad poslovne logike upravljanja
+ Promjena poslovne logike otvorite radni brže

##<a name="rules-concepts"></a>Pojmovi pravila

##<a name="vocabulary"></a>Rječnik

_Rječnik_ je zbirka definicije koji se sastoji od neslužbeni naziva za računalno objekte koji se koriste u pravilo uvjete i akcije. Pojmovnik definicije olakšati pravila za čitanje, razumijevanje i zajednički koristiti tako da osobe za određenu poslovnu domenu.

Vocabularies osmišljeni su za premostiti razmak između semantiku tvrtke i implementaciju. Povezivanje podataka za status odobrenja možda, na primjer, pokažite na određene stupca određenih retka u određenim baze podataka, predstavljen kao SQL upita. Umjesto umetanja ovu vrstu složene predstavljanje u pravilu, umjesto toga možda stvara definiciju rječnik pridružen tom povezivanje podataka s neslužbeni naziv "Stanje". Naknadno možete obuhvatiti "Status" bilo koji broj pravila, a modul pravilo možete dohvatiti odgovarajuće podatke iz tablice.

##<a name="rule"></a>Pravila

Poslovna pravila su deklarativno naredbe koje upravljaju vođenje poslovnih procesa. Pravilo se sastoji od uvjeta i akcije. Procjenjuje uvjet, a ako se vrednuje kao true, engine pravilo pokreće jednu ili više akcija.
Pravila u Framework pravila tvrtke definiraju pomoću sljedećih oblika:

_IF_ _uvjet_ _ZATIM_ _Akcija_

Razmislite o sljedećem primjeru:

*Iznos ako je manja od ili jednaka na dostupna sredstva*  
*ZATIM vođenje transakcije i ispis potvrda*

To pravilo određuje hoće li se transakcije provoditi primjenom poslovne logike u obrazac za usporedbu dviju novčane vrijednosti, u obliku transakcije iznos i dostupna sredstva.
Poslovna pravila možete koristiti za stvaranje, izmjena i implementacija poslovna pravila. Osim toga, prethodni zadatke možete izvršiti programski.

###<a name="conditions"></a>Uvjeta

Uvjet je na true i false (Booleovih) izraza koji se sastoji od jednog ili više predikati.
U našem primjeru predikata manje od ili jednako primjenjuje se na iznos i dostupna sredstva. Ovaj uvjet će uvijek vrednovani kao true ili false.
Predikati možete kombinirati pomoću logičkih operatora AND, OR, a ne obrasca logički izraz koji je potencijalno baš velik, ali će uvijek vrednovani kao true ili false.

###<a name="actions"></a>Akcija

Akcije su funkcionalne rezultate Procjena uvjeta. Ako se ispuni uvjet pravila, pokreće se odgovarajuće akcije ili akcije.
U našem primjeru "vođenje transakcije" i "ispis potvrda" su postupaka koji se izvršavaju prilikom i samo kada se, uvjet (u tom slučaju "Ako je iznos manja od ili jednaka na dostupna sredstva").
Akcije prikazani su u Framework pravila tvrtke izvođenjem operacije skup XML dokumente.

##<a name="policy"></a>Pravila

Pravilo je logički grupiranja pravila. Sastavite pravilo, spremite ga, ga testirate i, kada ste zadovoljni rezultatom, koristiti u radnom okruženju.

###<a name="policy-composition"></a>Sastavljanje pravila

Možete sastaviti pravila na portalu pravila. Pravilo može sadržavati arbitrarily velikog skupa pravila, ali obično sastavite pravila iz pravila koja se odnose na određene poslovnu domenu unutar konteksta aplikacije koja će koristiti pravila.

###<a name="policy-testing"></a>Testiranje pravila
Učinkovito možete izvršiti probno Pokreni vaše pravila prije nego što se koristi u okruženju proizvodnje. Portal za pravila omogućuje vam dati unosi u pravilo, pokrenite pravila i prikaz rezultat. Rezultat obuhvaća zapisnike, izvođenja pravilo, a zatim Procjena uvjeta, i konačne izlaza.

##<a name="sample-scenario---insurance-claims"></a>Uzorak scenarija - osiguranja zahtjevima
Pogledajmo potrajati uzorak scenarija i voditi kroz nju ne možemo sastavite poslovne logike za isti.

![Zamjenski tekst][1]

U scenariju s za really simple osiguranja zahtjevima za Claimant šalje svoj osiguranja zahtjeva (putem klijenta kao što je web-mjesto, telefonski aplikacije i Dr). Zahtjev zahtjeva dobiva poslane na poslovne jedinice u obrade zahtjeva i na temelju rezultata obrade, zahtjeva može biti bilo odobreno odbačeno ili šalje duž za daljnje Ručna obrada.
Zahtjeva obrade jedinice u našem scenariju bi jedan encompassing poslovne logike za sustav. Izvršava detaljnije Upoznavanje jedinicu, možemo vidjeti sljedeće:

![Zamjenski tekst][2]

Javite nam sada pomoću pravila tvrtke možete implementirati ovaj poslovne logike.


##<a name="creation-of-rules-api-app"></a>Stvaranje pravila Api aplikacije


1. Prijavite se na Portal za Azure
2. Odaberite novi -> Marketplace, a zatim traži *BizTalk pravila*
3. Odaberite pravila BizTalk s popisa rezultata. Otvorit će se plohu BizTalk pravila
4. Odaberite gumb *Stvori* ![zamjenski tekst][3]
1. U novi plohu koji će se otvoriti unesite sljedeće podatke:  
    1. Naziv – dati naziv aplikacije API pravila
    1. Aplikacije servisa za planiranje – odabir ili stvaranje je nove aplikacije servisa planiranje
    1. Cijene sloju – odabir cijene sloju želite da se nalaze u ovu aplikaciju
    1. Grupa resursa – odaberite ili stvorite gdje treba nalaziti aplikacije u grupu resursa
    2. Pretplata - odaberite pretplatu koju želite koristiti
    1. Mjesto – odaberite geografsku lokaciju na kojoj želite aplikaciju da biste se uvesti.
4.  Odaberite *Stvori*. Za nekoliko minuta BizTalk pravila API aplikacije stvorit će se.

##<a name="vocabulary-creation"></a>Stvaranje rječnik
Kada stvorite aplikaciju API pravila za BizTalk, na sljedeći korak bi da biste stvorili vocabularies. Na očekivanja da je razvojni uobičajene osobi za taj ovu vježbu. Evo kako to učiniti:


1. Pokretanje sustava BizTalk pravila API aplikacije na portalu tako da Pregledaj -> aplikacije API - ><Your Rules API App>. To će vam na nadzornu ploču aplikacije API pravila slično ispod:

   ![Zamjenski tekst][4]

2. odaberite "Vokabular definicije". To će vam pokazati 3. rječnik za izradu zaslona odaberite "Dodaj" da biste započeli s dodavanjem novih definicija rječnik.
Postoje 2 vrste rječnik definicija trenutno podržava – slova i XML.

##<a name="literal-definition"></a>Definicija literala
1.  Nakon klika na "Dodaj", novi plohu "Dodavanje definicija" otvara. Unesite sljedeće vrijednosti
  1.    Naziv – samo alfanumeričke znakove se očekuje bez posebne znakove. To također mora biti jedinstven postojeći popis definicija rječnik.
  2.    Opis – neobavezna polja.
  3.    Vrstu definicije – postoje 2 vrste podržane. U ovom primjeru odaberite Literal
  4.    Vrsta podataka – to korisnicima omogućuje da biste odabrali vrstu podataka definiciju. Trenutno su podržane vrste podataka 4: li.  Niz – te vrijednosti moraju biti unesena u dvostruke navodnike ("primjer niz")  
    ii. Booleova – to može biti true ili false  
    III.    Broj – može biti bilo koji decimalni broj  
    IV. DateTime – to znači da je u zadani vrste vrste datuma. Podaci moraju se unijeti koristite ovaj oblik – mm/dd/gggg hh AM\PM  
  5. Unos – to je koji unosite vrijednost definicije. Ovdje unesen vrijednosti mora biti usklađen sa odabranom vrstom podataka. Ili možete unijeti jednu vrijednost iz skupa vrijednosti odvojene zarezima ili raspon vrijednosti pomoću u ključne riječi *Da biste*. Na primjer, možete unijeti jedinstvenu vrijednost 1; skup 1; 2; 3; ili raspon 1 do 5. Imajte na umu da se raspon podržano samo za brojeve.
  6. Odaberite *u redu*.

![Zamjenski tekst][5]
##<a name="xml-definition"></a>XML definicija
Ako je vrsta rječnik odabran kao XML, sljedeće unose mora biti navedena  
  na.    Sheme – klikanja na ovoj otvorit će se novi korisnik allowing plohu ili odaberite s popisa već prenesene sheme ili dopuštanja da biste prenijeli novi.
b.    XPATH – to unos dobije otključana tek nakon što odaberete shemu u prethodnom koraku. Klikom na taj će se prikazivati sheme koja nije potvrđen, a zatim odaberite čvor čijim definiciju rječnik treba stvoriti korisniku omogućuje.  
  c.    FACT – taj unos označava objekt podataka s kojim se želite umeće za modul pravila za obradu. Ovo je naprednih svojstava i po zadanom postavljena na nadređenom odabrane izraza XPATH. FACT postaje osobito važno za prikupljanje i Ulančavanje scenarije.

![Zamjenski tekst][6]

### <a name="add-bulk"></a>Skupno Dodaj
Gore navedeni koraci su zabilježene sučelje za stvaranje definicije rječnik. Nakon stvaranja, definicije stvoreni rječnik pojavit će se obrazac popisa. Postoje preduvjete za moći generirati višestruke definicije iz na istu shemu umjesto ponavljajuće svakog jedan prema gore navedenim uputama. To je mjesto postaje vrlo koristan masovno dodavanje mogućnost.
Odabir "Skupno Dodavanje" će vas odvesti novi plohu. U prvi je korak da biste odabrali sheme za koju se više definicije su će biti stvoren. Odabir sljedeće otvorit će se novi plohu dopuštanja za bilo koju odaberete s popisa već prenesene sheme ili dopuštanja da biste prenijeli novi.
Sada će svojstvo XPATHS otključane. Odabir to će se otvoriti preglednik sheme koju je moguće odabrati više čvorove.
Imena za više definicije stvorili će zadani naziv odabran čvor. To možete uvijek se mijenjati nakon stvaranja.

![Zamjenski tekst][7]

##<a name="policy-creation"></a>Stvaranje pravila
Nakon što razvojni je potrebna vocabularies, na očekivanja je poslovni analitičar bio bi jedan stvaranje pravila tvrtke putem portala za Azure.  
    1. na aplikaciju pravila stvorili postoji pravila leće kliknete koji korisnik će otići na stranici Stvaranje pravila.  
    2. ove stranice prikazat će popis pravila sadrži određene aplikacije pravila. Na analitičar možete dodati novog pravilnika jednostavnim unosom naziv pravila i odlazak kartica dvaput. Više pravila se može smjestiti u jednu aplikaciju API pravila.  
    3. odaberete stvorili pravilo se korisniku stranicu s detaljima pravila gdje nešto mogu vidjeti pravila koja su pravila.  
    ![Zamjenski tekst][8]
 4.  Odaberite "Dodaj" da biste dodali novo pravilo. To će vas odvesti novi plohu.

##<a name="rule-creation"></a>Stvaranje pravila
Pravilo je skup uvjet i akciju izvješća. Akcije se izvršiti ako se procijeni kao true. U plohu Stvori pravilo dati naziv jedinstveni pravila (za to pravilo) i opis (neobavezno).
Okvir uvjet (Ako) se može koristiti za stvaranje složenih uvjetnih izraza. Slijede ključne riječi podržano:  
1.  I – uvjetno operator  
2.  Ili – uvjetno operator  
3.  ne\_ne\_postoji  
4.  postoji  
5.  FALSE  
6.  je\_jednak\_da biste  
7.  je\_veći\_od  
8.  je\_veći\_od\_jednak\_da biste  
9.  je\_u  
10. je\_manje\_od  
11. je\_manje\_od\_jednak\_da biste  
12. je\_ne\_u  
13. je\_ne\_jednak\_da biste  
14. Mod  
15. TRUE  

Okvir Action(THEN) može sadržavati više naredbi jedna po retku, da biste stvorili akcije koje će se izvršiti. Slijede ključne riječi podržano:  
1.  jednako  
2.  FALSE  
3.  TRUE  
4.  Zastoj  
5.  Mod  
6.  null  
7.  Ažuriranje  

Uvjet i akciju okvire pružaju Intellisense da vam brzo stvaranje pravila. To može aktivirati odlazak ctrl + RAZMAKNICA ili pokretanjem samo za unos. Ključne riječi koje odgovaraju upisani znakova se automatski filtrirani prema dolje i prikazano. Prozor intellisense prikazivat će se sve ključne riječi i definicije rječnik.
![Zamjenski tekst][9]

##<a name="explicit-forward-chaining"></a>Eksplicitno Proslijedi Ulančavanje
Pravila BizTalk podržava eksplicitnih Proslijedi, Ulančavanje Ako korisnicima želite omogućiti ponovno procijeniti pravila kao odgovor na određene akcije, oni možete pokrenuti to pomoću određene ključne riječi. Slijede ključne riječi podržano:  
   1.   Ažuriranje <vocabulary definition> – te ključne riječi ponovno procjenjuje sva pravila koje koriste definiciju navedeni rječnik u njegov uvjeta.  
   2.   Zastoj – te ključne riječi zaustavlja sva izvršavanja pravila

##<a name="enabledisable-rules"></a>Enable\Disable pravila
Svako pravilo u pravilo možete omogućiti ili onemogućiti. Sva pravila omogućena je prema zadanim postavkama. Onemogućeni pravila ne može izvršiti tijekom izvođenja pravila. Pravila Enable\Disable može učiniti s plohu pravilo izravno – naredbe dostupne na naredbenoj traci pri vrhu na plohu ili iz pravila, na kontekstnom izborniku (desnom tipkom miša kliknite pravilo) sadrži mogućnost enable\disable.

##<a name="rule-priority"></a>Prioritet pravila
Sva pravila pravila se izvode redoslijedom. Prioritet izvođenja određuje redoslijed koji se pojavljuju u pravilo. U ovom prioritet mogu mijenjati jednostavnim povlačenjem i ispuštanjem pravilo.

##<a name="test-policy"></a>Testiranje pravila
Možete testirati police pomoću naredbe "Testiranje pravila" u plohu testiranje pravila. U ovom plohu možete vidjeti popis definicije rječnik koji se koriste u pravilo koje je potrebno korisničkom unosu. Korisnici mogu ručno dodati vrijednosti za ove unosa za svoje scenarij test. Umjesto toga korisnicima možete odabrati da biste uvezli testirati XMLs unosa. Kada su sve unose, test se može pokrenuti i izlaze Svaka definicija rječnik prikazat će se u stupcu izlaz za jednostavno usporedbe. Da biste pogledali poslovni analitičar neslužbeni zapisnika, kliknite "Prikaz zapisnika" da biste pogledali zapisnike izvršavanja. Da biste spremili zapisnike, mogućnost "Spremi izlaza" dostupna za spremanje svih testiranje povezane podatke za analizu neovisno.

## <a name="using-rules-in-logic-apps"></a>Pomoću pravila u logike aplikacije
Kada je pravilnik Autor je i testirali, sada je spremna za potrošnju. Možete stvoriti novu aplikaciju logike tako da odaberete logike aplikacije s lijeve strane početnoj stranici portala sustava. Nakon što logike aplikacije, pokrenite ga, zatim odaberite *okidača i akcija*. Zatim možete odabrati predložak *stvoriti ispočetka* . Slijedite korake za dodavanje aplikacije za BizTalk pravila API-JA na aplikaciju logike. Kada to učinite, pojavit će se mogućnost da biste odabrali koje pravila API aplikacije (Akcija) odredište. Akcije uključuju popisom pravilnika koje će se izvršiti. Odaberite pravilo određene nakon kojeg se ulaza potrebne za pravilo mora biti zadano. Korisnici mogu koristiti Izlaz iz aplikacije pravila API slijedu za daljnje donošenje odluka.

## <a name="using-rules-via-apis"></a>Korištenje pravila putem API-ji
Aplikaciju API pravila mogu se pozvati i pomoću bogatog skupa API-ji. Ovaj način korisnici nisu ograničili samo korištenje logike aplikacija, a možete koristiti pravila u bilo kojoj aplikaciji upućivanje poziva za OSTALE. Exact REST API-ji dostupni moguće je prikazati tako da kliknete na "API Definiton" na nadzornoj ploči pravila.

![Zamjenski tekst][10]

Slijedi primjer kako nešto koristiti taj API na C#

            // Constructing the JSON message to use in API call to Rules API App

            // xmlInstance is the XML message instance to be passed as input to our Policy
            string xmlInstance = "<ns0:Patient xmlns:ns0=\"http://tempuri.org/XMLSchema.xsd\">  <ns0:Name>Name_0</ns0:Name>  <ns0:Email>Email_0</ns0:Email>  <ns0:PatientID>PatientID_0</ns0:PatientID>  <ns0:Age>10.4</ns0:Age>  <ns0:Claim>    <ns0:ClaimDate>2012-05-31T13:20:00.000-05:00</ns0:ClaimDate>    <ns0:ClaimID>10</ns0:ClaimID>    <ns0:TreatmentID>12</ns0:TreatmentID>    <ns0:ClaimAmount>10000.0</ns0:ClaimAmount>    <ns0:ClaimStatus>ClaimStatus_0</ns0:ClaimStatus>    <ns0:ClaimStatusReason>ClaimStatusReason_0</ns0:ClaimStatusReason>  </ns0:Claim></ns0:Patient>";

            JObject input = new JObject();

            // The JSON object is to be of form {"<XMLSchemName>_<RootNodeName>":"<XML Instance String>".
            // In the below case, we are using XML Schema - "insruanceclaimsschema" and the root node is "Patient".
            // This is CASE SENSITIVE.
            input.Add("insuranceclaimschema_Patient", xmlInstance);
            string stringContent = JsonConvert.SerializeObject(input);


            // Making REST call to Rules API App
            HttpClient httpClient = new HttpClient();

            // The url is the Host URL of the Rules API App
            httpClient.BaseAddress = new Uri("https://rulesservice77492755b7b54c3f9e1df8ba0b065dc6.azurewebsites.net/");
            HttpContent httpContent = new StringContent(stringContent);
            httpContent.Headers.ContentType = MediaTypeHeaderValue.Parse("application/json");

            // Invoking API "Execute" on policy "InsruranceClaimPolicy" and getting response JSON object. The url can be gotten from the API Definition Lens
            var postReponse = httpClient.PostAsync("api/Policies/InsuranceClaimPolicy?comp=Execute", httpContent).Result;

Imajte na umu da aplikacija za API iznad pravila sadrži postavke sigurnosti postavljeno na "Javno Anon". To možete postaviti iz aplikacije API pomoću – sve postavke -> postavke aplikacije -> razinu pristupa.

![Zamjenski tekst][11]

## <a name="editing-vocabulary-and-policy"></a>Uređivanje rječnik i pravila
Jedna od glavnih prednosti korištenja pravila tvrtke jest da promjene poslovne logike može biti Završi radnog mnogo brže. Promjene unesene u rječnik i pravila odmah primijeniti u proizvodnje. Korisnik mora se jednostavno za pregled definiciju odgovarajuća rječnik ili pravila i unesite željene promjene da biste ga snagu.

<!--Image references-->
[1]: ./media/app-service-logic-use-biztalk-rules/InsuranceScenario.PNG
[2]: ./media/app-service-logic-use-biztalk-rules/InsuranceBusinessLogic.png
[3]: ./media/app-service-logic-use-biztalk-rules/CreateRuleApiApp.png
[4]: ./media/app-service-logic-use-biztalk-rules/RulesDashboard.png
[5]: ./media/app-service-logic-use-biztalk-rules/LiteralVocab.PNG
[6]: ./media/app-service-logic-use-biztalk-rules/XMLVocab.PNG
[7]: ./media/app-service-logic-use-biztalk-rules/BulkAdd.PNG
[8]: ./media/app-service-logic-use-biztalk-rules/PolicyList.PNG
[9]: ./media/app-service-logic-use-biztalk-rules/RuleCreate.PNG
[10]: ./media/app-service-logic-use-biztalk-rules/APIDef.PNG
[11]: ./media/app-service-logic-use-biztalk-rules/PublicAnon.PNG
