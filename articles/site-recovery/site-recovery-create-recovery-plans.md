<properties
    pageTitle="Stvaranje tarife za oporavak | Microsoft Azure" 
    description="Stvaranje tarife za oporavak s Azure oporavak web-mjesta neće uspjeti iznad i oporavak grupa virtualnim strojevima i fizičke poslužiteljima." 
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
    ms.workload="storage-backup-recovery" 
    ms.date="10/05/2016"
    ms.author="raynew"/>

# <a name="create-recovery-plans"></a>Stvaranje tarife za oporavak

Oporavak Azure web-mjesta servisa doprinosa tvrtke continuity i Izrada oporavak (BCDR) strategije po orchestrating replikacije, prebacivanje i oporavak virtualnim strojevima i fizičke poslužiteljima. Moguće je replicirati strojeva Azure ili sekundarni lokalnog podatkovnog centra. Kratak pregled pročitajte [oporavak web-mjesta Azure?](site-recovery-overview.md).


## <a name="overview"></a>Pregled

Ovaj članak sadrži informacije o stvaranju i prilagodbi tarife za oporavak. 

Tarife za oporavak sastoje se od jedne ili više uređeni grupa koje sadrže virtualnim strojevima ili fizičke poslužitelje koje želite zajedno uspjeti putem. Tarife za oporavak učinite sljedeće:

- Definiranje grupa za strojevima koji se neće uspjeti iznad, a zatim Pokreni zajedno.
- Modela međuzavisnosti strojeva tako da ih grupirate zajedno u grupu plan za oporavak. Za primjer ako želite neće uspjeti iznad te otvorite određenu aplikaciju koju želite grupe virtualnim strojevima za tu aplikaciju u istoj grupi plan za oporavak.
- Automatizacija i proširivanje prebacivanje. Možete pokrenuti test, planirano, ili neplanirano prebacivanje na tarifu za oporavak. Možete prilagoditi tarife za oporavak skripte, Azure Automatizacija i ručno akcije.

Tarife za oporavak prikazuju se na **Tarife za oporavak** na portalu za oporavak web-mjesta.


Pri dnu ovog članka ili na [Forum servise za oporavak Azure](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr)objavljuju komentare ni pitanja.

## <a name="before-you-start"></a>Prije početka

Imajte na umu sljedeće:

- Plan za oporavak bi trebalo miješati VMs s jednostrukih i višestrukih mrežnih prilagodnika. To je zato Miješanje te VMs nije dopuštena za Azure oblaku.
- Možete proširiti tarife za oporavak s skripte i ručno akcije. Imajte na umu sljedeće:
    - Pisanje skripti pomoću komponente Windows PowerShell.
    - Provjerite je li skripte da koristite pokušajte zamka blokovi tako da iznimke rukuje se bez poteškoća. Ako postoji iznimku u skripti prestati izvoditi i zadatak prikazuje kao nije uspjelo.  Ako dođe do pogreške, sve preostale dio skripte neće se pokrenuti. Ako se to dogodi dok se izvodi neplanirano prebacivanje, nastavit će plan za oporavak. U tom slučaju kad se izvode planiranog prebacivanje prekinut će se plan za oporavak. U tom slučaju skripta za rješavanje, provjerite je li pokreće očekivani, a zatim ponovno pokrenite plan za oporavak.
    - Naredba pisanje Host ne funkcionira u skriptu plan oporavak, a skriptu neće uspjeti. Ako želite stvoriti izlaz stvoriti skriptu proxy pokrenute shodno glavni skripte i bili sigurni da su sve izlazne piped odgovor pomoću na >> naredbe.
    - Skripta ističe hoće li se prikazivati 600 sekundi.
    - Ako ništa zapisuje na STDERR, skripta će se klasificirati kao nije uspjelo. Ti podaci će se prikazati detalje izvođenja skripte.
    - Ako koristite VMM u implementaciji sustava Imajte na umu da:

        - Skripte u planu za oporavak pokreću se u kontekstu račun servisa VMM. Provjerite je li taj račun ima dozvole za čitanje na udaljenom zajedničko korištenje na kojem se nalazi skripta pa Testirajte skriptu da biste pokrenuli ovlasti razini VMM servisa račun.
        - Cmdleti za VMM isporučuju se u modula Windows PowerShell. Modul VMM Windows PowerShell instalira se prilikom instaliranja konzole za VMM. Modul za VMM možete staviti u skriptu pomoću sljedeće naredbe u skripti: Modul za uvoz-naziv virtualmachinemanager. [Pristup dodatnim informacijama](hhttps://technet.microsoft.com/library/hh875013.aspx).
        - Provjerite je li imati barem jedan poslužitelj biblioteke u implementaciji sustava VMM. Prema zadanim postavkama put zajedničko korištenje biblioteka za poslužitelj VMM nalazi lokalno na poslužitelju VMM naziva mape MSCVMMLibrary.
        - Ako je vaš put biblioteke zajedničko korištenje udaljenom (ili lokalni ali neće zajednički koristiti s MSCVMMLibrary, konfiguriranje zajednički koristi na sljedeći način (pomoću \\libserver2.contoso.com\share\ kao primjer):
            - Otvorite uređivač registra, a zatim otvorite Recovery\Registration HKEY_LOCAL_MACHINE\SOFTWARE\MICROSOFT\Azure web-mjesta.
            -  Uredite vrijednosti ScriptLibraryPath i postavite vrijednost kao \\libserver2.contoso.com\share\. navedite cijeli FQDN. Dodjelu dozvola za zajedničko korištenje lokacije.
            -  Provjerite je li testirate skripte korisničkih računa koji ima iste dozvole kao račun servisa VMM da biste bili sigurni samostalni testirano skripte pokreću se na isti način kao i oni će u tarifama za oporavak. Na poslužitelju VMM postaviti pravila izvođenja da biste zaobišli na sljedeći način:
                -  Otvorite konzolu 64-bitni Windows PowerShell pomoću dodatnim ovlastima.
                -  Vrsta: **Postavljanje-pravilnik izvođenja zaobići**. [Pristup dodatnim informacijama](https://technet.microsoft.com/library/ee176961.aspx).

## <a name="create-a-recovery-plan"></a>Stvaranje plana oporavak

Način u kojemu ćete stvoriti plan za oporavak ovisi o implementaciju sustava oporavak web-mjesta.

- **Replikaciju VMs VMware i fizičke poslužitelje**– stvaranje plana i dodajte replikacije grupe koje sadrže virtualnim strojevima i fizičke poslužitelje tarifu za oporavak.
- **Replikaciju VMs Hyper-V (u VMM oblaka)**– stvaranje plana i dodavanje zaštićeni Hyper-V virtualnim strojevima s prikazom oblaka VMM plan za oporavak.
- **Replikaciju VMs Hyper-V (ne u VMM oblaka)**– stvaranje plana i dodajte zaštićeni Hyper-V virtualnim strojevima iz grupe zaštitu tarifu za oporavak.
- **Replikacija SAN**– stvaranje plana i dodajte replikacije grupu koja sadrži virtualnim strojevima tarifu za oporavak. Odaberite grupu za replikaciju umjesto određene virtualnim strojevima jer sve virtualnim strojevima u grupi replikacije ne smije putem zajedno (prebacivanje pojavljuje se na sloj prostora za pohranu najprije).


Pažljivo osmislite oporavak na sljedeći način:

1. Kliknite karticu **Oporavak tarife** > **Stvaranje plana za oporavak**.
Unesite naziv za tarifu za oporavak i izvorišno i odredišno. Izvorni poslužitelj mora imati virtualnim strojevima omogućene za prebacivanje i oporavak.

    - Ako ste replikaciju VMM za VMM odaberite **Vrsta izvora** > **VMM**i VMM poslužitelja izvorišno i odredišno. Kliknite **Hyper-V tako** da biste vidjeli oblaka koja su konfigurirana za korištenje značajke Hyper-V replike. 
    - Ako ste replikaciju VMM za VMM SAN odaberite **Vrsta izvora** > **VMM**i VMM poslužitelja izvorišno i odredišno. Kliknite **SAN** da biste vidjeli oblaka koja su konfigurirana za replikaciju SAN.
    - Ako ste replikaciju VMM za Azure odaberite **Vrsta izvora** > **VMM**.  Odaberite izvor VMM poslužitelj i **Azure** kao cilj.
    - Ako ste replikaciju s web-mjesta Hyper-V odaberite **Vrsta izvora** > **Hyper-V web-mjesta**. Odaberite web-mjesto kao izvor i **Azure **kao cilj.
    - Ako ste replikaciju iz VMware ili fizičke lokalni poslužitelj za Azure, odaberite poslužitelj za konfiguraciju kao izvor i **Azure** kao cilj

2. U **Odaberite virtualnim strojevima** odaberite virtualnim strojevima (ili grupe replikacije) koji želite dodati u zadanu grupu (grupa 1) u planu za oporavak.

## <a name="customize-recovery-plans"></a>Prilagodba oporavak tarife

Kada dodate zaštićena virtualnim strojevima ili replikacije grupe u zadanu grupu plan oporavak i stvoriti plan moguće je prilagoditi:

- **Dodavanje nove grupe**– možete dodati dodatne oporavak plan grupe. Grupe koje dodate numeriranja redoslijedom kojim ih dodati. Možete dodati najviše sedam grupe. Možete dodati više strojeva ili replikacije grupe te nove grupe. Imajte na umu da virtualnog računala ili grupu za replikaciju samo mogu uključiti u jedan oporavak plan grupe.
- **Dodavanje skripte **– možete dodati skripte koje prije ili nakon na oporavak planiranje grupe. Kada dodate skriptu dodaje se novi skup akcija za grupu. Na primjer skup stara korake za grupu 1 stvorit će se s nazivom: grupe 1: prije koraka. Svi koraci stara će se prikazati unutar postavljeno. Imajte na umu da možete dodati samo skripte na web-mjestu primarni ako imate VMM poslužitelj implementiran.
- **Dodajte ručni akciju**– možete dodati ručno postupaka koji se pokreću prije ili nakon grupu plan za oporavak. Kada se pokrene plan za oporavak, tabulatora točke na kojoj ste umetnuli akciju ručno i dijaloški okvir od vas traži da biste odredili da ručno akcija dovršena.

## <a name="extend-recovery-plans-with-scripts"></a>Proširivanje tarife za oporavak skripti

Možete dodati skriptu tarifu za oporavak:

- Ako imate u VMM izvorišnog web-mjesta možete stvoriti skriptu na poslužitelju VMM i uključiti u vaša tarifa za oporavak.
- Ako ste replikaciju za Azure runbooks Azure Automatizacija možete integrirati u oporavak tarifa

### <a name="create-a-vmm-script"></a>Stvaranje VMM skripte


Stvaranje skripte na sljedeći način:

1. Stvaranje nove mape u biblioteci zajedničkog mrežnog resursa, primjerice \<VMMServerName > \MSSCVMMLibrary\RPScripts. Stavljanje izvora i ciljnog VMM poslužiteljima.
2. Stvaranje skripte (na primjer RPScript), a provjeriti funkcionira prema očekivanjima.
3. Postavite skripte na mjesto \<VMMServerName > \MSSCVMMLibrary na poslužiteljima VMM izvorišno i odredišno.

### <a name="create-an-azure-automation-runbook"></a>Stvaranje programa Azure Automatizacija runbook

Vaša tarifa za oporavak možete proširiti tako da pokrenete programa Azure Automatizacija runbook kao dio plan. [Dodatne informacije potražite u](site-recovery-runbook-automation.md).


### <a name="add-custom-settings-to-a-recovery-plan"></a>Dodavanje prilagođene postavke na tarifu za oporavak

1. Otvorite oporavak plan koji želite prilagoditi.
2. Kliknite da biste dodali virtualnim strojevima ili novu grupu.
3. Da biste dodali skripte ili ručno akcije kliknite bilo koju stavku na popisu **korak** , a zatim kliknite **skripte** ili **Ručno akcija**. Odredite želite li dodati skriptu ili akciju prije ili nakon odabrane stavke. Koristite naredbeni gumbi **Premjesti gore** i **Premjesti dolje** da biste premjestili skriptu prema gore ili dolje.
4. Ako dodate skriptu VMM, odaberite **Prebacivanje VMM skriptu**, a u **Put skripte** upišite relativni put za zajedničko korištenje. Tako, za našeg primjera u kojem se nalazi na zajedničko korištenje \\ <VMMServerName>\MSSCVMMLibrary\RPScripts, navedite put: \RPScripts\RPScript.PS1.
5. Ako dodajete Azure Automatizacija koji se pokrenuti book, navedite **Azure Automatizacija računa** u kojoj se nalazi na runbook pa odaberite odgovarajući **Azure Runbook skripte**.
5. Učinite prebacivanje tarifa za oporavak da biste bili sigurni skriptu radi prema očekivanjima.


## <a name="next-steps"></a>Daljnji koraci

Različite vrste failovers mogu se izvoditi na tarifu za oporavak, uključujući prebacivanje test da biste provjerili okruženju i planiranog ili neplanirano failovers. [Dodatne informacije](site-recovery-failover.md).


 
