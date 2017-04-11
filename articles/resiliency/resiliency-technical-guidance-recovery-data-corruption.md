<properties
   pageTitle="Otpornost Tehnički vodič za oporavak od oštećenja podataka ili nenamjerno brisanje | Microsoft Azure"
   description="Članak na objašnjenje kako podataka oštećenje podataka ili brisanje slučajne podataka da biste i dizajniranje pogreške aplikacije kvara prebacuju, Visoko dostupna, kao i planiranje Izrada oporavak"
   services=""
   documentationCenter="na"
   authors="adamglick"
   manager="saladki"
   editor=""/>

<tags
   ms.service="resiliency"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/18/2016"
   ms.author="aglick"/>

#<a name="azure-resiliency-technical-guidance-recovery-from-data-corruption-or-accidental-deletion"></a>Azure otpornost Tehnički vodič: oporavak od oštećenja podataka ili nenamjerno brisanje

Dio robusne poslovni plan continuity ima plan ako podataka dobiti oštećen ili je slučajno izbrisali. Sljedeće se informacije o oporavku nakon podataka oštećena ili slučajno izbrisali, zbog pogreške aplikacije ili operator pogreške.

##<a name="virtual-machines"></a>Virtualnim strojevima

Da biste zaštitili na virtualnim računalima sustava Azure (ponekad se zove i infrastrukture kao-na-usluga VMs) iz pogreške aplikacije ili nenamjerno brisanje, pomoću [Azure sigurnosnu kopiju](https://azure.microsoft.com/services/backup/). Azure sigurnosne kopije omogućuje stvaranje sigurnosne kopije koje su u skladu na više VM diskova. Osim toga, sigurnog sigurnosne kopije moguće je replicirati preko područja za oporavak od gubitka regija.

##<a name="storage"></a>Prostor za pohranu

Imajte na umu da dok Azure prostora za pohranu nudi otpornost podataka pomoću automatskog replike, to ne sprječava vaše aplikacije kod (ili razvojni inženjeri/korisnika) iz oštećenje podataka putem slučajne ili neželjeni brisanja, ažuriranje i tako dalje. Održavanje podataka vjernosti in the face of aplikacija ili korisnik pogreške zahtijeva više napredne postupke, kao što su kopiranja podataka na sekundarnom prostora za pohranu mjesto s zapisnik nadzora. Razvojni inženjeri mogu iskoristiti [mogućnost brze snimke](https://msdn.microsoft.com/library/azure/ee691971.aspx)blob koje možete stvarati samo za čitanje točke u vrijeme snimke blob sadržaja. To se može koristiti kao osnovu podataka vjernosti rješenja za pohranu Azure blob-ova.

###<a name="blob-and-table-storage-backup"></a>Blob i sigurnosno kopiranje tablice za pohranu

Dok su izrazito durable blob-ova i tablice, uvijek predstavljaju trenutno stanje podatke. Oporavak od neželjenih izmjena ili brisanje podataka možda ćete morati vraćanja podataka u prethodno stanje. To se može postići prihvaćanjem iskoristite prednosti upravljanja nudi Azure za pohranu i zadržati kopije točke u vrijeme.

Za Azure blob-ova, možete izvesti sigurnosne kopije točke u vrijeme pomoću [značajke brze snimke blob](https://msdn.microsoft.com/library/ee691971.aspx). Za svaki snimka, samo se naplatiti za spremište potrebno za pohranu razlike unutar blob-om od posljednjeg snimke stanja. Snimki ovise o postojanje parametra izvorne blob kojima se temelje, pa se postupak kopiranja drugi blob ili čak i neki drugi račun za pohranu. Provjerava je li sigurnosne kopije podataka ispravno zaštićen protiv nenamjerno brisanje. Za Azure tablice, možete učiniti kopije točke u vrijeme u neku drugu tablicu ili Azure blob-ova. Detaljnije upute i primjeri izvođenje razini aplikacije sigurnosne kopije tablice i blob polja nalazi se ovdje:

  * [Zaštita tablice na temelju pogreške aplikacije](https://blogs.msdn.microsoft.com/windowsazurestorage/2010/05/03/protecting-your-tables-against-application-errors/)
  * [Zaštita blob-ova protiv pogreške aplikacije](https://blogs.msdn.microsoft.com/windowsazurestorage/2010/04/29/protecting-your-blobs-against-application-errors/)

##<a name="database"></a>Baze podataka

Postoji nekoliko mogućnosti za [poslovanje](../sql-database/sql-database-business-continuity.md) (sigurnosne kopije, vraćanje) dostupni su za baze podataka SQL Azure. Baza podataka može kopirati pomoću funkcije [Kopiju baze podataka](../sql-database/sql-database-copy.md) , ili [Izvoz](../sql-database/sql-database-export.md) i [Uvoz](https://msdn.microsoft.com/library/hh710052.aspx) datoteke bacpac SQL Server. Baza podataka sadrži putem transakcije dosljedan rezultate, dok bacpac (putem servisa za uvoz/izvoz) ne. Obje mogućnosti Pokreni kao usluge utemeljene na red unutar podatkovnog centra, a ne trenutno pružaju vrijeme završetka SLA.

>[AZURE.NOTE]Mogućnost Kopiraj i uvoz/izvoz baze podataka postavite značajan stupanj Učitaj na izvorišne baze podataka. Ih možete pokrenuti Nadmetanje resursa ili Ograničavanje događaja.

###<a name="sql-database-backup"></a>Sigurnosno kopiranje baze podataka SQL

Sigurnosno kopiranje točke u vrijeme za baze podataka sustava Microsoft Azure SQL postići tako da [kopirate baze podataka Azure SQL](../sql-database/sql-database-copy.md). Koristite sljedeću naredbu da biste stvorili putem transakcije dosljedan kopiju baze podataka na istom poslužitelju logičke baze podataka ili na drugi poslužitelj. U svakom slučaju, kopiju baze podataka je potpuno funkcionalni i potpuno neovisno o izvorišne baze podataka. Svaku kopiju stvarate predstavlja mogućnost oporavka točke u vrijeme. Stanje baze podataka možete vratiti potpuno promjenom naziva nove baze podataka pomoću izvorni naziv baze podataka. Osim toga, možete oporaviti određeni podskup podataka iz nove baze podataka pomoću Transact-SQL upita. Dodatne informacije o SQL baze podataka potražite u članku [Pregled poslovanje s bazom podataka SQL Azure](../sql-database/sql-database-business-continuity.md).

###<a name="sql-server-on-virtual-machines-backup"></a>SQL Server na virtualnim strojevima sigurnosnog kopiranja

Za SQL Server s Azure infrastrukture crti na virtualnim strojevima servisa (često se nazivaju IaaS ili IaaS VMs), postoje dvije mogućnosti: tradicionalni sigurnosne kopije i dostave zapisnika. Pomoću klasične sigurnosne kopije omogućuje da biste vratili na određeno mjesto u vremenu, ali postupka oporavka je spora. Vraćanje tradicionalni sigurnosnih kopija zahtijeva počevši od do početne potpuno sigurnosno kopiranje i Primjena sve sigurnosne kopije snimanja nakon toga. Druga mogućnost je da biste konfigurirali zapisnik dostavu sesiju da biste odgodili vraćanja sigurnosne kopije zapisnika (na primjer, ako dva sata). To omogućuje prozora da biste vratili iz pogreške na primarni.

##<a name="other-azure-platform-services"></a>Druge usluge platforme Azure

Neki servisi platforme Azure pohrane podataka u korisnički kontrolirano prostora za pohranu računa ili baze podataka SQL Azure. Ako račun ili prostora za pohranu resursa se izbrišu ili oštećena, to može uzrokovati ozbiljne pogreške sa servisom. U tim slučajevima je važno da bi se zadržao sigurnosne kopije koju želite omogućiti da biste ponovno stvorili ove resurse ako ste izbrisali ili oštećen.

Azure web-mjesta i servisa Azure Mobile, sigurnosno kopirati i održavanje pridružene baze podataka. Servisa Azure Media i virtualnih računala, morate zadržati pridruženog računa Azure prostora za pohranu i sve resursa u taj račun. Ako, na primjer, virtualnim strojevima koje morate sigurnosno kopiranje i upravljanje njima diskova VM blobova platforme Azure.

##<a name="checklists-for-data-corruption-or-accidental-deletion"></a>Kontrolni popisi za oštećenja podataka ili nenamjerno brisanje

##<a name="virtual-machines-checklist"></a>Kontrolni popis za virtualnim strojevima

  1. Pročitajte odjeljak virtualnim strojevima ovog dokumenta.
  2. Stvaranje sigurnosne kopije i održavanje diskova VM Azure sigurnosne kopije (ili vlastite sigurnosne kopije sustava pomoću blobova platforme Azure i VHD snimke).

##<a name="storage-checklist"></a>Kontrolni popis za pohranu

  1. Pročitajte odjeljak za pohranu ovog dokumenta.
  2. Redovito sigurnosno kopirajte resurse ključnih prostora za pohranu.
  3. Preporučujemo da koristite značajku snimka za blob-ova.

##<a name="database-checklist"></a>Kontrolni popis za baze podataka

  1. Pročitajte odjeljak baze podataka ovog dokumenta.
  2. Stvaranje sigurnosne kopije točke u vrijeme pomoću naredbe Kopiraj bazu podataka.

##<a name="sql-server-on-virtual-machines-backup-checklist"></a>SQL Server na virtualnim strojevima sigurnosne kopije kontrolnog popisa

  1. Pregledajte SQL Server na virtualnim strojevima sigurnosne kopije sekcije ovog dokumenta.
  2. Pomoću klasične sigurnosno kopiranje i vraćanje tehnike.
  3. Stvaranje odgođeno zapisnika dostavu sesiju.

##<a name="web-apps-checklist"></a>Kontrolni popis za Web Apps

  1. Sigurnosno kopiranje i održavanje pridruženih baza podataka, ako postoje.

##<a name="media-services-checklist"></a>Kontrolni popis za Media Services

  1. Sigurnosno kopiranje i održavanje resurse povezane prostora za pohranu.

##<a name="more-information"></a>Dodatne informacije

Dodatne informacije o značajkama Azure sigurnosnog kopiranja i vraćanja potražite u članku [web-mjesto za pohranu, u okvir za sigurnosno kopiranje i vraćanje scenarijima](https://azure.microsoft.com/documentation/scenarios/storage-backup-recovery/).

##<a name="next-steps"></a>Daljnji koraci

Ovaj je članak dio niza filtriran na [Azure otpornost Tehnički vodič](./resiliency-technical-guidance.md). Ako tražite dodatne otpornost, oporavak Izrada i resursima visoke dostupnosti, potražite u članku Tehnički vodič Azure otpornost [Dodatne resurse](./resiliency-technical-guidance.md#additional-resources).