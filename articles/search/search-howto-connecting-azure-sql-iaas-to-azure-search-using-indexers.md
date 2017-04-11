<properties 
    pageTitle="Konfiguriranje veza indeksiranje pretraživanja Azure SQL Server na Azure virtualnog računala | Microsoft Azure | Indexers" 
    description="Omogućivanje šifrirane veze i konfiguriranje vatrozida da dopušta veze sa sustavom SQL Server na Azure virtualnog računala (VM) iz indeksiranje na Azure pretraživanja." 
    services="search" 
    documentationCenter="" 
    authors="jack4it" 
    manager="pablocas" 
    editor=""/>

<tags 
    ms.service="search" 
    ms.devlang="rest-api" 
    ms.workload="search" 
    ms.topic="article" 
    ms.tgt_pltfrm="na" 
    ms.date="09/26/2016" 
    ms.author="jackma"/>

# <a name="configure-a-connection-from-an-azure-search-indexer-to-sql-server-on-an-azure-vm"></a>Konfiguriranje veza indeksiranje pretraživanja Azure SQL Server na VM programa Azure

Kao što je naznačeno u [Povezivanje Azure baze podataka SQL Azure pretraživanje pomoću indexers](search-howto-connecting-azure-sql-database-to-azure-search-using-indexers-2015-02-28.md#frequently-asked-questions), stvaranje indexers protiv **SQL Server na Azure VMs** (ili **SQL Azure VMs** za kratki) podržava Azure pretraživanja, ali postoji nekoliko vezane uz sigurnost preduvjeta za voditi brigu o prvi. 

**Zadatka trajanje:** Otprilike 30 minuta, pod pretpostavkom da ste već instaliran certifikat na na VM.

## <a name="enable-encrypted-connections"></a>Omogućivanje šifrirane veze

Azure pretraživanja zahtijeva šifriranu kanal za sve zahtjeve za indeksiranje putem javne internetske veze. U ovom se odjeljku navedeni koraci da bi to funkcioniralo.

1. Provjerite svojstva potvrde da biste provjerili predmetni naziv na potpuno kvalificirani naziv domene (FQDN) Azure VM. Možete koristiti alat kao što su CertUtils ili dodatak za potvrde da biste pogledali svojstva. Možete dobiti FQDN iz odjeljka Essentials VM plohu servisa, u polju **javnu IP adresa/DNS naziv oznake** [Azure portal](https://portal.azure.com/).

    - Za VMs stvorena pomoću predloška za novije **Voditelj resursa** , FQDN oblikovan kao `<your-VM-name>.<region>.cloudapp.azure.com`. 

    - Za starije VMs stvoreni kao na **Klasični** VM, FQDN oblikovan kao `<your-cloud-service-name.cloudapp.net>`. 

2. Konfiguriranje sustava SQL Server za korištenje potvrde korištenja uređivača registra (regedit). 

    Iako Upravitelj konfiguracije SQL poslužitelja često se koristi za taj zadatak, nećete moći koristiti za taj scenarij. Ga nećete pronaći uvezene certifikat jer FQDN VM na Azure ne odgovara FQDN kao određen VM (to označava domenu kao na lokalnom računalu ili mrežnom domenom je pridruženo). Kada se nazivi ne odgovaraju, koristite regedit za određivanje certifikata.

    - U regedit, dođite do sljedećeg ključa registra: `HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Microsoft SQL Server\[MSSQL13.MSSQLSERVER]\MSSQLServer\SuperSocketNetLib\Certificate`.
     
    Na `[MSSQL13.MSSQLSERVER]` dio razlikuje se ovisno o verzije i instancu naziv. 

    - Postavite vrijednost ključa **certifikata** na **otisak** prsta SSL certifikat koju ste uvezli na VM.

    Da biste dobili otisak prsta, neki bolje nego drugim osobama na nekoliko načina. Ako kopirate iz **potvrde** dodatak u BLOG, vjerojatno će obraditi programa nevidljivi početnih znakova [kao što je opisano u ovom članku za podršku](https://support.microsoft.com/kb/2023869/), zbog čega se pogreška kada pokušate veze. Postoji nekoliko zaobilazna rješenja za ispravljanje tog problema. Najlakši je backspace iznad i zatim ponovno upišite prvi znak otisak prsta da biste uklonili vodeći znak u polju vrijednost ključa regedit. Osim toga, možete koristiti alat za različite da biste kopirali u otisak prsta.

3. Dodjela dozvola za račun servisa. 

    Provjerite je li račun servisa SQL Server moguć je odgovarajuće dozvole na privatni ključ SSL certifikata. Ako overlook ovaj korak, SQL Server neće se pokrenuti. Za ovaj zadatak možete koristiti **Certifikati** dodatak ili **CertUtils** .

4. Ponovno pokrenite servis sustava SQL Server.

## <a name="configure-sql-server-connectivity-in-the-vm"></a>Konfiguriranje veza s poslužiteljem SQL u na VM

Nakon postavljanja šifriranu vezu potrebnih Azure pretraživanje postoje dodatni koraci konfiguriranja intrinzična sa sustavom SQL Server na Azure VMs. Ako to još niste učinili, sljedeći je korak da biste dovršili konfiguraciju pomoću neki od ovih članaka:

- **Voditelj resursa** VM, potražite u članku [povezivanje za SQL Server virtualni stroj na Azure pomoću upravitelja resursa](../virtual-machines/virtual-machines-windows-sql-connect.md). 

- U **klasičnom** VM potražite u članku [povezivanje za SQL Server virtualni stroj na klasični Azure](../virtual-machines/virtual-machines-windows-classic-sql-connect.md).

Točnije, pročitajte odjeljak svakog članka za "povezivanje putem Interneta".

## <a name="configure-the-network-security-group-nsg"></a>Konfiguriranje mreže sigurnosne grupe (NSG)

Nije neobično konfiguriranje NSG i odgovarajuće Azure krajnja točka ili pristup kontrola popisa (ACL) da bi vaše VM Azure pristupačno drugim tvrtkama. Vjerojatno ste to prije gotovo da biste omogućili vlastite logike aplikacije za povezivanje s vašeg VM SQL Azure. Je ne razlikuje za veze na SQL Azure VM sustava Azure pretraživanja. 

Veze u nastavku sadrže upute o konfiguraciji NSG za VM implementacije. Slijedite ove upute za ACL krajnje Azure pretraživanje na temelju IP adrese.

> [AZURE.NOTE] Pozadina, potražite u članku [što je sigurnosne grupe mreže?](../virtual-network/virtual-networks-nsg.md)

- **Voditelj resursa** VM, potražite u članku [Stvaranje NSGs za ARM implementacije](../virtual-network/virtual-networks-create-nsg-arm-pportal.md). 

- U **klasičnom** VM potražite u članku [kako stvoriti NSGs za klasični implementacije](../virtual-network/virtual-networks-create-nsg-classic-ps.md).

IP adresiranja mogu predstavljati nekoliko izazove koji se jednostavno bi se prevladala ako ste na umu problema i potencijalne zaobilazna rješenja. Preostali odjeljcima preporuke za rukovanje problema vezanih uz IP adresa u ACL-a.

#### <a name="restrict-access-to-the-search-service-ip-address"></a>Ograničiti pristup IP adresa servisa za pretraživanje

Preporučujemo da ograničiti pristup IP adrese servisa za pretraživanje u ACL ne morate učiniti sustava SQL Azure VMs široko Otvori zahtjeva za sve veze. Možete jednostavno saznati IP adresu po pinging FQDN (na primjer, `<your-search-service-name>.search.windows.net`) servisa za pretraživanje.

#### <a name="managing-ip-address-fluctuations"></a>Upravljanje prilagođavati razlikama IP adresa

Ako je servis za pretraživanje samo jedna jedinica pretraživanje (to jest, jedan replike i particija), IP adresa promijenit će prilikom ponovnog pokretanja servisa redovno poništavanja ACL postojeće s IP adresom usluga pretraživanja.

Da biste izbjegli pogreške koja slijedi povezivanje je koristiti više od jedne replike i particija u pretraživanju Azure. Time se povećava trošak, ali i rješava problem IP adresa. U pretraživanju Azure IP adrese ne promijenite kad imate više od jedne jedinice za pretraživanje.

Drugi način je omogućiti vezu neće uspjeti i konfigurirajte ACL-a na NSG. U možete očekivati IP adrese da biste promijenili svakih nekoliko tjedana. Za korisnike koji ne nadziranim indeksiranja na osnovi rijetko, taj se način mogu izgledna.

Treći izgledna (ali nije osobito sigurno) pristup je da biste odredili rasponu IP adresa Azure regije u kojima je dodijeljena servisa za pretraživanje. Popis IP rasponi javnu IP adrese iz kojeg se dodijeliti Azure resursima je objavljeno na [Azure podatkovnog centra IP raspone](https://www.microsoft.com/download/details.aspx?id=41653). 

#### <a name="include-the-azure-search-portal-ip-addresses"></a>Uključite Azure pretraživanje portala IP adresa

Ako koristite Azure portal da biste stvorili indeksiranje pretraživanja Azure portala logike pristup vašem VM SQL Azure mora i vrijeme stvaranja Azure pretraživanje portala IP adresa možete pronaći tako da pinging `stamp2.search.ext.azure.com`.

## <a name="next-steps"></a>Daljnji koraci

S konfiguracijom van sada možete navesti SQL Server na Azure VM kao izvor podataka za indeksiranje pretraživanja Azure. Dodatne informacije potražite u članku [Povezivanje Azure baze podataka SQL Azure pretraživanje pomoću indexers](search-howto-connecting-azure-sql-database-to-azure-search-using-indexers-2015-02-28.md) .
