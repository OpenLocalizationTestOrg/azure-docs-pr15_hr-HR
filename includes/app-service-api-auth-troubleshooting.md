Najčešće pogreške provjere autentičnosti vrijeme proizlaze iz pogrešne ili nedosljedno konfiguracijske postavke. Slijedi nekoliko određene prijedloga za stvari koje treba provjeriti.

* Provjerite da niste propustite gumb **Spremi** bilo kojeg mjesta. To je često jednostavno učinite, a rezultat je koji će biti gledate odgovarajuće vrijednosti na stranici portala, ali ih još niste zapravo su spremljeni u Azure okruženje ili Azure AD aplikacije.
* Postavke konfigurirane **Postavke aplikacije** plohu portala za Azure, obavezno da točan API aplikacije ili web-aplikacije je kada su postavke.  Također provjerite da su postavke unesene kao **postavki aplikacije** i neće **nizove veze**, kao što je slično je oblik dva odjeljka.
* Za provjeru autentičnosti sučelje JavaScript, preuzmite manifest da biste potvrdili da `oauth2AllowImplicitFlow` uspješno mijenjaju se `true`.
* Provjerite je li koristiti HTTPS na svakom mjestu gdje ste konfigurirali URL-ova:

    * U kodu projekta
    * U CORS
    * U odjeljku postavke aplikacije Azure okruženje za svaku aplikaciju API-JA i web-aplikacije
    * U odjeljku postavke aplikacije Azure AD.
    
    Napomena Ako kopirate URL aplikaciju API-JA na portalu, često ima `http://` te ćete morati ručno promijeniti `https://`.

* Provjerite je li kod promjene uspješno su implementiran. Ako, na primjer, rješenja više projekta moguće je promijeniti kod projekta i slučajno odaberite jednu od ostalih kada namjeravate promjena.
* Pripazite da ćete HTTPS URL-ova u pregledniku HTTP URL adrese. Prema zadanim postavkama stvara Visual Studio objaviti profili s URL-ovima HTTP, a to je što će se otvoriti u pregledniku nakon implementacije projekta.
* Za provjeru autentičnosti sučelje JavaScript, provjerite je li CORS ispravno konfiguriran za aplikaciju u API JavaScript kod poziva. Ako se nalazite u niste sigurni o tome je li problema vezanih uz CORS, pokušajte "*" kao URL dopuštene polazište. 
* Za JavaScript sučelje, otvorite karticu konzole za alate za razvojne inženjere u web-pregledniku da biste dobili dodatne informacije o pogrešci, a Analiziraj HTTP zahtjeva na mreži. Međutim, poruke o pogreškama konzole mogu biti netočni. Ako primite poruku o pogrešci CORS, pravi problem može biti provjere autentičnosti. Ako je to slučaj tako da pokrenete aplikaciju s provjerom autentičnosti privremeno privremeno onemogućiti možete provjeriti.
* Za .NET API aplikaciju, provjerite je li dobivate suvišni informacije u porukama o pogreškama moguće postavljanjem [način customErrors na isključeno](../app-service-web/web-sites-dotnet-troubleshoot-visual-studio.md#remoteview).
* Za aplikaciju za .NET API pokrenuti [udaljenu sesiju za ispravljanje pogrešaka](../app-service-web/web-sites-dotnet-troubleshoot-visual-studio.md#remotedebug)i Analiziraj vrijednosti varijabli koji se prosljeđuju kod koji koristi ADAL dobiti nošenja token ili kod koji se provjerava zahtjevima u odnosu na očekivanog servisa glavni ID-a. Imajte na umu da kod obraditi konfiguracijskih vrijednosti iz različitih izvora, pa je moguće pronaći iznenađenja na taj način. Na primjer, ako ste pogrešno `ida:ClientId` kao `ida:ClientID` prilikom konfiguriranja postavki okruženja aplikacije servisa za Azure, kod mogla bi se na `ida:ClientId` vrijednost koja je tražite iz datoteke Web.config, zanemarujući postavku aplikacije servisa za Azure. 
* Ako stvari ne funkcioniraju u običnom prozora preglednika Internet Explorer, na postojeće zapisnika u možda ometaju automatsku reprodukciju; InPrivate pa pokušajte Chrome i Firefox.
