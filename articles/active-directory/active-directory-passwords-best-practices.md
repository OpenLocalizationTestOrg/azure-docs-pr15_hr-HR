<properties
    pageTitle="Dobra praksa: Azure AD lozinku upravljanje | Microsoft Azure"
    description="Praktični savjeti za implementaciju i korištenje, dokumentacija za krajnjeg korisnika uzorak i vodiči za obuku za upravljanje lozinkama u servisu Azure Active Directory."
    services="active-directory"
    documentationCenter=""
    authors="asteen"
    manager="femila"
    editor="curtand"/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/12/2016"
    ms.author="asteen"/>

# <a name="deploying-password-management-and-training-users-to-use-it"></a>Implementacija upravljanja lozinkom i obuka korisnika je koristiti

> [AZURE.IMPORTANT] **Jeste li ovdje jer je što imate poteškoća s prijavom?** Ako je tako, [Evo kako možete promijeniti i ponovno postavili vlastitu lozinku](active-directory-passwords-update-your-own-password.md).

Nakon što ponovno postavljanje lozinke za omogućivanjem, na sljedeći korak koji morate poduzeti je korisnicima putem servisa sustava u tvrtki ili ustanovi. Da biste to učinili, morat ćete provjerite jesu li vaši korisnici konfigurirane korištenja servisa ispravno i imaju li vaši korisnici obuka moraju biti uspješno u upravljanju vlastite lozinke. U ovom se članku će se objašnjava što vam sljedeće koncepata:

* [**Kako nabaviti korisnike koji je konfiguriran za upravljanje lozinkama**](#how-to-get-users-configured-for-password-reset)
  * [Što je račun konfiguriran za ponovno postavljanje lozinke čini](#what-makes-an-account-configured)
  * [Načini za popunjavanje podataka za provjeru autentičnosti](#ways-to-populate-authentication-data)
* [**Najbolji načini uvođenju za vraćanje izvorne lozinke u tvrtku ili ustanovu**](#what-is-the-best-way-to-roll-out-password-reset-for-users)
  * [Uvođenje e-pošte + uzorka komunikacije e-pošte](#email-based-rollout)
  * [Stvaranje vlastite portal za upravljanje prilagođene lozinke za korisnike](#creating-your-own-password-portal)
  * [Kako koristiti nametnuti Registracija da biste nametnuli korisnicima da biste registrirali na prijava](#using-enforced-registration)
  * [Upute za prijenos podataka za provjeru autentičnosti za korisničke račune](#uploading-data-yourself)
* [**Uzorak korisnika i materijala za obuku podršku (uskoro dostupno!)**](#sample-training-materials)

## <a name="how-to-get-users-configured-for-password-reset"></a>Kako nabaviti korisnici konfigurirana za ponovno postavljanje lozinke
U ovom se odjeljku opisuju vam različite metode u kojima možete biti sigurni da svaki korisnik u tvrtki ili ustanovi možete koristiti samostalno ponovno postavljanje lozinke učinkovito u slučaju da ih zaboraviti lozinku.

### <a name="what-makes-an-account-configured"></a>Što čini račun koji je konfiguriran
Korisnik može koristiti ponovno postavljanje lozinke, **Svi** sljedeći uvjeti zadovoljeni:

1.  Ponovno postavljanje lozinke mora biti omogućen u direktoriju.  Saznajte kako omogućiti za vraćanje izvorne lozinke tako da pročitate [korisnicima omogućili da ponovno postavljanje lozinke Azure AD](active-directory-passwords-getting-started.md#enable-users-to-reset-their-azure-ad-passwords) ili [korisnicima omogućili da ponovno postavljanje ili promjena lozinke AD](active-directory-passwords-getting-started.md#enable-users-to-reset-or-change-their-ad-passwords)
2.  Korisnik mora biti licenciran.
 - Za korisnike oblaka, korisnik mora imati **nikakvu plaćenu licencu za Office 365**ili **AAD osnovni** ili dodijeljena **licenca AAD Premium** .
 - Lokalno korisnicima (vanjskog ili raspršivanje sinkronizirali), korisnik **mora imati dodijelili licencu za AAD Premium**.
3.  Korisnik mora imati **minimalne skup podataka za provjeru autentičnosti definirani** skladu trenutni pravila za ponovno postavljanje lozinke.
 - Podatke za provjeru autentičnosti smatra definirani odgovarajuće polje u direktoriju sadrži li ispravnog podatke.
 - Minimalne skupa podataka za provjeru autentičnosti je definiran na **najmanje jedan** mogućnosti omogućeno provjere autentičnosti ako je konfiguriran jedan prolaz pravila ili **barem dvije** mogućnosti omogućeno provjere autentičnosti ako je konfiguriran dva prolaz pravila.
4.  Ako korisnik koristi lokalni račun, zatim [Lozinku Upisima](active-directory-passwords-getting-started.md#enable-users-to-reset-or-change-their-ad-passwords) mora biti omogućen i uključen

### <a name="ways-to-populate-authentication-data"></a>Načini popunjavanje podataka za provjeru autentičnosti
Imate nekoliko mogućnosti da biste odredili podatke za korisnike u tvrtki ili ustanovi koja će se koristiti za ponovno postavljanje lozinke.

- Uređivanje korisnika u [Portal za upravljanje Azure](https://manage.windowsazure.com) ili [Portala za administratore sustava Office 365](https://portal.microsoftonline.com)
- Korištenje Azure AD sinkronizaciju da biste sinkronizirali korisničkim svojstvima u Azure AD iz lokalne domene servisa Active Directory
- Pomoću komponente Windows PowerShell za uređivanje svojstava korisničkih tako da [slijedite korake u nastavku](active-directory-passwords-learn-more.md#how-to-access-password-reset-data-for-your-users).
- Dopusti korisnicima da biste registrirali vlastite podatke tako da ih navođenjem portal za registraciju na [http://aka.ms/ssprsetup](http://aka.ms/ssprsetup)
- Od korisnika da biste registrirali za kada se prijaviti na svoj račun za Azure AD postavljanjem za ponovno postavljanje lozinke u [**Zatraži od korisnika da biste registrirali prilikom prijave?**](active-directory-passwords-customize.md#require-users-to-register-when-signing-in) mogućnost konfiguracije na **da**.

Korisnici moraju Registrirajte se za ponovno postavljanje lozinke za sustav za rad.  Ako, na primjer, ako imate broj za pozive iz mobilne postojeće mreže ili office telefonskih brojeva u lokalnom direktoriju, ih možete sinkronizirati Azure AD i koristit ćemo ih za automatski za vraćanje izvorne lozinke.

Možete čitati i dodatne informacije o [korištenja podataka lozinkom vratiti](active-directory-passwords-learn-more.md#what-data-is-used-by-password-reset) i [kako popuniti pojedinačne provjere autentičnosti polja sa servisom PowerShell](active-directory-passwords-learn-more.md#how-to-access-password-reset-data-for-your-users).

## <a name="what-is-the-best-way-to-roll-out-password-reset-for-users"></a>Koji je najbolji način distribucija za vraćanje izvorne lozinke za korisnike?
Slijede općenite uvođenje korake za ponovno postavljanje lozinke:

1.  Omogućiti za vraćanje izvorne lozinke u direktoriju tako da odete do kartice **Konfiguriraj** [Azure Portal za upravljanje](https://manage.windowsazure.com) , a zatim odaberete **da** za mogućnost **Korisnicima omogućiti za ponovno postavljanje lozinke** .
2.  Dodijelili odgovarajuće licence za svakog korisnika kojemu želite ponuditi za vraćanje izvorne lozinke u na tako da odete do kartice **licence** za [Portal za upravljanje Azure](https://manage.windowsazure.com).
3.  Ako želite ograničiti za vraćanje izvorne lozinke da biste grupi korisnika distribucija značajku sporo vremenom postavka preklop **Ograničavanje pristupa za ponovno postavljanje lozinke** za **da** i odabirom sigurnosne grupe da biste omogućili za ponovno postavljanje lozinke (Imajte na umu ti korisnici moraju svi imati licence dodijeljene).
4.  Uputite korisnike da biste koristili za vraćanje izvorne lozinke slanjem ili poruku e-pošte podešavanju ih da biste registrirali, omogućivanje provest Registracija teksta ili prijenos podataka odgovarajuće provjere autentičnosti za ti korisnici sami putem DirSync, PowerShell ili [Portal za upravljanje Azure](https://manage.windowsazure.com).  Dodatne informacije o tome navedene u nastavku.
5.  Tijekom vremena, pregledajte korisnici Registracija tako da odete do kartice izvješća i prikaz izvješća [**Aktivnosti Registracija ponovno postavljanje lozinke**](active-directory-passwords-get-insights.md#view-password-reset-registration-activity) .
6.  Nakon što ste registrirali dobar broj korisnika, oni će se koristiti za vraćanje izvorne lozinke tako da odete do kartice izvješća i prikaz izvješća [**Aktivnosti ponovno postavljanje lozinke**](active-directory-passwords-get-insights.md#view-password-reset-activity) .

Obavještavanje korisnika koji mogu se registrirati za i koristiti za vraćanje izvorne lozinke u tvrtki ili ustanovi na nekoliko načina.  Su detaljno opisane ispod.

### <a name="email-based-rollout"></a>Uvođenje e-pošte
Možda je najjednostavniji način za obavještavanje korisnika o registrirati za ili koristite lozinku ponovno postaviti tako da im pošaljete poruku e-pošte podešavanju ih da biste to učinili.  U nastavku je predložak koji možete koristiti da biste to učinili.  Slobodno da biste zamijenili boja / logotipa s onima vlastitu odabir da biste prilagodili da odgovara vašim potrebama.

  ![][001]

Možete [preuzeti ovdje predložak e-pošte](http://1drv.ms/1xWFtQM).

### <a name="creating-your-own-password-portal"></a>Stvaranje vlastite portal za lozinke
Jedna od mogućih strategija je dobro funkcionira veće kupci implementacija mogućnosti upravljanja lozinku da biste stvorili jedan "lozinku portal" koje korisnici mogu koristiti za upravljanje sve što vezane uz svoje lozinke na jednom mjestu.  

Mnoge od najvećeg klijentima odaberite da biste stvorili korijenski stavku DNS-a, kao što je https://passwords.contoso.com s vezama na Azure AD lozinke ponovno postavljanje portala, portala za registraciju za ponovno postavljanje lozinke i stranice za promjenu lozinke.  Na taj način u porukama e-pošte ni fliers pošaljete, možete uključiti jedan Divno, URL koji korisnici mogu prijeći za čije su druge za početak rada sa servisom.

Za brz početak ovdje, ne možemo ste stvorili jednostavan stranice koja koristi u najnoviji odredište korisničkog Sučelja dizajn paradigms i će funkcionirati na svim preglednicima i mobilnih uređaja.

  ![][007]

Možete [preuzeti ovdje predložak web-mjesta](https://github.com/kenhoff/password-reset-page).  Preporučujemo da prilagodbi logotip i boje za potrebe tvrtke ili ustanove.

### <a name="using-enforced-registration"></a>Korištenje nametnuti Registracija
Ako želite da korisnici registrirati za vraćanje izvorne lozinke sami, ih da biste registrirali kada se prijaviti na ploču pristup pri [http://myapps.microsoft.com](http://myapps.microsoft.com)možete i pokrenuti.  Tu mogućnost, na kartici **Konfiguriraj** vaš imenik možete omogućiti tako da omogućite mogućnost **Potrebna korisnicima Register prilikom prijave na ploču programa Access** .  

Također možete odrediti hoće li oni će se zatražiti da ponovno registrirajte nakon konfigurirati vremenskog razdoblja izmjenom mogućnost **broj dana prije korisnici morate potvrditi svoje podatke za kontakt** da biste dobili vrijednosti od nule. Dodatne informacije potražite u članku [Prilagodba ponašanje upravljanje korisničko lozinku](active-directory-passwords-customize.md#password-management-behavior) .

  ![][002]

Nakon što ste omogućili tu mogućnost, korisnici za prijavu na ploču programa access, prikazat će se skočni prozor koja ih obavještava da njihove administrator je potrebno ih da biste provjerili svoje podatke za kontakt. Će ga moći koristiti da biste ponovno postavili svoju lozinku ako oni ikad izgubiti pristup svoj račun.

  ![][003]

**Provjerite je li sada** klikom ih premješta na **portal za registraciju za ponovno postavljanje lozinke** na [http://aka.ms/ssprsetup](http://aka.ms/ssprsetup) web-mjesta i potrebne da biste registrirali.  Registracija putem ovu metodu koje se mogu nehotice tako da kliknete gumb **Odustani** ili zatvaranjem prozora, ali korisnicima se podsjetnik svaki put kada se prijavite se ako se registrirali.

  ![][004]

### <a name="uploading-data-yourself"></a>Prijenos podataka
Ako želite prenijeti podatke za provjeru autentičnosti, zatim korisnici moraju ne Registrirajte se za za vraćanje izvorne lozinke mogli za ponovno postavljanje lozinke.  Pod uvjetom da korisnici imaju pravila koje ste definirali za ponovno postavljanje podataka za provjeru autentičnosti definiran na svoj račun koji ispunjava lozinku, a zatim ti korisnici će moći ponovno postavljanje lozinke.

Da biste saznali što svojstva koje možete postaviti putem povezivanje AAD ili komponente Windows PowerShell potražite u članku [podataka koje koriste ponovno postavljanje lozinke](active-directory-passwords-learn-more.md#what-data-is-used-by-password-reset).

Ne možete prenositi podataka za provjeru autentičnosti putem [Portal za upravljanje Azure](https://manage.windowsazure.com) slijedeći korake u nastavku:

1.  Dođite do direktorija u **proširenja za Active Directory** za [Portal za upravljanje Azure](https://manage.windowsazure.com).
2.  Na kartici **korisnika** , kliknite.
3.  Odabir korisnika koji vas zanima s popisa.
4.  Na kartici prvi tražit će **Zamjenska e-pošte**, koji se može koristiti kao svojstvo da biste omogućili ponovno postavljanje lozinke.

    ![][005]

5.  Kliknite na kartici **Informacije rad** .
6.  Na ovoj stranici ćete pronaći **Office telefona**, **mobilni telefon**, **Telefona za provjeru autentičnosti**i **Provjeru autentičnosti e-pošte**.  Tih svojstava moguće je postaviti i da biste korisniku za ponovno postavljanje lozinke upisom.

    ![][006]

U odjeljku da biste vidjeli kako svaki od tih svojstava mogu koristiti [podataka koje koriste ponovno postavljanje lozinke](active-directory-passwords-learn-more.md#what-data-is-used-by-password-reset) .

Pogledajte [kako pristupiti lozinku ponovno postavljanje podataka za korisnike iz PowerShell](active-directory-passwords-learn-more.md#how-to-access-password-reset-data-for-your-users) da biste vidjeli kako mogu čitati i postavljanje te podatke sa servisom PowerShell.

## <a name="sample-training-materials"></a>Ogledna materijal za obuku
Radimo na materijala za obuku uzorka koje možete koristiti da biste dobili IT organizacija i vaši korisnici ukorak brzo kako uvesti i koristiti ponovno postavljanje lozinke.  Ostanite definirati!


<br/>
<br/>
<br/>

## <a name="links-to-password-reset-documentation"></a>Dokumentacija za ponovno postavljanje veze na lozinke
U nastavku su veze na sve stranice dokumentaciju za ponovno postavljanje lozinke za Azure AD:

* **Jeste li ovdje jer je što imate poteškoća s prijavom?** Ako je tako, [Evo kako možete promijeniti i ponovno postavili vlastitu lozinku](active-directory-passwords-update-your-own-password.md).
* [**Kako to funkcionira**](active-directory-passwords-how-it-works.md) – Saznajte više o šest različitih komponenata servisa i što svaki ne
* [**Početak rada**](active-directory-passwords-getting-started.md) – upute kako bi korisnici mogli vratiti i mijenjati oblaka ili na lokalnim poslužiteljima lozinke
* [**Prilagodba**](active-directory-passwords-customize.md) – Saznajte kako prilagoditi izgled i dojam i ponašanje servisa potrebama vaše tvrtke ili ustanove
* [**Početak uvida**](active-directory-passwords-get-insights.md) – dodatne informacije o oglednim integrirane mogućnosti izvješćivanja
* [**Najčešća pitanja vezana uz**](active-directory-passwords-faq.md) – Pronađite odgovore na najčešća pitanja
* [**Otklanjanje poteškoća**](active-directory-passwords-troubleshoot.md) – Saznajte kako brzo otklanjanje poteškoća sa servisom
* [**Saznajte više**](active-directory-passwords-learn-more.md) – otvorite obuhvaća više razina u tehničke pojedinosti funkcioniranje servisa



[001]: ./media/active-directory-passwords-best-practices/001.jpg "Image_001.jpg"
[002]: ./media/active-directory-passwords-best-practices/002.jpg "Image_002.jpg"
[003]: ./media/active-directory-passwords-best-practices/003.jpg "Image_003.jpg"
[004]: ./media/active-directory-passwords-best-practices/004.jpg "Image_004.jpg"
[005]: ./media/active-directory-passwords-best-practices/005.jpg "Image_005.jpg"
[006]: ./media/active-directory-passwords-best-practices/006.jpg "Image_006.jpg"
[007]: ./media/active-directory-passwords-best-practices/007.jpg "Image_007.jpg"
