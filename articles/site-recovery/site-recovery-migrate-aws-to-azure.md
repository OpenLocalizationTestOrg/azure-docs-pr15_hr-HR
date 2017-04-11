<properties
    pageTitle="Migriranje virtualnim strojevima sa sustavom Windows iz web-servisa Amazon Azure s web-mjesta oporavak | Microsoft Azure"
    description="U ovom se članku opisuje kako migrirati Windows virtualnim strojevima sa sustavom u Amazon Web Services (AWA) Azure pomoću oporavak Azure web-mjesta."
    services="site-recovery"
    documentationCenter=""
    authors="rayne-wiselman"
    manager="jwhit"
    editor=""/>

<tags
    ms.service="site-recovery"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="backup-recovery"
    ms.date="08/22/2016"
    ms.author="raynew"/>

#  <a name="migrate-windows-virtual-machines-in-amazon-web-services-aws-to-azure-with-azure-site-recovery"></a>Migriranje Windows virtualnim strojevima u Amazon Web Services (AWS) u Azure s oporavak Azure web-mjesta

## <a name="overview"></a>Pregled

Dobro došli u oporavak Azure web-mjesta. Migrirajte instance sustava Windows radi u AWS za Azure s oporavak web-mjesta pomoću ovog članka. Prije nego što počnete, imajte na umu da:

- Azure sadrži dva različitoj implementaciji modela za stvaranje i rad s resursima: Voditelj resursa Azure i classic. Azure ima dvije portalima – Azure klasični portala koji podržava model klasični implementacije i portalu Azure s podrškom za oba modele implementacije. Osnovni koraci za migraciju isti su hoće li se konfiguriranje oporavak web-mjesta u upravitelju resursa ili klasičnom. No upute korisničkog Sučelja i snimke zaslona u ovom članku relevantne za Azure portal.
- **Trenutno možete samo migrirati iz AWS za Azure. Uspijeva putem VMs iz AWS Azure, ali vam ne može se neće ih ponovno ponovno. Postoji bez replikacije tijeku.**
- Upute za migraciju u ovom članku temelje se na upute za replikaciju fizičke stroj za Azure. Obuhvaća veze na korake u [replicirati VMs VMware ili fizičke poslužitelji za Azure](site-recovery-vmware-to-azure.md), koji opisuje kako replicirati fizičke server Azure portalu.
- Ako postavljate oporavak web-mjesta na portalu klasični, slijedite detaljne upute u [ovom članku](site-recovery-vmware-to-azure-classic.md). **Više ne trebate koristiti** upute u ovom [članku naslijeđene](site-recovery-vmware-to-azure-classic-legacy.md).

Pri dnu ovog članka ili na [Forum servise za oporavak Azure](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr) objavljuju komentare ni pitanja


## <a name="prerequisites"></a>Preduvjeti

Evo što vam je potrebno za ovaj implementacije

- **Poslužitelj za konfiguraciju**: na lokalni VM izvodi Windows Server 2012 R2 koji funkcionira kao poslužitelj za konfiguraciju. Instalacije druge oporavak web-mjesta komponente (uključujući poslužitelj za postupak i osnovne odredišni poslužitelj) na ovom VM previše. Dodatne informacije u [slučaju arhitektura](site-recovery-vmware-to-azure.md#scenario-architecture) i [preduvjeti za konfiguraciju poslužitelja](site-recovery-vmware-to-azure.md#configuration-server-prerequisites).
- **Instance EC2 VM**: instance sa sustavom Windows želite migrirati.

## <a name="deployment-steps"></a>Koraci za implementaciju

U ovom se odjeljku opisuju koraci implementacije na novom portalu Azure. Ako vam je potrebna korake za implementaciju za oporavak web-mjesta na portalu klasični, pogledajte [u ovom se članku](site-recovery-vmware-to-azure-classic.md).

1. [Stvaranje na zbirke ključeva](site-recovery-vmware-to-azure.md#create-a-recovery-services-vault).
2. [Uvođenje poslužitelja za konfiguraciju](site-recovery-vmware-to-azure.md#step-2-set-up-the-source-environment).
3. Nakon što ste implementiran poslužitelj za konfiguraciju, provjeriti mogu li komunicirati s VMs koju želite migrirati.
4. [Postavljanje ponavljanja postavke](site-recovery-vmware-to-azure.md#step-4-set-up-replication-settings). Stvaranje pravila za replikaciju i dodijelite poslužitelj za konfiguraciju.
5. [Instalirajte servis za mobilnost](site-recovery-vmware-to-azure.md#step-6-replication-application). Svaki VM koji želite zaštititi mora usluga mobilnost instalirana. Taj servis šalje podatke na poslužitelj za postupak. Servis za mobilnost mogu ručno instalirati ili pomiču i automatski instalirao poslužitelj za postupak kada je omogućen zaštitu na VM. Pravila vatrozida na instance EC2 koje želite migrirati mora biti konfigurirano tako da omogućuje automatske instalacije tog servisa. Sigurnosna grupa EC2 instanci moraju se korištenje sljedećih pravila:

    ![Pravila vatrozida](./media/site-recovery-migrate-aws-to-azure/migrate-firewall.png)

6. [Omogućiti replikaciju](site-recovery-vmware-to-azure.md#enable-replication). Omogućivanje replikacije za VMs želite migrirati. Možete otkriti EC2 slučajeve korištenja privatne IP adrese, koju možete preuzeti iz konzole za EC2.
7. [Pokretanje neplanirano prebacivanje](site-recovery-failover.md#run-an-unplanned-failover). Po dovršetku početne replikacije neplanirano prebacivanje možete pokrenuti iz AWS za Azure za svaki VM. Po želji možete stvoriti plan za oporavak i pokrenuti neplanirano prebacivanje, migriranje više virtualnim strojevima iz AWS u Azure. [Saznajte više](site-recovery-create-recovery-plans.md) o tarifama za oporavak.

## <a name="next-steps"></a>Daljnji koraci

Dodatne informacije o drugim situacijama replikacije u [oporavak web-mjesta Azure?](site-recovery-overview.md)
