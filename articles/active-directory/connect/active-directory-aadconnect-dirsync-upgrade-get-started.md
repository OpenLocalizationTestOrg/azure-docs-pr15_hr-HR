<properties
   pageTitle="Azure AD Connect: Nadogradili DirSync | Microsoft Azure"
   description="Saznajte kako nadograditi DirSync Azure AD Connect. Članci opisuju koraci za nadogradnju s DirSync na Azure AD Connect."
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
   ms.topic="get-started-article"
   ms.date="08/19/2016"
   ms.author="shoatman;billmath"/>

# <a name="azure-ad-connect-upgrade-from-dirsync"></a>Azure AD Connect: Nadogradnja iz DirSync
Azure AD Connect nije naslijediti da biste DirSync. Pronađite načine možete nadograditi DirSync u ovoj temi. Ti koraci ne funkcioniraju za nadogradnju s drugom izdanje Azure AD Connect ili Azure AD sinkronizaciju.

Prije pokretanja instalacije Azure AD Connect, provjerite je li da biste [preuzeli Azure AD Connect](http://go.microsoft.com/fwlink/?LinkId=615771) i dovršite stara obavezna koraka u [Azure AD Connect: hardver i preduvjeti](../active-directory-aadconnect-prerequisites.md). Posebno koju želite pročitati o:, budući da se razlikuju od DirSync ta područja:

- Potrebna verzija .net i PowerShell. Novija verzija moraju biti na poslužitelju nego što DirSync potrebno.
- Konfiguracija proxy poslužitelja. Ako koristite proxy poslužitelj dosegne s Internetom, tu postavku morate konfigurirati prije nadogradnje. DirSync uvijek koristi proxy poslužitelj konfiguriran za korisnika instalacije, ali postavke na računalu koristi Azure AD Connect umjesto toga.
- URL-ove moraju biti otvoreni u proxy poslužitelj. Osnovni scenarije i podržava DirSync, zahtjevima su uvijek jednake. Ako želite koristiti bilo koju od novih značajki u sklopu Azure AD Connect neki novi URL-ovi moraju biti otvoreni.

Ako ne instalirate nadogradnju DirSync, potražite u članku [povezani dokumentaciju](#related-documentation) za drugim situacijama.

## <a name="upgrade-from-dirsync"></a>Nadogradnja s DirSync
Ovisno o trenutnom DirSync implementacije dostupne su različite mogućnosti za nadogradnju. Ako je očekivano vrijeme tijekom nadogradnje manje od tri sata, zatim na preporuke je za nadogradnju na mjestu. Ako je očekivano vrijeme tijekom nadogradnje više od tri sata, zatim na preporuke je učiniti paralelno implementacije na drugi poslužitelj. Procijenjeno je da ako imate više od 50.000 objekata potrebno više od tri sata za nadogradnju.

Scenarij |  
---- | ----
[Nadogradnja na mjestu](#in-place-upgrade)  | Željenu mogućnost ako je nadogradnja se očekuje da bi manje od 3 sata.
[Paralelni implementacije](#parallel-deployment) | Željenu mogućnost ako nadogradnje očekuje potrajati više od 3 sata.

>[AZURE.NOTE] Kada planirate nadograditi DirSync Azure AD Connect nije deinstalirajte DirSync sami prije nadogradnje. Azure AD Connect će pročitati i popis svakodnevnih zadataka konfiguraciju DirSync i Deinstalacija nakon pregled poslužitelj.

**Nadogradnja na mjestu**  
Očekivani put da biste dovršili nadogradnju prikazat će se čarobnjak. Ovaj članak Procjena uvjeta temelji se na pretpostavci traje tri sata da biste dovršili nadogradnju za bazu podataka s 50.000 objektima (korisnika, kontakte i grupe). Ako je broj objekata u bazi podataka manja od 50.000, Azure AD Connect preporučuje nadogradnju na mjestu. Ako odlučite nastaviti trenutne postavke primjenjuju automatski tijekom nadogradnje, a vaš poslužitelj automatski nastavlja sinkronizacija sa servisom active.

Ako želite učiniti konfiguracija migracije i učinite paralelno implementaciju, možete nadjačati preporuke nadogradnje na istome mjestu. Na primjer može potrajati prilike da biste osvježili hardver i operacijski sustav. Dodatne informacije potražite u odjeljku [paralelno implementacije](#parallel-deployment) .

**Paralelni implementacije**  
Ako imate više od 50.000 objekata, preporučuje se paralelno implementacije. Taj se način izbjegavaju sve radu kašnjenja iskusnih korisnicima. Instalacija Azure AD Connect pokušava procjenu nedostupnost za nadogradnju, no ako ste nadogradili DirSync u prošlosti, vlastite sučelje je vjerojatno će biti Praktični vodič.

### <a name="supported-dirsync-configurations-to-be-upgraded"></a>Podržani DirSync konfiguracije za nadogradnju
Konfiguracija sljedeće promjene podržani su uz DirSync su nadograditi:

- Domene i OU filtriranja
- Zamjenski ID (UPN)
- Sinkroniziranje lozinkom i postavke hibridnog sustava Exchange
- Skup stabala/domene i postavke Azure AD
- Filtriranje na temelju korisničke atribute

Nije moguće nadograditi sljedeće promjene. Ako imate tu konfiguraciju, Nadogradnja je blokirana:

- Nepodržane DirSync promjene, na primjer ukloniti atributi i korištenje prilagođenih nastavka DLL

![Nadogradnja je blokirana](./media/active-directory-aadconnect-dirsync-upgrade-get-started/analysisblocked.png)

U tim slučajevima u preporuka je da biste instalirali novi poslužitelj za Azure AD Connect u [pripremna način](../active-directory-aadconnectsync-operations.md#staging-mode) i provjerite je li DirSync staru i novu konfiguraciju Azure AD Connect. Ponovna Primjena promjene pomoću prilagođene konfiguracije, kao što je opisano u [sinkronizaciju povezivanje Azure AD prilagođene konfiguracije](../active-directory-aadconnectsync-whatis.md).

Lozinke koristi DirSync računa servisa nije moguće dohvatiti, a ne migriraju. Tijekom nadogradnje Vrati izvorne postavke te lozinke.

### <a name="high-level-steps-for-upgrading-from-dirsync-to-azure-ad-connect"></a>Više razine koraci za nadogradnju s DirSync na Azure AD Connect

1. Dobro došli u Azure AD povezivanje
2. Analiza Trenutna konfiguracija DirSync
3. Prikupljanje Azure AD globalni administratorsku lozinku
4. Prikupljanje vjerodajnice za račun za administratore enterprise (samo za koristi se tijekom instalacije Azure AD Connect)
5. Povezivanje instalacije Azure AD
    * Deinstalacija DirSync (ili privremeno onemogućiti)
    * Povezivanje Instaliraj Azure AD
    * Ako želite započeti sinkronizaciju

Dodatni koraci potrebni su kada:

* Već koristite cijelog sustava SQL Server – lokalnom ili udaljenom
* Imate više od 50.000 objekata u opsegu za sinkronizaciju

## <a name="in-place-upgrade"></a>Nadogradnja na mjestu

1. Pokretanje instalacijskog programa za Azure AD Connect (MSI).
2. Pregledajte i prihvatite licencne odredbe i obavijest o zaštiti privatnosti.
![Dobro došli u Azure AD](./media/active-directory-aadconnect-dirsync-upgrade-get-started/Welcome.png)
3. Da biste pokrenuli analizu postojeće instalacije DirSync, kliknite Dalje.
![Analiza postojeće instalacije Sinkroniziranje direktorija](./media/active-directory-aadconnect-dirsync-upgrade-get-started/Analyze.png)
4. Kada se dovrši analizu, vidjet ćete preporuke za nastavak.  
    - Ako koristite SQL Server Express i imaju manje od 50.000 objekte, na sljedećem zaslonu prikazuju: ![analizu dovršiti spremna za nadogradnju s DirSync](./media/active-directory-aadconnect-dirsync-upgrade-get-started/AnalysisReady.png)
    - Ako koristite cijelog sustava SQL Server za DirSync, vidite ovu stranicu umjesto: ![analizu dovršiti spremna za nadogradnju s DirSync](./media/active-directory-aadconnect-dirsync-upgrade-get-started/AnalysisReadyFullSQL.png)  
Prikazuje se podatak postojećeg poslužitelja baze podataka SQL Server koristi DirSync. Po potrebi možete napraviti odgovarajuće prilagodbe. Kliknite **Dalje** da biste nastavili s instalacijom.
    - Ako imate više od 50.000 objekata, umjesto toga pogledajte ovaj zaslon: ![analizu dovršiti spremna za nadogradnju s DirSync](./media/active-directory-aadconnect-dirsync-upgrade-get-started/AnalysisRecommendParallel.png)  
Da biste nastavili s nadogradnju na mjestu, potvrdite okvir uz sljedeća poruka: **nastaviti nadogradnje DirSync na ovom računalu.**
Izvršite [paralelno implementacije](#parallel-deployment) umjesto izvoz postavki konfiguriranje DirSync i premještanje konfiguracije novi poslužitelj.
5. Unesite lozinku za račun koji trenutno koristite za povezivanje s Azure AD. To mora biti račun koji se trenutno koristi DirSync.
![Unesite vjerodajnice za Azure AD](./media/active-directory-aadconnect-dirsync-upgrade-get-started/ConnectToAzureAD.png)  
Ako obavijest o pogrešci i imate problema s povezivanjem, pročitajte članak [Otklanjanje poteškoća s povezivanjem](../active-directory-aadconnect-troubleshoot-connectivity.md).
6. Navedite račun administrator tvrtke za Active Directory.
![Unesite vjerodajnice za ZBRAJA](./media/active-directory-aadconnect-dirsync-upgrade-get-started/ConnectToADDS.png)
7. Sada ste spremni za konfiguriranje. Kada kliknete **nadogradnju**, deinstalirate DirSync i Azure AD Connect je konfiguriran i počinje sinkroniziranje.
![Jeste li spremni za konfiguraciju](./media/active-directory-aadconnect-dirsync-upgrade-get-started/ReadyToConfigure.png)
8. Nakon dovršetka instalacije, Odjava i ponovno se prijavite u sustav Windows prije pomoću sinkronizacije servisa za upravljanje uređivaču pravila sinkronizacije ili pokušate unijeti drugih promjena konfiguracije.

## <a name="parallel-deployment"></a>Paralelni implementacije

### <a name="export-the-dirsync-configuration"></a>Izvoz konfiguracije DirSync
**Paralelni implementacija s više od 50.000 objekata**

Ako imate više od 50.000 objekata, instalacija Azure AD Connect preporučuje paralelno implementacije.

Prikazuje se na zaslon sličnu ovoj:

![Analiza dovršeno](./media/active-directory-aadconnect-dirsync-upgrade-get-started/AnalysisRecommendParallel.png)

Ako želite da biste nastavili s paralelno implementaciju, morate poduzeti sljedeće korake:

- Kliknite gumb **Izvoz postavki** . Kada instalirate Azure AD Connect na istom poslužitelju, te postavke migriraju se od svoje trenutne DirSync s novom instalacijom Azure AD Connect.

Nakon postavki ste uspješno izvoza, izađite iz čarobnjaka za Azure AD Connect na poslužitelj DirSync. Nastavite na sljedeći korak da biste [instalirali Azure AD Connect na istom poslužitelju](#installation-of-azure-ad-connect-on-separate-server)

**Paralelni implementacija s manje od 50.000 objekata**

Ako imate manje od 50.000 objekata, ali i dalje želite učiniti paralelno implementaciju, a zatim učinite sljedeće:

1. Pokrenite instalacijski program za Azure AD Connect (MSI).
2. Kada se prikaže zaslon **dobrodošlice za Azure AD Connect** , zatvorite čarobnjak za instalaciju tako da kliknete "X" u gornjem desnom kutu prozora.
3. Otvorite naredbeni redak.
4. Iz mjesto za instalaciju Azure AD Connect (zadano: povezivanje Active Directory c:\Programske datoteke\Microsoft Azure) pokrenite sljedeću naredbu:  `AzureADConnect.exe /ForceExport`.
5. Kliknite gumb **Izvoz postavki** .  Kada instalirate Azure AD Connect na istom poslužitelju, te postavke migriraju se od svoje trenutne DirSync s novom instalacijom Azure AD Connect.

![Analiza dovršeno](./media/active-directory-aadconnect-dirsync-upgrade-get-started/forceexport.png)

Nakon postavki ste uspješno izvoza, izađite iz čarobnjaka za Azure AD Connect na poslužitelj DirSync. Nastavite na sljedeći korak da biste [instalirali Azure AD Connect na istom poslužitelju](#installation-of-azure-ad-connect-on-separate-server)

### <a name="install-azure-ad-connect-on-separate-server"></a>Instalacija Azure AD Connect na istom poslužitelju

Kada instalirate Azure AD Connect na novi poslužitelj, pretpostavlja se da želite izvršiti čistoj instalaciji Azure AD Connect. Budući da želite konfiguracija DirSync postoje morate poduzeti neke dodatne korake:

1. Pokrenite instalacijski program za Azure AD Connect (MSI).
2. Kada se prikaže zaslon **dobrodošlice za Azure AD Connect** , zatvorite čarobnjak za instalaciju tako da kliknete "X" u gornjem desnom kutu prozora.
3. Otvorite naredbeni redak.
4. Iz mjesto za instalaciju Azure AD Connect (zadano: povezivanje Active Directory c:\Programske datoteke\Microsoft Azure) pokrenite sljedeću naredbu:  `AzureADConnect.exe /migrate`.
    Čarobnjak za instalaciju Azure AD Connect pokreće i predstavlja na sljedećem zaslonu: ![unesite vjerodajnice za Azure AD](./media/active-directory-aadconnect-dirsync-upgrade-get-started/ImportSettings.png)
5. Odaberite datoteku s postavkama koje izvezli iz instalacije sustava DirSync.
6. Konfiguriranje sve dodatne mogućnosti, uključujući:
    - Mjesto za Azure AD Connect prilagođenu instalaciju.
    - Postojeće instance komponente SQL Server (zadano: Azure AD Connect instalira SQL Server 2012 Express). Nemojte koristiti isti instanca baze podataka kao poslužitelj DirSync.
    - Servis za račun koji se koristi za povezivanje sa sustavom SQL Server (Ako je ovaj račun mora biti račun servisa za domene je udaljenom bazom podataka sustava SQL Server).
Ove mogućnosti koje se mogu vidjeti na zaslonu: ![unesite vjerodajnice za Azure AD](./media/active-directory-aadconnect-dirsync-upgrade-get-started/advancedsettings.png)
7. Kliknite **Dalje**.
8. Na stranici **spremno za konfiguraciju** ostavite **pokretanje postupka za sinkronizaciju čim se dovrši konfiguraciju** potvrđen. Poslužitelj je sada u [pripremna način](../active-directory-aadconnectsync-operations.md#staging-mode) da bi se promjene nisu podržani Azure AD.
9. Kliknite **Instaliraj**.
10. Nakon dovršetka instalacije, Odjava i ponovno se prijavite u sustav Windows prije pomoću sinkronizacije servisa za upravljanje uređivaču pravila sinkronizacije ili pokušate unijeti drugih promjena konfiguracije.

>[AZURE.NOTE] Započne sinkronizacija između Windows Server Active Directory i Azure Active Directory, ali bez promjena izvoze se Azure AD. Samo jedan alat za sinkronizaciju može biti aktivno izvoz promjene istodobno. Ovaj status zove [pripremna načinu rada](../active-directory-aadconnectsync-operations.md#staging-mode).

### <a name="verify-that-azure-ad-connect-is-ready-to-begin-synchronization"></a>Provjerite je li Azure AD Connect jeste li spremni za Početak sinkronizacije

Da biste provjerili jeste li spremni za preuzimanje uloge iz DirSync Azure AD Connect, morate da biste otvorili **Upravitelj za sinkronizaciju servisa** **Azure AD Connect** iz grupe na izborniku start.

U aplikaciji, idite na karticu **operacije** . Na ovoj kartici potvrdite da ste dovršili sljedeće postupke:

- Uvoz AD Connector
- Uvoz Azure AD Connector
- Potpuna Sinkroniziraj u AD poveznika
- Potpuna sinkronizacija Azure AD Connector

![Uvoz i sinkronizacija je dovršena](./media/active-directory-aadconnect-dirsync-upgrade-get-started/importsynccompleted.png)

Pregled rezultata iz te operacije, a zatim provjerite nema li pogrešaka.

Ako želite da biste vidjeli i pregledati promjene koje su o da biste se izvesti Azure AD pročitajte kako provjeriti konfiguraciju [pripremna](../active-directory-aadconnectsync-operations.md#staging-mode)načinu. Promjene konfiguracije potrebne dok ne vide išta neočekivane.

Spremni ste da biste se prebacili iz DirSync Azure AD kada dovršite korake u nastavku i su zadovoljni rezultat.

### <a name="uninstall-dirsync-old-server"></a>Deinstalacija DirSync (stari server)

- U odjeljku **Programi i značajke** pronaći **Alat za sinkronizaciju Windows Azure Active Directory**
- Deinstalacija **Alat za sinkronizaciju Windows Azure Active Directory**
- Deinstalaciju može potrajati i do 15 minuta.

Ako biste radije da biste deinstalirali DirSync kasnije, možete i privremeno isključiti poslužitelja ili onemogućite taj servis. Ako nešto pošlo po redu, ovaj postupak omogućuje vam da biste ponovno omogućiti. Međutim, nije očekuje da na sljedeći korak neće uspjeti tako da je to potrebno neće biti potrebne.

Uz DirSync deinstalirati ili onemogućen, poslužitelj ne postoji aktivni izvoz Azure AD. Sve promjene u lokalni Active Directory će se i dalje se sinkronizirati s Azure AD, morate izvršiti sljedeće korake da biste omogućili Azure AD Connect.

### <a name="enable-azure-ad-connect-new-server"></a>Omogućivanje Azure AD Connect (novi poslužitelj)
Nakon instalacije, ponovno otvoriti Azure AD povezivanje će omogućuju vam da biste promijenili dodatna konfiguracija. Pokreće **Azure AD Connect** s izbornika start ili prečac na radnoj površini. Provjerite je li pokušaja pokrenite instalaciju MSI.

Trebali biste vidjeti sljedeće:

![Dodatni zadaci](./media/active-directory-aadconnect-dirsync-upgrade-get-started/AdditionalTasks.png)

- Odaberite **Konfiguriraj pripremna načinu rada**.
- Isključite pripremna poništavanjem potvrdnog okvira **omogućeno pripremna način** .

![Unesite vjerodajnice za Azure AD](./media/active-directory-aadconnect-dirsync-upgrade-get-started/configurestaging.png)

- Kliknite gumb **Dalje**
- Na stranici za potvrdu kliknite gumb **instalirati** .

Azure AD Connect sada je aktivnog poslužitelja.

## <a name="next-steps"></a>Daljnji koraci
Sad kad ste instalirali Azure AD Connect možete [provjeriti instalaciju i dodjela licenci](../active-directory-aadconnect-whats-next.md).

Dodatne informacije o tim novim značajkama koje su omogućena za instalaciju: [Automatska Nadogradnja](../active-directory-aadconnect-feature-automatic-upgrade.md), [Onemogućivanje slučajne briše](../active-directory-aadconnectsync-feature-prevent-accidental-deletes.md)i [Povežite zdravlje Azure AD](../active-directory-aadconnect-health-sync.md).

Dodatne informacije o ovim temama uobičajenih: [Raspored i kako pokrenuti sinkronizaciju](../active-directory-aadconnectsync-feature-scheduler.md).

Dodatne informacije o [integraciji vaših lokalnih identiteta sa servisu Azure Active Directory](../active-directory-aadconnect.md).

## <a name="related-documentation"></a>Srodni dokumentacija

Tema |  
--------- | ---------
Pregled Azure AD Connect | [Integriranje sustava lokalnih identiteta sa Azure Active Directory](../active-directory-aadconnect.md)
Nadogradnja s prethodne verzije za povezivanje | [Nadogradnja s prethodne verzije za povezivanje](../active-directory-aadconnect-upgrade-previous-version.md)
Instalacija pomoću postavki Express | [Express instalacije Azure AD Connect](active-directory-aadconnect-get-started-express.md)
Instalacija pomoću prilagođenih postavki | [Prilagođene instalacije Azure AD Connect](active-directory-aadconnect-get-started-custom.md)
Račune koji se koriste za instalaciju | [Dodatne informacije o Azure AD Connect računa i dozvola](active-directory-aadconnect-accounts-permissions.md)
