<properties
    pageTitle="Što radnih opterećenja možete zaštititi pomoću oporavak Azure web-mjesta?"
    description="Oporavak Azure web-mjesta štiti radnih opterećenja i aplikacijama tako koordinaciju replikacije, prebacivanje i oporavak lokalnog virtualnim strojevima i fizičke poslužitelje Azure ili sekundarni lokalnog web-mjesta"
    services="site-recovery"
    documentationCenter=""
    authors="rayne-wiselman"
    manager="cfreeman"
    editor=""/>

<tags
    ms.service="site-recovery"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.tgt_pltfrm="na"
    ms.workload="storage-backup-recovery"
    ms.date="10/10/2016"
    ms.author="raynew"/>

# <a name="what-workloads-can-you-protect-with-azure-site-recovery"></a>Što radnih opterećenja možete zaštititi pomoću oporavak Azure web-mjesta?


U ovom se članku opisuju radnih opterećenja i aplikacijama mogli ponoviti sa servisom Azure oporavak web-mjesta.

Pri dnu ovog članka ili na [Forum servise za oporavak Azure](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr)objavljuju komentare ni pitanja.

## <a name="overview"></a>Pregled

Tvrtke i ustanove moraju tvrtke continuity i Izrada oporavak (BCDR) strategije da biste zadržali radnih opterećenja i podataka sigurnih i dostupna tijekom planirane i Neplanirana nedostupnost čim oporavak običnog rad uvjeta.

Oporavak web-mjesta je Azure service doprinosa strategije BCDR. Pomoću oporavak web-mjesta, možete implementirati aplikaciju umu replikacije oblaka ili sekundarnog web-mjesta. Hoće li se nalaze aplikacije Windows ili Linux temelji, pokrenute na fizičke poslužiteljima, VMware ili Hyper-V oporavak web-mjesta možete koristiti da biste orkestrirali replikacije, izvođenje Izrada oporavak ispitivanja i pokretanje failovers i failback.


Oporavak web-mjesta integrira aplikacije Microsoft, uključujući sustava SharePoint, Exchange, Dynamics, SQL Server i servisa Active Directory. Microsoft funkcionira i usko s početne dobavljačima uključujući Oracle, SAP, IBM i crveno razgovor. Možete prilagoditi replikacije rješenja na osnovi aplikacije po aplikacije.

## <a name="why-use-site-recovery-for-application-replication"></a>Zašto koristiti oporavak web-mjesta za replikaciju aplikacije?

Oporavak web-mjesta cjelini prema zaštitu na razini aplikacije i oporavak na sljedeći način:

- Aplikacija – agnostic, pruža replikacije za sve radnih opterećenja izvodi na podržani računalo.
- Pri sinkronizirano s replikacijom s RPOs niske, do 30 sekundi prema potrebama najvažnija poslovnih aplikacija.
- Aplikacija dosljedan snimki za jednu ili više razina aplikacija.
- Integracija sa SQL Server AlwaysOn i partnerstvo s drugim razini aplikacije tehnologije za replikaciju, uključujući replikacije AD SQL AlwaysOn, grupe dostupnosti baze podataka sustava Exchange (DAGs) i straža podataka tvrtke Oracle.
- Tarife fleksibilne oporavak koje omogućuju vam da biste oporavili u stogu cijelu aplikaciju jednim klikom, a obuhvaćaju za uključivanje vanjskih skripte i ručno akcije u planu.
- Upravljanje Napredne mrežne oporavak web-mjesta i Azure da biste pojednostavnili aplikacije mreže, uključujući mogućnost rezervirati IP adrese konfigurirati ujednačavanje opterećenja i integracija s Azure promet za upravljanje malo switchovers RTO mreže.
-  Obogaćeni Automatizacija biblioteke koja omogućuje podrška za proizvodnju, specifičnim aplikacijama skripte koje se mogu preuzeti i integriran s tarife za oporavak.



## <a name="workload-summary"></a>Radno opterećenje sažetka

Oporavak web-mjesta mogu replicirati bilo koju aplikaciju izvodi na podržani računalo. Uz to ćemo ste partnered sa timovima proizvoda da biste izveli dodatne testiranje specifične za aplikacije.

**Radno opterećenje** | **Replicirati Hyper-V VMs sekundarne web-mjesto** | **Da biste Azure replicirati VMs Hyper-V** | **VMware VMs replikaciju sekundarnu web-mjesto** | **Replicirati VMware VMs za Azure**
---|---|---|---|---
Active Directory, DNS-a | Y | Y | Y | Y
Web-aplikacije (IIS, SQL) | Y | Y | Y | Y
Centar za sustav Operations Manager | Y | Y | Y | Y
SharePoint | Y | Y | Y | Y
SAP-A<br/><br/>Replicirati SAP-a web-mjesta za Azure za koje nisu klaster | Y (testirati Microsoft) | Y (testirati Microsoft) | Y (testirati Microsoft) | Y (testirati Microsoft)
Exchange (koji nisu DAG) | Y | dolazim brzo | Y | Y
Remote Desktop/VDI | Y | Y | Y | N/D
Linux (operacijski sustav i aplikacije) | Y (testirati Microsoft) | Y (testirati Microsoft) | Y (testirati Microsoft) | Y (testirati Microsoft)
Dynamics AX za Outlook | Y | Y | Y | Y
Dynamics CRM | Y | dolazim brzo | Y | dolazim brzo
Oracle | Y (testirati Microsoft) | Y (testirati Microsoft) | Y (testirati Microsoft) | Y (testirati Microsoft)
Datotečnom poslužitelju sustava Windows | Y | Y | Y | Y


## <a name="replicate-active-directory-and-dns"></a>Replicirati servisa Active Directory i DNS- a

Active Directory i DNS infrastrukture su ključne za većinu enterprise aplikacije. Tijekom oporavka Izrada, morat ćete Zaštita i oporavak te komponente infrastrukture prije oporaviti radnih opterećenja i aplikacije.

Oporavak web-mjesta možete koristiti da biste stvorili plan za oporavak dovršeno automatiziranog Izrada za Active Directory i DNS- a. Na primjer, ako želite da se neće putem sustava SharePoint i SAP iz primarni sekundarnog web-mjesta, možete postaviti plan za oporavak koji prekida putem servisa Active Directory, a zatim tarifu dodatne specifične za aplikaciju oporavak uvoza putem u ostale aplikacije koje ovise o servisa Active Directory.

[Dodatne informacije](site-recovery-active-directory.md) o zaštiti servisa Active Directory i DNS- a.

## <a name="protect-sql-server"></a>Zaštita sustava SQL Server

SQL Server nudi foundation za usluge podataka za podatkovni servisi za mnoge poslovnih aplikacija na lokalni podatkovnog centra.  Oporavak web-mjesta može se koristiti zajedno s tehnologije SQL Server HA/DR da biste zaštitili višestruki tiered enterprise aplikacije koje koristi SQL Server. Oporavak web-mjesta omogućuje:

- Oporavak rješenje jednostavne i učinkovit Izrada za SQL Server. Replicirati više verzija i izdanja sustava SQL Server samostalne poslužitelje i klastere, Azure ili sekundarnog web-mjesta.  
- Integracija sa SQL AlwaysOn dostupnost grupe da biste upravljali prebacivanje i failback tarife za oporavak oporavak Azure web-mjesta.
- Oporavak završetka do kraja tarife za sve razine u aplikaciji, uključujući baze podataka sustava SQL Server.
- Učitava uz oporavak web-mjesta tako da "bursting" ih u veće virtualnog računala IaaS u Azure skaliranje sustava SQL Server za Vršna.
- Jednostavno testiranje oporavak Izrada SQL Server. Možete pokrenuti test failovers analiza podataka i pokretanje provjere usklađenosti bez utjecaja radnom okruženju.

[Dodatne informacije](site-recovery-sql.md) o zaštiti sustava SQL server.

##<a name="protect-sharepoint"></a>Zaštita sustava SharePoint

Oporavak Azure web-mjesta štiti implementacijama sustava SharePoint, na sljedeći način:

- Nema potrebe i troškove povezane infrastrukture za farmu po za umetanje crtičnog koda za oporavak Izrada. Upotrijebite oporavak web-mjesta za replikaciju cijele farme poslužitelja (razine Web, aplikacija i baze podataka) Azure ili sekundarnog web-mjesta.
- Pojednostavljuje aplikacije implementacije i upravljanja. Ažuriranja implementiran na web-mjestu primarni automatski replicirati, a time dostupno nakon prebacivanje i oporavak farme u sekundarnog web-mjesta. Spušta i složenosti upravljanja i troškove vezane uz zadržavanje farme za umetanje crtičnog koda po ažuran.
- Pojednostavljuje razvoj aplikacija za SharePoint i testiranje stvaranjem okruženju proizvodnje nalik Kopiraj replike na zahtjev za testiranje i ispravljanje pogrešaka.
- Pojednostavljuje prijelaza s oblakom pomoću oporavak web-mjesta za migraciju implementacijama sustava SharePoint u Azure.

[Dodatne informacije](https://gallery.technet.microsoft.com/SharePoint-DR-Solution-f6b4aeae) o zaštiti sustava SharePoint.


## <a name="protect-dynamics-ax"></a>Zaštita Dynamics AX za Outlook

Oporavak Azure web-mjesta štiti rješenje Dynamics AX ERP po:

- Orchestrating replikacije cijelu Dynamics AX okruženja (Web- a AOS razine, razine baze podataka, SharePoint) Azure ili sekundarnog web-mjesta.
- Pojednostavniti migracije Dynamics AX implementacijama s oblakom (Azure).
- Pojednostavniti razvoj aplikacija za Dynamics AX i testiranje stvaranjem na kopiju radnog nalik na zahtjev, za testiranje i ispravljanje pogrešaka.

[Dodatne informacije](https://gallery.technet.microsoft.com/Dynamics-AX-DR-Solution-b2a76281) o zaštiti dinamičke AX za Outlook.

## <a name="protect-rds"></a>Zaštita ZAPISI

Servise udaljene radne površine (ZAPISI) omogućuje Infrastruktura virtualne radne površine (VDI-JA), temelji na sesiji stolna računala i aplikacije, koji omogućuje korisnicima rad s bilo kojeg mjesta. Oporavak Azure web-mjesta omogućuje sljedeće:

- Sekundarnog web-mjesta, i Udaljena aplikacije i sesija sekundarnog web-mjesta ili Azure replicirati upravljanih ili neupravljani zajedničke virtualne stolna računala.
- Evo što možete replicirati:

**ZAPISI** | **Replicirati Hyper-V VMs sekundarne web-mjesto** | **Da biste Azure replicirati VMs Hyper-V** | **VMware VMs replikaciju sekundarnu web-mjesto** | **Replicirati VMware VMs za Azure** | **Replicirati fizičke poslužitelje sekundarne web-mjesto** | **Replicirati fizičke poslužitelji za Azure**
---|---|---|---|---|---|---
**Zajedničke virtualne radne površine (nekontrolisano)** | Da | ne | Da | ne | Da | ne
**Zajedničke virtualne radne površine (upravljanih i bez UPD)** | Da | ne | Da | ne | Da | ne
**Udaljena aplikacije i sesije radne površine (bez UPD)** | Da | Da | Da | Da | Da | Da


[Dodatne informacije](https://gallery.technet.microsoft.com/Remote-Desktop-DR-Solution-bdf6ddcb) o zaštiti RDS.


## <a name="protect-exchange"></a>Zaštita sustava Exchange

Oporavak web-mjesta pomaže u zaštiti sustava Exchange, na sljedeći način:

- Small implementacije sustava Exchange, kao što su pojedinačne ili samostalne poslužitelje oporavak web-mjesta mogu replicirati i neće uspjeti putem Azure ili sekundarnog web-mjesta.
- Za veće implementacije oporavak web-mjesta integrira DAGS sustava Exchange.
- Exchange DAGs su preporučeno rješenje za oporavak Izrada sustava Exchange u tvrtki.  Tarife za oporavak oporavak web-mjesta mogu sadržavati DAGs da biste orkestrirali DAG prebacivanje na web-mjesta.


[Dodatne informacije](https://gallery.technet.microsoft.com/Exchange-DR-Solution-using-11a7dcb6) o zaštiti sustava Exchange.

## <a name="protect-sap"></a>Zaštita SAP-a

Koristite oporavak web-mjesta da biste zaštitili svoju implementaciju SAP-a na sljedeći način:

- Omogući zaštitu cijele implementacije SAP po replikaciju slojevima implementacije različite Azure ili sekundarnog web-mjesta.
- Pojednostavnite oblaka migracije pomoću oporavak web-mjesta da biste migrirali implementaciju sustava SAP-a za Azure.
- Pojednostavnite razvoj SAP i testirati stvaranjem radnih nalik kopirajte na zahtjev za testiranje i ispravljanje pogrešaka aplikacije.

[Dodatne informacije](http://aka.ms/asr-sap) o zaštiti SAP-a.

## <a name="next-steps"></a>Daljnji koraci

[Priprema za implementaciju oporavak web-mjesta](site-recovery-best-practices.md) 
