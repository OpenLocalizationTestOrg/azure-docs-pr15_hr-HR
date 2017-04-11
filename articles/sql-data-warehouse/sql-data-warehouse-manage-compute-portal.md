<properties
   pageTitle="Upravljanje računalnim power u skladištu podataka SQL Azure (portal za Azure) | Microsoft Azure"
   description="Azure zadaci portala za upravljanje izračunati power. Promjena veličine izračunati resursi prilagodbom DWUs. Ili, zadržite pokazivač i nastaviti računalnim resursi troškova."
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="barbkess"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="08/22/2016"
   ms.author="barbkess;sonyama"/>

# <a name="manage-compute-power-in-azure-sql-data-warehouse-azure-portal"></a>Upravljanje računalnim power u skladištu podataka SQL Azure (portal za Azure)

> [AZURE.SELECTOR]
- [Pregled](sql-data-warehouse-manage-compute-overview.md)
- [Portal](sql-data-warehouse-manage-compute-portal.md)
- [PowerShell](sql-data-warehouse-manage-compute-powershell.md)
- [OSTALE](sql-data-warehouse-manage-compute-rest-api.md)
- [TSQL](sql-data-warehouse-manage-compute-tsql.md)


Skaliranje Vremensko mjerilo uspješnosti izračunati resurse i memorije da bi odgovarao zahtjeva za promjenom od svoje radno opterećenje. Troškova skaliranja natrag resursima tijekom vremena koje nisu Vršna ili privremeno zaustavljanje računalnim potpuno. 

Zbirku zadataka koristi Azure portala:

- Promjena veličine računalnim
- Zaustavljanje računalnim
- Životopis računalnim

Dodatne informacije potražite u članku [Upravljanje izračunati pregled][].

<a name="scale-performance-bk"></a>
<a name="scale-compute-bk"></a>

## <a name="scale-compute-power"></a>Promjena veličine računalnim power

[AZURE.INCLUDE [SQL Data Warehouse scale DWUs description](../../includes/sql-data-warehouse-scale-dwus-description.md)]

Da biste promijenili računalnim resurse:

1. Otvorite [portal za Azure][], otvorite bazu podataka pa kliknite **mjerilo**.

    ![Kliknite mjerilo][1]

1. U plohu skaliranje premjestite klizač ulijevo ili udesno da biste promijenili postavku DWU.

    ![Pomaknite klizač][2]

1. Kliknite **Spremi**. Pojavit će se poruka potvrde. Kliknite **da** da biste potvrdili ili **ne** da biste ih poništili.

    ![Kliknite Spremi][3]

<a name="pause-compute-bk"></a>

## <a name="pause-compute"></a>Zaustavljanje računalnim

[AZURE.INCLUDE [SQL Data Warehouse pause description](../../includes/sql-data-warehouse-pause-description.md)]

Pauziranje baze podataka:

1. Otvorite [portal za Azure][] i otvorite bazu podataka. Obratite pozornost na to je li Status **Online**. 

    ![Prikaz mrežnog statusa][6]

1. Da biste privremeno obustavljanje računalnim i memorije, kliknite **Pauziraj**, a zatim potvrdnu poruku pojavit će se. Kliknite **da** da biste potvrdili ili **ne** da biste ih poništili.

    ![Potvrda Pauziraj][7]

1. SQL Data Warehouse se pokreće bazu podataka, status je **Pausing**.
2. Kad je status **Paused**, obavlja postupak Pauziraj i vam se više ne naplaćuju za DWUs.

    ![Zaustavljanje stanja][4]

<a name="resume-compute-bk"></a>

## <a name="resume-compute"></a>Životopis računalnim

[AZURE.INCLUDE [SQL Data Warehouse resume description](../../includes/sql-data-warehouse-resume-description.md)]Da biste nastavili baze podataka:

1. Otvorite [portal za Azure][] i otvorite bazu podataka. Obratite pozornost na to je li Status **Paused**. 

    ![Zaustavljanje baze podataka][4]

1. Da biste nastavili baze podataka kliknite **Start**, a zatim će se prikazati potvrdnu poruku. Kliknite **da** da biste potvrdili ili **ne** da biste ih poništili.

    ![Potvrda životopis][5]

1. SQL Data Warehouse se pokreće bazu podataka, status je "Resuming".
2. Status je na **mreži**, baza podataka spremna je.

    ![Prikaz mrežnog statusa][6]

<a name="next-steps-bk"></a>

## <a name="next-steps"></a>Daljnji koraci
Dodatne informacije potražite u članku [Pregled upravljanja][].

<!--Image references-->
[1]: ./media/sql-data-warehouse-manage-compute-portal/click-scale.png
[2]: ./media/sql-data-warehouse-manage-compute-portal/move-slider.png
[3]: ./media/sql-data-warehouse-manage-compute-portal/click-save.png
[4]: ./media/sql-data-warehouse-manage-compute-portal/resume-database.png
[5]: ./media/sql-data-warehouse-manage-compute-portal/resume-confirm.png
[6]: ./media/sql-data-warehouse-manage-compute-portal/pause-database.png
[7]: ./media/sql-data-warehouse-manage-compute-portal/pause-confirm.png

<!--Article references-->
[Pregled upravljanja]: ./sql-data-warehouse-overview-manage.md
[Upravljanje računalnim pregled]: ./sql-data-warehouse-manage-compute-overview.md

<!--MSDN references-->


<!--Other Web references-->

[Portal za Azure]: http://portal.azure.com/
