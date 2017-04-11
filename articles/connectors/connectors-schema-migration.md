<properties
    pageTitle="Migriranje logike aplikacije u shemi verzija 2015-08-01-pretpregled | Aplikacije servisa za Microsoft Azure"
    description="Jednostavno možete migrirati logike aplikacija na najnoviju verziju sheme. Slijedite ove korake."
    services="logic-apps"
    documentationCenter=""
    authors="MSFTMAN"
    manager="erikre"
    editor=""
    tags="connectors"/>

<tags
    ms.service="logic-apps"
    ms.workload="integration"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="08/23/2016"
    ms.author="deonhe"/>

# <a name="how-to-migrate-logic-apps-to-schema-version-2015-08-01-preview"></a>Migriranje logike aplikacije u shemi verzija 2015-08-01-pretpregled

Da biste premjestili postojeće aplikacije logike novu shemu, učinite sljedeće:  
1. Otvorite aplikaciju logike na portalu za Azure  
2. Kliknite Ažuriraj sheme:

 ![Ikona API-JA][step1]   
Ažuriranje sheme stranice prikazuje i navode veze na dokument koji sadrže pojedinosti na poboljšanja u novu shemu: ![ikona API-JA][step2]

>[AZURE.NOTE] Kad odaberete **Ažuriranje shemu**, automatski pokrenuti koraka migracije i navedite izlaz kod za. To možete koristiti da biste ažurirali definicije, no pobrinite slijede dobar kodiranje prakse kao što su oni navedene u odjeljku **najbolje prakse** .

## <a name="best-practices-when-migrating-your-logic-apps-to-the-latest-schema-version"></a>Najbolje prakse prilikom migracije logike aplikacija na najnoviju verziju sheme:  

- Kopiranje migriranim skripte za novu aplikaciju logike – ne prebrišete stari jednu dok ste obavljeni testiranja i potvrđuje migriranim aplikaciju funkcionira ispravno.
- Testirajte svoje logike aplikaciju **prije** umetanja u radnog
- Po završetku migracije pokrenite ažuriranje aplikacija sustava logike koriste [upravljani API-ji](./apis-list.md) gdje je to moguće. Na primjer, možete početi koristiti v2 Dropbox, whereever koristite v1 zajedničke mrežne mape.


## <a name="whats-next"></a>Daljnji koraci
-  [Saznajte kako ručno migrirajte logike aplikacije](../app-service-logic/app-service-logic-schema-2015-08-01.md)


<!--Icon references-->
[step1]: ./media/connectors-schema-migration/migrateschema1.png
[step2]: ./media/connectors-schema-migration/migrateschema2.png






