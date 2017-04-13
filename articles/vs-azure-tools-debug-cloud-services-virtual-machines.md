<properties 
    pageTitle="Ispravljanje pogrešaka Azure u oblaku ili virtualnog računala u Visual Studio | Microsoft Azure"
    description="Ispravljanje pogrešaka u Oblaku ili virtualnog računala u Visual Studio"
    services="visual-studio-online"
    documentationCenter="na"
    authors="TomArcher"
    manager="douge"
    editor="" />
<tags 
    ms.service="visual-studio-online"
    ms.devlang="multiple"
    ms.topic="article"
    ms.tgt_pltfrm="multiple"
    ms.workload="na"
    ms.date="08/15/2016"
    ms.author="tarcher" />

# <a name="debugging-an-azure-cloud-service-or-virtual-machine-in-visual-studio"></a>Ispravljanje pogrešaka Azure u oblaku ili virtualnog računala u Visual Studio

Visual Studio daje različite mogućnosti za ispravljanje pogrešaka servise u oblaku Azure i virtualnih računala.



## <a name="debug-your-cloud-service-on-your-local-computer"></a>Ispravljanje pogrešaka u oblaku na lokalnom računalu

Uštedjet ćete vrijeme i novac pomoću na Azure izračunati emulator za ispravljanje pogrešaka u oblaku na lokalnom računalu. Po ispravljanje pogrešaka servisa lokalno prije njegove implementacije kod, možete poboljšati pouzdanost i performanse bez plaćanja računalnim put. Međutim, neke pogreške mogu se pojaviti samo kada pokrenete servis u oblaku u Azure sam. Ako omogućite daljinsko uklanjanje programskih pogrešaka kada objaviti na servisu, a zatim priložite program za ispravljanje pogrešaka uloga instance možete ispraviti pogreške te pogreške.

Na emulator simulira servisa Azure izračunati i pokreće se u vašem lokalnom okruženju tako da možete testirati i ispravljanje pogrešaka servis u oblaku prije njegove implementacije kod. Na emulator rukuje životni ciklus instanci uloga i omogućuje pristup Simulirani resurse, kao što su lokalno spremište. Kada ispravljanje pogrešaka ili pokrenuti na servisu s Visual Studio, automatski se pokreće na emulator kao pozadinu aplikacija i zatim uvodi na servisu da biste na emulator. Na emulator možete koristiti da biste pogledali na servisu kada se pokrene u lokalnom okruženju. Možete pokrenuti na punu verziju ili eksplicitnih verziju na emulator. (Počevši od Azure 2.3, eksplicitnih verziju na emulator je zadana.) Potražite u članku [Korištenje Emulator Express da biste pokrenuli i servise u Oblaku lokalno za ispravljanje pogrešaka](https://msdn.microsoft.com/library/dn339018.aspx).

### <a name="to-debug-your-cloud-service-on-your-local-computer"></a>Za ispravljanje pogrešaka u oblaku na lokalnom računalu

1. Na traci izbornika odaberite **ispravljanje pogrešaka**, **Pokrenite ispravljanje pogrešaka** da biste pokrenuli projekta servisa Azure oblaka. Umjesto toga, pritisnite F5. Vidjet ćete poruku koja se pokreće Emulator izračunati. Kada se pokrene u emulator, palete sustava potvrđuje ga.

    ![Azure emulator u paleti sustava](./media/vs-azure-tools-debug-cloud-services-virtual-machines/IC783828.png)

1. Prikaz korisničkog sučelja za emulator računalnim tako da otvorite izbornik prečaca za Azure ikonu u području obavijesti, a zatim odaberite **Prikaži izračunati Emulator korisničkog Sučelja**.

    U lijevom oknu korisničkog sučelja prikazuje servise koji su trenutno raspoređeni emulator računalnim i instance uloge koje se pokreće svaki servis. Možete odabrati servisa ili uloge da biste prikazali životni ciklus i Zapisivanje dijagnostičkih informacija u desnom oknu. Ako u Gornja margina uključene prozor stavite fokus, polje će se proširiti ispuni desno okno.

1. Odabirom naredbe na izborniku **za ispravljanje pogrešaka** i postavljanje prekidne točke u kodu Prođite aplikacije. Kao što je Prođite aplikaciju u program za ispravljanje pogrešaka okna se ažuriraju trenutni status aplikacija. Kada zaustavite ispravljanje pogrešaka, briše se implementaciju aplikacije. Ako aplikacija sadrži web uloga i postavite svojstvo akcija pokretanje da biste pokrenuli web-preglednik, Visual Studio pokreće web-aplikacije u pregledniku. Ako promijenite broj instanci uloga u Konfiguracija servisa, morate zaustaviti servis u oblaku i pa ponovno pokrenite tako da možete ispraviti pogreške tih novih instanci uloge za ispravljanje pogrešaka.

    **Bilješke:** Kada zaustavite radi ili usluge za ispravljanje pogrešaka, zaustaviti nisu lokalni računalnim emulator i emulator prostora za pohranu. Ih morate zaustaviti izričito iz područja obavijesti.


## <a name="debug-a-cloud-service-in-azure"></a>Ispravljanje pogrešaka u oblaku Azure

Za ispravljanje pogrešaka u oblaku na udaljenom računalu, morate omogućiti tu funkcionalnost izričito ako pokrenete servis u oblaku tako da se potrebno services (msvsmon.exe, na primjer) instaliranih na virtualnim strojevima koji se izvode vaša uloga instance. Ako niste omogućili daljinsko uklanjanje programskih pogrešaka prilikom objavljivanja servis, morate ponovno objaviti servis s udaljenog ispravljanje pogrešaka omogućena.

Ako omogućite daljinsko uklanjanje programskih pogrešaka za servise u oblaku, ne imati su smanjene performanse ili doći do dodatnih troškova. Bi trebalo koristiti daljinsko uklanjanje programskih pogrešaka servis za proizvodnju jer klijenti koje koriste servis mogu negativno utjecati na.

>[AZURE.NOTE] Kad objavite oblaka servisa Visual Studio, možete omogućiti **IntelliTrace** za sve uloge u servis koji 4 za .NET Framework ili 4,5 za .NET Framework. Pomoću **IntelliTrace**pregledajte događaje do kojih je došlo u ulozi instanci u prošlosti i reproduciranje kontekstu iz tog razdoblja. U odjeljku [ispravljanje pogrešaka na objavljeni u oblaku s IntelliTrace i Visual Studio](http://go.microsoft.com/fwlink/?LinkID=623016) i [Pomoću IntelliTrace](https://msdn.microsoft.com/library/dd264915.aspx).

### <a name="to-enable-remote-debugging-for-a-cloud-service"></a>Da biste omogućili daljinsko uklanjanje programskih pogrešaka za servise u oblaku

1. Otvaranje izbornika prečaca za Azure projekta, a zatim odaberite **Objavi**.

1. Odaberite okruženje **pripremna** i konfiguracija **za ispravljanje pogrešaka** .

    To je samo smjernica. Možete odabrati da biste pokrenuli vašeg okruženja za testiranje u radnom okruženju. Međutim, možete oštetiti korisnika Ako omogućite daljinsko uklanjanje programskih pogrešaka u radnom okruženju. Možete odabrati konfiguracije izdanja, ali konfiguracije ispravljanje pogrešaka čini olakšavaju pogrešaka.

    ![Odaberite konfiguraciju za ispravljanje pogrešaka](./media/vs-azure-tools-debug-cloud-services-virtual-machines/IC746717.gif)

1. Slijedite korake za uobičajene, ali potvrdite okvir **Omogući udaljene ispravljanje pogrešaka za sve uloge** dijaloški okvir **Napredne postavke** .

    ![Konfiguracija za ispravljanje pogrešaka](./media/vs-azure-tools-debug-cloud-services-virtual-machines/IC746718.gif)

### <a name="to-attach-the-debugger-to-a-cloud-service-in-azure"></a>Da biste priložili program za ispravljanje pogrešaka u oblaku Azure

1. U programu Explorer poslužitelja Proširite čvor za servis u oblaku.

1. Otvaranje izbornika prečaca za uloge i uloge instancu koju želite priložiti, a zatim **Priložite alat za ispravljanje pogrešaka**.

    Ako je ispraviti pogreške u ulogu ispravljanje pogrešaka za Visual Studio pridružuje svaku instancu te uloge. Program za ispravljanje pogrešaka će prekinuti na točku prekida za prvu instancu uloga koji pokreće tom retku kod zadovoljava sve uvjete tu točku prekida. Ako ispravljanje pogrešaka instancu i ispravljanje pogrešaka pridružuje samo tu instancu i prijelome na točku prekida samo kada tu instancu izvodi tom retku kod, a ispunjavaju uvjete za točku prekida.

    ![Prilaganje ispravljanje pogrešaka](./media/vs-azure-tools-debug-cloud-services-virtual-machines/IC746719.gif)

1. Kada program za ispravljanje pogrešaka pridružuje instance, ispravljanje pogrešaka kao i obično. Program za ispravljanje pogrešaka automatski pridružuje postupka odgovarajuće glavno računalo za vašu ulogu. Ovisno o tome što je ulogu, program za ispravljanje pogrešaka pridružuje w3wp.exe, WaWorkerHost.exe ili WaIISHost.exe. Da biste potvrdili postupak koji je pridružen program za ispravljanje pogrešaka, proširite čvor instancu u programu Explorer poslužitelja. Dodatne informacije o Azure procesi potražite u članku [Arhitektura Azure uloge](http://blogs.msdn.com/b/kwill/archive/2011/05/05/windows-azure-role-architecture.aspx) .

    ![Kod vrsta dijaloški okvir Odabir](./media/vs-azure-tools-debug-cloud-services-virtual-machines/IC718346.png)

1. Da biste odredili procesa koji je pridružen program za ispravljanje pogrešaka, otvorite procesa dijaloški okvir, na izborniku traci odabirom mogućnosti za ispravljanje pogrešaka, Windows, a zatim procesa. (Tipkovni: Ctrl + Alt + Z) Odvajanje određenog procesa, otvorite njezin izbornički prečac, a zatim **Odvajanje postupak**. Ili, pronađite čvor instancu u programu Explorer poslužitelj, pronađite postupka, otvorite njezin izbornički prečac, a zatim **Odvajanje postupak**.

    ![Postupke za ispravljanje pogrešaka](./media/vs-azure-tools-debug-cloud-services-virtual-machines/IC690787.gif)

>[AZURE.WARNING] Izbjegavanje dugo tabulatora pri prekidne točke kad je Udaljena ispravljanje pogrešaka. Azure tretira procesa koji se zaustavio dulje od nekoliko minuta kao ne reagira i slati promet na tu instancu. Ako zaustavite predugo msvsmon.exe odvaja od postupka.

Odvajanje ispravljanje pogrešaka iz svih procesa u ulogu ili instancu, otvaranje izbornika prečaca za ulogu ili instancu koju ste ispravljanje pogrešaka, a zatim **Odvajanje alat za ispravljanje pogrešaka**.

## <a name="limitations-of-remote-debugging-in-azure"></a>Ograničenja daljinsko uklanjanje programskih pogrešaka u Azure

Iz Azure SDK 2,3 daljinsko uklanjanje programskih pogrešaka ima sljedeća ograničenja.

- S udaljenog ispravljanje pogrešaka omogućena, nije moguće objaviti neki servis u oblaku u kojem sve uloga ima više od 25 instance.

- Program za ispravljanje pogrešaka koristi priključke 30400 i 30424, 31400 i 31424 i 32400 i 32424. Ako pokušate koristiti bilo koju od tih priključaka, nećete moći objaviti na servisu i jedan od sljedećih poruka o pogrešci pojavit će se u zapisnik aktivnosti za Azure: 

    - Pogreška u provjeri valjanosti datoteke .cscfg protiv .csdef datoteku. 
    Rezervirane priključak raspon "raspon" za krajnju točku Microsoft.WindowsAzure.Plugins.RemoteDebugger.Connector uloge 'uloga' preklapa s već definiranih ili raspona.
    - Nije uspjela dodjela. Pokušajte ponovno kasnije, pokušati smanjiti veličinu VM ili broj instanci uloga ili pokušajte implementacija na drugo područje.


## <a name="debugging-azure-virtual-machines"></a>Ispravljanje pogrešaka Azure virtualnim strojevima

Možete ispraviti pogreške programe koji se izvode na Azure virtualnim strojevima pomoću programa Explorer poslužitelja u Visual Studio. Kada omogućite daljinsko uklanjanje programskih pogrešaka na Azure virtualnog računala, Azure instalira udaljene proširenja za ispravljanje pogrešaka na virtualnog računala. Nakon toga priložite procese na virtualnog računala i ispravljanje pogrešaka kao što to obično činite.

>[AZURE.NOTE] Pomoću programa Explorer oblak u Visual Studio 2015 može daljinski debugged virtualnim strojevima stvorili kroz stog Upravitelj Azure resursa. Dodatne informacije potražite u članku [Upravljanje resursima Azure pomoću programa Explorer oblaka](http://go.microsoft.com/fwlink/?LinkId=623031).

### <a name="to-debug-an-azure-virtual-machine"></a>Za ispravljanje pogrešaka Azure virtualnog računala

1. U programu Explorer poslužitelja Proširite čvor virtualnim računalima sustava, a zatim odaberite čvor virtualnog računala koje želite za ispravljanje pogrešaka.

1. Otvaranje kontekstnog izbornika i odaberite **Omogući ispravljanje pogrešaka**. Kada se pojavi pitanje jeste li sigurni ako želite omogućiti ispravljanje pogrešaka na virtualnog računala, odaberite **da**.

    Azure instalira udaljene proširenja za ispravljanje pogrešaka na virtualnog računala da biste omogućili ispravljanje pogrešaka.

    ![Virtualni stroj omogućiti naredbu za ispravljanje pogrešaka](./media/vs-azure-tools-debug-cloud-services-virtual-machines/IC746720.png)

    ![Zapisnik aktivnosti Azure](./media/vs-azure-tools-debug-cloud-services-virtual-machines/IC746721.png)

1. Završetku udaljene proširenja za ispravljanje pogrešaka instalacije, otvorite virtualnog računala Kontekstni izbornik, a zatim je odaberite **Priloži ispravljanje pogrešaka...**

    Azure može vidjeti popis procese na virtualnog računala i prikazuje ih u prilaganje postupak dijaloški okvir.

    ![Naredba za ispravljanje pogrešaka Priloži](./media/vs-azure-tools-debug-cloud-services-virtual-machines/IC746722.png)

1. U dijaloškom okviru **Dodaj privitak u tijeku** odaberite **Odaberite** da biste ograničili na popisu rezultata da biste prikazali samo vrste kod koji želite za ispravljanje pogrešaka. Možete ispraviti pogreške 32 - ili 64-bitni upravljani kod, izvorni kod ili i jedno i drugo.

    ![Kod vrsta dijaloški okvir Odabir](./media/vs-azure-tools-debug-cloud-services-virtual-machines/IC718346.png)

1. Odaberite procesa želite ispraviti pogreške na virtualnog računala, a zatim odaberite **Dodaj privitak**. Ako, na primjer, možete postupka w3wp.exe ako ste željeli za ispravljanje pogrešaka u web-aplikacijama na virtualnog računala. Dodatne informacije potražite u [ispravljanje pogrešaka jedan ili više procesa u Visual Studio](https://msdn.microsoft.com/library/jj919165.aspx) i [Arhitektura Azure uloge](http://blogs.msdn.com/b/kwill/archive/2011/05/05/windows-azure-role-architecture.aspx) .

## <a name="create-a-web-project-and-a-virtual-machine-for-debugging"></a>Stvaranje projekta web i virtualnog računala za ispravljanje pogrešaka

Prije objavljivanja Azure projekta, vam može poslužiti ga da biste testirali u okruženju sadržavao koji podržava ispravljanje pogrešaka i testiranje scenariji, a koje možete instalirati testiranje i programi za nadzor. Jedan od načina je za daljinsko ispravljanje pogrešaka aplikacije na virtualnog računala.

Visual Studio ASP.NET projekti nudi mogućnost da biste stvorili pri ruci virtualnog računala koje možete koristiti za testiranje aplikacije. Virtualnog računala sadrži često potrebno krajnje točke kao što su PowerShell, udaljene radne površine i WebDeploy.

### <a name="to-create-a-web-project-and-a-virtual-machine-for-debugging"></a>Da biste stvorili web-mjesto projekta web i u okvir za virtualnog računala za ispravljanje pogrešaka

1. U Visual Studio, stvorite novu ASP.NET web-aplikaciju.

1. U dijaloškom okviru novi projekt ASP.NET, u odjeljku Azure odaberite **virtualnog računala** u okvir padajućeg popisa. Ostavite **udaljenim resursima za stvaranje** poništavati potvrdni okvir. Odaberite **u redu** da biste nastavili.

    Pojavit će se dijaloški okvir **Stvaranje virtualnog računala na Azure** .


    ![Stvaranje ASP.NET web povezanih prstena](./media/vs-azure-tools-debug-cloud-services-virtual-machines/IC746723.png)

    **Bilješke:** Će se zatražiti da biste se prijavili račun za Azure ako se još niste prijavili.

1. Odaberite različite postavke za virtualnog računala, a zatim odaberite **u redu**. Dodatne informacije potražite u članku [virtualnih računala]( http://go.microsoft.com/fwlink/?LinkId=623033) .

    Naziv unesite naziv DNS bit će naziv virtualnog računala. 

    ![Stvaranje virtualnog računala Azure u dijaloškom okviru](./media/vs-azure-tools-debug-cloud-services-virtual-machines/IC746724.png)

    Azure stvara sustava virtualnog računala, a zatim dodjeljuje resurse i konfigurira krajnje točke, kao što su udaljene radne površine i implementacija Web



1. Nakon što potpuno konfiguriran virtualnog računala, u programu Explorer poslužitelja odaberite čvor virtualnog računala.

1. Otvaranje kontekstnog izbornika i odaberite **Omogući ispravljanje pogrešaka**. Kada se pojavi pitanje jeste li sigurni ako želite omogućiti ispravljanje pogrešaka na virtualnog računala, odaberite **da**. 

    Azure instalira udaljene pogrešaka proširenje virtualnog računala da biste omogućili ispravljanje pogrešaka.

    ![Virtualnog računala omogućiti naredbu za ispravljanje pogrešaka](./media/vs-azure-tools-debug-cloud-services-virtual-machines/IC746720.png)

    ![Zapisnik aktivnosti Azure](./media/vs-azure-tools-debug-cloud-services-virtual-machines/IC746721.png)

1. Objavite projekt, kao što je vidljivo [Kako: implementacija web Web projekt pomoću jednim klikom objaviti u Visual Studio](https://msdn.microsoft.com/library/dd465337.aspx). Budući da za ispravljanje pogrešaka na virtualnog računala, na stranici **Postavke** čarobnjaka za **Objavljivanje Web** kao konfiguraciju odaberite **ispravljanje pogrešaka** . Ovime kod simboli jesu li dostupne tijekom ispravljanje pogrešaka.

    ![Postavke objavljivanja](./media/vs-azure-tools-debug-cloud-services-virtual-machines/IC718349.png)

1. U odjeljku **Mogućnosti objavljivanja datoteke**, odaberite **Uklanjanje dodatne datoteke na odredištu** ako projekt već je uveden u starijim vrijeme.

1. Kada projekt objavljuje na virtualnog računala Kontekstni izbornik u programu Explorer poslužitelj, odaberite **Priloži ispravljanje pogrešaka...**

    Azure može vidjeti popis procese na virtualnog računala i prikazuje ih u prilaganje postupak dijaloški okvir.

    ![Naredba za ispravljanje pogrešaka Priloži](./media/vs-azure-tools-debug-cloud-services-virtual-machines/IC746722.png)

1. U dijaloškom okviru **Dodaj privitak u tijeku** odaberite **Odaberite** da biste ograničili na popisu rezultata da biste prikazali samo vrste kod koji želite za ispravljanje pogrešaka. Možete ispraviti pogreške 32 - ili 64-bitni upravljani kod, izvorni kod ili i jedno i drugo.

    ![Kod vrsta dijaloški okvir Odabir](./media/vs-azure-tools-debug-cloud-services-virtual-machines/IC718346.png)

1. Odaberite procesa želite ispraviti pogreške na virtualnog računala, a zatim odaberite **Dodaj privitak**. Ako, na primjer, možete postupka w3wp.exe ako ste željeli za ispravljanje pogrešaka u web-aplikacijama na virtualnog računala. Dodatne informacije potražite [ispravljanje pogrešaka jedan ili više procesa u Visual Studio](https://msdn.microsoft.com/library/jj919165.aspx) .

## <a name="next-steps"></a>Daljnji koraci

- Omogućuje prikupljanje zapisnika poziva i događaje s poslužitelja izdanje **Intellitrace** . Pogledajte [na objavljeni u Oblaku s IntelliTrace i Visual Studio za ispravljanje pogrešaka](http://go.microsoft.com/fwlink/?LinkID=623016).
- Koristite **Azure Dijagnostika** za prijavu detaljne informacije iz kod koji se izvodi unutar uloge, hoće li uloge se izvode u okruženje za razvoj ili Azure. Potražite u članku [Prikupljanje zapisivanje podataka korištenjem dijagnostike Azure](http://go.microsoft.com/fwlink/p/?LinkId=400450).
