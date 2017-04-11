<properties 
    pageTitle="Zaštita sustava SQL Server s oporavak Izrada SQL Server i oporavak web-mjesta Azure | Microsoft Azure" 
    description="U ovom se članku opisuje kako replicirati SQL Server pomoću mogućnosti Izrada sustava SQL Server Azure oporavak web-mjesta." 
    services="site-recovery" 
    documentationCenter="" 
    authors="rayne-wiselman" 
    manager="jwhit" 
    editor=""/>

<tags 
    ms.service="site-recovery" 
    ms.workload="backup-recovery" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/04/2016" 
    ms.author="raynew"/>


# <a name="protect-sql-server-with-sql-server-disaster-recovery-and-azure-site-recovery"></a>Zaštita sustava SQL Server s oporavak Izrada SQL Server i oporavak Azure web-mjesta 


Oporavak Azure web-mjesta servisa doprinosa tvrtke continuity i Izrada oporavak (BCDR) strategije po orchestrating replikacije, prebacivanje i oporavak virtualnim strojevima i fizičke poslužiteljima. Moguće je replicirati strojeva Azure ili sekundarni lokalnog podatkovnog centra. Kratak pregled pročitajte [oporavak web-mjesta Azure?](site-recovery-overview.md).

 U ovom se članku opisuje kako zaštititi SQL Server pozadinska aplikacije pomoću kombinacije tehnologije SQL Server BCDR i oporavak Azure web-mjesta. Trebali biste dobiti dobar razumijevanje mogućnosti za oporavak Izrada SQL Server (prebacivanje Klasteriranje, grupe dostupnosti AlwaysOn, baze podataka zrcaljenja, prijavu dostave) i oporavak iz web-mjesta Azure, prije implementacije scenariji opisane u ovom članku.



## <a name="overview"></a>Pregled

Mnoge radnih opterećenja pomoću SQL Server kao temelj. Aplikacija kao što je SharePoint, Dynamics i SAP proizvod pomoću SQL Server za implementaciju podatkovne usluge.  Aplikacija implementacije sustava SQL Server u nekoliko različitih načina:

- **Samostalne SQL Server**: SQL Server i sve baze podataka koje se nalaze na jednom računalu (fizičke ili virtualne). Kada je virtualiziranom, glavno računalo Klasteriranje koristi se za lokalni visoke dostupnosti. Nema goste razinom visoke dostupnosti je implementirana.
- **SQL Server prebacivanje Klasteriranje instance (uvijek na FCI)**: čvorove dva ili više instanci SQL server s zajedničke diskova konfigurirane klasteru prebacivanje sustava Windows. Ako je neki instanci klaster prema dolje, klaster uspijeva putem sustava SQL Server u drugoj instanci. Ovaj će instalacijski program obično koristi za HA primarni web-mjesta. Zaštita ne protiv neuspjeh ili prekida u sloju zajedničkog prostora za pohranu. Zajednički se koristi diska može se implementirati pomoću ISCSI optičkog kanalu ili VHDx zajedničko korištenje.
- **SQL uvijek na dostupnost grupe**: U ovaj će instalacijski program, u zajedničkoj ništa skupine s bazama podataka SQL Server konfiguriran u grupi dostupnosti s sinkrono replikacije i automatsko prebacivanje su dvije čvorovi.

U izdanjima Enterprise, SQL Server nudi nativni Izrada oporavak tehnologije za oporavak baze podataka na udaljenom mjestu. U ovom se članku smo ćete pod utjecajem i integracija s te nativni tehnologije oporavak SQL Izrada: 

- SQL uvijek na dostupnost grupe za oporavak Izrada za SQL Server 2012 ili izdanja Enterprise 2014..
- SQL baze podataka sustava u načinu Visoki sigurnost za SQL Server Standard edition (bilo koja verzija) ili SQL Server 2008 R2.


Oporavak web-mjesta možete zaštititi SQL Server kao što je prikazano u tablici.

 |**Lokalni na lokalni** | **Lokalni za Azure** 
---|---|---
**Hyper-V** | Da | Da
**VMware** | Da | Da 
**Fizička poslužitelja** | Da | Da


## <a name="support-and-integration"></a>Podrška i integracija

Te verzije sustava SQL Server podržava scenariji u ovom članku:


- SQL Server 2014 Enterprise i Standard
- SQL Server 2012 Enterprise i Standard
- SQL Server 2008 R2 Enterprise i Standard


Oporavak web-mjesta može se integrirati s izvorni SQL Server BCDR tehnologije sažeta u tablici u nastavku omogućuju rješenja za oporavak Izrada.

**Značajka** |**Pojedinosti** | **Verzija programa SQL Server** 
---|---|---
**Grupe dostupnosti AlwaysOn** | Više instanci samostalne sustava SQL Server svaki pokrenite prebacivanje klaster koja ima više čvorove.<br/><br/>Baza podataka može biti grupiran u Prebacivanje grupe koju je moguće kopirati (zrcaljene) na SQL Server instancama tako da je potrebno bez zajedničkog prostora za pohranu.<br/><br/>Omogućuje oporavak Izrada između primarni web-mjesta i jednu ili više sekundarne web-mjesta. Dva čvorove moguće je postaviti prema gore u zajedničku ništa klaster s bazama podataka SQL Server konfiguriran u grupi dostupnosti s sinkrono replikacije i automatsko prebacivanje. | 2014 SQL Server i Enterprise edition 2012.
**Prebacivanje Klasteriranje (AlwaysOn FCI)** | SQL Server upravlja Windows prebacivanje Klasteriranje za visoke dostupnosti radnih opterećenja lokalnog sustava SQL Server.<br/><br/>Čvorovi pokrenute instance sustava SQL Server s zajedničke diskova konfigurirane klasteru prebacivanje. Ako je instanca dolje klaster ne uspije iznad drugoga.<br/><br/>Klaster ne zaštitili neuspjeh ili kvarove u zajedničkog prostora za pohranu. Zajedničke diska možete implementirati s iSCSI, kanala optičkog ili se zajednički koristi VHDXs. | Izdanja sustava SQL Server Enterprise<br/><br/>SQL Server Standard edition (ograničen na samo dva čvorove)
**Baza podataka zrcaljenje (visoka sigurnost način rada)** | Štiti jednu bazu podataka za jednu sekundarne kopiju. Dostupno u oba visoka sigurnost (sinkronizirano) i načina (asinkronog) replikacije visoke performanse. Prebacivanje klaster nisu potrebne. | SQL Server 2008 R2<br/><br/>SQL Server Enterprise sva izdanja
**Samostalne SQL Server** | SQL Server i baza podataka nalaze se na jednom poslužitelju (fizičke ili virtualne). Glavno računalo Klasteriranje koristi se za visoke dostupnosti ako je poslužitelj virtualne. Nema goste razinom visoke dostupnosti. | Enterprise ili standardno izdanje

## <a name="deployment-recommendations"></a>Preporuke za implementaciju


U ovoj su tablici navedene naš preporuke za integraciju tehnologije SQL Server BCDR s oporavak web-mjesta.

**Verzija** |**Izdanje** | **Uvođenje** | **Lokalno na lokalno** | **Lokalno za Azure** 
---|---|---|---|---
SQL Server 2014 ili 2012. | Enterprise | Prebacivanje klaster instanci | Grupe dostupnosti AlwaysOn | Grupe dostupnosti AlwaysOn
 | Enterprise | Grupe dostupnosti AlwaysOn za visoke dostupnosti | Grupe dostupnosti AlwaysOn | Grupe dostupnosti AlwaysOn
 | Standardna | Prebacivanje klaster instancu (FCI) | Oporavak replikacije s lokalnom zrcalne web-mjesta | Oporavak replikacije s lokalnom zrcalne web-mjesta
 | Enterprise ili Standard | Samostalni | Replikacija oporavak web-mjesta | Replikacija oporavak web-mjesta
SQL Server 2008 R2 | Enterprise ili Standard | Prebacivanje klaster instancu (FCI) | Oporavak replikacije s lokalnom zrcalne web-mjesta | Oporavak replikacije s lokalnom zrcalne web-mjesta
 | Enterprise ili Standard | Samostalni | Replikacija oporavak web-mjesta | Replikacija oporavak web-mjesta
SQL Server (bilo koja verzija) | Enterprise ili Standard | Prebacivanje klaster instance - DTC aplikacije | Replikacija oporavak web-mjesta | Nije podržano


## <a name="deployment-prerequisites"></a>Preduvjeti za implementaciju

Evo što vam je potrebno prije nego što pokretanje:

- Lokalnog sustava SQL Server implementacije sustava podržanu verziju sustava SQL Server. Obično trebat ćete servisa Active Directory za SQL server.
- Preduvjeti za scenarij koji želite uvesti. Preduvjeti pronaći ćete u članku svaki implementacije. Veze na te omogućuje [Pregled oporavak web-mjesta](site-recovery-overview.md).
- Ako želite da biste postavili oporavak servisu Azure, morat ćete pokrenuti alat za [Procjenu za pripremu Azure virtualnog računala](http://www.microsoft.com/download/details.aspx?id=40898) na virtualnim računalima sustava SQL Server da biste bili sigurni postanu kompatibilan s Azure i oporavak web-mjesta.


## <a name="set-up-active-directory"></a>Postavljanje servisa Active Directory

Morat ćete na web-mjestu sekundarne oporavak za SQL Server pravilno izvršavanje servisa Active Directory. Postoji nekoliko mogućnosti:

- **Mali enterprise**– ako imate mali broj aplikacije i kontroler jedne domene lokalnog web-mjesta, a želite neće uspjeti preko cijelog web-mjesta, preporučujemo da koristite repication oporavak web-mjesta za replikaciju kontrolerom domene sekundarne podatkovnim centrom ili Azure.

- **Srednja do velike enterprise**– ako imate velik broj aplikacije, koristite skup stabala u servisu Active Directory i želite da se neće ispočetka tako aplikacije ili radno opterećenje, preporučujemo da postavljate dodatnu domenu kontroler u sekundarni podatkovnog centra ili Azure. Imajte na umu da ako koristite grupe dostupnosti AlwaysOn da biste vratili na udaljenom mjestu preporučujemo postavljanje drugog kontrolera dodatnu domenu na sekundarnog web-mjesta ili Azure, da biste koristili oporavljene instancu sustava SQL Server.

Upute u ovom dokumentu presume kontroler domene je dostupan na sekundarnom mjestu. [Dodatne informacije potražite u](site-recovery-active-directory.md) o zaštiti Active Directory s oporavak web-mjesta.

## <a name="integrate-protection-with-sql-server-always-on-on-premises-to-azure"></a>Zaštita integrirati Always-On SQL Server (lokalni za Azure)


Oporavak web-mjesta kao takav podržava SQL AlwaysOn. Ako ste stvorili grupu SQL dostupnosti s Azure virtualnog računala postaviti kao "Sekundarni", a zatim koristite oporavak web-mjesta za upravljanje Prebacivanje grupe dostupnosti. 

>[AZURE.NOTE] Ova mogućnost je trenutno u pretpregledu i dostupan kada Hyper-V glavnog računala poslužitelja u primarni podatkovnog centra upravlja se u VMM oblaka i postavljanje VMware upravlja se putem [Poslužitelja za konfiguraciju](site-recovery-vmware-to-azure.md#configuration-server-prerequisites). Desnom tipkom miša sada tu mogućnost nije dostupna u novom portalu Azure.

#### <a name="prerequisites"></a>Preduvjeti

Evo što vam je potrebno da biste integrirali SQL AlwaysOn s oporavak web-mjesta:

- Programa lokalnog sustava SQL Server (samostalni poslužitelj ili prebacivanje klaster).
- Jedan ili više Azure virtualnim strojevima sa sustavom SQL Server instaliran
- Dostupnost grupu SQL postavljanje između lokalnog sustava SQL Server i SQL Server u Azure
- PowerShell remoting mora biti omogućen na računalu lokalnog sustava SQL Server. Poslužitelj VMM ili poslužitelja za konfiguraciju trebali biste moći remote PowerShell pozive na SQL Server.
- Korisnički račun treba dodati sustavu SQL Server lokalnog u te grupe korisnika SQL s najmanje sljedeće dozvole:
    - GRUPU ALTER DOSTUPNOST: dozvole za [ovdje](https://msdn.microsoft.com/library/hh231018.aspx)i [ovdje](https://msdn.microsoft.com/library/ff878601.aspx#Anchor_3)
    - Iskaz ALTER baze podataka – dozvole[ovdje](https://msdn.microsoft.com/library/ff877956.aspx#Security)
- Račun za RunAs treba stvoriti na poslužitelju VMM ili račun treba stvoriti na poslužitelj za konfiguraciju pomoću na CSPSConfigtool.exe za korisnika koji se spominju u prethodnom koraku 
- Modul za SQL PS potrebno instalirati SQL poslužitelja koji se izvodi lokalno, a zatim na Azure virtualnim strojevima
- Agent za VM mora biti instaliran virtualnim strojevima sustavom Azure
- NTAUTHORITY\System mora imati pratiti dozvole na SQL Server na virtualnim strojevima u Azure:
    - Izmjena GRUPE dostupnosti – dozvole [ovdje](https://msdn.microsoft.com/library/hh231018.aspx)i [ovdje](https://msdn.microsoft.com/library/ff878601.aspx#Anchor_3)
    - Iskaz ALTER baze podataka – dozvole [ovdje](https://msdn.microsoft.com/library/ff877956.aspx#Security)

####  <a name="step-1-add-a-sql-server"></a>Korak 1: Dodavanje SQL Server


1. Kliknite **Dodavanje SQL** da biste dodali novi SQL Server. 

    ![Dodavanje SQL](./media/site-recovery-sql/add-sql.png)

2. U odjeljku **Postavke za konfiguriranje SQL** > **naziv** navedite neslužbeni naziv da biste se pozvali na SQL Server.
3. **U SQL Server (FQDN)** navedite FQDN izvorišnog web-mjesta sustava SQL Server u koju želite dodati. U slučaju da je na prebacivanje klaster instaliran SQL Server, navedite FQDN od klaster, a ne svih čvorove klaster.  
4. U **Instancu sustava SQL Server** zadane instance odaberite ili navedite naziv prilagođene instance.
5. Na **Poslužitelju za upravljanje** odaberite VMM poslužitelja ili poslužitelj za konfiguraciju registriran u sigurnog oporavak web-mjesta. Oporavak web-mjesta koristi ovaj poslužitelj za upravljanje možete komunicirati s SQL Server.
6. U **pokreće kao račun** Davanje naziva računa RunAs koja je stvorena na navedenom poslužitelju VMM ili računa koja je stvorena na poslužitelju Configuraaaon. Taj račun se koristi za pristup sustava SQL Server, a trebali biste imati dozvole za čitanje i prebacivanje na dostupnost grupe na računalu SQL Server.

    ![Prijava za SQL](./media/site-recovery-sql/add-sql-dialog.png)

Nakon što dodate SQL Server pojavit će se na kartici **SQL poslužitelja** . 

![Popis za SQL Server](./media/site-recovery-sql/sql-server-list.png)


#### <a name="step-2-add-a-sql-availability-group"></a>Korak 2: Dodavanje grupe dostupnosti SQL

1. Nakon SQL Server računalu dodaje sljedeći je korak da biste dodali grupe dostupnosti oporavak web-mjesta. Da biste to učinili, kroz razine naniže u SQL Server dodali u prethodnom koraku pa kliknite Dodaj grupu SQL dostupnost. 

    ![Dodavanje SQL AG](./media/site-recovery-sql/add-sqlag.png)

2. Grupe dostupnosti SQL možete biti replikaciju u jednu ili više virtualnim strojevima u Azure. Prilikom dodavanja grupe dostupnosti sql ćete morati unijeti naziv i pretplatu za Azure virtualnog računala mjesto na kojem želite da se dostupnost grupu tako da se nije uspjela tijekom po oporavak web-mjesta.

    ![Dodavanje dijaloški SQL AG](./media/site-recovery-sql/add-sqlag-dialog.png)

3. U primjeru iznad će postati dostupnost grupe DG1-AG primarni na virtualnog računala SQLAGVM2 unutar pretplate DevTesting2 sustavom na prebacivanje. 

>[AZURE.NOTE] Samo dostupnost grupe koje su primarni na SQL Server u koraku gore dodali dostupnih za dodavanje oporavak web-mjesta. Ako ste unijeli na dostupnost grupe primarni sustavu SQL Server ili ako ste dodali više grupa dostupnost na SQL Server nakon dodavanja, osvježite ga pomoću mogućnosti osvježavanja dostupna na SQL Server.

#### <a name="step-3-create-a-recovery-plan"></a>Korak 3: Stvaranje plana oporavak

Sljedeći je korak da biste stvorili plan oporavak korištenje virtualnih računala i grupe dostupnosti. U koraku 1 Odaberite istom poslužitelju VMM ili poslužitelja za konfiguraciju koje ste koristili kao izvor i Microsoft Azure kao odredište.

![Stvaranje plana za oporavak](./media/site-recovery-sql/create-rp1.png)

![Stvaranje plana za oporavak](./media/site-recovery-sql/create-rp2.png)

U ovom primjeru aplikacije sustava Sharepoint sastoji se od 3 virtualnim strojevima koja koristiti grupe za dostupnosti SQL kao njegov pozadinskog. U ovaj plan oporavak smo nije odaberite oba dostupnost grupu kao i virtualnog računala koje čine aplikacije. 

Plan za oporavak možete dodatno prilagoditi premještanjem virtualnim strojevima u različitim Prebacivanje grupe da biste se redni redoslijeda prebacivanje. Grupe dostupnosti je uvijek nije uspjela tijekom prvog kao što je želite koristiti kao pozadinskom sve aplikacije. 

![Prilagodba oporavak Plan](./media/site-recovery-sql/customize-rp.png)

### <a name="step-4--fail-over"></a>Korak 4: Neće uspjeti iznad

Prebacivanje različite mogućnosti dostupne su kada granu dostupnost je dodana oporavak Plan.

Prebacivanje | Pojedinosti
--- | ---
**Planirani prebacivanje** | Planirani prebacivanje podrazumijeva na nema podataka prebacivanje gubitka. Da biste postigli SQL dostupnost grupnom dostupnost načinu najprije postavite na Synchronous, a zatim je prebacivanje aktivira da biste grupi dostupnost primarni na virtualnog računala navedene prilikom dodavanja grupe dostupnosti oporavak web-mjesta. Nakon dovršetka na prebacivanje dostupnost način postavljanja jednaku vrijednost kao i prije planiranog prebacivanje ga je pokrenula.
**Prebacivanje neplanirano** | Prebacivanje neplanirano može uzrokovati u gubitka podataka. Vrijeme pokretanje neplanirano prebacivanje neće se promijeniti način dostupnost grupe dostupnosti i it postala je primarni na virtualnog računala navedene prilikom dodavanja grupe dostupnosti oporavak web-mjesta. Nakon dovršetka neplanirano prebacivanje i lokalnog poslužitelja koja se izvodi SQL Server dostupna je ponovno, obrnuti replikacije mora biti pokrenut na grupe dostupnosti. Imajte na umu da ova akcija nije dostupna u planu oporavak i se može preuzeti na grupe dostupnosti SQL kartici SQL Server
**Prebacivanje test** | Prebacivanje test za grupe dostupnosti SQL nije podržana. Pokretanje Test prebacivanje od na oporavak Plan koji sadrži SQL grupe dostupnosti, prebacivanje će se preskočiti za grupe dostupnosti.


Razmislite o tim mogućnostima prebacivanje.

Mogućnost | Pojedinosti
--- | ---
**Mogućnost 1** | 1. izvođenje testiranja prebacivanje aplikacije i pristupne razine.<br/><br/>2. ažurirajte razina aplikacije da biste pristupili replike kopiju u načinu samo za čitanje, a izvođenje testiranja samo za čitanje aplikacije.
**Mogućnost 2** | 1. stvoriti kopiju replike instancu sustava SQL Server virtualnog računala (pomoću VMM Kloniraj za web-mjesto ili sigurnosne kopije Azure) i Premjesti u mreži test<br/><br/> 2. obavite prebacivanje test koristi plan za oporavak.

Korak 5: Nije uspjelo natrag

Ako želite da bi bili grupe dostupnosti ponovno primarni lokalnog sustava SQL Server pa to možete učiniti tako pokretanje planirano prebacivanje na oporavak Plan, a zatim smjer iz Microsoft Azure lokalnog VMM poslužitelj.

>[AZURE.NOTE] Nakon neplanirano prebacivanje obrnutim replikacije ne može se pokrenuti na grupi dostupnost da biste nastavili s replikacijom. Dok je to činite ostaje replikacije obustavljeno.



### <a name="protect-machines-without-a-vmm-server-or-a-configuration-server"></a>Zaštita računala bez VMM poslužitelja ili poslužitelj za konfiguraciju

Za okruženja u kojima ne upravlja VMM poslužitelja ili poslužitelj za konfiguraciju, Azure Automatizacija Runbooks može se koristiti za konfiguriranje skriptiranih prebacivanje SQL dostupnost grupa. U nastavku su navedeni koraci za konfiguriranje koji:

1.  Stvaranje lokalne datoteke skripte uvoza putem i dostupnosti grupu. Ovaj primjer skripte navodi put do grupe dostupnosti na Azure replike i neuspješan pokazivač na tu instancu replike. Ova skripta će se izvoditi na SQL Server replike virtualnog računala prosljeđivanjem je s nastavkom prilagođene skripte.

        Param(
        [string]$SQLAvailabilityGroupPath
        )
        import-module sqlps
        Switch-SqlAvailabilityGroup -Path $SQLAvailabilityGroupPath -AllowDataLoss -force

2.  Prenesite skriptu blob u račun za Azure prostora za pohranu. Koristite ovaj primjer:

        $context = New-AzureStorageContext -StorageAccountName "Account" -StorageAccountKey "Key"
        Set-AzureStorageBlobContent -Blob "AGFailover.ps1" -Container "script-container" -File "ScriptLocalFilePath" -context $context

3.  Stvaranje programa Azure Automatizacija runbook pozvati skripte na SQL Server replike virtualnog računala u Azure. Koristite ovaj primjer skripte da biste to učinili. [Dodatne informacije](site-recovery-runbook-automation.md) o korištenju automatizacije runbooks u tarifama za oporavak. 

        workflow SQLAvailabilityGroupFailover
        {
            param (
                [Object]$RecoveryPlanContext
            )

            $Cred = Get-AutomationPSCredential -name 'AzureCredential'
    
            #Connect to Azure
            $AzureAccount = Add-AzureAccount -Credential $Cred
            $AzureSubscriptionName = Get-AutomationVariable –Name ‘AzureSubscriptionName’
            Select-AzureSubscription -SubscriptionName $AzureSubscriptionName
    
            InLineScript
            {
            #Update the script with name of your storage account, key and blob name
            $context = New-AzureStorageContext -StorageAccountName "Account" -StorageAccountKey "Key";
            $sasuri = New-AzureStorageBlobSASToken -Container "script-container"- Blob "AGFailover.ps1" -Permission r -FullUri -Context $context;
     
            Write-output "failovertype " + $Using:RecoveryPlanContext.FailoverType;
               
            if ($Using:RecoveryPlanContext.FailoverType -eq "Test")
                {
                #Skipping TFO in this version.
                #We will update the script in a follow-up post with TFO support
                Write-output "tfo: Skipping SQL Failover";
                }
            else
                {
                Write-output "pfo/ufo";
                #Get the SQL Azure Replica VM.
                #Update the script to use the name of your VM and Cloud Service
                $VM = Get-AzureVM -Name "SQLAzureVM" -ServiceName "SQLAzureReplica";     
       
                Write-Output "Installing custom script extension"
                #Install the Custom Script Extension on teh SQL Replica VM
                Set-AzureVMExtension -ExtensionName CustomScriptExtension -VM $VM -Publisher Microsoft.Compute -Version 1.3| Update-AzureVM; 
                    
                Write-output "Starting AG Failover";
                #Execute the SQL Failover script
                #Pass the SQL AG path as the argument.
       
                $AGArgs="-SQLAvailabilityGroupPath sqlserver:\sql\sqlazureVM\default\availabilitygroups\testag";
       
                Set-AzureVMCustomScriptExtension -VM $VM -FileUri $sasuri -Run "AGFailover.ps1" -Argument $AGArgs | Update-AzureVM;
       
                Write-output "Completed AG Failover";

                }
        
            }
        }

4.  Prilikom stvaranja oporavak plan za aplikaciju dodati "unaprijed grupe i pokretanje 1" skriptiranih korak koji poziva runbook Automatizacija uvoza putem dostupnosti grupe.

## <a name="integrate-protection-with-sql-alwayson-on-premises-to-on-premises"></a>Zaštita integrirati SQL AlwaysOn (lokalni na lokalni)

Ako SQL Server koristi dostupnost grupe za visoke dostupnosti ili instancu komponente prebacivanje klaster, preporučujemo da koristite dostupnost grupe na njega oporavak. Imajte na umu da ova smjernice za aplikacije koje ne koristite raspodijeljeno transakcije.

1. [Konfiguriranje baze podataka](https://msdn.microsoft.com/library/hh213078.aspx) u dostupnost grupe.
2. Stvorite novi virtualne mreže na sekundarnog web-mjesta.
3. Postavljanje VPN-a web-mjesto između nove virtualne mreže i primarni web-mjesta.
4. Stvaranje virtualnog računala na web-mjestu oporavak i instalirati SQL Server na njemu.
5. Proširivanje postojeće grupe dostupnosti AlwaysOn za nove SQL Server virtualnog računala. Konfiguriranje ovu instancu sustava SQL Server kao programa asinkronog replike kopiju.
6. Stvorite programa ga slušatelj grupe dostupnosti ili ažurirati postojeće ga slušatelj da biste uključili asinkronog replike virtualnog računala.
7. Provjerite je li na farmi poslužitelja aplikacije postavljanje pomoću ga slušatelj. Ako je postavljanje pomoću naziv poslužitelja baze podataka, ažurirajte ga da bi sadržavao ga slušatelj tako da ne morate ponovo konfigurirati nakon na prebacivanje.

Za aplikacije koje koriste raspodijeljeno transakcije smo preporuke [Oporavak web-mjesta s replikacijom SAN](site-recovery-vmm-san.md) ili [VMWare/fizičke poslužitelja web-mjesto replikacije](site-recovery-vmware-to-vmware.md)koristite.

### <a name="recovery-plan-considerations"></a>Razmatranja plan za oporavak

1. Dodati ovaj primjer skripte VMM biblioteku na web-mjestima primarnih i sekundarnih.

        Param(
        [string]$SQLAvailabilityGroupPath
        )
        import-module sqlps
        Switch-SqlAvailabilityGroup -Path $SQLAvailabilityGroupPath -AllowDataLoss -force

2. Prilikom stvaranja oporavak plan za aplikaciju dodati "unaprijed grupe i pokretanje 1" skriptiranih korak koji poziva skripte uvoza putem dostupnosti grupe.



## <a name="protect-a-standalone-sql-server"></a>Zaštita samostalan proizvod sustava SQL Server

U ovoj konfiguraciji preporučujemo da koristite replikacije oporavak web-mjesta radi zaštite računala sustava SQL Server. Točne korake ovise SQL Server je postavljen kao virtualnog računala ili fizičke poslužitelj i tome želite li za replikaciju Azure ili sekundarni lokalnog web-mjesta. Da biste dobili upute za sve scenariji za implementaciju u [Web-mjesta oporavak pregled](site-recovery-overview.md).


## <a name="protect-a-sql-server-cluster-standard-or-2008-r2"></a>Zaštita sustava SQL Server klaster (standardno ili 2008 R2)

Klaster sa sustavom SQL Server Standard edition ili SQL Server 2008 R2 preporučujemo korištenje replikacije oporavak web-mjesta da biste zaštitili SQL Server.

### <a name="on-premises-to-on-premises"></a>Lokalni na lokalni

- Ako se aplikacija koristi raspodijeljeno transakcije preporučujemo da implementacija [Oporavak web-mjesta s replikacijom SAN](site-recovery-vmm-san.md) Hyper-V okruženje i [VMware/fizičke poslužitelja VMware](site-recovery-vmware-to-vmware.md) VMware okruženju.

- Za aplikacije koje nisu DTC pod utjecajem iznad pristup da biste oporavili klaster kao samostalni poslužitelj tako da korištenje lokalne visoka sigurnost zrcalne DB.

### <a name="on-premises-to-azure"></a>Lokalni za Azure

Oporavak web-mjesta ne podržava goste klaster podršku kada replikaciju Azure. SQL Server ne omogućuje i rješenja za oporavak Izrada najniža trošak za Standard edition. Preporučujemo da zaštita klaster lokalnog sustava SQL Server kao samostalne SQL Server i oporavak u Azure.


1. Konfiguracija instancu sustava SQL Server dodatne samostalne na lokalno mjesto.
2. Konfiguriranje ovu instancu da bi služio kao zrcalne za baze podataka koje je potrebno zaštitu. Konfiguriranje zrcaljenje u načinu Visoki sigurnost.
3.  Konfiguriranje oporavak web-mjesta na lokalno mjesto ovise o okruženju ([Hyper-V](site-recovery-hyper-v-site-to-azure.md) ili [VMware/fizičke poslužitelj](site-recovery-vmware-to-azure-classic.md).
4.  Koristite replikacije oporavak web-mjesta za replikaciju novu instancu sustava SQL Server za Azure. Je zrcalna kopija visoka sigurnost i tako bit će sinkroniziran sa primarni klaster, ali ćete je replicirati na Azure pomoću replikacije oporavak web-mjesta.

Sljedeća grafika prikazuje instalacijski program.

![Standardni klaster](./media/site-recovery-sql/BCDRStandaloneClusterLocal.png)


### <a name="failback-considerations"></a>Razmatranja Failback

Za standardne klastere SQL, failback nakon neplanirano prebacivanje će zahtijevaju SQL sigurnosnu kopiju i vratiti iz instance zrcalne izvorne klaster i ponovno uspostaviti na zrcalne.

## <a name="next-steps"></a>Daljnji koraci
[Saznajte više](site-recovery-best-practices.md) o tome kako izvući jeste li spremni za implementaciju oporavak web-mjesta.










 