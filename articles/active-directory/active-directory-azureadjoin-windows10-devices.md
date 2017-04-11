<properties
    pageTitle="Pomoću uređaja za Windows 10 na svom radnom mjestu | Microsoft Azure"
    description="Omogućuje snimke mogućnosti za korisnike i IT, kontrastom različite načine na uređaju se mogu dodjeli i koristiti u sustavu Windows 10."
    services="active-directory"
    documentationCenter=""
    authors="femila"
    manager="swadhwa"
    editor=""
    tags="azure-classic-portal"/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/17/2016"
    ms.author="femila"/>

# <a name="using-windows-10-devices-in-your-workplace"></a>Pomoću uređaja za Windows 10 na svom radnom mjestu

Odnosi se na: računala za Windows 10

Windows 10 nudi tri modelima tvrtkama ili ustanovama koje omogućuju korisnicima pristup resursima Rad na siguran i jednostavan način.

- **Uključivanje Azure Active Directory** (Azure AD spoj), za koji pristup resursima kao što je Office 365 prvenstveno u oblaku. Azure AD spoj je samostalno rad dodjeljivanje sučelje, koje je novo u sustavu Windows 10.
- **Uključivanje domene**tvrtkama ili ustanovama koje su ulaganja u lokalni aplikacija i resursa. Uključivanje domene nudi Poboljšano sučelje u sustavu Windows 10 kada ste povezani s Azure AD.
- **Novo sučelje pojednostavljeni BYOD**, za korisnike koji želite dodati računa tvrtke ili obrazovne ustanove u Windows i lako pristupanje resursima na osobnim uređaja.

Sljedeća tablica predstavlja snimke mogućnosti za korisnike i INFORMATIČKI administratori kontrastom različite načine na uređaju se mogu dodjeli i koristiti u sustavu Windows 10:

|                                                                                                                                                                 | Uključivanje domene     | Azure AD spoj | Osobnim uređaja |
|-----------------------------------------------------------------------------------------------------------------------------------------------------------------|-----------------|---------------|-----------------|
| Windows uređaj prijavu računa tvrtke ili obrazovne ustanove.                                                                                                                      | Da             | Da           | ne              |
| Korisnik jedinstvene prijave (SSO) za aplikacije za Office 365 i Azure AD. SSO je mogućnost da biste se prijavili samo jednom pristup tvrtke ili ustanove. | Da             | Da           | Da             |
| Korisnik SSO Kerberos/NTLM aplikacije.                                                                                                                                  | Da             | Ograničeno       | Da, putem VPN-a         |
| Istaknuti autorizacije i praktične prijava za tvrtke ili obrazovne ustanove račune za Microsoft Passport i Windows pozdrav.                                                                   | Da             | Da           | Da             |
| Pristup enterprise iz Windows trgovine pomoću računa tvrtke ili obrazovne ustanove (ne Microsoftov račun).                                                                                    | Da             | Da           | Da             |
| Enterprise usklađen roaming od korisničkih postavki svim uređajima pomoću računa tvrtke ili obrazovne ustanove.                                                                                 | Da             | Da           | Da             |
| Mogućnost da biste ograničili pristup tvrtke ili ustanove aplikacija na uređajima koji su u skladu s pravilima tvrtke ili ustanove uređaja.                                                      | Da             | Da           | Da             |
| Korisnik samoposlužnog dodjeljivanje uređaja za "posao s bilo kojeg mjesta".                                                                                                | ne              | Da           | Da             |
| Mogućnost upravljanje uređajima.                                                                                                                                       | Da, putem GP/SCCM | Da           | Da             |

## <a name="use-work-owned-devices-with-azure-ad-join-and-domain-join-in-windows-10"></a>Uključivanje korištenje vlasništvu rad uređaja s Azure AD spoj i domenu u sustavu Windows 10

Windows 10 nudi dva načina za rad u vlasništvu uređaje da biste pristupili resursima za rad:

- Azure AD spoj
- Uključivanje domene

 Oba može biti valjane mogućnosti ovisno o potrebama i preduvjeti za tvrtke ili ustanove. U nekim slučajevima tvrtke ili ustanove mogu koristiti Omogućivanje obje metode implementacije.

## <a name="when-to-use-azure-active-directory-join"></a>Kada koristiti Azure Active Directory uključivanje

Azure AD spoj je novi samostalno posao dodjeljivanje sučelja u sustavu Windows 10.  Ciljani je na koji se pristup resursima rada kao što je Office 365 prvenstveno u oblaku. To je laganih način da biste konfigurirali računala, tablete i telefona za tvrtke. Uređaji upravlja se putem upravljanje mobilnim uređajima pomoću dosljedan kontrola preko platforme Windows.

**Korištenje Azure AD uključiti iz nekog od sljedećih razloga**:

- Želite omogućiti za Samostalno dodjeljivanje uređaja za zaposlenici zaduženi za u pokretu.
- Korisnicima omogućili rad u vlasništvu mobilnim uređajima kao što su tablete i mobitele.
- Želite upravljati određeni skup korisnika u Azure AD umjesto u servisu Active Directory, kao što su zaposlenici zaduženi za sezonski, radnici ili učenicima.
- Želite li omogućiti spojno mogućnosti zaposlenicima koji rade na udaljenom podružnicama s s ograničeni lokalnog infrastrukture.
- Nemate lokalni Active Directory.

Neke tvrtke i ustanove će koristiti Azure AD uključivanje kao glavni način implementacije uređaji za rad u vlasništvu osobito kao što su migrirati većinu ili sve svoje resursa u oblak. Hibridno tvrtke i ustanove čije Active Directory i Azure AD možete odabrati za implementaciju način ili neki drugi ovisno o korisnika ili odjel.

Su područja škole i universities, na primjer, može upravljati osoblje u servisu Active Directory i učenicima Azure AD. Neke tvrtke možda želite upravljati udaljene ureda ili prodaje odjela za Azure AD. Azure AD spoj i metode spoj domene ne može se koristiti u tvrtki ili ustanovi hibridnog. Azure AD spoj može biti odličan komplementaran alatu za pridruživanje domene za implementaciju uređaja u radnom okruženju.

**Ako je najčešće uobičajeni pristup poslovni resursi putem oblaka, vašoj tvrtki ili ustanovi mogu uživati u dodatnim prednostima ako**:

- Možete ukloniti međuzavisnosti lokalnih identiteta infrastrukture.
- Može pojednostavniti model za implementaciju sustava uređaja početak izvan slike rješenja dopuštanjem samostalno konfiguracije.
- Koristite upravljanje mobilnim uređajima za upravljanje svim svojim uređajima na različitim platformama.

Dodatne informacije o Azure AD uključivanje potražite u članku [Mogućnosti oblaka Extending na uređajima Windows 10 do Azure Active Directory uključiti](active-directory-azureadjoin-overview.md).

## <a name="when-to-use-domain-join-or-keep-using-it"></a>Kada koristiti domenu spoj (ili Zadrži korištenja)

Za zadnjih 15 godina mnoge organizacije imaju spoj domene koristi za povezivanje rad uređaja. Omogućuje korisnicima prijavite se u svoje uređaje s rade servisa Active Directory ili školske račune. Omogućuje uključivanje domene i IT potpuno i središnje upravljanje sljedećim uređajima. Tvrtke i ustanove obično ovise o slike metode Dodjela resursa za uređaje i često koriste web-mjesto sustava centar za konfiguraciju Manager (SCCM) ili u okvir za pravila grupe (GP) da biste upravljali njima.

**Tvrtke trebali biste koristiti domenu spoj (ili Zadrži upotrebljavaju) iz nekog od sljedećih razloga**:

- Imate Win32 aplikacija implementiran na ove uređaje koji koriste NTLM/Kerberos.
- Potrebna vam je GP ili SCCM/DCM za upravljanje uređajima.
- Želite nastaviti koristiti slike rješenja da biste konfigurirali uređaji za svoje zaposlenike.

**Uključivanje domene u sustavu Windows 10 i pruža sljedeće prednosti kada ste povezani s Azure AD**:

- Istaknuti uređaja povezanih s provjeru autentičnosti i praktične prijava za tvrtke ili obrazovne ustanove račune za Microsoft Passport i Windows pozdrav.
- Pristup enterprise iz Windows trgovine za uređaje koji koriste rad ili školske račune (bez Microsoft potreban je račun).
- Enterprise usklađen roaming od korisničkih postavki preko uređaje koji se pomoću računa tvrtke ili obrazovne ustanove (bez Microsoft potreban je račun).
- Mogućnost da biste ograničili pristup tvrtke ili ustanove aplikacija na uređajima koji su u skladu s pravilima tvrtke ili ustanove uređaja.

Dodatne informacije o Azure AD uključivanje potražite u članku [Mogućnosti oblaka Extending na uređajima Windows 10 do Azure Active Directory uključiti](active-directory-azureadjoin-overview.md).

## <a name="enable-joining-of-personally-owned-devices-for-work-or-school"></a>Omogućivanje uključivanja osobno vlasništvo uređaja za tvrtke ili obrazovne ustanove

Da biste podržali BYOD tvrtke, Windows 10 omogućuje korisnika da biste dodali račun tvrtke ili obrazovne ustanove na svom računalu, tableta ili telefona. Nakon korisnika dodaje račun tvrtke ili obrazovne ustanove, uređaj registriran Azure AD i po želji upisani u sustavu za upravljanje mobilnim uređajima koji je konfiguriran tvrtke ili ustanove. Imenika prikazat će ti uređaji kao 'Registrirani' nasuprot "Azure AD pridruženo". IT administraors možete primijeniti različita pravila na temelju tih informacija nudi svjetlija dodirom na osobno vlasništvo uređajima od uređaji za rad u vlasništvu po želji.

Korisnike možete dodati tvrtke ili obrazovne ustanove za svoje osobnim uređaja vrlo praktično. To će moći učiniti prilikom pristupanja u aplikaciji prvi put ili ih možete stvoriti ručno putem izbornika postavke. Taj račun dat će SSO resursima tvrtke ili ustanove.

Dodatne informacije o Azure AD uključivanje potražite u članku [Povezivanje domene pridruženo uređajima Azure AD za Windows 10 iskustvo](active-directory-azureadjoin-devices-group-policy.md).

## <a name="enable-domain-join-or-azure-ad-join"></a>Da biste omogućili spoj domene ili Azure AD spoj

Sve načine opisanih (spoj domene, Azure AD spoj i dodavanje ili školske računa) imaju točke unosa u odjeljku korisnički doživljaj sustava Windows 10. Međutim, sve potreban je IT administrator da biste omogućili funkcionalnost Infrastruktura prije nego što funkcioniraju iskustvo.

## <a name="requirements-for-deploying-azure-ad-join"></a>Preduvjeti za implementaciju uključivanje za Azure AD

Za implementaciju uključivanje za Azure AD za sve skup korisnika potrebno je sljedeće:

- Pretplatu na Azure AD.
- Azure AD pretplatu Premium, kao što je mobilni upravljanje automatski-Registracija uređaja, ako su vam potrebne dodatne mogućnosti.
- Upravljanje mobilnim uređajima – na primjer, pretplatu na Microsoft Intune, upravljanje mobilnim uređajima za Office 365 ili neki proizvođači za upravljanje mobilnim uređajima partnera integriranih s Azure AD. (Potražite u [odjeljku najčešća Pitanja](#frequently-asked-questions) pri kraju ovog članka dodatne informacije).

Ako vaš funkcije hibridnog, ne možemo se preporučuje implementacije Azure AD Connect da biste proširili lokalnog imenika za Azure AD.

## <a name="requirements-for-using-domain-join-with-azure-ad"></a>Preduvjeti za korištenje spoj domene za Azure AD

Uključivanje domenu i dalje funkcionira kao uvijek ima. Međutim, da biste dobili pogodnosti Azure AD ćete sljedeće:

- Pretplatu na Azure AD.
- Implementacija Azure AD Connect da biste proširili lokalnog imenika za Azure AD.
- Pravila koja omogućuje domene pridruženo uređaji uvjetno pristup Azure AD.
- Pravila koja omogućuje pristup "domene-pridruženo" uređaja ako želite imati mogućnost ograničavanja pristupa za neke uređaje.
- Sustav centar Upravitelj konfiguracije verzija 1509 za Tehnički pretpregled da biste omogućili pravila za koje je obavezna uređaji. (Pogledajte članak dokumentaciju i objavu na blogu).

Dodatne informacije o spoj domene u sustavu Windows 10, potražite u članku [Povezivanje domene pridruženo uređajima Azure AD za Windows 10 sučelja](active-directory-azureadjoin-devices-group-policy.md)

## <a name="requirements-for-using-byod-and-add-a-work-or-school-account"></a>Preduvjeti za korištenje BYOD i "Dodavanje računa tvrtke ili obrazovne ustanove"

Da biste omogućili "Premjesti vlastite uređaj" (BYOD) pomoću računa tvrtke ili obrazovne ustanove, potrebno je sljedeće:

- Pretplatu na Azure AD.
- Azure AD pretplatu Premium, kao što je mobilni upravljanje automatski-Registracija uređaja, ako su vam potrebne dodatne mogućnosti.

## <a name="requirements-for-using-microsoft-passport"></a>Preduvjeti za korištenje Microsoft Passport

Da biste omogućili Microsoft Passport će vam sljedeće:

- Na Infrastruktura javnog ključa (PKI) za podršku za provjeru autentičnosti utemeljenu na certifikat koji koristi Microsoft Passport.
- Pretplata Intune za podršku za provjeru autentičnosti utemeljenu na certifikat koji koristi Microsoft Passport za Azure AD uključiti i računi tvrtke ili obrazovne ustanove.
- Upravitelj konfiguracije centar sustava verzije 1509 za Tehnički pretpregled (pogledajte članak dokumentaciju i objavu na blogu) za podršku za provjeru autentičnosti utemeljenu na certifikat koji koristi Microsoft Passport za domenu spoj.
- Pravila za omogućivanje Microsoft Passport u tvrtki ili ustanovi.

Kao zamjena za korištenje na PKI, ključ sustavom Microsoft Passport možete omogućiti tako da učinite sljedeće:

- Implementacija sustava Windows Server 2016 "Radni pretpregled 1" skupa (nema potrebe za domene ili skup stabala funkcionalne razine; nekoliko skupa za posluživanje zalihosti će suffice svakog web-mjesta servisa Active Directory).
- Postavljanje pravila da biste omogućili Microsoft Passport u tvrtki ili ustanovi.

Dodatne informacije o Microsoft Passport i Windows pozdrav u sustavu Windows 10, potražite u članku < link-to-MS-Passport-and-Windows-Hello-document >.

## <a name="frequently-asked-questions"></a>Najčešća pitanja
### <a name="which-partner-mobile-device-management-products-integrate-with-azure-ad"></a>Proizvode upravljanja mobilnim uređajima partnera integrirati s Azure AD?

Sljedeće proizvode dobavljača integrirati s Azure AD za Sjedinjeno komuniciranje registraciju i uvjetno pristup u sustavu Windows 10:

- AirWatch po VMware
- Citrix Xenmobile
- Upravitelj mobilne Lightspeed
- SOTI lokalni upravljanje mobilnim uređajima
- MobileIron

### <a name="what-about-workplace-join-in-windows-10"></a>Što je s radnom mjestu uključivanje u sustavu Windows 10?
Uključivanje radnog prostora da biste omogućili BYOD korišten u sustavu Windows 8.1. U sustavu Windows 10, BYOD je omogućeno putem "Dodajte tvrtke ili obrazovne ustanove računa", kao što je opisano ranije u ovom dokumentu. Za tvrtke i ustanove integriranih ne njihove upravljanje mobilnim uređajima s Azure AD korisnicima možete Registracija uređaja u upravljanju ručno putem **Postavke** > **računi** > **rada programa access**.


### <a name="can-users-connect-their-microsoft-account-to-their-domain-account-in-windows-10"></a>Korisnici povežite putem Microsoftova računa računa domene u sustavu Windows 10?
Ne u sustavu Windows 10. U sustavu Windows 8.1 korisnici domene pridruženo uređaja nije "povezati" putem Microsoftova računa (na primjer, Hotmail, Live, Outlook, Xbox, itd.) za svoj račun domene da biste omogućili određene sučelja kao što je SSO uživo usluge, korištenje iz Windows trgovine i roaming od korisničkih postavki svim uređajima. U sustavu Windows 10 s Microsoftovim računom "povezivanje" funkcionalnost sadrži je iz upotrebe. Korisnik može dodati jedan ili više Microsoftova računa kao dodatne račune da biste omogućili SSO za korisničke servise kao što je Windows trgovine. To možete učiniti u **postavkama** > **računi** > **vaš račun**.

Korisnici koji su Nadogradnja s uređaja domene pridruženo Windows 8.1 i tko vodili putem Microsoftova računa povezani, će automatski imati povezanog Microsoftova računa dodane na popis dodatne račune koji se koriste.


## <a name="additional-information"></a>Dodatne informacije
* [Windows 10 za tvrtke: načina korištenja uređaji za posao](active-directory-azureadjoin-windows10-devices-overview.md)
* [Proširivanje mogućnosti oblaka na uređajima Windows 10 do Azure Active Directory uključivanje](active-directory-azureadjoin-user-upgrade.md)
* [Saznajte više o korištenju scenariji za Azure AD uključivanje](active-directory-azureadjoin-deployment-aadjoindirect.md)
* [Povezivanje domene pridruženo uređajima s Azure AD za Windows 10 sučelja](active-directory-azureadjoin-devices-group-policy.md)
* [Postavljanje uključivanje za Azure AD](active-directory-azureadjoin-setup.md)
