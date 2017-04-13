<properties
    pageTitle="Povezivanje s SQL Server virtualnog računala (Voditelj resursa) | Microsoft Azure"
    description="Saznajte kako se povezati sa sustavom SQL Server koji se izvode na virtualnog računala u Azure. U ovoj se temi koristi model klasični implementacije. Scenariji razlikuju se ovisno o mrežna konfiguracija i mjesto klijent."
    services="virtual-machines-windows"
    documentationCenter="na"
    authors="rothja"
    manager="jhubbard"    
    tags="azure-resource-manager"/>
<tags
    ms.service="virtual-machines-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="vm-windows-sql-server"
    ms.workload="infrastructure-services"
    ms.date="09/21/2016"
    ms.author="jroth" />

# <a name="connect-to-a-sql-server-virtual-machine-on-azure-resource-manager"></a>Povezivanje s SQL Server virtualnog računala na Azure (Voditelj resursa)

> [AZURE.SELECTOR]
- [Voditelj resursa](virtual-machines-windows-sql-connect.md)
- [Klasični](virtual-machines-windows-classic-sql-connect.md)

## <a name="overview"></a>Pregled

U ovoj se temi opisuje kako se povezati sa instancu sustava SQL Server na sustavom Azure virtualnog računala. Obuhvaća neke [scenarije Općenito povezivanje](#connection-scenarios) i sadrži [detaljne upute za konfiguriranje veza s poslužiteljem SQL u VM programa Azure](#steps-for-manually-configuring-sql-server-connectivity-in-an-azure-vm).

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-rm-include.md)]Uvođenje klasičnog model. Da biste pogledali klasičnu verziju ovog članka, potražite u članku [povezivanje za SQL Server virtualni stroj na klasični Azure](virtual-machines-windows-classic-sql-connect.md).

Ako biste radije imali puni walk-through dodjele resursa i povezivanje, potražite u članku [Dodjeljivanje SQL Server virtualnog računala na Azure](virtual-machines-windows-portal-sql-server-provision.md).

## <a name="connection-scenarios"></a>Scenariji za povezivanje

Način na koji se klijent povezuje sa sustavom SQL Server koji se izvode na virtualnog računala razlikuje se ovisno o lokaciji klijentskog programa i konfiguracije računala/mreže. Sljedećim scenarijima obuhvaćaju sljedeće:

- [Povezivanje sa sustavom SQL Server putem Interneta](#connect-to-sql-server-over-the-internet)
- [Povezivanje sa sustavom SQL Server u istoj virtualne mreže](#connect-to-sql-server-in-the-same-virtual-network)

### <a name="connect-to-sql-server-over-the-internet"></a>Povezivanje sa sustavom SQL Server putem Interneta

Ako se želite povezati modul za baze podataka sustava SQL Server s Interneta, postoji nekoliko koraka koji su potrebni, kao što je Konfiguriranje vatrozida, omogućivanje SQL provjeru autentičnosti i konfiguriranje na mreži sigurnosne grupe mora imati pravilo mreže sigurnosne grupe omogućuje TCP promet na priključak 1433.

Ako koristite portalu Dodjela sliku virtualnog računala sustava SQL Server s Voditelj resursa, korake u nastavku su obavljen kada odaberete **javno** povezivanje mogućnost SQL:

![Javni mogućnost povezivanja SQL prilikom dodjele resursa](./media/virtual-machines-windows-sql-connect/sql-vm-portal-connectivity.png)

Ako to nije tijekom dodjele resursa, pa možete ručno konfigurirati SQL Server i virtualnim strojevima tako da slijedite [korake u ovom se članku ručno Konfiguriranje povezivanja s](#steps-for-manually-configuring-sql-server-connectivity-in-an-azure-vm).

>[AZURE.NOTE] Slika virtualnog računala za SQL Server Express edition omogućiti automatski TCP/IP protokola. Za Express edition, morate koristiti Upravitelj konfiguracije SQL poslužitelja [ručno](#configure-sql-server-to-listen-on-the-tcp-protocol) omogućavanje TCP/IP protokola nakon stvaranja na VM.

Kada to učinite, bilo kojeg klijenta s Internetom možete povezati s instancu sustava SQL Server navođenjem ili na javnu IP adresu virtualnog računala ili natpis DNS dodijeljene tom IP adresa. Ako je priključak za SQL Server 1433, ne morate odrediti u nizu za povezivanje.

    "Server=sqlvmlabel.eastus.cloudapp.azure.com;Integrated Security=false;User ID=<login_name>;Password=<your_password>"

Iako to omogućuje povezivanje za klijente putem Interneta, to znači da svi se mogu povezati sa sustavom SQL Server. Izvan klijenti morati ispravno korisničko ime i lozinku. Radi dodatne sigurnosti možete izbjeći poznati priključak 1433. Ako, na primjer, ako konfigurirati SQL Server da biste preslušali na priključak 1500 i uspostaviti proper vatrozid i mreže sigurnosne grupe pravila, povezati dodavanjem broj priključka za naziv poslužitelja kao u sljedećem primjeru:

    "Server=sqlvmlabel.eastus.cloudapp.azure.com,1500;Integrated Security=false;User ID=<login_name>;Password=<your_password>"

>[AZURE.NOTE] Nije važno Imajte na umu da kada koristite ovu tehniku možete komunicirati s sustava SQL Server, sve izlazne podatke s podatkovnim centrom za Azure podložni normalni [cijene na prijenosi za izlazne podatke](https://azure.microsoft.com/pricing/details/data-transfers/).

### <a name="connect-to-sql-server-in-the-same-virtual-network"></a>Povezivanje sa sustavom SQL Server u istoj virtualne mreže

[Virtualne mreže](../virtual-network/virtual-networks-overview.md) omogućuje dodatne scenarije. Možete se povezati VMs u istom virtualne mreže, čak i ako te VMs resursa za različite grupe. I [VPN-a web-mjesto](../vpn-gateway/vpn-gateway-site-to-site-create.md)omogućuje stvaranje arhitekturu hibridnog VMs povezan s lokalnim mreža i računalima.

Virtualne mreže omogućuje vam da biste se uključili vaš VMs Azure s domenom. To je jedini način da biste koristili provjeru autentičnosti sustava Windows na SQL Server. Drugim situacijama vezu potreban je SQL provjera autentičnosti pomoću korisničkog imena i lozinke.

Ako koristite portalu Dodjela sliku virtualnog računala sustava SQL Server s Voditelj resursa, proper Vatrozid za komunikaciju na virtualne mreže su pravila postavljanje kad odaberete **privatne** za povezivanje mogućnost SQL. Ako to nije tijekom dodjele resursa, pa možete ručno konfigurirati SQL Server i virtualnim strojevima tako da slijedite [korake u ovom se članku ručno Konfiguriranje povezivanja s](#steps-for-manually-configuring-sql-server-connectivity-in-an-azure-vm). No ako planirate za konfiguriranje okruženja domene i provjeru autentičnosti sustava Windows, morate konfigurirati SQL provjeru autentičnosti i prijave pomoću koraka u ovom članku. Ne morate konfigurirati pravila mreže sigurnosne grupe za pristup putem Interneta.

>[AZURE.NOTE] Slika virtualnog računala za SQL Server Express edition omogućiti automatski TCP/IP protokola. Za Express edition, morate koristiti Upravitelj konfiguracije SQL poslužitelja [ručno](#configure-sql-server-to-listen-on-the-tcp-protocol) omogućavanje TCP/IP protokola nakon stvaranja na VM.

Uz pretpostavku da ste konfigurirali DNS-a u virtualne mreže, možete se povezati na instancu sustava SQL Server navođenjem naziv računala SQL Server VM u nizu za povezivanje. U sljedećem primjeru i pretpostavlja da provjera autentičnosti sustava Windows i konfigurirana, a koje korisnik ima pristup instancu sustava SQL Server.

    "Server=mysqlvm;Integrated Security=true"

Imajte na umu da u tom slučaju nije određujete IP adresu na VM.

## <a name="steps-for-manually-configuring-sql-server-connectivity-in-an-azure-vm"></a>Koraci za ručno konfiguriranje veza s poslužiteljem SQL u VM programa Azure

Sljedeći koraci pokazuju kako ručno postavljanje veza s instancu sustava SQL Server i po želji povezivanje putem interneta pomoću SQL Server Management Studio (SSMS). Imajte na umu da mnoge od ovih koraka su obavljen kada odaberete odgovarajuće mogućnosti povezivanja za SQL Server na portalu.

Prije instancu sustava SQL Server možete povezati s drugom VM ili na Internetu, kao što je opisano u odjeljcima morate poduzeti sljedeće zadatke:

- [Otvorite TCP priključke u vatrozidu za Windows](#open-tcp-ports-in-the-windows-firewall-for-the-default-instance-of-the-database-engine)
- [Konfiguriranje sustava SQL Server da biste preslušali na protokolom TCP](#configure-sql-server-to-listen-on-the-tcp-protocol)
- [Konfiguriranje sustava SQL Server za kombiniranim načina provjere autentičnosti](#configure-sql-server-for-mixed-mode-authentication)
- [Stvaranje prijave za provjeru autentičnosti sustava SQL Server](#create-sql-server-authentication-logins)
- [Konfiguriranje pravila ulazne mreže sigurnosne grupe](#configure-a-network-security-group-inbound-rule-for-the-vm)
- [Konfiguriranje DNS oznaku na javnu IP adresu](#configure-a-dns-label-for-the-public-ip-address)
- [Povezivanje s modul baze podataka s drugog računala](#connect-to-the-database-engine-from-another-computer)

[AZURE.INCLUDE [Connect to SQL Server in a VM](../../includes/virtual-machines-sql-server-connection-steps.md)]

[AZURE.INCLUDE [Connect to SQL Server in a VM Resource Manager](../../includes/virtual-machines-sql-server-connection-steps-resource-manager-nsg-rule.md)]

[AZURE.INCLUDE [Connect to SQL Server in a VM Resource Manager](../../includes/virtual-machines-sql-server-connection-steps-resource-manager.md)]

## <a name="next-steps"></a>Daljnji koraci

Da biste pogledali upute uz ove korake za povezivanje za dodjelu resursa, potražite u članku [Dodjeljivanje SQL Server virtualnog računala na Azure](virtual-machines-windows-portal-sql-server-provision.md).

[Istraživanje tečaj](https://azure.microsoft.com/documentation/learning-paths/sql-azure-vm/) za SQL Server na virtualnim računalima za Azure.

Druge teme vezane uz izvodi SQL Server u Azure VMs, potražite u članku [SQL Server na virtualnim strojevima sa sustavom Azure](virtual-machines-windows-sql-server-iaas-overview.md).
