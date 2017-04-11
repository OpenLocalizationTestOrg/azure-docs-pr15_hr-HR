<properties
    pageTitle="DocumentDB globalni replikacije baze podataka | Microsoft Azure"
    description="Informirajte se o upravljanju globalni replikacijom vašeg računa DocumentDB putem portala za Azure."
    services="documentdb"
    keywords="Globalni baze podataka, replikacijom"
    documentationCenter=""
    authors="mimig1"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="documentdb"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/17/2016"
    ms.author="mimig"/>

# <a name="how-to-perform-documentdb-global-database-replication-using-the-azure-portal"></a>Kako izvesti DocumentDB globalni baze podataka replikacijom pomoću portala za Azure

Saznajte kako koristiti Azure portal za replikaciju podataka u više područja za globalnu dostupnost podataka u Azure DocumentDB.

Informacije o kako globalni baze podataka replikacije funkcionira u DocumentDB potražite u odjeljku [Podaci Raspodijeli globalno DocumentDB](documentdb-distribute-data-globally.md). Informacije o izvođenju replikacije baze podataka za globalnu programski potražite u članku [razvojnom programiranju s računima DocumentDB više područja](documentdb-developing-with-multiple-regions.md).

> [AZURE.NOTE] Globalni distribucija baze podataka DocumentDB Općenito dostupan je i automatski omogućena za sve novostvorenu DocumentDB račune. Radimo da biste omogućili globalni raspodjele na sve postojeće račune, ali u privremeni, ako želite da se globalni raspodjele omogućena na svoj račun, [obratite se podršci](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) i smo ćete omogućite je za vas sada.

## <a id="addregion"></a>Dodavanje područja globalni baze podataka

DocumentDB je dostupan u većini [Azure područja] [azureregions]. Nakon odabira Zadana razina dosljednost za vaš račun baze podataka, možete povezati jedan ili više područja (ovisno o vašem odabiru zadani dosljednost razine i globalnim raspodjele potrebama).

1. [Azure portal](https://portal.azure.com/)u Jumpbar, kliknite **DocumentDB računa**.
2. U plohu **DocumentDB računa** odaberite račun baze podataka da biste izmijenili.
3. U plohu računa kliknite **globalno replicirati podataka** na izborniku.
4. U plohu **globalno replicirati podataka** odaberite područja za dodavanje ili uklanjanje, a zatim **Spremi**. Postoji troškova s dodavanjem područja, pročitajte članak [cijene stranice](https://azure.microsoft.com/pricing/details/documentdb/) ili [podatke Raspodijeli globalno DocumentDB](documentdb-distribute-data-globally.md) članka dodatne informacije.

    ![Kliknite regija na karti da biste dodali ili ih ukloniti][1]

### <a name="selecting-global-database-regions"></a>Odabir područja globalni baze podataka

Prilikom konfiguriranja dva ili više područja, preporučuje se da su regije odabrana na temelju parove regija opisane u na [tvrtke continuity i Izrada oporavak (BCDR): Uparena područja Azure]  [ bcdr] članka.

Konkretno, prilikom konfiguriranja više područja, provjerite da biste odabrali isti broj područja (+/-1 za čak i odd /) s svim stupcima upareni regija. Ako, na primjer, ako želite implementirati četiri područja SAD-a, odaberite dviju regija SAD-a iz lijevog stupca i dva s desne strane. Tako, sljedeće bi odgovarajući skup: Zapad SAD-a, Istočni SAD-a, Sjeverna središnje NAM i Jug središnje NAM.

Ovaj vodič je važno da biste pratili kada samo dva područja konfigurirane Izrada oporavak scenarije. Za više od dviju regija u ovom uputama dobro je, ali ne ključnih pod uvjetom da neke od odabranog područja u skladu ovaj uparivanje.

<!---
## <a id="selectwriteregion"></a>Select the write region

While all regions associated with your DocumentDB database account can serve reads (both, single item as well as multi-item paginated reads) and queries, only one region can actively receive the write (insert, upsert, replace, delete) requests. To set the active write region, do the following  


1. In the **DocumentDB Account** blade, select the database account to modify.
2. In the account blade, if the **All Settings** blade is not already opened, click **All Settings**.
3. In the **All Settings** blade, click **Write Region Priority**.
    ![Change the write region under DocumentDB Account > Settings > Add/Remove Regions][2]
4. Click and drag regions to order the list of regions. The first region in the list of regions is the active write region.
    ![Change the write region by reordering the region list under DocumentDB Account > Settings > Change Write Regions][3]
-->

## <a id="next"></a>Daljnji koraci

Informirajte se o upravljanju dosljednost globalno repliciranu račun tako da pročitate [dosljednost razina u DocumentDB](documentdb-consistency-levels.md).

Informacije o kako globalni baze podataka replikacije funkcionira u DocumentDB potražite u odjeljku [Podaci Raspodijeli globalno DocumentDB](documentdb-distribute-data-globally.md). Informacije o programski replikaciju podataka u više područja, potražite u članku [razvojnom programiranju s računima DocumentDB više područja](documentdb-developing-with-multiple-regions.md).

<!--Image references-->
[1]: ./media/documentdb-portal-global-replication/documentdb-add-region.png
[2]: ./media/documentdb-portal-global-replication/documentdb_change_write_region-1.png
[3]: ./media/documentdb-portal-global-replication/documentdb_change_write_region-2.png

<!--Reference style links - using these makes the source content way more readable than using inline links-->
[bcdr]: https://azure.microsoft.com/documentation/articles/best-practices-availability-paired-regions/
[consistency]: https://azure.microsoft.com/documentation/articles/documentdb-consistency-levels/
[azureregions]: https://azure.microsoft.com/en-us/regions/#services
[offers]: https://azure.microsoft.com/en-us/pricing/details/documentdb/
