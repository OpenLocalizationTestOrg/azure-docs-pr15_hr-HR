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

## <a name="backup"></a>Sigurnosno kopiranje

| Postupak stvaranja sigurnosne kopije | Detalje o pogrešci | Zaobilazno rješenje |
| -------- | -------- | -------|
|Sigurnosno kopiranje|    Nije moguće izvesti operaciju kao VM više ne postoji. -Prekini zaštitu virtualnog računala bez brisanja sigurnosne kopije podataka. Dodatne informacije na http://go.microsoft.com/fwlink/?LinkId=808124   |To se događa kada se izbrisati primarni VM, ali sigurnosne kopije pravila i dalje da biste potražili VM za sigurnosno kopiranje. Da biste ispravili tu pogrešku: <ol><li> Ponovno stvorite virtualnog računala s istim nazivom i isti naziv grupe resursa [naziv oblaka usluge]<br>(ILI)</li><li> Zaustavite zaštita virtualnog računala sa ili bez brisanje podataka iz sigurnosne kopije. [Dodatne informacije](http://go.microsoft.com/fwlink/?LinkId=808124)</li></ol>|
|Sigurnosno kopiranje|Nije moguće komunicirati s VM agent za brze snimke stanja. -VM provjerite ima li pristup Internetu. Agent za VM ažurirati i, kao što je rečeno u vodiču za otklanjanje poteškoća pri http://go.microsoft.com/fwlink/?LinkId=800034 |Ako postoji problem s VM Agent ili pristup mreži Azure infrastrukture blokiran na mobitel, ta se pogreška se Expression.Error. Saznajte više o ispravljanje pogrešaka gore VM snimke problema.<br> Ako VM agent je uzrok problema, zatim ponovno pokrenite na VM. Ponekad je netočan VM stanje može izazvati probleme sa i ponovno pokrenuti na VM vraća ovu "odgovarajućem stanju"|
|Sigurnosno kopiranje|    Oporavak services proširenje nije uspjelo. -Provjerite najnovije agent virtualnog računala postoji virtualnog računala te je pokrenut servis agenta. Ponovite postupak stvaranja sigurnosne kopije i ako ga ne uspije, obratite se Microsoftovoj podršci.|   Ta se pogreška se Expression.Error kada VM agent je istekao rok. Pogledajte odjeljak "Ažuriranje u VM Agent" u nastavku da biste ažurirali VM agent.|
|Sigurnosno kopiranje |Ne postoji virtualnog računala. -Ponovno provjerite je li taj virtualnog računala postoji ili odaberite drugi virtualnog računala. | To se događa kada se izbrisati primarni VM, ali sigurnosne kopije pravila i dalje da biste potražili VM za sigurnosno kopiranje. Da biste ispravili tu pogrešku: <ol><li> Ponovno stvorite virtualnog računala s istim nazivom i isti naziv grupe resursa [naziv oblaka usluge]<br>(ILI)<br></li><li>Zaustavite zaštita virtualnog računala bez brisanja sigurnosne kopije podataka. [Dodatne informacije](http://go.microsoft.com/fwlink/?LinkId=808124)</li></ol>|
|Sigurnosno kopiranje |Izvršavanje naredbe nije uspjelo. -U tijeku za tu stavku je drugi postupak. Pričekajte da se na prethodni ne dovrši, a zatim ponovite |Postojeću sigurnosnu kopiju ili vraćanje zadatak za na VM sustavom, a novi zadatak se ne može pokrenuti dok se izvodi postojeći posao.|
| Sigurnosno kopiranje | Kopiranje VHDs iz sigurnosne kopije sigurnog isteklo - ponovite postupak za nekoliko minuta. Ako se problem nastavi pojavljivati, obratite se Microsoft Support. | To se događa kada previše podataka za kopiranje. Provjerite koristite li manje od 16 diskova podataka. |
| Sigurnosno kopiranje | Sigurnosno kopiranje nije uspjelo Interna pogreška – ponovite postupak za nekoliko minuta. Ako se problem nastavi pojavljivati, obratite se Microsoft Support | Ta se pogreška može se 2 razloga: <ol><li> Postoji problem sa tranzitne u pristupa VM prostora za pohranu. Provjerite [Azure Status](https://azure.microsoft.com/en-us/status/) da biste vidjeli postoji neki problem na početak povezane s računalnim/prostora za pohranu/mreže u regiji. Provjerite je mitigated pokušaj problem sigurnosne kopije objavu. <li>Izvorni VM izbrisana i stoga se ne mogu preuzeti sigurnosnu kopiju. Da biste zadržali sigurnosne kopije podataka za izbrisane VM, no Zaustavi sigurnosne kopije pogreške, Ukloni zaštitu na VM, a zatim odaberite mogućnost da se podaci. Time će se zaustaviti raspored sigurnosnog kopiranja i ponavljajućeg poruke o pogreškama. |
| Sigurnosno kopiranje | Nije uspio instalirati oporavak servisa Azure nastavak na odabranoj stavki - VM Agent je stara obavezna za proširenje servise za oporavak Azure. Agent za Azure VM instalirajte i pokrenite postupka Registracija | <ol> <li>Potvrdite ako VM agent instaliran pravilno. <li>Provjerite je li zastavicu VM config ispravno postavljen.</ol> [Dodatne informacije potražite u](#validating-vm-agent-installation) VM agent za instalaciju i upute za instalaciju VM agent za provjeru valjanosti. |
| Sigurnosno kopiranje | Proširenje instalacija nije uspjela zbog pogreške "COM + nije uspio Razgovarajte s koordinatorom Microsoft Distributed | To najčešće znači da je servis COM + nije pokrenut. Pomoć za rješavanje taj problem, obratite se Microsoftovoj podršci. |
| Sigurnosno kopiranje | Snimka operacija nije uspjela s pogreškom operacija VSS "pogonu je zaključan BitLocker šifriranja pogona. Morate otključati pogon putem upravljačke ploče. | BitLocker isključiti za sve jedinice na na VM i pridržavajte se VSS problem riješen |
| Sigurnosno kopiranje | Virtualnim strojevima pojavljuju virtualne tvrdi disk koji su pohranjenu na Premium prostora za pohranu nisu podržani za stvaranje sigurnosne kopije | Ništa |
| Sigurnosno kopiranje | Azure virtualnog računala nije pronađen. | To se događa kada se izbrisati primarni VM, ali sigurnosne kopije pravila i dalje da biste potražili VM za sigurnosno kopiranje. Da biste ispravili tu pogrešku: <ol><li>Ponovno stvorite virtualnog računala s istim nazivom i isti naziv grupe resursa [naziv oblaka usluge] <br>(ILI) <li> Onemogućivanje zaštiti za ovaj VM tako da će se stvoriti sigurnosnu kopiju poslova </ol> |
| Sigurnosno kopiranje | Agent za virtualnog računala ne postoji virtualnog računala - instalirajte potrebne stara obavezna, VM agent te ponovno pokrenite postupak. | [Dodatne informacije potražite u](#vm-agent) VM agent za instalaciju i upute za instalaciju VM agent za provjeru valjanosti. |

## <a name="jobs"></a>Zadaci

| Postupak | Detalje o pogrešci | Zaobilazno rješenje |
| -------- | -------- | -------|
| Otkazivanje posla | Otkazivanje nije podržana za tu vrstu posla – Pričekajte da se Završi zadatak. | Ništa |
| Otkazivanje posla | Posao nije započeo stanje – Pričekajte da se Završi zadatak. <br>OR<br> Odabranog posla nije započeo stanje – pričekajte dovršetak posla.| U svim vjerojatnost posao gotovo dovršetka; Pričekajte da se dovrši posao |
| Otkazivanje posla | Ne možete poništiti posao jer nije u tijeku - otkazivanja podržano je samo za zadatke koji su u tijeku. Ponovno pokušaj otkazivanje na u tijeku zadatka. | To se događa zbog transitory stanje. Pričekajte nekoliko minuta, a zatim ponovite postupak za otkazivanje |
| Otkazivanje posla | Nije moguće otkazati posao – Pričekajte dok Završi zadatak. | Ništa |


## <a name="restore"></a>Vraćanje
| Postupak | Detalje o pogrešci | Zaobilazno rješenje |
| -------- | -------- | -------|
| Vraćanje | Vraćanje nije uspjelo oblaka Interna pogreška | <ol><li>Servis u oblaku na koji želite vratiti konfiguriran postavke DNS-a. Možete provjeriti <br>$deployment = get-AzureDeployment - naziv servisa "naziv servisa"-vremensko razdoblje "Radni" Get-AzureDns - DnsSettings $deployment. DnsSettings<br>Ako postoji adresu, to znači da su konfigurirane postavke DNS-a.<br> <li>ReservedIP konfiguriran u oblaku na koju se želite vratiti, a postojeće VMs u oblaku su u prestao stanju.<br>Možete provjeriti na neki servis u oblaku rezervira IP pomoću sljedećih cmdleta ljuske powershell:<br>$deployment = get-AzureDeployment - naziv servisa "naziv servisa"-vremensko razdoblje "Radni" $dep. ReservedIPName <br><li>Pokušavate da biste vratili virtualnog računala pomoću sljedećih konfiguracija posebnim mrežnim u istom servis u oblaku. <br>-Virtualnim strojevima u odjeljku konfiguracije raspoređivača opterećenja (interno i vanjske)<br>-Virtualnim strojevima s više rezerviranih IP-ovi<br>-Virtualnim strojevima s više NIC-ovi<br>Odaberite novi servis u oblaku u korisničkom Sučelju ili pogledajte da biste [vratili pitanja vezana uz](./backup-azure-arm-restore-vms.md/#restoring-vms-with-special-network-configurations) za VMs s posebnim mrežnim konfiguracijama</ol> |
| Vraćanje | Odabrani DNS naziv već preuzeli - Navedite neki drugi naziv DNS-a i pokušajte ponovno. | Ovdje naziv DNS upućuje na naziv oblaka usluge (obično završavaju. cloudapp.net). To mora biti jedinstvena. Ako se pojavi Ova pogreška, morate odabrati neki drugi naziv VM tijekom vraćanja. <br><br> Imajte na umu da ta se pogreška prikazuje se samo na korisnike portala za Azure. Postupak vraćanja putem komponente PowerShell neće uspjeti jer se samo vraća na diskova i neće stvoriti na VM. Pogreška će biti prečica kada se VM je izričito koje ste načinili vi nakon postupak vraćanja na disku. |
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

## <a name="troubleshoot-vm-snapshot-issues"></a>Otklanjanje poteškoća s VM snimke
Sigurnosno kopiranje VM ovisi o izdavanja naredba snimke u podlozi za pohranu. Nemate pristup web-mjesto za pohranu ili u okvir za kašnjenje u izvršavanje zadataka snimke uspijeva sigurnosnu kopiju. Sljedeće može uzrokovati pogreške snimku stanja zadatka.

1. Pristup mreži za pohranu blokiran pomoću NSG<br>
   Dodatno se informirajte o [omogućiti pristup mreži](backup-azure-vms-prepare.md#2-network-connectivity) pomoću ili stvaranja popisa dopuštenih stavki od IP-ovi prostora za pohranu ili preko proxy poslužitelj.
2.  VMs pomoću sigurnosnog kopiranja Sql Server konfiguriran mogu prouzročiti odgode zadatka snimke <br>
    Prema zadanim postavkama VM sigurnosno kopirajte problemi VSS potpunu sigurnosnu kopiju na Windows VMs. Na VMs koje se izvode Sql poslužitelja i Sql Server konfiguriran sigurnosne kopije, to nepravilnim odgode izvođenja snimke. Postavite sljedeći ključ registra ako imate sigurnosne kopije neuspjeha zbog problema s snimke.

    ```
    [HKEY_LOCAL_MACHINE\SOFTWARE\MICROSOFT\BCDRAGENT]
    "USEVSSCOPYBACKUP"="TRUE"
    ```
3.  VM status prijavio neispravno jer VM je isključen u RDP.  <br>
    Ako ste isključi virtualnog računala u RDP, provjerite natrag na portalu da VM status prikazuje se ispravno. Ako nije, isključite VM portalu pomoću mogućnosti 'Isključivanje' VM nadzornoj ploči.
4.  Ako se isti cloud više od četiri VM zajedničko korištenje servisa, konfigurirati više sigurnosne kopije pravila faze sigurnosne kopije vremena pa nema više od četiri sigurnosne kopije VM pokrenute u isto vrijeme. Pokušajte podjele sigurnosne kopije start vremena čas udaljenosti između pravila. 
5.  VM se izvodi pri opterećenje procesora i memorije.<br>
    Ako se izvodi virtualnog računala na visok usage(>90%) ili memorije, snimka zadatak nije u redu čekanja, odgođeno i naposljetku dobiva timed out. Pokušajte sigurnosne kopije na zahtjev u tim situacijama.

<br>

## <a name="networking"></a>Povezivanje s mrežom
Kao što su sva proširenja proširenje sigurnosne kopije potreban pristup javni internet da biste radili. Nemate pristup Internetu javno manifesta može sam brojne načine:

- Proširenje instalacije uspijeva
- Uspijeva operacije sigurnosnog kopiranja (kao što je snimka na disku)
- Prikaz statusa sigurnosne kopije postupak uspijeva

Potrebe za rješavanje javno internetske adrese sadrži je articulated [ovdje](http://blogs.msdn.com/b/mast/archive/2014/06/18/azure-vm-provisioning-stuck-on-quot-installing-extensions-on-virtual-machine-quot.aspx). Morat ćete provjeri DNS konfiguraciju za na VNET i bili sigurni da će se ji Azure moći riješiti.

Kada razlučivanje naziva pravilno završi, pristup na Azure IP-ovi i potrebno omogućiti. Da biste deblokirati pristup infrastruktura za Azure, slijedite neki od ovih koraka:

1. WhiteList Azure podatkovnog centra IP raspone.
    - Pronađite popis [Azure podatkovnog centra IP-ovi](https://www.microsoft.com/download/details.aspx?id=41653) biti whitelisted.
    - Deblokirati u IP-ovi pomoću cmdleta [New-NetRoute](https://technet.microsoft.com/library/hh826148.aspx) . Pokrenite ovaj cmdlet unutar Azure VM povećane PowerShell prozor (Pokreni kao Administrator).
    - Dodavanje pravila u NSG (Ako imate na mjestu) dopustili pristup na IP-ovi.
2. Stvorite put HTTP promet na tijek pločica
    - Ako imate neki ograničenja mreže na mjestu (mreže sigurnosne grupe, na primjer) implementirati HTTP proxy poslužitelja da biste usmjerili promet. Možete pronaći korake za implementaciju HTTP Proxy poslužitelja [u nastavku](backup-azure-vms-prepare.md#2-network-connectivity).
    - Dodavanje pravila u NSG (Ako imate na mjestu) dopustili pristup Internetu iz HTTP Proxy.

>[AZURE.NOTE] DHCP mora biti omogućen unutar goste za sigurnosno kopiranje VM IaaS rad.  Ako vam je potrebna statičke IP privatne, potrebno je konfigurirati putem platforme. Mogućnost DHCP unutar na VM lijevo želite omogućiti.
Pogledajte dodatne informacije o [postavljanju statičke IP Interna privatnim](../virtual-network/virtual-networks-reserved-private-ip.md).
