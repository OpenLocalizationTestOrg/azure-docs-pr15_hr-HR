<properties
    pageTitle="Migriranje Azure IaaS virtualnim strojevima iz jednog Azure regije u drugu pomoću oporavak web-mjesta | Microsoft Azure"
    description="Migrirajte Azure IaaS virtualnim strojevima iz jednog Azure regije u drugu pomoću oporavak Azure web-mjesta."
    services="site-recovery"
    documentationCenter=""
    authors="rayne-wiselman"
    manager="jwhit"
    editor="tysonn"/>

<tags
    ms.service="site-recovery"
    ms.workload="backup-recovery"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/21/2016"
    ms.author="raynew"/>

#  <a name="migrate-azure-iaas-virtual-machines-between-azure-regions-with-azure-site-recovery"></a>Migriranje Azure IaaS virtualnim strojevima među Azure područjima s oporavak Azure web-mjesta

## <a name="overview"></a>Pregled

Dobro došli u oporavak Azure web-mjesta! U ovom se članku koristite ako želite migrirati Azure VMs između Azure područja. Prije nego što počnete, imajte na umu da:

- Azure sadrži dva različitoj implementaciji modela za stvaranje i rad s resursima: Voditelj resursa Azure i classic. Azure ima dvije portalima – Azure klasični portala koji podržava model klasični implementacije i portalu Azure s podrškom za oba modele implementacije. Osnovni koraci za migraciju isti su hoće li se konfiguriranje oporavak web-mjesta u upravitelju resursa ili klasičnom. No upute korisničkog Sučelja i snimke zaslona u ovom članku relevantne za Azure portal.
- **Trenutno možete samo migrirati iz jednog područja na drugo. Uspijeva putem VMs iz jednog Azure regije u drugu, ali vam ne može se neće ih ponovno ponovno.**
- Upute za migraciju u ovom članku temelje se na upute za replikaciju fizičke stroj za Azure. Obuhvaća veze na korake u [replicirati VMs VMware ili fizičke poslužitelji za Azure](site-recovery-vmware-to-azure.md), koji opisuje kako replicirati fizičke server Azure portalu.
- Ako postavljate oporavak web-mjesta na portalu klasični, slijedite detaljne upute u [ovom članku](site-recovery-vmware-to-azure-classic.md). **Više ne trebate koristiti** upute u ovom [članku naslijeđene](site-recovery-vmware-to-azure-classic-legacy.md).

Pri dnu ovog članka ili na [Forum servise za oporavak Azure](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr)objavljuju komentare ni pitanja.


## <a name="prerequisites"></a>Preduvjeti

Evo što vam je potrebno za ovaj implementacije:

- **Poslužitelj za konfiguraciju**: na lokalni VM izvodi Windows Server 2012 R2 koji funkcionira kao poslužitelj za konfiguraciju. Instalacije druge oporavak web-mjesta komponente (uključujući poslužitelj za postupak i osnovne odredišni poslužitelj) na ovom VM previše. Dodatne informacije u [slučaju arhitektura](site-recovery-vmware-to-azure.md#scenario-architecture) i [preduvjeti za konfiguraciju poslužitelja](site-recovery-vmware-to-azure.md#configuration-server-prerequisites).
- **IaaS virtualnim strojevima**: U VMs želite migrirati. Migrirati te VMs tako da ih tretira kao fizičke strojeva.

## <a name="deployment-steps"></a>Koraci za implementaciju

U ovom se odjeljku opisuju koraci implementacije na novom portalu Azure. Ako vam je potrebna korake za implementaciju za oporavak web-mjesta na portalu klasični, pogledajte [u ovom se članku](site-recovery-vmware-to-azure-classic.md).

1. [Stvaranje na zbirke ključeva](site-recovery-vmware-to-azure.md#create-a-recovery-services-vault).
2. [Uvođenje poslužitelja za konfiguraciju](site-recovery-vmware-to-azure.md#step-2-set-up-the-source-environment).
3. Nakon što ste implementiran poslužitelj za konfiguraciju, provjerite mogu li komunicirati s VMs koju želite migrirati.
4. [Postavljanje ponavljanja postavke](site-recovery-vmware-to-azure.md#step-4-set-up-replication-settings). Stvaranje pravila za replikaciju i dodijelite poslužitelj za konfiguraciju.
5. [Instalirajte servis za mobilnost](site-recovery-vmware-to-azure.md#step-6-replication-application). Svaki VM koji želite zaštititi mora usluga mobilnost instalirana. Taj servis šalje podatke na poslužitelj za postupak. Servis za mobilnost mogu ručno instalirati ili pomiču i automatski instalirao poslužitelj za postupak kada je omogućen zaštitu na VM. Pravila vatrozida na VMs koju želite migrirati mora biti konfigurirano tako da omogućuje automatske instalacije tog servisa.
6. [Omogućiti replikaciju](site-recovery-vmware-to-azure.md#enable-replication). Omogućivanje replikacije za VMs želite migrirati. Možete otkriti virtualnim strojevima IaaS koju želite migrirati Azure privatne IP adrese virtualnih računala. Pronađite tu adresu na nadzornoj ploči za virtualnog računala u Azure. Kada omogućite replikacije, postaviti vrstu računala za na VMs kao fizičke strojeva.
7. [Pokretanje neplanirano prebacivanje](site-recovery-failover.md#run-an-unplanned-failover). Po dovršetku početne replikacije možete pokrenuti neplanirano prebacivanje iz određenu regiju Azure na drugi. Po želji možete stvoriti plan za oporavak i pokrenuti neplanirano prebacivanje, migriranje više virtualnim strojevima među područjima. [Saznajte više](site-recovery-create-recovery-plans.md) o tarifama za oporavak.

## <a name="next-steps"></a>Daljnji koraci

Dodatne informacije o drugim situacijama replikacije u [oporavak web-mjesta Azure?](site-recovery-overview.md)
