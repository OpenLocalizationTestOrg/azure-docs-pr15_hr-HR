<properties
    pageTitle="Otklanjanje poteškoća s sigurnosne kopije Azure virtualnog računala | Microsoft Azure"
    description="Otklanjanje poteškoća s sigurnosnog kopiranja i vraćanja Azure virtualnih računala"
    services="backup"
    documentationCenter=""
    authors="trinadhk"
    manager="shreeshd"
    editor=""/>

<tags
    ms.service="backup"
    ms.workload="storage-backup-recovery"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/26/2016"
    ms.author="trinadhk;jimpark;"/>


# <a name="troubleshoot-azure-virtual-machine-backup"></a>Otklanjanje poteškoća s sigurnosne kopije Azure virtualnog računala

> [AZURE.SELECTOR]
- [Oporavak servisa sigurnog](backup-azure-vms-troubleshoot.md)
- [Sigurnosno kopiranje zbirke ključeva](backup-azure-vms-troubleshoot-classic.md)

Otklonite pogreške prilikom sigurnosne kopije Azure pomoću informacije navedene u tablici u nastavku.

## <a name="discovery"></a>Otkrivanje

| Postupak stvaranja sigurnosne kopije | Detalje o pogrešci | Zaobilazno rješenje |
| -------- | -------- | -------|
| Otkrivanje | Otkrijte nove stavke – Microsoft Azure Backup naišao na i interna pogreška nije uspjelo. Pričekajte nekoliko minuta i pokušajte ponovno. | Ponovite postupak otkrivanja nakon 15 minuta.
| Otkrivanje | Otkrijte nove stavke – nije uspjelo drugi otkrivanje operacija je već u tijeku. Pričekajte da se trenutni postupak otkrivanje dovrši. | Ništa |

## <a name="register"></a>Registrirajte se
| Postupak stvaranja sigurnosne kopije | Detalje o pogrešci | Zaobilazno rješenje |
| -------- | -------- | -------|
| Registrirajte se | Broj podataka diskova priložiti virtualnog računala dopuštenog podržani – provjerite odvajanje diskova neke podatke na ovom računalu virtualne i ponovite postupak. Azure sigurnosne kopije podržava do 16 podataka diskova priložiti Azure virtualnog računala za stvaranje sigurnosne kopije | Ništa |
| Registrirajte se | Stvaranje sigurnosne kopije Microsoft Azure naišao Interna pogreška – čekanja za nekoliko minuta i pokušajte ponoviti postupak. Ako se problem nastavi pojavljivati, obratite se Microsoft Support. | Ta se pogreška zbog nekog od sljedećih konfiguracija nije podržana za VM možete dobiti na Premium LRS. <br> Premium prostora za pohranu VMs mogu se sigurnosno pomoću sigurnog servise za oporavak. [uči više](backup-introduction-to-azure-backup.md/#back-up-and-restore-premium-storage-vms) |
| Registrirajte se | Registracija nije uspjela vremenskog ograničenja postupak instalacije Agent | Provjerite podržava li verzija OS-a od virtualnog računala. |
| Registrirajte se | Naredbe izvođenja nije uspjelo – drugi postupak je u tijeku za tu stavku. Pričekajte dok se ne prethodne operacije | Ništa |
| Registrirajte se | Virtualnim strojevima pojavljuju virtualne tvrdi disk koji su pohranjenu na Premium prostora za pohranu nisu podržani za stvaranje sigurnosne kopije | Ništa |
| Registrirajte se | Agent za virtualnog računala ne postoji virtualnog računala - instalirajte potrebne stara obavezna, VM agent te ponovno pokrenite postupak. | [Dodatne informacije potražite u](#vm-agent) VM agent za instalaciju i upute za instalaciju VM agent za provjeru valjanosti. |

## <a name="backup"></a>Sigurnosno kopiranje

| Postupak stvaranja sigurnosne kopije | Detalje o pogrešci | Zaobilazno rješenje |
| -------- | -------- | -------|
| Sigurnosno kopiranje | Nije moguće komunicirati s VM agent za brze snimke stanja. Snimka VM sub zadatka isteklo. – Pogledajte vodič za otklanjanje poteškoća na način da biste riješili taj problem. | Ako postoji problem s VM Agent ili pristup mreži Azure infrastrukture blokiran na mobitel, ta se pogreška se Expression.Error. Saznajte više o [ispravljanje pogrešaka gore VM snimke problemi](backup-azure-troubleshoot-vm-backup-fails-snapshot-timeout.md). <br> Ako VM agent je uzrok problema, zatim ponovno pokrenite na VM. Ponekad je netočan VM stanje može izazvati probleme sa i ponovno pokrenuti na VM vraća ovu "odgovarajućem stanju" |
| Sigurnosno kopiranje | Sigurnosno kopiranje nije uspjelo Interna pogreška – ponovite postupak za nekoliko minuta. Ako se problem nastavi pojavljivati, obratite se Microsoft Support | Provjerite postoji li tranzitne problem u pristupa VM prostora za pohranu. Provjerite [Azure Status](https://azure.microsoft.com/en-us/status/) da biste vidjeli postoji neki problem na početak povezane s računalnim/prostora za pohranu/mreže u regiji. Provjerite je mitigated pokušaj problem sigurnosne kopije objavu. |
| Sigurnosno kopiranje | Nije moguće izvesti operaciju kao VM više ne postoji. | Sigurnosno kopiranje nije moguće izvesti kao što je izbrisana VM konfigurirana za sigurnosno kopiranje. Zaustavljanje daljnje sigurnosne kopije tako da prilikom prelaska u zaštićenom prikazu stavke, zaštićeni stavku i kliknite Zaustavi zaštitu. Podatke možete zadržati tako da odaberete mogućnost zadržati sigurnosne kopije podataka. Možete kasnije nastaviti zaštite za ovaj virtualnog računala klikom na konfiguracija zaštite iz prikaza registrirana stavki|
| Sigurnosno kopiranje | Nije uspio instalirati oporavak servisa Azure nastavak na odabranoj stavki - VM Agent je stara obavezna za proširenje servise za oporavak Azure. Agent za Azure VM instalirajte i pokrenite postupka Registracija | <ol> <li>Potvrdite ako VM agent instaliran pravilno. <li>Provjerite je li zastavicu VM config ispravno postavljen.</ol> [Dodatne informacije potražite u](#validating-vm-agent-installation) VM agent za instalaciju i upute za instalaciju VM agent za provjeru valjanosti. |
| Sigurnosno kopiranje | Naredbe izvođenja nije uspjelo – drugi postupak u tijeku je za tu stavku. Pričekajte da se na prethodni ne dovrši, a zatim ponovite | Postojeću sigurnosnu kopiju ili vraćanje zadatak za na VM sustavom, a novi zadatak se ne može pokrenuti dok se izvodi postojeći posao. |
| Sigurnosno kopiranje | Proširenje instalacija nije uspjela zbog pogreške "COM + nije uspio Razgovarajte s koordinatorom Microsoft Distributed | To najčešće znači da je servis COM + nije pokrenut. Pomoć za rješavanje taj problem, obratite se Microsoftovoj podršci. |
| Sigurnosno kopiranje | Snimka operacija nije uspjela s pogreškom operacija VSS "pogonu je zaključan BitLocker šifriranja pogona. Morate otključati pogon putem upravljačke ploče. | BitLocker isključiti za sve jedinice na na VM i pridržavajte se VSS problem riješen |
| Sigurnosno kopiranje | Virtualnim strojevima pojavljuju virtualne tvrdi disk koji su pohranjenu na Premium prostora za pohranu nisu podržani za stvaranje sigurnosne kopije | Ništa |
| Sigurnosno kopiranje | Azure virtualnog računala nije pronađen. | To se događa kada se izbrisati primarni VM, ali sigurnosne kopije pravila i dalje da biste potražili VM za sigurnosno kopiranje. Da biste ispravili tu pogrešku: <ol><li>Ponovno stvorite virtualnog računala s istim nazivom i isti naziv grupe resursa [naziv oblaka usluge] <br>(ILI) <li> Zaštita za ovaj VM onemogućiti tako da se kasnije sigurnosne kopije će se pokrene. </ol> |
| Sigurnosno kopiranje | Agent za virtualnog računala ne postoji virtualnog računala - instalirajte potrebne stara obavezna, VM agent te ponovno pokrenite postupak. | [Dodatne informacije potražite u](#vm-agent) VM agent za instalaciju i upute za instalaciju VM agent za provjeru valjanosti. |

## <a name="jobs"></a>Zadaci
| Postupak | Detalje o pogrešci | Zaobilazno rješenje |
| -------- | -------- | -------|
| Otkazivanje posla | Otkazivanje nije podržana za tu vrstu posla – Pričekajte da se Završi zadatak. | Ništa |
| Otkazivanje posla | Posao nije započeo stanje – Pričekajte da se Završi zadatak. <br>OR<br> Odabranog posla nije započeo stanje – pričekajte dovršetak posla.| U svim vjerojatnost posao gotovo dovršetka; Pričekajte da se dovrši posao |
| Otkazivanje posla | Ne možete poništiti posao jer nije u tijeku - otkazivanja podržano je samo za zadatke koji su u tijeku. Ponovno pokušaj otkazivanje na u tijeku zadatka. | To se događa zbog transitory stanje. Pričekajte nekoliko minuta, a zatim ponovite postupak za otkazivanje |
| Otkazivanje posla | Nije moguće otkazati posao – Pričekajte da se Završi zadatak. | Ništa |


## <a name="restore"></a>Vraćanje
| Postupak | Detalje o pogrešci | Zaobilazno rješenje |
| -------- | -------- | -------|
| Vraćanje | Vraćanje nije uspjelo oblaka Interna pogreška | <ol><li>Servis u oblaku na koji želite vratiti konfiguriran postavke DNS-a. Možete provjeriti <br>$deployment = get-AzureDeployment - naziv servisa "naziv servisa"-vremensko razdoblje "Radni" Get-AzureDns - DnsSettings $deployment. DnsSettings<br>Ako postoji adresu, to znači da su konfigurirane postavke DNS-a.<br> <li>ReservedIP konfiguriran u oblaku na koju se želite vratiti, a postojeće VMs u oblaku su u prestao stanju.<br>Možete provjeriti na neki servis u oblaku rezervira IP pomoću sljedećih cmdleta ljuske powershell:<br>$deployment = get-AzureDeployment - naziv servisa "naziv servisa"-vremensko razdoblje "Radni" $dep. ReservedIPName <br><li>Pokušavate da biste vratili virtualnog računala pomoću sljedećih konfiguracija posebnim mrežnim u istom servis u oblaku. <br>-Virtualnim strojevima u odjeljku konfiguracije raspoređivača opterećenja (interno i vanjske)<br>-Virtualnim strojevima s više rezerviranih IP-ovi<br>-Virtualnim strojevima s više NIC-ovi<br>Odaberite novi servis u oblaku u korisničkom Sučelju ili pogledajte da biste [vratili pitanja vezana uz](./backup-azure-restore-vms.md/#restoring-vms-with-special-network-configurations) za VMs s posebnim mrežnim konfiguracijama</ol> |
| Vraćanje | Odabrani DNS naziv već preuzeli - Navedite neki drugi naziv DNS-a i pokušajte ponovno. | Ovdje naziv DNS upućuje na naziv oblaka usluge (obično završavaju. cloudapp.net). To mora biti jedinstvena. Ako se pojavi Ova pogreška, morate odabrati neki drugi naziv VM tijekom vraćanja. <br><br> Ta se pogreška prikazuje se samo na korisnike portala za Azure. Postupak vraćanja putem komponente PowerShell uspijeva jer on samo vraća na diskova i neće stvoriti na VM. Pogreška će biti prečica kada se VM je izričito koje ste načinili vi nakon postupak vraćanja na disku. |
| Vraćanje | Konfiguracija navedeni virtualne mreže nije ispravna - navedite konfiguracija za različite virtualne mreže i pokušajte ponovno. | Ništa |
| Vraćanje | Navedeni oblaku koristi Rezervirana IP koji se ne podudaraju s konfiguracijom virtualnog računala vraćaju – provjerite navedite servis za različite oblak koji ne koristi Rezervirana IP ili odabrati drugu točku oporavak da biste vratili iz. | Ništa |
| Vraćanje | Oblak servis dostigao je ograničenje broja unos krajnje točke - ponovite postupak navođenjem na drugu u oblaku, ili pomoću postojeće krajnjoj točki. | Ništa |
| Vraćanje | Sigurnosno kopiranje zbirke ključeva i ciljne prostora za pohranu računa u dvije različite regije - provjerite je li na račun za pohranu koji je naveden u postupak vraćanja na istom Azure području kao sigurnosnu kopiju zbirke ključeva. | Ništa |
| Vraćanje | Račun za pohranu naveden za postupak vraćanja nije podržan – samo Basic/standardna prostora za pohranu računa s lokalno suvišnih ili postavke suvišnih replikacije zemlj podržane. Odaberite račun podržani prostora za pohranu | Ništa |
| Vraćanje | Vrsta računa za pohranu za postupak vraćanja nije u mrežnom načinu – provjerite nalazi li se na račun za pohranu koji je naveden u postupak vraćanja online | To se može dogoditi zbog tranzitne pogreške u spremište Azure ili se prekida. Odaberite neki drugi račun za pohranu. |
| Vraćanje | Do kvote za grupu resursa – izbrišite neke grupe resursa s portala za Azure ili kontakt Azure podršku da biste povećali ograničenja. | Ništa |
| Vraćanje | Odabrani podmreže ne postoji – odaberite podmreže koji postoji | Ništa |


## <a name="policy"></a>Pravila
| Postupak | Detalje o pogrešci | Zaobilazno rješenje |
| -------- | -------- | -------|
| Stvaranje pravila | Nije uspjelo stvaranje pravila - smanjite zadržavanja mogućnosti da biste nastavili s konfiguracija pravila. | Ništa |


## <a name="vm-agent"></a>Agent za VM

### <a name="setting-up-the-vm-agent"></a>Postavljanje VM Agent
Obično VM Agent već nalazi u VMs koje su stvorene iz galerije Azure. Međutim, virtualnim strojevima migriraju se od lokalnog podatkovnim centrima želite imati instaliran Agent VM. Za pretraživanje kao VMs VM Agent potrebno je instalirati izričito. Saznajte više o [instaliranju agent VM na postojeće VM](http://blogs.msdn.com/b/mast/archive/2014/04/08/install-the-vm-agent-on-an-existing-azure-vm.aspx).

Za Windows VMs:

- Preuzmite i instalirajte [agent MSI](http://go.microsoft.com/fwlink/?LinkID=394789&clcid=0x409). Trebat će vam administratorske ovlasti da biste dovršili instalaciju.
- Određivanje je li instaliran agenta [ažurirali svojstvo VM](http://blogs.msdn.com/b/mast/archive/2014/04/08/install-the-vm-agent-on-an-existing-azure-vm.aspx) .

Za Linux VMs:

- Instalirajte najnovije [Linux agent](https://github.com/Azure/WALinuxAgent) iz github.
- Određivanje je li instaliran agenta [ažurirali svojstvo VM](http://blogs.msdn.com/b/mast/archive/2014/04/08/install-the-vm-agent-on-an-existing-azure-vm.aspx) .


### <a name="updating-the-vm-agent"></a>Ažuriranje VM Agent
Za Windows VMs:

- Ažuriranje VM Agent jednostavan je ponovno instalirati program [VM Agent binarne datoteke](http://go.microsoft.com/fwlink/?LinkID=394789&clcid=0x409). Međutim, morate da biste bili sigurni da nema sigurnosne kopije postupak je u tijeku dok VM Agent ažurira.

Za Linux VMs:

- Slijedite upute na [Ažuriranje Agent VM Linux](../virtual-machines/virtual-machines-linux-update-agent.md).


### <a name="validating-vm-agent-installation"></a>Provjera valjanosti instalacije VM Agent
Kako potražiti verziju VM Agent na Windows VMs:

1. Prijavite se na Azure virtualnog računala, a zatim otvorite mapu *C:\WindowsAzure\Packages*. Trebali biste pronađete datoteku WaAppAgent.exe prezentacija.
2. Desnom tipkom miša kliknite datoteku, idite na **Svojstva**, a zatim odaberite karticu **Detalji** . Polje verziju proizvoda mora biti 2.6.1198.718 ili noviji





