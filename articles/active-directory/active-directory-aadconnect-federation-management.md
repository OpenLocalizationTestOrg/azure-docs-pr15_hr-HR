<properties
    pageTitle="Active Directory Federation Services Management i prilagodbu s Azure AD Connect | Microsoft Azure"
    description="Upravljanje AD FS s Azure AD Connect i prilagodbu AD FS prijavu doživljaja rada s Azure AD Connect i PowerShell."
    keywords="AD FS, ADFS, AD FS upravljanje AAD povezati, povezivanje, prijavu, AD FS prilagodbu, popravite pouzdanost, O365, vanjski pristup, potrebe za oslanjanjem proizvođača"
    services="active-directory"
    documentationCenter=""
    authors="anandyadavmsft"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/01/2016"
    ms.author="anandy"/>

# <a name="active-directory-federation-services-management-and-customization-with-azure-ad-connect"></a>Active Directory Federation Services upravljanje i prilagođavanje s Azure AD Connect

U ovom članku pojedinosti zadataka vezanih uz Active Directory Federation Services (AD FS) koje se mogu izvršiti pomoću Microsoft Azure Active Directory povezivanje, uz druge uobičajene zadatke za AD FS koji mogu biti potrebni za dovršavanje konfiguraciju farme poslužitelja za AD FS.

| Tema | Što obuhvaća |
|:------|:-------------|
|**Upravljanje AD FS**|
|[Popravak Vjeruj](#repairthetrust)| Popravak pouzdanost vanjski pristup sustavu Office 365 |
|[Dodavanje poslužitelj za ADFS](#addadfsserver) | Proširivanje farmu AD FS s dodatne poslužitelj za ADFS|
|[Dodavanje proxy poslužitelj servisa AD FS web aplikacije](#addwapserver) | Proširivanje farmu AD FS s dodatni WAP poslužitelj|
|[Dodavanje pridruženoj domeni](#addfeddomain)| Dodavanje pridruženoj domeni|
| **Prilagodba AD FS**|
|[Dodajte prilagođeni logotip ili slika](#customlogo)| Prilagodba programa AD FS stranica za prijavu u s logotipa tvrtke i slika |
|[Dodavanje opisa za prijavu](#addsignindescription) | Dodavanje stranica za prijavu u opis |
|[Izmjena pravila zahtjeva za AD FS](#modclaims) | Izmjena zahtjeva za AD FS različitim scenarijima za vanjski pristup za |

## <a name="ad-fs-management"></a>Upravljanje AD FS

Azure AD Connect nudi razne Zadaci vezani uz AD FS koje možete izvršiti pomoću čarobnjaka za Azure AD Connect intervencije minimalnog korisnika. Nakon dovršetka instalacije Azure AD Connect tako da pokrenete čarobnjak, možete pokrenuti čarobnjak koji će se izvršavati dodatne zadatke.

### Popravak Vjeruj<a name=repairthetrust></a>

Azure AD Connect možete potražiti trenutno stanje pouzdanost AD FS i Azure Active Directory i poduzeti odgovarajuće akcije da biste popravili Vjeruj. Slijedite ove korake da biste popravili Azure AD i pouzdanost AD FS.

1. Odaberite **Popravi AAD i ADFS pouzdanim** popisa dodatne zadatke.
![Popravak AAD i ADFS pouzdanosti](media\active-directory-aadconnect-federation-management\RepairADTrust1.PNG)

2. Na stranici **Povezivanje s Azure AD** unijeti vjerodajnice globalni administrator za Azure AD, a zatim kliknite **Dalje**.
![Povezivanje s Azure AD](media\active-directory-aadconnect-federation-management\RepairADTrust2.PNG)

3. Na stranici **vjerodajnica za daljinski pristup** unesite vjerodajnice za administrator domene.
![Vjerodajnice za daljinski pristup](media\active-directory-aadconnect-federation-management\RepairADTrust3.PNG)

    Nakon što kliknete **Dalje**, Azure AD Connect će potražiti stanja certifikat i prikaz problema.

    ![Stanje certifikata](media\active-directory-aadconnect-federation-management\RepairADTrust4.PNG)

    **Jeste li spremni za konfiguriranje** stranice prikazat će popis akcija koji će biti obavljeni da biste popravili Vjeruj.

    ![Jeste li spremni za konfiguraciju](media\active-directory-aadconnect-federation-management\RepairADTrust5.PNG)

4. Kliknite da biste popravili Vjeruj **instalirati** .

>[AZURE.NOTE] Azure AD Connect mogu samo popravak ili čin na certifikate koji se samopotpisani. Azure AD Connect nije moguće popravite certifikate drugih proizvođača.

### Dodavanje poslužitelj za ADFS<a name=addadfsserver></a>

> [AZURE.NOTE] Azure AD Connect zahtijeva datoteka certifikata PFX da biste dodali poslužitelj za AD FS. Stoga ovog postupka možete izvršiti samo ako ste konfigurirali na farmi poslužitelja za AD FS pomoću Azure AD Connect.

1. Odaberite **uvođenja dodatne vanjskom poslužitelju** , a zatim kliknite **Dalje**.
![Dodatni vanjskom poslužitelju](media\active-directory-aadconnect-federation-management\AddNewADFSServer1.PNG)

2. Na stranici **Povezivanje s Azure AD** za Azure AD unesite vjerodajnice globalni administrator, a zatim kliknite **Dalje**.
![Povezivanje s Azure AD](media\active-directory-aadconnect-federation-management\AddNewADFSServer2.PNG)

3. Navedite domenu administratorske vjerodajnice.
![Vjerodajnice administratora domene](media\active-directory-aadconnect-federation-management\AddNewADFSServer3.PNG)

4. Azure AD Connect će zatražiti lozinka PFX datoteku koju ste naveli prilikom konfiguriranja nove AD FS farmi s Azure AD Connect. Kliknite **Unos lozinke** možete unijeti lozinku za datoteku PFX.
![Potvrda lozinke](media\active-directory-aadconnect-federation-management\AddNewADFSServer4.PNG)

    ![Odredite SSL certifikata](media\active-directory-aadconnect-federation-management\AddNewADFSServer5.PNG)

5. Na stranici **AD FS poslužitelja** unesite naziv poslužitelja ili IP adresu koja će se dodati farmu AD FS.
![AD FS poslužitelja](media\active-directory-aadconnect-federation-management\AddNewADFSServer6.PNG)

6. Kliknite **Dalje** , a proći kroz posljednjoj stranici **Konfiguracija** . Kada završi Azure AD Connect dodavanje poslužitelji u farmi poslužitelja za AD FS, imat ćete mogućnost da biste provjerili spajanje.
![Jeste li spremni za konfiguraciju](media\active-directory-aadconnect-federation-management\AddNewADFSServer7.PNG)

    ![Instalacija je dovršena](media\active-directory-aadconnect-federation-management\AddNewADFSServer8.PNG)

### Dodavanje proxy poslužitelj servisa AD FS web aplikacije<a name=addwapserver></a>

> [AZURE.NOTE] Azure AD Connect zahtijeva datoteka certifikata PFX da biste dodali web-aplikacije proxy poslužitelj. Zbog toga moći za izvođenje ovog postupka samo ako ste konfigurirali na farmi poslužitelja za AD FS pomoću Azure AD Connect.

1. **Implementacija Proxy Web aplikacije** odaberite s popisa dostupnih zadataka.
![Implementacija web proxy aplikacije](media\active-directory-aadconnect-federation-management\WapServer1.PNG)

2. Unesite vjerodajnice za Azure globalni administrator.
![Povezivanje s Azure AD](media\active-directory-aadconnect-federation-management\wapserver2.PNG)

3. Na stranici **Određivanje SSL certifikat** unesite lozinku za PFX datoteku koju ste naveli prilikom konfiguriranja farme poslužitelja za AD FS s Azure AD Connect.
![Potvrda lozinke](media\active-directory-aadconnect-federation-management\WapServer3.PNG)

    ![Odredite SSL certifikata](media\active-directory-aadconnect-federation-management\WapServer4.PNG)

4. Dodajte poslužitelju koji želite dodati kao proxy aplikacije za web. Jer web-aplikacije proxy poslužitelj možda se spajaju na domenu, čarobnjak će vas pitati administratorske vjerodajnice za poslužitelj dodani.
![Vjerodajnice administratora poslužitelja](media\active-directory-aadconnect-federation-management\WapServer5.PNG)

5. Na stranici **Proxy pouzdanost vjerodajnice** pružaju administratorske vjerodajnice za konfiguriranje pouzdanost proxy i pristup primarni poslužitelj na farmi poslužitelja za AD FS.
![Vjerodajnice za pouzdanost proxy poslužitelj](media\active-directory-aadconnect-federation-management\WapServer6.PNG)

6. Na stranici **spremno za konfiguraciju** čarobnjak prikazuje na popisu akcija koji će biti obavljeni.
![Jeste li spremni za konfiguraciju](media\active-directory-aadconnect-federation-management\WapServer7.PNG)

7. Kliknite da biste dovršili konfiguraciju **instalirati** . Po dovršetku konfiguracije Čarobnjak nudi mogućnost da biste provjerili povezivanje s poslužiteljima. Kliknite **Provjeri** da biste provjerili povezivanje.
![Instalacija je dovršena](media\active-directory-aadconnect-federation-management\WapServer8.PNG)

### Dodavanje pridruženoj domeni<a name=addfeddomain></a>

Jednostavno dodati domenu ako želim vanjskog s Azure AD pomoću Azure AD Connect je. Azure AD Connect dodaje domene za vanjski pristup, a mijenja pravila zahtjeva za kad imate više domena povezani Azure AD odražava izdavač.

1. Da biste dodali pridruženoj domeni, odaberite zadatak **Dodavanje dodatnih Azure AD domene**.
![Dodatni Azure AD domene](media\active-directory-aadconnect-federation-management\AdditionalDomain1.PNG)

2. Na sljedećoj stranici čarobnjaka unijeti vjerodajnice globalni administrator za Azure AD.
![Povezivanje s Azure AD](media\active-directory-aadconnect-federation-management\AdditionalDomain2.PNG)

3. Na stranici **vjerodajnica za daljinski pristup** navedite domenu administratorske vjerodajnice.
![Vjerodajnice za daljinski pristup](media\active-directory-aadconnect-federation-management\additionaldomain3.PNG)

4. Na sljedećoj stranici čarobnjaka pronaći ćete popis domena Azure AD kojima možete združivanje lokalnog imenika. Na popisu odaberite domenu.
![Azure AD domene](media\active-directory-aadconnect-federation-management\AdditionalDomain4.PNG)

    Nakon što odaberete domenu, čarobnjak će vam pružiti odgovarajuće podatke o učinak konfiguracije i dodatne akcije koje će se čarobnjak. U nekim slučajevima, ako odaberete domenu koja nije niste potvrdili u Azure AD čarobnjak će vam pružiti informacije pomoću kojih ste potvrdili domenu. Dodatne informacije potražite u članku [Dodavanje prilagođeni naziv domene Azure Active Directory](active-directory-add-domain.md) .

5. Kliknite **Dalje**, te stranici **spreman za konfiguriranje** će prikazati popis akcija koje ćete izvršiti Azure AD Connect. Kliknite da biste dovršili konfiguraciju **instalirati** .
![Jeste li spremni za konfiguraciju](media\active-directory-aadconnect-federation-management\AdditionalDomain5.PNG)

## <a name="ad-fs-customization"></a>Prilagodba AD FS

Sljedeći odjeljci sadrže detalje o neke uobičajene zadatke koje možda ćete morati izvršiti prilikom prilagodbe AD FS prijava stranice.

### Dodajte prilagođeni logotip ili slika<a name=customlogo></a>

Da biste promijenili logotipa tvrtke koji se prikazuje na stranici za **prijavu** , koristite sljedeći cmdlet komponente Windows PowerShell i sintakse.

> [AZURE.NOTE] Preporučena dimenzije logotipa sustava su 260 x 35 @ 96 dpi veličina datoteke nije veći od 10 KB.

    Set-AdfsWebTheme -TargetName default -Logo @{path="c:\Contoso\logo.PNG"}

> [AZURE.NOTE] Potreban je parametar *TargetName* . Zadana tema koje je izdao AD fs pod nazivom zadani.


### Dodavanje opisa za prijavu<a name=addsignindescription></a>

Da biste dodali opis stranice za prijavu na **stranicu za prijavu**, koristite sljedeći cmdlet komponente Windows PowerShell i sintakse.

    Set-AdfsGlobalWebContent -SignInPageDescriptionText "<p>Sign-in to Contoso requires device registration. Click <A href='http://fs1.contoso.com/deviceregistration/'>here</A> for more information.</p>"

### Izmjena pravila zahtjeva za AD FS<a name=modclaims></a>

AD FS podržava obogaćeni zahtjeva jezika koje možete koristiti da biste stvorili prilagođeni zahtjeva pravila. Dodatne informacije potražite u članku [U ulozi pravilo jezika zahtjeva](https://technet.microsoft.com/library/dd807118.aspx).

U sljedećim odjeljcima opisuju kako možete upisati prilagođeni pravila za neke scenarije vezani uz u Azure AD i vanjski pristup za AD FS.

#### <a name="immutable-id-conditional-on-a-value-being-present-in-the-attribute"></a>Uvjetno na vrijednosti u sustavu atribut immutable ID

Azure AD Connect omogućuje vam da odredite atribut će se koristiti kao izvor sidrenja objekti su sinkronizirani s Azure AD. Ako je vrijednost u prilagođeni atribut prazan, možda ćete morati izdati immutable zahtjeva za ID-a. Ako, na primjer, ne odaberete **ms-ds-consistencyguid** kao atribut za sidro izvora, a želite izdati **ImmutableID** kao **ms-ds-consistencyguid** u slučaju atribut ima vrijednost na temelju ga. Ako nema vrijednosti na temelju atribut, problema **objectGuid** kao immutable ID-a.  Možete sastaviti skup pravila za prilagođene zahtjeva kao što je opisano u sljedećoj sekciji.

**Pravilo 1: Atributi upit**

    c:[Type == "http://schemas.microsoft.com/ws/2008/06/identity/claims/windowsaccountname"]
    => add(store = "Active Directory", types = ("http://contoso.com/ws/2016/02/identity/claims/objectguid", "http://contoso.com/ws/2016/02/identity/claims/msdsconcistencyguid"), query = "; objectGuid,ms-ds-consistencyguid;{0}", param = c.Value);

U tim se pravilom dohvaćate vrijednosti **ms-ds-consistencyguid** i **objectGuid** za korisnika iz servisa Active Directory. Promijenite naziv pohrane u imenu odgovarajuće pohrane u implementaciji sustava AD FS. Promijeniti vrstu proper zahtjevima za vanjski pristup onako kako su definirana za **objectGuid** i **ms-ds-consistencyguid**i vrstu zahtjevima.

Uz to, pomoću **dodati** i nije **problem**koji izbjegavajte dodavati odlazne problem za entitet i kao posredna vrijednosti možete koristiti vrijednosti. Zahtjeva u pravilu kasnije će izdati kada uspostavite vrijednost koja želite koristiti kao immutable ID-a.

**Pravilo 2: Provjerite postoji li ms-ds-consistencyguid za korisnika**

    NOT EXISTS([Type == "http://contoso.com/ws/2016/02/identity/claims/msdsconcistencyguid"])
    => add(Type = "urn:anandmsft:tmp/idflag", Value = "useguid");

Tim se pravilom definira zastavicu privremene pod nazivom **idflag** koje je postavljeno na **useguid** ako nema **ms-ds-concistencyguid** popunjeno za korisnika. Logika iza to je fact AD FS Dopusti prazan zahtjevima. Tako da kada dodate zahtjevima http://contoso.com/ws/2016/02/identity/claims/objectguid i http://contoso.com/ws/2016/02/identity/claims/msdsconcistencyguid u pravilo 1, koje završetka prema gore s **msdsconsistencyguid** zahtjeva samo ako je vrijednost popunjen za korisnika. Ako nije popunjeno, AD FS vidi je da će imati praznu vrijednost i odmah ga ispušta. Svi objekti će sadržavati **objectGuid**pa taj zahtjev će uvijek se kada se izvršava pravilo 1.

**Pravilo 3: Problema ms-ds-consistencyguid kao immutable ID ako postoje**

    c:[Type == "http://contoso.com/ws/2016/02/identity/claims/msdsconcistencyguid"]
    => issue(Type = "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/nameidentifier", Value = c.Value);

Ovo je implicitno potvrdite za **postoje** . Ako postoji vrijednost za zahtjev, zatim problema koji kao immutable ID-a. U prethodnom primjeru koristi zahtjeva **nameidentifier** . Morat ćete to promijeniti vrsta odgovarajuće zahtjeva za immutable ID u svom okruženju.

**Pravilo 4: Problema objectGuid kao immutable ID ako nema ms-ds-consistencyGuid**

    c1:[Type == "urn:anandmsft:tmp/idflag", Value =~ "useguid"]
    && c2:[Type == "http://contoso.com/ws/2016/02/identity/claims/objectguid"]
    => issue(Type = "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/nameidentifier", Value = c2.Value);

U tim se pravilom jednostavno provjeravate privremene zastavice **idflag**. Odlučite želite li problema zahtjeva na temelju njegove vrijednosti.

> [AZURE.NOTE] Važno je niz ta pravila.

#### <a name="sso-with-a-subdomain-upn"></a>SSO s poddomene UPN-a

Možete dodati više domena da biste se vanjskog pomoću Azure AD Connect, kao što je opisano u [dodajte nove pridruženoj domeni](active-directory-aadconnect-federation-management.md#add-a-new-federated-domain). Morate izmijeniti zahtjeva UPN tako da se ID izdavač odgovara korijensku domenu, a ne poddomene jer pridruženim korijensku domenu pokriva i podređenim.

Prema zadanim postavkama pravila zahtjeva za ID izdavač je postavljen kao:

    c:[Type
    == “http://schemas.xmlsoap.org/claims/UPN“]

    => issue(Type = “http://schemas.microsoft.com/ws/2008/06/identity/claims/issuerid“, Value = regexreplace(c.Value, “.+@(?<domain>.+)“, “http://${domain}/adfs/services/trust/“));

![Zadani izdavač ID zahtjeva](media\active-directory-aadconnect-federation-management\issuer_id_default.png)

Zadano pravilo jednostavno vodi UPN nastavka i koristi ID zahtjeva izdavač. Ako, na primjer, Nevena je korisnik u sub.contoso.com, a contoso.com je povezani Azure AD. Unosi Nevena john@sub.contoso.com kao korisničko ime prilikom prijave za Azure AD i zadani ID izdavač zahtjeva pravilo u AD FS obrađuje na sljedeći način.

    c:[Type
    == “http://schemas.xmlsoap.org/claims/UPN“]

    => issue(Type = “http://schemas.microsoft.com/ws/2008/06/identity/claims/issuerid“, Value = regexreplace(john@sub.contoso.com, “.+@(?<domain>.+)“, “http://${domain}/adfs/services/trust/“));

**Zahtjeva vrijednost:** http://sub.contoso.com/adfs/services/trust/

Da bi samo korijensku domenu u vrijednost zahtjeva izdavača, promijenite pravila zahtjeva da sadrže sljedeće.

    c:[Type == “http://schemas.xmlsoap.org/claims/UPN“]

    => issue(Type = “http://schemas.microsoft.com/ws/2008/06/identity/claims/issuerid“, Value = regexreplace(c.Value, “^((.*)([.|@]))?(?<domain>[^.]*[.].*)$”, “http://${domain}/adfs/services/trust/“));

## <a name="next-steps"></a>Daljnji koraci

Dodatne informacije o [Mogućnosti za prijavu korisnika](active-directory-aadconnect-user-signin.md).
