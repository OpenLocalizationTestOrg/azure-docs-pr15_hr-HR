<properties
    pageTitle="Sigurnosnog kopiranja i vraćanja za SQL Server | Microsoft Azure"
    description="U članku se opisuje sigurnosno kopiranje i vraćanje zahtjevi za baze podataka sustava SQL Server pokreću virtualnim računalima sustava na Azure."
    services="virtual-machines-windows"
    documentationCenter="na"
    authors="rothja"
    manager="jhubbard"
    editor=""
    tags="azure-resource-management" />

<tags
    ms.service="virtual-machines-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="vm-windows-sql-server"
    ms.workload="infrastructure-services"
    ms.date="08/19/2016"
    ms.author="jroth" />

# <a name="backup-and-restore-for-sql-server-in-azure-virtual-machines"></a>Sigurnosno kopiranje i vraćanje za SQL Server na virtualnim računalima za Azure

## <a name="overview"></a>Pregled

Sigurnosno kopiranje podataka u bazama podataka za SQL Server je važan dio strategije u zaštiti od gubitka podataka zbog pogreške aplikacija ili korisnik. To vrijedi ravnomjerno za SQL Server na virtualnim računalima sustava Azure (VMs) pokrenut.

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-both-include.md)]

Za SQL Server koji se izvodi u Azure VMs, možete koristiti izvorni sigurnosno kopiranje i vraćanje tehnike pomoću priloženu diskova za odredište sigurnosne kopije. No postoji ograničenje broj diskova možete priložiti programa Azure virtualnog računala, ovisno o [veličini virtualnog računala](virtual-machines-linux-sizes.md). Postoji indirektni disk upravljanja treba uzeti u obzir.

Počevši od 2014 SQL Server, možete sigurnosno kopiranje i vraćanje spremište blobova platforme Microsoft Azure. SQL Server 2016 sadrži i poboljšanja za tu mogućnost. Osim toga, datoteke baze podataka koji su spremljeni u spremište blobova platforme Microsoft Azure, SQL Server 2016 predstavlja mogućnost za gotovo su trenutačne sigurnosne kopije i brz vraća pomoću Azure snimke. Ovaj članak sadrži pregled tih mogućnosti, a dodatne informacije možete pronaći na [sigurnosno SQL Server kopiranje i vraćanje sa servisima za pohranu blobova platforme Microsoft Azure](https://msdn.microsoft.com/library/jj919148.aspx).

>[AZURE.NOTE] Rasprave mogućnosti za sigurnosno kopiranje vrlo velike baze podataka potražite u članku [Višestruki terabajta SQL poslužitelja baze podataka sigurnosne kopije strategije virtualnim računalima sustava za Azure](http://blogs.msdn.com/b/igorpag/archive/2015/07/28/multi-terabyte-sql-server-database-backup-strategies-for-azure-virtual-machines.aspx).

U odjeljcima obuhvatiti informacije koje se odnose na različitim verzijama sustava SQL Server podržava Azure virtualnog računala.

## <a name="sql-server-virtual-machines"></a>Virtualnim računalima sustava SQL Server

Kada instancu sustava SQL Server je pokrenut na programa Azure virtualnog računala, datoteke baze podataka već nalaze na diskova podataka u Azure. Tih diskova live u spremište blobova platforme Azure. Pa razlozima za stvaranje sigurnosne kopije baze podataka i na postupke koje potrebno promjena malo. Imajte na umu sljedeće. 

- Više ne morate izvršiti sigurnosne kopije baze podataka omogućuje zaštitu od hardver ili medijskih sadržaja nije uspjelo jer te zaštite kao dio sustava Microsoft Azure servisa Microsoft Azure.

- Svejedno morate izvršiti sigurnosne kopije baze podataka omogućuje zaštitu protiv pogreške korisnika ili za arhiviranje svrhe, propisima razloga ili administrativne svrhe.

- Datoteku sigurnosne kopije možete spremiti izravno u Azure. Dodatne informacije potražite u sljedećim se odjeljcima koji daju upute u različitim verzijama sustava SQL Server.

## <a name="sql-server-2016"></a>SQL Server 2016

Microsoft SQL Server 2016 podržava [sigurnosnog kopiranja i vraćanja s blob-ova Azure](https://msdn.microsoft.com/library/jj919148.aspx) značajke koje se nalaze u 2014 SQL Server. No sadrži i sljedeća poboljšanja:

| Poboljšanje 2016               | Pojedinosti                          |
|---------------------|-------------------------------|
| **Podjela**              | Kada se sigurnosno kopiranje blobova sustava Microsoft Azure SQL Server 2016 podržava sigurnosnom do više blob polja da biste omogućili sigurnosno kopiranje velikih baza podataka maksimalno 12.8 TB do.      |
| **Sigurnosne kopije brze snimke**                | Pomoću Azure snimke sigurnosnu kopiju sustava SQL Server datoteke snimke gotovo su trenutačne sigurnosne kopije i nudi brz vraća za pohranjene putem servisa za spremište blobova platforme Azure datoteke baze podataka. Ova mogućnost omogućuje vam da biste pojednostavnili sigurnosnog kopiranja i vraćanja pravila. Sigurnosne kopije datoteke snimke podržava i točke u vrijeme vraćanja. Dodatne informacije potražite u članku [Snimku sigurnosne kopije za datoteke baze podataka u Azure](https://msdn.microsoft.com/library/mt169363%28v=sql.130%29.aspx).   |
| **Upravljani planiranje sigurnosnih kopija**            | SQL Server upravlja sigurnosnu kopiju za Azure sada podržava prilagođeni raspored. Dodatne informacije potražite u članku [SQL Server upravlja sigurnosnu kopiju za Microsoft Azure](https://msdn.microsoft.com/library/dn449496.aspx).   |

Praktični vodič mogućnosti sustava SQL Server 2016 prilikom korištenja spremište blobova platforme Azure, potražite u članku [Praktični vodič: putem servisa za spremište blobova platforme Microsoft Azure s bazama podataka SQL Server 2016](https://msdn.microsoft.com/library/dn466438.aspx).

## <a name="sql-server-2014"></a>SQL Server 2014.

2014 SQL Server obuhvaća sljedeća poboljšanja:

1. **Sigurnosno kopiranje i vraćanje Azure**:

 - *Sigurnosno kopiranje SQL Server na URL* sada ima podršku u SQL Server Management Studio. Mogućnost sigurnosno kopirati Azure sada je dostupna prilikom korištenja zadatka sigurnosnog kopiranja i vraćanja ili čarobnjak za održavanje plan u SQL Server Management Studio. Dodatne informacije potražite u članku [Sigurnosne kopije SQL Server na URL](https://msdn.microsoft.com/library/jj919148%28v=sql.120%29.aspx).
 - *SQL Server upravlja sigurnosne kopije Azure* ima nove funkcije koja omogućuje automatsko upravljanje sigurnosne kopije. To je posebno korisno za automatske sigurnosnog kopiranja upravljanje za 2014 SQL Server instanci koje su pokrenute na računalu na Azure. Dodatne informacije potražite u članku [SQL Server upravlja sigurnosnu kopiju za Microsoft Azure](https://msdn.microsoft.com/library/dn449496%28v=sql.120%29.aspx).
 - *Automatsko sigurnosno kopiranje* pruža dodatne Automatizacija da biste automatski omogućili *SQL Server upravlja sigurnosnu kopiju za Azure* na sve postojeće i nove baze podataka za programa SQL Server VM servisu Azure. Dodatne informacije potražite u članku [Automatsko sigurnosno kopiranje za SQL Server na virtualnim strojevima sa sustavom Azure](virtual-machines-windows-sql-automated-backup.md).
 - Pregled svih mogućnosti za SQL Server 2014 kopija Azure, potražite u članku [sigurnosno SQL Server kopiranje i vraćanje sa servisima za pohranu blobova platforme Microsoft Azure](https://msdn.microsoft.com/library/jj919148%28v=sql.120%29.aspx).

1. **Šifriranje**: SQL Server 2014 podržava šifriranje podataka prilikom stvaranja sigurnosnu kopiju. Podržava nekoliko algoritama šifriranja i korištenja osf certifikat ili asimetričnim ključ. Dodatne informacije potražite u članku [Šifriranje sigurnosnu kopiju](https://msdn.microsoft.com/library/dn449489%28v=sql.120%29.aspx).

## <a name="sql-server-2012"></a>SQL Server 2012.

Detaljne informacije o SQL Server sigurnosna kopija i vraćanje u sustavu SQL Server 2012, potražite u članku [sigurnosno kopiranje i vraćanje sustava SQL Server baze podataka (SQL Server 2012)](https://msdn.microsoft.com/library/ms187048%28v=sql.110%29.aspx).

Pokretanje u SQL Server 2012 SP1 o kumulativnom ažuriranju 2, možete se i vratiti na servisu za spremište blobova platforme Azure. Ovaj dodatak može se koristiti za sigurnosno kopiranje baze podataka SQL Server na SQL Server koja se izvodi na programa Azure virtualnog računala ili instancu lokalnog. Dodatne informacije potražite u članku [SQL Server sigurnosna kopija i vraćanje sa servisom za spremište blobova platforme Azure](https://msdn.microsoft.com/library/jj919148%28v=sql.110%29.aspx).

Neke od prednosti korištenja usluga spremišta blobova platforme Azure obuhvaćaju mogućnost da biste zaobišli 16 na disku ograničenje priložene diskova lakšeg upravljanja, Izravni raspoloživost datoteku sigurnosne kopije drugu instancu programa SQL Server instanci sustavom Azure virtualnog računala ili instancu lokalnog za migraciju ili Izrada oporavak svrhe. Potpuni popis prednosti putem servisa za spremište blobova platforme Azure sigurnosnih kopija SQL Server, u odjeljku *pogodnosti* u [SQL Server sigurnosna kopija i vraćanje sa servisom za spremište blobova platforme Azure](https://msdn.microsoft.com/library/jj919148%28v=sql.110%29.aspx).

Preporučenim načinom rada preporuke i informacije o otklanjanju poteškoća potražite u članku [sigurnosno kopiranje i vraćanje najbolje prakse (servis za pohranu blobova platforme Azure)](https://msdn.microsoft.com/library/jj919149%28v=sql.110%29.aspx).

## <a name="sql-server-2008"></a>SQL Server 2008

SQL Server sigurnosna kopija i vraćanje u SQL Server 2008 R2, potražite u članku [stvaranju sigurnosnih kopija i vraćanje baze podataka SQL Server (SQL Server 2008 R2)](https://msdn.microsoft.com/library/ms187048%28v=sql.105%29.aspx).

SQL Server sigurnosna kopija i vraćanje u SQL Server 2008, potražite u članku [stvaranju sigurnosnih kopija i vraćanje baze podataka SQL Server (SQL Server 2008)](https://msdn.microsoft.com/library/ms187048%28v=sql.100%29.aspx).

## <a name="next-steps"></a>Daljnji koraci

Ako planirate implementaciju sustava sustava SQL Server u programa Azure VM, upute za dodjelu resursa možete pronaći sljedeći praktičnog vodiča: [SQL Server virtualnog računala na Azure s Azure Voditelj resursa za dodjelu resursa](virtual-machines-windows-portal-sql-server-provision.md).

Iako se sigurnosno kopiranje i vraćanje može koristiti za migraciju podataka, postoje potencijalno jednostavnije putovi migracije podataka za SQL Server na VM programa Azure. Cjelokupnu raspravu mogućnosti migracije i preporukama potražite u članku [Migracija baze podataka SQL Server na programa Azure VM](virtual-machines-windows-migrate-sql.md).

Pregledajte ostale [resurse za pokretanje sustava SQL Server na virtualnim strojevima sa sustavom Azure](virtual-machines-windows-sql-server-iaas-overview.md).
