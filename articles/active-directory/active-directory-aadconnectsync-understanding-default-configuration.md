<properties
    pageTitle="Azure AD Connect sinkronizacije: objašnjenje zadanu konfiguraciju | Microsoft Azure"
    description="U ovom se članku opisuju zadanu konfiguraciju u Azure AD Connect sinkronizaciju."
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
    ms.date="09/01/2016"
    ms.author="billmath"/>

# <a name="azure-ad-connect-sync-understanding-the-default-configuration"></a>Azure AD Connect sinkronizacije: objašnjenje zadanu konfiguraciju
U ovom se članku objašnjava konfiguriranje – okvir pravila. Ga dokumenti pravila i način utječe na ta pravila konfiguracije. Također vas vodi kroz zadanu konfiguraciju Azure AD Connect sinkronizacije. Cilj je čitač razumije kako funkcionira konfiguracije model, pod nazivom deklarativno dodjele resursa u stvarne primjeru. U ovom se članku pretpostavlja da ste već instalirali i konfiguriranje sinkronizacije Azure AD Connect pomoću čarobnjaka za instalaciju.

Da biste shvatili detalje o konfiguraciji model, pročitajte [Objašnjenje deklarativno dodjele resursa](active-directory-aadconnectsync-understanding-declarative-provisioning.md).

## <a name="out-of-box-rules-from-on-premises-to-azure-ad"></a>Pravila – okvir s lokalnim Azure AD
Sljedeći izrazi pronaći ćete u konfiguraciji izvan okvira.

### <a name="user-out-of-box-rules"></a>Korisnik – okvir pravila
Ta pravila primjenjuju na Vrsta objekta iNetOrgPerson.

Korisnički objekt mora zadovoljavati sljedeće da biste se sinkronizirati:

- Morate imati na sourceAnchor.
- Kad objekt u Azure AD, zatim sourceAnchor nije moguće promijeniti. Ako je vrijednost promijenjene lokalnog, objekt zaustavlja sinkroniziranje dok se ne promijeni u sourceAnchor povratak na prethodnu vrijednost.
- Morate imati atribut accountEnabled (kontrola korisničkih računa) unose. Lokalni Active Directory, taj atribut je uvijek izlaganje i popunjena.

Sljedeće objekte korisnika se **neće** sinkronizirati s Azure AD:

- `IsPresent([isCriticalSystemObject])`. Provjerite je li više objekata iz okvira u servisu Active Directory, kao što su ugrađeni administratorski račun, neće se sinkronizirati.
- `IsPresent([sAMAccountName]) = False`. Provjerite je li korisnik objekte s nema atribut sAMAccountName ne sinkroniziraju. U ovom slučaju bi samo gotovo dogoditi domene nadogradili NT4.
- `Left([sAMAccountName], 4) = "AAD_"`, `Left([sAMAccountName], 5) = "MSOL_"`. Sinkronizacija računa servisa koriste Azure AD Connect sinkronizaciju i njegove starije verzije.
- Računi za Exchange koji će funkcionirati u sustavu Exchange Online neće se sinkronizirati.
    - `[sAMAccountName] = "SUPPORT_388945a0"`
    - `Left([mailNickname], 14) = "SystemMailbox{"`
    - `(Left([mailNickname], 4) = "CAS_" && (InStr([mailNickname], "}") > 0))`
    - `(Left([sAMAccountName], 4) = "CAS_" && (InStr([sAMAccountName], "}")> 0))`
- Sinkronizacija objekata koji će funkcionirati u sustavu Exchange Online.
`CBool(IIF(IsPresent([msExchRecipientTypeDetails]),BitAnd([msExchRecipientTypeDetails],&H21C07000) > 0,NULL))`  
U ovom bit-maska kad (i H21C07000) bi filtriraju sljedeće objekte:
    - Podrškom za e-poštu javnim mapama
    - Automata poštanski sandučić sustava
    - Baza podataka poštanskog sandučića poštanski sandučić (poštanski sandučić sustava)
    - Univerzalni sigurnosne grupe (ne može primijeniti za korisnika, ali postoji naslijeđene razloga)
    - Osobe koje nisu univerzalno grupe (ne može primijeniti za korisnika, ali postoji naslijeđene razloga)
    - Planiranje poštanskog sandučića
    - Poštanskog sandučića otkrivanja
- `CBool(InStr(DNComponent(CRef([dn]),1),"\\0ACNF:")>0)`. Sinkronizirajte sve objekte žrtve replikacije.

Primjenjuju se sljedeća pravila atribut:

- `sourceAnchor <- IIF([msExchRecipientTypeDetails]=2,NULL,..)`. Atribut sourceAnchor nije pridonio iz povezanih poštanskog sandučića. Pretpostavlja se da ako je pronašao povezane poštanski sandučić stvarni račun pridruženo kasnije.
- Exchange odnose se atributi sinkroniziraju samo ako atribut **mailNickName** ima vrijednost.
- Ako postoji više šuma, atribute potrošiti sljedećim redoslijedom:
    1. Atributi vezane uz prijavu (na primjer userPrincipalName) su pridonio iz skupa stabala s omogućenom računom.
    2. Atributi koji se nalazi se u GAL sustava Exchange (globalni popis adresa) su pridonio iz skupa stabala s poštanskim sandučićem sustava Exchange.
    3. Ako nijedan poštanski sandučić možete pronaći, te atribute mogu potjecati iz bilo kojeg skupa stabala.
    4. Exchange povezane su atributa (Tehnički atributa nije vidljiva na globalnom popisu adresa) pridonio iz u skup stabala gdje `mailNickname ISNOTNULL`.
    5. Ako postoji više šuma koji bi ispunjavaju jedan od ta pravila, stvaranje redoslijeda (datuma/vremena) poveznika (šuma) se koristi da biste utvrdili koji skupa stabala doprinosa atribute.

### <a name="contact-out-of-box-rules"></a>Obratite se – okvir pravila
Kontaktu objekt mora zadovoljavati sljedeće da biste se sinkronizirati:

- Kontakt mora biti s omogućenom e-poštom. Provjerava je sa sljedećim pravilima:
    - `IsPresent([proxyAddresses]) = True)`. Moraju biti ispunjena atributa proxyAddresses.
    - Primarne adrese pronaći ćete u atributu proxyaddresses ili atribut pošta. Prisutnost u @ se koristi da biste provjerili je li sadržaj adresu e-pošte. Jedan od ta dva pravila morate vrednuje na True.
        - `(Contains([proxyAddresses], "SMTP:") > 0) && (InStr(Item([proxyAddresses], Contains([proxyAddresses], "SMTP:")), "@") > 0))`. Postoji li unos "SMTP:" i ako postoji, možete je @ pronađen u nizu?
        - `(IsPresent([mail]) = True && (InStr([mail], "@") > 0)`. Je atribut pošte popunjeni, a ako jest, možete je @ pronađen u nizu?

Sljedeći kontakt objekti su **neće** sinkronizirati s Azure AD:

- `IsPresent([isCriticalSystemObject])`. Provjerite je li nema kontakta objekata označene kao ključnih sinkroniziraju. Bi trebalo biti nešto sa zadanom konfiguracijom.
- `((InStr([displayName], "(MSOL)") > 0) && (CBool([msExchHideFromAddressLists])))`.
- `(Left([mailNickname], 4) = "CAS_" && (InStr([mailNickname], "}") > 0))`. Ti objekti ne funkcioniraju u sustavu Exchange Online.
- `CBool(InStr(DNComponent(CRef([dn]),1),"\\0ACNF:")>0)`. Sinkronizirajte sve objekte žrtve replikacije.

### <a name="group-out-of-box-rules"></a>Grupa-okvir pravila
Objekta grupe mora zadovoljavati sljedeće da biste se sinkronizirati:

- Morate imati manje od 50.000 članova. U ovom count je broj članova u grupi lokalnog.
    - Ako sadrži više članova prije sinkronizacije započinje prvi put, grupi nisu sinkronizirane.
    - Ako je broj članova šire iz prethodno stvaranja, zatim kada dođe do 50.000 članovi prestane sinkroniziranje dok count članstvo je manja od 50.000 ponovno.
    - Napomena: Count 50.000 članstva i postavio je Azure AD. Niste moći sinkronizirati grupe s više članova, čak i ako izmjena ili uklanjanje ovo pravilo.
- Ako je grupa **Za raspodjelu**, tada mora također biti pošte omogućen. Potražite u članku se provodi [kontaktu izvan okvira pravila](#contact-out-of-box-rules) za ovo pravilo.

Sljedeće Grupiraj objekte se **neće** sinkronizirati s Azure AD:

- `IsPresent([isCriticalSystemObject])`. Provjerite mnogo objekte iz okvira u servisu Active Directory, kao što su grupe administratora ugrađene, ne sinkroniziraju.
- `[sAMAccountName] = "MSOL_AD_Sync_RichCoexistence"`. Grupa za stari koristi DirSync.
- `BitAnd([msExchRecipientTypeDetails],&amp;H40000000)`. Grupe uloga.
- `CBool(InStr(DNComponent(CRef([dn]),1),"\\0ACNF:")>0)`. Sinkronizirajte sve objekte žrtve replikacije.

### <a name="foreignsecurityprincipal-out-of-box-rules"></a>ForeignSecurityPrincipal – okvir pravila
"Nešto" pridruženo FSPs (\*) objekt u na metaverse. U stvari, ova spoj pojavljuje samo za korisnike i sigurnosne grupe. Tu konfiguraciju osigurava unakrsno-skupa stabala članstva su riješiti i predstavljeni pravilno u Azure AD.

### <a name="computer-out-of-box-rules"></a>Računalo-okvir pravila
Objekt programa računalo mora zadovoljavati sljedeće da biste se sinkronizirati:

- `userCertificate ISNOTNULL`. Samo računala za Windows 10 popunite ovaj atribut. Svi objekti na računalu s vrijednošću u taj atribut sinkroniziraju.

## <a name="understanding-the-out-of-box-rules-scenario"></a>Objašnjenje scenarij – okvir pravila
U ovom primjeru smo koriste implementacija s jednog skupa stabala račun (A), jedan skupa stabala resursa (R) i jedan Azure AD direktorija.

![Slika s opisom scenarija](./media/active-directory-aadconnectsync-understanding-default-configuration/scenario.png)

U ovoj konfiguraciji, pretpostavlja se da je postoji račun omogućeni u skup stabala računa i onemogućeni račun u skupa stabala resursa s poštanskim sandučićem web povezani.

Naš cilj sa zadanom konfiguracijom je:

- Vezane uz prijavi se atributi sinkroniziraju se iz skupa stabala s omogućenom računom.
- Atributi koji se nalazi se na globalnom popisu adresa (globalni popis adresa) se sinkroniziraju iz skupa stabala s poštanskim sandučićem. Ako nijedan poštanski sandučić možete pronaći, koristit će se drugi skup stabala.
- Ako se pronađe povezane poštanskog sandučića, objekta koji želite izvesti Azure AD se povezani poslovni subjekt omogućeno pronađen.

### <a name="synchronization-rule-editor"></a>Uređivač pravila za sinkronizaciju
Konfiguracija možete prikazati i promijeniti pomoću alata za sinkronizaciju pravila Editor (SRE) i prečac do nje nalazi se na izborniku start.

![Sinkronizacija pravila uređivač ikona](./media/active-directory-aadconnectsync-understanding-default-configuration/sre.png)

U SRE je komplet alata resursa i njegove instalacije za Azure AD Connect sinkronizaciju. Da biste mogli da biste ga pokrenuli, morate biti član grupe ADSyncAdmins. Kada pokrene, vidjet ćete otprilike ovako:

![Ulazna pravila za sinkronizaciju](./media/active-directory-aadconnectsync-understanding-default-configuration/syncrulesinbound.png)

U oknu, pogledajte sva sinkronizacije pravila za konfiguraciju stvara. Svaki redak u tablici je jedno pravilo za sinkronizaciju. S lijeve strane u odjeljku vrste pravila navedene su dvije vrste: ulazni i izlazni. Ulazni i izlazni je iz prikaza na metaverse. Uglavnom namjeravate fokusu na ulazna pravila u ovom pregled. Stvarni popisa pravila sinkronizacije ovisi o shemi otkriven u AD. Na gornjoj slici, skup stabala račun (fabrikamonline.com) nema servisa, kao što su Exchange i Lync, i pravila za sinkronizaciju stvorene za te servise. No u skupa stabala resursa (res.fabrikamonline.com) pronađete sinkronizacije pravila za te servise. Sadržaj pravila razlikuje se ovisno o verziji otkrivena. Na primjer, u implementaciji sustava Exchange 2013 postoje više atribut tokova konfiguriran od u Exchange 2010 i 2007.

### <a name="synchronization-rule"></a>Sinkronizacija pravila
Pravilo sinkronizacije je objekt konfiguracije sa skupom atributa slijedi kada se zadovolje uvjet. Koristi se i opisuje način objekta u razmak poveznik povezan s objekta u metaverse, poznato pod nazivom **spoj** ili **podudaraju**. Pravila za sinkronizaciju imaju prednost vrijednost koja ukazuje kakav je odnos između međusobno povezani. Pravilo za sinkronizaciju s donjem numeričku vrijednost ima veće prednost i u atribut tijek sukoba, veća prednost wins Razrješenje sukoba.

Na primjer, pogledajte pravilo za sinkronizaciju **u iz AD – AccountEnabled korisnika**. Označavanje retkom u u SRE, a zatim odaberite **Uređivanje**.

Budući da to pravilo je pravilo – okvir, pojavit će se upozorenje kada otvorite pravilo. Napravite [promjene – okvir pravila](active-directory-aadconnectsync-best-practices-changing-default-configuration.md)nisu tako da vas se upita koji su vašim potrebama. U ovom slučaju samo želite prikazati pravilo. Odaberite " **ne**".

![Sinkronizacija pravila upozorenje](./media/active-directory-aadconnectsync-understanding-default-configuration/warningeditrule.png)

Sinkronizacija pravilo ima četiri odjeljka konfiguracije: opis, određivanje opsega filtar, uključite pravila i transformacije.

#### <a name="description"></a>Opis
Prvi dio sadrži osnovne podatke kao što su naziv i opis.

![Opis kartica uređivač pravila u sinkronizaciji ](./media/active-directory-aadconnectsync-understanding-default-configuration/syncruledescription.png)

Također, pronaći informacije o koji povezanog sustav to pravilo odnosi, koji objekt vrsta u sustavu povezanog se primjenjuje i Vrsta objekta metaverse. Vrsta objekta metaverse uvijek je osoba bez obzira na to kada je vrsta objekta izvora korisnika, iNetOrgPerson ili kontakt. Vrsta objekta metaverse nikad trebali biste promijeniti tako da se stvara se kao vrstu Generička. Vrsta veze moguće je postaviti za uključivanje, StickyJoin ili dodjele resursa. Ova postavka funkcionira zajedno s odjeljku pravila za uključivanje i kasnije prekriveno.

Možete vidjeti i ovo pravilo za sinkronizaciju služi za sinkronizaciju lozinke. Ako je korisnik u opsegu za ovo pravilo za sinkronizaciju, lozinku sinkronizira se s lokalnim oblak (Ako ste omogućili značajku sinkronizacija lozinku).

#### <a name="scoping-filter"></a>Određivanje opsega filtra
U odjeljku filtar za određivanje opsega koristi se za konfiguraciju vrijeme primjene pravila sinkronizacije. Budući da se naziv pravila sinkronizacije gledate označava moraju se primijeniti samo omogućeni korisnici, opsega konfiguriran tako da u AD atribut **kontrola korisničkih računa** ne smije imati bitne 2 postavljanje. Kada modula za sinkroniziranje pronađe korisnika u AD, ona se primjenjuje ovo pravilo za sinkronizaciju kada **kontrola korisničkih računa** postavljen na decimalnu vrijednost 512 (omogućeno običan korisnik). Ako korisnik ima **kontrola korisničkih računa** postavljena na 514 (onemogućene običan korisnik) ga ne primjenjuje pravilo.

![Određivanje opsega kartica u uređivaču pravila za sinkronizaciju ](./media/active-directory-aadconnectsync-understanding-default-configuration/syncrulescopingfilter.png)

Scoping filtar ima grupe i rečenice koje može se ugnijezditi. Sve rečenice unutar grupe moraju biti zadovoljeni primijenilo pravilo o sinkronizacije. Kada definirate više grupa, barem jedna grupa mora zadovoljavati primijenilo pravilo. To jest, logičkim ili je izračunat između grupa i u logičke i procjenjuje unutar grupe. Primjer tu konfiguraciju pronaći ćete u na izlazni sinkronizacije pravilo **Odjava AAD – uključivanje grupe**. Postoji nekoliko filtar grupe za sinkronizaciju, na primjer jedan za sigurnosnih grupa (`securityEnabled EQUAL True`) i jedan za raspodjelu (`securityEnabled EQUAL False`).

![Određivanje opsega kartica u uređivaču pravila za sinkronizaciju ](./media/active-directory-aadconnectsync-understanding-default-configuration/syncrulescopingfilterout.png)

Tim se pravilom koristi se za definiranje grupa koje mora biti dodijeljena da Azure AD. Grupa za raspodjelu mora biti pošte omogućen za sinkronizaciju s Azure AD, ali za sigurnosne grupe poruke e-pošte nije obavezno.

#### <a name="join-rules"></a>Uključivanje pravila
Treći dio koristi se za konfiguraciju objekata u prostoru poveznik odnosa objekata na metaverse. Pravilo koje ste gledali ranije imati sve konfiguraciju za uključivanje pravila tako da umjesto namjeravate pogledajte **u iz AD – uključivanje korisnika**.

![Uključivanje kartice pravila u uređivaču pravila za sinkronizaciju ](./media/active-directory-aadconnectsync-understanding-default-configuration/syncrulejoinrules.png)

Sadržaj pravilo spoj ovisi o odgovarajuću mogućnost odabrali u čarobnjaku za instalaciju. Za pravilo ulazne analizu započinje objekta iz izvorišnog prostora poveznika i procjenjuje svake grupe u pravilima za uključivanje u nizu. Ako izvorni objekt procjene tako da odgovara točno jedan objekt u metaverse pomoću jednog od pravila spoj, spojeni na objekte. Ako ste analizira sva pravila, a ne postoji podudarnost, koristi se vrsta veze na stranici opis. Ako tu konfiguraciju je postavljena za **dodjelu resursa**, novi objekt se stvara u ciljnom na metaverse. Dodjela novi objekt da biste na metaverse se zove **projekt** objekta u metaverse.

Pravila za uključivanje samo jednom vrednuje. Kada se pridružite objekt programa poveznik prostora i metaverse objekta, ostat će spojene dok god je i dalje zadovoljni djelokrug pravila sinkronizacije.

Prilikom procjene pravila za sinkronizaciju samo jedno pravilo za sinkronizaciju s pravilima spoj definirani mora biti u opsegu. Ako više pravila za sinkronizaciju s pravilima spoj nalaze se za jedan objekt, pogreška će se Expression.Error. Zbog toga, najbolje je da bi samo jedno pravilo za sinkronizaciju s spoj definirani kada više pravila za sinkronizaciju u opsegu za objekt. U konfiguraciji – okvir za sinkronizaciju Azure AD Connect ta pravila možete pronaći tako da pogledate naziv i pronaći one s programom word na kraju naziv **uključiti** . Pravilo sinkronizacije bez bilo koji spoj pravila definiranih primjenjuje tokova atribut kada drugo pravilo sinkronizacije zajedno pridruženo objekte ili dodjeli novi objekt u ciljnom.

Ako pogledate gornjoj slici, moći ćete vidjeti pravilo pokušava uključivanje **objectSID** s **msExchMasterAccountSid** (Exchange) i **msRTCSIP OriginatorSid** (Lync), koji je očekivanog u skup stabala topologija račun resursa. Pronađite isti pravilo na sve šuma. Pretpostavlja se da svaki skup stabala možda nije skupa stabala poslovnim subjektom ili resursa. Tu konfiguraciju funkcionira i ako imate račune koji žive u jedan skup stabala nemaju koji se spajaju.

#### <a name="transformations"></a>Transformacije
U odjeljku transformacija definira svih atributa tokova koji se odnose na ciljnog objekta kada su povezani objekti i zadovoljeni opseg filtar. Vraćanje na **u iz AD – AccountEnabled korisnika** pravilo sinkronizacije Traži sljedeće transformacije:

![Transformacije kartica uređivač pravila u sinkronizaciji ](./media/active-directory-aadconnectsync-understanding-default-configuration/syncruletransformations.png)

Da biste postavili tu konfiguraciju u kontekstu u implementacije sustava skupa stabala račun resursa očekuje da biste pronašli račun omogućeni u skup stabala računa i onemogućeni račun u skupa stabala resursa s postavkama sustava Exchange i Lync. Pravilo za sinkronizaciju gledate sadrži informacije potrebne za prijavu i tih atributa treba kreću se od u skup stabala gdje je omogućen račun. Ove atribut tokova smještaju zajedno u jedno pravilo za sinkronizaciju.

Transformaciju može imati različite vrste: konstantu, izravno i izraz.

- Konstanta tijek uvijek teče koji nisu na vrijednost. U slučaju iznad, uvijek postavlja vrijednost **True** u metaverse atributa pod nazivom **accountEnabled**.
- Izravni tijek uvijek teče vrijednost atributa u izvoru ciljni atribut kao-je.
- Treći vrsta tijek izraza i omogućuje za naprednije konfiguracije.

Jezik izraz je VBA (Visual Basic for Applications), pa osobe doživljaj sustava Microsoft Office ili VBScript prepoznat će oblik. Atributi bit će u uglatim zagradama, [attributeName]. Atribut nazivi i nazivi funkcija se velika i mala slova, ali uređivaču pravila sinkronizacije procjenjuje izrazi i ponuditi upozorenje ako izraz nije valjan. Svi izrazi su izražena u jedan redak s ugniježđenim funkcijama. Da biste prikazali power konfiguriranje jezika, Evo tijek pwdLastSet, ali s dodali još komentara umetnuti:

```
// If-then-else
IIF(
// (The evaluation for IIF) Is the attribute pwdLastSet present in AD?
IsPresent([pwdLastSet]),
// (The True part of IIF) If it is, then from right to left, convert the AD time format to a .Net datetime, change it to the time format used by Azure AD, and finally convert it to a string.
CStr(FormatDateTime(DateFromNum([pwdLastSet]),"yyyyMMddHHmmss.0Z")),
// (The False part of IIF) Nothing to contribute
NULL
)
```

Dodatne informacije o jeziku izraz za tijekove atribut potražite u članku [Objašnjenje deklarativno izraza dodjele resursa](active-directory-aadconnectsync-understanding-declarative-provisioning-expressions.md) .

### <a name="precedence"></a>Prednost
Sada gledali neka pravila za pojedinačne sinkronizacije, ali pravila suradnja u konfiguraciji. U nekim slučajevima vrijednost atributa pridonio s više pravila za sinkronizaciju da biste iste ciljni atribut. U ovom slučaju prednost atribut se koristi da biste utvrdili koji atribut pobjeđuje. Na primjer, pogledajte sourceAnchor atribut. Taj atribut je važno atribut da biste se mogli prijaviti na Azure AD. Za taj atribut u dva različita sinkronizacije pravila, možete pronaći u tijeku atribut **u iz AD – korisnika AccountEnabled** i **u iz AD – uobičajeni korisnik**. Zbog prvenstva pravila sinkronizacije, atribut sourceAnchor je pridonio iz skupa stabala s računom omogućeno prvi put kada postoji nekoliko objekata koji se spajaju metaverse objekt. Ako nema omogućeno računa, a zatim modula za sinkroniziranje koristi pravila sinkronizacije zamka sve **u iz AD – uobičajeni korisnik**. Tu konfiguraciju osigurava čak i za račune koji su onemogućene, ima li i dalje je sourceAnchor.

![Ulazna pravila za sinkronizaciju](./media/active-directory-aadconnectsync-understanding-default-configuration/syncrulesinbound.png)

Prednost za pravila za sinkronizaciju postavlja se u grupe pomoću čarobnjaka za instalaciju. Sva pravila u grupi imaju isti naziv, no oni povezani s različitim povezanog direktorija. Čarobnjak za instalaciju daje pravilo **u iz AD – uključivanje korisnika** najveći prioritet i zadanu ponovno izračunava sve postavite pokazivač povezani AD direktorija. Zatim se nastavlja na sljedeće grupe pravila unaprijed određenim redoslijedom. Unutar grupe, pravila dodaju se u redoslijedu poveznika dodani u čarobnjaku. Ako drugi poveznik dodaje se kroz čarobnjak, pravila za sinkronizaciju se ponovno naručiti, a novi poveznik pravila umeću zadnje u svakoj grupi.

### <a name="putting-it-all-together"></a>Izgradnja je sve
Ne možemo sada upoznati dovoljno o pravila za sinkronizaciju da biste mogli shvatiti način funkcioniranja konfiguraciju s različitim pravila za sinkronizaciju. Ako pogledate korisnika i atributa koji su pridonio na metaverse pravila primjenjuju se sljedećim redoslijedom:

Ime | Komentar
:------------- | :-------------
U iz AD – uključivanje korisnika | Pravilo za pridruživanje poveznik prostora objekte s metaverse.
U iz AD – omogućeno korisnički račun | Informacije potrebne za prijavu za Azure AD i Office 365. Želimo tih atributa s omogućenom računa.
U iz AD – zajednički korisnika iz sustava Exchange | Atributi koji se nalaze u globalni popis adresa. Pretpostavimo da kvalitete podataka najbolje je u skup stabala gdje ste naišli korisnikovog poštanskog sandučića.
U iz AD – zajednički korisnika | Atributi koji se nalaze u globalni popis adresa. U slučaju Nismo pronašli poštanskog sandučića, bilo koji drugi spojene objekt mogu pridonijeti vrijednost atributa.
U iz AD – korisnika sustava Exchange | Postoji samo ako je otkriven je sustava Exchange. Teče sve atribute Infrastruktura sustava Exchange.
U iz AD – korisnika programa Lync | Postoji samo ako je Lync otkrivena. Teče sve atribute Lync infrastrukture.

## <a name="next-steps"></a>Daljnji koraci

- Dodatne informacije o model konfiguraciju u [Razumijevanje deklarativno dodjele resursa](active-directory-aadconnectsync-understanding-declarative-provisioning.md).
- Saznajte više o jeziku izraz u [Razumijevanje deklarativno izraza dodjele resursa](active-directory-aadconnectsync-understanding-declarative-provisioning-expressions.md).
- Nastaviti s čitanjem funkcioniranje konfiguracija – okvir [korisnici razumijevanje](active-directory-aadconnectsync-understanding-users-and-contacts.md) i kontakata
- Saznajte kako izraditi praktično promjenu pomoću deklarativno Dodjela resursa u [kako promijeniti zadanu konfiguraciju](active-directory-aadconnectsync-change-the-configuration.md).

**Pregled tema**

- [Azure AD Connect sinkronizacije: razumijevanje i Prilagodba sinkronizacije](active-directory-aadconnectsync-whatis.md)
- [Integriranje sustava lokalnih identiteta sa Azure Active Directory](active-directory-aadconnect.md)
