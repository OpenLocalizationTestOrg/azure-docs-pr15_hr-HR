<properties
    pageTitle="Proširivanje mogućnosti oblak na uređajima Windows 10 do Azure Active Directory uključivanje | Microsoft Azure"
    description="Sadrži detaljne pregled kako Windows 10 uređaji mogu koristiti Azure AD pridružite se registrirana na servisu Azure Active Directory."
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
    ms.date="09/27/2016"
    ms.author="femila"/>

# <a name="extending-cloud-capabilities-to-windows-10-devices-through-azure-active-directory-join"></a>Proširivanje mogućnosti oblaka na uređajima Windows 10 do Azure Active Directory uključivanje

## <a name="what-is-azure-active-directory-join"></a>Što je Azure Active Directory uključiti?
Azure Active Directory uključivanje (Azure AD pridružiti) je funkcionalnost koja registrira uređaj tvrtke vlasnik Azure Active Directory da biste omogućili središnje upravljanje uređaja. To omogućuje korisnicima kao što su zaposlenici i učenicima da biste se povezali s oblakom enterprise kroz Azure Active Directory. To omogućuje pojednostavljeni implementacijama sustava Windows i pristup tvrtke ili ustanove aplikacija i resursa s bilo kojeg uređaja Windows, oba se korporativni posjeduje i osobno vlasništvo (BYOD).

Azure AD spoj namijenjen velike tvrtke koje su prvog/oblaka – samo oblak – obično male - i srednjim tvrtkama koje imaju infrastruktura za Windows Server Active Directory programa lokalnog. Rečeno, Azure AD spoj možete te će se mogu koristiti u velikim tvrtkama i ustanovama na uređajima koji su prestari način pridruživanja tradicionalni domene (mobilnih uređaja, na primjer) ili za korisnike koji uglavnom moraju imati pristup sustavu Office 365 ili drugih aplikacija za Azure AD SaaS.

Iako spoja tradicionalni domenu i dalje nudi na najbolji lokalnog doći na uređajima koji se mogu uključiti u domenu, Azure AD spoj je prikladan za uređaje koji se ne može spoj domene. Azure AD spoj je prikladna za upravljanje korisnicima u oblaku. To radi pomoću mogućnosti upravljanja mobilnim uređajem umjesto pomoću alata za upravljanje tradicionalni domene kao što su pravila grupe i Upravitelj konfiguracije sustava centra (SCCM).

![Pregled web-mjesto tvrtke uređajima i u okvir za osobnih uređaja s lokalnim servisom Active Directory i Azure AD](./media/active-directory-azureadjoin/active-directory-azureadjoin-overview.png)


## <a name="why-should-enterprises-adopt-azure-ad-join"></a>Zašto treba korporacije prihvaćaju Azure AD uključiti?

* **Tvrtke koje su uglavnom u oblaku**: Ako ste premjestili ili premještate u kojem su smanjivanje vaše lokalne ostavlja manji trag pri i drugih značajki u oblak funkcioniranja modela, pridruživanje za Azure AD nije vam koristiti. Možda ste stvorili račune za Azure AD ručno ili putem sinkronizacije lokalni Active Directory. U svakom slučaju, imate račun u Azure AD pa koristite za prijavu u Windows 10. Vaš će se korisnici mogu pridružiti svojim računalima za Azure AD putem ili niste u uredu okvir doživljaja (OOBE) ili putem izbornika postavke. Nakon uključivanja, korisnici će Uživajte jedan prijavu (SSO) pristup oblaku resursima kao što je Office 365 u preglednicima ili u aplikacijama sustava Office.

* **Obrazovne ustanove**: jedna od scenariji smo primanje obavijesti o često jest da obrazovne ustanove imaju dvije vrste korisnika: Nastavnici i studente. Članovi nastavnika smatraju longer-term članova tvrtke ili ustanove, pa stvaranje lokalnih računa za njih je poželjno. No učenici shorter-term članova tvrtke ili ustanove i tako može upravljati u Azure AD. To znači da se skaliranje direktorija možete pomiču u oblak umjesto pohranjuju lokalno. Također znači učenici mogu prijavite se u sustav Windows pomoću računa za Azure AD i dobiti pristup resursima sustava Office 365, u preglednicima ili u aplikacijama sustava Office.

* **Maloprodajne tvrtke**: drugi scenarij čuli smo da ste o klijenata je njihove želju da biste jednostavnije upravljali zaposlenici zaduženi za sezonski.  Ponovno računa za zaposlenike longer-term, stalnu obično se stvaraju kao lokalni računi na domeni pridružili. No zaposlenici zaduženi za sezonski su shorter-term članovi tvrtke ili ustanove, tako da je poželjno upravljajte gdje korisničkih licenci možete se jednostavnije premještati. Stvaranje ove korisničkih računa u oblaku licenci za Office 365 omogućuje korisnicima da biste dobili prednostima prijave Windows i aplikacijama sustava Office s računom Azure AD. U međuvremenu, održavate veću fleksibilnost njihove licenci nakon što napuste.
* **Druge tvrtke**: čak i ako korisnici održavate na lokalni Active Directory, koje nije moguće i dalje prednosti riziku biti Azure-AD-pridružili. To je zato Azure AD nudi pojednostavljeni spojno sučelje, upravljanje učinkovitog uređajima, registraciju za upravljanje automatsko mobilnog uređaja i jednu mogućnost za prijavu za Azure AD i lokalnog resursa.  

## <a name="what-capabilities-does-azure-ad-join-offer"></a>Uključivanje za Azure AD ne nudi koje mogućnosti?
S Azure AD uključili, primit ćete sljedeće:

* **Samostalno dodjeljivanje korporativni vlasništvu uređaja**: sa sustavom Windows 10, korisnici mogu konfigurirati novost shrink-wrapped uređaj u doživljaj – okvir bez IT involvement.


* **Podrška za Moderna obrasca čimbenika**: Azure AD spoj funkcionira na uređajima koji nemaju tradicionalni domene uključivanje mogućnosti.  


* **Podrška za postojećih računa tvrtke ili ustanove**: korisnici više ne morate stvoriti i održavati web osobni Microsoftov račun da biste dobili najbolje mogućnosti rada tvrtke izdavanja uređaja, kao što su koristila za Windows 8. Mogu koristiti svoje postojeće račune za rad u Azure AD umjesto toga. Za mnoge organizacije to zapravo znači da korisnici mogu postaviti i prijavite se u sustav Windows pomoću istih vjerodajnica koje koriste za pristup sustavu Office 365.


* **Registracija upravljanja automatsko mobilnog uređaja**: uređaje možete se automatski prijavili za upravljanje mobilnim uređajima kada ste povezani s Azure AD. Ovaj postupak funkcionira s Microsoft Intune i partner rješenja upravljanja mobilnim uređajima. Kad je upravljanje uređajima sa servisu, administratori mogu monitor/možete upravljati Azure AD pridruženo duž domene pridruženo uređaja u konzolu za upravljanje SCCM.


* **Jedinstvenu prijavu na resurse za tvrtke**: korisnici uživati jedinstvenu prijavu na radnu površinu sustava Windows aplikacija i resursa u oblaku, kao što su Office 365 i tisuće poslovne aplikacije koje ovise o Azure AD za provjeru autentičnosti putem [Azure AD Connect](active-directory-azureadjoin-deployment-aadjoindirect.md). Tvrtku u vlasništvu uređajima koji su se pridružili Azure AD i Uživajte SSO lokalnim resursima kada je uređaj na mreži tvrtke i s bilo kojeg mjesta kada ove resurse ponudit će se putem [Proxy aplikacije za Azure AD](https://msdn.microsoft.com/library/azure/Dn768219.aspx).


* **OS stanje Roaming**: postavke pristupačnosti, web-mjesta, Wi-Fi lozinke i ostale postavke sinkroniziraju putem uređaja tvrtke vlasništvu bez osobnog Microsoftova računa.


* **Podrška za Enterprise Windows Store**: U iz Windows trgovine podržava aplikacija nabavu i licenciranje s računima za Azure AD. Tvrtke i ustanove mogu količinsko licenciranje aplikacija i učinite ih dostupnima korisnicima u tvrtki ili ustanovi.

## <a name="how-do-different-devices-work-with-azure-ad-join"></a>Kako se drugi uređaji funkcioniraju sa Azure AD uključiti?

| Tvrtke uređaj (koje se pridružite lokalne domene)                                                                                                                                                                                         | Tvrtke uređaj (koja se uključili u oblak)                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                     | Osobnim uređaja                                                                                                         |
|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------|
| Korisnici mogu se prijaviti u Windows s vjerodajnicama rada (kao i danas).                                                                                                                                                                        | Korisnici mogli prijaviti u sustav Windows s vjerodajnicama rad kojim se upravlja iz Azure AD. To vrijedi za uređaje tvrtke u tri slučajeva: 1) tvrtke ili ustanove nema servisa Active Directory lokalno (na primjer, small business). 2) tvrtke ili ustanove ne stvorite sve korisničke račune u servisu Active Directory (na primjer, račune za učenika, savjetnika ili zaposlenici zaduženi za sezonski ne stvaraju se u servisu Active Directory). 3) na tvrtka ili ustanova ima korporativni uređaje koji se ne može biti pridruženo domene (lokalno), kao što su telefone i tablete koji se izvodi Mobile SKU (Ako, na primjer, sekundarne uređaj preusmjereni na tvorničke/maloprodaja floor). Azure AD spoj podržava uključivanja korporativni uređaja za tvrtke ili ustanove s upravljanim i pridruženi. | Korisnici prijavite u sustav Windows pomoću svoje osobne vjerodajnice Microsoftova računa (bez promjene).                                                |
| Korisnici imaju pristup zajedničke postavke i enterprise iz Windows trgovine. Ove usluge rad s računima za rad, a ne zahtijevaju osobnog Microsoftova računa. Potreban je tvrtkama ili ustanovama povezati svoje lokalnog servisa Active Directory Azure AD.                                        | Korisnici mogu učiniti Samostalno postavljanje. Oni mogu proći kroz prvog pokretanja sustava (FRX) putem računa svoje tvrtke kao zamjena za potrebe Dodjela resursa za IT uređaja, iako podržani su obje metode.                                                                                                                                                                                                                                                                                                                                                                             | Korisnici mogu jednostavno dodati poslovnog računa kojim se upravlja web-mjesto servisa Active Directory ili u okvir za Azure AD.                                                      |
| Korisnici koji imaju SSO mogućnost na radnoj površini rada aplikacije, web-mjesta i resurse – uključujući lokalnog resursa i oblaka aplikacije koje koristi Azure AD za provjeru autentičnosti.                                                                                                            | Uređaji su automatski registered u imeniku tvrtke (Azure AD) i automatski prijavili za upravljanje mobilnim uređajima. (Azure AD Premium značajka).                                                                                                                                                                                                                                                                                                                                                                                                                                                  | Korisnici imati mogućnost SSO svim aplikacijama i web-mjesta i resursima s ovim računom rad.                                              |
| Korisnici mogu dodavati osobni računi Microsoft pristup svojim osobnih slika i datoteka bez utjecaja podataka tvrtke. (Roaming postavke nastaviti rad s računa za rad.) Microsoftov račun omogućuje SSO i više ne pogoni roaming postavke.  | Korisnici mogu učiniti s samostalno ponovno postavljanje lozinke (SSPR) na winlogon, što znači da ih možete ponovno postaviti zaboravljene lozinke. (Azure AD Premium značajka).                                                                                                                                                                                                                                                                                                                                                                                                                                   | Korisnici imaju pristup enterprise iz Windows trgovine tako da mogu dobiti i koristiti redak poslovnih aplikacija osobne uređajima. |                                                               |


## <a name="additional-information"></a>Dodatne informacije
* [Windows 10 za tvrtke: načina korištenja uređaji za posao](active-directory-azureadjoin-windows10-devices-overview.md)
* [Proširivanje mogućnosti oblaka na uređajima Windows 10 do Azure Active Directory uključivanje](active-directory-azureadjoin-user-upgrade.md)
* [Provjera autentičnosti identiteta bez lozinke kroz Microsoft Passport](active-directory-azureadjoin-passport.md)
* [Saznajte više o korištenju scenariji za Azure AD uključivanje](active-directory-azureadjoin-deployment-aadjoindirect.md)
* [Povezivanje domene pridruženo uređajima s Azure AD za Windows 10 sučelja](active-directory-azureadjoin-devices-group-policy.md)
* [Postavljanje uključivanje za Azure AD](active-directory-azureadjoin-setup.md)
