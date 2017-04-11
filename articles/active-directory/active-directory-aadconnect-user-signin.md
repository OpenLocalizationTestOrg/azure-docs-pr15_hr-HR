<properties
    pageTitle="Azure AD Connect: Prijava korisnika u | Microsoft Azure"
    description="Azure AD Connect za prijavu za prilagođene postavke."
    services="active-directory"
    documentationCenter=""
    authors="billmath"
    manager="femila"
    editor="curtand"/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/16/2016"
    ms.author="billmath"/>



# <a name="azure-ad-connect-user-sign-on-options"></a>Azure AD povezivanje korisnika za prijavu mogućnosti

Azure AD Connect omogućuje korisnicima da biste se prijavili oblak i lokalnog resursa isti lozinkama. U ovom se članku opisuju koncepti za svaki identitet model da biste lakše odabrali identitet koji želite koristiti za svoje prijava Azure AD.

Ako već poznajete Azure AD identiteta modela, a želite li saznati više o određeni način, jednostavno kliknite ispod na odgovarajućoj temi.

* [Sinkroniziranje lozinkom](#password-synchronization)
* [Pridruženi SSO (uz ADFS)](#federation-using-a-new-or-existing-ad-fs-in-windows-server-2012-r2-farm)


## <a name="choosing-a-user-sign-in-method"></a>Odabir metode Prijava korisnika
Za većinu tvrtki ili ustanova koja samo želite omogućiti korisniku se prijaviti na Office 365, SaaS aplikacije i druge Azure AD na temelju resursa, zadana mogućnost za sinkronizaciju lozinke preporučuje se.
Neke tvrtke ili ustanove, međutim, imati određenog razloga za korištenje pridruženim prijavu na mogućnost kao što su AD FS.  To obuhvaća:

- Tvrtka ili ustanova već ima AD FS ili 3 implementiran davatelja za vanjski pristup za zabavu
- Sigurnosna pravila ne dopuštaju sinkronizaciju lozinke raspršivanja u oblak
- Tražite da korisnici primijetiti objedinjenog SSO (bez dodatnih lozinku Traži) prilikom pristupa resursima oblaka iz domene spajaju strojeva poslovnoj mreži
- Ako su vam potrebne neke određene mogućnosti sadrži AD FS
    - Lokalni višestruka provjera autentičnosti pomoću drugih proizvođača davatelja ili pametne kartice (Dodatne informacije o davateljima MFA drugih proizvođača za AD FS u sustavu Windows Server 2012 R2)
    - Active Directory integracije značajke kao što su lockout meki računa ili AD lozinke i raditi pravila sati
    - Lokalni uvjetno pristup oba i resursima pomoću Registracija uređaja, uključite Azure AD ili pravilnike za Intune MDM u oblaku

### <a name="password-synchronization"></a>Sinkronizaciju lozinke
Uz sinkronizaciju lozinke, raspršivanja korisničkih lozinki se sinkroniziraju iz lokalnog Active Directory Azure AD.  Kada lozinke su promijenjene ili ponovno postavljanje lokalno, nove lozinke se sinkroniziraju odmah Azure AD tako da korisnici uvijek možete koristiti istu lozinku za oblak resurse kao i lokalne.  Lozinke se nikad ne šalju Azure AD niti pohranjene u Azure AD u običan tekst.
Sinkronizaciju lozinke se može koristiti zajedno s pisanje natrag za lozinku da biste omogućili sebe u Azure AD za ponovno postavljanje lozinke za servis.

<center>![Oblak](./media/active-directory-aadconnect-user-signin/passwordhash.png)</center>

[Dodatne informacije o sinkronizaciji lozinke](active-directory-aadconnectsync-implement-password-synchronization.md)


### <a name="federation-using-a-new-or-existing-ad-fs-in-windows-server-2012-r2-farm"></a>Vanjski pristup korištenje novi ili postojeći AD FS u farmi sustava Windows Server 2012 R2
S pridruženim prijaviti, korisnici mogu se prijaviti Azure AD temelji servisa s lokalnim lozinke i, na mreži tvrtke, bez potrebe za unosom lozinke ponovno.  Mogućnosti vanjski pristup za AD FS omogućuje implementacije novog ili odredite postojeće AD FS u farmi sustava Windows Server 2012 R2.  Ako odaberete da biste odredili postojeća farma, Azure AD Connect će konfigurirati pouzdanosti između farmi i Azure AD tako da korisnici mogu se prijaviti na.

<center>![Oblak](./media/active-directory-aadconnect-user-signin/federatedsignin.png)</center>

#### <a name="to-deploy-federation-with-ad-fs-in-windows-server-2012-r2-you-will-need-the-following"></a>Da biste implementirali pridruženi ADFS u sustavu Windows Server 2012 R2, potrebno je sljedeće

Ako implementirate nove farme:

- Windows Server 2012 R2 poslužitelj za vanjskom poslužitelju.
- Windows Server 2012 R2 poslužitelj za Proxy aplikacije za Web.
- .pfx datoteku s jednog SSL certifikat za svrhu vanjski pristup naziv servisa, kao što su fs.contoso.com.

Ako su uvođenje nove farme ili pomoću postojeća farma:

- Lokalni administratorske vjerodajnice na vanjskim poslužiteljima.
- Lokalni administratorske vjerodajnice na sve radne grupe (koji nisu domenama pridruženo) poslužitelji na kojoj namjeravate uloga Proxy aplikacije za Web.
- Na računalu na kojem je izvršavanje čarobnjaka mora biti moći spojiti na druga računala na kojem želite instalirati AD FS ili proxy poslužitelj aplikacije Web putem daljinsko upravljanje sustavom Windows.

[Konfiguriranje SSO AD fs](./aad-connect/active-directory-aadconnect-get-started-custom.md#configuring-federation-with-ad-fs) 

#### <a name="sign-on-using-an-earlier-version-of-ad-fs-or-a-third-party-solution"></a>Prijava pomoću starije verzije programa AD FS ili drugog proizvođača rješenja

Ako ste već konfigurirali oblaka znak na pomoću starije verzije programa AD FS (kao što su AD FS 2.0) ili vanjski pristup davatelja trećih strana, možete odabrati da biste preskočili Prijava korisnika u konfiguraciji putem Azure AD Connect.  To će vam omogućiti da biste dobili zadnje sinkronizacije i druge mogućnosti Azure AD Connect dok još uvijek koriste postojeće rješenje za prijavu na.

[Azure AD popis kompatibilnosti trećih strana vanjski pristup](./active-directory-aadconnect-federation-compatibility.md)

## <a name="user-sign-in-and-user-principal-name-upn"></a>Korisničku prijavu i korisnikovo Glavno ime (UPN)

### <a name="understanding-user-principal-name"></a>Razumijevanje korisnikovo Glavno ime

U servisu Active Directory UPN nastavka zadani je naziv DNS-a domene u kojem se stvorili korisnički račun. U većini slučajeva, to je naziv domene koja je registrirana kao domene tvrtke na Internetu. Međutim, možete dodati više UPN nastavke pomoću Active Directory Domains and Trusts.

UPN-a korisnika je u obliku username@domai. Na primjer, za active directory contoso.com korisnika Nevena možda UPN john@contoso.com. UPN-a korisnika temelji se na RFC 822. Iako UPN-a i e-pošte omogućili zajedničko korištenje istog oblika, možda se vrijednost UPN-a za korisnika ili možda neće biti jednaki adresu e-pošte korisnika.

### <a name="user-principal-name-in-azure-ad"></a>Korisnikovo Glavno ime u Azure AD

Čarobnjak za Azure AD Connect će koristiti atribut userPrincipalName ili omogućuju vam navođenje atributa (u prilagođenu instalaciju) koja će se koristiti s lokalnim kao glavni naziv korisnika u Azure AD. Ovo je vrijednost koja će se koristiti za prijavu u Azure AD. Ako je vrijednost atributa glavni naziv korisnik ne odgovara provjerene domene u Azure AD, zatim Azure AD će ga zamijeniti zadanu. onmicrosoft.com vrijednost.

Svaki direktorij servisa Azure Active Directory dolazi s nazivom domene ugrađene u contoso.onmicrosoft.com obrazac koji vam omogućuje prvi koraci pri korištenju Azure i druge Microsoftove servise. Možete poboljšati i pojednostaviti prijavu pomoću prilagođene domene. Informacije o prilagođenu domenu imena u Azure AD i provjera domenu, pročitajte [Dodavanje prilagođeni naziv domene Azure Active Directory](active-directory-add-domain.md#add-your-custom-domain-name-to-azure-active-directory)

## <a name="azure-ad-sign-in-configuration"></a>Azure AD prijavu konfiguracija

### <a name="azure-ad-sign-in-configuration-with-azure-ad-connect"></a>Azure AD prijavu u konfiguraciji s Azure AD Connect
Azure AD prijavu sučelje ovisi o tome je li Azure AD moći odgovaraju sufiks glavni naziv korisnika korisnika koji se sinkroniziraju na neki od prilagođenih domena potvrđena u imeniku Azure AD. Azure AD Connect nudi pomoć prilikom konfiguriranja Azure AD prijavu postavke tako da je slično iskustvo lokalnog korisničkog sučelja za prijavu u oblaku. Azure AD Connect popisi nastavke UPN definirali za na domain(s) i pokušava pronaći ih nosi naziv prilagođene domene u Azure AD i pomaže vam odgovarajuće akcije koje treba poduzeti.
Stranica za prijavu u Azure AD navodi out nastavke UPN definirali za lokalni Active Directory i prikazuje status odgovarajuće protiv svaki sufiks. Vrijednosti stanja može biti nešto od u nastavku:

| Stanje | Opis | Potrebno ništa poduzimati |
|:----|:----|:----|
|Provjeriti| Azure AD Connect pronaći odgovarajući potvrde domene u Azure AD. Sve korisnike za tu domenu mogli prijaviti pomoću vjerodajnica za lokalni| Potreban je nijedna akcija |
|Nije provjereno| Azure AD Connect nije moguće pronaći odgovarajuće prilagođene domene u Azure AD, ali je potvrđena. UPN nastavka korisnika te domene promijenit će se i zadane postavke i. onmicrosoft.com sufiks nakon sinkronizacije ako domena je potvrđena. | Potvrdite prilagođenu domenu u Azure AD. [uči više](./active-directory-add-domain.md#verify-the-domain-name-with-azure-ad) |  
|Niste dodali| Azure AD Connect nije moguće pronaći prilagođenu domenu koja odgovara UPN nastavka. UPN nastavka korisnika s ove domene promijenit će se i zadane postavke i. onmicrosoft.com ako domena ne dodali i verifeid u Azure.| Dodavanje i provjeru prilagođenu domenu koja odgovara UPN nastavka [Saznajte više](./active-directory-add-domain.md)|

Azure AD stranica za prijavu u popis suffix(s) UPN definirane za lokalni Active Directory i odgovarajuće prilagođene domene u Azure AD pomoću trenutnog statusa provjere. U prilagođenu instalaciju, sada možete odabrati atribut za korisnikovo Glavno ime na stranici za **prijavu Azure AD** .

![Azure AD stranica za prijavu u](./media/active-directory-aadconnect-user-signin/custom_azure_sign_in.png)

Klikom na gumb Osvježi da biste ponovno iz Azure AD dohvatite najnoviji status prilagođene domene.

###<a name="selecting-attribute-for-user-principal-name-in-azure-ad"></a>Odabir atribut korisnikovo Glavno ime u Azure AD

UserPrincipalName - atribut userPrincipalName je će korisnici koristiti kada se prijavite u Azure AD atribut i Office 365. Domena koristi, poznata i kao UPN-sufiks, treba provjeriti u Azure AD prije korisnika koji se sinkroniziraju. Preporučuje da biste zadržali atribut userPrincipalName zadani. Ako je taj atribut koji nisu usmjeravati i ne može provjeriti, a zatim je moguće da biste odabrali drugi atribut, primjerice e-pošte kao atribut držanjem prijavu. To se naziva alternativni ID-a. Vrijednost atributa zamjenski ID moraju pratiti standardu RFC822. Zamjenski ID može se koristiti s lozinke jednog prijavu (SSO) i vanjski pristup SSO kao rješenje za prijavu.

>[AZURE.NOTE] Korištenje zamjenske ID nije kompatibilan sa svih radnih opterećenja sustava Office 365. Da biste saznali više, pogledajte [Konfiguriranje zamjenski ID za prijavu](https://technet.microsoft.com/library/dn659436.aspx).

#### <a name="different-custom-domain-states-and-effect-on-azure-sign-in-experience"></a>Država različite prilagođenu domenu i učinak na Azure sučelje za prijavu
Vrlo je važno da biste shvatili odnos između stanja prilagođene domene u direktoriju Azure AD i nastavke UPN definirani lokalnog. Javite nam proći kroz na različit moguće Azure prijavu korisnički doživljaj prilikom postavljanja sycnhronization pomoću Sastavljač Azure AD poveznog.

Ispod informacije Javite nam pretpostavlja da ne možemo su zabrinuti zbog contoso.com UPN nastavka koji se koriste u lokalnog imenika kao dio UPN-a, na primjer user@contoso.com.

###### <a name="express-settings--password-synchronization"></a>Ekspresne postavke / sinkronizaciju lozinke
| Stanje         | Učinak na Azure prijava doživljaja rada |
|:-------------:|:----------------------------------------|
| Niste dodali | U ovom slučaju ne prilagođene domene za contoso.com dodana u direktoriju Azure AD. Korisnici koji imaju UPN lokalnog s kraticom @contoso.com, nećete moći koristiti svoje lokalnog UPN-a za prijavu Azure. Umjesto toga će imati da biste koristili novog UPN dodijelio ih Azure AD dodavanjem sufiks zadani Azure AD direktorij. Na primjer, ako su sinkroniziranje korisnicima Azure AD direktorija azurecontoso.onmicrosoft.com zatim lokalni korisnik user@contoso.com će dobiti UPN oduser@azurecontoso.onmicrosoft.com|
| Nije provjereno | U ovom slučaju imamo prilagođenu domenu contoso.com dodane u direktoriju Azure AD, no još ne provjeriti. Ako ste nastaviti sinkronizacija korisnicima bez provjere domene, zatim korisnika će se dodijeliti novog UPN po Azure AD kao što je bio u scenariju "Ne dodaje".|
| Provjereno | U ovom slučaju imamo prilagođenu domenu contoso.com već dodali i provjeriti u Azure AD za UPN nastavka. Korisnici će moći koristiti svoje lokalnog korisnikovo Glavno ime, npr. user@contoso.com, za prijavu za Azure kada se sinkroniziraju Azure AD|

###### <a name="ad-fs-federation"></a>Vanjski pristup za AD FS
Ne možete stvoriti na vanjski pristup sa zadanim. domena onmicrosoft.com u Azure AD ili Neprovjereni prilagođene domene u Azure AD. Kada koristite čarobnjak za Azure AD Connect Ako odaberete Neprovjereni domene da biste stvorili vanjski pristup sustavu zatim Azure AD Connect će vas sa zapisima potrebne za stvaranje gdje se nalazi DNS za domenu. Dodatne informacije potražite [u nastavku](active-directory-aadconnect-get-started-custom.md#verify-the-azure-ad-domain-selected-for-federation).

Ako ste odabrali prijavu mogućnost korisnika kao "Pridruženi AD FS", morate imati prilagođenu domenu da biste nastavili sa stvaranjem na vanjski pristup u Azure AD. Za naše rasprave, to znači da ne možemo mora imati prilagođenu domenu contoso.com dodali u direktoriju Azure AD.

| Stanje         | Učinak na Azure prijava doživljaja rada |
|:-------------:|:----------------------------------------|
| Niste dodali | U ovom slučaju Azure AD Connect nije moguće pronaći odgovarajuće prilagođenu domenu contoso.com nastavka UPN u direktoriju Azure AD. Najprije morate dodati prilagođenu domenu contoso.com ako vam je potrebna korisnicima prijavu pomoću servisa AD FS sa svojim lokalnog UPN-a kao što su user@contoso.com.|
| Nije provjereno | U ovom slučaju Azure AD Connect će vas s odgovarajuće detalje o kako možete provjeriti vaše domene kasnije fazi|
| Provjereno | U ovom slučaju koje možete nastaviti s konfiguracijom bez sve dodatne akcije|  

## <a name="changing-user-sign-in-method"></a>Promjena načina za prijavu korisnika

Možete promijeniti način prijave korisnika iz vanjski pristup za korištenje avaialble zadataka u Azure AD Connect nakon početnog konfiguraciju Azure AD Connect pomoću čarobnjaka za sinkronizaciju lozinke. Ponovno pokrenite čarobnjak za Azure AD Connect, a koje će se prikazivati s popisom zadataka koje možete izvršiti. Odaberite **Promijeni korisničku prijavu** na popisu zadataka. 

![Promjena korisničku prijavu"](./media/active-directory-aadconnect-user-signin/changeusersignin.png)

Na sljedećoj stranici će se tražiti da unesite vjerodajnice za Azure AD.

![Povezivanje s Azure AD](./media/active-directory-aadconnect-user-signin/changeusersignin2.png)

Na stranici **korisničku prijavu** odaberite **Sinkronizaciju lozinke**. To će se promijeniti direktorija iz vanjskog upravljanih jedan.

![Povezivanje s Azure AD](./media/active-directory-aadconnect-user-signin/changeusersignin3.png)

>[AZURE.NOTE] Ako stvarate samo parametru privremenu lozinku sinkronizacije, provjerite **pretvoriti korisničke račune**. Ne provjerava na mogućnost vodi do pretvorbe svakog korisnika da biste vanjskog, a može potrajati nekoliko sati.
  
## <a name="next-steps"></a>Daljnji koraci
Dodatne informacije o [integraciji vaših lokalnih identiteta sa servisu Azure Active Directory](active-directory-aadconnect.md).

Dodatne informacije o [Azure AD Connect: dizajniranje koncepata](active-directory-aadconnect-design-concepts.md)


