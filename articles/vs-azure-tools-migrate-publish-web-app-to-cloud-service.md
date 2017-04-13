<properties
   pageTitle="Kako se migrirati i objavljivanje web-aplikacije sa servisom Azure oblaka iz Visual Studio | Microsoft Azure"
   description="Saznajte kako migrirati i objavljivanje web-aplikacije sa servisom Azure oblak pomoću Visual Studio."
   services="visual-studio-online"
   documentationCenter="na"
   authors="TomArcher"
   manager="douge"
   editor="" />
<tags
   ms.service="multiple"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="multiple"
   ms.date="08/15/2016"
   ms.author="tarcher" />

# <a name="how-to-migrate-and-publish-a-web-application-to-an-azure-cloud-service-from-visual-studio"></a>Kako: migriranje i objavljivanje web-aplikacije sa servisom Azure oblaka iz Visual Studio

Da biste preuzeli prednost usluge hostinga i skalabilnost Azure, možda ćete morati migrirati i objavljivanje web-aplikacije sa servisom Azure oblaka. Web-aplikaciju možete pokrenuti u Azure s minimalnim promjenama s postojećom aplikacijom.

>[AZURE.NOTE] U ovoj se temi se o implementaciji servise u oblaku, ne na web-mjesta. Informacije o implementaciji na web-mjesta potražite u članku [uvođenja web app u aplikacije servisa za Azure](./app-service-web/web-sites-deploy.md).

Popis određene predloške koje su podržane za Visual C# i Visual Basic, potražite u odjeljku **Podržani predlošcima projekata** u nastavku ovog članka.

Najprije morate omogućiti web-aplikacija za Azure iz Visual Studio. Sljedeća ilustracija prikazuje ključne korake da biste objavili postojeću web-aplikaciju dodavanjem Azure projekta za implementaciju. Ovaj postupak dodaje Azure projekta s ulogom potrebna web rješenje. Ovisno o vrsti web projekta koji imate, svojstva projekta za sklopova i ažuriraju ako paket servisa zahtijeva dodatne skupine radi implementacije.

![Objavljivanje web-aplikaciji Microsoft Azure](./media/vs-azure-tools-migrate-publish-web-app-to-cloud-service/IC748917.png)

>[AZURE.NOTE] Za **Pretvaranje**, naredba **Pretvori u Azure oblaka servisa Project** prikazat će se samo za project web u rješenje. Ako, na primjer, naredba nije dostupna za projekt Silverlight u rješenje.
Kada stvorite paket servisa ili objavljivanje aplikacija za Azure, upozorenja ili pogreške mogu se pojaviti. Ove upozorenja i pogreške omogućuju otklanjanje problema vezanih uz prije implementacije Azure. Na primjer, možda ćete primiti upozorenje o nedostaju skupa. Dodatne informacije o sva upozorenja Smatraj pogrešaka potražite u članku [Konfiguriranje projekta servisa oblaka Azure s Visual Studio](vs-azure-tools-configuring-an-azure-project.md). Ako sastavljanje aplikacije, pokretati lokalno pomoću emulator računalnim ili ga objavite Azure, možda će se prikazati sljedeća pogreška u prozoru **Popis pogreške** : **su predugi navedenom parametru path, naziv datoteke ili i jedno i drugo**. Ta se pogreška pojavljuje jer je predugačak duljinu naziva potpuno kvalificiran Azure projekta. Dužina naziva projekta, uključujući cijeli put ne može biti više 146 znakova. Na primjer, to je naziv projekata uključujući put datoteke za Azure projekta koji je stvoren za aplikaciju za Silverlight: `c:\users\<user name>\documents\visual studio 2015\Projects\SilverlightApplication4\SilverlightApplication4.Web.Azure.ccproj`. Možda ćete morati premjestiti rješenje drugi direktorij koja ima kraći put do Skratite naziv potpuno kvalificiran projekta.

Migriranje i objavljivanje web-aplikaciju za Azure s Visual Studio, slijedite ove korake.

## <a name="enable-a-web-application-for-deployment-to-azure"></a>Omogućivanje web-aplikaciju za implementaciju za Azure

### <a name="to-enable-a-web-application-for-deployment-to-azure"></a>Da biste omogućili web-aplikaciju za implementaciju za Azure

1. Da biste omogućili web-aplikaciju za implementaciju za Azure, otvorite izbornik prečaca za projekt web u rješenje te odaberite dodavanje projekta za implementaciju Azure.

    Pojavljuju se sljedeće radnje:

    - Pod nazivom Azure projekta `<name of the web project>.Azure` dodaje se rješenja za svoju aplikaciju.

    - Uloga web za web project dodaje se taj Azure projekt.

    - **Lokalnu kopiju** svojstvo je postavljeno na true za sve skupovi koji su potrebni za MVC MVC 2, MVC 3 4 i Silverlight poslovnim aplikacijama. Time se dodaje te sklopova paket servisa koji se koristi za implementaciju.

  >[AZURE.IMPORTANT] Ako imate sklopova drugih datoteka koje su potrebne za ovu web-aplikaciju, morate ručno postaviti svojstva za te datoteke. Informacije o postavljanju tih svojstava potražite u odjeljku **Uključiti datoteke u paketu servisa** u nastavku ovog članka.

  >[AZURE.NOTE] Ako ulogu web za projekt određene web već postoji u Azure projekta u rješenje **pretvoriti**, **pretvorite Azure oblaka servisa Project** ne prikazuju se na izborniku prečaca za taj projekt web.

  Ako imate više projekata web u web-aplikaciji, a želite stvoriti web uloge za svaki projekt web, morate poduzeti korake ovog postupka za svaki projekt web. Ovako možete stvoriti zaseban Azure projekata za svaku ulogu web. Svaki projekt web mogu objavljivati zasebno. Osim toga, možete ručno dodati drugu ulogu web u postojeći projekt Azure u web-aplikaciji. Da biste to učinili, otvorite izbornik prečaca za mape **uloge** u Azure projektu, odaberite **Dodaj**, a zatim **Web uloga projekta u rješenje**, odaberite projekta da biste dodali kao uloge web, a zatim odaberite gumb **u redu** .

## <a name="use-an-azure-sql-database-for-your-application"></a>Korištenje baze podataka Azure SQL aplikacije

Ako imate niz za povezivanje za web-aplikaciju koja koristi baze podataka SQL Server koja je na lokalno, morate promijeniti niz veze da biste koristili instance komponente SQL baze podataka koji sadrži Azure umjesto toga.

>[AZURE.IMPORTANT] Vaša pretplata morate omogućiti korištenje baze podataka SQL. Ako vaša pretplata pristupiti s [portala za Azure klasični](http://go.microsoft.com/fwlink/?LinkID=213885), možete odrediti koji su servisi omogućuje pretplatu. Sljedeće upute odnose na objavljenu [Azure klasični portal](http://go.microsoft.com/fwlink/?LinkID=213885). Ako koristite [Azure portal](http://portal.microsoft.com), prijeđite na sljedeći postupak.

### <a name="to-use-a-sql-database-instance-in-your-web-role-for-your-connection-string"></a>Da biste koristili instancu komponente SQL baze podataka u sklopu vaše web-uloge niz za povezivanje

1. Da biste stvorili instance komponente baze podataka SQL [Azure klasični portal](http://go.microsoft.com/fwlink/?LinkID=213885), slijedite korake u sljedećem članku: [Stvaranje SQL Server za bazu podataka](http://go.microsoft.com/fwlink/?LinkId=225109).

    >[AZURE.NOTE] Kada postavite pravila vatrozida za instanci programa SQL baze podataka, morate odabrati potvrdni okvir **Dopusti drugih Azure servisa za pristup ovaj poslužitelj** .

1. Da biste stvorili instance komponente SQL baze podataka za niz za povezivanje, slijedite korake u sljedećem odjeljku u sljedećem članku: [Stvaranje SQL baze podataka](http://go.microsoft.com/fwlink/?LinkId=225110).

1. Da biste kopirali niz za povezivanje servisa za ADO.NET za niz za povezivanje, [Azure klasični portal](http://go.microsoft.com/fwlink/?LinkID=213885)poduzeti sljedeće korake.  

  1. Odaberite gumb **baze podataka** , a zatim otvorite čvor za pretplatu u koju ste koristili za stvaranje instanci programa SQL baze podataka.

  1. Da biste prikazali dostupne instance SQL baze podataka, odaberite čvor **Baze podataka SQL** .

  1. Da biste prikazali svojstva za bazu podataka, odaberite bazu podataka. Pojavit će se prikaz **svojstava** .

      >[AZURE.NOTE] Prikaz **svojstava** ne prikazuje, možda ćete morati otvoriti u programu razdjelnik.

  1. Da biste prikazali nizove veze, odaberite gumb tri točke (...) uz prikaz.

    Pojavit će se dijaloški okvir **Nizove veze** .

  1. Da biste kopirali niz za povezivanje servisa za ADO.NET, označite tekst pa odaberite tipki Ctrl + C.

  1. Da biste zatvorili dijaloški okvir, odaberite gumb **Zatvori** .

1. Da biste zamijenili niz za povezivanje u datoteci web.config da biste koristili ovu instancu SQL baze podataka, otvaranje web.config datoteke, istaknite u postojećem unosu niz veze, a zatim odaberite tipki Ctrl + V. Niz za povezivanje servisa za ADO.NET instance baze podataka SQL zamjenjuje postojeće niz za povezivanje.

1. Morate dodati i parametar `MultipleActiveResultSets=True` u nizu za povezivanje. Niz za povezivanje mora imati sljedećem obliku:

    ```
    connectionString=”Server=tcp:<database_server>.database.windows.net,1433;Database=<database_name>;User ID=<user_name>@<database_server>;Password=<myPassword>;Trusted_Connection=False;Encrypt=True;MultipleActiveResultSets=True"
    ```

1. (Neobavezno) Neke druge metode mijenjanju niz za povezivanje izravno u datoteci web.config tako da dodate sekcije u jednu transformaciju datoteke web.config, ovisno o konfiguraciji Sastavi koju koristite za stvaranje paketa za servis. Otvorite datoteku Web.Debug.Config ili Web.Release.Config datoteku. Dodajte sljedeći odjeljak u ovoj datoteci:

    ```
    XMLCopy<connectionStrings><addname="DefaultConnection"connectionString="Server=tcp:<database_server>.database.windows.net,1433;Database=<database_name>;User ID=<user_name>@<database_server>;Password=<myPassword>;Trusted_Connection=False;Encrypt=True;MultipleActiveResultSets=True"xdt:Transform="SetAttributes"xdt:Locator="Match(name)"/></connectionStrings>
    ```

1. Spremite datoteku koju ste izmijenili i ponovno objavite aplikaciju.

### <a name="to-use-an-instance-of-sql-database-by-using-the-azure-classic-portal"></a>Da biste koristili instance komponente SQL baze podataka pomoću portala za Azure klasični

1. [Azure klasični portala](http://go.microsoft.com/fwlink/?LinkID=213885)odaberite čvor baze podataka SQL.

  - Ako se pojavi instancu SQL baze podataka koji želite koristiti, odaberite da biste ga otvorili.

  - Ako niste stvorili sve instance, odaberite odgovarajuću vezu, a zatim stvoriti instancu.

1. Kada otvorite ili stvorite instanca baze podataka, odaberite vezu **Nizove veze** .

1. Pri dnu stranice, odaberite vezu da biste konfigurirali postavke vatrozida i prihvatite zadane vrijednost ili konfigurirajte vrijednosti koje su vam potrebne.

1. Kopirajte niz za povezivanje servisa za ADO.NET, zalijepite je u web.config datoteke putem stare niz za povezivanje za lokalne baze podataka i biti sigurni da biste dodali `MultipleActiveResultSets=True`.

## <a name="publish-a-web-application-to-azure"></a>Objavljivanje web-aplikaciju za Azure

### <a name="to-publish-a-web-application-to-azure"></a>Da biste objavili web-aplikaciju za Azure

1. Da biste testirali aplikacije u lokalnom razvoja okruženja pomoću na Azure izračunati emulator, otvaranje izbornika prečaca za Azure projekta za ulogu web i odaberite **Postavi kao pokretanje projekt**. Odaberite **za ispravljanje pogrešaka**, **Pokrenite ispravljanje pogrešaka** (tipkovnici: **F5**).

    Otvorit će se dijaloški okvir **Pokreni okruženje za ispravljanje pogrešaka Azure** i aplikacije pokrenuti u pregledniku. Za određene detalje o pokretanju svake vrste web-aplikacije emulator računalnim potražite u tablici u ovom odjeljku.

1. Da biste postavili servise za svoju aplikaciju za objavljivanje Azure, morate imati web-mjesto Microsoftova računa i u okvir za Azure pretplate. Slijedite korake u temi da biste postavili servisa: [Priprema da biste objavili ili uvođenje Azure aplikacije Visual Studio](vs-azure-tools-cloud-service-publish-set-up-required-services-in-visual-studio.md).

1. Da biste objavili web-aplikaciji Azure, otvorite izbornički prečac za web project, a zatim odaberite **Objavi za Azure**.

    Otvara se dijaloški okvir **Objavi Azure aplikacije** i Visual Studio pokreće postupak implementacije. Dodatne informacije o tome da biste objavili aplikacije potražite u odjeljku **Objavljivanje Azure aplikacije Visual Studio** na [neki servis u Oblaku pomoću alata za Azure za objavljivanje](vs-azure-tools-publishing-a-cloud-service.md).

    >[AZURE.NOTE] Možete objaviti i web-aplikacije iz Azure projekta. Da biste to učinili, otvorite izbornički prečac za Azure projekt, a zatim odaberite **Objavi**.

1. Da biste vidjeli tijek implementacije, možete pogledati prozor **Azure zapisnik aktivnosti** . Ovaj zapisnik automatski se prikazuje prilikom pokretanja postupka implementacije. Možete proširiti stavku u zapisnik aktivnosti da biste prikazali detaljne informacije, kao što je prikazano na sljedećoj slici:

    ![VST_AzureActivityLog](./media/vs-azure-tools-migrate-publish-web-app-to-cloud-service/IC744149.png)

1. (Neobavezno) Da biste prekinuli postupak implementacije, otvorite izbornik prečaca za stavku retka u zapisnik aktivnosti i odaberite **Odustani i ukloniti**. Time se zaustavlja se postupak implementacije i briše okruženje za implementaciju s Azure.

    >[AZURE.NOTE] Da biste uklonili okruženje za implementaciju kada je uveden, morate koristiti [Azure klasični portal](http://go.microsoft.com/fwlink/?LinkID=213885).

1. (Neobavezno) Nakon što ste pokrenuli vaša uloga instance, Visual Studio automatski prikazuje okruženje za implementaciju u čvor **Azure za izračun** u **Oblak Explorer** ili **Explorer poslužitelja**. Na tom mjestu možete prikazati stanje instanci pojedinačne uloge.

    Sljedeća ilustracija prikazuje instance uloga u **Programu Explorer poslužitelja** dok su i dalje u stanju Initializing:

    ![VST_DeployComputeNode](./media/vs-azure-tools-migrate-publish-web-app-to-cloud-service/IC744134.png)

1. Da biste pristupili aplikacija nakon implementacije, odaberite strelicu uz implementaciju sustava kada **zapisnik aktivnosti Azure**prikazuje status **Dovršeno** . Prikazat će se URL za web-aplikaciju u Azure. Potražite u sljedećoj tablici detalje o pokretanju određene vrste web-aplikacije iz Azure.

    U sljedećoj su tablici navedeni detalje o pokretanje određene web-aplikacije iz Azure ili da biste pokrenuli ili web-aplikacije lokalno pomoću Emulator za izračunavanje Azure za ispravljanje pogrešaka:

  	|Vrsta aplikacije za web|Pokreni/ispravljanje pogrešaka lokalno pomoću Emulator računalnim|Izvodi u Azure|
  	|---|---|---|
  	|ASP.NET web-aplikacije|Na traci izbornika odaberite **ispravljanje pogrešaka**, **Pokrenite ispravljanje pogrešaka** (tipkovnici: odaberite tipku **F5** .).|Odaberite hipervezu URL koji se prikazuju na kartici **implementacije** **zapisnik aktivnosti Azure** učitavanja početne stranice u pregledniku.|
  	|ASP.NET MVC 2 web-aplikacije|Na traci izbornika odaberite **ispravljanje pogrešaka**, **Pokrenite ispravljanje pogrešaka** (tipkovnici: odaberite tipku **F5** .).|Odaberite hipervezu URL koji se prikazuju na kartici **implementacije** **zapisnik aktivnosti Azure** učitavanja početne stranice u pregledniku.|
  	|ASP.NET MVC 3 web-aplikacije|Na traci izbornika odaberite **ispravljanje pogrešaka**, **Pokrenite ispravljanje pogrešaka** (tipkovnici: odaberite tipku **F5** .).|Odaberite hipervezu URL koji se prikazuju na kartici **implementacije** **zapisnik aktivnosti Azure** učitavanja početne stranice u pregledniku.|
  	|ASP.NET MVC 4 web-aplikacije|Na traci izbornika odaberite **ispravljanje pogrešaka**, **Pokrenite ispravljanje pogrešaka** (tipkovnici: odaberite tipku **F5** .).|Odaberite hipervezu URL koji se prikazuju na kartici **implementacije** **zapisnik aktivnosti Azure** učitavanja početne stranice u pregledniku.|
  	|ASP.NET prazno web-aplikacije|Morate dodati .aspx stranicu u programu koji ste postavili kao početnu stranicu web projekta. Zatim na traci izbornika odaberite **ispravljanje pogrešaka**, **Pokrenite ispravljanje pogrešaka** (tipkovnici: odaberite tipku **F5** .).|Ako imate zadane stranice .aspx u aplikaciji, odaberite URL hiperveze prikazuju na kartici **implementacije** **zapisnik aktivnosti Azure** i ova stranica je učitana u web-pregledniku. Ako imate drugu .aspx stranicu, morate otiđite na određenu stranicu pomoću sljedećih oblik za url:`<url for deployment>/<name of page>.aspx`|
  	|Aplikacija programa Silverlight|Na traci izbornika odaberite **ispravljanje pogrešaka**, **Pokrenite ispravljanje pogrešaka** (tipkovnici: odaberite tipku **F5** .).|Morate idite na određenu stranicu za svoju aplikaciju za url pomoću sljedećih oblika:`<url for deployment>/<name of page>.aspx`|
  	|Silverlight poslovnoj aplikaciji|Na traci izbornika odaberite **ispravljanje pogrešaka**, **Pokrenite ispravljanje pogrešaka** (tipkovnici: odaberite tipku **F5** .).|Morate idite na određenu stranicu za svoju aplikaciju za url pomoću sljedećih oblika:`<url for deployment>/<name of page>.aspx`|
  	|Aplikacija programa Silverlight navigacije|Na traci izbornika odaberite **ispravljanje pogrešaka**, **Pokrenite ispravljanje pogrešaka** (tipkovnici: odaberite tipku **F5** .).|Morate idite na određenu stranicu za svoju aplikaciju za url pomoću sljedećih oblika:`<url for deployment>/<name of page>.aspx`|
  	|WCF servisne aplikacije|Datoteka .svc morate postaviti kao početnu stranicu servisa WCF projekta. Zatim na traci izbornika odaberite **ispravljanje pogrešaka**, **Pokrenite ispravljanje pogrešaka** (tipkovnici: odaberite tipku **F5** .).|Morate dođite do datoteke svc za svoju aplikaciju za url pomoću sljedećih oblika:`<url for deployment>/<name of service file>.svc`|
  	|WCF tijeka rada za servisnu aplikaciju|Datoteka .svc morate postaviti kao početnu stranicu servisa WCF projekta. Zatim na traci izbornika odaberite **ispravljanje pogrešaka**, **Pokrenite ispravljanje pogrešaka** (tipkovnici: odaberite tipku **F5** .).|Morate dođite do datoteke svc za svoju aplikaciju za url pomoću sljedećih oblika:`<url for deployment>/<name of service file>.svc`|
  	|Entiteti dinamičkih platforme ASP.NET|Na traci izbornika odaberite **ispravljanje pogrešaka**, **Pokrenite ispravljanje pogrešaka** (tipkovnici: odaberite tipku **F5** .).|Morate ažurirati niz za povezivanje (pogledajte sljedeći odjeljak). Morate idite na određenu stranicu za svoju aplikaciju za url pomoću sljedećih oblika:`<url for deployment>/<name of page>.aspx`|
  	|Dinamični ASP.NET Linq podataka za SQL|Na traci izbornika odaberite **ispravljanje pogrešaka**, **Pokrenite ispravljanje pogrešaka** (tipkovnici: odaberite tipku **F5** .).|Morate slijediti korake u ovom postupku: korištenje baze podataka SQL Azure aplikacije (pogledajte odjeljak ranije u ovoj temi). Morate idite na određenu stranicu za svoju aplikaciju za url pomoću sljedećih oblika:`<url for deployment>/<name of page>.aspx`|

## <a name="update-a-connection-string-for-aspnet-dynamic-entities"></a>Ažuriranje niza za povezivanje za dinamičku ASP.NET entiteti

### <a name="to-update-a-connection-string-for-aspnet-dynamic-entities"></a>Da biste ažurirali niza za povezivanje za entiteti dinamičkih platforme ASP.NET

1. Stvaranje baze podataka SQL Azure koje je moguće koristiti za dinamičku entiteti ASP.NET web-aplikaciju, slijedite korake postupka **korištenja baze podataka SQL Azure aplikacije** u ovoj temi.

1. Dodavanje tablica i polja koja su potrebna za bazu podataka s [Azure klasični portal](http://go.microsoft.com/fwlink/?LinkID=213885).

1. Niz za povezivanje za tu vrstu aplikacije sadrži sljedeći oblik u datoteci web.config:  

    ```
    <addname="tempdbEntities"connectionString="metadata=res://*/Model1.csdl|res://*/Model1.ssdl|res://*/Model1.msl;provider=System.Data.SqlClient;provider connection string=&quot;data source=<server name>\SQLEXPRESS;initial catalog=<database name>;integrated security=True;multipleactiveresultsets=True;App=EntityFramework&quot;"providerName="System.Data.EntityClient"/>
    ```

    Ažuriranje vrijednosti *connectionString* s nizom za povezivanje servisa za ADO.NET baze podataka SQL Azure na sljedeći način:

    ```
    XMLCopy<addname="tempdbEntities"connectionString="metadata=res://*/Model1.csdl|res://*/Model1.ssdl|res://*/Model1.msl;provider=System.Data.SqlClient;provider connection string=&quot;Server=tcp:<SQL Azure server name>.database.windows.net,1433;Database=<database name>;User ID=<user name>;Password=<password>;Trusted_Connection=False;Encrypt=True;multipleactiveresultsets=True;App=EntityFramework&quot;"providerName="System.Data.EntityClient"/>
    ```

1. Da biste spremili web.config datoteke s promjenama koje ste napravili niz za povezivanje, na traci izbornika odaberite **datoteku**, **spremite web.config**.

## <a name="supported-project-templates"></a>Predlošci podržani projekata

Da biste objavili web-aplikaciju za Azure, aplikacija morate koristiti neki od predložaka programa project za C# ili Visual Basic koji je naveden u tablici u nastavku.

|Grupa predloška projekta|Predložak projekta|
|---|---|
|Web|ASP.NET web-aplikacije|
|Web|ASP.NET MVC 2 web-aplikacije|
|Web|ASP.NET MVC 3 web-aplikacije|
|Web|ASP.NET MVC4 web-aplikacije|
|Web|ASP.NET prazno web-aplikacije|
|Web|ASP.NET MVC 2 prazno web-aplikacije|
|Web|Dinamični ASP.NET podataka entiteti web-aplikaciji|
|Web|Dinamični ASP.NET Linq podataka SQL web-aplikaciju|
|Silverlight|Aplikacija programa Silverlight|
|Silverlight|Silverlight poslovnoj aplikaciji|
|Silverlight|Aplikacija programa Silverlight navigacije|
|WCF|WCF servisne aplikacije|
|WCF|WCF tijek rada servisne aplikacije|
|Tijek rada|WCF tijeka rada za servisnu aplikaciju|

## <a name="next-steps"></a>Daljnji koraci
Dodatne informacije o objavljivanje potražite u odjeljku [Priprema za objavljivanje ili uvođenje Azure aplikacije Visual Studio](vs-azure-tools-cloud-service-publish-set-up-required-services-in-visual-studio.md). I da pročitate [Postavke gore pod nazivom vjerodajnice za provjeru autentičnosti](vs-azure-tools-setting-up-named-authentication-credentials.md).
