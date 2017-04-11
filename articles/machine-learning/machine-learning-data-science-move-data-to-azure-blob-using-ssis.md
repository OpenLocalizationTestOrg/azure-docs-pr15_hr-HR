<properties
    pageTitle="Premještanje podataka iz blobova platforme Azure pomoću poveznika SSIS | Microsoft Azure"
    description="Premještanje podataka u ili iz blobova platforme Azure pomoću poveznika SSIS."
    services="machine-learning,storage"
    documentationCenter=""
    authors="bradsev"
    manager="jhubbard"
    editor="cgronlun" />

<tags
    ms.service="machine-learning"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/14/2016"
    ms.author="bradsev" />

# <a name="move-data-to-or-from-azure-blob-storage-using-ssis-connectors"></a>Premještanje podataka iz blobova platforme Azure pomoću poveznika SSIS

[Paket značajki usluge web-aplikacije SQL Server Integracija za Azure](https://msdn.microsoft.com/library/mt146770.aspx) nudi komponente za povezivanje s Azure, prijenos podataka između Azure i lokalnih izvora podataka, a proces podataka pohranjenih u Azure.

Smjernice o tehnologije koje služe za premještanje podataka da biste i/ili iz spremišta blobova platforme Azure povezani ovdje:

[AZURE.INCLUDE [blob-storage-tool-selector](../../includes/machine-learning-blob-storage-tool-selector.md)]


Kada korisnici su premješteni lokalnih podataka u oblak, oni mogli pristupati s bilo kojeg Azure servisa odražava punu snagu programa paketa Azure tehnologije. Mogu se koristiti, na primjer, u Azure strojnog učenja ili na programa HDInsight klaster.

To je obično se u prvom koraku za vodiči [SQL](machine-learning-data-science-process-sql-walkthrough.md) i [HDInsight](machine-learning-data-science-process-hive-walkthrough.md) .

Rasprave Kanonski scenarija koji će se koristiti SSIS da biste izvršili poslovne potrebe zajednička scenarija za integraciju hibridnog podataka potražite u članku blog [na taj način više s paket značajki usluge web-aplikacije SQL Server Integracija za Azure](http://blogs.msdn.com/b/ssis/archive/2015/06/25/doing-more-with-sql-server-integration-services-feature-pack-for-azure.aspx) .

> [AZURE.NOTE] Uvod u spremište blobova platforme Azure priručniku osnove [blobova platforme Azure](../storage/storage-dotnet-how-to-use-blobs.md) i usluzi [blobova platforme Azure](https://msdn.microsoft.com/library/azure/dd179376.aspx).

## <a name="prerequisites"></a>Preduvjeti

Izvođenje zadataka opisane u ovom članku, morate imati web-mjesto Azure pretplata i u okvir za postavljanje računa za Azure prostor za pohranu. Morate znati Azure prostora za pohranu imenom i računom ključ računa prijenos ili preuzimanje podataka.

- Da biste postavili **Azure pretplatu**, pročitajte članak [besplatnu probnu verziju jedan mjesec](https://azure.microsoft.com/pricing/free-trial/).

- Upute za stvaranje **računa za pohranu** i dohvaćanje račun i podaci o ključu, pročitajte članak [o Azure prostora za pohranu računi](../storage/storage-create-storage-account.md).


Da biste koristili **SSIS poveznika**, morate preuzeti:

- **2014 SQL Server ili 2016 Standard (ili noviji)**: instalacija sadrži SQL Integracijskim uslugama poslužitelja.
- **2014 sustava Microsoft SQL Server ili paketa sa značajkom 2016 integraciju servisa za Azure**: to se može preuzeti, odnosno sa [SQL Integracijskim uslugama poslužitelja 2014](http://www.microsoft.com/download/details.aspx?id=47366) i [Servisima za integraciju sustava SQL Server 2016](https://www.microsoft.com/download/details.aspx?id=49492) stranica.

> [AZURE.NOTE] SSIS instaliran u sustavu SQL Server, ali nije uvršten u verziji Express. Informacije o koje aplikacije uključeni u različite izdanja sustava SQL Server potražite u odjeljku [Izdanja sustava SQL Server](http://www.microsoft.com/en-us/server-cloud/products/sql-server-editions/)

Materijal za obuku na SSIS, potražite u članku [Ruke na tečajeve za SSIS](http://www.microsoft.com/download/details.aspx?id=20766)

Informacije o tome kako se gore i pokretanje pomoću SISS da biste sastavili jednostavne izdvajanjem, transformaciju i paketi učitavanja (ETL), pročitajte članak [SSIS Praktični vodič: Stvaranje jednostavne paket ETL](https://msdn.microsoft.com/library/ms169917.aspx).

## <a name="download-nyc-taxi-dataset"></a>Preuzimanje dataset taksi SEBI  
Primjer opisane ovdje pomoću javno dostupna skup podataka – [NEW taksi Trips](http://www.andresmh.com/nyctaxitrips/) skupu podataka. Skup podataka sastoji se od otprilike 173 milijuna taksi posao jednim automobilom u NEW u godini 2013. Postoje dvije vrste podataka: putovanja detalja podataka i prijevoz podataka. Kao za svaki mjesec datoteke, imamo 24 datoteke u svim, od kojih svaka je približno 2GB dekomprimirati.


## <a name="upload-data-to-azure-blob-storage"></a>Prijenos podataka u spremište blobova platforme Azure
Da biste premjestili podatke pomoću u SSIS značajka paket lokalnim blobova platforme Azure, koristimo instance komponente [**Zadatak za prijenos blobova platforme Azure**](https://msdn.microsoft.com/library/mt146776.aspx), što je prikazano ovdje:

![Konfiguriranje-podataka – znanstvenog-vm](./media/machine-learning-data-science-move-data-to-azure-blob-using-ssis/ssis-azure-blob-upload-task.png)


Ovdje opisani su parametri koji koristi zadatka:


Polje|Opis|
----------------------|----------------|
**AzureStorageConnection**|Određuje na postojeće Upravitelja veze za pohranu za Azure ili stvara novi koja se odnosi na račun Azure prostora za pohranu koji pokazuje na kojem se nalaze datoteke blob.|
**BlobContainer**|Određuje naziv spremnika blob držite prenesenih datoteka kao BLOB-ova.|
**BlobDirectory**|Određuje direktorija blob pohranjuju prenesenih datoteka kao blob blok. Direktorija blob je virtualne hijerarhijske strukture. Ako već postoji blob-om, it ia zamijeniti.|
**LocalDirectory**|Određuje lokalni direktorij koja sadrži datoteke je moguće prenijeti.|
**Naziv datoteke**|Određuje naziv filtar da biste odabrali datoteke s uzorkom čiji je naziv naveden. Na primjer, MySheet\*.xls\* obuhvaća datoteke kao što su MySheet001.xls i MySheetABC.xlsx|
**TimeRangeFrom/TimeRangeTo**|Određuje raspon filtrom vrijeme. Datoteka izmijenjena nakon *TimeRangeFrom* i prije *TimeRangeTo* dodani su skriveni.|


> [AZURE.NOTE] Vjerodajnice **AzureStorageConnection** moraju biti točan i **BlobContainer** mora postojati prije prijenosa pokušava.

## <a name="download-data-from-azure-blob-storage"></a>Preuzimanje podataka iz spremišta blobova platforme Azure

Preuzimanje podataka iz spremišta blobova platforme Azure za pohranu s SSIS na lokaciji, koristite instance komponente [Zadatak za prijenos blobova platforme Azure](https://msdn.microsoft.com/library/mt146779.aspx).

##<a name="more-advanced-ssis-azure-scenarios"></a>Scenariji za naprednije SSIS Azure
Ne možemo Imajte na umu ovdje dopušta li SSIS paket značajki za složenije tijekove za rukovanje pakiranje zadaci zajedno. Na primjer, blob podataka može dolaziti izravno HDInsight klaster, čiji je izlaz se može preuzeti natrag u blob, a zatim na lokaciji prostora za pohranu. SSIS mogu se izvoditi grozd i Svinja na programa klaster HDInsight pomoću dodatnih poveznika SSIS:

- Da biste pokrenuli grozd skripte na programa Azure HDInsight klaster s SSIS, koristite [Azure HDInsight vrste Hive zadatka](https://msdn.microsoft.com/library/mt146771.aspx).
- Da biste pokrenuli Svinja skripte na programa Azure HDInsight klaster s SSIS, koristite [Azure HDInsight Svinja zadatka](https://msdn.microsoft.com/library/mt146781.aspx).
