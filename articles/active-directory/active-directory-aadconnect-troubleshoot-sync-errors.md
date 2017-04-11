<properties
    pageTitle="Azure AD Connect: Otklanjanje poteškoća prilikom sinkronizacije | Microsoft Azure"
    description="U članku se objašnjava kako otkloniti poteškoće pogreške prilikom sinkronizacije s Azure AD Connect."
    services="active-directory"
    documentationCenter=""
    authors="karavar"
    manager="samueld"
    editor="curtand"/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/18/2016"
    ms.author="vakarand"/>

# <a name="troubleshooting-errors-during-synchronization"></a>Otklanjanje poteškoća prilikom sinkronizacije
Nije moguće dođe do pogreške prilikom identiteta podaci sinkroniziraju s Windows Server Active Directory (AD DS) Azure Active Directory (Azure AD). Ovaj članak sadrži pregled različitih vrsta pogreške prilikom sinkronizacije, neke od mogućih scenarija koji uzrokuju te pogreške i potencijalne načina da biste ispravili pogreške. Ovaj članak sadrži uobičajene pogreške vrste i ne pokrivaju mogućih pogrešaka.

 U ovom se članku pretpostavlja da je poznato povezane čitač [dizajniranje konceptima Azure AD i Azure AD Connect](active-directory-aadconnect-design-concepts.md).

Najnoviju verziju preglednika Azure AD Connect \(kolovoz 2016 ili noviji\), izvješće pogreške u sinkronizaciji je dostupan [Portal za Azure](https://aka.ms/aadconnecthealth) kao dio Azure AD povezivanje stanje sinkronizacije.


Pokretanje rujan 1, 2016 će je omogućiti značajku [Azure Active Directory dupliciranje atribut otpornost](active-directory-aadconnectsyncservice-duplicate-attribute-resiliency.md) prema zadanim postavkama za sve *nove* Azure Active Directory klijenata. Ta značajka automatski će se omogućiti za postojeće klijenata u nadolazeće mjesece.

Azure AD Connect izvodi 3 vrste operacija iz imenika zadržava u sinkronizaciji: Uvoz, sinkronizacija i izvoz. Pogreške može obaviti u sve operacije. U ovom se članku uglavnom usredotočuje se na pogreške tijekom izvoza za Azure AD.

## <a name="errors-during-export-to-azure-ad"></a>Pogreške prilikom izvoza za Azure AD
Slijedi sekcije opisuju se različite vrste pogrešaka pri sinkronizaciji koji se pojavljuju tijekom operacije izvoza za Azure AD pomoću poveznika Azure AD. Ovaj poveznik možete otkrije oblik naziva koji se "contoso. *onmicrosoft.com*".
Pogreške tijekom izvoza za Azure AD ukazuje na to operacija \(dodavanje, ažuriranje, brisanje itd\) pokušaj po Azure AD Connect \(modula za sinkroniziranje\) na Azure Active Directory nije uspjelo.

![Pregled pogrešaka za izvoz](.\media\active-directory-aadconnect-troubleshoot-sync-errors\Export_Errors_Overview_01.png)

## <a name="data-mismatch-errors"></a>Pogreške uslijed neodgovarajuće podataka
### <a name="invalidsoftmatch"></a>InvalidSoftMatch
#### <a name="description"></a>Opis
- Kada se Azure AD Connect \(modula za sinkroniziranje\) upućuje Azure Active Directory za dodavanje ili ažuriranje objekata, Azure AD odgovara dolazne objekta u Azure AD pomoću atribut **sourceAnchor** atribut **immutableId** objekata. Tu podudarnost zove je **Teško podudaraju**.
- Kada Azure AD **pronaći** bilo koji objekt koji odgovara **immutableId** atribut s atribut **sourceAnchor** dolazne objekta prije dodjele resursa novi objekt pada natrag da biste koristili atributa ProxyAddresses ni UserPrincipalName pronađeno podudaranje. Tu podudarnost naziva na **Meki podudaraju**. Meki odgovaraju osmišljene tako da odgovara objekata koje su već postoje u Azure AD (to su izvorni termini u Azure AD) s novih objekata koji se dodaju i ažuriraju tijekom sinkronizacije koji predstavlja isti entitet (korisnika, grupa) lokalno.
- **InvalidSoftMatch** pogreška se pojavljuje kada je teško match ne pronađe odgovarajući objekte **i** meki odgovaraju pronalazi podudaranja objekta, ali objekt ima različite vrijednost *immutableId* od dolazne objekt *SourceAnchor*, predlaže da odgovarajući objekt nije sinkronizirana s drugog objekta iz lokalnog imeničkog servisa Active Directory.

Drugim riječima, na umu da meki podudaranje za rad objekt bude mekih uparuje s treba ne sadrži sve vrijednosti parametra *immutableId*. Ako bilo koji objekt s *immutableId* postaviti vrijednost je neuspješnih teško-podudaranje, ali koje zadovoljavaju kriterij mekih podudaranje postupak dovodi do pogreške sinkronizacije InvalidSoftMatch.

Azure Active Directory sheme omogućite dva ili više objekata imaju iste vrijednosti od sljedećih atributa. \(Ovo nije iscrpan popis.\)

- ProxyAddresses
- UserPrincipalName
- onPremisesSecurityIdentifier
- ID objekta

>[AZURE.NOTE] Značajka [Azure AD atribut duplikat atributa stabilnosti](active-directory-aadconnectsyncservice-duplicate-attribute-resiliency.md) i koja se izvršavanja kao zadano ponašanje Azure Active Directory.  To će znatno smanjiti broj pogrešaka pri sinkronizaciji vide Azure AD Connect (kao i drugim klijentskim programima za sinkronizaciju) tako da odaberete Azure AD više prebacuju način na koji je rukovanja dupliciranu ProxyAddresses UserPrincipalName atributi i prisutne u AD lokalna okruženja. Ta značajka ispraviti pogreške dupliciranje. Podaci i dalje potrebno ispraviti. No omogućuje dodjeljivanje od novih objekata koji inače blokirana je u tijeku dodjeli zbog dupliciranog vrijednosti u Azure AD. To će smanjite broj pogrešaka pri sinkronizaciji vraćaju u klijent za sinkronizaciju.
Ako ta značajka je omogućen za vaš klijent, nećete vidjeti InvalidSoftMatch sinkronizacijom vidjeti tijekom dodjeljivanja od novih objekata.


#### <a name="example-scenarios-for-invalidsoftmatch"></a>Ogledni scenariji za InvalidSoftMatch
1. Dva ili više objekata s istom vrijednošću atributa ProxyAddresses postoji lokalnog servisa Active Directory. Jedina je pristup tom servisu dodijeljeni resursi Azure AD.
2. Dva ili više objekata s istom vrijednošću userPrincipalName postoji lokalnog servisa Active Directory. Jedina je pristup tom servisu dodijeljeni resursi Azure AD.
3. Objekt je dodana u na lokalni Active Directory s istom vrijednošću atributa ProxyAddresses kao postojećeg objekta servisa Azure Active Directory. Objekt dodali na lokalni nije početak dodijeljena Azure Active Directory.
4. Objekt je dodana u lokalni Active Directory s istom vrijednošću atributa userPrincipalName kao račun servisa Azure Active Directory. Objekt nije početak dodijeljena Azure Active Directory.
5. Sinkronizirane račun premješten iz skupa stabala od A do skupa stabala B. Azure AD Connect (modula za sinkroniziranje) koristio ObjectGUID atribut za izračun u SourceAnchor. Nakon premještanja skupa stabala razlikuje se vrijednost u SourceAnchor. Novi objekt (iz skupa stabala B) se ne uspijeva sinkronizirati s postojećeg objekta u Azure AD.
6. Sinkronizirane objekt imate slučajno izbrisane iz lokalnog imeničkog servisa Active Directory i novi objekt stvaranja u servisu Active Directory za isti entitet (kao što je korisnik) bez brisanja računa servisa Azure Active Directory. Novi račun ne uspijeva sinkronizirati s postojećeg Azure AD objekta.
7. Azure AD Connect nije deinstalirati i ponovno instalirati. Tijekom instalacije ponovno različite atribut nije odabran kao u SourceAnchor. Sve objekte koji ste prethodno sinkronizirane zaustaviti sinkronizaciju s pogreškom InvalidSoftMatch.

#### <a name="example-case"></a>Primjer slučaj:
1. **Teo Novak** je sinkronizirane korisnik servisa Azure Active Directory iz lokalnog servisa Active Directory *contoso.com*
2. Teo Novak **UserPrincipalName** postavljeno kao **bobs@contoso.com**.
3. **"abcdefghijklmnopqrstuv =="** je **SourceAnchor** izračunava Azure AD Connect pomoću Teo Novak **objectGUID** iz na lokalni Active Directory, koji je **immutableId** Teo Novak servisa Azure Active Directory.
4. Teo sadrži i pratiti vrijednosti za atributa **proxyAddresses** :
    - smtp:bobs@contoso.com
    - smtp:bob.smith@contoso.com
    - **smtp:bob@contoso.com**
5.  Novi korisnik **Teo Taylor**se dodaje na lokalni Active Directory.
6. Teo Taylor **UserPrincipalName** postavljeno kao **bobt@contoso.com**.
7. **"abcdefghijkl0123456789 ==" "** je **sourceAnchor** izračunava Azure AD Connect pomoću Teo Taylor **objectGUID** iz na lokalni Active Directory. Objekt Teo Taylor sadrži nisu sinkronizirane Azure Active Directory još.
8. Teo Taylor sadrži sljedeće vrijednosti za atributa proxyAddresses
    - smtp:bobt@contoso.com
    - smtp:bob.taylor@contoso.com
    - **smtp:bob@contoso.com**
9. Tijekom sinkronizacije, Azure AD Connect će prepoznaje zbrajanja Taylor Teo u lokalni Active Directory i pitajte Azure AD da biste ista će se promjena.
10. Azure AD najprije će izvesti teško podudaranje. To jest, pretraživanje ako je bilo koji objekt u immutableId jednako "abcdefghijkl0123456789 ==". Tvrdi podudaranje neće uspjeti kao drugog objekta u Azure AD će imati tu immutableId.
11. Azure AD pa će pokušati mekih podudaranje Teo Taylor. To jest, pretraživanje ako je bilo koji objekt s proxyAddresses jednaka tri vrijednosti, uključujućismtp:bob@contoso.com
12. Azure AD tražit će Teo Novak objekt odgovaraju kriterijima mekih podudaranje. No taj objekt ima vrijednost immutableId = "abcdefghijklmnopqrstuv ==". što znači da se taj objekt je sinkronizirati iz drugog objekta iz lokalnog imeničkog servisa Active Directory. Dakle, Azure AD ne mekih-match tih objekata i uzrokovat će pogrešku **InvalidSoftMatch** sinkronizaciju.

#### <a name="how-to-fix-invalidsoftmatch-error"></a>Kako riješiti pogreške InvalidSoftMatch
Najčešćih razloga pogreške InvalidSoftMatch je dva objekte s različitim SourceAnchor \(immutableId\) imaju jednaku vrijednost atributa ProxyAddresses i/ili UserPrincipalName, koji se koriste tijekom postupka mekih match na Azure AD. Da biste riješili problem koji nisu valjani meki podudaranje

1.  Odredite dupliciranu proxyAddresses, userPrincipalName ili druga vrijednost atributa koji je uzrok pogreške. Također prepoznavanje koje dva \(ili više\) objekata uključeni su u sukobu. Izvješće generira [Azure AD povezivanje stanje sinkronizacije](https://aka.ms/aadchsyncerrors) možete lakše prepoznali dva objekta.
2. Odredite koji objekt moraju se i dalje imati dupliciranu vrijednost, a ne bi trebala koji objekt.
3. Uklonite dupliciranu vrijednost iz objekta koji treba imati tu vrijednost. Imajte na umu da treba unesite željene promjene u direktoriju u kojem je izvorni objekt termini iz. U nekim slučajevima možda ćete morati izbrisali jedan od objekata u sukobu.
4. Ako ste izvršili promjene u na lokalni AD omogućuju Azure AD Connect sinkronizirati promjene.

Imajte na umu izvješće o pogrešci sinkronizacije unutar Azure AD povezivanje stanje sinkronizacije se ažurira svakih 30 minuta te sadrži pogreške od najnovijih pokušaj sinkronizacije.

>[AZURE.NOTE] ImmutableId, po definiciji, trebali biste neće se promijeniti u vijek objekt. Ako Azure AD Connect nije konfiguriran s nekim scenarijima na umu iznad popisa, nije moguće završiti prema gore u slučaju gdje Azure AD Connect izračunava drugu vrijednost SourceAnchor AD objekta koji predstavlja isti entitet (isti korisnika/grupe/kontakt itd) koji sadrži postojeće Azure AD objekt koji želite nastaviti koristiti.

#### <a name="related-articles"></a>Povezani članci
- [Atributi duplicirane ili koji nisu valjani onemogućuju Sinkroniziranje direktorija u sustavu Office 365](https://support.microsoft.com/en-us/kb/2647098)

### <a name="objecttypemismatch"></a>ObjectTypeMismatch
#### <a name="description"></a>Opis
Kada Azure AD pokuša mekim odgovaraju dva objekta, moguće je da dva objekta različitih "Vrsta objekta" (kao što je korisnik, grupa, kontakta itd.) imaju iste vrijednosti za atributi koji se koriste za izvođenje meki podudaranje. Kao što je dupliciranje tih atributa nije dopuštena u Azure AD, postupak može uzrokovati pogreške sinkronizacije "ObjectTypeMismatch".

#### <a name="example-scenarios-for-objecttypemismatch-error"></a>Ogledni scenariji za ObjectTypeMismatch pogreške
- Sigurnosne grupe pošte omogućen se stvara u sustavu Office 365. Administrator dodaje novog korisnika ili kontakt u lokalnoj AD (koja će se sinkronizirati još Azure AD) s istom vrijednošću atributa ProxyAddresses kao grupe sustava Office 365.

#### <a name="example-case"></a>Primjer slučaja

1. Administrator stvara novu grupu pošta omogućena sigurnost u sustavu Office 365 odjela porez za, a sadrži adresu e-pošte kao tax@contoso.com. To dodjeljuje atributa ProxyAddresses za ovu grupu s vrijednošću**smtp:tax@contoso.com**
2. Novi korisnik spaja Contoso.com i račun se stvara za korisnika lokalnog s proxyAddress kao**smtp:tax@contoso.com**
3. Kada Azure AD Connect će se sinkronizirati novog korisničkog računa, će se pogreška "ObjectTypeMismatch".

#### <a name="how-to-fix-objecttypemismatch-error"></a>Kako riješiti pogreške ObjectTypeMismatch
Najčešćih razloga pogreške ObjectTypeMismatch je dva objekta druge vrste (korisnika, grupa, kontakta itd.) imaju jednaku vrijednost atributa ProxyAddresses. Da biste riješili problem s ObjectTypeMismatch:

1.  Prepoznavanje dupliciranu proxyAddresses (ili druge atribut) vrijednost koja uzrokuje pogrešku. Također prepoznavanje koje dva \(ili više\) objekata uključeni su u sukobu. Izvješće generira [Azure AD povezivanje stanje sinkronizacije](https://aka.ms/aadchsyncerrors) možete lakše prepoznali dva objekta.
2. Odredite koji objekt moraju se i dalje imati dupliciranu vrijednost, a ne bi trebala koji objekt.
3. Uklonite dupliciranu vrijednost iz objekta koji treba imati tu vrijednost. Imajte na umu da treba unesite željene promjene u direktoriju u kojem je izvorni objekt termini iz. U nekim slučajevima možda ćete morati izbrisali jedan od objekata u sukobu.
4. Ako ste izvršili promjene u na lokalni AD omogućuju Azure AD Connect sinkronizirati promjene. Izvješće o pogrešci sinkronizacije unutar Azure AD povezivanje stanje sinkronizacije ažurira svakih 30 minuta, a obuhvaća pogreške od najnovijih pokušaj sinkronizacije.


## <a name="duplicate-attributes"></a>Duplikate atributa
### <a name="attributevaluemustbeunique"></a>AttributeValueMustBeUnique
#### <a name="description"></a>Opis
Azure Active Directory sheme omogućite dva ili više objekata imaju iste vrijednosti od sljedećih atributa. To je sve objekte u Azure AD mora imati jedinstveni vrijednost od ovih atributa na navedenom instancom.

- ProxyAddresses
- UserPrincipalName

Ako Azure AD Connect pokušava da biste dodali novi objekt ili ažurirati postojeće objekt s vrijednošću iznad atributa koji je već dodijeljen drugog objekta servisa Azure Active Directory, postupak uzrokovat će pogrešku "AttributeValueMustBeUnique".
#### <a name="possible-scenarios"></a>Moguće situacije:
1. Već sinkronizirane objekt koji je u sukobu s drugim objektom sinkronizirane je dodijeljen duplicirane vrijednosti.

#### <a name="example-case"></a>Primjer slučaj:
1. **Teo Novak** je sinkronizirane korisnik servisa Azure Active Directory iz lokalnog servisa Active Directory contoso.com
2. Teo Novak **UserPrincipalName** lokalno postavljeno kao **bobs@contoso.com**.
3. Teo sadrži i pratiti vrijednosti za atributa **proxyAddresses** :
    - smtp:bobs@contoso.com
    - smtp:bob.smith@contoso.com
    - **smtp:bob@contoso.com**
4. Novi korisnik **Teo Taylor**se dodaje na lokalni Active Directory.
5. Teo Taylor **UserPrincipalName** postavljeno kao **bobt@contoso.com**.
6. **Teo Taylor** ima sljedeće vrijednosti za atributa **ProxyAddresses** i. smtp:bobt@contoso.comii. smtp:bob.taylor@contoso.com
7. Objekt Teo Taylor sinkroniziraju s Azure AD uspješno.
8. Administrator odlučili da biste ažurirali atributa **ProxyAddresses** Teo Taylor pomoću sljedeće vrijednosti: li. **smtp:bob@contoso.com**
9. Azure AD će pokušati ažuriranje Teo Taylor objekta u Azure AD uz gornje vrijednosti, ali koje operacija neće uspjeti kao da vrijednost ProxyAddresses već je dodijeljen Teo Novak, čiji je rezultat pogreška "AttributeValueMustBeUnique".

#### <a name="how-to-fix-attributevaluemustbeunique-error"></a>Kako riješiti pogreške AttributeValueMustBeUnique
Najčešćih razloga pogreške AttributeValueMustBeUnique je dva objekte s različitim SourceAnchor \(immutableId\) imaju jednaku vrijednost atributa ProxyAddresses i/ili UserPrincipalName. Da biste ispravili pogrešku AttributeValueMustBeUnique

1.  Odredite dupliciranu proxyAddresses, userPrincipalName ili druga vrijednost atributa koji je uzrok pogreške. Također prepoznavanje koje dva \(ili više\) objekata uključeni su u sukobu. Izvješće generira [Azure AD povezivanje stanje sinkronizacije](https://aka.ms/aadchsyncerrors) možete lakše prepoznali dva objekta.
2. Odredite koji objekt moraju se i dalje imati dupliciranu vrijednost, a ne bi trebala koji objekt.
3. Uklonite dupliciranu vrijednost iz objekta koji treba imati tu vrijednost. Imajte na umu da treba unesite željene promjene u direktoriju u kojem je izvorni objekt termini iz. U nekim slučajevima možda ćete morati izbrisali jedan od objekata u sukobu.
4. Ako ste izvršili promjene u na lokalni AD omogućuju Azure AD Connect sinkronizirati promjene za pogrešku da biste dobili Fiksno.

#### <a name="related-articles"></a>Povezani članci
-[Duplicirane ili koji nisu valjani onemogućuju Sinkroniziranje direktorija u sustavu Office 365](https://support.microsoft.com/en-us/kb/2647098)


## <a name="data-validation-failures"></a>Neuspjele provjere valjanosti podataka
### <a name="identitydatavalidationfailed"></a>IdentityDataValidationFailed
#### <a name="description"></a>Opis
Azure Active Directory primjenjuje različite ograničenja same podatke prije nego što te podatke staviti u direktoriju. To je da biste bili sigurni da krajnji korisnici dobiti najbolje moguće sučelja prilikom korištenja aplikacije koje ovise o ovim podacima.
#### <a name="scenarios"></a>Scenariji
na. Vrijednost atributa UserPrincipalName sadrži nije valjan/nepodržane znakova.
b. Atribut UserPrincipalName slijedite potrebnom obliku.
#### <a name="how-to-fix-identitydatavalidationfailed-error"></a>Kako riješiti pogreške IdentityDataValidationFailed

na. Provjerite je li je podržano atribut userPrincipalName znakova i potrebnom obliku.

#### <a name="related-articles"></a>Povezani članci
- [Priprema za dodjelu resursa korisnicima pomoću sinkronizacije direktorija u sustavu Office 365]( https://support.office.com/en-us/article/Prepare-to-provision-users-through-directory-synchronization-to-Office-365-01920974-9e6f-4331-a370-13aea4e82b3e)


### <a name="datavalidationfailed"></a>DataValidationFailed
#### <a name="description"></a>Opis
To je vrlo konkretnih slučaj čiji je rezultat pogreška pri sinkronizaciji **"DataValidationFailed"** kada sufiks korisničke UserPrincipalName mijenja se iz jednog pridruženoj domeni drugi pridruženoj domeni.

#### <a name="scenarios"></a>Scenariji
Za sinkroniziranih korisnika, sufiks UserPrincipalName promijenio iz jednog pridruženoj domeni u drugom pridruženoj domeni lokalno. Na primjer, *UserPrincipalName = bob@contoso.com * promijenjen *UserPrincipalName = bob@fabrikam.com *.

#### <a name="example"></a>Primjer
1. Teo Novak, račun za Contoso.com, će dodani kao novog korisnika u servisu Active Directory s na UserPrincipalNamebob@contoso.com
2. Teo premješta različitih odjela Contoso.com pod nazivom Fabrikam.com i njegovog UserPrincipalName mijenja sebob@fabrikam.com
3. Domene contoso.com i fabrikam.com su pridruženim domenama s Azure Active Directory.
4. UserPrincipalName, Teo se ažurirati i javlja se pogreška za sinkronizaciju "DataValidationFailed".

#### <a name="how-to-fix"></a>Upute za rješavanje
Ako sufiks UserPrincipalName korisničke ažurirana iz bob@ **contoso.com** bob@ **fabrikam.com**, gdje **contoso.com** i **fabrikam.com** su **pridruženim domenama**, slijedite na ispod korake da biste ispravili pogreške pri sinkronizaciji

1. Ažuriranje UserPrincipalName korisnika u Azure AD iz bob@contoso.com da biste bob@contoso.onmicrosoft.com. Koristite sljedeću naredbu komponente PowerShell s modul Azure AD PowerShell:`Set-MsolUserPrincipalName -UserPrincipalName bob@contoso.com -NewUserPrincipalName bob@contoso.onmicrosoft.com`

2. Dopusti idućeg kruga sinkronizaciju pokušaja sinkronizacije. Ova sinkronizacije vrijeme neće biti uspješan i ažurirat će UserPrincipalName Teo da biste bob@fabrikam.com prema očekivanjima.


## <a name="largeobject"></a>LargeObject
### <a name="description"></a>Opis
Kada atribut premašuje ograničenje veličine, ograničenje duljine ili broj postavljenog prema shemi Azure Active Directory, postupak sinkronizacije uzrokovat će pogrešku **LargeObject** ili **ExceededAllowedLength** . Obično je ta se pogreška pojavljuje za sljedećim atributima

- userCertificate
- thumbnailPhoto
- proxyAddresses

### <a name="possible-scenarios"></a>Mogući scenariji
1. Atribut userCertificate Teo na je spremanje previše certifikati dodijeljene Teo. To može obuhvaćati i stariji, istekle certifikata.
2. ThmubnailPhoto Teo, postavite u servisu Active Directory je prevelik da bi se sinkronizirati u Azure AD.
3. Tijekom automatske populacije atributa ProxyAddresses u servisu Active Directory, imate dodijeljeni objekt > 500 ProxyAddresses.

### <a name="how-to-fix"></a>Upute za rješavanje

1. Provjerite je li atribut uzrokuje pogrešku unutar dopušteno ograničenje.

## <a name="related-links"></a>Srodne veze
- [Pronađite objektima servisa Active Directory u servisu Active Directory administracijski centar] (https://technet.microsoft.com/library/dd560661.aspx)
- [Kako postaviti upit Azure Active Directory za neki objekt pomoću komponente PowerShell Azure Active Directory](https://msdn.microsoft.com/library/azure/jj151815.aspx)
