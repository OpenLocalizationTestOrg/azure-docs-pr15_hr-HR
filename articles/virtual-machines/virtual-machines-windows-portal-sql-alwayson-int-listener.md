<properties
   pageTitle="Stvaranje ga Slušatelj AlwaysOn availabilty grupe za SQL Server na virtualnim strojevima sa sustavom Azure"
   description="Detaljne upute za stvaranje ga slušatelj za availabilty grupi AlwaysOn za SQL Server na virtualnim strojevima sa sustavom Azure"
   services="virtual-machines"
   documentationCenter="na"
   authors="MikeRayMSFT"
   manager="jhubbard"
   editor="monicar"/>

<tags
   ms.service="virtual-machines"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-windows-sql-server"
   ms.workload="infrastructure-services"
   ms.date="07/12/2016"
   ms.author="MikeRayMSFT"/>

# <a name="configure-an-internal-load-balancer-for-an-alwayson-availability-group-in-azure"></a>Konfiguriranje Interna opterećenja za grupi dostupnosti AlwaysOn u Azure

U ovoj se temi objašnjava kako stvoriti Interna opterećenja grupe dostupnosti SQL Server AlwaysOn u Azure virtualnim strojevima koji se izvodi u modelu upravitelj resursa. Dostupnost granu AlwaysOn zahtijeva raspoređivača opterećenja kada instance sustava SQL Server na virtualnim računalima za Azure. Raspoređivača opterećenja pohranjuje IP adresu da ga slušatelj dostupnost grupe. Ako je grupe dostupnosti obuhvaća višestruki područja, svako područje mora raspoređivača opterećenja.

Da biste dovršili ovaj zadatak, morate imati grupu dostupnost SQL Server AlwaysOn implementiran na Azure virtualnim strojevima u modelu upravitelj resursa. Oba sustava SQL Server na virtualnim strojevima mora pripadati isti skup dostupnost. [Predložak programa Microsoft](virtual-machines-windows-portal-sql-alwayson-availability-groups.md) možete koristiti da biste automatski stvorili grupe dostupnosti AlwaysOn u upravitelju Azure resursa. Ovaj predložak automatski stvara Interna opterećenja umjesto vas. 

Ako želite, možete ga [ručno konfiguriranje programa grupe dostupnosti AlwaysOn](virtual-machines-windows-portal-sql-alwayson-availability-groups-manual.md).

U ovoj se temi zahtijeva su već konfigurirana availablity grupe.  

Povezane teme obuhvaćaju sljedeće:

 - [Konfiguriranje grupe dostupnosti AlwaysOn u Azure VM (GUI)](virtual-machines-windows-portal-sql-alwayson-availability-groups-manual.md)   
 
 - [Konfiguriranje veze VNet VNet pomoću upravitelja resursa Azure i komponente PowerShell](../vpn-gateway/vpn-gateway-vnet-vnet-rm-ps.md)

## <a name="steps"></a>Koraci

Po prolaska kroz dokument će stvoriti i konfigurirati raspoređivača opterećenja na portalu za Azure. Po dovršetku koji će konfigurirati klaster da koristi IP adresu iz raspoređivača opterećenja da ga slušatelj AlwaysOn dostupnost grupe.

## <a name="create-and-configure-the-load-balancer-in-the-azure-portal"></a>Stvaranje i konfiguriranje raspoređivača opterećenja na portalu za Azure

U ovom dio zadatak će učinite sljedeće korake na portalu za Azure:

1. Stvaranje raspoređivača opterećenja i konfiguriranje IP adresa

1. Konfiguriranje skup pozadinskog

1. Stvaranje na probni 

1. Postavljanje pravila za ujednačavanje opterećenja

>[AZURE.NOTE] Ako SQL Server u grupe različite resursa i regija, će učinite svi ti koraci dvaput nakon u svaku grupu resursa.

## <a name="1-create-the-load-balancer-and-configure-the-ip-address"></a>1. na opterećenja stvaranje i konfiguriranje IP adresa

U prvi je korak da biste stvorili raspoređivača opterećenja. Na portalu Azure otvorite grupu resursa koji sadrži virtualnim računalima sustava SQL Server. U grupi resursa, kliknite **Dodaj**.

- Traženje **raspoređivača učitavanja**. U rezultatima pretraživanja odaberite **Opterećenja**, koji je izdala **Microsoft**.

- Na plohu **Opterećenja** kliknite **Stvori**.

- Na **Stvaranje učitavanje raspoređivača**, konfigurirati na raspoređivača opterećenja na sljedeći način:

| Postavka | Vrijednost |
| ----- | ----- |
| **Ime** | Tekst naziva koji predstavlja raspoređivača opterećenja. Na primjer, **sqlLB**. |
| **Shema** | **Interna** |
| **Virtualne mreže** | Odaberite virtualne mreže koji su na SQL Server.   |
| **Podmreže**  | Odaberite podmreže koji su na SQL Server. |
| **Pretplate** | Ako imate više pretplata, to polje može se prikazati. Odaberite pretplatu u koju želite pridružiti ovaj resurs. To je obično iste subcription kao sve resurse za grupe dostupnosti.  |
| **Grupa resursa** | Odaberite grupu resursa koji su na SQL Server. | 
| **Mjesto** | Odaberite mjesto za Azure podatke koji su na SQL Server. |

- Kliknite **Stvori**. 

Azure stvara raspoređivača opterećenja koje ste konfigurirali iznad. Raspoređivača opterećenja pripada navedena mreža, podmreže, grupa resursa i mjesto. Po završetku Azure provjerite postavke raspoređivača opterećenja servisu Azure. 

Sada možete konfigurirati IP adresa raspoređivača opterećenja.  

- Na **Postavke** plohu raspoređivača opterećenja kliknite **IP adresa**. Plohu **IP adresa** prikazuje da je ovo privatnim opterećenja na istoj virtualne mreži kao SQL poslužitelja. 

- Postavite sljedeće postavke: 

| Postavka | Vrijednost |
| ----- | ----- |
| **Podmreže** | Odaberite podmreže koji su na SQL Server. |
| **Dodjela** | **Statički** |
| **IP adresa** | Upišite Neiskorišteni virtualne IP adresu iz podmreži.  |

- Spremanje postavki.

Sada raspoređivača opterećenja je IP adresa. Zapis ovu IP adresu. Koristite ovu IP adresu prilikom stvaranja na ga slušatelj na klaster. U skriptu PowerShell u nastavku ovog članka, koristite ovu adresu za na `$ILBIP` varijabli.



## <a name="2-configure-the-backend-pool"></a>2. konfiguriranje skup pozadinskog

Sljedeći je korak da biste stvorili skupna adresa za pozadinskog. Azure poziva skup adresa pozadinskog *pozadinskog skup*. U ovom slučaju skup pozadinskog je adrese dva SQL poslužitelja u vašoj grupi dostupnost. 

- U grupu resursa, kliknite opterećenja koji ste stvorili. 

- U odjeljku **Postavke**kliknite **pozadinskog grupe**.

- Na **pozadinskog adresu grupe**, kliknite **Dodaj** da biste stvorili skupna adresa za pozadinskog. 

- Na **skup pozadinskog Dodaj** u odjeljku **naziv**upišite naziv za skup pozadinskog.

- U odjeljku **virtualnim strojevima** kliknite **+ Dodaj virtualnog računala**. 

- U odjeljku **Odabir virtualnim strojevima** kliknite **Odaberite skup dostupnosti** , a zatim navedite na availablity skupa virtualnim računalima sustava SQL Server pripadate.

- Nakon što ste odabrali postavljanje dostupnosti, kliknite **Odaberite virtualnih računala**. Kliknite dva virtualnih računala koje hostira instance sustava SQL Server u grupi dostupnost. Kliknite **Odaberi**. 

- Kliknite **u redu** da biste zatvorili u blades za **Odabir virtualnim strojevima**i **Dodavanje pozadinskog skup**. 

Azure ažurirati postavke skupna adresa pozadinskog. Dostupnost skupu se skup dva SQL poslužitelja.

## <a name="3-create-a-probe"></a>3. na probni stvaranje

Sljedeći je korak da biste stvorili na probni. Na probni definira kako će Azure provjeriti koji SQL poslužitelja trenutno je vlasnik grupe ga slušatelj dostupnost. Azure će isprobati usluge na temelju IP adresu na priključak koje sami definirate prilikom stvaranja na probni.

- Na **Postavke** plohu raspoređivača opterećenja kliknite **Probes**. 

- Na plohu **Probes** kliknite **Dodaj**.

- Konfiguriranje na probni na plohu **probni Dodaj** . Da biste konfigurirali na probni pomoću sljedeće vrijednosti:

| Postavka | Vrijednost |
| ----- | ----- |
| **Ime** | Tekst naziva koji predstavlja na probni. Na primjer, **SQLAlwaysOnEndPointProbe**. |
| **Protokol** | **TCP** |
| **Priključak** | Možete koristiti bilo koji dostupni priključak. Na primjer, *59999*.    |
| **Interval**  | *5* | 
| **Dobro praga.**  | *2* | 

- Kliknite **u redu**. 

>[AZURE.NOTE] Provjerite je li priključak navedete otvorena na vatrozid i SQL Server. Oba poslužitelji zahtijevaju ulazna pravila za TCP priključak koji koristite. Potražite u članku [Dodavanje ili uređivanje pravila vatrozida](http://technet.microsoft.com/library/cc753558.aspx) za dodatne informacije. 

Azure stvara sustava probni. Azure koristit će se probni da biste testirali koje SQL Server sadrži ga slušatelj dostupnost grupe.

## <a name="4-set-the-load-balancing-rules"></a>4. postavljanje pravila za ujednačavanje opterećenja

Postavljanje pravila za ujednačavanje opterećenja. Pravila za ujednačavanje opterećenja konfigurirati kako raspoređivača opterećenja usmjerava promet na SQL Server. Za ovaj opterećenja će omogućiti Izravni poslužitelja povrata jer samo jedan od dva poslužitelja SQL će ikada vlasnik resursa za ga slušatelj dostupnost grupe odjednom.

- Na **Postavke** plohu raspoređivača opterećenja kliknite **Učitaj ujednačavanje pravila**. 

- Na plohu **opterećenja pravila** kliknite **Dodaj**.

- Koristite plohu **Dodaj učitavanje ujednačavanje pravila** za konfiguriranje pravila za ujednačavanje opterećenja. Pomoću sljedećih postavki: 

| Postavka | Vrijednost |
| ----- | ----- |
| **Ime** | Tekst naziva koji predstavlja pravila za ujednačavanje opterećenja. Na primjer, **SQLAlwaysOnEndPointListener**. |
| **Protokol** | **TCP** |
| **Priključak** | *1433*   |
| **Pozadinskog priključak** | *1433*. Imajte na umu da to će biti onemogućena jer to pravilo koristi **Pluta IP (Izravni server povrata)**.   |
| **Isprobati** | Nazovite probni koji ste stvorili za ovaj opterećenja. |
| **Sesije persistance**  | **Ništa** | 
| **Neaktivne Prekoračenje vremena (u minutama)**  | *4* | 
| **Plutajuća IP (Izravni server povrata)**  | **Omogućeno** | 

 >[AZURE.NOTE] Možda ćete se morati pomaknuti prema dolje na plohu da biste vidjeli sve postavke.

- Kliknite **u redu**. 

- Azure konfigurira pravilo za ujednačavanje opterećenja. Sada raspoređivača opterećenja je konfiguriran za usmjeravanje prometa SQL poslužitelja koji hostira ga slušatelj dostupnost grupe. 

Sada grupa resursa ima raspoređivača opterećenja povezati oba računala sustava SQL Server. Raspoređivača opterećenja sadrži i IP adrese da ga slušatelj SQL Server AlwaysOn availablity grupe tako da se neki računala možete odgovoriti na zahtjeve za grupe dostupnosti.

>[AZURE.NOTE] Ako SQL Server u dva zasebna područja, ponovite korake iz drugih područja. Svako područje zahtijeva raspoređivača opterećenja. 

## <a name="configure-the-cluster-to-use-the-load-balancer-ip-address"></a>Konfiguriranje klaster da koristi IP adresu raspoređivača učitavanja 

Sljedeći je korak konfiguriranje ga slušatelj na klaster i otvorili ga slušatelj putem Interneta. Da biste to postigli, učinite sljedeće: 

1. Stvaranje grupe ga slušatelj availablity na prebacivanje klaster 

1. Premjesti ga slušatelj putem Interneta

## <a name="1-create-the-availablity-group-listener-on-the-failover-cluster"></a>1. stvaranje ga slušatelj availablity grupe na prebacivanje klaster

U ovom ćete koraku ručno stvoriti grupu ga slušatelj dostupnost u upravitelju klaster prebacivanje i SQL Server Management Studio (SSMS).

- Korištenje RDP za povezivanje s Azure virtualnog računala koje hostira primarni replike. 

- Otvorite upravitelj klaster prebacivanje.

- Odaberite čvor **mreža** i zabilježite naziv mreže klaster. Taj naziv koristit će se u na `$ClusterNetworkName` varijable u skriptu PowerShell.

- Proširite naziv klaster, a zatim kliknite **uloge**.

- U oknu **uloge** , desnom tipkom miša kliknite naziv grupe dostupnosti, a zatim odaberite **Dodaj resursa** > **Klijent pristupnoj točci**.

- U okvir **naziv** stvaranje naziva za ovaj novi ga slušatelj, a zatim dvaput kliknite **Dalje** , a zatim **Završi**. Ne premjestiti ga slušatelj ili resursa na Internetu sada.

 >[AZURE.NOTE] Naziv da ga slušatelj novi je naziv mreže koju aplikacija će se koristiti za povezivanje s bazama podataka u grupi dostupnost SQL Server.

- Kliknite karticu **resursa** , a zatim proširite pristupnu točku klijenta koji ste upravo stvorili. Desnom tipkom miša kliknite IP resursa, a zatim kliknite Svojstva. Zapamtite naziv IP adresa. Koristite ovaj naziv u na `$IPResourceName` varijable u skriptu PowerShell.

- U odjeljku **IP adresa** kliknite **Statičke IP adrese** i postavite statičke IP adrese na istu adresu koju ste koristili kada postavite IP adresa raspoređivača opterećenja portala za Azure. Omogućivanje NetBIOS za tu adresu, a zatim kliknite **u redu**. Ponovite ovaj korak za svaki resurs IP Ako rješenje obuhvaća više VNets Azure. 

- Na klaster čvor koji trenutno hostira primarni replike otvorite povećane Očisti filtar i zalijepite sljedeće naredbe u novu skriptu.

        $ClusterNetworkName = "<MyClusterNetworkName>" # the cluster network name (Use Get-ClusterNetwork on Windows Server 2012 of higher to find the name)
        $IPResourceName = "<IPResourceName>" # the IP Address resource name
        $ILBIP = “<X.X.X.X>” # the IP Address of the Internal Load Balancer (ILB). This is the static IP address for the load balancer you configured in the Azure portal.
    
        Import-Module FailoverClusters
    
        Get-ClusterResource $IPResourceName | Set-ClusterParameter -Multiple @{"Address"="$ILBIP";"ProbePort"="59999";"SubnetMask"="255.255.255.255";"Network"="$ClusterNetworkName";"EnableDhcp"=0}

- Ažurirajte varijabli i pokrenuti skriptu PowerShell da biste konfigurirali IP adresa i priključaka da ga slušatelj novi.

 >[AZURE.NOTE] Ako SQL Server u zasebnom područja, morate pokrenuti skriptu PowerShell dvaput. Kada prvi put koristite klaster naziv mreže naziv resursa za IP klaster te ga učitajte opterećenja IP adrese iz prve grupe resursa. Drugi put kada koristite naziv mreže klaster, naziv resursa za IP klaster te ga učitajte opterećenja IP adrese iz druge grupe resursa.

Skupine se na dostupnost resursa ga slušatelj u grupe.

## <a name="2-bring-the-listener-online"></a>2. Premjesti ga slušatelj putem Interneta

S dostupnost grupe ga slušatelj resursa konfigurirana, možete prenijeti ga slušatelj internetu tako da aplikacije mogu se povezati s bazama podataka u grupi dostupnosti s ga slušatelj.

- Prijeđite natrag u programu prebacivanje klaster Manager. Proširite **uloge** i označite grupu dostupnost. Na kartici **resursa** , desnom tipkom miša kliknite naziv ga slušatelj, a zatim kliknite **Svojstva**.

- Kliknite karticu **ovisnosti** . Ako postoji više resursa, IP adrese provjerite imaju li ili ne AND, ovisnosti. Kliknite **u redu**.

- Desnom tipkom miša kliknite naziv ga slušatelj, a zatim kliknite **Premjesti Online**.


- Nakon što ga slušatelj je na Internetu, na kartici **Resursi** , grupe dostupnosti desnom tipkom miša, a zatim kliknite **Svojstva**.

- Stvaranje ovisnosti na ga slušatelj naziv resursa (ne IP adresa resursi naziv). Kliknite **u redu**.


- Pokrenite SQL Server Management Studio i povezati primarni replike.


- Dođite do **dostupnosti AlwaysOn visoke** | **grupe dostupnosti** | **slušače grupe dostupnosti**. 


- Prikazat će se sada ga slušatelj naziv koji ste stvorili u upravitelju za prebacivanje klaster. Desnom tipkom miša kliknite naziv ga slušatelj, a zatim kliknite **Svojstva**.


- U okviru **priključak** navedite broj priključka da ga slušatelj dostupnost grupu pomoću $EndpointPort koji ste koristili ranije (1433 je zadana postavka), zatim kliknite **u redu**.

Sada imate grupe dostupnosti SQL Server AlwaysOn na Azure virtualnim računalima sustava u načinu rada upravitelj resursa. 

## <a name="test-the-connection-to-the-listener"></a>Testiranje veze da biste ga slušatelj

Da biste testirali veze:

1. RDP sa sustavom SQL Server koja se nalazi u istom virtualne mreže, ali vlasnik replike. To može biti druge SQL Server u klasteru.

1. Koristite **sqlcmd** utility za testiranje veze. Na primjer, sljedeću skriptu uspostavlja vezu **sqlcmd** primarni replike kroz ga slušatelj s provjeru autentičnosti sustava Windows:

        sqlcmd -S <listenerName> -E

Veza SQLCMD automatsko povezivanje s bez obzira instancu sustava SQL Server hostira primarni replike. 

## <a name="guidelines-and-limitations"></a>Smjernice i ograničenja

Imajte na umu sljedeće smjernice za availablity ga slušatelj grupe u Azure pomoću internog učitati opterećenja:

- Samo jedan ga slušatelj grupe Interna availablity podržano je po servis u oblaku jer ga slušatelj je konfiguriran za raspoređivača opterećenja i postoji samo jedan Interna opterećenja. No moguće je da biste stvorili vanjski slušače multipe. 

- S internim opterećenja samo pristupiti ga slušatelj iz unutar iste virtualne mreže.
 
