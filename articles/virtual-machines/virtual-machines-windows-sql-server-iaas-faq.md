<properties
    pageTitle="SQL Server na virtualnim Azure računalima najčešća pitanja vezana uz | Microsoft Azure"
    description="U ovom se članku navedeni odgovori na najčešća pitanja o sustavom SQL Server Azure VMs."
    services="virtual-machines-windows"
    documentationCenter=""
    authors="v-shysun"
    manager="felixwu"
    editor=""
    tags="azure-service-management"/>

<tags
    ms.service="virtual-machines-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="vm-windows-sql-server"
    ms.workload="infrastructure-services"
    ms.date="09/13/2016"
    ms.author="v-shysun"/>

# <a name="sql-server-on-azure-virtual-machines-faq"></a>SQL Server na Azure virtualnim strojevima najčešća Pitanja

Ova tema sadrži odgovore na neka od najčešćih pitanja o izvodi [SQL Server na virtualnim strojevima sa sustavom Azure](https://azure.microsoft.com/services/virtual-machines/sql-server/).

[AZURE.INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

## <a name="frequently-asked-questions"></a>Najčešća pitanja

1. **Kako stvoriti Azure virtualnog računala sa sustavom SQL Server?**

    Da biste to učinili na dva načina. Najjednostavnije je rješenje da biste stvorili virtualnog računala koja obuhvaća SQL Server. Praktični vodič na registracije za Azure i stvaranju SQL VM s portala sustava, potražite u članku [Dodjela resursa za SQL Server virtualnog računala na portalu za Azure](virtual-machines-windows-portal-sql-server-provision.md). Imate mogućnost ručno instalacija sustava SQL Server na na VM i ponovno korištenje licencu za lokalni s [Mobilnost licenci putem Tehnološko jamstvo na Azure](https://azure.microsoft.com/pricing/license-mobility/).

1. **Koja je razlika između SQL VMs i servisa SQL baze podataka?**

    Pojmovno, sustavom SQL Server Azure virtualnog računala ne koji se razlikuje od izvodi SQL Server u udaljene podatkovnog centra. Nasuprot tome, [Baze podataka SQL](../sql-database/sql-database-technical-overview.md) nudi baze podataka kao-na-usluga. SQL baze podataka, nemate pristup računala koje hostira baze podataka. Potpuna usporedbe, potražite u članku [Odaberite oblak mogućnost SQL Server: baze podataka SQL Azure (PaaS) ili SQL Server na Azure VMs (IaaS)](../sql-database/sql-database-paas-vs-sql-server-iaas.md).

1. **Kako migrirati lokalne baze podataka SQL Server u oblak?**

    Najprije stvorite Azure virtualnog računala s instancu komponente SQL Server. Zatim migrirati lokalne baze podataka za tu instancu. Strategije migracije podataka, potražite u članku [Migracija baze podataka SQL Server na SQL Server u VM programa Azure](virtual-machines-windows-migrate-sql.md).

2. **Možete promijeniti instalirane značajke ili instalirati drugu instancu sustava SQL Server na istom VM?**

    Da. Medij za instalaciju sustava SQL Server nalazi u mapi na pogon **C** . Pokrenite **Setup.exe** s tog mjesta da biste dodali nove instance sustava SQL Server ili da biste promijenili druge instalirane značajke sustava SQL Server na računalu.

3. **Kako se nadograditi na novu verziju/izdanje sustava SQL Server na VM programa Azure?**

    Trenutno postoji bez nadogradnje za SQL Server u VM programa Azure. Stvorite novi Azure virtualnog računala s željenu verziju/izdanja sustava SQL Server, a zatim migriranje baze podataka u novi poslužitelj pomoću standardnih [tehnike za migraciju podataka](virtual-machines-windows-migrate-sql.md).

4. **Kako instalirati kopiju licencirani sustava SQL Server na VM programa Azure?**

    Kopiranje medij za instalaciju sustava SQL Server VM za Windows Server, a zatim instalirati SQL Server na na VM. Za licenciranje razloga, morate imati [Licencu mobilnost kroz Tehnološko jamstvo na Azure](https://azure.microsoft.com/pricing/license-mobility/).

5. **Imate li platiti SQL troškove na VM ako se koja se koristi samo čekanja/prebacivanje?**

    Ako stvarate VM SQL kroz galeriju, morate imati zasebne licencu za čekanja VM SQL i cijene je ista. Ako instalirate SQL Server ručno na virtualnog računala s [Mobilnost licence](https://azure.microsoft.com/pricing/license-mobility/), imate mogućnost da jedan besplatne pasivni instancom SQL-a za prebacivanje. Pregledajte ograničenja i preduvjete.

6. **Kako se ažuriranja i servisne pakete primjenjuju na programa SQL Server VM?**

    Virtualnim strojevima dati kontrolu nad glavno računalo, uključujući kada i kako primijeniti ažuriranja. Operacijski sustav možete ručno primijenite ažuriranja sustava windows ili možete omogućiti servisa za zakazivanje naziva [Automatski zakrpa](virtual-machines-windows-classic-sql-automated-patching.md). Automatski instalacije zakrpa promjene koje su označene važno, uključujući ažuriranja za SQL Server iz te kategorije. Ručno mora biti instaliran ostala neobavezna ažuriranja sa sustavom SQL Server.

7. **Je li moguće da biste postavili konfiguracije koji se ne prikazuju se u galeriji virtualnog računala (na primjer Windows 2008 R2 + SQL Server 2012)?**

    ne. Za virtualnog računala Galerija slike kojima je SQL Server, morate odabrati jednu od navedenih slike.

9. **Kako instalirati Alati podataka sustava SQL na moje VM Azure?**

    Preuzmite i instalirajte SQL podataka alata iz [Microsoft SQL Server Data Tools – Business Intelligence za Visual Studio 2013 ](https://www.microsoft.com/en-us/download/details.aspx?id=42313).

## <a name="resources"></a>Resursi

Da biste saznali kako sustava SQL Server na virtualnim strojevima sa sustavom Azure, pogledajte videozapis [Azure VM je najbolje platformu za SQL Server 2016](https://channel9.msdn.com/Events/DataDriven/SQLServer2016/Azure-VM-is-the-best-platform-for-SQL-Server-2016). Možete dobiti i dobro Uvod u temi [SQL Server na virtualnim računalima sustava Azure pregled](virtual-machines-windows-sql-server-iaas-overview.md).

Ostali resursi obuhvaćaju sljedeće:

- [Dodjela resursa za SQL Server virtualnog računala na portalu za Azure](virtual-machines-windows-portal-sql-server-provision.md)
- [Migracija baze podataka SQL Server na Azure VM](virtual-machines-windows-migrate-sql.md)
- [Visoke dostupnosti i oporavak Izrada za SQL Server na virtualnim računalima za Azure](virtual-machines-windows-sql-high-availability-dr.md)
- [Performanse najbolje prakse za SQL Server na virtualnim strojevima sa sustavom Azure](virtual-machines-windows-sql-performance.md)
- [Uzorci aplikacije i razvoja Strategije za SQL Server na virtualnim računalima za Azure](virtual-machines-windows-sql-server-app-patterns-dev-strategies.md)
