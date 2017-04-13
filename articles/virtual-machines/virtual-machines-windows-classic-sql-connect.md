<properties
    pageTitle="Povezivanje s SQL Server virtualnog računala (klasični) | Microsoft Azure"
    description="Saznajte kako se povezati sa sustavom SQL Server koji se izvode na virtualnog računala u Azure. U ovoj se temi koristi model klasični implementacije. Scenariji razlikuju se ovisno o mrežna konfiguracija i mjesto klijent."
    services="virtual-machines-windows"
    documentationCenter="na"
    authors="rothja"
    manager="jhubbard"
    tags="azure-service-management"/>
<tags
    ms.service="virtual-machines-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="vm-windows-sql-server"
    ms.workload="infrastructure-services"
    ms.date="09/22/2016"
    ms.author="jroth" />

# <a name="connect-to-a-sql-server-virtual-machine-on-azure-classic-deployment"></a>Povezivanje s SQL Server virtualnog računala na Azure (klasični implementacija)

> [AZURE.SELECTOR]
- [Voditelj resursa](virtual-machines-windows-sql-connect.md)
- [Klasični](virtual-machines-windows-classic-sql-connect.md)

## <a name="overview"></a>Pregled

U ovoj se temi opisuje kako se povezati sa instancu sustava SQL Server na sustavom Azure virtualnog računala. Obuhvaća neke [scenarije Općenito povezivanje](#connection-scenarios) i sadrži [detaljne upute za konfiguriranje veza s poslužiteljem SQL u VM programa Azure](#steps-for-configuring-sql-server-connectivity-in-an-azure-vm).

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]Ako koristite VMs resursima potražite u članku [povezivanje za SQL Server virtualni stroj na Azure pomoću upravitelja resursa](virtual-machines-windows-sql-connect.md).

## <a name="connection-scenarios"></a>Scenariji za povezivanje

Način na koji se klijent povezuje sa sustavom SQL Server koji se izvode na virtualnog računala razlikuje se ovisno o lokaciji klijentskog programa i konfiguracije računala/mreže. Sljedećim scenarijima obuhvaćaju sljedeće:

- [Povezivanje sa sustavom SQL Server u istoj servis u oblaku](#connect-to-sql-server-in-the-same-cloud-service)
- [Povezivanje sa sustavom SQL Server putem Interneta](#connect-to-sql-server-over-the-internet)
- [Povezivanje sa sustavom SQL Server u istoj virtualne mreže](#connect-to-sql-server-in-the-same-virtual-network)

>[AZURE.NOTE] Prije povezivanja s bilo kojom od ovih metoda, morate slijediti [korake u ovom se članku Konfiguriranje povezivanja s](#steps-for-configuring-sql-server-connectivity-in-an-azure-vm).

### <a name="connect-to-sql-server-in-the-same-cloud-service"></a>Povezivanje sa sustavom SQL Server u istoj servis u oblaku

Više virtualnim strojevima mogu stvoriti u istom servis u oblaku. Da biste shvatili scenarij virtualnim strojevima, potražite [u](virtual-machines-windows-classic-connect-vms.md#connect-vms-in-a-standalone-cloud-service)članku povezivanje virtualnim strojevima sa servisom virtualne mreže ili oblačić. Taj se scenarij kada pokuša klijentskog računala na jedan virtualnog računala da biste se povezali sa sustavom SQL Server radi na nekom drugom računalu virtualne u istom servis u oblaku.

U ovom scenariju, možete se povezati VM **naziv** (i prikazana kao **Naziv računala** ili **naziv glavnog računala** na portalu). To je naziv naveden u VM tijekom stvaranja. Ako, na primjer, ako pod nazivom SQL VM **mysqlvm**klijenta VM isti servis u oblaku nije moguće sljedeće niz za povezivanje povezati pomoću:

    "Server=mysqlvm;Integrated Security=false;User ID=<login_name>;Password=<your_password>"

### <a name="connect-to-sql-server-over-the-internet"></a>Povezivanje sa sustavom SQL Server putem Interneta

Ako se želite povezati modul za baze podataka sustava SQL Server s Interneta, morate stvoriti krajnje virtualnog računala za dolazne TCP komunikaciju. Ovaj korak Azure konfiguracije usmjerava TCP priključak promet TCP priključka koji je dostupan virtualnog računala.

Da biste povezali putem Interneta, morate koristiti u VM DNS naziva i broj priključka krajnjoj točki VM (konfigurirano u nastavku ovog članka). Da biste pronašli naziv DNS-a, otvorite centar za Azure pa odaberite **virtualnim strojevima (klasični)**. Zatim odaberite virtualnog računala. U odjeljku **Pregled** prikazuje se **naziv DNS-a** .

Na primjer, razmotrite klasični virtualnog računala pod nazivom **mysqlvm** s DNS naziva **mysqlvm7777.cloudapp.net** i VM krajnjoj točci **57500**. Pod pretpostavkom da pravilno konfigurirani povezivanje, sljedeće niz za povezivanje ne može se koristiti za pristup virtualnog računala s bilo kojeg mjesta na Internetu:

    "Server=mycloudservice.cloudapp.net,57500;Integrated Security=false;User ID=<login_name>;Password=<your_password>"

Iako to omogućuje povezivanje za klijente putem Interneta, to znači da svi se mogu povezati sa sustavom SQL Server. Izvan klijenti morati ispravno korisničko ime i lozinku. Radi dodatne sigurnosti, nemojte koristiti poznati priključak 1433 za krajnju točku javno virtualnog računala. A ako je to moguće, razmislite o dodavanju ACL na na krajnjoj točki da biste ograničili promet samo u klijentskim programima koji imaju odgovarajuće dozvole. Upute o korištenju ACL-a s krajnje točke potražite u članku [Upravljanje ACL krajnje točke](virtual-machines-windows-classic-setup-endpoints.md#manage-the-acl-on-an-endpoint).

>[AZURE.NOTE] Nije važno Imajte na umu da kada koristite ovu tehniku možete komunicirati s sustava SQL Server, sve izlazne podatke s podatkovnim centrom za Azure podložni normalni [cijene na prijenosi za izlazne podatke](https://azure.microsoft.com/pricing/details/data-transfers/).

### <a name="connect-to-sql-server-in-the-same-virtual-network"></a>Povezivanje sa sustavom SQL Server u istoj virtualne mreže

[Virtualne mreže](../virtual-network/virtual-networks-overview.md) omogućuje dodatne scenarije. Možete se povezati VMs u istom virtualne mreže, čak i ako te VMs servise u oblaku za različite. I [VPN-a web-mjesto](../vpn-gateway/vpn-gateway-site-to-site-create.md)omogućuje stvaranje arhitekturu hibridnog VMs povezan s lokalnim mreža i računalima.

Virtualne mreže omogućuje vam da biste se uključili vaš VMs Azure s domenom. To je jedini način da biste koristili provjeru autentičnosti sustava Windows na SQL Server. Drugim situacijama vezu potreban je SQL provjera autentičnosti pomoću korisničkog imena i lozinke.

Ako namjeravate konfiguriranje okruženja domene i provjeru autentičnosti sustava Windows, ne morate slijedite korake u ovom članku da biste konfigurirali javno krajnja točka ili SQL provjeru autentičnosti i prijave. U ovom scenariju možete povezati instancu sustava SQL Server tako da navedete ime SQL Server VM u nizu za povezivanje. U sljedećem primjeru pretpostavlja da provjera autentičnosti sustava Windows i konfigurirana, a koje korisnik ima pristup instancu sustava SQL Server.

    "Server=mysqlvm;Integrated Security=true"

## <a name="steps-for-configuring-sql-server-connectivity-in-an-azure-vm"></a>Upute za konfiguriranje veza s poslužiteljem SQL u VM programa Azure

Sljedeći koraci pokazuju kako se povezati sa instancu sustava SQL Server putem interneta pomoću SQL Server Management Studio (SSMS). Međutim, isti se koraci primjenjuju izradi virtualnog računala sustava SQL Server dostupne za aplikacije sustava pokrenut i lokalne i Azure.

Prije instancu sustava SQL Server možete povezati s drugom VM ili na Internetu, kao što je opisano u odjeljcima morate poduzeti sljedeće zadatke:

- [Stvaranje krajnje točke TCP za virtualnog računala](#create-a-tcp-endpoint-for-the-virtual-machine)
- [Otvorite TCP priključke u vatrozidu za Windows](#open-tcp-ports-in-the-windows-firewall-for-the-default-instance-of-the-database-engine)
- [Konfiguriranje sustava SQL Server da biste preslušali na protokolom TCP](#configure-sql-server-to-listen-on-the-tcp-protocol)
- [Konfiguriranje sustava SQL Server za kombiniranim načina provjere autentičnosti](#configure-sql-server-for-mixed-mode-authentication)
- [Stvaranje prijave za provjeru autentičnosti sustava SQL Server](#create-sql-server-authentication-logins)
- [Određivanje DNS naziva virtualnog računala](#determine-the-dns-name-of-the-virtual-machine)
- [Povezivanje s modul baze podataka s drugog računala](#connect-to-the-database-engine-from-another-computer)

Put veze zbraja se tako da na sljedećem su dijagramu:

![Povezivanje s virtualnog računala za SQL Server](../../includes/media/virtual-machines-sql-server-connection-steps/SQLServerinVMConnectionMap.png)

[AZURE.INCLUDE [Connect to SQL Server in a VM Classic TCP Endpoint](../../includes/virtual-machines-sql-server-connection-steps-classic-tcp-endpoint.md)]

[AZURE.INCLUDE [Connect to SQL Server in a VM](../../includes/virtual-machines-sql-server-connection-steps.md)]

[AZURE.INCLUDE [Connect to SQL Server in a VM Classic Steps](../../includes/virtual-machines-sql-server-connection-steps-classic.md)]

## <a name="next-steps"></a>Daljnji koraci

Ako su planiranje i za korištenje grupe dostupnosti AlwaysOn visoke dostupnosti i oporavak Izrada, razmislite o implementacije u ga slušatelj. Baza podataka se klijenti povezuju da biste ga slušatelj umjesto izravno na neki od instance sustava SQL Server. Ga slušatelj preusmjerava klijente primarni replike u grupi dostupnost. Dodatne informacije potražite u članku [Konfiguriranje programa ILB ga slušatelj za grupe dostupnosti AlwaysOn u Azure](virtual-machines-windows-classic-ps-sql-int-listener.md).

Važno je da pregled svih sigurnosnih najbolje prakse za SQL Server na Azure virtualnog računala. Dodatne informacije potražite u članku [Sigurnosna pitanja vezana uz za SQL Server na virtualnim strojevima sa sustavom Azure](virtual-machines-windows-sql-security.md).

[Istraživanje tečaj](https://azure.microsoft.com/documentation/learning-paths/sql-azure-vm/) za SQL Server na virtualnim računalima za Azure. 

Druge teme vezane uz izvodi SQL Server u Azure VMs, potražite u članku [SQL Server na virtualnim strojevima sa sustavom Azure](virtual-machines-windows-sql-server-iaas-overview.md).
