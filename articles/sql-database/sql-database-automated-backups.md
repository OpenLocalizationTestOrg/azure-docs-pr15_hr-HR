<properties
   pageTitle="Sigurnosno kopiranje baze podataka SQL - automatsko, suvišne zemlj. | Microsoft Azure" 
   description="Baze podataka SQL automatski stvara sigurnosnu kopiju lokalne baze podataka svakih pet minuta i osigurati zemlj redundanciju koristi Azure pristup za čitanje zemlj suvišnih prostora za pohranu (RA GRS). "
   services="sql-database"
   documentationCenter=""
   authors="CarlRabeler"
   manager="jhubbard"
   editor="monicar"/>

<tags
   ms.service="sql-database"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="10/20/2016"
   ms.author="carlrab;barbkess"/>

<!------------------
This topic is annotated with TEMPLATE guidelines for FEATURE TOPICS.


Metadata guidelines

pageTitle
    60 characters or less. Includes name of the feature - primary benefit. Not the same as H1. Its 60 characters or fewer including all characters between the quotes and the Microsoft Azure site identifier.

description
    115-145 characters. Duplicate of the first sentence in the introduction. This is the abstract of the article that displays under the title when searching in Bing or Google. 

    Example: "SQL Database automatically creates a local database backup every few minutes and uses Azure read-access geo-redundant storage for geo-redundancy."
------------------>

<!----------------

TEMPLATE GUIDELINES for feature topics

The Feature Topic is a one-pager (ok, sometimes longer) that explains a capability of the product or service. It explains what the capability is and characteristics of the capability.  

It is a "learning" topic, not an action topic.

DO explain this:
    • Definition of the feature terminology.  i.e., What is a database backup?
    • Characteristics and capabilities of the feature. (How the feature works)
    • Common uses with links to overview topics that recommend when to use the feature.
    • Reference specifications (Limitations and Restrictions, Permissions, General Remarks, etc.)
    • Next Steps with links to related overviews, features, and tasks.

DON'T explain this:
    • How to steps for using the feature (Tasks)
    • How to solve business problems that incorporate the feature (Overviews)
------------------->

<!------------------
GUIDELINES for the H1 
    
    The H1 should answer the question "What is in this topic?" Write the H1 heading in conversational language and use search key words as much as possible. Since this is a learning topic, make sure the title indicates that and doesn't mislead people to think this will tell them how to do tasks.  
    
    To help people understand this is a learning topic and not an action topic, start the title with "Learn about ... "

    Heading must use an industry standard term. If your feature is a proprietary name like "Elastic database pools", use a synonym. For example:    "Learn about elastic database pools for multi-tenant databases". In this case multi-tenant database is the industry-standard term that will be an anchor for finding the topic.

-------------------->

# <a name="learn-about-sql-database-backups"></a>Dodatne informacije o sigurnosne kopije baze podataka SQL

<!------------------
    GUIDELINES for introduction
    
    The introduction is 1-2 sentences.  It is optimized for search and sets proper expectations about what to expect in the article. It should contain the top key words that you are using throughout the article.The introduction should be brief and to the point of what the feature is, what it is used for, and what's in the article. 

    If the introduction is short enough, your article can pop to the top in Google Instant Answers.

    In this example:
    
 

Sentence #1 Explains what the article will cover, which is what the feature is or does. This is also the metadata description. 
    SQL Database automatically creates a local database backup every five minutes and uses Azure read-access geo-redundant storage (RA-GRS) to provide geo-redundancy. 

Sentence #2 Explains why I should care about this.  
    Database backups are an essential part of any business continuity and disaster recovery strategy because they protect your data from accidental corruption or deletion.

-------------------->

Baze podataka SQL automatski stvara sigurnosnu kopiju lokalne baze podataka svakih nekoliko minuta i koristi Azure pristup za čitanje zemlj suvišnih prostora za pohranu za zemlj zalihosti. Sigurnosne kopije baze podataka su ključni dio sve poslovne continuity i Izrada strategije oporavak jer zaštita podataka iz slučajne oštećenja ili brisanja. 

<!-- This image needs work, so not putting it in right now.

This diagram shows SQL Database running in the US East region. It creates a database backup every five minutes, which it stores locally to Azure Read Access Geo-redundant Storage (RA-GRS). Azure uses geo-replication to copy the database backups to a paired data center in the US West region.

![geo-restore](./media/sql-database-geo-restore/geo-restore-1.png)

-->

<!---------------
GUIDELINES for the first ## H2.

    The first ## describes what the feature encompasses and how it is used. It points to related task articles.
    
    For consistency, being the heading with "What is ... "
----------------->

## <a name="what-is-a-sql-database-backup"></a>Što je sigurnosnu kopiju baze podataka SQL?  

<!-- 
    Explains what a SQL Database backup is and answers an important question that people want to know.
-->

Sigurnosno kopiranje baze podataka SQL sadrži sigurnosne kopije lokalne baze podataka i zemlj suvišnih sigurnosne kopije. Ove sigurnosne kopije se stvaraju automatski i bez dodatnih troškova. Ne morate ništa da biste ih dogoditi.

<!----------------- 
    Explains first component of the backup feature
------------------>

Za lokalno sigurnosno kopiranje baze podataka SQL koristi tehnologije SQL Server da biste stvorili [cijelog](https://msdn.microsoft.com/library/ms186289.aspx) [razlikovno](https://msdn.microsoft.com/library/ms175526.aspx )i [zapisnik transakcija](https://msdn.microsoft.com/library/ms191429.aspx) sigurnosne kopije. Kopija zapisnik transakcija dogoditi svakih pet minuta, što vam omogućuje da do točke u vrijeme vraćanje na isti poslužitelj koji hostira baze podataka. Vraćanje baze podataka, odlučuje koje cijelog, sigurnosnoj kopiji promjena i transakcije zapisnika sigurnosne kopije moraju biti vraćena o servis.

<!--------------- 
    Explicit list of what to do with a local backup. "Use a ..." helps people to scan the topic and find the uses quickly.
---------------->

Koristite sigurnosnu kopiju lokalne baze podataka za:

- Vraćanje baze podataka da biste točke-na-jedan unutar razdoblje zadržavanja. S sigurnosnu kopiju baze podataka možete vratiti baze podataka da biste točke-na-jedan, vraćanje izbrisanih baze podataka u vrijeme je izbrisana ili vraćanje baze podataka na drugi zemljopisnom području. Da biste izvršili vraćanja, potražite u članku [Vraćanje baze podataka iz sigurnosne kopije baze podataka](sql-database-recovery-using-backups.md).

- Kopirajte bazu podataka sustava SQL server u istom ili drugom području. Kopija je putem transakcije usklađene s trenutne baze podataka SQL. Da biste izvršili kopiju, potražite u članku [kopiranje baze podataka](sql-database-copy.md).

- Arhiviranje sigurnosnu kopiju baze podataka izvan razdoblje zadržavanja sigurnosne kopije. Da biste izvršili arhiviranje, [Izvezi u SQL baze podataka u BACPAC](sql-database-export.md) datoteku. Možete arhivirati BACPAC Dugoročne za pohranu i spremite ga izvan vaše razdoblje zadržavanja. Ili pomoću u BACPAC da biste prenijeli kopiju baze podataka sustava SQL Server, bilo lokalno ili na Azure virtualnog računala (VM).

<!----------------- 
    Explains first component of the backup feature
------------------>

Za zemlj suvišnih sigurnosne kopije baze podataka SQL koristi [replikacije Azure prostora za pohranu](../storage/storage-redundancy.md). SQL baze podataka pohranjuje datoteke sigurnosne kopije lokalne baze podataka na račun [Za pohranu zemlj suvišnih pristup za čitanje (RA GRS)](../storage/storage-redundancy.md#read-access-geo-redundant-storage) . Azure replicira datoteke sigurnosne kopije u [paru podatkovnog centra](../best-practices-availability-paired-regions.md). 

<!--------------- 
    Explicit list of what to do with a geo-redundant backup. "Use a ..." helps people to scan the topic and find the uses quickly.
---------------->

Korištenje zemlj suvišnih sigurnosnu kopiju da biste:

- Vraćanje baze podataka na drugo područje zemljopisnim u slučaju da ne može pristupiti sigurnosnu kopiju baze podataka u vašoj regiji primarnom bazom podataka. 

>[AZURE.NOTE] U Azure spremište termina *replikacije* se odnosi na kopiranje datoteka s jednog mjesta na drugo. SQL- *replikacije baze podataka* se odnosi na čuvanje više sekundarne baza podataka sinkronizira s primarnom bazom podataka. 

<!----------------
    The next ## H2's discuss key characteristics of how the feature works. The title is in conversational language and asks the question that will be answered.
------------------->
## <a name="how-much-backup-storage-is-included-at-no-cost"></a>Koliko prostora za pohranu sigurnosne kopije se nalazi pri bez trošak?

Baze podataka SQL daje do 200% prostora za pohranu Maksimalna dodijeljenu bazu podataka kao sigurnosnu kopiju prostor za pohranu uz bez dodatnih troškova. Ako, na primjer, ako imate standardni DB instance s veličinom dodijeljenu DB 250 GB, imate 500 GB prostora za pohranu za sigurnosno kopiranje bez dodatnih troškova. Ako bazu podataka prelazi navedeni sigurnosne kopije prostora za pohranu, možete odabrati da biste smanjili razdoblje zadržavanja tako da se obratite podršci za Azure. Druga mogućnost je za plaćanje vrlo sigurnosne kopije za pohranu naplate pri standardna satnica pristup za čitanje geografski suvišnih prostora za pohranu (RA GRS). 

## <a name="how-often-do-backups-happen"></a>Koliko se često učinite dogoditi sigurnosne kopije?

Za lokalne sigurnosne kopije baze podataka, sigurnosne kopije baze podataka za cijeli dogoditi tjedno sigurnosne kopije baze podataka za razlikovno pokreću hourly i zapisnik transakcija sigurnosne kopije dogoditi svakih pet minuta. Prvi potpuno sigurnosno kopiranje zakazanog odmah nakon stvaranja baze podataka. Obično dovršava sljedećih 30 minuta, ali može potrajati više kada se baza podataka nalazi značajan veličine. Ako, na primjer, Početna sigurnosne kopije može trajati dulje vraćenu bazu podataka ili kopiju baze podataka. Nakon prve potpune sigurnosne sve daljnje sigurnosne kopije su zakazali automatski i upravlja tihu u pozadini. Točno vrijeme cijelog i [razlikovnog](https://msdn.microsoft.com/library/ms175526.aspx) sigurnosne kopije baze podataka određuje se pri uravnoteživanju radno opterećenje za cjelokupnog sustava. 

Za zemlj suvišnih sigurnosne kopije punog i razlikovnog sigurnosne kopije se kopiraju prema rasporedu replikacije Azure prostora za pohranu.

## <a name="how-long-do-you-keep-my-backups"></a>Koliko dugo zadržati moje sigurnosne kopije

Svaki sigurnosnu kopiju baze podataka SQL ima razdoblje zadržavanja koja se temelji na [servis sloju](sql-database-service-tiers.md) baze podataka. Razdoblje zadržavanja za bazu podataka u na:

<!------------------

    Using a list so the information is easy to find when scanning.
------------------->

- Osnovna usluga sloju je sedam dana.
- Razina standardnog servisa je 35 dana.
- Razina Premium servis je 35 dana.


Ako ste prijeći na nižu bazu podataka iz servisa razine standardni prikaz ili Premium osnovni, sigurnosnih kopija spremaju se sedam dana. Sve postojeće sigurnosne kopije starije od sedam dana više nisu dostupni. 

Ako nadogradite vašu bazu podataka iz sloju osnovna usluga za standardni prikaz ili Premium baze podataka SQL zadržava postojeće sigurnosne kopije dok su 35 dana. Kako nastaju 35 dana, zadržava nove sigurnosne kopije.
 
Ako izbrišete baze podataka, baze podataka SQL zadržava sigurnosnih kopija na isti način kao što bi online baze podataka. Ako, na primjer, pretpostavimo da izbrišete osnovni bazu podataka koja sadrži razdoblje zadržavanja od sedam dana. Sigurnosne kopije koju nije četiri dana spremaju se za tri više dana.

>[AZURE.IMPORTANT]
    Ako izbrišete Azure SQL server domaćini SQL baze podataka, a zatim sve baze podataka koji pripadaju poslužitelj brišu se i i nije moguć. Nije moguće vratiti izbrisane poslužitelja.

<!-------------------
OPTIONAL section
## Best practices 
--------------------->

<!-------------------
OPTIONAL section
## General remarks
--------------------->

<!-------------------
OPTIONAL section
## Limitations and restrictions
--------------------->

<!-------------------
OPTIONAL section
## Metadata
--------------------->

<!-------------------
OPTIONAL section
## Performance
--------------------->

<!-------------------
OPTIONAL section
## Permissions
--------------------->

<!-------------------
OPTIONAL section
## Security
--------------------->

<!-------------------
GUIDELINES for Next Steps

    The last section is Next Steps. Give a next step that would be relevant to the customer after they have learned about the feature and the tasks associated with it.  Perhaps point them to one or two key scenarios that use this feature.

    You don't need to repeat links you have already given them.
--------------------->

## <a name="next-steps"></a>Daljnji koraci

Sigurnosne kopije baze podataka su ključni dio sve poslovne continuity i Izrada strategije oporavak jer zaštita podataka iz slučajne oštećenja ili brisanja. Da biste vidjeli kako baze podataka sigurnosnih kopija u šira strategiju, potražite u članku [Pregled continuity tvrtke](sql-database-business-continuity.md).


