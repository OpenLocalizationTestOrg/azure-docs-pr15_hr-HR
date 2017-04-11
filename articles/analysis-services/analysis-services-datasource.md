<properties
   pageTitle="Veze za izvor podataka | Microsoft Azure"
   description="U članku se opisuje veze s izvorima podataka za podatkovne modele u Azure Analysis Services."
   services="analysis-services"
   documentationCenter=""
   authors="minewiskan"
   manager="erikre"
   editor=""
   tags=""/>
<tags
   ms.service="analysis-services"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="na"
   ms.date="10/25/2016"
   ms.author="owend"/>

# <a name="datasource-connections"></a>Veza izvora podataka

Podatkovni modeli u Azure Analysis Services može zahtijevati davatelji podataka prilikom povezivanja s određenim izvorima podataka. U nekim slučajevima tabličnim modelima povezivanje s izvorima podataka pomoću nativnog davatelja usluge kao što je SQL Server nativni klijent (SQLNCLI11) može vratiti pogrešku.

Ako, na primjer; Ako ste se u memoriji ili izravne upit podatkovnog modela koji povezuje s izvorom podataka oblaku kao što su baze podataka SQL Azure, ako koristite nativni davatelji osim SQLOLEDB, vidjet ćete poruku o pogrešci: **"Provider"SQLNCLI11.1"nije registriran"**.

Ili, ako imate model DirectQuery povezivanje s lokalnim izvorima podataka, ako koristite davatelja usluga za nativni može se pojaviti poruka o pogrešci: **"Pogreška pri stvaranju postavljanje OLE DB retka. Neispravna sintaksa blizu 'Ograničenje' "**.

## <a name="data-source-providers"></a>Davatelji izvora podataka

Sljedećih davatelja izvora podataka je podržan u memoriji ili izravno upita podatkovne modele prilikom povezivanja s lokalnim ili izvora podataka u oblaku:

|               | **Izvor podataka**                     | **U memoriji**                            |  **Usmjeravanje upita**                                           |
|---------------------------|-------------------------------|---------------------------------------------|---------------------------------------------|
| **Oblak**                     | Data Warehouse Azure SQL      | .NET framework davatelj podataka za SQL Server | .NET framework davatelj podataka za SQL Server |
|                           | Baze podataka Azure SQL            | .NET framework davatelj podataka za SQL Server | .NET framework davatelj podataka za SQL Server |
| **Lokalni** (putem pristupnik) | SQL Server                    | SQL Server nativni klijent 11.0               | .NET framework davatelj podataka za SQL Server |
|                           |  SQL Server                             | Microsoft OLE DB davatelj usluga za SQL Server    |   .NET framework davatelj podataka za SQL Server                                          |
|                           |  SQL Server                             | .NET framework davatelj podataka za SQL Server |  .NET framework davatelj podataka za SQL Server                                           |
|                           | Oracle                        | Microsoft OLE DB davatelj usluga za Oracle        | Oracle Data Provider za .NET               |
|                           |  Oracle                             | Oracle Data Provider za .NET               | Oracle Data Provider za .NET                                            |
|                           | Teradata                      | OLE DB davatelj usluga za proizvode tvrtke Teradata                | Teradata Data Provider za .NET             |
|                           |  Teradata                             | Teradata Data Provider za .NET             |  Teradata Data Provider za .NET                                            |
|                           | Analitički sustav u platforme | .NET framework davatelj podataka za SQL Server | .NET framework davatelj podataka za SQL Server |


> [AZURE.NOTE] Provjerite je li davatelji 64-bitni instaliraju prilikom korištenja lokalnog pristupnika.

Prilikom migracije na tabličnog modela komponente SQL Server Analysis Services lokalnog za Azure Analysis Services, možda će biti potrebno promijeniti davatelja usluge.

**Da biste odredili davatelj izvora podataka**

1. U SSDT > **Tabličnog modela Explorer** > **Izvora podataka**, desnom tipkom miša kliknite vezu s izvorom podataka, a zatim kliknite **Uredi izvor podataka**.

2. U **Uređivanje veze**kliknite **Dodatno** da biste otvorili prozor svojstva Advance.

3. U **Postavili dodatna svojstva** > **davatelje usluga**, a zatim odaberite odgovarajuće davatelja usluga.

## <a name="impersonation"></a>Oponašanje
U nekim slučajevima možda će biti potrebno navesti oponašanja drugi račun. Oponašanje račun možete navesti SSDT ili SSMS.

Za lokalne izvore podataka:

- Ako koristite SQL provjeru autentičnosti, oponašanja mora biti račun servisa.
- Ako koristite provjeru autentičnosti sustava Windows, postaviti korisničku lozinku sustava Windows. Za SQL Server provjeru autentičnosti sustava Windows s računom određene oponašanja je podržano samo za u memoriji podatkovne modele.

Za izvori podataka u oblaku:

- Ako koristite SQL provjeru autentičnosti, oponašanja mora biti račun servisa.


## <a name="next-steps"></a>Daljnji koraci

Ako imate lokalnih izvora podataka, svakako instalirajte [lokalnog pristupnika](analysis-services-gateway.md). Dodatne informacije o upravljanju na poslužitelju u SSDT ili SSMS potražite u članku [Upravljanje poslužitelja](analysis-services-manage.md).
