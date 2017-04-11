<properties
    pageTitle="Što je novo u Azure komplet alata za Eclipse"
    description="Informirajte se o najnovijim značajkama u komplet alata za Azure za Eclipse."
    services=""
    documentationCenter="java"
    authors="rmcmurray"
    manager="wpickett"
    editor=""/>

<tags
    ms.service="multiple"
    ms.workload="na"
    ms.tgt_pltfrm="multiple"
    ms.devlang="Java"
    ms.topic="article"
    ms.date="08/26/2016" 
    ms.author="robmcm;asirveda;martinsawicki"/>

<!-- Legacy MSDN URL = https://msdn.microsoft.com/library/azure/hh694270.aspx -->

# <a name="whats-new-in-the-azure-toolkit-for-eclipse"></a>Što je novo u Azure komplet alata za Eclipse

## <a name="azure-toolkit-for-eclipse-releases"></a>Azure komplet alata za Eclipse izdanja

Ovaj članak sadrži informacije o raznim izdanja i najnovija ažuriranja za Azure alata za Eclipse.

> [AZURE.NOTE] Postoji Azure komplet alata za za IntelliJ IDE. Dodatne informacije potražite u članku [Azure komplet alata za IntelliJ].

### <a name="august-26-2016"></a>Kolovoz 26, 2016

Komplet alata za Azure za Eclipse - 2016 kolovoz izdanje obuhvaća sljedeća poboljšanja:

* **Prilagođeni JDK distribucije**. Komplet alata za Azure za Eclipse sada podržava određivanje i implementaciji sustava proizvoljne JDK verzije sustava spremniku Azure WebApp:
  - Osim JDKs nudi Azure, možete odabrati iz široke odabira verzija Zulu OpenJDK učinio dostupnima na Azure Azul sustave.
  - Možete odrediti i vlastitu JDK raspodjelu Ako prenesete ga kao ZIP datoteke na račun servisa za pohranu.
* **Poboljšanja prikaz pretraživača Azure**:
  - Podrška za upravljanje virtualnog računala pomoću novog modela resursima Azure korisnika: popis, stvaranje i na brisanje resursa utemeljen na Upravitelj virtualnim strojevima bez pomicanja na IDE.
  - Podrška za računa spremišta blobova platforme upravljanja pomoću upravitelja resursa Azure korisnika koji dopuna postojeće funkcionalnost za upravljanje računima "klasični" prostora za pohranu.
* **Upravljački program za Microsoft JDBC 6.0 za SQL Server**. To ažuriranje sadrži najnoviji JDBC upravljački program za Microsoft SQL Server (v6.0), što je sada uključeni kao biblioteku u kojoj možete jednostavno dodati Java projekte, čime ćete zamjene starije verzije.

### <a name="june-29-2016"></a>Lipnja 29, 2016

Komplet alata za Azure za Eclipse - 2016 lipnja izdanje obuhvaća sljedeća poboljšanja:

* **Preduvjet Java 8**. Komplet alata za Azure za Eclipse sada zahtijeva Java 8, iako se taj zahtjev je samo za kompleta alata za - aplikacija možete nastaviti koristiti sve verzije Java koje podržava Azure.
* **Podrška za najnovije Java JDKs**. Komplet alata za Azure za Eclipse sada podržava najnovije verzije Java JDKs.
* **Podrška za Azure SDK v2.9.1**. Najnoviju verziju Azure SDK sada je minimalne stara obavezna za Azure alata za Eclipse.
* **Integrirano uzorka**. Komplet alata za Azure za Eclipse sada nudi nekoliko oglednih aplikacija da razvojnim inženjerima početak rada.
* **Integracija alat za HDInsight**. Na Azure HDInsight Alati sada su vezanoj instalaciji s komplet alata za Azure za Eclipse. Dodatne informacije potražite u članku [HDInsight dodatak Alati za Eclipse].
* **Udaljena ispravljanje pogrešaka web-aplikacija Java Web Apps**. Komplet alata za Azure za Eclipse sada podržava udaljene pogrešaka Java web apps na aplikacije servisa za Azure.
* **Podrška za Eclipse Luna izdanje.** Novi najmanji potreban Eclipse IDE verzija je Luna.

### <a name="april-12-2016"></a>Travanj 12, 2016

Komplet alata za Azure za Eclipse - 2016 u travnju izdanje obuhvaća sljedeća poboljšanja:

* **Podrška za Azure SDK v2.9.0**. Najnoviju verziju Azure SDK sada je minimalne stara obavezna za Azure alata za Eclipse.
* **Različiti upotrebljivosti, odziv i performanse poboljšanja vezane uz podrške za Azure Web App**. Broj optimizacije performanse u kako komunicira kompleta alata za Azure rezultatom u više odgovara korisničkog Sučelja.
* **Mogućnost brisanja postojeće web-aplikacije spremnika u Azure s unutar Eclipse**. Komplet alata za Azure za Eclipse sada omogućuje da biste izbrisali postojeću spremnik za Azure Web ne napuštajući Eclipse.

### <a name="march-7-2016"></a>Ožujak 7, 2016

Komplet alata za Azure za Eclipse - 2016 ožujak izdanje obuhvaća sljedeća poboljšanja:

* **Podrška za brze implementacije laganih Java aplikacija**. Komplet alata za Azure za Eclipse sada podržava brzog uvođenja laganih Java aplikacija u Azure Web App spremnika, tako da se sada implementacija aplikacije Java traje sekundi umjesto minuta.
* **Podrška za upravljanje web-aplikacije pomoću programa Explorer Azure prikaza**. Prikaz pretraživača Azure u kompleta alata za sada podržava za popis, pokretanje i zaustavljanje Azure web-aplikacije.
* **Distribucija ažuriranja Tomcat, Jetty, i Zulu OpenJDK**. Komplet alata za Azure za Eclipse pruža podršku za ažurirane verzije Tomcat, Jetty i Zulu OpenJDK za Java implementacije u servisa Azure oblaka.

### <a name="january-4-2016"></a>Siječanj 4, 2016

Komplet alata za Azure za Eclipse - 2016 siječanj izdanje obuhvaća sljedeća poboljšanja:

* **Podrška za Zulu OpenJDK ažuriranja**. Dodatne informacije potražite u članku [Azul sustavi web-stranice na Zulu OpenJDK].
* **Ažuriranje Tomcat i distribucija Jetty**. Distribucija Jetty i Tomcat koje su dostupne na Microsoft Azure za korištenje s komplet alata za Azure za Eclipse ažurirane.
* **Značajka slične značajke između Eclipse i kompleta alata IntelliJ za Azure**. Komplet alata za Azure Eclipse i [Azure komplet alata za IntelliJ] sada podržava isti skup značajki.

### <a name="september-1-2015"></a>1. rujan 2015.

Komplet alata za Azure za Eclipse - rujan 2015 izdanje obuhvaća sljedeća poboljšanja:

* **Podrška za Zulu OpenJDK ažuriranja**. Dodatne informacije potražite u članku [Azul sustavi web-stranice na Zulu OpenJDK].
* **Ažuriranje Tomcat i distribucija Jetty**. Distribucija Jetty i Tomcat koje su dostupne na Microsoft Azure za korištenje s komplet alata za Azure za Eclipse ažurirane. (Te distribucija razvojnim inženjerima omogućuje stvaranje brzi razvoj i testiranje projektima pomoću alata za Azure za Eclipse.
* **Podrška za automatski ažurirana Tomcat i Jetty referenci**. Osim određene verzije Tomcat i Jetty koje su dostupne na Azure, sada razvojni inženjeri mogu se nazivaju raspodjele na "najnovije (automatski ažurirane)", koji će automatski ažurirati najnovije raspodjele svake glavne verzije Jetty ili Tomcat sljedeći put vaša uloga instanci su brisanja. (Recikliranje pojavljuje se automatski, ali razvojnim inženjerima možete ručno pokrenuti koš za smeće putem portala za Azure.) Nova značajka znači razvojni inženjeri ne implementirati svoje aplikacije da biste mogli ste njihove poslužiteljski softver ažurirati. (
*  Ta je funkcija namijenjen je samo za razvoj i testiranje svrhe ili zaštita njihove privatnosti ovise-rješavaju aplikacija, a ne preporučuje se za proizvodnju.)
* **Prikaz pretraživača azure blob-ova, redovima i tablica u Azure prostora za pohranu**. Time se razvojnim programerima da biste izvršili skup uobičajenih zadataka s njihovim artefakte prostora za pohranu izravno iz Eclipse IDE. Na primjer: brisanje, prijenos ili preuzimanje blob-ova.

### <a name="august-1-2015"></a>1. kolovoz 2015.

Komplet alata za Azure za Eclipse - kolovoz 2015 izdanje obuhvaća sljedeća poboljšanja:

* **Key Management Instrumentation uvida aplikacije**. To ažuriranje omogućuje Nabava, stvaranje i upravljanje ključeva instrumentation aplikacije uvida izravno iz Eclipse IDE.
* **Upravljački program za Microsoft JDBC 4.1 za SQL Server**. To ažuriranje sadrži podršku za najnoviji JDBC upravljački program za Microsoft SQL Server.
* **Verzija 2.7 Azure SDK**. U ovom najnovija ažuriranja za Azure SDK je novi stara obavezna za kompleta alata za prilikom instalacije u sustavu Windows. (Imajte na umu nije potrebna operacijskim sustavima koji nisu Windows.)
* **Podrška za Zulu OpenJDK v7 ažuriranje**. Dodatne informacije potražite u članku [Azul sustavi web-stranice na Zulu OpenJDK].

### <a name="may-1-2015"></a>1. svibanj 2015.

Komplet alata za Azure za Eclipse - svibanj 2015 izdanje obuhvaća sljedeća poboljšanja:

* **Poboljšani poslužitelja odabira korisničkog Sučelja**. U ovom izdanju pojednostavljuje korištenje kompleta alata za operacijskim sustavima koji nisu u sustavu Windows.
* **Podrška za Maven projekte**. U ovom izdanju podržava Maven projekte kao aplikacije, kompleta alata za možete implementirati za Azure i konfigurirajte aplikacije uvide.
* **Verzija 2,6 Azure SDK**. U ovom najnovija ažuriranja za Azure SDK je novi stara obavezna za kompleta alata za prilikom instalacije u sustavu Windows. (Imajte na umu nije potrebna operacijskim sustavima koji nisu Windows.)
* **Ponovno objaviti implementaciju nadogradnje umjesto**. Ako projekt implementacije objavljujete kad je već na prethodnu verziju uživo, kompleta alata za sada koristi Azure na implementaciju nadogradnje funkcionalnost umjesto isključuje prethodne implementacije i ponovno objavljivanje ispočetka jednak onome u prošlosti. Time se omogućuje oblaku da se pokreće bez prekida kad god je to moguće, što pomaže postigli visoke dostupnosti čak i tijekom ažuriranje, i ubrzava postupak ponovno objavljivanje.
* **Podršku najnovije v8 Zulu OpenJDK – ažuriranje 40**. Dodatne informacije potražite u članku [Azul sustavi web-stranice na Zulu OpenJDK].

### <a name="march-9-2015"></a>9. ožujak 2015.

Komplet alata za Azure za Eclipse - ožujak 2015 izdanje obuhvaća sljedeća poboljšanja:

* **Podrška za Mac, Ubuntu i dodatnih flavors Linux**. U ovom izdanju programa Azure alata za Eclipse dodaje podršku za Mac OS i nekoliko Unix platforme, da razvojni inženjeri mogli instalirati komplet alata za stvaranje, konfiguriranje i objavljivanje Java projekata Azure servisima u Oblaku (PaaS) iz Eclipse koji se izvodi na operacijskim sustavima koji nije Windows.

>[AZURE.NOTE] Ova mogućnost je u pretpregledu, a ne preporučuje se za korištenje u radnog okruženja. Nema podrške za korisnike ugovor o razini na usluge (SLA), ali appreciated i potiče sve povratne informacije.

* **Dodatak za nove uvide aplikacije**. Razvojni inženjeri mogu moći konfigurirati poslužitelj za automatsko telemetrijskih pomoću aplikacije uvida Azure.
* **Automatizacija implementacije utemeljen na ant naredbenog retka**. Ova značajka omogućuje inženjerima omogućuje automatizaciju objavljivanje u novijim verzijama postupak implementacije pomoću Ant izvan Eclipse. Unaprijed generirane skripte automatski konfigurirana za projekt kada prvi put ga je implementiran putem Eclipse i kasnije implementacijama možete koristiti skriptu da biste potpuno automatizirali implementacijama putem naredbenog retka samo.
* **Tomcat i Jetty dostupnost na Azure jednostavniji, brži implementacije**. Sada razvojni inženjeri mogu različite verzije Tomcat i Jetty koje su dostupne na Azure izravno umjesto da da biste prenijeli na poslužitelj Java njihovi računi (ili putem alata), tako da nema potrebe da biste prenijeli Java poslužitelja za scenarije brzog i početak rada.
* **Postupak prečac za objavljivanje Java web-aplikacije servisa Azure oblaka**. Da biste smanjili krivulje učenje za jednostavne scenariji za razvoj i testiranje razvojnim inženjerima sada možete objaviti aplikacijama Java više izravno Azure. Umjesto da prođite kroz postupak stvaranja i konfiguriranju programa project Azure implementaciju, aplikacija će biti implementirano s zadane instance Tomcat v8 i Zulu JVM (OpenJDK).

### <a name="january-30-2015"></a>30. siječnju 2015.

Komplet alata za Azure za Eclipse - siječnju 2015 izdanje obuhvaća sljedeća poboljšanja:

* **Podrška za IBM® WebSphere® aplikacije Server Liberty Core**. U ovom izdanju dodaje IBM WebSphere aplikacije Server Liberty Core na popisu podržanih aplikacija poslužitelja iz kojeg je moći uvesti Azure kompleta alata za. Ovaj najnovije zbrajanja proširuje trenutni popis poslužitelja aplikacije koje su podržane &quot;Izlaz u-tvorničke&quot; po kompleta alata za koje već uključuje različite verzije Tomcat, Jetty, JBoss i GlassFish.
* **Uključivanje aplikacije uvida SDK**. Ova biblioteka klijent upravo objavio API-JA (v0.9.0) sada je dio paketa za Azure biblioteke Java.
* **Ažuriranje paket za Azure biblioteke Java**. To ažuriranje uključuje Azure biblioteke za Java v0.7.0 i v2.0.0 API za pohranu klijenta, kao i aplikacije uvida SDK v0.9.0 upravo objavio.

### <a name="november-12-2014"></a>12. studenom 2014.

Komplet alata za Azure za Eclipse - studenom 2014 izdanje obuhvaća sljedeća poboljšanja:

* **Podrška za Azure SDK 2,5**. U ovom najnovija ažuriranja za Azure SDK je novi stara obavezna za kompleta alata za.
* **Podrška za ažuriranu verziju Zulu OpenJDK v1.8, v1.7 i v1.6 paketa**. Dodatne informacije potražite u članku [Azul sustavi web-stranice na Zulu OpenJDK].
* **Podrška za nove veličine standardne D za servise u oblaku**, koji ponuda povećati performanse i dodatnu memoriju resursi. Dodatne informacije potražite u članku [virtualnog računala i veličine servisa oblaka za Azure].

### <a name="october-17-2014"></a>17. listopad 2014.

Komplet alata za Azure za Eclipse - listopad 2014 izdanje obuhvaća sljedeća poboljšanja:

* **Poboljšanja performansi u Objavi na scenarije oblaka**. Korisnici imati više pretplata i račune za pohranu je mnogo brže Učitavanje informacija o pretplati.
* **Podrška za ažuriranu verziju paketa v1.8 Zulu OpenJDK**. Dodatne informacije potražite u članku [Azul sustavi web-stranice na Zulu OpenJDK].
* **Podrška za deprecating starijih verzija 3 JDKs proizvođača**. Zastarjeli paketa JDK više neće se prikazivati u na padajućem izborniku za nove projekte implementacije. Postojeće projekte referentni zastarjeli JDK paketa će i dalje povezivanja moći učiniti tako da se put se, ali preporučuje se da biste nadogradili takve projekata navikli najnoviji.
* **Ažuriranje verziju paketa za Azure biblioteke Java klijent API biblioteke**. Dodatne informacije potražite u članku [Microsoft Azure klijent API].
* **Popravci programskih pogrešaka.** Ovo izdanje sadrži broj razne popravci programskih pogrešaka koje se temelje na izvješća korisnika i testiranje.

### <a name="august-5-2014"></a>5. kolovoz 2014.

Komplet alata za Azure za Eclipse - kolovoz 2014 izdanje obuhvaća sljedeća poboljšanja

* **Podrška za Azure SDK 2.4.** Starije verzije komplet alata za Eclipse neće funkcionirati ako taj upravo objavljenu SDK.
* **Ažurirana verzija v1.6 Zulu OpenJDK 1.7 i v1.8 paketa.** Dodatne informacije potražite u članku [Azul sustavi web-stranice na Zulu OpenJDK].
* **Ažuriranu verziju paketa za Azure biblioteke za Java klijentska API biblioteka.** Dodatne informacije potražite u članku [Microsoft Azure klijent API].
* **Podrška za najnoviji objaviti postavke oblik datoteke.** Podrška za dodani verzije 2.0 datotečnog oblika postavke objavljivanja.
* **Arhitektonski promjene iza Objavi u oblak značajke.** Kompleta alata za sada koristi upravo objavljenu Microsoft Azure klijent API za Java za svoju podršku za objavljivanje-na-oblaka.
* **Popravci programskih pogrešaka.** Ovo izdanje sadrži broj korisnika zatražili popravci programskih pogrešaka.

### <a name="june-12-2014"></a>12. lipnja 2014.

Komplet alata za Azure za Eclipse – izdanje iz lipnja 2014 je manji servisiranja ažuriranja koja ima sljedeća poboljšanja:

* **Podrška za paket v1.8 Zulu OpenJDK.** Dodatne informacije potražite u članku [Azul sustavi web-stranice na Zulu OpenJDK].
* **Ažurirane verzije v1.6 Zulu OpenJDK i 1.7 paketa.** Dodatne informacije potražite u članku [Azul sustavi web-stranice na Zulu OpenJDK].
* **Ažuriranu verziju paketa za Azure biblioteke za Java klijentska API biblioteka.** Dodatne informacije potražite u članku [Microsoft Azure klijenta API].
* **Popravci programskih pogrešaka.** Ovo izdanje sadrži broj korisnika zatražili popravci programskih pogrešaka.

### <a name="april-4-2014"></a>4. u travnju 2014.

Dodatak za Azure za Eclipse – izdanje Travanj 2014 je objavio. To je ažuriranje popratnim izdanje SDK 2.3 Azure koja je stara obavezna i preuzet će se automatski kada instalirate dodatak. To ažuriranje sadrži nove značajke, popravci programskih pogrešaka i neka poboljšanja upotrebljivosti utemeljenih na povratne informacije, budući da pregled veljača 2014.:

* **Podrška za Azure SDK 2.3 izdanje.** Dodatak za Azure za Eclipse – izdanje Travanj 2014 zahtijeva Azure SDK 2.3. Ako prilikom korištenja dodatak za nove već nemate Azure SDK 2.3, zatražit će se da bi se instalacija. Nemojte koristiti Azure SDK 2.3 sa starijim verzijama programa dodatak.
* **Nadogradnja programa bez implementacije cijeli paket.** Prilikom implementacije Java programa koji su dio projekta, dodatak sada automatski prenosi ih na račun za odabrani prostor za pohranu tako da možete ažurirati i koša za slučajeve uloge za implementaciju najnoviju aplikaciju bitova bez potrebe za ponovno stvaranje i ponovno implementirate cijeli paket.
* **Tomcat 8 sada je poslužitelj prepoznati aplikacije.** Ako ste odabrali direktorij instalacija Tomcat 8 na računalu na kartici **poslužitelj** dijaloškog okvira **Azure implementacije projekta** , dodatak će sada automatski ga otkriti i moći uvesti Tomcat 8 programa automatiziranog način, slično kao u starijim verzijama programa Tomcat već na popisu.
* **Azul Zulu OpenJDK paketa ažuriranja: ažuriranje v1.7 51 i v1.6 ažuriranje 47.** Učinkovitih s ovom izdanju sustava Azul Zulu Otvori JDK v7 paketa 51 je dostupno ažuriranje. Osim toga, Zulu Otvori JDK v6 paketa pokrenuti budu dostupne, počevši od ažuriranje 47. Sljedeća ažuriranja su uz prije bile dostupno paket v7 Zulu Otvori JDK ažuriranje 45 ažuriranje 40 i ažurirati 25.
* **Podrška za veličinu A8 i A9 Microsoft Azure virtualnog računala.** Sada možete implementirati servis u oblaku za opterećenje memorije A8 i veličine A9 virtualnog računala. Dodatne informacije o te veličine VM potražite u članku [virtualnog računala i veličine servisa oblaka za Azure].
* **Automatsko preusmjeravanje iz HTTP HTTPS za SSL omogućen uloge.** Kada servis u oblaku sadrži samo HTTPS uloge, ako je zahtjev za korisnika određuje HTTP, on će automatski preusmjeriti HTTPS. Nema potrebe da biste stvorili zasebnom ulogu rukovanja zahtjeva za HTTP.
* **Izrazite Emulator koji se koriste za lokalni emulacija.** Azure Express Emulator sada se koristi kao u emulator kada ispravljanje pogrešaka aplikacije na lokalno.
* **Azure preimenovana je kao Microsoft Azure.** Korisničko Sučelje zaslonima sada odražavaju da Azure rebranded i više naziva Azure.

### <a name="february-6-2014"></a>6. veljača 2014.

Dodatak za Azure Eclipse - veljača 2014 je objavio pretpregled. To ažuriranje sadrži nove značajke, popravci programskih pogrešaka i neka poboljšanja upotrebljivosti utemeljenih na povratnih informacija od listopad 2013 (pretpregled):

* **Podrška za rasteretite SSL.** Sigurna rasteretite Sockets Layer (SSL) dodana kao značajka, što omogućuje jednostavno podržava Hypertext Transfer Protocol sigurne (HTTPS) u implementaciji sustava Java na Azure, bez konfiguriranje SSL u vaš poslužitelj Java aplikacije. To je osobito prikladno u sesiju afinitet i/ili scenariji komunikacije autentičnost. Na primjer, kada koristite filtriranje pristup kontrola servisa (ACS) koje već podržava alata. Dodatne informacije potražite u članku [Rasteretite SSL] te [upute za korištenje SSL rasteretite].
* **GlassFish 4 sada je poslužitelj prepoznati aplikaciju.** Ako ste odabrali direktorija za instalaciju GlassFish 4 na računalu na kartici **poslužitelj** dijaloškog okvira **Azure implementacije projekta** , dodatak će sada automatski ga otkriti i moći implementacije GlassFish OSE 4 u programa automatiziranog, sličan način verzije GlassFish OSE 3 već na popisu.
* **Azul Zulu OpenJDK paketa ažuriranja 45.** Učinkovite s ovom izdanju sustava Azul Zulu (Otvori JDK v7 paketa) ažuriranje 45 sada je dostupan; Ovo je uz prije bile dostupno ažuriranje 40 i ažuriranje 25.
* **Podrška za automatsko privatno priključke krajnjoj točki.** Privatni priključak možete postaviti na automatsko za unos krajnje točke i interna krajnje točke da biste omogućili Azure automatski dodjeljuje priključak za taj krajnje točke. Prethodno može dodijeliti samo na određeni broj priključka.
* **Podrška za prilagodbu naziv certifikata (CN) stvaranje samopotpisane potvrde korisničkog Sučelja.** Prethodno, s istim nazivom programiranih korišten za sve nove certifikate; Sada možete odrediti vlastiti naziv certifikata za lakše razlikovanje među više certifikata na portalu Azure koristi za različite potrebe.
* **Azure alatne trake:** Alatna traka za Azure sadrži programa ažurirani pomoću sljedeće promjene: 
    * ![][ic710876]Ta ikona dodan je **Novi projekt Azure implementacije**.
    * ![][ic710877]Ta ikona dodan kao prečac dijaloškom okviru stvaranje samopotpisane potvrde.
* **Podrška za veličinu A5 Azure virtualnog računala.** Sada možete implementirati servis u oblaku za opterećenje memorije veličina A5 virtualnog računala. Dodatne informacije o ovom veličina VM potražite u članku [virtualnog računala i veličine servisa oblaka za Azure].
* **Podrška za Microsoft Windows Server 2012 R2.** Sada možete odabrati Windows Server 2012 R2 kao operacijski sustav oblaka.

### <a name="october-22-2013"></a>Listopad 22 2013.

Dodatak za Azure Eclipse - listopad 2013 pretpregled Izdana. To ažuriranje sadrži nove značajke, popravci programskih pogrešaka i neka poboljšanja upotrebljivosti utemeljenih na povratnih informacija od rujan 2013 (pretpregled):

* **Podrška za Azure SDK 2.2 izdanje.** Dodatak za Azure za Eclipse - listopadu 2013 (pretpregled) podržava Azure SDK 2.2. Dodatak će i dalje funkcionirati s Azure SDK 2.1 i automatski instalirati Azure SDK 2.2 ako već nemate barem Azure SDK 2.1 instaliran.
* **Azul Zulu OpenJDK paketa ažuriranja 40.** Kako objaviti za rujan 2013 pretpregled omogućuje sada dodatak pomoću pod uvjetom trećih strana JDK izravno na Azure, a da nije potrebno prenijeti vlastite JDK. U izdanju listopad 2013 ažuriranje za Zulu (Otvori JDK v7 paketa) Azul sustav 40 sada je dostupan; Ovo je uz izvorno objavljeni ažuriranje 25.
* **Oblak implementacije vezu u zapisnik aktivnosti.** Unutar zapisnik aktivnosti Azure kada implementaciju sustava sa stanjem **objavljena**, možete kliknuti **objavljene** jer je sada veza na implementaciju; implementaciju sustava će se otvoriti u pregledniku. (Status **objavljeno** je prethodno označen **pokrenut**.)
* **OS odabira ciljne dostupne na objaviti vremena.** Dijaloški okvir **Objavi u Azure** sadrži novo polje **Cilj OS**, koji sadrži više vidljivim omogućuje vam da biste postavili operacijski sustav cilj.
* **Automatsko prebrisali prethodno implementacija.** Dijaloški okvir **Objavi u Azure** prikazuje novi potvrdni okvir, **Prebriši prethodne implementacije**. Ako ta mogućnost nije potvrđen, kada novu implementaciju se objavljuje automatski će prebrisati prethodne implementacije; ne želite sučelje &quot;409 sukoba&quot; problemi pri objavljivanju na isto mjesto bez prvog poništavanje objave prethodne implementacije.
* **Jetty 9 sada je poslužitelj prepoznati aplikacije.** Ako ste odabrali direktorij Jetty 9 instalacije na računalu na kartici **poslužitelj** dijaloškog okvira **Azure implementacije projekta** , dodatak će sada automatski ga otkriti i moći implementacije Jetty 9 u programa automatiziranog, sličan način starijih verzija Jetty već na popisu.
* **Dodajte uloge na kontekstnom izborniku projekta.** Kontekstni izbornik za **Azure** projekta sada sadrži nove stavke izbornika, **Dodavanje uloge**, koji omogućuje na brže i dodatne vidljivim način da biste dodali novo radno mjesto Azure projekta.
* **Ažuriranje paket za Azure biblioteke Java biblioteke.** Temelji se na verziju 0.4.6 sustava [Microsoft Azure klijent API].

### <a name="september-25-2013"></a>Rujan 25 2013.

Dodatak za Azure Eclipse - 2013 rujan Izdana pretpregled. To ažuriranje sadrži nove značajke, popravci programskih pogrešaka i neka poboljšanja upotrebljivosti utemeljenih na povratnih informacija od kolovoz 2013 (pretpregled):

* **Mogućnost postaviti Azul Zulu OpenJDK paket koji je dostupan na Azure.** Nova mogućnost je dodan pri određivanju JDK će se koristiti za implementaciju sustava Azure. Pomoću ove mogućnosti možete implementirati paket JDK trećih strana izravno na Azure oblaka bez potrebe za prijenos vlastitu. Sustavi Azul pruža prve kao što su pakiranje pod nazivom Zulu temelju OpenJDK sada može uvesti pomoću ove mogućnosti.
* **Ažuriranje paket za Azure biblioteke Java biblioteke.** Temelji se na verziju 0.4.5 sustava [Microsoft Azure klijent API].

### <a name="august-1-2013"></a>Kolovoz 1 2013.

Dodatak za Azure Eclipse - 2013 kolovoz Izdana pretpregled. To je ažuriranje popratnim izdanje SDK 2.1 Azure koja je stara obavezna i preuzet će se automatski kada instalirate dodatak. To ažuriranje sadrži nove značajke, popravci programskih pogrešaka i neka poboljšanja upotrebljivosti utemeljenih na povratnih informacija od srpanj 2013 (pretpregled):

* **Uklanjanje mogućnosti da biste uključili lokalni JDK i poslužitelj za lokalnu aplikaciju kao dio paketa za implementaciju.** Preuzimanje u JDK te se preporučuje za ugrađivanje te komponente u paketu nakon preuzimanja stavki rezultira manja veličina paketa za implementaciju, brže implementacije vremena i lakše održavanje poslužitelj aplikacije iz za pohranu u oblaku tijekom uvođenja. Kao rezultat, mogućnosti za uključivanje u JDK i poslužitelj aplikacije u paketa za implementaciju uklonjene. Postojeće projekte podešena za uključivanje lokalni JDK i poslužitelj za lokalnu aplikaciju kao dio paketa za implementaciju automatski pretvorit će se automatski prijenos na JDK i poslužitelj aplikacije za pohranu u oblaku.
* **Podrška za Azure SDK 2.1 izdanje.** Dodatak za Azure Eclipse - kolovoz 2013 pretpregled zahtijeva Azure SDK 2.1. Pomoću pretpregleda kolovoz 2013 sa starijim verzijama programa Azure SDK, a ne koristite Azure SDK 2.1 sa starijim verzijama programa Azure dodatak za Eclipse.
* **Podrška za Eclipse Kepler izdanje.** Vezane uz to, nova vrijednost za minimum potreban je verzija Eclipse IDE Indigo. Dodatak za Azure za Eclipse više službeno testirati na Helios.

### <a name="july-3-2013"></a>Srpanj 3 2013.

Dodatak za Azure Eclipse - srpanj 2013 pretpregled Izdana. To ažuriranje sadrži nove značajke, popravci programskih pogrešaka i neka poboljšanja upotrebljivosti utemeljenih na povratnih informacija od svibanj 2013 (pretpregled):

* **Mogućnost da biste stvorili novi račun za pohranu.** Gumb **Novo** dodana u dijaloški okvir **Dodavanje računa za pohranu** . Omogućuje stvaranje računa za pohranu unutar dodatak Eclipse bez potrebno prijaviti na Portal za upravljanje Azure. (Morate već imati Azure pretplate da biste koristili tu značajku.) Dodatne informacije o stvaranju novog računa za pohranu potražite u članku [Stvaranje novog računa za pohranu].
* **Novi &quot;(automatski)&quot; mogućnost za pohranu računa koji se koristi za automatsko implementacije JDK i poslužitelja, a za predmemoriranje.** Kada koristite mogućnost **automatski prijenos** za na JDK i poslužitelj aplikacije, sada možete navesti **(automatski)** za račun URL i prostora za pohranu za korištenje prilikom prijenosa na JDK i poslužitelj aplikacije ili pri korištenju predmemoriranje Azure. Nakon toga te značajke automatski će koristiti isti račun za pohranu kao onaj koji ste odabrali u dijaloškom okviru **Objavi na Azure** . Praktični vodič za [Stvaranje pozdrav svijeta aplikacije za Azure u Eclipse] ažurirana je da biste koristili novi mogućnost **(automatski)** .
* **Mogućnost da biste postavili svoje Azure service krajnje točke.** Navedite krajnje točke servisa da biste odredili hoće li se aplikacija implementiran na i upravlja globalni platforme Azure, Azure kojim upravlja 21vianet u Kini ili privatnu platforme Azure. Dodatne informacije potražite u članku [Krajnje točke servisa Azure].
* **Velike implementacije možete odrediti lokalno spremište resursa.** U slučaju da je prevelik da bi se nalaziti u zadanu mapu approot implementaciju sustava, sada možete navesti lokalno spremište resursa kao odredišta za implementaciju za vaše JDK i poslužitelj aplikacije. Dodatne informacije potražite u članku [Implementacija velike implementacije].
* **Podrška za A6 i A7 Azure virtualnog računala.** Sada možete implementirati servis u oblaku za opterećenje memorije A6 i A7 virtualnog računala veličine. Dodatne informacije o te veličine potražite u članku [virtualnog računala i veličine servisa oblaka za Azure].
* **Ažuriranje paket za Azure biblioteke Java biblioteke.** Temelji se na verziju 0.4.4 sustava [Microsoft Azure klijent API].

### <a name="may-1-2013"></a>Možda 1 2013.

Dodatak za Azure za Eclipse – možda 2013 pretpregled je objavio. Ovo je važnija ažuriranja popratnim izdanje SDK 2.0 Azure koja je stara obavezna i preuzet će se automatski kada instalirate dodatak. Ovo izdanje sadrži nove značajke, popravci programskih pogrešaka i neka poboljšanja upotrebljivosti utemeljenih na povratnih informacija od veljače 2013 pretpregled:

* **Automatski prijenos JDK i poslužitelj aplikacije i implementaciju s Azure prostora za pohranu.** Nova mogućnost koja automatski prenosi odabrane JDK i poslužitelj aplikacije, po potrebi, s računom navedeni Azure prostora za pohranu i uvodi te komponente iz nje, umjesto ulaganje u paketa za implementaciju ili ručno zatim pojavljuju prijenos korisnika. Značajka često Tražena možete znatno poboljšati olakšani implementacija komponente JDK i poslužitelj, osobito početnicima. Walk-through koji koristi ove mogućnosti potražite u članku [Stvaranje pozdrav svijeta aplikacije za Azure u Eclipse].
* **Centralizirane računa za pohranu za praćenje i mogućnost referenca za pohranu računi više jednostavno (putem padajućeg izbornika kontrola).** To se odnosi na više značajki koje ovise o prostor za pohranu, kao što su JDK i implementacija komponente poslužitelja i predmemoriranja. Dodatne informacije potražite u članku [Popisa programa Azure prostora za pohranu računa].
* **Postavljanje pojednostavljeni daljinski pristup u Objavi u oblak čarobnjak.** Sve što trebate napraviti je upišite korisničko ime i lozinku za Omogućivanje daljinskog pristupa ili ostavite prazno da biste zadržali daljinski pristup onemogućene.
* **Ažuriranje paket za Azure biblioteke Java biblioteke.** Temelji se na verziju 0.4.2 sustava [Microsoft Azure klijent API].
* **Podrška za ljepljive sesija u sustavu Windows Server 2012.** Prethodno, ljepljive sesije radili samo Windows Server 2008 R2 sada oba cloud operacijski sustav ciljnih web-mjesta podršku sesiju afinitet.
* **Poboljšanja performansi učitavanja paketa.** Čak i kada se JDK i poslužitelj aplikacije su ugrađene u paketa za implementaciju, prijenos dio postupak implementacije može biti približno dvaput brzo, to prethodne verzije.

### <a name="february-8-2013"></a>Veljača 8 2013.

Dodatak za Azure Eclipse - veljače 2013 pretpregled Izdana. To je manji update, koji uključuje popravci programskih pogrešaka, poboljšanja upotrebljivosti utemeljenih na povratne informacije i neke od novih značajki jer 2012 studenom pretpregled:

* Podrška za implementaciju JDKs, poslužitelji aplikacija i proizvoljne drugih komponenti iz javna ili privatna Azure bloba preuzimanja za pohranu umjesto uključujući ih u paketa za implementaciju pri implementaciji u oblak.
* Mogućnost da biste promijenili redoslijed u kojem korisnički definirane komponente uloge obrađuju se kroz zbrajanja gumbi **Premjesti gore** i **Premjesti dolje** u odjeljku **komponente** **Azure uloga svojstva**.
* Ažuriranje **paket za Azure biblioteke Java** biblioteku, ovisno o verziji 0.4.0 sustava [Microsoft Azure klijent API].

### <a name="november-5-2012"></a>5. studenom 2012.

Dodatak za Azure Eclipse - 2012 studenom je objavio pretpregled. To je važnija ažuriranja koja obuhvaća broj novih značajki, kao i dodatnih popravci programskih pogrešaka i poboljšanja upotrebljivosti utemeljenih na povratnih informacija od 2012 rujan pretpregled:

* Podrška za Microsoft Windows Server 2012 kao operacijski sustav oblaka.
* Podrška za Azure Suradnja nalazi predmemoriranja podršku za klijente memcached.
* Uključivanje biblioteke klijent Apache Qpid JMS za iskoristi Azure AMQP sustavom razmjene poruka.
* Poboljšani čarobnjaka **Novi projekt** , s nove stranice na kraju koji korisnicima omogućuje brzo omogućiti nekoliko uobičajenih ključne značajke u svoje projektu: ljepljive sesije, predmemoriranje i daljinsko uklanjanje programskih pogrešaka.
* Automatsko smanjivanje instanci uloga 1 kada se pokrene u emulator računalnim, da biste izbjegli sukobe povezivanje priključak između instanci poslužitelja.

### <a name="september-28-2012"></a>28. rujan 2012.

Dodatak za Azure Eclipse - rujan 2012 Izdana pretpregled. To ažuriranje servisa sadrži nekoliko dodatnih popravci programskih pogrešaka Budući da pregled kolovoz 2012., kao i neka poboljšanja upotrebljivosti utemeljenih na povratnih informacija u postojećih značajki:

* Podrška za Microsoft Windows 8 i Microsoft Windows Server 2012 kao operacijski sustav razvoj rješavanje problema koji je prethodno spriječio dodatak radi ispravno u tim operacijskim sustavima.
* Poboljšana podrška za navođenju raspona priključak krajnjoj točki.
* Popravci programskih pogrešaka vezanih uz putova datoteka koje sadrže razmake.
* Uloga Kontekstni izbornik poboljšanja brži pristup specijalizirane konfiguracijske postavke.
* Pomoćna poboljšanja u čarobnjak za **Objavljivanje na cloud** i nekoliko dodatnih popravci programskih pogrešaka.

### <a name="august-28-2012"></a>28. kolovoz 2012.

Dodatak za Azure Eclipse - kolovoz 2012 Izdana pretpregled. Servis za ažuriranje obuhvaća dodatne popravci programskih pogrešaka Budući da pregled srpanj 2012, kao i nekoliko poboljšanja upotrebljivosti utemeljenih na povratne informacije za postojećih značajki:

* U dijaloškom okviru Azure pristup kontrola servisa filtra:
    * **Mogućnost da biste ugradili potpisnog certifikata** u datoteci WAR vaše aplikacije, da biste pojednostavnili implementacije u oblaku.
    * **Mogućnosti za stvaranje samopotpisane potvrde** unutar filtru ACS korisničkog Sučelja. Dodatne informacije o Azure filtar za pristup kontrola Services potražite u članku [autentičnost Web korisnicima s Azure pristup kontrola servisa pomoću Eclipse].
* U čarobnjaku za Azure implementacije projekta (odnosi i na željenu ulogu konfiguraciju poslužitelja svojstava stranice):
    * **Automatsko otkrivanje JDK mjesto** na računalu (koji možete nadjačati po želji).
    * **Automatsko otkrivanje vrste poslužitelja** kad odaberete direktorija za instalaciju aplikacije poslužitelja.

### <a name="july-15-2012"></a>Srpanj 15, 2012.

Dodatak za Azure Eclipse - srpanj 2012 Izdana pretpregled, adrese broj najveći prioritet pogreškama pronaći i/ili dojavi korisnici nakon objavljivanja lipnja 2012. Budući da je ažuriranje servisa samo, nalaze se novih značajki.

### <a name="june-7-2012"></a>7. lipnja 2012.

Dodatak za Azure Eclipse - lipnja 2012 CTP je objavio. Nove značajke obuhvaćaju sljedeće:

* **Čarobnjak za novi projekt Azure implementacije:** Omogućuje odabir JDK, Java poslužitelj aplikacije i Java aplikacije izravno u čarobnjaku za poboljšani korisničkog Sučelja. Naći na popisu poslužitelja Izlaz u-tvorničke konfiguracije na raspolaganju su Tomcat 6, Tomcat 7, GlassFish OSE 3, Jetty 7, Jetty 8, JBoss 6 i 7 JBoss (samostalno). Osim toga, možete prilagoditi popis konfiguracije poslužitelja. Tog poboljšanja korisničkog Sučelja je alternativu povlačenje i ispuštanje komprimirane datoteke i kopiranje putem skripte za pokretanje, koji je prethodno glavni pristup. Taj način još uvijek dobra, no vjerojatno će se koristiti samo za naprednije scenarije.
* **Stranica svojstava za konfiguraciju poslužitelja uloga:** Omogućuje vam da jednostavno promijeniti JDKs, jezika Java poslužitelji aplikacija i aplikacije pridružene u svojoj implementaciji nakon stvaranja projekta. Dodatne informacije potražite u članku [Svojstva za konfiguraciju poslužitelja].
* **&quot;Objavljivanje oblak&quot; čarobnjak:** pruža jednostavan način implementacije projekta Azure izravno iz Eclipse, automatizacija na prethodno ručno podebljano-dizanja o dohvaćanju vjerodajnice, prijave u Azure portala za upravljanje, prijenos paketa, itd. Primjer izravno uvesti projekt Azure potražite u članku [Stvaranje pozdrav svijeta aplikacije za Azure u Eclipse].
* **Azure alatne trake:** Azure alatna traka sada je dostupna na Eclipse koji sadrži gumbe za pozivanje sljedeće značajke:
    * ![][ic710879]**Pokretanje u Azure Emulator**: pokreće projekta u na emulator.
    * ![][ic710880]**Ponovno postavljanje Emulator Azure**: vraća na emulator.
    * ![][ic710881]**Stvaranje paketa oblaka za Azure**: kompilira vašeg paketa radi implementacije.
    * ![][ic710876]**Novi projekt Azure implementacije**: stvara novi projekt Azure implementacije.
    * ![][ic710882]**Objavljivanje Azure oblak**: objavljuje projekt na Azure.
    * ![][ic710883]**Poništi objavu**: briše implementaciju sustava.
    * Mnoge od tih Azure gumbi alatne trake se koriste u [Stvaranje pozdrav svijeta aplikacije za Azure u Eclipse].
* **Azure biblioteke za Java:** Sad je dostupna kao dio jedan paket za Azure biblioteke za biblioteku Java u Eclipse, popratnim dodatak za instalaciju i koji sadrži sve potrebne ovisnosti kao i. Jednostavno dodajte jednu referencu na biblioteku u projektu Java i ne morate ništa preuzeti zasebno. Dodatne informacije potražite u članku [Instaliranje alata za Azure za Eclipse].
* **Microsoft JDBC 4.0 upravljački program za SQL Server dostupne tijekom instalacije dodatka:** Tijekom instalacije dodatka za nove, moguće je instalirati najnovija verzija sustava Microsoft JDBC upravljački program za SQL Server.
* **Azure pristup kontrola filtar usluga dostupni tijekom instalacije dodatka:** U ovom novu komponentu uključeni kao biblioteku Eclipse u kompleta alata za omogućuje web-aplikacije Java jednostavno iskoristiti provjere autentičnosti Azure pristup kontrola servisa (ACS) pomoću različitih davatelji identiteta, kao što su Google, Live.com i Yahoo!. Nećete morati pisanje logike za provjeru autentičnosti, samo konfiguracija nekoliko mogućnosti i omogućuju filtar učinite lifting Tamni omogućivanja korisnicima da se prijaviti pomoću ACS. Možete se fokusirati samo na pisanja koda za koji omogućuje korisnicima pristup resursi koji se temelji na svoj identitet vrate u aplikaciji filtrom unutar objekta zahtjev. Praktični vodič na pomoću filtra ACS, potražite u članku [autentičnost Web korisnicima s Azure pristup kontrola servisa pomoću Eclipse].
* **Automatsko otkrivanje preduvjeta Azure SDK 1.7:** Prilikom stvaranja novog projekta implementacije Azure Azure SDK 1.7 automatski će se preuzeti ako već nije instaliran.
* **Instancu krajnje točke:** Omogućuje pristup krajnjoj točki Izravni priključak za komunikaciju s rasporediti opterećenje uloga instance. Krajnje točke instancu mogu se dodati putem krajnje točke UI, dostupne putem stranici [krajnje točke svojstva] . Pomaže omogućiti daljinsko uklanjanje programskih pogrešaka i JMX Dijagnostika za određeni izračunati instanci koje su pokrenute u oblaku u scenariji s više – instancu implementacije. 
* **Komponente korisničkog Sučelja:** Olakšava za napredne korisnike da biste postavili projekta međuzavisnosti pojedinačne Azure uloge u projektu i drugim vanjskim izvorima kao što su projekti aplikacije Java; Također olakšava opišite svoje logike implementacije. Dodatne informacije potražite u članku [komponente svojstva].
* **Automatska Nadogradnja starijih verzija projekata:** Kada otvorite radni prostor koji sadrži Azure projekt stvoren pomoću prethodne verzije dodatak, stari projekata prikazat će se u Eclipse kao Zatvoreno, jer prethodnih verzija projekata nisu kompatibilne s novim izdanjima. Ako pokušate otvoriti jednu od ove stare projektima, počet će se čarobnjak za nadogradnju. Ako se slažete nadogradnje, novi projekt, s **_Upgraded** dodan na naziv će mogu stvoriti i automatski ažurirati za rad s novim izdanjima. Novi projekt možete preimenovati prema potrebi. Kao dio nadogradnje, izvorna projekta bit će izmijenjeno (i će ostati zatvorena).

### <a name="december-10-2011"></a>Prosinac 10, 2011

Dodatak za Azure Eclipse – prosinac 2011 CTP je objavio. Nove značajke obuhvaćaju sljedeće:

* **Sesiju afinitet (&quot;ljepljive sesije&quot;) podržava:** Doprinos omogućiti s praćenjem stanja, grupirani Java aplikacije sa samo jednom potvrdni okvir. Dodatne informacije potražite u članku [Afinitet sesiju].
* **Unaprijed unijeli prilikom pokretanja skripte uzoraka:** Za najčešće korištene Java poslužitelja (Tomcat, Jetty, JBoss, GlassFish), koje možete samo kopirajte i zalijepite iz imenika uzoraka u projekt u skriptu za pokretanje.
* **Emulator pokretanje Izlaz u stvarnom vremenu:** Sada pogledajte izvođenja svih koraka iz skriptu za pokretanje u prozoru namjenski konzole prikazuje napredak i pogrešaka u skriptu kao što je izvršio Azure.
* **Nadzor automatsko, osnovne java.exe:** Kada java.exe će se prestati izvoditi, pomoću skripte laganih, prethodno napravljene automatski uključena u implementaciji sustava koji će obavezno uloga koš za smeće.
* **Udaljene Java aplikaciju ispravljanje pogrešaka konfiguriranje korisničkog Sučelja:** Omogućuje vam da jednostavno omogućiti Eclipse na udaljenom ispravljanje pogrešaka da biste pristupili aplikaciju Java izvodi na Emulator ili Azure oblaka, da biste mogli počnete slijediti i ispravljanje pogrešaka u kodu programskog jezika Java u stvarnom vremenu. Dodatne informacije potražite u članku [Ispravljanje pogrešaka aplikacije Azure Eclipse].
* **Lokalno spremište resursa konfiguriranje korisničkog Sučelja:** Tako da više ne morate konfigurirati Lokalni resursi po izravno rukovanje XML. Ova značajka omogućuje vam pristup učinkovitih put lokalnog resursa nakon što je uvedena putem varijabla okruženja možete referencirati izravno iz skriptu za pokretanje. Dodatne informacije potražite u članku [lokalno spremište svojstava].
* **Varijable konfiguracije okruženja korisničkog Sučelja:** Tako da više ne morate postaviti varijable okruženja putem ručno uređivanje konfiguracije XML. Dodatne informacije potražite u članku [Svojstva varijable okruženja].
* **JDBC upravljački program za SQL Azure:** Instalirali putem dodatak kao jednostavno integrirani Eclipse biblioteci, omogućivanje olakšavaju programiranje protiv SQL Azure dobiti. 
* **Brzi kontekstnog izbornika pristup uloga konfiguriranje korisničkog Sučelja**: samo desnom tipkom miša kliknite mapu ulogu i kliknite **Svojstva**.
* **Azure prilagođeni projekt i uloge ikone mapa:** Za bolje vidljivosti i lakše navigacije unutar radnog prostora i project.

## <a name="see-also"></a>Vidi također ##

Dodatne informacije o Kompleti alata Azure za Java IDEs potražite na sljedećim vezama:

- [Azure komplet alata za Eclipse]
  - [Instalacija alata za Azure za Eclipse]
  - [Stvaranje web-aplikacijama svijeta pozdrav za Azure u Eclipse]
  - *Što je novo u Azure komplet alata za Eclipse (Ovaj članak)*
- [Azure komplet alata za IntelliJ]
  - [Instalacija alata za Azure za IntelliJ]
  - [Stvaranje web-aplikacijama svijeta pozdrav za Azure u IntelliJ]
  - [Što je novo u Azure komplet alata za IntelliJ]

Dodatne informacije o korištenju Azure s Java potražite u članku [Razvojni centar za Azure Java].

<!-- URL List -->

[Azure komplet alata za Eclipse]: ./azure-toolkit-for-eclipse.md
[Azure komplet alata za IntelliJ]: ./azure-toolkit-for-intellij.md
[Stvaranje web-aplikacijama svijeta pozdrav za Azure u Eclipse]: ./app-service-web/app-service-web-eclipse-create-hello-world-web-app.md
[Stvaranje web-aplikacijama svijeta pozdrav za Azure u IntelliJ]: ./app-service-web/app-service-web-intellij-create-hello-world-web-app.md
[Instalacija alata za Azure za Eclipse]: ./azure-toolkit-for-eclipse-installation.md
[Instalacija alata za Azure za IntelliJ]: ./azure-toolkit-for-intellij-installation.md
[What's New in the Azure Toolkit for Eclipse]: ./azure-toolkit-for-eclipse-whats-new.md
[Što je novo u Azure komplet alata za IntelliJ]: ./azure-toolkit-for-intellij-whats-new.md

[Razvojni centar za Azure Java]: http://go.microsoft.com/fwlink/?LinkID=699547

[Sustavi Azul web-stranice na Zulu OpenJDK]: http://go.microsoft.com/fwlink/?LinkId=402457
[Krajnje točke servisa Azure]: http://go.microsoft.com/fwlink/?LinkID=699526
[Popis poslovnih subjekata Azure prostora za pohranu]: http://go.microsoft.com/fwlink/?LinkID=699528
[Svojstva komponente]: http://go.microsoft.com/fwlink/?LinkID=699525#components_properties
[Stvaranje aplikacije svijeta pozdrav za Azure u Eclipse]: http://go.microsoft.com/fwlink/?LinkID=699533
[Ispravljanje pogrešaka Azure aplikacije u Eclipse]: http://go.microsoft.com/fwlink/?LinkID=699535
[Implementacija velike implementacije]: http://go.microsoft.com/fwlink/?LinkID=699536
[Svojstva krajnje točke]: http://go.microsoft.com/fwlink/?LinkID=699525#endpoints_properties
[Svojstva varijable okruženja]: http://go.microsoft.com/fwlink/?LinkID=699525#environment_variables_properties
[HDInsight dodatak Alati za Eclipse]: ./hdinsight/hdinsight-apache-spark-eclipse-tool-plugin.md
[Upute za provjeru autentičnosti korisnika weba uslugom kontrola pristupa Azure pomoću Eclipse]: http://go.microsoft.com/fwlink/?LinkID=264703
[Kako koristiti rasteretite SSL]: http://go.microsoft.com/fwlink/?LinkID=699545
[Instalacija alata za Azure za Eclipse]: http://go.microsoft.com/fwlink/?LinkId=699546
[Lokalno spremište svojstava]: http://go.microsoft.com/fwlink/?LinkID=699525#local_storage_properties
[Klijentski API za Microsoft Azure]: http://go.microsoft.com/fwlink/?LinkId=280397
[Svojstva poslužitelja za konfiguraciju]: http://go.microsoft.com/fwlink/?LinkID=699525#server_configuration_properties
[Sesije afinitet]: http://go.microsoft.com/fwlink/?LinkID=699548
[SSL rasteretite]: http://go.microsoft.com/fwlink/?LinkID=699549
[Da biste stvorili novi račun za pohranu]: http://go.microsoft.com/fwlink/?LinkID=699528#create_new
[Virtualnog računala i veličine servisa oblaka za Azure]: http://go.microsoft.com/fwlink/?LinkId=466520

<!-- IMG List -->

[ic710876]: ./media/azure-toolkit-for-eclipse-whats-new/ic710876.png
[ic710877]: ./media/azure-toolkit-for-eclipse-whats-new/ic710877.png
[ic710879]: ./media/azure-toolkit-for-eclipse-whats-new/ic710879.png
[ic710880]: ./media/azure-toolkit-for-eclipse-whats-new/ic710880.png
[ic710881]: ./media/azure-toolkit-for-eclipse-whats-new/ic710881.png
[ic710876]: ./media/azure-toolkit-for-eclipse-whats-new/ic710876.png
[ic710882]: ./media/azure-toolkit-for-eclipse-whats-new/ic710882.png
[ic710883]: ./media/azure-toolkit-for-eclipse-whats-new/ic710883.png
