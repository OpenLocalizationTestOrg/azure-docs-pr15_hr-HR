<properties
   pageTitle="Konfiguriranje Azure projektu pomoću više konfiguracija servisa | Microsoft Azure"
   description="Saznajte kako konfigurirati za projekt servisa Azure oblak tako da promijenite ServiceDefinition.csdef i ServiceConfiguration.cscfg datoteke."
   services="visual-studio-online"
   documentationCenter="na"
   authors="TomArcher"
   manager="douge"
   editor="" />
<tags
   ms.service="multiple"
   ms.devlang="multiple"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/15/2016"
   ms.author="tarcher" />

# <a name="configuring-your-azure-project-using-multiple-service-configurations"></a>Konfiguriranje pomoću više konfiguracija servisa Azure projekta

Za projekt servisa Azure oblaka sadrži dvije datoteke konfiguracije: ServiceDefinition.csdef i ServiceConfiguration.cscfg. Ove datoteke su isporučen servisne aplikacije za Azure oblaka i implementirati Azure.

- Datoteka **ServiceDefinition.csdef** sadrži metapodatke koje je potrebno za Azure okruženja za preduvjeti za servisnu aplikaciju oblaka, uključujući koje uloge sadrži. Datoteka sadrži i konfiguracijske postavke koje se odnose na sve instance. Postavke konfiguracije je moguće čitati prilikom izvođenja pomoću Hosting API Runtime preglednika Azure usluge. Datoteku nije moguće ažurirati pokrenutom uslugu u Azure.

- Datoteka **ServiceConfiguration.cscfg** skupova vrijednosti za postavke konfiguracije definirano u datoteci definicije servisa i određuje koliko je instanci da biste pokrenuli za svaku ulogu. Datoteka se može ažurirati pokrenutom servis u oblaku u Azure.

Alati za Azure za Microsoft Visual Studio sadrže stranice svojstava koje možete koristiti da biste postavili konfiguracijske postavke koje se pohranjuju u te datoteke. Da biste pristupili stranice svojstava, dvokliknite referencu uloga ispod projekta servisa Azure oblak u pregledniku rješenja ili desnom tipkom miša kliknite referenca uloge, a zatim odaberite **Svojstva**, kao što je prikazano na sljedećoj slici.

![VS_Solution_Explorer_Roles_Properties](./media/vs-azure-tools-multiple-services-project-configurations/IC784076.png)

Informacije o pozadini sheme za definiranje servisa i datoteke za konfiguraciju servisa potražite u članku [Referenca sheme](https://msdn.microsoft.com/library/azure/dd179398.aspx). Dodatne informacije o konfiguraciji servisa potražite [u](./cloud-services/cloud-services-how-to-configure.md)članku konfiguriranje servise u Oblaku.

## <a name="configuring-role-properties"></a>Konfiguriranje svojstava uloga

Na stranicama svojstava za web uloge i uloge suradnika su slične, iako je nekoliko razlike pokazali u sljedećim odjeljcima.

Na stranici **Međuspremanje** možete konfigurirati Azure predmemoriranje services.

### <a name="configuration-page"></a>Stranica za konfiguraciju

Na stranici **Konfiguracija** možete postaviti svojstva:

**Instance**

Postavite svojstvo count **Instance** broj instanci usluge trebale bi funkcionirati za ta uloga.

Postavite svojstvo **Veličina VM** **Dodatni Small**, **Small**, **Srednja**, **velika**ili **Vrlo velik**.  Dodatne informacije potražite u članku [veličine za servise u Oblaku](./cloud-services/cloud-services-sizes-specs.md).

**Akcije pokretanja** (Samo web uloga)

Postavljanje tog svojstva da biste odredili Visual Studio treba pokretanje web-preglednika za krajnje točke HTTP ili HTTPS krajnjih točaka, ili pak i kada pokrenete ispravljanje pogrešaka.

HTTPS krajnjoj točki mogućnost je dostupna samo ako ste već definirali krajnje HTTPS za vašu ulogu. Možete definirati krajnje HTTPS na stranici svojstva **krajnje točke** .

Ako ste već dodali krajnje HTTPS, po zadanom je omogućena mogućnost HTTPS krajnjoj točki i Visual Studio pokretanje preglednika za ovu krajnju točku prilikom pokretanja ispravljanje pogrešaka, osim preglednika za krajnju točku sustava HTTP. Pretpostavlja se da su obje mogućnosti pokretanja omogućeni.

**Dijagnostika**

Dijagnostika po zadanom je omogućena za ulogu Web. Azure oblaka projekta i pohranu račun servisa su postavljeni za korištenje emulator lokalno spremište. Kada ste spremni za implementaciju Azure, možete odabrati gumb sastavljača (****...) da biste ažurirali račun za pohranu za korištenje Azure prostora za pohranu u oblaku. Dijagnostika podatke možete prenijeti na račun za pohranu na zahtjev ili automatski zakazan intervalima. Dodatne informacije o Azure Dijagnostika potražite u članku [Omogućavanje dijagnostike Azure servise u Oblaku i virtualnih računala](./cloud-services/cloud-services-dotnet-diagnostics.md).

## <a name="settings-page"></a>Postavke stranice

Na stranici s **postavkama** možete dodati konfiguracijske postavke servisa. Postavke konfiguracije su parove naziv vrijednosti. Kod koji se izvodi u ulozi može čitati vrijednosti postavki konfiguriranje prilikom izvođenja pomoću klase nudi [Biblioteke upravlja Azure](http://go.microsoft.com/fwlink?LinkID=171026). Konkretno, metodu [GetConfigurationSettingValue](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleenvironment.getconfigurationsettingvalue.aspx) vraća vrijednost postavka imenovani konfiguracije tijekom rada.

### <a name="configuring-a-connection-string-to-a-storage-account"></a>Konfiguriranje niza za povezivanje s računom za pohranu

Niz za povezivanje je konfiguracije postavku koja omogućuje veze i provjeru autentičnosti informacija za pohranu emulator ili račun za Azure prostora za pohranu. Kad god kod morate pristupiti podacima iz servisa Azure prostora za pohranu – odnosno blob, reda čekanja ili podatke u tablici – iz kod koji se izvodi u ulozi, morat ćete odrediti niza za povezivanje za taj račun za pohranu.

Niz za povezivanje koja upućuje na račun za Azure prostora za pohranu morate koristiti definirani oblik. Informacije o stvaranju nizu za povezivanje potražite u članku [Konfiguriranje Azure prostora za pohranu nizu za povezivanje](./storage/storage-configure-connection-string.md).

Kada ste spremni ispitati usluge protiv servisa Azure prostora za pohranu ili kada ste spremni za implementaciju servis u oblaku za Azure, možete promijeniti vrijednost bilo koju vezu nizova da upućuje na svoj račun za Azure prostora za pohranu. Odaberite (****...), odaberite **vjerodajnice računa za pohranu Enter**. Unesite podatke svog računa koja sadrži naziv računa i ključ za račun. U dijaloškom okviru **Niz za povezivanje za pohranu računa** možete označiti želite li koristiti zadanu HTTPS krajnje točke (zadana mogućnost), krajnje točke HTTP zadane ili prilagođene krajnje točke. Možda odlučite koristiti prilagođeni krajnje točke ako ste registrirali naziv prilagođene domene servisa, kao što je opisano u članku [Konfiguriranje prilagođenog naziva domene za blob podataka na račun za Azure prostora za pohranu](./storage/storage-custom-domain-name.md).

>[AZURE.IMPORTANT] Morate izmijeniti nizovima veze na račun za Azure prostora za pohranu prije implementacije usluge. Nemogućnost to može uzrokovati vaša uloga ne da biste započeli ili da biste prošli kroz stanja prilikom inicijalizacije, zauzet i zaustavljanje.

## <a name="endpoints-page"></a>Stranica za krajnje točke

Uloga suradnika može imati bilo koji broj HTTP, HTTPS ili TCP krajnje točke. Krajnje točke može biti unos krajnje točke koje su dostupne za vanjski klijenti, ili interne krajnje točke koje su dostupne za druge uloge koji su pokrenuti na servisu.

- Da biste unijeli krajnje HTTP dostupna vanjski klijenti i web-preglednika, promijenite vrste krajnje točke za unos, a Navedite naziv i broj priključka javno.

- Da biste krajnje HTTPS dostupna vanjski klijenti i web-preglednika, promijenite vrstu krajnju točku **unosa**i navedite naziv, broj priključka javno i naziv certifikata za upravljanje.

    Imajte na umu da navedete certifikat upravljanja morate definirati certifikata na stranici svojstva **potvrde** .

- Da biste krajnje dostupne za interne access tako da druge uloga servisa u oblaku, promjena vrste krajnje točke za interne i navedite naziv i moguće privatne priključke za ovu krajnju točku.

## <a name="local-storage-page"></a>Lokalno spremište stranice

Da biste rezervirati resurse lokalno spremište za uloge, poslužite se stranica svojstava **Lokalno spremište** . Lokalno spremište resurs je rezervirane direktorija u datotečnom sustavu Azure virtualnog računala u kojem se izvodi instance komponente uloge.

## <a name="certificates-page"></a>Stranica certifikata

Na stranici **Potvrda** vaša uloga možete pridružiti certifikata. Certifikati koje dodate može se koristiti za konfiguriranje vaše HTTPS krajnje točke na stranici svojstva **krajnje točke** .

Stranica svojstava **potvrde** dodaje informacije o certifikatima konfiguraciju servisa. Imajte na umu da certifikati su pakirat sa servisom; ne morate prenijeti certifikatima zasebno Azure putem [Azure klasični portal](http://go.microsoft.com/fwlink/?LinkID=213885).

Da biste pridružili vaša uloga certifikat, navedite naziv certifikata. Koristite ovaj naziv za upućivanje certifikata prilikom konfiguriranja krajnje HTTPS na stranici svojstva **krajnje točke** . Nakon toga navedite nalazi li se u spremište certifikata **Lokalnog računala** ili **Trenutnog korisnika** i naziva u trgovini. Na kraju, unesite otisak prsta potvrde. Ako je certifikat u trenutnom User\Personal (Moje) iz trgovine, možete unijeti otisak prsta potvrde tako da odaberete željeni certifikat popunjena popisu. Ako se nalazi na drugom mjestu, ručno unesite vrijednost otisak prsta.

Kada dodate certifikat iz spremišta certifikata, sve Srednja certifikati automatski će se dodaju konfiguracijske postavke za vas. Da bi se ispravno konfiguriranje usluge za SSL, za Azure morate moguće prenijeti ove Srednja potvrde.

Sve upravljanje certifikate koji se povezati sa servisima primjenjuju se na servisu samo kada je pokrenuta u oblaku. Kada na servisu se izvodi u lokalnom platforme, koristit će se standardni certifikat koji upravlja emulator računalnim.

## <a name="configuring-the-azure-cloud-service-project"></a>Konfiguriranje servisa project Azure oblaka

Da biste konfigurirali postavke koje se odnose na projekt programa cijelu Azure oblaka servisa, otvorite izbornički prečac za taj čvor projekta i pa odaberite Svojstva da biste otvorili njegov stranice svojstava. Sljedeća tablica prikazuje te svojstava stranice.

|Stranica svojstava|Opis|
|---|---|
|Aplikacija|S ove stranice možete prikazati informacije o verziji Azure alatima koji koristi taj projekt servisa oblaka, a možete nadograditi na trenutnu verziju alata.|
|Stvaranje događaja|S ove stranice možete postaviti događaje prije i poslije Sastavi.|
|Razvoj|S ove stranice možete odrediti upute za konfiguriranje Sastavi i uvjeta pod kojim će se izvoditi Svi događaji nakon Sastavi.|
|Web|S ove stranice možete konfigurirati postavke koje se odnose na web-poslužitelj.|
