<properties
    pageTitle="Azure AD Connect: Prilagođenu instalaciju | Microsoft Azure"
    description="Mogućnosti prilagođene instalacije Azure AD Connect podatke o ovom dokumentu. Instalirajte Active Directory putem Azure AD Connect pomoću ove upute."
    services="active-directory"
    keywords="što je Azure AD Connect, instalirajte Active Directory, potrebne komponente za Azure AD"
    documentationCenter=""
    authors="andkjell"
    manager="femila"
    editor="curtand"/>

<tags
    ms.service="active-directory"  
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="09/13/2016"
    ms.author="billmath"/>

# <a name="custom-installation-of-azure-ad-connect"></a>Prilagođene instalacije Azure AD Connect
Azure AD Connect **prilagođenih postavki** koristi se kada želite da se dodatne mogućnosti instalacije. Koristi se ako imate više šuma ili ako želite konfigurirati nije obuhvaćeno eksplicitnih instalacije značajke. Koristi se u svim slučajevima gdje mogućnost [**izričitog instalacije**](active-directory-aadconnect-get-started-express.md) ispunila implementacija ili topologije.

Prije pokretanja instalacije Azure AD Connect, provjerite je li da biste [preuzeli Azure AD Connect](http://go.microsoft.com/fwlink/?LinkId=615771) i dovršite stara obavezna koraka u [Azure AD Connect: hardver i preduvjeti](../active-directory-aadconnect-prerequisites.md). I provjerite je li imaju obavezne računi dostupni kao što je opisano u [Azure AD Connect računa i dozvola](active-directory-aadconnect-accounts-permissions.md).

Prilagođene postavke odgovara topologije, na primjer da biste nadogradili DirSync, potražite u članku [povezani dokumentaciju](#related-documentation) za drugim situacijama.

## <a name="custom-settings-installation-of-azure-ad-connect"></a>Prilagođene postavke instalacije Azure AD Connect

### <a name="express-settings"></a>Ekspresne postavke
Na ovoj stranici, kliknite **Prilagodi** da biste pokrenuli prilagođene postavke instalacije.

### <a name="install-required-components"></a>Instalirajte potrebne komponente
Kada instalirate servisi za sinkronizaciju servisa, sekciji neobavezno konfiguracija možete ostaviti poništen, a Azure AD Connect postavlja sve automatski. Postavlja instancu komponente SQL Server 2012 Express LocalDB, stvorite odgovarajuće grupe i dodijelite dozvole. Ako želite promijeniti zadane vrijednosti, u sljedećoj su tablici možete koristiti da biste razumjeli mogućnosti neobavezno konfiguracije koji su dostupni.

![Potrebne komponente](./media/active-directory-aadconnect-get-started-custom/requiredcomponents.png)

Neobavezni konfiguracija  | Opis
------------- | -------------
Korištenje postojeće SQL Server | Omogućuje vam da navedete naziv SQL poslužitelja i naziv instance. Tu mogućnost odaberite ako već imate poslužitelj baze podataka koje želite koristiti. Unesite naziv instance slijedi zarez i priključak broj u **Naziv Instance** ako SQL Server ne morate omogućiti pregledavanje weba.
Koristi postojeći račun servisa | Prema zadanim postavkama Azure AD Connect stvara lokalnim servisom računa za servise za sinkronizaciju da biste koristili. Lozinka je automatski generirani i s nepoznatim osobi instalacije Azure AD Connect. Ako koristite udaljene SQL server ili koristite proxy poslužitelj zahtijeva provjeru autentičnosti, morat ćete servisa račun na domeni i znate lozinku. U tom slučaju unesite račun servisa da biste koristili. Provjerite je li korisnik koji se pokreće instalaciju programa Pacifička u SQL tako da se može stvoriti prijava za račun servisa. U odjeljku [Azure AD Connect računa i dozvola](active-directory-aadconnect-accounts-permissions.md#custom-settings-installation)
Odredite sinkronizaciju prilagođene grupe | Prema zadanim postavkama Azure AD Connect stvara četiri grupe lokalni poslužitelj kada su instalirani servisi za sinkronizaciju servisa. Ove grupe su: grupe administratora, operatori grupe, Pregledaj grupe i grupe za ponovno postavljanje lozinke. Možete navesti vlastite grupe. Grupa mora biti lokalni na poslužitelju i ne može se nalaziti u domeni.

### <a name="user-sign-in"></a>Korisničku prijavu
Nakon instalacije potrebne komponente vas se da biste odabrali korisnika jedan prijave načina. Sljedeća tablica sadrži kratak opis dostupnih mogućnosti. Puni opis načini prijave, potražite u članku [korisničku prijavu](../active-directory-aadconnect-user-signin.md).

![Prijava korisnika](./media/active-directory-aadconnect-get-started-custom/usersignin.png)

Jednu mogućnost za prijave | Opis
------------- | -------------
Sinkroniziranje lozinkom | Korisnici ne mogu se prijaviti u Microsoftovim servisima u oblaku, kao što je Office 365, koristeći istu lozinku koji se koriste u svoje lokalne mreže. Lozinke korisnika se sinkroniziraju Azure AD kao lozinku raspršivanje i provjeru autentičnosti koji se pojavljuje u oblak. Dodatne informacije potražite u članku [sinkronizaciju lozinke](../active-directory-aadconnectsync-implement-password-synchronization.md) .
Vanjski pristup sustavu AD FS | Korisnici ne mogu se prijaviti u Microsoftovim servisima u oblaku, kao što je Office 365, koristeći istu lozinku koji se koriste u svoje lokalne mreže.  Korisnici koji vas preusmjerava njihove na lokalni AD FS instancu da biste se prijavili i provjeru autentičnosti pojavljuje lokalnog.
Konfiguriranje | Ni značajka je instalacije i konfiguracije. Tu mogućnost odaberite ako već imate 3 strana vanjskom poslužitelju ili neki drugi postojeće rješenje na mjestu.

### <a name="connect-to-azure-ad"></a>Povezivanje s Azure AD
Na povezivanje Azure AD zaslon, unesite globalni administrator račun i lozinku. Ako ste odabrali **pridruženi AD FS** na prethodnu stranicu, ne prijavite se pomoću računa za domenu koju namjeravate omogućiti za vanjski pristup. Preporuku tako da koristi račun u zadanu domenu **onmicrosoft.com** , koja se isporučuje s direktorija Azure AD.

Taj račun koristi se samo da biste stvorili račun servisa u Azure AD, a ne koristi se kada se čarobnjak dovrši.  
![Prijava korisnika](./media/active-directory-aadconnect-get-started-custom/connectaad.png)

Ako je vaš račun globalnog administratora MFA omogućena, pa ćete morati lozinku ponovno u skočnom za prijavu i dovršite test MFA. Problem može biti u pruža kod za potvrdu ili telefonskom pozivu.  
![Prijava korisnika MFA](./media/active-directory-aadconnect-get-started-custom/connectaadmfa.png)

Računa globalnog administratora također može imati [Povlaštene upravljanje identitetom](../active-directory-privileged-identity-management-getting-started.md) omogućena.

Ako primate poruku o pogrešci, a imate problema s povezivanjem, pogledajte [Otklanjanje poteškoća s povezivanjem](../active-directory-aadconnect-troubleshoot-connectivity.md).

## <a name="pages-under-the-section-sync"></a>Stranica u odjeljku sinkronizacije

### <a name="connect-your-directories"></a>Povežite svoje direktorija
Da biste povezali servis Active Directory domene, Azure AD Connect treba vjerodajnice računa koji ima potrebne dozvole. Možete unijeti dio domene u obliku koji je NetBios ili FQDN, odnosno FABRIKAM\syncuser ili fabrikam.com\syncuser. Taj račun može biti običnog korisnički račun jer je potrebno samo zadane dozvole za čitanje. Međutim, ovisno o scenariju, možda je potrebno više dozvole. Dodatne informacije potražite u članku [Azure AD povezivanje računa i dozvola](../active-directory-aadconnect-accounts-permissions.md#create-the-ad-ds-account)

![Povezivanje direktorija](./media/active-directory-aadconnect-get-started-custom/connectdir.png)

### <a name="azure-ad-sign-in-configuration"></a>Azure AD prijavu konfiguracija
Ova stranica omogućuje vam da pregledate koje su prisutne u lokalne domene UPN AD DS i što provjereno u Azure AD. Ova stranica omogućuje i konfigurirali za na userPrincipalName.

![Neprovjereni domene](./media/active-directory-aadconnect-get-started-custom/aadsigninconfig.png)  
Pregledajte svake domene označeni **Niste dodali** i **Ne provjeriti**. Provjerite je li tih domena koristite provjereno Azure AD. Kliknite simbol osvježavanja kada ste potvrdili domenu. Dodatne informacije potražite u članku [Dodavanje i provjeru domene](../active-directory-add-domain.md)

**UserPrincipalName** - atribut userPrincipalName je korisnicima atribut koristite kada se prijaviti za Azure AD i Office 365. Domena koristi, poznata i kao UPN-sufiks, treba provjeriti u Azure AD prije korisnika koji se sinkroniziraju. Microsoft preporučuje se da biste zadržali atribut userPrincipalName zadani. Ako je taj atribut se nisu usmjeravati, a ne može provjeriti, a zatim je moguće da biste odabrali drugi atribut. Na primjer odaberite e-pošte kao atribut držanjem prijavu. Korištenje drugi atribut od userPrincipalName naziva **Alternativni ID -a**. Vrijednost atributa zamjenski ID moraju pratiti standardu RFC822. Zamjenski ID može se koristiti uz sinkronizaciju lozinke i vanjski pristup.

>[AZURE.WARNING]
Pomoću zamjenski ID-a nije kompatibilna sa svih radnih opterećenja sustava Office 365. Dodatne informacije potražite [Konfiguriranje zamjenski ID za prijavu](https://technet.microsoft.com/library/dn659436.aspx).

### <a name="domain-and-ou-filtering"></a>Domene i OU filtriranja
Prema zadanom sve domene i organizacijske jedinice se sinkroniziraju. Ako postoje neka domena ili organizacijske jedinice ne želite sinkronizirati Azure AD, možete poništiti te domene i organizacijske jedinice.  
![Filtriranje DomainOU](./media/active-directory-aadconnect-get-started-custom/domainoufiltering.png) Ova stranica čarobnjaka za konfiguriranje domene sustavom filtriranje. Dodatne informacije potražite u članku [utemeljen na domeni filtriranja](../active-directory-aadconnectsync-configure-filtering.md#domain-based-filtering).

Moguće je da su neke domene nije dostupan zbog ograničenja vatrozid. Ove domene su nije odabrano po zadanom i imaju upozorenje.  
![Nedostupno domene](./media/active-directory-aadconnect-get-started-custom/unreachable.png)  
Ako se prikaže upozorenje, provjerite jesu li se te domene uistinu nedostupan te se očekuje upozorenje.

### <a name="uniquely-identifying-your-users"></a>Jedinstveno označavanje korisnika
Matching preko šuma značajka omogućuje vam da biste odredili način na koji su korisnika iz vaše šuma AD DS predstavljeni u Azure AD. Korisnik može ili predstavljati samo jednom preko svih šuma ili pomoću kombinacije omogućeni i onemogućene računi. Korisnik također predstavljene kao kontakt u nekim šuma.

![Jedinstveni](./media/active-directory-aadconnect-get-started-custom/unique.png)

Postavka | Opis
------------- | -------------
[Korisnici samo jednom predstavljene preko svih šuma](../active-directory-aadconnect-topologies.md#multiple-forests-separate-topologies) | Svi korisnici stvaraju se kao pojedinačne objekte u Azure AD. U metaverse nije pridružen objekte.
[Atribut pošta](../active-directory-aadconnect-topologies.md#multiple-forests-full-mesh-with-optional-galsync) | Ta mogućnost spaja korisnika i kontakte ako atribut pošte ima jednaku vrijednost u različitim šuma. Tu mogućnost koristite kada kontakata je stvorena pomoću GALSync.
[ObjectSID i msExchangeMasterAccountSID / msRTCSIP OriginatorSid](../active-directory-aadconnect-topologies.md#multiple-forests-account-resource-forest) | Ta mogućnost spaja omogućeno korisnika u skup stabala na račun s korisnikom onemogućene u skupa stabala resursa. Sustava Exchange, tu konfiguraciju naziva povezane poštanski sandučić. Tu mogućnost i može se koristiti ako koristite samo Lync i Exchange ne postoji u skupa stabala resursa.
ni sAMAccountName ni MailNickName | Ta mogućnost spaja na atribute koje očekuje moguće je pronaći ID prijave za korisnika.
Određeni atribut | Ta mogućnost omogućuje odaberite vlastite atribut. **Ograničenje:** Obavezno odaberite atribut koji se već nalazi se u na metaverse. Ako odaberete prilagođene atributa (ne u metaverse), čarobnjak nije moguće dovršiti.

**Izvor sidro** - sourceAnchor atribut je atribut koji je immutable tijekom života korisničkom objektu. Je primarni ključ povezivanju lokalni korisnik s korisnikom Azure AD. Budući da nije moguće promijeniti atribut, potrebno je planirati dobar atributa da biste koristili. Dobar prijedlog je objectGUID. Atribut se mijenjaju, osim ako korisnički račun se premješta između šuma/domene. U okruženju više skupa stabala gdje računi prebacivanje šuma drugi atribut mora koristiti, kao što je atribut s na IDZaposlenika. Izbjegavajte atribute koje želite promijeniti kada marries neke osobe ili dodjele. Ne možete koristiti atribute pomoću programa @-sign, tako da e-pošte i userPrincipalName ne mogu se koristiti. Atribut se velika i mala slova pa kad premjestite objekt između šuma, provjerite da biste sačuvali gornjem/mala slova. Binarni atributi su base64 kodirani, ali druge vrste atributa ostati u stanje unencoded. U nekim Azure AD sučelja i scenariji za vanjski pristup, taj atribut se zove immutableID. Dodatne informacije o sidro izvor pronaći ćete u [konceptima dizajna](../active-directory-aadconnect-design-concepts.md#sourceAnchor).

### <a name="sync-filtering-based-on-groups"></a>Sinkronizacija filtriranje na temelju grupa
Filtriranje značajku grupe omogućuje vam da biste sinkronizirali samo mali podskup objekata pilota. Da biste koristili ovu značajku, stvorite grupu u tu svrhu u lokalni Active Directory. Zatim dodati korisnike i grupe koje se sinkronizirati Azure AD kao Izravni članovi. Naknadno možete dodati i ukloniti korisnike u ovu grupu da biste zadržali na popisu objekata koji moraju postojati u Azure AD. Sve objekte koje želite sinkronizirati mora biti izravno član grupe. Korisnike, grupe, kontakata i računalima i uređajima svi moraju biti članovi izravno. Članstvo u grupama ugniježđene nije riješen. Kada dodate grupu prilikom dodavanja člana grupe sam i ne članovima.

![Sinkronizacija filtriranja](./media/active-directory-aadconnect-get-started-custom/filter2.png)

>[AZURE.WARNING]
Značajka je namijenjen samo podržava testnih implementacije. Ne koristite radni full-blown implementacije.

U uvođenja full-blown radni je će biti teško da biste zadržali jedne grupe sa svim objektima za sinkronizaciju. Umjesto toga biste trebali koristiti jedan od načina [Konfiguriraj filtriranja](../active-directory-aadconnectsync-configure-filtering.md).

### <a name="optional-features"></a>Dodatne značajke
Ovaj zaslon omogućuje vam da biste odabrali dodatne značajke za nekim konkretnim scenarijima.

![Dodatne značajke](./media/active-directory-aadconnect-get-started-custom/optional.png)

>[AZURE.WARNING]
Ako trenutno imate DirSync ili Azure AD sinkronizaciju aktivna, nije aktivirati neku od značajki upisima u Azure AD Connect.

Dodatne značajke | Opis
------------------- | -------------
Hibridna implementacija sustava Exchange | Značajka Hibridna implementacija sustava Exchange omogućuje za koegzistencija poštanski sandučići sustava Exchange i lokalne i u sustavu Office 365. Azure AD Connect sinkronizira određeni skup [atribute](../active-directory-aadconnectsync-attributes-synchronized.md#exchange-hybrid-writeback) iz Azure AD zaključali lokalnog imenika.
Aplikacija za Azure AD atribut filtriranja i | Omogućivanjem aplikacije Azure AD atribut filtriranja i skup sinkronizirane atributa možete prilagoditi. Tu mogućnost dodaje dvije dodatne konfiguracije stranice čarobnjaka. Dodatne informacije potražite u članku [aplikacija Azure AD atribut filtriranja i](#azure-ad-app-and-attribute-filtering).
Sinkronizaciju lozinke | Ako ste odabrali vanjski pristup kao rješenje za prijavu, možete omogućiti tu mogućnost. Sinkronizaciju lozinke može se koristiti kao mogućnost za sigurnosno kopiranje. Dodatne informacije potražite u članku [sinkronizaciju lozinke](../active-directory-aadconnectsync-implement-password-synchronization.md).
Lozinka upisima | Omogućivanjem lozinku upisima promjene lozinke koja proizlazi u Azure AD zapisuje natrag na lokalnog imenika. Dodatne informacije potražite u članku [Početak rada s upravljanjem lozinku](../active-directory-passwords-getting-started.md).
Grupa upisima | Ako koristite značajku **Grupama sustava Office 365** , možete postaviti te grupe predstavljeni u lokalni Active Directory. Ova mogućnost dostupna samo ako su prisutne u Active Directory lokalnog sustava Exchange. Dodatne informacije potražite u članku [upisima grupe](../active-directory-aadconnect-feature-preview.md#group-writeback).
Uređaj upisima | Omogućuje objekata upisima uređaja u Azure AD lokalni Active Directory za scenarija uvjetno pristupa. Dodatne informacije potražite u članku [Omogućavanje uređaja upisima u Azure AD Connect](../active-directory-aadconnect-feature-device-writeback.md).
Sinkroniziranje atribut proširenje direktorija | Omogućivanjem sinkroniziranje atribut proširenja direktorija navedenih atributa se sinkroniziraju Azure AD. Dodatne informacije potražite u članku [proširenja direktorija](../active-directory-aadconnectsync-feature-directory-extensions.md).

### <a name="azure-ad-app-and-attribute-filtering"></a>Aplikacija za Azure AD atribut filtriranja i
Ako želite ograničiti koji se atributi sinkroniziraju Azure AD, počnite tako da odaberete koje servise koristite. Ako unesete promjene konfiguracije na ovoj stranici, novog servisa mora biti odabrao izričito rerunning Čarobnjak za instalaciju.

![Značajke aplikacije](./media/active-directory-aadconnect-get-started-custom/azureadapps2.png)

Ovisno o servisima u prethodnom koraku odabrali, ova stranica prikazuje svih atributa koji se sinkroniziraju. Ovaj popis je kombinacija sve vrste objekata koji se sinkroniziraju. Ako postoje neki određeni atributi morate sinkronizirati, možete poništiti te atribute.

![Dodatne značajke atributa](./media/active-directory-aadconnect-get-started-custom/azureadattributes2.png)

>[AZURE.WARNING]
Uklanjanje atribute može utjecati funkcije. Najbolje prakse i preporukama potražite u članku [atributi sinkroniziraju](../active-directory-aadconnectsync-attributes-synchronized.md#attributes-to-synchronize).

### <a name="directory-extension-attribute-sync"></a>Sinkroniziranje atribut proširenje direktorija
Proširivanje sheme na Azure AD pomoću prilagođene atribute dodao tvrtke ili ustanove ili druge atribute u servisu Active Directory. Da biste koristili ovu značajku, odaberite **sinkronizaciju atributa direktorija nastavak** na stranici **Dodatne značajke** . Možete odabrati više atribute za sinkronizaciju na ovoj stranici.

![Proširenja direktorija](./media/active-directory-aadconnect-get-started-custom/extension2.png)

Dodatne informacije potražite u članku [proširenja direktorija](../active-directory-aadconnectsync-feature-directory-extensions.md).

## <a name="configuring-federation-with-ad-fs"></a>Konfiguriranje vanjskog pristupa AD fs
Konfiguriranje servisa AD FS s Azure AD Connect je jednostavno uz samo nekoliko klikova. Sljedeće potreban je prije konfiguracije.

- Na poslužitelju Windows Server 2012 R2 za vanjskom poslužitelju s daljinsko upravljanje omogućeno
- Na poslužitelju Windows Server 2012 R2 Web aplikacije Proxy poslužitelja s daljinsko upravljanje omogućeno
- SSL certifikat za vanjski pristup naziv usluge namjeravate koristiti (na primjer sts.contoso.com)

### <a name="ad-fs-configuration-pre-requisites"></a>Prije requisites konfiguracije za AD FS
Da biste konfigurirali farmi AD FS pomoću Azure AD Connect, provjerite je li WinRM omogućen na udaljenim poslužiteljima. Osim toga, proći kroz obavezne priključke naveden u [tablice 3 – Azure AD Connect i poslužitelji WAP vanjski pristup](../active-directory-aadconnect-ports.md#table-3---azure-ad-connect-and-federation-serverswap).

### <a name="create-a-new-ad-fs-farm-or-use-an-existing-ad-fs-farm"></a>Stvaranje nove farme AD FS ili korištenje postojeća farma za AD FS
Možete koristiti postojeća farma za AD FS ili možete stvoriti novu farmu AD FS. Ako odaberete da biste stvorili novi, su potrebne za pružanje SSL certifikata. Ako SSL certifikat zaštićena lozinkom, od vas će se traži lozinku.

![AD FS farme](./media/active-directory-aadconnect-get-started-custom/adfs1.png)

Ako odlučite koristiti postojeća farma za AD FS, uzimaju izravno u konfiguraciji odnos pouzdanosti između AD FS i Azure AD zaslona.

### <a name="specify-the-ad-fs-servers"></a>Navedite poslužitelje servisa AD FS
Unesite s poslužiteljima koji želite instalirati AD FS na. Možete dodati jedan ili više poslužitelja koji se temelji na vaše potrebe za planiranje kapaciteta. Uključivanje poslužiteljima sa servisom Active Directory prije nego što načinite tu konfiguraciju. Microsoft preporučuje instalacije pojedinačni AD FS poslužitelj za testiranje i pilot implementacije. Dodajte i implementacija više poslužitelja svojim potrebama skaliranja tako da pokrenete Azure AD Connect ponovno nakon početnog konfiguracije.

>[AZURE.NOTE]
Provjerite da na poslužiteljima na kojima su pridruženi domenom sustava AD prije tu konfiguraciju.

![AD FS poslužitelja](./media/active-directory-aadconnect-get-started-custom/adfs2.png)

### <a name="specify-the-web-application-proxy-servers"></a>Navedite Web aplikacije Proxy poslužitelje
Unesite s poslužiteljima koji želite koristiti kao proxy poslužitelja web-aplikacije. Web-aplikacije proxy poslužitelj je uveden u DMZ (ekstranet dostupnog) te podržava provjeru autentičnosti zahtjevi za ekstranet. Možete dodati jedan ili više poslužitelja koji se temelji na vaše potrebe za planiranje kapaciteta. Microsoft preporučuje instalacije jedan Web aplikacije proxy poslužitelja za testiranje i pilot implementacije. Dodajte i implementacija više poslužitelja svojim potrebama skaliranja tako da pokrenete Azure AD Connect ponovno nakon početnog konfiguracije. Preporučujemo da imate ekvivalentne broj proxy poslužitelji za ispunjavanje provjere autentičnosti s intraneta.

>[AZURE.NOTE]
<li> Ako račun koji koristite nije lokalni administrator na poslužiteljima AD FS, dobit ćete upit za administratorske vjerodajnice.</li>
<li> Provjerite postoji li HTTP/HTTPS veze između poslužitelj za Azure AD Connect i Web aplikacije Proxy poslužitelj prije nego što pokrenete ovaj korak.</li>
<li> Provjerite postoji li HTTP/HTTPS veze između web-poslužitelj aplikacije i poslužitelj za ADFS dopustili tijeka kroz zahtjeva za provjeru autentičnosti.</li>

![Web-aplikacije](./media/active-directory-aadconnect-get-started-custom/adfs3.png)

Se od vas zatraži da unesete vjerodajnice da bi se web-poslužitelj aplikacije možete uspostaviti sigurnu vezu s poslužiteljem za AD FS. Te vjerodajnice moraju biti lokalni administrator na poslužitelju za AD FS.

![Proxy poslužitelj](./media/active-directory-aadconnect-get-started-custom/adfs4.png)

### <a name="specify-the-service-account-for-the-ad-fs-service"></a>Odredite na račun servisa servisa AD FS
Servis za AD FS potreban je račun servisa domene za provjeru autentičnosti korisnika i korisničkih podataka za pretraživanje u servisu Active Directory. Podržanom dvije vrste računa servisa:

- **Grupa upravljanih računa servisa** - uvedena u servisu Active Directory Domain Services sa sustavom Windows Server 2012. Ove vrste računa nudi servisa, kao što su AD FS, jedan račun bez obzira na to redovito ažuriranje lozinke za račun. Koristite ovu mogućnost ako već imate Windows Server 2012 kontrolera domena u domeni koji pripadaju poslužitelja za AD FS.
- **Korisnički račun domene** - ove vrste računa potrebno lozinku i redovito ažurirati lozinku kada lozinka Promijeni ili istekne. Tu mogućnost koristite samo kada nemate kontrolera domena u sustavu Windows Server 2012 domene kojoj pripadate poslužitelja za AD FS.

Ako ste odabrali račun servisa upravlja grupe, a ta značajka nikada niste koristili u servisu Active Directory, od vas će se traži Enterprise administratorske vjerodajnice. Da biste započeli ključa pohrane i omogućio značajku u servisu Active Directory se koristi te vjerodajnice.

![Račun za servis za AD FS](./media/active-directory-aadconnect-get-started-custom/adfs5.png)

### <a name="select-the-azure-ad-domain-that-you-wish-to-federate"></a>Odaberite Azure AD domenu koju želite združivanje
Tu konfiguraciju koristi se za vanjski pristup odnos između AD FS i Azure AD za postavljanje. Konfigurira AD FS da biste problem sigurnosnih tokena za Azure AD i konfigurira Azure AD pouzdanosti tokena iz tu instancu AD FS. Ova stranica samo omogućuje konfigurirati jednu domenu u početne instalacije. Još domena kasnije, konfigurirati tako da ponovno pokrenete Azure AD Connect.

![Azure AD domene](./media/active-directory-aadconnect-get-started-custom/adfs6.png)

### <a name="verify-the-azure-ad-domain-selected-for-federation"></a>Provjerite je li odabrana za vanjski pristup domeni Azure AD
Kad odaberete domene koje želite biti pridružene, Azure AD Connect omogućuje potrebne informacije da biste potvrdili Neprovjereni domenu. Potražite u članku [Dodavanje i provjeru domene](../active-directory-add-domain.md) kako koristiti taj podatak.

![Azure AD domene](./media/active-directory-aadconnect-get-started-custom/verifyfeddomain.png)

>[AZURE.NOTE]
AD Connect će se pokušati radi potvrde domene tijekom faze Konfiguriraj. Ako i dalje da biste konfigurirali bez dodavanja potrebne DNS zapise, čarobnjak nije mogao da biste dovršili konfiguraciju.

## <a name="configure-and-verify-pages"></a>Konfiguriranje i provjerite je li stranice
Konfiguracija događa na ovoj stranici.

>[AZURE.NOTE]
Prije nego što nastavite instalaciju i ako ste konfigurirali vanjski pristup, provjerite je li da ste konfigurirali [razlučivanje imena u pošti vanjskim poslužiteljima](../active-directory-aadconnect-prerequisites.md#name-resolution-for-federation-servers).

![Jeste li spremni za konfiguraciju](./media/active-directory-aadconnect-get-started-custom/readytoconfigure2.png)

### <a name="staging-mode"></a>Pripremna načinu
Moguće je postaviti novi poslužitelj za sinkronizaciju paralelno s pripremna načinu rada. Podržana je samo da bi jednog sinkronizacijom poslužitelja Izvoz u jedan direktorija u oblaku. No ako želite premjestiti iz drugi poslužitelj, na primjer jedan izvodi DirSync, zatim možete omogućiti Azure AD Connect u pripremna načinu rada. Kada je omogućen, modula za sinkroniziranje uvoz i sinkroniziranje podataka kao normalne, ali ga ne izvoz ništa Azure AD ili AD. Dok se nalazite u načinu rada pripremna onemogućene su upisima na značajke lozinku i lozinku za sinkronizaciju.

![Pripremna načinu](./media/active-directory-aadconnect-get-started-custom/stagingmode.png)

Dok je u načinu za pripremna je moguće izvršiti potrebne promjene u modula za sinkroniziranje i pregledajte što uskoro će se izvesti. Kada konfiguraciju izgleda dobro, ponovno pokrenite čarobnjak za instalaciju i onemogućiti pripremna način rada. Podaci se sada izvozi Azure AD s poslužitelja. Obavezno onemogućite drugi poslužitelj u isto vrijeme tako da samo jedan poslužitelj aktivno izvoz.

Dodatne informacije potražite u članku [pripremna način](../active-directory-aadconnectsync-operations.md#staging-mode).

### <a name="verify-your-federation-configuration"></a>Provjerite je li konfiguraciju vanjski pristup
Azure AD Connect potvrđuje postavke DNS-a za vas kada kliknete gumb potvrdi valjanost.

![Dovršavanje](./media/active-directory-aadconnect-get-started-custom/completed.png)

![Provjerite je li](./media/active-directory-aadconnect-get-started-custom/adfs7.png)

Osim toga, učinite sljedeće provjere valjanosti:

- Provjerite valjanost da se možete prijaviti putem preglednika s računala spojene domene na intranetu: povezivanje s https://myapps.microsoft.com pa provjerite je li se prijaviti pomoću računa prijavljenog. Ugrađeni AD DS administratorski račun nije sinkroniziran i ne može se koristiti za potvrdu.
- Provjerite valjanost da se možete prijaviti s uređaja s na ekstranet. Na kućnom računalu ili mobilnom uređaju, povežite se s https://myapps.microsoft.com i navesti vjerodajnice.
- Provjerite valjanost prijavu obogaćenog klijenta. Povezivanje s https://testconnectivity.microsoft.com, odaberite karticu **Office 365** i odaberite **Office 365 jednom prijave testirajte**.

## <a name="next-steps"></a>Daljnji koraci
Nakon dovršetka instalacije, Odjava i ponovno se prijavite u sustav Windows prije korištenja Upravitelj sinkronizacije servisa ili uređivač pravila za sinkronizaciju.

Sad kad ste instalirali Azure AD Connect možete [provjeriti instalaciju i dodjela licenci](../active-directory-aadconnect-whats-next.md).

Dodatne informacije o tim značajkama koje su omogućena za instalaciju: [Sprječavanje slučajne briše](../active-directory-aadconnectsync-feature-prevent-accidental-deletes.md) i [Povežite zdravlje Azure AD](../active-directory-aadconnect-health-sync.md).

Dodatne informacije o ovim temama uobičajenih: [Raspored i kako pokrenuti sinkronizaciju](../active-directory-aadconnectsync-feature-scheduler.md).

Dodatne informacije o [integraciji vaših lokalnih identiteta sa servisu Azure Active Directory](../active-directory-aadconnect.md).

## <a name="related-documentation"></a>Srodni dokumentacija

Tema |  
--------- | ---------
Pregled Azure AD Connect | [Integriranje sustava lokalnih identiteta sa Azure Active Directory](../active-directory-aadconnect.md)
Instalacija pomoću postavki Express | [Express instalacije Azure AD Connect](active-directory-aadconnect-get-started-express.md)
Nadogradnja s DirSync | [Nadogradnja s alat za sinkronizaciju Azure AD (DirSync)](active-directory-aadconnect-dirsync-upgrade-get-started.md)
Račune koji se koriste za instalaciju | [Dodatne informacije o Azure AD Connect računa i dozvola](active-directory-aadconnect-accounts-permissions.md)
