<properties
    pageTitle="Azure AD Connect sinkronizacije: Konfiguriranje filtriranje | Microsoft Azure"
    description="U članku se objašnjava kako konfigurirati filtriranje u Azure AD Connect sinkronizaciju."
    services="active-directory"
    documentationCenter=""
    authors="andkjell"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/13/2016"
    ms.author="andkjell;markvi"/>


# <a name="azure-ad-connect-sync-configure-filtering"></a>Azure AD Connect sinkronizacije: Konfiguriranje filtriranja
Filtriranje, možete odrediti koji objekti prikazivati Azure AD iz lokalnog imenika. Zadana konfiguracija uzima svih objekata u svim domena u konfigurirano šuma. Općenito govoreći, to je preporučena konfiguracija. Krajnji korisnici pomoću radnih opterećenja sustava Office 365, kao što je Exchange Online i Skype za tvrtke, prednosti dovršeno globalnog popisa adresa da bi mogli slati e-poštu i poziv svima. Sa zadanom konfiguracijom će dobiti isti sučelje oni bi s lokalnim implementacija sustava Exchange i Lync.

U nekim slučajevima je potrebna je za neke promjene uz zadanu konfiguraciju. Evo nekoliko primjera:

- Planirate li koristiti [više Azure AD direktorija topologije](active-directory-aadconnect-topologies.md#each-object-only-once-in-an-azure-ad-directory). Trebate primijenite filtar da biste odredili koji objekt moraju se sinkronizirati s određenom Azure AD direktorija.
- Pokretanje probnog za Azure ili Office 365, a vi želite podskup korisnika u Azure AD. U malim pilot nije važno je da bi se dovrši globalnog popisa adresa da bismo pokazali funkciju.
- Imate više računa servisa i drugih računa-osobno koje želite Azure AD.
- Usklađenost razloga ne briše sve korisnički računi lokalnog. Onemogućite samo ih. No u Azure AD želite samo aktivne korisničke račune za postojati.

U ovom se članku objašnjava kako konfigurirati različite metode filtriranja.

> [AZURE.IMPORTANT]Microsoft ne podržava izmjene ili postupak sinkronizacije Azure AD Connect izvan tih akcija formally navedenih. Neku od ovih radnji može uzrokovati u stanju koje nisu usklađene ili nepodržane Azure AD Connect sinkronizacije i zbog toga Microsoft ne pruža tehničku podršku za pretraživanje kao implementacije.

## <a name="basics-and-important-notes"></a>Osnove i važne bilješke
Tijekom sinkronizacije Azure AD Connect možete omogućiti filtriranje u bilo kojem trenutku. Ako pokrenete uz zadanu konfiguraciju sinkronizacije direktorija i konfiguriranje filtriranja, objekte koji su filtrira više će se sinkronizirati s Azure AD. Uslijed tu promjenu u Azure AD sve objekte koji su prethodno sinkronizirati, ali zatim filtrirane brišu se u Azure AD.

Prije nego što počnete upućivanje mijenja se u filtriranja, provjerite je li vam [onemogućiti zakazani zadatak](#disable-scheduled-task) tako da slučajno ne izvezete promjene koje ste nije još provjerili ispravni.

Nakon filtriranja možete ukloniti više objekata u isto vrijeme, želite provjerite jesu li nove filtre ispravni prije nego što počnete izvoz sve promjene u Azure AD. Nakon što dovršite navedeni koraci za konfiguraciju, preporučuje se slijedite [korake za potvrdu](#apply-and-verify-changes) prije izvoza i promjene Azure AD.

Zaštititi od brisanje više objekata, ponašanja, značajka [sprječava nenamjerno brisanje](active-directory-aadconnectsync-feature-prevent-accidental-deletes.md) po zadanom je uključeno. Ako ste izbrisali više objekata zbog filtriranje (500 po zadanom), morate slijedite korake u ovom članku da biste omogućili briše prođite kroz za Azure AD.

Ako koristite na Sastavi prije studenom 2015 ([1.0.9125](active-directory-aadconnect-version-history.md#1091250)), promjene konfiguracije filtar i koristite sinkronizaciju lozinke, morate pokrenuti cijelog Sinkroniziraj sve lozinki nakon što ste dovršili konfiguraciju. Upute za pokretanje punu sinkronizaciju lozinke potražite u članku [okidača cijelog Sinkroniziraj sve lozinki](active-directory-aadconnectsync-implement-password-synchronization.md#trigger-a-full-sync-of-all-passwords). Ako ste na 1.0.9125 ili noviji, zatim akciju običnog **potpune sinkronizacije** i izračunava Ako treba sinkronizaciju lozinke i u ovom dodatnog koraka više nije potrebna.

Ako **korisničke** objekte slučajno ste izbrisali Azure AD zbog filtriranja pogreške, možete ponovno stvoriti korisnički objekti u Azure AD uklanjanjem vaše filtriranja konfiguracije i ponovno sinkronizirati svoje mape. Ova akcija vraća korisnike iz koša za smeće u Azure AD. Međutim, nije moguće vratiti drugih vrsta objekta. Na primjer, ako slučajno izbrišete sigurnosne grupe i njegovo korištenje ACL resursa, grupe i njegov ACL-a nije moguć.

Azure AD Connect briše se samo na objekte sadrži jednom smatra da je u opsegu. Ako nema objekata u Azure AD koje je stvorio drugi modula za sinkroniziranje i ti objekti nisu opseg, dodavanje filtriranje nije ih uklonite. Ako, na primjer, ako počinju poslužitelj DirSync je stvorena cijelu kopiju cijelom direktoriju Azure AD i instalirate novi poslužitelj za Azure AD Connect sinkronizaciju paralelno filtriranje omogućeno od početka, ga ukloniti dodatni objekata koji je stvorio DirSync.

Filtriranje konfiguraciju zadržava se nakon instalacije ili nadogradnja na noviju verziju Azure AD Connect. Uvijek je najbolji način da biste potvrdili da konfiguraciju nije slučajno promijenjen nakon nadogradnje na noviju verziju prije pokretanja ciklusa prva sinkronizacija.

Ako imate više od jednog skupa stabala, filtriranja konfiguracije opisane u ovoj temi potrebno primijeniti na svaki skup stabala (Ako želite istu konfiguraciju za sve).

### <a name="disable-scheduled-task"></a>Onemogućivanje zakazan zadatak
Da biste onemogućili ugrađenih rasporeda koji pokreće sinkronizaciju ciklusa svakih 30 minuta, slijedite ove korake:

1. Idite na PowerShell upita.
2. Pokrenite `Set-ADSyncScheduler -SyncCycleEnabled $False` da biste onemogućit.
3. Unesite željene promjene, kao što je navedenih u nastavku.
4. Pokrenite `Set-ADSyncScheduler -SyncCycleEnabled $True` da biste ponovno omogućiti na raspored.

**Ako koristite programa Azure AD Connect Sastavi prije 1.1.105.0**  
Da biste onemogućili zakazani zadatak koji se pokreće sinkronizaciju ciklusa svaki 3 sata, slijedite ove korake:

1. Pokretanje **Rasporeda zadataka** s izbornika start.
2. Izravno u odjeljku **Biblioteka rasporeda zadataka**, pronađite zadatak pod nazivom **Azure AD sinkronizaciju rasporeda**, desnom tipkom miša, a zatim odaberite **Onemogući**.  
![Raspored zadataka](./media/active-directory-aadconnectsync-configure-filtering/taskscheduler.png)  
3. Sada možete promjene konfiguracije i ručno pokretanje modula za sinkroniziranje s konzole za **Upravitelj servisa za sinkronizaciju** .

Nakon što dovršite sve filtriranja promjene, ne zaboravite da biste došli natrag i **omogućite** na zadatak.

## <a name="filtering-options"></a>Mogućnosti filtriranja
Filtriranje konfiguracije sljedeće se mogu primijeniti na alata za sinkronizaciju direktorija:

- [**Grupa temelji**](active-directory-aadconnect-get-started-custom.md#sync-filtering-based-on-groups): filtriranje jedne grupe na temelju samo može konfigurirati na početni instalacija pomoću čarobnjaka za instalaciju. To ne dodatno opisana su u ovoj temi.

- [**Utemeljen na domenu**](#domain-based-filtering): ta mogućnost omogućuje vam da biste odabrali koje domene koji se sinkroniziraju Azure AD. Omogućuje dodavanje i uklanjanje domena iz konfiguracije modul sinkronizaciju Ako promijenite infrastrukture lokalnog nakon instalacije Azure AD Connect sinkronizaciju.

- [**Tvrtke ili ustanove jedinica – sustavom**](#organizational-unitbased-filtering): ta mogućnost filtriranja omogućuje vam da biste odabrali koji se sinkroniziraju Azure AD organizacijske jedinice. Ta je mogućnost za sve vrste objekata u odabrani organizacijske jedinice.

- [**Koji se temelji na atribut –**](#attribute-based-filtering): ta mogućnost omogućuje vam da biste filtrirali objekata na temelju vrijednosti atributa na objekte. Možete imati i različite filtre za drugi objekt vrste.

Možete koristiti više mogućnosti filtriranja u isto vrijeme. Ako, na primjer, možete koristiti utemeljen na OU filtriranje da uvrstite samo objekata u jedan OU i na isti vrijeme utemeljen na atribut filtriranje da biste filtrirali na objekte. Kada koristite više načina filtriranja, filtri pomoću logički i između filtre.

## <a name="domain-based-filtering"></a>Filtriranje utemeljen na domeni
Ovo poglavlje sadrži upute za konfiguriranje filtar za domenu. Ako imate dodati ili ukloniti domena u vašem skupa stabala nakon što ste instalirali Azure AD Connect, morate ažurirati filtriranja konfiguraciju.

Željeni način da biste promijenili utemeljen na domeni filtriranje je tako da pokrenete instalaciju te promijenite navigaciju Čarobnjak za [domene i organizacijske jedinice filtriranja](active-directory-aadconnect-get-started-custom.md#domain-and-ou-filtering). Čarobnjak za instalaciju je Automatizacija sve zadatke u ovoj temi.

Slijedite ove korake samo ako iz nekog razloga ne možete pokrenuti čarobnjak za instalaciju.

Utemeljen na domeni filtriranja konfiguracije sastoji se od ovih koraka:

- [Odaberite domene](#select-domains-to-be-synchronized) koje želite uvrstiti u sinkronizaciji.
- Za svaku dodatnu i ukloniti domenu, prilagodite [pokrenuti profila](#update-run-profiles).
- [Primijeni i provjerite je li promjene](#apply-and-verify-changes).

### <a name="select-domains-to-be-synchronized"></a>Odaberite domene za sinkronizaciju
**Da biste postavili domenu filtar, učinite sljedeće:**

1. Prijavite se na poslužitelju sa sustavom Azure AD Connect sinkronizaciju pomoću računa koji je član **ADSyncAdmins** sigurnosne grupe.
2. Pokrenite **Servis za sinkronizaciju** s izbornika start.
3. Odaberite **poveznika** , a zatim na popisu **poveznika** odaberite poveznik s vrstom **Active Directory Domain Services**. **Akcije**, odaberite **Svojstva**.  
![Svojstva poveznika](./media/active-directory-aadconnectsync-configure-filtering/connectorproperties.png)  
4. Kliknite **Konfiguriranje particije direktorija**.
5. Na popisu **Odaberite direktorija particije** odaberite, a zatim po potrebi poništite odabir domene. Provjerite je li odabrano samo particije koje želite sinkronizirati.  
![Particije](./media/active-directory-aadconnectsync-configure-filtering/connectorpartitions.png)  
Ako ste promijenili lokalnih AD Infrastruktura i domena dodani ili uklonjeni iz skupa stabala, zatim kliknite gumb **Osvježi** da biste dobili ažurirani popis. Kada osvježite, od vas će se zatražiti vjerodajnice. Navedite vjerodajnice s pristupom čitanja lokalni Active Directory. Ne sadrži biti korisnika koji je unaprijed unesene u dijaloškom okviru.  
![Potrebno je osvježavanje](./media/active-directory-aadconnectsync-configure-filtering/refreshneeded.png)  
6. Kada završite, zatvorite dijaloški okvir **Svojstva** klikom na **u redu**. Ako uklonite domene iz sustava skupa stabala skočni prozor poruke koja kaže da je uklonjena domene i taj konfiguracije će se očistiti.
7. I dalje da biste prilagodili [pokrenuti profila](#update-run-profiles).

### <a name="update-run-profiles"></a>Ažuriranje profila za pokretanje
Ako ste ažurirali filtar za domenu, morate ažurirati izvođenja profila.

1. Na popisu **poveznika** obavezno poveznik koji ste promijenili u prethodnom koraku. **Akcije**, odaberite **Konfigurirati pokretanje profila**.  
![Poveznik za pokretanje profila](./media/active-directory-aadconnectsync-configure-filtering/connectorrunprofiles1.png)  

Morate prilagoditi sljedeće profila:

- Potpuni uvoz
- Potpune sinkronizacije
- Uvoz Delta
- Delta sinkronizacije
- Izvoz

Za svaku od pet profila, provedite sljedeće korake za svaku domenu **dodati** :

1. Odaberite Pokreni profil, a zatim kliknite **Novi korak**.
2. Na stranici **Korak konfiguriranje** u **vrstu** padajućem popisu, odaberite vrstu korak s istim nazivom kao profil konfigurirate. Zatim kliknite **Dalje**.  
![Poveznik za pokretanje profila](./media/active-directory-aadconnectsync-configure-filtering/runprofilesnewstep1.png)  
3. Na stranici **Konfiguracija Connector** **particija** padajućem popisu, odaberite naziv domene koji ste dodali u filtar za domenu.  
![Poveznik za pokretanje profila](./media/active-directory-aadconnectsync-configure-filtering/runprofilesnewstep2.png)  
4. Da biste zatvorili dijaloški okvir **Konfiguriranje pokrenuti profil** , kliknite **Završi**.

Za svaku od pet profila, provedite sljedeće korake za svaku domenu **ukloniti** :

1. Odaberite Pokreni profil.
2. Ako je **vrijednost** atributa **particije** GUID, odaberite Pokreni korak, a zatim kliknite **Izbriši korak**.  
![Poveznik za pokretanje profila](./media/active-directory-aadconnectsync-configure-filtering/runprofilesdeletestep.png)  

Rezultat mora biti sve domene koje želite sinkronizirati mora biti navedena kao koraka u svakom izvođenja profilu.

Da biste zatvorili dijaloški okvir **Konfiguriranje profila za pokretanje** , kliknite **u redu**.

- Da biste dovršili konfiguraciju, [Primijeni i provjerite je li promjene](#apply-and-verify-changes).

## <a name="organizational-unitbased-filtering"></a>Tvrtke ili ustanove jedinica – sustavom filtriranja
Željeni način da biste promijenili utemeljen na OU filtriranje je tako da pokrenete instalaciju te promijenite navigaciju Čarobnjak za [domene i organizacijske jedinice filtriranja](active-directory-aadconnect-get-started-custom.md#domain-and-ou-filtering). Čarobnjak za instalaciju je Automatizacija sve zadatke u ovoj temi.

Slijedite ove korake samo ako iz nekog razloga ne možete pokrenuti čarobnjak za instalaciju.

**Da biste konfigurirali Organizacijska jedinica – sustavom filtriranja, učinite sljedeće:**

1. Prijavite se na poslužitelju sa sustavom Azure AD Connect sinkronizaciju pomoću računa koji je član **ADSyncAdmins** sigurnosne grupe.
2. Pokrenite **Servis za sinkronizaciju** s izbornika start.
3. Odaberite **poveznika** , a zatim na popisu **poveznika** odaberite poveznik s vrstom **Active Directory Domain Services**. **Akcije**, odaberite **Svojstva**.  
![Svojstva poveznika](./media/active-directory-aadconnectsync-configure-filtering/connectorproperties.png)  
4. Kliknite **Konfiguriranje particije direktorija**, odaberite domenu koju želite konfigurirati, a zatim **spremnika**.
5. Kada se to od vas zatraži, unesite sve vjerodajnice s pristupom čitanja lokalni Active Directory. Ne sadrži biti korisnika koji je unaprijed unesene u dijaloškom okviru.
6. U dijaloškom okviru **Odaberite spremnika** poništite organizacijske jedinice koje ne želite sinkronizirati s direktorijem oblaka, a zatim kliknite **u redu**.  
![ORGANIZACIJSKA JEDINICA](./media/active-directory-aadconnectsync-configure-filtering/ou.png)  
  - Spremnik **računala** mora biti označena za računala za Windows 10 da biste uspješno sinkronizirati Azure AD. Ako pridruženo domene računala nalaze u drugim organizacijske jedinice, provjerite je li one koji su potvrđeni.
  - Spremnik **ForeignSecurityPrincipals** mora biti označena ako imate više šuma s trusts. Ovaj spremnik omogućuje unakrsno-skupa stabala sigurnost članstvo u grupi se riješiti.
  - Organizacijska Jedinica **RegisteredDevices** mora biti označena ako ste omogućili značajku upisima uređaja. Ako koristite neki drugi značajku upisima, kao što su grupe upisima, obavezno ova mjesta su.
  - Odaberite drugi OU gdje se nalaze korisnike, iNetOrgPersons, grupe, kontakata i računala. Na slici sve to nalaze parametra OU ManagedObjects.
7. Kada završite, zatvorite dijaloški okvir **Svojstva** klikom na **u redu**.
8. Da biste dovršili konfiguraciju, [Primijeni i provjerite je li promjene](#apply-and-verify-changes).

## <a name="attribute-based-filtering"></a>Atribut sustavom filtriranja
Provjerite je li na 2015 studenom ([1.0.9125](active-directory-aadconnect-version-history.md#1091250)) ili kasnije sastavljanje za ove korake da biste radili.

Atribut koji se temelje filtriranje je najčešće fleksibilne način objekti filtra. Da biste odredili gotovo svaki aspekt kada objekta moraju se sinkronizirati s Azure AD možete koristiti na potenciju [deklarativno dodjele resursa](active-directory-aadconnectsync-understanding-declarative-provisioning.md) .

Filtriranje mogu se primijeniti i na na [dolazni](#inbound-filtering) iz servisa Active Directory u metaverse i [Izlazni](#outbound-filtering) iz na metaverse za Azure AD. Preporučuje se da biste primijenili filtriranje na dolazni Budući da se najlakši da biste zadržali. Izlazni filtriranja mogu koristiti samo ako je potrebno da biste se pridružili objekte iz više od jednog skupa stabala prije analizu može obaviti.

### <a name="inbound-filtering"></a>Dolazni filtriranja
Dolazni na temelju filtriranja koristi zadanu konfiguraciju gdje objekte koje namjeravate Azure AD mora imati cloudFiltered atribut metaverse ne postavite na vrijednost koja će se sinkronizirati. Ako je vrijednost toga atributa postavljen na **True**, zatim objekt nije sinkroniziran. Mora neće biti postavljena na **False** dizajnom. Da biste provjerili drugih pravila mogućnost da biste slali vrijednosti, taj atribut samo bi trebao imati vrijednosti **True** ili **NULL** (nema).

U ulaznog filtriranje power **opseg** koristite da biste utvrdili koji objekti trebali biste ili neće se sinkronizirati. Ovo je odabirati prilagodbe kako bi odgovaralo potrebama vlastite tvrtke ili ustanove. Modul za opseg sadrži **grupe** i **uvjet** da biste odredili ako pravilo sinkronizaciju mora biti u opsegu. **Grupa** sadrži jedan ili više **uvjeta**. Postoji logički i među više uvjeta i s logičkim ili više grupa.

Javite nam potražite na primjer:  
![Opseg](./media/active-directory-aadconnectsync-configure-filtering/scope.png) to trebaju biti **(odjel = IT) ili (odjel = prodaje i c = US)**.

U primjerima i korake u nastavku, koristite korisničkom objektu, primjerice, ali to možete koristiti za sve vrste objekata.

U nastavku uzoraka vrijednost prioritet počet 500. Ta vrijednost osigurava ta pravila vrednuju se nakon pravila – okvir (donjem prednost, veća numeričku vrijednost).

#### <a name="negative-filtering-do-not-sync-these"></a>Negativni filtriranja, "se neće sinkronizirati te"
U sljedećem primjeru, koji filtriraju (neće se sinkronizirati) sve korisnike kojima **extensionAttribute15** imaju vrijednost **NoSync**.

1. Prijavite se na poslužitelju sa sustavom Azure AD Connect sinkronizaciju pomoću računa koji je član **ADSyncAdmins** sigurnosne grupe.
2. Da biste pokrenuli **Sinkronizaciju pravila Editor** na izborniku start.
3. Provjerite je li **ulazni** , a zatim kliknite **Dodaj novo pravilo**.
4. Dodijelite pravilo opisni naziv, kao što su "*u iz AD – DoNotSyncFilter korisnika*". **MV vrsta objekta**odaberite na odgovarajuće skupa stabala, **korisnik** kao **Vrsta objekta CS**i **osobe** . Kao **Vrsta veze**, odaberite **Uključivanje** u redoslijedu upišite vrijednost trenutno ne koristi sinkronizaciju drugo pravilo (na primjer 500) i zatim kliknite **Dalje**.  
![Dolazni 1 opis](./media/active-directory-aadconnectsync-configure-filtering/inbound1.png)  
5. U **Scoping filtar**, kliknite **Dodaj grupu**, kliknite **Dodaj uvjet**i u atributu odaberite **ExtensionAttribute15**. Provjerite je li Operator je postavljeno na **JEDNAKI** , a zatim upišite vrijednost **NoSync** u okvir vrijednost. Kliknite **Dalje**.  
![Dolazni 2 dosega](./media/active-directory-aadconnectsync-configure-filtering/inbound2.png)  
6. Pravila za **Uključivanje** ostavite prazno, a zatim kliknite **Dalje**.
7. Kliknite **Dodavanje transformaciju**, odaberite **FlowType** da biste **konstantu**, odaberite ciljni atribut **cloudFiltered** i u tekstni okvir izvor upišite **True**. Kliknite **Dodaj** da biste spremili pravilo.  
![Dolazni 3 transformacije](./media/active-directory-aadconnectsync-configure-filtering/inbound3.png)
8. Da biste dovršili konfiguraciju, [Primijeni i provjerite je li promjene](#apply-and-verify-changes).

#### <a name="positive-filtering-only-sync-these"></a>Pozitivan filtriranja, "sinkronizirati samo one"
Kojem se iznosi ono pozitivan filtriranje može biti zahtjevnije jer morate uzeti objekte koji nisu vidljivi može sinkronizirati, kao što su sobe za sastanke.

Pozitivni mogućnost filtriranja, morate imati dvije pravila za sinkronizaciju. Jedan (ili nekoliko) uz ispravan opsega objekte za sinkronizaciju i drugi zamka sve sinkronizaciju pravilo filtar sve objekte koji još nije označen kao objekt koji se sinkronizirati.

U sljedećem primjeru samo sinkronizirati korisnički objekti gdje atribut odjel ima vrijednost **prodaje**.

1. Prijavite se na poslužitelju sa sustavom Azure AD Connect sinkronizaciju pomoću računa koji je član **ADSyncAdmins** sigurnosne grupe.
2. Da biste pokrenuli **Sinkronizaciju pravila Editor** na izborniku start.
3. Provjerite je li **ulazni** , a zatim kliknite **Dodaj novo pravilo**.
4. Dodijelite pravilo opisni naziv, primjerice "*u iz AD – Prodaja korisnika sinkronizirati*". **MV vrsta objekta**odaberite na odgovarajuće skupa stabala, **korisnik** kao **Vrsta objekta CS**i **osobe** . Kao **Vrsta veze**, odaberite **Uključivanje** u redoslijedu upišite vrijednost trenutno ne koristi sinkronizaciju drugo pravilo (na primjer 501) i zatim kliknite **Dalje**.  
![Dolazni 4 opis](./media/active-directory-aadconnectsync-configure-filtering/inbound4.png)  
5. U **Scoping filtar**, kliknite **Dodaj grupu**, kliknite **Dodaj uvjet**i u atributu odaberite **odjela**. Provjerite je li Operator je postavljeno na **JEDNAKI** , a zatim upišite vrijednost **prodaje** u okvir vrijednost. Kliknite **Dalje**.  
![Dolazni 5 dosega](./media/active-directory-aadconnectsync-configure-filtering/inbound5.png)  
6. Pravila za **Uključivanje** ostavite prazno, a zatim kliknite **Dalje**.
7. Kliknite **Dodavanje transformaciju**, odaberite **FlowType** da biste **konstantu**, odaberite ciljni atribut **cloudFiltered** i u tekstni okvir izvor upišite **False**. Kliknite **Dodaj** da biste spremili pravilo.  
![Dolazni 6 transformacije](./media/active-directory-aadconnectsync-configure-filtering/inbound6.png)  
To je posebno slučaj kojem odrediti cloudFiltered izričito False.

    Sada imamo da biste stvorili pravilo zamka sve sinkronizacije.

8. Dodijelite pravilo opisni naziv, kao što su "*u iz AD – zamka svih korisnika filtar*". **MV vrsta objekta**odaberite na odgovarajuće skupa stabala, **korisnik** kao **Vrsta objekta CS**i **osobe** . **Vrsta veze**, odaberite **Uključivanje** te u redoslijedu upišite vrijednost trenutno ne koristi sinkronizaciju drugo pravilo (na primjer 600). Ste odabrali prednost vrijednost veća (donjem prioriteta) od prethodnog pravila sinkronizacije, ali i ostavili prostora pa ćemo kasnije možete dodati više pravila sinkronizaciju filtriranja kada želite da biste pokrenuli sinkronizaciju dodatne odjela. Kliknite **Dalje**.  
![Dolazni 7 opis](./media/active-directory-aadconnectsync-configure-filtering/inbound7.png)  
9. **Filtar Scoping** ostavite prazno, a zatim kliknite **Dalje**. Prazan filtar upućuje na to pravilo primjenjuje na sve objekte.
10. Pravila za **Uključivanje** ostavite prazno, a zatim kliknite **Dalje**.
11. Kliknite **Dodavanje transformaciju**, odaberite **FlowType** da biste **konstantu**, odaberite ciljni atribut **cloudFiltered** i u tekstni okvir izvor upišite **True**. Kliknite **Dodaj** da biste spremili pravilo.  
![Dolazni 3 transformacije](./media/active-directory-aadconnectsync-configure-filtering/inbound3.png)  
12. Da biste dovršili konfiguraciju, [Primijeni i provjerite je li promjene](#apply-and-verify-changes).

Ako je potrebno, možete stvoriti više pravila prva vrsta koje uključuju više objekata u našem sinkronizacije.

### <a name="outbound-filtering"></a>Izlazni filtriranja
U nekim slučajevima je potrebno učiniti filtriranje tek nakon što se objekti su se uključili u na metaverse. Na primjer, možda potrebne za pregled atribut pošte iz skupa stabala resursa i atribut userPrincipalName iz skupa stabala za račun da biste odredili može li sinkronizirati objekta. U tim slučajevima, stvorite filtriranje na izlazni pravilo.

U ovom primjeru, promijenite filtriranja tako samo korisnici kojima pošta i userPrincipalName završavati @contoso.com sinkroniziraju:

1. Prijavite se na poslužitelju sa sustavom Azure AD Connect sinkronizaciju pomoću računa koji je član **ADSyncAdmins** sigurnosne grupe.
2. Da biste pokrenuli **Sinkronizaciju pravila Editor** na izborniku start.
3. U odjeljku **Vrste pravila**kliknite **Izlazni**.
4. Pronađite pravilo pod nazivom **Odjava AAD – uključivanje SOAInAD korisnika**. Kliknite **Uređivanje**.
5. U skočnom prozoru, odgovorite **da** biste stvorili kopiju pravila.
6. Na stranici **Opis** promijenite prednost na koji se ne koriste vrijednost, primjerice 50.
7. Kliknite **Filtar Scoping** u navigaciji. Kliknite **Dodaj uvjet**, atribut odaberite **pošta**, odaberite Operator **ENDSWITH**i vrsta vrijednosti **@contoso.com**. Kliknite **Dodaj uvjet**, u odaberite atribut **userPrincipalName**, odaberite Operator **ENDSWITH**i vrsta vrijednosti **@contoso.com**.
8. Kliknite **Spremi**.
9. Da biste dovršili konfiguraciju, [Primijeni i provjerite je li promjene](#apply-and-verify-changes).

## <a name="apply-and-verify-changes"></a>Primjena i provjerite je li promjene
Nakon što ste napravili promjene konfiguracije, to morate primijeniti na objekte koji su već postoje u sustavu. Također može biti objekata trenutno ne u modula za sinkroniziranje treba biti obrađen, a modula za sinkroniziranje treba čitati u izvorišnom sustavu ponovno da biste provjerili njegov sadržaj.

Ako ste promijenili konfiguraciju korištenje **domene** ili **organizacijsku jedinicu** filtriranje, morate učiniti **potpuni uvoz** slijedi **Delta sinkronizacije**.

Ako ste promijenili konfiguracije pomoću **atribut** filtriranja, morate učiniti **potpune sinkronizacije**.

Poduzmite sljedeće korake:

1. Pokrenite **Servis za sinkronizaciju** s izbornika start.
2. Odaberite **poveznika** , a zatim na popisu **poveznika** odaberite poveznik gdje ste promijenili neke starije verzije. **Akcije**, odaberite **Pokreni**.  
![Poveznik za pokretanje](./media/active-directory-aadconnectsync-configure-filtering/connectorrun.png)  
3. **Pokretanje profila**, odaberite operaciju u navedenim u prethodnom odjeljku. Ako morate pokrenuti dvije akcije, pokrenite drugi po dovršetku prvoga (stupac **Stanje** je **stanje neaktivnosti** za odabrani poveznik).

Nakon sinkronizacije, sve promjene su kopirana bez postavljanja da biste se izvesti. Prije nego što zapravo unesite željene promjene u Azure AD, preporučujemo da te promjene provjerite jesu li točni.

1. Pokretanje cmd redak, a zatim prijeđite na drugi`%Program Files%\Microsoft Azure AD Sync\bin`
2. Pokrenite:`csexport "Name of Connector" %temp%\export.xml /f:x`  
Naziv poveznik pronaći ćete u servis za sinkronizaciju. Ima naziv koji je slična "contoso.com – AAD" za Azure AD.
3. Pokrenite:`CSExportAnalyzer %temp%\export.xml > %temp%\export.csv`
4. Sada imate datoteke u % temp % pod nazivom export.csv koje možete pregledavaju u programu Microsoft Excel. Datoteka sadrži sve promjene koje želite izvesti.
5. Izvršite potrebne promjene podataka ili konfiguraciju i pokretanje korake ponovno (uvoz, sinkronizacija i potvrda) sve dok se očekuje promjene koje želite izvesti.

Kada ste zadovoljni, izvoz promjene Azure AD.

1. Odaberite **poveznika** , a zatim na popisu **poveznika** odaberite poveznik za Azure AD. **Akcije**, odaberite **Pokreni**.
2. **Pokretanje profila**, odaberite **Izvoz**.
3. Ako promjene konfiguracije koje ste izbrisali više objekata, zatim vidjet ćete pogrešku na Izvoz kada je broj veći od praga konfigurirani (po zadanom 500). Ako se prikaže Ova pogreška, morate privremeno onemogućiti značajke [sprječava nenamjerno brisanje](active-directory-aadconnectsync-feature-prevent-accidental-deletes.md).

Sada je vrijeme da biste ponovno omogućiti na raspored.

1. Pokretanje **Rasporeda zadataka** s izbornika start.
2. Izravno u odjeljku **Biblioteka rasporeda zadataka**, pronađite zadatak pod nazivom **Azure AD sinkronizaciju rasporeda**, desnom tipkom miša, a zatim odaberite **Omogući**.

## <a name="next-steps"></a>Daljnji koraci
Dodatne informacije o konfiguraciji [Azure AD Connect sinkronizirati](active-directory-aadconnectsync-whatis.md) .

Dodatne informacije o [integraciji vaših lokalnih identiteta sa servisu Azure Active Directory](active-directory-aadconnect.md).
