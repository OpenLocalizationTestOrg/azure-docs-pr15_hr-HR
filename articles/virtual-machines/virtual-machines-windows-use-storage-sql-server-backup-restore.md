<properties
    pageTitle="Kako koristiti Azure prostora za pohranu za SQL Server sigurnosno kopiranje i vraćanje | Microsoft Azure"
    description="Saznajte kako sigurnosnu kopiju sustava SQL Server Azure prostora za pohranu. U članku se objašnjava prednosti sigurnosno kopiranje baze podataka SQL Azure za pohranu."
    services="virtual-machines-windows"
    documentationCenter=""
    authors="MikeRayMSFT"
    manager="jhubbard"
    tags="azure-service-management"/>

<tags
    ms.service="virtual-machines-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="vm-windows-sql-server"
    ms.workload="infrastructure-services"
    ms.date="07/22/2016"
    ms.author="mikeray"/>

# <a name="use-azure-storage-for-sql-server-backup-and-restore"></a>Korištenje Azure prostora za pohranu za SQL Server sigurnosna kopija i vraćanje

## <a name="overview"></a>Pregled

Počevši od SQL Server 2012 SP1 CU2, sada možete zapisivati sigurnosne kopije sustava SQL Server izravno sa servisom spremište blobova platforme Azure. Ta je funkcija možete koristiti da biste se i vraćanje iz servisa blobova platforme Azure s baze podataka SQL Server na lokalni ili baze podataka SQL Server u Azure virtualnog računala. Sigurnosna kopija na cloud nudi prednosti dostupnost, neograničen zemlj replicirati službenoj prostora za pohranu i olakšani migracije podatke i iz oblaka. Pomoću Transact-SQL ili SMO možete izdati izjave sigurnosnog KOPIRANJA i VRAĆANJA.

SQL Server 2016 predstavlja nove mogućnosti; [Sigurnosna kopija datoteke snimke](http://msdn.microsoft.com/library/mt169363.aspx) možete koristiti za izvođenje gotovo su trenutačne sigurnosne kopije i incredibly brzi vraća.

U ovoj se temi objašnjava zašto možda odlučite koristiti Azure prostora za pohranu sigurnosnih kopija SQL, te opisuje komponente koji je uključen. Resursi na kraju članka možete koristiti da biste pristupili vodiči i dodatne informacije da biste počeli koristiti taj servis pomoću sigurnosne kopije sustava SQL Server.

## <a name="benefits-of-using-the-azure-blob-service-for-sql-server-backups"></a>Prednosti korištenja servisa blobova platforme Azure sigurnosnih kopija SQL Server

Postoji nekoliko izazove koji licem kada sigurnosno kopiranje sustava SQL Server. Ove izazove obuhvaćaju upravljanje pohranom, rizik od pogreške prostora za pohranu, pristup službenoj prostora za pohranu i hardverske konfiguracije. Mnoge od tih izazove adresirane pomoću servisa spremište blobova platforme Azure sigurnosnih kopija SQL Server. Imajte na umu sljedeće prednosti:

- **Olakšalo njihovo korištenje**: spremanje sigurnosne kopije u Azure blob-Ova je je mogućnost praktična, fleksibilne i jednostavno pristupiti službenoj mogućnost. Stvaranje službenoj prostora za pohranu za sigurnosne kopije sustava SQL Server može biti dovoljno izmjena vaše postojeće skripte/zadataka pomoću ove sintakse **Sigurnosne KOPIJE i URL** . Prostor za pohranu službenoj obično mora biti dovoljno daleko od radnog mjesta baze podataka da biste spriječili jedan Izrada koji bi mogli utjecati na oba službenoj i radnih baze podataka mjesta. Odabirom [zemlj replicirati na Azure blob-ova](../storage/storage-redundancy.md), imate dodatni slojeva zaštite u slučaju Izrada koji može utjecati na prikaz cijelog područja.
- **Sigurnosno kopiranje arhiva**: U spremište blobova platforme Azure servis nudi bolji odabir mogućnosti često korištenih vrpcu za arhiviranje sigurnosne kopije. Vrpca za pohranu možda će biti potrebno fizičke prijevoza službenoj funkcijom i mjere da biste zaštitili medijskih sadržaja. Spremanje sigurnosne kopije u spremište blobova platforme Azure nudi izravnih, Visoko dostupne i na durable arhiviranja mogućnost.
- **Upravljani hardver**: postoji bez indirektni hardver upravljanja pomoću servisa Azure. Azure services upravljanje hardver i navedite zemlj replikacije zalihosti i zaštitu od hardverske pogreške.
- **Neograničeno prostora za pohranu**: omogućivanjem Izravni sigurnosnu kopiju za Azure blob-ova kojima imate pristup gotovo neograničeno prostora za pohranu. Osim toga, sigurnosno do na disk Azure virtualnog računala ima ograničenja ovisno o veličini računala. Postoji ograničenje broj diskova možete priložiti Azure virtualnog računala sigurnosnih kopija. To ograničenje je 16 diskova za instancu vrlo velike i manje manje instanci.
- **Sigurnosno kopiranje dostupnost**: sigurnosne kopije pohranjene u Azure blob-ova su dostupne s bilo kojeg mjesta i u bilo kojem trenutku i jednostavno može se pristupiti vraća sustava SQL Server na lokalni ili drugu SQL Server izvodi u programa Azure virtualnog računala, bez potrebe za bazu podataka priložite/odvajanje ili preuzimanje i pridruživanje na VHD.
- **Trošak**: samo plaćate uslugu koji se koristi. Može biti učinkovit kao mogućnost za arhiviranje službenoj i sigurnosno kopiranje. Potražite u članku [Azure cijene Kalkulator](http://go.microsoft.com/fwlink/?LinkId=277060 "Kalkulator cijene")i [cijene Azure članak](http://go.microsoft.com/fwlink/?LinkId=277059 "cijene članka") dodatne informacije.
- **Prostor za pohranu snimke**: kada datoteke baze podataka spremaju se u blobova platforme Azure i koristite SQL Server 2016, možete koristiti [sigurnosnu kopiju datoteke snimke](http://msdn.microsoft.com/library/mt169363.aspx) izvođenje gotovo su trenutačne sigurnosne kopije i incredibly brzi vraća.

Dodatne informacije potražite u članku [SQL Server sigurnosna kopija i vraćanje sa servisom za spremište blobova platforme Azure](http://go.microsoft.com/fwlink/?LinkId=271617).

Sljedeća dva odjeljka predstavljanje prostora za pohranu servisa blobova platforme Azure, uključujući obavezne komponente SQL Server. Važno je da razumijete komponente i njihovih interakcije uspješno korištenje sigurnosno kopiranje i vraćanje iz usluga spremišta blobova platforme Azure.

## <a name="azure-blob-storage-service-components"></a>Komponente za servis spremište blobova platforme Azure

Sljedeće komponente Azure koriste se kada sigurnosno na servis za pohranu blobova platforme Azure.

| Komponenta               | Opis                          |
|---------------------|-------------------------------|
| **Račun za pohranu** | Račun za pohranu je početnu točku za sve servise za pohranu. Da biste pristupili servis za spremište blobova platforme Azure, najprije stvorite račun za Azure prostora za pohranu. Dodatne informacije o servisu spremište blobova platforme Azure potražite u članku [kako koristiti servis za spremište blobova platforme Azure](https://azure.microsoft.com/develop/net/how-to-guides/blob-storage/) |
| **Spremnik** | Spremnik nudi grupiranja skupa blob-ova i mogu pohraniti neograničen broj blob-ova. Da biste napisali SQL Server sigurnosne kopije na servis blobova platforme Azure morate imati barem spremnik stvorili. |
| **Blob** | Datoteka bilo koje vrste i veličine. Blob-Ova se adresirati pomoću sljedećih oblika URL: **https://[storage account].blob.core.windows.net/[container]/[blob]**. Dodatne informacije o blob-Ova stranica potražite u članku [objašnjenje blok i blob-Ova stranica](http://msdn.microsoft.com/library/azure/ee691964.aspx) |

## <a name="sql-server-components"></a>Komponente SQL Server

Sljedeće komponente SQL Server koriste se kada se na servis za spremište blobova platforme Azure sigurnosno kopiranje.

| Komponenta               | Opis                          |
|---------------------|-------------------------------|
| **URL-A** | URL-a određuje na Uniform Resource Identifier (URI) da biste jedinstveni datoteku sigurnosne kopije. URL koristi radi mjesto i naziv datoteke sigurnosne kopije sustava SQL Server. Stvarni blob, a ne samo spremniku moraju pokazivati URL-a. Ako ne postoji blob-om, stvaranja. Ako postojeće blob nije naveden, sigurnosno kopiranje ne uspije, osim ako se > navedena je mogućnost FORMATIRANJA s. Slijedi primjer URL-a želite navesti naredba sigurnosne KOPIJE: **http[s]://[storageaccount].blob.core.windows.net/[container]/[FILENAME.bak]**. HTTPS preporučuje se, ali nije obavezno. |
| **Vjerodajnica** | Informacije koje su potrebne da bi povezao i provjeru autentičnosti usluga spremišta blobova platforme Azure pohranjen kao vjerodajnice.  Redoslijedom za SQL Server za pisanje blobova platforme Azure ili vraćanje sigurnosne kopije iz nje, potrebno je stvoriti vjerodajnica za SQL Server. Dodatne informacije potražite u članku [Vjerodajnica za SQL Server](https://msdn.microsoft.com/library/ms189522.aspx). |

> [AZURE.NOTE] Ako odaberete da biste kopirali i prijenos sigurnosnu kopiju datoteke na servis za spremište blobova platforme Azure, morate koristiti vrstu blob stranice kao mogućnost za pohranu namjeravate koristiti ovu datoteku za vraćanje operacije. VRAĆANJE iz blob vrste bloka neće uspjeti poruku o pogrešci.

## <a name="next-steps"></a>Daljnji koraci

1. Stvorite račun za Azure ako ga već nemate. Ako su procjene Azure, preporučuje se [besplatnu probnu verziju](https://azure.microsoft.com/free/).

1. Zatim prijeđite preko jedne od sljedeći vodiči za koje će voditi kroz stvaranje računa za pohranu i izvođenja vraćanja.

    - **SQL Server 2014.**: [Praktični vodič: sigurnosno SQL Server 2014 kopiranje i vraćanje na Microsoft Azure bloba servis za pohranu](https://msdn.microsoft.com/library/jj720558\(v=sql.120\).aspx).
    - **SQL Server 2016**: [Praktični vodič: putem servisa za spremište blobova platforme Microsoft Azure s bazama podataka SQL Server 2016](https://msdn.microsoft.com/library/dn466438.aspx)

1. Pregledajte dodatna dokumentacija počevši od [sigurnosnog SQL Server kopiranja i vraćanja sa servisima za pohranu blobova platforme Microsoft Azure](https://msdn.microsoft.com/library/jj919148.aspx).

Ako naiđete na probleme, pročitajte članak tema [Sigurnosnu kopiju sustava SQL Server URL Praktični savjeti i otklanjanje poteškoća](https://msdn.microsoft.com/library/jj919149.aspx).

Drugi poslužitelj za SQL sigurnosno kopiranje i vraćanje mogućnosti potražite u odjeljku [sigurnosno kopiranje i vraćanje za SQL Server na virtualnim strojevima sa sustavom Azure](../virtual-machines/virtual-machines-windows-sql-backup-recovery.md).
