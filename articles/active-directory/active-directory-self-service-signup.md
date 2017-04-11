<properties
    pageTitle="Što je samoposlužnog prijava za Azure? | Microsoft Azure"
    description="Za pregled samostalno prijava za Azure, kako upravljati postupka prijave i kako preuzeti DNS naziv domene."
    services="active-directory"
    documentationCenter=""
    authors="curtand"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="identity"
    ms.date="08/23/2016"
    ms.author="curtand"/>


# <a name="what-is-self-service-signup-for-azure"></a>Što je samoposlužnog prijava za Azure?

U ovoj se temi objašnjava postupak prijave samostalno te o tome kako preuzeti DNS naziv domene.  

## <a name="why-use-self-service-signup"></a>Zašto koristiti samostalno prijave?

- Pronađite kupci servisima brže žele.
- Stvaranje ponuda za e-pošte za uslugu.
- Stvaranje tokova Prijava putem e-pošte koji se brzo korisnicima omogućuje stvaranje identiteta pomoću njihovih jednostavno-na-Imajte na umu rad pseudonima e-pošte.
- Neupravljani Azure direktorija možete stvoriti u upravljanih direktorija kasnije, a može ponovno koristiti za druge servise.

## <a name="terms-and-definitions"></a>Uvjeti i definicije

+ **Registracija samostalne**: to je način na koji korisnik registrira za servise u oblaku i sadrži identitet automatski stvara za njih u Azure Active Directory (Azure AD) na temelju njihove domena e-pošte.
+ **Neupravljani Azure directory**: to je direktoriju u kojem je stvorena tog identiteta. Neupravljani direktorija je direktoriju nema globalni administrator.
+ **Potvrđena za e-pošte korisnika**: to je vrsta korisnički račun u Azure AD. Korisnik koji je stvorena automatski nakon registracije za ponude za samostalno zove korisnik sustava e-pošte provjeriti identitet. Korisnik sustava e-pošte provjeriti pripada običnog direktorija koji je označen creationmethod = EmailVerified.

## <a name="user-experience"></a>Korisnički doživljaj

Na primjer, pretpostavimo da korisnika čije e-pošte je Dan@BellowsCollege.com prima osjetljivih datoteka putem e-pošte. Datoteke su zaštićena upravljanjem pravima za Azure (Azure RMS). No Dan, tvrtke ili ustanove, studentski Bellows ne ustanova registrirala za Azure RMS niti je postavila Active Directory RMS. U ovom slučaju Dan registrirati za besplatna pretplata korisnik RMS-a za osobe da bi se čitati zaštićenim datotekama.

Ako je Dan prvog korisnika s adresom e-pošte iz BellowsCollege.com registrirati za ovaj samostalne koja nudi, a zatim neupravljani direktorija za stvorit će se BellowsCollege.com u Azure AD. Ako drugim korisnicima s domene BellowsCollege.com registrirate za ovu ponudu ili slične samostalno nuditi, oni će imati e-pošte provjeriti korisnički računi stvoreni u istom neupravljani direktoriju u Azure.

## <a name="admin-experience"></a>Sučelje za administratore

Administrator vlasniku DNS naziva domene neupravljani Azure directory možete preuzeti ili spajanje imenika nakon potvrđivanje vlasništva. U sljedećim odjeljcima objašnjavaju okruženja za administratore u više detalja, ali ovo je sažetak:

- Kada preuzimanje uloge neupravljani Azure directory, jednostavno postaju globalni administrator neupravljani direktorija. To se katkad naziva interne takeover.
- Kada spajanje neupravljani Azure directory, DNS naziva domene u neupravljanom imenik dodati upravljani Azure direktorij i stvara se mapiranje resursa korisnicima tako da korisnici mogu nastaviti pristupati servisima bez prekida. To se katkad naziva vanjski takeover.

## <a name="what-gets-created-in-azure-active-directory"></a>Što se stvaraju Azure Active Directory?

#### <a name="directory"></a>direktorija

- U imeniku Azure Active Directory za domenu je stvoriti jednu direktorija po domene.
- Azure AD directory sadrži bez globalni administrator.

#### <a name="users"></a>Korisnici

- Za svakog korisnika koji je registrira u direktoriju Azure AD stvara se korisničkom objektu.
- Svaki korisnik objekt je označena kao vanjska.
- Svakom korisniku dali pristup servis koji su se registrirali.

### <a name="how-do-i-claim-a-self-service-azure-ad-directory-for-a-domain-i-own"></a>Kako preuzeti samostalno Azure AD imeničkog za sam vlasnik domene?

Možete preuzeti samostalno Azure AD imeničkog izvođenjem provjere valjanosti domene. Provjera valjanosti domene dokazuje da ste vlasnik domene stvaranjem DNS zapisa.

Da biste učinili DNS takeover direktorija za Azure AD na dva načina:

- Interna takeover (administrator otkrije neupravljani Azure directory i želi pretvoriti u upravljani direktorij)
- Vanjski takeover (administrator pokušava da biste dodali novu domenu upravljanih Azure directory)

Možda će vas u provjeri valjanosti da ste vlasnik domene jer prenosite putem neupravljani direktorija kada korisnik izvršiti samostalno prijava ili možda dodavanje nove domene u postojeći upravljani imenik. Ako, na primjer, imate domenu pod nazivom contoso.com, a želite dodati novu domenu pod nazivom contoso.co.uk ili contoso.uk.

## <a name="what-is-domain-takeover"></a>Što je takeover domene?  

U ovom se odjeljku opisuje kako provjeriti da ste vlasnik domene

### <a name="what-is-domain-validation-and-why-is-it-used"></a>Što je provjera valjanosti domene i zašto se koristi?

Da bi se izvođenje operacije na imenik Azure AD potrebna je provjera valjanosti vlasništvo nad domenom DNS-a.  Provjera valjanosti domene omogućuje vam da nećete polagati imenik, a zatim Promicanje directory samostalno radi upravljani direktorij ili imenika samostalno spojiti postojeći upravljani imenik.

## <a name="examples-of-domain-validation"></a>Primjeri provjere valjanosti domene

Da biste učinili DNS takeover od direktorij na dva načina:

+ Interna takeover (Ako, na primjer, administrator otkrije samostalno, neupravljani direktorija i želi pretvoriti u upravljani direktorij)
+ Vanjski takeover (na primjer, administrator sustava pokuša da biste dodali novu domenu upravljani direktorij)

### <a name="internal-takeover---promote-a-self-service-unmanaged-directory-to-be-a-managed-directory"></a>Interna Takeover - Promicanje samostalno, neupravljani direktorij bio upravljani direktorij

Kada to učinite Interna takeover, imenik dobiva pretvara iz neupravljani imenika upravljani direktorij. Koje morate poduzeti DNS domene naziv provjere valjanosti, gdje stvarate MX zapis ili TXT zapis u DNS zone. Akcija:

+ Provjeri valjanost da ste vlasnik domene
+ Čini upravlja imenika
+ Čini globalni administrator direktorija

Recimo da IT administrator iz Bellows studentski otkrije da korisnici iz obrazovna ustanova se registrirali za samostalno ponude. Registrirani vlasnik DNS-om naziv BellowsCollege.com, IT administrator možete provjeriti vlasništvo nad naziv DNS-a u Azure i preuzeti neupravljani direktorija. Imenik zatim postaje upravljani direktorij i IT administrator je dodijeljena uloga globalnog administratora za direktorij BellowsCollege.com.

### <a name="external-takeover---merge-a-self-service-directory-into-an-existing-managed-directory"></a>Vanjski Takeover - direktorij samostalno spojiti postojeći upravljani imenik

U vanjskim takeover, već imate upravljani direktorij i želite da se svi korisnici i grupe iz imenika neupravljani da biste se uključili tu upravljani direktorij umjesto vlastite dva odvojite direktorija.

Kao administrator upravljani direktorij dodati domenu, a da bi se u neupravljanom imeniku pridružena će se dogoditi tu domenu.

Ako, na primjer, recimo da ste IT administrator i već upravljani direktorij Contoso.com, naziv domene koji je registriran za vašu organizaciju. Otkrijte da korisnike u tvrtki ili ustanovi izvršite samostalnom prijavom prema gore za programa nuditi tako da koristi naziv domene za e-pošte user@contoso.co.uk, koji je naziv druge domene vlasništvu u vašoj tvrtki ili ustanovi. Ti korisnici trenutno imate računi u neupravljanom direktorija za contoso.co.uk.

Ne želite upravljati dva zasebna direktorija, pa neupravljani direktorija za contoso.co.uk spojiti postojeći imenik kojim upravlja IT za contoso.com.

Vanjski takeover slijedi isti postupak provjere valjanosti DNS kao interne takeover.  Razlika prikaza: korisnicima i usluge neispravno direktorij kojim upravlja IT.

#### <a name="whats-the-impact-of-performing-an-external-takeover"></a>Kakav je učinak na izvođenje vanjski takeover?

S vanjskim takeover, mapiranje resursa korisnicima stvorit će se tako da korisnici mogu se i dalje pristupati servisima bez prekida. Mnogo programa osobama, uključujući RMS rukovati mapiranje resursa korisnicima i i korisnici mogu nastaviti da biste pristupili tim servisima bez promjena. Ako aplikaciju rukovati mapiranje resursa korisnicima učinkovito, vanjski takeover možda izričito blokira onemogućavanje nisku iskustvo.

#### <a name="directory-takeover-support-by-service"></a>direktorija takeover podršku usluga

Sljedeći servisi podržavamo takeover:

- RMS-A


Sljedeći servisi će biti uskoro podrške takeover:

- PowerBI

Sljedeće ne i potrebni su dodatni administrator akcija za migriranje korisničkih podataka nakon vanjski takeover.

- SharePoint OneDrive


## <a name="how-to-perform-a-dns-domain-name-takeover"></a>Kako izvesti takeover naziv domene s DNS-a

Imate nekoliko mogućnosti za izvođenje provjere valjanosti domene (i učiniti je takeover ako želite):

1.  Portal za upravljanje Azure

    Na takeover aktivira se način dodavanja za domenu.  Ako za domenu već postoji direktorij, imat ćete mogućnost da biste izvršili vanjski takeover.

    Prijavite se na portal Azure pomoću vjerodajnica.  Otvorite postojeći imenik, a zatim **Dodaj domenu**.

2.  Office 365

    Mogućnosti na stranici [Upravljanje domenama](https://support.office.com/article/Navigate-to-the-Office-365-Manage-domains-page-026af1f2-0e6d-4f2d-9b33-fd147420fac2/) u sustavu Office 365 možete koristiti da biste radili s domenama i DNS zapisima. Potražite u članku [Potvrđivanje domene u sustavu Office 365](https://support.office.com/article/Verify-your-domain-in-Office-365-6383f56d-3d09-4dcb-9b41-b5f5a5efd611/).

3.  Komponente Windows PowerShell

    Sljedeće korake potrebne za izvršavanje Provjera valjanosti pomoću komponente Windows PowerShell.

    Korak    |   Cmdlet za korištenje
    ------- | -------------
    Stvaranje objekta vjerodajnica | Get-vjerodajnica
    Povezivanje s Azure AD | Povezivanje MsolService
    dobit ćete popis domena   | Get-MsolDomain
    Stvaranje na pitanja i odgovora  | Get-MsolDomainVerificationDns
    Stvaranje DNS zapisa   | To učinite na DNS poslužitelj
    Provjerite je li u pitanja i odgovora    | Potvrda MsolEmailVerifiedDomain

Ako, na primjer:

1. Povezivanje s Azure AD pomoću vjerodajnica korištenima odgovoriti nuditi samostalno: Uvoz modul MSOnline $msolcred = povezivanje vjerodajnicu za get-msolservice-vjerodajnica $msolcred

2. Pojavljuje se popis domena:

    Get-MsolDomain

3. Zatim pokrenite cmdlet Get-MsolDomainVerificationDns da biste stvorili na pitanja i odgovora:

    Get-MsolDomainVerificationDns – DomainName *naziv_vaše_domene* – DnsTxtRecord način rada

    Ako, na primjer:

    Get-MsolDomainVerificationDns – DomainName contoso.com – DnsTxtRecord način rada

4. Kopirajte vrijednost (test) koji je vratio sljedeću naredbu.

    Ako, na primjer:

    MS = 32DD01B82C05D27151EA9AE93C5890787F0E65D9

5. U javnoj DNS naziva stvaranje DNS zapisa txt koja sadrži vrijednost koju ste kopirali u prethodnom koraku.

    Naziv ovog zapisa je naziv domene nadređenog, pa ako stvorite zapis resursa pomoću uloga DNS poslužitelja sustava Windows, ostavite zapisa naziv prazan i samo lijepljenje vrijednosti u tekstni okvir

6. Pokrenite cmdlet potvrda MsolDomain da biste provjerili problem:

    Potvrda MsolEmailVerifiedDomain - DomainName *naziv_vaše_domene*

    Ako, na primjer:

    Potvrda MsolEmailVerifiedDomain - DomainName contoso.com

Uspješan test vraća upit bez pogrešaka.

## <a name="how-do-i-control-self-service-settings"></a>Kako kontrolirati samostalno postavke?

Administratori imaju dvije kontrole za samostalno danas. Možete kontrolirati:

- Hoće li korisnici mogu pridružiti directory putem e-pošte.
- Hoće li korisnici mogu licence same za aplikacija i servisa.


### <a name="how-can-i-control-these-capabilities"></a>Kako odrediti sljedeće mogućnosti?

Administrator može konfigurirati sljedeće mogućnosti pomoću cmdleta Set-MsolCompanySettings parametara Azure AD:

+ **AllowEmailVerifiedUsers** određuje hoće li korisnik možete stvoriti ili uključivanje neupravljani direktorija. Ako taj parametar postavljen na $false, bez e-pošte provjeriti korisnici mogu pridružiti imenika.
+ **AllowAdHocSubscriptions** kontrole korisnicima da biste izvršili samostalnom prijavom prema gore. Ako taj parametar postavljen na $false, bez korisnici mogu obavljati samostalno prijava.


### <a name="how-do-the-controls-work-together"></a>Kako kontrole funkcioniraju zajedno?

Da biste definirali preciznije kontrolirati samostalnom prijavom prema gore u kombinaciji se poslužite te dva parametra. Ako, na primjer, sljedeću naredbu omogućuje korisnicima izvođenje samostalnom prijavom prema gore, ali samo ako tim korisnicima već imate postavljen račun u Azure AD (drugim riječima, korisnici koji su potrebni račun e-pošte provjeriti će biti stvoren ne može izvesti samostalnom prijavom gore):

    Set-MsolCompanySettings -AllowEmailVerifiedUsers $false -AllowAdHocSubscriptions $true

Sljedeći dijagram toka objašnjava raznih kombinacija za ove parametre i konačne uvjete za direktorija i samostalnom prijavom prema gore.

![][1]

Dodatne informacije i primjeri kako koristiti parametara potražite u članku [Set-MsolCompanySettings](https://msdn.microsoft.com/library/azure/dn194127.aspx).

## <a name="see-also"></a>Vidi također

-  [Kako instalirati i konfigurirati Azure PowerShell](../powershell-install-configure.md)

-  [Azure PowerShell](https://msdn.microsoft.com/library/azure/jj156055.aspx)

-  [Azure Cmdlet Reference](https://msdn.microsoft.com/library/azure/jj554330.aspx)

-  [Set-MsolCompanySettings](https://msdn.microsoft.com/library/azure/dn194127.aspx)

<!--Image references-->
[1]: ./media/active-directory-self-service-signup/SelfServiceSignUpControls.png
