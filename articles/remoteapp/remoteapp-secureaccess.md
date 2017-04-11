
<properties 
    pageTitle="Zaštita pristup Azure RemoteApp i izvan | Microsoft Azure"
    description="Saznajte koliko siguran pristup Azure RemoteApp pomoću uvjetnog programa access u servisu Azure Active Directory"
    services="remoteapp"
    documentationCenter="" 
    authors="piotrci" 
    manager="mbaldwin" />

<tags 
    ms.service="remoteapp" 
    ms.workload="compute" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/15/2016" 
    ms.author="elizapo" />

# <a name="securing-access-to-azure-remoteapp-and-beyond"></a>Zaštita pristup Azure RemoteApp i izvan

> [AZURE.IMPORTANT]
> Ukinute Azure RemoteApp je u tijeku. Čitanje [Objava](https://go.microsoft.com/fwlink/?linkid=821148) detalje.

U ovom članku ćemo vam kako administrator postaviti kanala sigurnog pristupa počevši od krajnjeg korisnika pomoću servisa Azure RemoteApp i završavaju sigurne resursa kao što su baze podataka SQL ili neki drugi program pozadinske pregled. Cilj je da biste provjerili samo autoriziranih korisnika željene uvjete za sastanak možete pristupiti daljinski aplikacije, a da sigurne pozadinskoj može pristupiti samo iz nadziranim Azure RemoteApp okruženja, a ne s drugih mjesta.

Postoje 3 glavna područja, administrator mora pogledajte:

![Azure RemoteApp uvjetno pristup pitanja vezana uz](./media/remoteapp-secureaccess/ra-conditionalenvironment.png)

Čitanje na informacije i odgovori na ta pitanja.

## <a name="who-can-access-the-collection"></a>Tko može pristupiti u zbirci?
Administrator odabire korisnika koji mogu pristupiti daljinski aplikacije u zbirci. Možete koristiti Azure Active Directory (Azure AD) ili školske računi (prije se zvao, "računi tvrtke ili ustanove") ili Microsoft računi (npr. @outlook.com). Većina tvrtki scenariji koristite Azure AD računa. oni omogućuju korištenje uvjetnog pristup značajke spominju kasnije i su i jedini izbor za domenu pridruženo zbirke. Ostatak u članku pretpostavlja da koristite Azure AD računi sa Azure RemoteApp.

**Što smo ste napraviti:**

Korištenje računa za Azure AD za kontrolu pristupa Azure RemoteApp daje nam dvije stvari:

1.  Ne možemo uvijek znali tko može pristupiti aplikacijama ćemo objavljeno i pristupati svim natrag zaustavlja se tim aplikacijama povezati.
2.  Ne možemo kontrolirati podlozi Azure AD tako da možemo stvoriti i brisanje korisničkih računa, postavljanje lozinki, koristiti višestruke provjere autentičnosti, itd. 

## <a name="how-is-the-collection-accessed-from-where"></a>Kako se u zbirku pristupa? Odakle?
Najčešće administratori želite definirati pravila za pristup javno mjesto na Internetu okruženju, kao što su Azure RemoteApp. Na primjer, žele da biste bili sigurni da korisnici pristupa u okruženju izvan poslovnoj mreži morate koristiti višestruke provjere autentičnosti (MFA) da biste pristupili; ili možda su blokirani potpuno.

Azure RemoteApp administratori mogu koristiti funkcije dostupne putem Azure AD Premium da biste postavili pravila uvjetnog pristupa za svoje okruženje Azure RemoteApp. Izvješćivanje bogatih upozorenjem značajke mogu koristiti i praćenje kako je pristupa okruženje.

### <a name="how-to-set-up-conditional-access-for-azure-remoteapp"></a>Kako postaviti uvjetno pristup za Azure RemoteApp
Ne možemo prolaze da biste prošli kroz scenarij primjer – Azure RemoteApp administrator želi da biste blokirali pristup okruženje kada korisnici su izvan poslovnoj mreži.

>[AZURE.NOTE] Pretpostavimo da ste nadogradili Azure AD sloju Premium i da ste stvorili najmanje jednu zbirku Azure RemoteApp.

1.  Azure portalu karticu **Servisa Active Directory** . Zatim imenik želite konfigurirati.

    Imajte na umu: Uvjetno pristup je svojstvo imenik, a ne Azure RemoteApp tako da sva je gotova konfiguracija i na razini direktorija. To također znači da morate biti administrator direktorija da te promjene.

2.  Kliknite **aplikacije**, a zatim kliknite **Microsoft Azure RemoteApp** da biste postavili uvjetno programa access. Imajte na umu da možete postaviti uvjetno pristupa za svaku aplikaciju "softver kao usluga" u direktoriju zasebno.
![Postavljanje uvjetno pristupa za Azure RemoteApp](./media/remoteapp-secureaccess/ra-conditionalaccessscreen.png)
 

3.  Na kartici **Konfiguriraj** postavite **Pravila za omogućiti pristup** na Uključeno.
![Omogućivanje pristupa pravila za Azure RemoteApp](./media/remoteapp-secureaccess/ra-enableaccessrules.png)
 

4.  Sada možete konfigurirati različita pravila i odaberite tko da biste ih da biste primijenili:

    1. Odaberite **Blokiranje pristupa kada nisu na poslu** da biste potpuno korisnicima onemogućili pristup Azure RemoteApp izvan mrežnom okruženju koju navedete.
    2. Kliknite mogućnost dolje da biste definirali rasponi IP adresa koji čine "pouzdanih mreže". Sve izvan onih će odbijena.

5.  Konfiguraciju testirajte pokretanje klijenta Azure RemoteApp s IP adresom izvan raspona koje ste naveli. Kada se prijavite pomoću vjerodajnica za Azure AD trebali biste vidjeti poruku ovako:

![Pristup odbijen Azure RemoteApp](./media/remoteapp-secureaccess/ra-accessdenied.png)
 

### <a name="future-conditional-access-features"></a>Značajke za buduće uvjetno programa access 
Tim za Azure Active Directory radi na nove mogućnosti uvjetnog programa Access. Administratori mogu stvoriti nove vrste pravila izvan mreže mjestu pravila. Javni pretpregled o novim funkcijama mora biti uskoro dostupno.

### <a name="how-to-monitor-access-to-azure-remoteapp"></a>Upute za praćenje pristup Azure RemoteApp
Odlične značajke da biste koristili duž uvjetnog pristup je funkcija izvješćivanja Azure Active Directory Premium. Možete koristiti izvješća nadzor nad pristupa okruženju sustava i otkriti bilo koju sumnjivu aktivnost.

Na primjer, možete vidjeti imena korisnika kojima se pristupa Azure RemoteApp koliko je puta oni jeste li i kada.

1.  Portalu za Azure **Active Directory**kliknite, a zatim direktorija.

2.  Idite na karticu **izvješća** .

3.  Na popisu izvješća odaberite **korištenje aplikacije** u odjeljku **integrirane aplikacije**.

    Vidjet ćete neke Zbrojeno Statistika za Azure RemoteApp. 
![Zbrojeno stat Azure RemoteApp programa access](./media/remoteapp-secureaccess/ra-accessstats.png)
 
5.  Kliknite aplikaciju da biste vidjeli informacije o korisnicima pristup Azure RemoteApp.
![Korisnički pristup stat za Azure RemoteApp](./media/remoteapp-secureaccess/ra-userstats.png)
 
### <a name="summary"></a>Sažetak
S Azure Active Directory Premium možete postaviti pravila za pristup Azure RemoteApp (i ostalog softvera kao servisne aplikacije dostupne putem Azure AD). Pravila trenutno ograničeni su na mreži mjestu pravila, no u budućnosti će se proširiti ostalih aspekata korporacijsko upravljanje.

Azure AD Premium nudi izvješćivanje i mogućnosti koje dodatno proširiti kontrolu administrator za nadzor pred svoje okruženje za Azure RemoteApp.

## <a name="how-do-i-make-sure-my-secure-resource-is-accessible-only-from-my-azure-remoteapp-environment"></a>Kako mogu provjeriti Moje sigurne resursa dostupna je samo iz Moje okruženje Azure RemoteApp?
U ranijim odlomcima ovog članka možemo filtriran na zaštita pristup okruženje za Azure RemoteApp. Ne možemo ste koji postiže odabir korisnicima koji imaju dozvolu pristupa te postavljanje pravila za pristup da biste dodatno upravljali način korištenja servisa.

Uobičajeni scenarij za Azure RemoteApp implementacije je da udaljena aplikacije moraju možete komunicirati s pozadinske resursa, primjerice bazom podataka SQL. Ovaj resurs nalazi bilo lokalno (npr. u mreži tvrtke) ili u oblak (npr. u Azure IaaS). Administratori često želite biti sigurni da resursa pozadinske može pristupiti samo aplikacija implementiran putem Azure RemoteApp, a ne na primjer aplikacija pokretanja izravno na Računalu korisnika i pristup putem javnog Interneta. Azure RemoteApp često prikazivati kao okruženje za središnje upravljane i zaštićenim a time i samo put kroz koje korisnici upotrebljavaju treba pozadinske resursa.

Rješenje je da biste postavili okruženje za Azure RemoteApp i sigurne resursa u na istom Azure virtualne mreže (VNET). Ako je resurs u drugom web-mjestu, možete uspostaviti VPN veza web-mjesto, na primjer da biste stvorili VNet koje se protežu na centar za Azure podataka i lokalnog okruženja klijenta.

Azure RemoteApp podržava dvije vrste implementacijama zbirke koje možete unijeti vlastite VNET:

-   Osobe koje nisu-domene-pridruženo: aplikacija neće imati "Pogled" druge resurse u na VNET. Ako, na primjer, to možete koristi za povezivanje aplikacije s bazom podataka SQL koja koristi provjeru autentičnosti SQL (aplikacije provjere autentičnosti korisnika izravno u odnosu na bazu podataka)

-   Domene pridruženo: virtualnim strojevima koristi Azure RemoteApp pridruženim kontroler domene u na VNET. To je korisno kada aplikacija morate dobiti pristup resursu pozadinsku provjeru autentičnosti kontroleru domene sa sustavom Windows.
![Zbirka domene pridruženo RemoteApp Azure](./media/remoteapp-secureaccess/ra-domainjoined.png)
 
### <a name="how-to-create-a-secure-connection-between-azure-and-my-on-premises-environment"></a>Kako stvoriti sigurnu vezu između Azure i Moje lokalnog okruženja
Postoji nekoliko mogućnosti konfiguracije za povezivanje okruženja u kojima Azure i lokalnih. Dobar pregled mogućnosti dostupna je u nastavku.

S Azure RemoteApp morate konfigurirati svoje VNet prvi put, a zatim je pomoću tijekom stvaranja zbirke web. 

## <a name="the-complete-solution"></a>Potpuno rješenje
Dijagramu u nastavku prikazuje potpuno rješenje gdje ćemo imaju ugrađenu kanala sigurnog pristupa iz krajnjeg korisnika po stavci RemoteApp Azure (ARA), pozadinskog resursa.
![Sigurne Azure RemoteApp](./media/remoteapp-secureaccess/ra-secureoverview.png) u fazu 1 smo odabrane korisnike, a zatim stvoriti pravila za pristup koje određuju kako se može pristupiti ARA. U primjeru u nastavku ćemo samo dopustiti pristup korisnicima koji rade s korporacijskom mrežom. Osobe koje nisu usklađene sa korisnici nećete moći pristupiti u okruženju ARA uopće.
U "Fazi 2" ćemo imati izložen resursa pozadinskog samo putem konfiguracije VNet/VPN-a koji smo kontrolu. Azure RemoteApp stavljen u istoj VNet. Rezultat je da resurs može pristupiti samo putem ARA okruženju.


