<properties
   pageTitle="Azure AD Connect sinkronizacije: raspored | Microsoft Azure"
   description="U ovoj se temi opisuje značajku ugrađenih rasporeda u Azure AD Connect sinkronizaciju."
   services="active-directory"
   documentationCenter=""
   authors="AndKjell"
   manager="femila"
   editor=""/>

<tags
   ms.service="active-directory"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="identity"
   ms.date="08/04/2016"
   ms.author="billmath"/>

# <a name="azure-ad-connect-sync-scheduler"></a>Azure AD Connect sinkronizacije: raspored
U ovoj se temi opisuju ugrađenih rasporeda u sinkronizaciji Azure AD Connect (poznatog Sinkronizacija modul).

Značajka je uvedena s Sastavi 1.1.105.0 (objavljeno veljača 2016).

## <a name="overview"></a>Pregled
Azure AD Connect sinkronizacija će se sinkronizirati promjene događa u direktoriju lokalne koristiti na raspored. Postoje dva rasporeda procesa, jednu za sinkronizaciju lozinke, a drugo za sinkronizaciju objekta i atributa i zadatke održavanja. U ovoj se temi obrađuje drugu mogućnost.

Iz prethodnih izdanja raspored za objekti i atributi koje je izvan modula za sinkroniziranje i raspored zadataka sustava Windows ili Windows zaseban korišten za pokretanje postupka sinkronizacije. Zakazivanje osvježavanja je s ugrađene 1.1 izdanja za sinkroniziranje modul i omogućiti neke prilagodbe. Novi zadani učestalosti sinkronizacije je 30 minuta.

Zakazivanje osvježavanja je zadužen za dvije stvari:

- **Sinkronizacija ciklusa**. Postupak da biste uvezli, sinkronizacija i izvoz promjene.
- **Zadatke održavanja**. Obnoviti ključeve i certifikati za ponovno postavljanje lozinke i servis za registraciju na uređaju (DRS). Čišćenje starih stavki u zapisniku operacije.

Raspored sam uvijek pokrenuti, ali ga možete konfigurirati da biste se izvoditi samo jednu ili nijedno od ovih zadataka. Na primjer ako morate imati vlastitu sinkronizacija ciklusa možete onemogućiti ovaj zadatak u alat za zakazivanje ali i dalje pokretanje zadatka održavanja.

## <a name="scheduler-configuration"></a>Konfiguriranje rasporeda
Da biste vidjeli svoje trenutne postavke konfiguracije, otvorite PowerShell i pokrenite `Get-ADSyncScheduler`. Pokazat će vam otprilike ovako:

![GetSyncScheduler](./media/active-directory-aadconnectsync-feature-scheduler/getsynccyclesettings.png)

Ako vidite **naredbu Sinkroniziraj ili cmdlet nije dostupna je** kada pokrenete ovaj cmdlet, zatim modul ljuske PowerShell neće biti učitan. To se može dogoditi ako pokrenete Azure AD Connect na kontroler domene ili na poslužitelju s više razine PowerShell ograničenje od zadane postavke. Ako se prikaže Ova pogreška, pokrenite `Import-Module ADSync` da bi cmdlet dostupnim.

- **AllowedSyncCycleInterval**. Najčešće Azure AD da bi sinkronizacija dogoditi. Češće od to ne može sinkronizirati, a i dalje biti podržano.
- **CurrentlyEffectiveSyncCycleInterval**. Raspored trenutno na snazi. Neće imati iste vrijednosti kao CustomizedSyncInterval (Ako postavite) ako ona još nije češće od AllowedSyncInterval. Ako promijenite CustomizedSyncCycleInterval, to će primjenjivati nakon idućeg kruga sinkronizacije.
- **CustomizedSyncCycleInterval**. Ako želite da se raspored za izvođenje bilo kojeg učestalost od zadane 30 minuta, će konfigurirati tu postavku. Na slici iznad alat za zakazivanje postavljena radi svaki sat. Ako ste postavili to na vrijednost manju od AllowedSyncInterval, drugu mogućnost će se koristiti.
- **NextSyncCyclePolicyType**. Delta ili inicijal. Definira sljedećeg izvođenja treba samo delta promjene procesa, ili ako sljedećeg izvođenja biste trebali učiniti potpuno uvoz i sinkronizacija, koje želite i ponovo obradi sve nove ili promijenjene pravila.
- **NextSyncCycleStartTimeInUTC**. Sljedeći put alat za zakazivanje počet će idućeg kruga sinkronizaciju.
- **PurgeRunHistoryInterval**. Vrijeme operacija zapisnicima će biti zadržane. Te se mogu pregledavati na Upravitelj sinkronizacije servisa. Zadano je da biste zadržali te sedam dana.
- **SyncCycleEnabled**. Pokazuje na raspored je li pokrenut uvoz, sinkroniziranje i procesa izvoza kao dio njegov operacije.
- **MaintenanceEnabled**. Prikazuje ako je omogućen postupak održavanja. Bit će ažuriranje certifikata/ključeva i čišćenja zapisnika operacije.
- **IsStagingModeEnabled**. Prikazuje [pripremna način](active-directory-aadconnectsync-operations.md#staging-mode) je li omogućeno.

Možete promijeniti neke od tih postavki s `Set-ADSyncScheduler`. Moguće je izmijeniti sljedećih parametara:

- CustomizedSyncCycleInterval
- NextSyncCyclePolicyType
- PurgeRunHistoryInterval
- SyncCycleEnabled
- MaintenanceEnabled

Konfiguriranje rasporeda se pohranjuju u Azure AD. Ako imate pripremna poslužitelj, bilo kakve promjene na primarnom poslužitelju i efekta pripremna server (osim IsStagingModeEnabled).

### <a name="customizedsynccycleinterval"></a>CustomizedSyncCycleInterval
Sintaksa:`Set-ADSyncScheduler -CustomizedSyncCycleInterval d.HH:mm:ss`  
d - dana, HH - h "," mm - minuta, ss - sekundi

Primjer:`Set-ADSyncScheduler -CustomizedSyncCycleInterval 03:00:00`  
Raspored da biste pokrenuli svaki 3 sata će se promijeniti.

Primjer:`Set-ADSyncScheduler -CustomizedSyncCycleInterval 1.0:0:0`  
Raspored pokreću se svakog dana će se promijeniti.

## <a name="start-the-scheduler"></a>Pokrenuti raspored
Alat za zakazivanje pokrenut će se po zadanom svakih 30 minuta. U nekim slučajevima možda želite pokrenuti sinkronizaciju ciklusa između zakazani ciklusa ili morate pokrenuti neku drugu vrstu.

**Delta sinkronizacije ciklusa**  
Sinkronizacija krugu delta obuhvaća sljedeće korake:

- Uvoz Delta na sve poveznika
- Delta Sinkroniziraj u svim poveznika
- Izvoz na sve poveznika

Možda imate programa hitno promjena koje morate sinkronizirati odmah koji je Zašto morate ručno pokrenuti krugu. Ako je potrebno ručno pokrenite ciklus, zatim iz PowerShell pokrenite `Start-ADSyncSyncCycle -PolicyType Delta`.

**Potpuna sinkronizacija ciklusa**  
Ako ste nešto od sljedećeg promjene konfiguracije, morate pokrenuti punu sinkronizaciju ciklusa (poznatog Početni):

- Dodaje više objekata ili atribute za uvoz iz izvora direktorija
- Unijeli promjene u pravila za sinkronizaciju
- Promijenio [Filtriranje](active-directory-aadconnectsync-configure-filtering.md) te stoga različit broj objekata mora biti obuhvaćene

Ako ste jednu od tih promjena, morate pokreće krugu punu sinkronizaciju da bi modula za sinkroniziranje je priliku reconsolidate poveznik razmake. Potpuna sinkronizacija ciklusa obuhvaća sljedeće korake:

- Potpuni uvoz na sve poveznika
- Potpuna sinkronizacija na sve poveznika
- Izvoz na sve poveznika

Da biste pokrenuli punu sinkronizaciju ciklusa, pokrenite `Start-ADSyncSyncCycle -PolicyType Initial` iz odzivniku komponente PowerShell. Započet će krugu punu sinkronizaciju.

## <a name="stop-the-scheduler"></a>Zaustaviti raspored
Ako alat za zakazivanje trenutno radi sinkronizacije ciklusa bilo bi dobro da biste ga zaustavili. Na primjer, ako ste pokrenuli čarobnjak za instalaciju i se sljedeća pogreška:

![SyncCycleRunningError](./media/active-directory-aadconnectsync-feature-scheduler/synccyclerunningerror.png)

Kada je pokrenut sinkronizaciju ciklusa, nećete moći unositi promjene konfiguracije. Nije moguće Pričekajte dok alat za zakazivanje završi postupak, ali možete i prekinuti ga da biste mogli dodati promjene odmah. Zaustavljanje trenutnog ciklusa nije štetnih i sve promjene i dalje nije obrađen obradit će se pomoću sljedeće cilja.

1. Najprije koja upozorava raspored da biste prestali njegov trenutni ciklus pomoću cmdleta ljuske PowerShell `Stop-ADSyncSyncCycle`.
2. Zaustavljanje alat za zakazivanje će prekinuti trenutni Connector iz njegova trenutnog zadatka. Da biste nametnuli poveznik da biste zaustavili, učiniti sljedeće: ![StopAConnector](./media/active-directory-aadconnectsync-feature-scheduler/stopaconnector.png)
    - Pokrenite **Servis za sinhronizaciju sa programom** s izbornika start. Idite na **poveznika**, isticanje poveznik sa statusom **pokrenut** i odaberite **Zaustavi** akcije.

Alat za zakazivanje uvijek aktivan i će ponovno pokrenite na sljedeći prilike.

## <a name="custom-scheduler"></a>Prilagođeni raspored
Cmdleti za navedenih u ovom odjeljku su samo dostupne u Sastavi [1.1.130.0](active-directory-aadconnect-version-history.md#111300) i noviji.

Ako ugrađenih rasporeda zadovoljavaju potrebama, možete zakazati poveznika pomoću komponente PowerShell.

### <a name="invoke-adsyncrunprofile"></a>Pozivanje ADSyncRunProfile
Profil možete pokrenuti poveznik na ovaj način:

```
Invoke-ADSyncRunProfile -ConnectorName "name of connector" -RunProfileName "name of profile"
```

Nazivi koji se koriste za [poveznik naziva](active-directory-aadconnectsync-service-manager-ui-connectors.md) i [Pokretanje naziva profil](active-directory-aadconnectsync-service-manager-ui-connectors.md#configure-run-profiles) pronaći ćete u [Upravitelj sinkronizacije servisa korisničkog Sučelja](active-directory-aadconnectsync-service-manager-ui.md).

![Pozivanje izvođenja profila](./media/active-directory-aadconnectsync-feature-scheduler/invokerunprofile.png)  

Na `Invoke-ADSyncRunProfile` cmdlet je sinkrono, odnosno ga neće vratiti kontrolu dok je poveznik je dovršena postupak, uspješno ili poruku o pogrešci.

Kada zakazujete vaše poveznika, na preporuka je da biste zakazali ih sljedećim redoslijedom:

1. (Pune na Delta) Uvoz iz lokalnog imenika, kao što je Active Directory
2. (Pune na Delta) Uvoz iz Azure AD
3. (Pune na Delta) Sinkronizacija s lokalnim direktorijima, kao što je Active Directory
4. (Pune na Delta) Sinkronizacija s Azure AD
5. Izvoz u Azure AD
6. Izvoz u lokalni direktorijima, kao što je Active Directory

Ako pogledate ugrađenih rasporeda, to je redoslijed poveznika će se pokrenuti.

### <a name="get-adsyncconnectorrunstatus"></a>Get-ADSyncConnectorRunStatus
Možete nadzirati i modul sinkronizaciju da biste vidjeli ako je zauzet ili neaktivan. Ovaj cmdlet dat će prazan rezultat ako modula za sinkroniziranje je u stanju mirovanja, a nije pokrenut poveznik. Ako je pokrenut poveznika, će vratiti naziv poveznik.

```
Get-ADSyncConnectorRunStatus
```

![Status izvođenja poveznika](./media/active-directory-aadconnectsync-feature-scheduler/getconnectorrunstatus.png)  
Na gornjoj slici, prvi redak je u stanju gdje je neaktivan modula za sinkroniziranje. Drugi redak iz kada je pokrenut poveznik za Azure AD.

## <a name="scheduler-and-installation-wizard"></a>Čarobnjak za raspored i instalacije
Ako pokrenete čarobnjak za instalaciju, zatim alat za zakazivanje će biti privremeno obustavljena. To je zato pretpostavlja se da je unesete promjene konfiguracije te ih nije moguće primijeniti ako modula za sinkroniziranje aktivno pokrenut. Zbog toga ne ostavite Čarobnjak za instalaciju Otvori jer je će se zaustaviti sinkronizaciju modula iz izvođenje radnje sinkronizacije.

## <a name="next-steps"></a>Daljnji koraci
Dodatne informacije o konfiguraciji [Azure AD Connect sinkronizirati](active-directory-aadconnectsync-whatis.md) .

Dodatne informacije o [integraciji vaših lokalnih identiteta sa servisu Azure Active Directory](active-directory-aadconnect.md).
