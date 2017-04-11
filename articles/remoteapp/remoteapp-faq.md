<properties 
    pageTitle="Azure RemoteApp najčešća pitanja vezana uz | Microsoft Azure" 
    description="Saznajte odgovore na najčešće najčešća pitanja o Azure RemoteApp." 
    services="remoteapp" 
    documentationCenter="" 
    authors="lizap" 
    manager="swadhwa" 
    editor=""/>

<tags 
    ms.service="remoteapp" 
    ms.workload="compute" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="get-started-article" 
    ms.date="08/15/2016" 
    ms.author="elizapo"/>

# <a name="azure-remoteapp-faq"></a>Najčešća pitanja vezana uz Azure RemoteApp

> [AZURE.IMPORTANT]
> Ukinute Azure RemoteApp je u tijeku. Čitanje [Objava](https://go.microsoft.com/fwlink/?linkid=821148) detalje.

Koja na sljedeća pitanja o Azure RemoteApp. Imate drugim korisnicima? Posjetite [forume RemoteApp](https://social.msdn.microsoft.com/Forums/azure/home?forum=AzureRemoteApp) i Recite nam što vam je potrebno znati ili padajući komentar ispod.

## <a name="cant-find-what-youre-looking-for-have-a-question-we-didnt-answer"></a>Ne možete pronaći ono što tražite? Imate li pitanje smo niste odgovor?
Ako ne možete pronaći podatke morate ili koje ste dodatna pitanja koje mi se ne koji prekriva ovdje, idite na [forum Azure RemoteApp](http://aka.ms/araforum) i postavite pitanje postoji. Ne možemo uvijek možete dodati više odgovora ovdje.

## <a name="what-is-azure-remoteapp"></a>Što je Azure RemoteApp? ##


- **Što je Azure RemoteApp?** RemoteApp je Azure service pomaže vam omogućuju pristup sigurne, udaljene aplikacije s brojnim uređajima drugog korisnika. Dodatne informacije o [Azure RemoteApp](remoteapp-whatis.md).
- **Što su mogućnosti implementacije?** Postoje dvije vrste zbirki RemoteApp: oblaka i hibridnog. Koji će vam je potrebna ovisi o nekoliko čimbenika, kao što su trebate li spoj domene. Ćemo objasniti što sve te odluke [ovdje](remoteapp-collections.md).

## <a name="quick-tips-on-using-azure-remoteapp"></a>Kratke savjete o korištenju Azure RemoteApp ##
- **Koliko dugo dok se ne mogu se odvojenim? Koliko dugo biti neaktivnosti prije nego što mi dali pokretanje?** četiri sata. Ako vi ili jednog od korisnika neaktivnosti 4 sata, ćete automatski se prijaviti iz Azure RemoteApp. Pogledajte druge zadane postavke u [pretplatu Azure i ograničenja servisa, kvote, i ograničenja](../azure-subscription-service-limits.md).
- **Je li moguće isprobati servis besplatno?** Da. Nema besplatnu probnu verziju 30 dana. Nakon završetka za probno razdoblje možete prijelaz u plaćenu račun (koji možete koristiti u proizvodnje) ili prestanak korištenja servisa. Pokretanje besplatnu probnu verziju tako da [portal.azure.com](http://portal.azure.com) – da biste stvorili novu instancu RemoteApp. U besplatnu probnu verziju u možete izraditi 2 pojavljivanja RemoteApp s 10 korisnika po instance. Imajte na umu da se ove probne verzije samo nalazi 30 dana.
## <a name="azure-remoteapp-subscription-details"></a>Detalji o pretplati Azure RemoteApp ##

- **Koja su ograničenja usluga?** Koje dodatne informacije o zadane postavke i ograničenja servisa Azure RemoteApp u [pretplatu Azure i ograničenja servisa, kvote, i ograničenja](../azure-subscription-service-limits.md). Recite nam ako imate pitanja.
- **Koliko korisnika imati da bi?** Postoji najmanje 20 korisnika. Dopusti ponovite da samo da razjasnimo super - MINIMALNE iznosi 20. Će se naplatiti za 20. 
- **Koliko stoji RemoteApp trošak?** Pogledajte [Detalje cijene za Azure RemoteApp ](https://azure.microsoft.com/pricing/details/remoteapp/).
- **Jedne vrste zbirke stoji više drugu?** Da, može se, ovisno o potrebama zbirke. Zbirka hibridnog potrebna je veza Azure RemoteApp lokalne mreže. Ako koristite postojeće usmjeravanje za VNET/Express, postoji bez dodatnih troškova. No ako koristite novu VNET Azure i mora biti pristupnika ili Express usmjeravanje, koje naplatiti za [pristupnik za VPN-a](https://azure.microsoft.com/pricing/details/vpn-gateway) ili [Usmjeravanje Express](https://azure.microsoft.com/pricing/details/expressroute/). Taj trošak (detaljna veze) je pri vrhu mjesečni Azure RemoteApp cijena.

## <a name="collections---whats-supported-which-should-you-use-and-others"></a>Zbirke – što je podržano, koje morate koristiti i drugim korisnicima
- **Prilagođeni redak specifični za poslovanje (LOB) aplikacija podržava?** Da. Da biste koristili prilagođenu aplikaciju RemoteApp Azure, stvorite [prilagođeni predložak sliku](remoteapp-create-custom-image.md), a prijenos zbirke RemoteApp.
- **Moje prilagođene aplikacije LOB funkcioniraju RemoteApp Azure?** Najbolji način slika koji izgleda ga testirate. Pogledajte [RD centar za kompatibilnost](http://www.rdcompatibility.com/compatibility/default.aspx).
- **Koju metodu implementacije (oblaka ili hibridnog) je najbolje za moje tvrtke ili ustanove?** Hibridno zbirke sadrže potpunije doživljaj ako želite Potpuna Integracija s jedinstvenu prijavu (SSO) i sigurne lokalne mreže povezivanje. Oblak zbirke pružaju agilno i jednostavan način izdvojiti implementaciju sustava pomoću više metoda provjere autentičnosti. Dodatne informacije o [Mogućnosti implementacije](remoteapp-whatis.md).
- **Imamo SQL ili drugu bazu podataka bilo lokalno ili na Azure. Koju vrstu implementacije koristiti?** To ovisi o gdje se nalazi SQL ili pozadinske baze podataka. Ako je baza podataka u privatne mreže, koristite hibridnog zbirke. Ako baza podataka je izložen putem Interneta, a omogućuje klijent veze s njim povezati, možete koristiti zbirke oblaka.
- **Je li moguće koristiti pogon mapiranje, USB i serijskog priključka, zajedničko korištenje međuspremnika i pisača preusmjeravanje?** Sve te značajke podržane u Azure RemoteApp. Zajedničko korištenje i pisača preusmjeravanje međuspremnika omogućena je prema zadanim postavkama. Dodatne informacije o preusmjeravanju [ovdje](remoteapp-redirection.md). 


## <a name="template-images"></a>Slika predložaka
- **Mogu li koristiti oblaka ili postojeći virtualnog računala kao predložak za moj zbirku RemoteApp?** Da! Možete stvaranje slike na temelju programa Azure VM, primijenite jednu od slike u sklopu pretplate ili stvoriti prilagođenu sliku. Pogledajte [Mogućnosti slike RemoteApp](remoteapp-imageoptions.md).


## <a name="network-options"></a>Mogućnosti za mrežu
- **Zbirka hibridnog potreban je VNET. Ne možemo koristiti naše postojeće VNET?** Možete li postojeće VNET programa Azure VNET. U odjeljku "korak 1: postavljanje virtualne mreže" na [hibridnog zbirke upute](remoteapp-create-hybrid-deployment.md) za dodatne informacije.
- **Možete koristiti na VNET s zbirku oblak?** Uistinu možete. Potražite u članku [Stvaranje zbirku oblaka](remoteapp-create-cloud-deployment.md), osobito korak 1, dodatne informacije.

## <a name="authentication-options"></a>Mogućnosti provjere autentičnosti



- **Mislite o autentičnosti? Koja je metoda podržava?** Oblak zbirke podržava Microsoft računi i računi servisa Azure Active Directory, koje su kao i računa za Office 365. Zbirka hibridnog podržava samo Azure Active Directory račune koji su sinkronizirani (pomoću alata kao što je [Azure Active Directory Sync](http://blogs.technet.com/b/ad/archive/2014/09/16/azure-active-directory-sync-is-now-ga.aspx)) iz Windows Server Active Directory implementacije; Konkretno, ili sinkronizirali s mogućnošću sinkronizaciju lozinke ili sinkronizirali s vanjskim pristupom Active Directory Federation Services (AD FS) konfiguriran. Možete konfigurirati i [Višestruke provjere autentičnosti (MFA)](https://azure.microsoft.com/services/multi-factor-authentication/).

>[AZURE.NOTE]Azure Active Directory korisnici moraju se nalaziti iz klijenta koji je povezan s pretplatom. (Možete pogledati i izmjena pretplate na kartici **Postavke** na portalu. Potražite u članku [Promjena klijentu Azure Active Directory koriste RemoteApp](remoteapp-changetenant.md) dodatne informacije)

- **Zašto se moj pristupa računu Azure Active Directory ne može dati?** Azure Active Directory korisnici moraju se nalaziti iz imenika koji je povezan s pretplatom. Možete pogledati ili izmijeniti taj imenik na kartici postavke na portalu. Dodatne informacije potražite [koristi promjena klijentu Azure Active Directory RemoteApp](remoteapp-changetenant.md) .

## <a name="clients---what-device-can-i-use-to-access-azure-remoteapp"></a>Klijenti - koje uređaja možete koristiti da biste pristupili Azure RemoteApp?
Možete pronaći podatke dobar klijent, uključujući koracima instalacije različitih klijente na [pristup aplikacijama RemoteApp Azure](remoteapp-clients.md).

- **Koje uređaje i operacijski sustavi klijentske aplikacije podržavaju?**
Prvo, računala i tablete: 
    - Windows 10 (klijent pretpregled)
    - Windows 8.1 i Windows 8
    - Windows 7 Service Pack 1
    - Mac OS X
    - Windows RT
    - Tablete sa sustavom android
    - uređaje Ipad i na telefonima:
    - iPhone
    - Telefon sa sustavom android
    - Windows Phone
 
    [Preuzmite](https://www.remoteapp.windowsazure.com/ClientDownload/AllClients.aspx) klijent RemoteApp.
- **Podržava li Azure RemoteApp Tanki klijenti?** Da, podržane su sljedeće Windows ugrađene Tanki klijenti:
    - Windows ugrađene standardne 7
    - Windows ugrađene 8 Standard
    - Windows ugrađene 8.1 industrijskih Pro
    - Windows 10 IoT Enterprise

- **Koju verziju sustava Windows Server je podržano za na udaljene radne površine sesiju glavnog računala (RDSH)?** Windows Server 2012 R2.

## <a name="support-and-feedback"></a>Podrška i povratnih informacija


- **Što je planiranje podrške za RemoteApp?** Podrška za upravljanje naplatom i pretplatom navedeni su na nema trošak. Tehnička podrška je dostupan putem [servisa Azure tarife](https://azure.microsoft.com/support/plans/). Podrška za besplatne zajednice možete dobiti i putem našem [forumu Azure rasprave](http://social.msdn.microsoft.com/Forums/windowsazure/home?forum=AzureRemoteApp). 
- **Kako poslati povratne informacije?** Posjetite [forum za povratne informacije](https://feedback.azure.com/forums/247748-azure-remoteapp/).
- **Tko možete I razgovarati da biste saznali više o Azure RemoteApp?** Osim našem [forumu rasprave](http://social.msdn.microsoft.com/Forums/windowsazure/home?forum=AzureRemoteApp), koja je primjereno postavljati pitanja, možete se uključiti u tjedni [Ask webinar stručnjaka](https://azureinfo.microsoft.com/US-Azure-WBNR-FY15-11Nov-AzureRemoteAppAskTheExperts-Registration-Page.html), gdje ćemo objasniti što sve njom RemoteApp.
- **Je li moguće koristiti dokumentaciju RemoteApp?** Ispričavamo se pa Drago od vas zatraži. Uz sadržaj pomoći u ladica portala pomoći (samo kliknite **?** na bilo kojoj stranici na portalu), u sljedećim člancima na raspolaganju usvojili sve o RemoteApp:
    - **Početak rada:**
        - [Što je RemoteApp?](remoteapp-whatis.md)
        - [Što je slika predložaka RemoteApp?](remoteapp-images.md)
        - [Kako licenciranje funkcionira?](remoteapp-licensing.md)
        - [Kako RemoteApp i Office funkcioniraju zajedno?](remoteapp-o365.md)
        - [Preusmjeravanje rad s RemoteApp](remoteapp-redirection.md)?
    - **Implementacija:**
        - [Stvaranje prilagođenog predloška slike](remoteapp-create-custom-image.md)
        - [Stvaranje zbirke hibridnog](remoteapp-create-hybrid-deployment.md)
        - [Stvaranje zbirke oblaka](remoteapp-create-cloud-deployment.md)
        - [Konfiguriranje Azure Active Directory za RemoteApp](remoteapp-ad.md)
        - [Objavljivanje aplikacije RemoteApp](remoteapp-publish.md)
    - **Upravljanje:**
        - [Dodavanje korisnika](remoteapp-user.md)
        - [Najbolje prakse za konfiguriranje i korištenje RemoteApp](remoteapp-bestpractices.md)  

    Videozapisi! Imamo i broj videozapisi o RemoteApp. Neke pružaju Uvod ([Uvod u Azure RemoteApp](https://azure.microsoft.com/documentation/videos/cloud-cover-ep-150-azure-remote-app-with-thomas-willingham-and-nihar-namjoshi/)) dok drugi vas voditi kroz implementacije ([Implementacija oblaka](https://www.youtube.com/watch?v=3NAv2iwZtGc&feature=youtu.be) i [Hibridne implementacije](https://www.youtube.com/watch?v=GCIMxPUvg0c&feature=youtu.be)). Ih pogledajte!

 
### <a name="help-us-help-you"></a>Pomozite nam da vam pomoći 
Jeste li znali da osim ocjena u ovom se članku i upućivanje komentare dolje ispod, koje možete unijeti promjene sam članak? Nešto nedostaje? Nešto nije u redu? Nije li moguće napisati nešto što je samo pregledniji? Pomicanje prema gore, a zatim kliknite **Uređivanje na GitHub** da biste unijeli promjene – one će dođite do nas za pregled, a zatim jednom smo odjaviti na njima, vidjet ćete promjene i poboljšanja ovdje.
