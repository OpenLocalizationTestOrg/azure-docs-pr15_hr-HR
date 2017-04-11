<properties
    pageTitle="Identitet sinkronizacije te duplikat atributa otpornost | Microsoft Azure"
    description="Novo ponašanje kako rukovati objekte s UPN-a ili ProxyAddress sukoba tijekom sinkronizacije direktorija pomoću Azure AD Connect."
    services="active-directory"
    documentationCenter=""
    authors="markusvi"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/29/2016"
    ms.author="markusvi"/>



# <a name="identity-synchronization-and-duplicate-attribute-resiliency"></a>Identitet sinkronizacije te duplikat atributa otpornost
Duplicirane atribut otpornost je značajka servisa Azure Active Directory koji će uklonili friction uzrokovanih **UserPrincipalName** i **ProxyAddress** sukobe prilikom izvođenja jedan od Microsoftovih alata za sinkronizaciju.

Ta dva atributa se obično trebaju biti jedinstvena preko svih **korisnika**, **grupe**ili **kontakta** objekata u zadani klijent za Azure Active Directory.

> [AZURE.NOTE] Samo korisnici mogu sadržavati UPNs.

Novo ponašanje koje ta značajka omogućuje da je u oblak dio kanal za sinkronizaciju, stoga je klijent agnostic i relevantnim oznakama za svaki proizvod Microsoft sinkronizacije uključujući Azure AD Connect, DirSync i MIM + poveznik. Generički termina "klijent za sinkronizaciju" se koristi u ovom dokumentu za predstavljanje bilo koju od tih proizvoda.

## <a name="current-behavior"></a>Trenutni ponašanje
Ako je pokušaj Dodjela novi objekt s vrijednošću UPN-a ili ProxyAddress koji krši ovo ograničenje jedinstvenosti, Azure Active Directory blokira taj objekt stvoren. Isto tako, ako objekt ažurira koje nisu jedinstvene UPN-a ili ProxyAddress, ažuriranje neće uspjeti. Pokušaj dodjele resursa ili ažuriranje je ponoviti po klijent za sinkronizaciju nakon svakog ciklusa izvoz, a i dalje neće uspjeti dok se ne Razriješi sukob. Nakon svakog pokušaj generira se poruke e-pošte za izvješće o pogrešci, a zatim pogreška prijavljen je tako da klijent za sinkronizaciju.

## <a name="behavior-with-duplicate-attribute-resiliency"></a>Ponašanje s otpornost duplicirane atributa
Umjesto potpuno ne uspijeva Dodjela resursa ili ažurirajte objekt duplicirane atribut, Azure Active Directory "quarantines" duplicirane atribut koji želite kršiti ograničenje jedinstvenosti. Ako je taj atribut potrebni za dodjelu resursa, kao što su UserPrincipalName, servis dodjeljuje vrijednost rezervirano mjesto. Odaberite oblik ove privremene vrijednosti  
"***<OriginalPrefix>+<4DigitNumber>@<InitialTenantDomain>. onmicrosoft.com***".  
Ako atribut nije obavezno, kao što su **ProxyAddress**, Azure Active Directory jednostavno quarantines atribut sukoba i nastavlja se sa stvaranjem objekta ili ažuriranje.

Nakon quarantining atribut, informacije o sukobu šalje se u istu pogreške izvješća e-pošte koristi stari ponašanje. Međutim, informacije se prikazuje samo u izvješće o pogrešci jednu sesiju kada se dogodi u karantene, ne on biti prijavljeni buduće poruke e-pošte. Također, budući da je uspio izvoz za taj objekt, klijent za sinkronizaciju Prijavite pogrešku i pokušajte ponovno stvaranje / ažuriranje operacija prilikom sljedeće sinkronizacije ciklusa.

Da biste podržavaju tu funkciju novi atribut dodana objekt klase korisnika, grupa i kontakata:  
**DirSyncProvisioningErrors**

To je atribut s više vrijednosti koja se koristi za pohranu sukobljenih atributa koji bi ograničenje jedinstvenosti treba ih dodati normalno. Zadatak mjerača vremena u pozadini u omogućena Azure Active Directory koja se pokreće svaki sat za traženje duplikata atribut sukoba koji ste riješen i automatski uklanja atribute dotičnog iz karantene.

### <a name="enabling-duplicate-attribute-resiliency"></a>Omogućivanje otpornost duplicirane atributa
Dupliciranih atribut otpornost će se novi zadano ponašanje preko svih klijenata za Azure Active Directory. Bit će na zadano za sve klijenata za koje je omogućena sinkronizacija prvo na kolovoz 22nd, 2016 ili noviji. Klijenata za koje je omogućena sinkronizacija prije no što taj datum će imati omogućena u grupama. U ovom uvođenje će početi u rujnu 2016, a obavijest putem e-pošte poslat će se svaki klijent kontaktu tehnička obavijest putem određeni datum kada će biti omogućene značajke.

Kada dupliciranje otpornost atribut je uključen ne može se onemogućiti.

Da biste provjerili značajku omogućena za vaš klijent, to možete učiniti tako da preuzimanje najnoviju verziju modula Azure Active Directory PowerShell i pokretanje:

`Get-MsolDirSyncFeatures -Feature DuplicateUPNResiliency`

`Get-MsolDirSyncFeatures -Feature DuplicateProxyAddressResiliency`

Ako želite doći omogućio značajku prije nego što je uključen za vaš klijent, to možete učiniti preuzimanje najnoviju verziju modula Azure Active Directory PowerShell i pokretanje:

`Set-MsolDirSyncFeature -Feature DuplicateUPNResiliency -Enable $true`

`Set-MsolDirSyncFeature -Feature DuplicateProxyAddressResiliency -Enable $true`

## <a name="identifying-objects-with-dirsyncprovisioningerrors"></a>Označavanje objekte s DirSyncProvisioningErrors
Da biste odredili objekte koji nemaju te pogreške zbog sukoba duplicirana svojstva Azure Active Directory PowerShell i centar za administratore sustava Office 365 na trenutno dva načina. Postoje tarife da biste proširili za dodatne portala na temelju izvješćivanje o pogreškama u budućnosti.

### <a name="azure-active-directory-powershell"></a>PowerShell Azure Active Directory
Za cmdleta ljuske PowerShell u ovoj temi su ispunjeni sljedeći uvjeti:

- Sve sljedeće Cmdlete su velika i mala slova.
- **– ErrorCategory PropertyConflict** uvijek mora biti uključeno. Trenutno nema druge vrste **ErrorCategory**, ali to može biti u budućnosti proširena.

Najprije početak rada pokrenut **Povezivanje MsolService** i unosom vjerodajnice za administratora klijenta.

Zatim, poduzmite sljedeće Cmdlete i operatora da biste pogledali pogreške na različite načine:

1. [Pogledajte sve](#see-all)

2. [Po vrsti svojstava](#by-property-type)

3. [Nije li u sukobu vrijednošću](#by-conflicting-value)

4. [Pomoću niza pretraživanja](#using-a-string-search)

5. [Sortirati](#sorted)

6. [Ograničeno količinom ili sve](#in-a-limited-quantity-or-all)


#### <a name="see-all"></a>Pogledajte sve
Nakon uspostave da biste vidjeli Opći popis atribut aktiviranja pogrešaka u klijentu pokrenuti:

`Get-MsolDirSyncProvisioningError -ErrorCategory PropertyConflict`

To daje rezultat kao što je sljedeća:  
 ![Get-MsolDirSyncProvisioningError](./media/active-directory-aadconnectsyncservice-duplicate-attribute-resiliency/1.png "Get-MsolDirSyncProvisioningError")  


#### <a name="by-property-type"></a>Po vrsti svojstava
Da biste vidjeli pogrešaka prema vrsti svojstva, dodajte zastavicu **- PropertyName** s argumentom **UserPrincipalName** ili **ProxyAddresses** :

`Get-MsolDirSyncProvisioningError -ErrorCategory PropertyConflict -PropertyName UserPrincipalName`

Ili

`Get-MsolDirSyncProvisioningError -ErrorCategory PropertyConflict -PropertyName ProxyAddresses`

#### <a name="by-conflicting-value"></a>Nije li u sukobu vrijednošću
Da biste pogledali pogreške koji se odnose na određene svojstvo dodali zastavicu **- VrijednostSvojstva** (**- PropertyName** mora se koristiti kao i kada dodajete se oznaka):

`Get-MsolDirSyncProvisioningError -ErrorCategory PropertyConflict -PropertyValue User@domain.com -PropertyName UserPrincipalName`


#### <a name="using-a-string-search"></a>Pomoću niza pretraživanja
Da biste učinili općenite niz pretraživanja pomoću zastavica **- SearchString** . To se može koristiti neovisno iz svih iznad zastavice, osim **-ErrorCategory PropertyConflict**koje je uvijek potrebno:

`Get-MsolDirSyncProvisioningError -ErrorCategory PropertyConflict -SearchString User`

#### <a name="in-a-limited-quantity-or-all"></a>Ograničeno količinom ili sve
1. **MaxResults <Int> ** može se koristiti za ograničavanje upita za određeni broj vrijednosti.

2. **Sve** se može se koristiti da biste bili sigurni da svi rezultati učitavaju u slučaju koji postoji velik broj pogreške.

`Get-MsolDirSyncProvisioningError -ErrorCategory PropertyConflict -MaxResults 5`

## <a name="office-365-admin-portal"></a>Portal za administratore sustava Office 365

Pogreške prilikom sinkronizacije direktorija možete pogledati u centru za administratore sustava Office 365. Izvješće na portalu za Office 365 prikazuje se samo na objekte **korisnika** koji nemaju te pogreške. Ne prikazuj informacije o sukoba između **grupa** i **kontakata**.


![Aktivni korisnici] (./media/active-directory-aadconnectsyncservice-duplicate-attribute-resiliency/1234.png "Aktivni korisnici")

Upute za prikaz sinkronizacijom direktorija u Centar za administratore sustava Office 365 potražite u članku [otkrivanje pogreške prilikom sinkronizacije direktorija u sustavu Office 365](https://support.office.com/en-us/article/Identify-directory-synchronization-errors-in-Office-365-b4fc07a5-97ea-4ca6-9692-108acab74067).


### <a name="identity-synchronization-error-report"></a>Izvješće o pogrešci sinkronizacije za identitet
Kada se rukuje objekta s duplicirane atribut sukob s taj novi način ponašanja obavijest uvrštava u standardni izvješće o pogrešci sinkronizacije identiteta e-pošte koji se šalju tehnička obavijest zatražite klijentu. No postoji važnih promjena u takvo ponašanje. U prošlosti, informacije o sukob duplicirane atribut želite uvrstiti u svaku kasnije izvješće o pogrešci dok ste razriješili sukob. Uz taj novi način ponašanja obavijest o pogrešci za dani sukoba pojavit samo jedanput – trenutku sukobljenih atributa je poslana u karantenu.

Evo primjera kako obavijesti e-pošte izgleda za ProxyAddress sukoba:  
    ![Aktivni korisnici](./media/active-directory-aadconnectsyncservice-duplicate-attribute-resiliency/6.png "Active Users")  

## <a name="resolving-conflicts"></a>Razrješavanje sukoba
Otklanjanje poteškoća taktike strategije i rješenja za te pogreške ne razlikuju od način duplicirane atribut pogreške obrađene su u prošlosti. Jedina razlika je da se zadatak mjerača vremena informacija putem klijenta na strani servis da se automatski dodaje u pitanju atribut proper objekta kada sukob riješen.

U sljedećem članku navode različite Strategije za otklanjanje poteškoća i razlučivost: [duplikat ili atributi koji nisu valjani onemogućuju Sinkroniziranje direktorija u sustavu Office 365](https://support.microsoft.com/kb/2647098).

## <a name="known-issues"></a>Poznati problemi
Ništa te poznatih problema uzrokuje smanjene performanse gubitka ili servis podataka. Nekoliko ih su iz estetskih, drugima standardni "*stara otpornost*" uzrokuju pogreške dupliciranih atribut da biste se izbačena umjesto quarantining atribut sukoba, a drugi uzrokuje određene pogreške obavezne vrlo ručni popravak gore.

**Temeljni ponašanje:**

1. Objekti s konfiguracijama atribut nastavit ćete primati izvoz pogreške umjesto dupliciranih attribute(s) se poslana u karantenu.  
Ako, na primjer:

    na. Novi korisnik stvoriti u AD s UPN od **Joe@contoso.com** i ProxyAddress**smtp:Joe@contoso.com**

    b. Svojstva objekta u sukobu s postojećom grupom, pri čemu je ProxyAddress **SMTP:Joe@contoso.com**.

    c. Nakon izvoza **ProxyAddress sukoba** pogreške se Expression.Error umjesto da atribute sukoba poslana u karantenu. Postupak je ponoviti nakon svakog ciklusa sljedeće sinkronizacije, kao što je želite su prije omogućivanja značajku otpornost.

2. Ako dva grupe se stvaraju lokalnog s istom adresom SMTP, jedan neće uspjeti za dodjelu resursa na prvi pokušaj standardne duplicirane **ProxyAddress** pogreška. Međutim, duplicirane vrijednosti ispravno karanteni nakon početka idućeg kruga sinkronizaciju.

**Office portala izvješća**:

1. Na detaljne poruka o pogrešci za dva objekta u skupu sukoba UPN jednak je. To označava da su oba imao njihove UPN promijenili / karanteni kada zapravo samo jedan od njih imali mijenjati podatke.

2. Na detaljne poruka o pogrešci za UPN sukoba prikazuje pogrešne zaslonski naziv za korisnike koji su se pojavili njihove UPN promijenili/poslana u karantenu. Ako, na primjer:

    na. **Korisnik A** sinkronizira se prvi s **UPN = User@contoso.com **.

    b. **Korisnik B** pokušali moguće sinkronizirati prema gore dok **UPN = User@contoso.com **.

    c. **Korisnik B** Mijenja se UPN **User1234@contoso.onmicrosoft.com** i **User@contoso.com** dodaje se **DirSyncProvisioningErrors**.

    d. Poruka o pogrešci za **Korisnika B** trebali biste naznačili da **korisnik** već ima **User@contoso.com** kao za UPN, ali prikazuje **Korisnik B** vlastite riješiti problem.



**Izvješće o pogrešci sinkronizacije identiteta**:

Vezu na *upute da biste riješili taj problem* nije valjana:  
    ![Aktivni korisnici](./media/active-directory-aadconnectsyncservice-duplicate-attribute-resiliency/6.png "Active Users")  

Moraju pokazivati [https://aka.ms/duplicateattributeresiliency](https://aka.ms/duplicateattributeresiliency).


## <a name="see-also"></a>Vidi također

- [Azure AD Connect sinkronizacije](active-directory-aadconnectsync-whatis.md)

- [Integriranje sustava lokalnih identiteta sa Azure Active Directory](active-directory-aadconnect.md)

- [Prepoznavanje pogreške sinkronizacije direktorija u sustavu Office 365](https://support.office.com/en-us/article/Identify-directory-synchronization-errors-in-Office-365-b4fc07a5-97ea-4ca6-9692-108acab74067)
