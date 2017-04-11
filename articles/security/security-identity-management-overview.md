<properties
   pageTitle="Pregled sigurnosti upravljanje Azure identiteta | Microsoft Azure"
   description=" Microsoft identiteta i pristup rješenja pomoć za upravljanje IT zaštiti pristup aplikacijama i resursima preko tvrtke podatkovnog centra i u oblaku, omogućivanje dodatne razine provjere valjanosti kao što su višestruka provjera autentičnosti i pravila uvjetnog programa access. Ovaj članak sadrži pregled temeljni Azure sigurnosne značajke koje se pomoć za upravljanje identitetom. "
   services="security"
   documentationCenter="na"
   authors="TerryLanfear"
   manager="MBaldwin"
   editor="TomSh"/>

<tags
   ms.service="security"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/09/2016"
   ms.author="terrylan"/>

# <a name="azure-identity-management-security-overview"></a>Pregled sigurnosti upravljanje Azure identiteta

Microsoft identiteta i pristup rješenja pomoć za upravljanje IT zaštiti pristup aplikacijama i resursima preko tvrtke podatkovnog centra i u oblaku, omogućivanje dodatne razine provjere valjanosti kao što su višestruka provjera autentičnosti i pravila uvjetnog programa access. Nadzor sumnjivoj aktivnosti kroz dodatnom sigurnošću izvješćivanja, nadzor i upozorenjem omogućuje prevladavanje potencijalne sigurnosnim problemima. [Azure Active Directory Premium](../active-directory/active-directory-editions.md) sadrži jedinstvenu prijavu na tisuće oblaka aplikacije (SaaS) i pristup web-aplikacije pokrenete lokalnog.

Sigurnost pogodnosti programa Azure Active Directory (AD) obuhvaćaju mogućnost:

- Stvaranje i upravljanje jedan identiteta za svakog korisnika preko tvrtki hibridnog održavanje korisnike, grupe i uređaje u sinkronizaciji
- Jedan prijave pristup svojim aplikacijama uključujući tisuće unaprijed integrirane aplikacije SaaS
- Omogućivanje Sigurnost aplikacije programa access primjenom pravila temeljiti višestruke provjere autentičnosti za oba lokalni i aplikacije u oblaku
- Dodjela resursa za sigurnu daljinski pristup lokalnog web-aplikacijama putem Proxy aplikacije za Azure AD

Je nude pregled temeljni Azure sigurnosne značajke koje se pomoć za upravljanje identitetom cilj ovog članka. Nudimo vam i veze na članke koji steći pojedinosti svake značajke da biste saznali više.  

U članku usredotočuje se na sljedeće mogućnosti upravljanja Azure identiteta core:

- Jedinstvenu prijavu
- Obrnuti proxy poslužitelja
- Višestruka provjera autentičnosti
- Nadzor sigurnosti, upozorenja i izvješća koja se temelje na učenje-računala
- Upravljanje potrošača identiteta i pristup
- Registracija uređaja
- Upravljanje identitetom povlaštene
- Zaštita identiteta
- Upravljanje identitetom hibridnog

## <a name="single-sign-on"></a>Jedinstvenu prijavu

Jedan prijavu (SSO) znači moći pristupati svim aplikacijama i resursima koje su vam potrebne da biste učinili tvrtke, tako da se prijavite u samo jednom pomoću jednog korisničkog računa. Kada se prijavite, možete pristupiti svim aplikacijama nužnim bez se potreban za provjeru autentičnosti (primjerice se upišite lozinku) drugi put.

Mnoge organizacije ovise o softver kao aplikacija servisa (SaaS) kao što je Office 365, okvira i Salesforce za produktivnost krajnjeg korisnika. Prethodno, IT osoblju potrebne za pojedinačno stvaranje i ažuriranje korisničkim računima u svakoj aplikaciji SaaS i korisnike imala Zapamti lozinku za svaku aplikaciju SaaS.

Azure AD proširuje lokalnog imeničkog servisa Active Directory u oblak, omogućavanje korisnicima omogućili korištenje njihove primarni račun tvrtke ili ustanove ne samo prijavite se u svoje domene pridruženo uređaje i resurse za tvrtke, ali i svi web-a SaaS aplikacije koja su potrebna za njegov posao.

Ne samo korisnici nemaju da biste upravljali više skupova korisnička imena i lozinke, aplikacije programa access mogu biti automatski dodijeljenu ili deaktivirali dodijeljenu grupe na temelju tvrtke ili ustanove i njihovo stanje kao zaposlenik. Azure AD predstavlja sigurnost i pristup kontrolama za upravljanja koje omogućuju središnje upravljanje pristupom korisnika preko SaaS aplikacije.

uči više:

- [Pregled jedinstvene prijave](https://azure.microsoft.com/documentation/videos/overview-of-single-sign-on/)
- [Što je aplikacija programa access i jedinstvenu prijavu pomoću servisa Azure Active Directory?](../active-directory/active-directory-appssoaccess-whatis.md)
- [Azure Active Directory jedinstvenu prijavu integrirane aplikacije SaaS](../active-directory/active-directory-sso-integrate-saas-apps.md)

## <a name="reverse-proxy"></a>Obrnuti proxy poslužitelja

Proxy aplikacije za Azure AD možete objaviti na lokalni, kao što je [SharePoint](https://support.office.com/article/What-is-SharePoint-97b915e6-651b-43b2-827d-fb25777f446f?ui=en-US&rs=en-US&ad=US) web-mjesta, [Web-aplikacije Outlook Web App](https://technet.microsoft.com/library/jj657718.aspx)i [IIS](http://www.iis.net/)-temelje aplikacije unutar privatne mreže i omogućuje siguran pristup korisnicima izvan mreže. Proxy poslužitelj aplikacije nudi daljinski pristup i jedinstvenu prijavu (SSO) za mnoge vrste lokalnog web-aplikacije s tisuće SaaS programe s podrškom za Azure AD. Zaposlenici prijavite se u aplikacijama iz polaznu na vlastita uređaja i provjeru autentičnosti putem ovaj oblaku proxyja.

uči više:

- [Omogućivanje proxy poslužitelj za Azure AD aplikacije](../active-directory/active-directory-application-proxy-enable.md)
- [Objavljivanje aplikacije koje koriste Proxy aplikacije za Azure AD](../active-directory/active-directory-application-proxy-publish.md)
- [Jedinstvene prijave pomoću Proxy aplikacije](../active-directory/active-directory-application-proxy-sso-using-kcd.md)
- [Rad s programom access uvjetno](../active-directory/active-directory-application-proxy-conditional-access.md)

## <a name="multi-factor-authentication"></a>Višestruka provjera autentičnosti
Azure višestruke provjere autentičnosti (MFA) je metoda provjere autentičnosti koja zahtijeva korištenje više način potvrde i dodaje ključnih drugu razinu zaštite znak dodaci za korisnika i transakcije. MFA olakšava zaštitu programa access s podacima i aplikacijama tijekom sastanka korisnika zahtjev za jednostavne postupak prijave. On nudi Jaka provjera autentičnosti putem raspon mogućnosti provjere – telefonski poziv, tekstne poruke ili mobilne aplikacije obavijesti ili potvrdu Šifra i 3 strana OAuth tokena.

uči više:

- [Višestruka provjera autentičnosti](https://azure.microsoft.com/documentation/services/multi-factor-authentication/)
- [Što je Azure višestruka provjera autentičnosti?](../multi-factor-authentication/multi-factor-authentication.md)
- [Kako funkcionira Azure višestruka provjera autentičnosti](../multi-factor-authentication/multi-factor-authentication-how-it-works.md)

## <a name="security-monitoring-alerts-and-machine-learning-based-reports"></a>Nadzor sigurnosti, upozorenja i izvješća koja se temelje na učenje-računala

Nadzor sigurnosti i upozorenja i strojno izvješća u utemeljen na učenje u koji se odredili uzorke koje nisu usklađene pristup omogućuju zaštita poslovanja. Azure Active Directory, access i izvješća o korištenju možete koristiti da bi se dobio uvid u integritet i sigurnost imenik tvrtke ili ustanove. Pomoću tih informacija direktorija administrator možete bolje odrediti gdje može biti moguće sigurnosni rizik tako da ih možete planirati odgovarajući način prevladavanje tih rizika.

Azure klasični portalu izvješća kategorizirane na sljedeće načine:

- Značajkom izvješća – sadržavati događaje koje smo pronašli biti anomalous za prijavu. Naš cilj je pretvorite svoju takve aktivnost i omogućuju vam da biste mogli bi određivanja o tome je li događaj sumnjiva.
- Integrirane aplikacije izvješća – pružaju uvida u kako oblaka aplikacije koji se koriste u tvrtki ili ustanovi. Azure Active Directory nudi Integracija s tisuće oblaka aplikacije.
- Izvješća o pogreškama – označio pogreške koji se mogu pojaviti prilikom dodjele resursa računi da biste vanjskim aplikacijama.
- Izvješća značajke specifične za korisnika – prikaz uređaja i prijavite se u aktivnosti podataka za određenog korisnika.
- Zapisnici aktivnosti – sadrži zapis o svim događajima nadziru unutar zadnja 24 sata, zadnjih sedam dana ili zadnjih 30 dana, kao i promjena aktivnost u grupama i vratiti i registracija aktivnosti lozinku.

uči više:

- [Prikaz izvješća pristupa i korištenja](../active-directory/active-directory-view-access-usage-reports.md)
- [Početak rada s Azure Active Directory izvješćivanje o pogreškama](../active-directory/active-directory-reporting-getting-started.md)
- [Vodič za izvješćivanje o pogreškama Azure Active Directory](../active-directory/active-directory-reporting-guide.md)

## <a name="consumer-identity-and-access-management"></a>Upravljanje potrošača identiteta i pristup

Azure Active Directory B2C je vrlo dostupan, globalne identiteta upravljanje servis za aplikacije za korisničke dostupnog mijenja veličinu da biste stotine milijuna identiteta. Može se integrirati preko mobile i web-platformama. Na koje korisnici mogu prijaviti na svim aplikacijama putem sučelja prilagodljivi pomoću postojećih društvenih računa ili tako da stvorite novu vjerodajnice.

U prošlosti, razvojnim inženjerima koji su željeli prijavite se i prijavite se u koje korisnici u svojim aplikacijama bi ste napisali vlastite kod. A želite imati koriste lokalne baze podataka ili sustavi za spremanje korisnička imena i lozinke. Azure Active Directory B2C nudi vašoj tvrtki ili ustanovi bolji način integracije upravljanje identitetom consumer u aplikacijama uz pomoć sigurne, standarde platforme i velikog skupa extensible pravila.

Kada koristite Azure Active Directory B2C, vaše koje korisnici registrirati za aplikacije pomoću svoje postojeće računi društvenih (Facebook, Google, Amazon, LinkedIn) ili tako da stvorite novu vjerodajnice (adresu e-pošte i lozinku, ili korisničko ime i lozinka).

uči više:

- [Što je Azure Active Directory B2C?](https://azure.microsoft.com/services/active-directory-b2c/)
- [Azure Active Directory B2C pretpregled: prijavite se gore i prijavite se u koje korisnici u aplikacija](../active-directory-b2c/active-directory-b2c-overview.md)
- [Pretpregled B2C Azure Active Directory: Vrste aplikacija](../active-directory-b2c/active-directory-b2c-apps.md)

## <a name="device-registration"></a>Registracija uređaja

Registracija uređaja Azure AD je foundation scenarije utemeljen na uređaju [uvjetno programa access](../active-directory/active-directory-conditional-access-on-premises-setup.md) . Kada je registrirana na uređaju, Azure Active Directory Registracija uređaja omogućuje uređaj s identitet koji se koristi za provjeru autentičnosti uređaja kada se korisnik prijavi. Čija je autentičnost provjerena uređaja i atribute uređaja, zatim se poslužite da biste nametnuli pravila uvjetnog pristup za aplikacije koje se nalaze u oblak i lokalne.

U kombinaciji s mobilnog uređaja (MDM) za upravljanje rješenje kao što je Intune atribute uređaj u servisu Azure Active Directory ažuriraju s dodatnim informacijama o uređaju. Omogućuje stvaranje pravila za uvjetno pristup kojih se provode pristup s uređaja da biste zadovoljavaju vaše standarde za sigurnost i usklađenost.

uči više:

- [Početak rada s Azure Active Directory Registracija uređaja](../active-directory/active-directory-conditional-access-device-registration-overview.md)
- [Postavljanje pristupa uvjetno lokalnog pomoću Azure Active Directory Registracija uređaja](../active-directory/active-directory-conditional-access-on-premises-setup.md)
- [Registracija za automatsko uređaja s uređajima domene pridruženo Azure Active Directory za Windows](../active-directory/active-directory-conditional-access-automatic-device-registration.md)

## <a name="privileged-identity-management"></a>Upravljanje identitetom povlaštene
Azure Active Directory (AD) povlaštene upravljanje identitetom možete upravljati, nadzor, te praćenje vaših povlaštene identiteta i pristup resursa u Azure AD, kao i druge Microsoftove internetske servise kao što su Office 365 ili Microsoft Intune.

Katkad korisnici morati izvršavanje povlaštene operacije u resursi Azure ili Office 365 ili drugih aplikacija SaaS. To obično znači tvrtke ili ustanove da biste im dodijelili trajna povlaštene programa access u Azure AD. To je sve veći sigurnosni rizik za oblak hostira resurse jer tvrtkama ili ustanovama potpuno nije moguće nadzirati što ti korisnici rade s njihovim administratorskim ovlastima. Uz to, korisnički račun s pristupom povlaštene ugrožena, taj jedan kršenja mogli bi utjecati njihova cjelokupan oblaka sigurnost. Upravljanje identitetom Azure AD povlaštene pomaže da biste riješili taj rizik.

Upravljanje identitetom Azure AD povlaštene omogućuje:

- U odjeljku korisnike koji su administratori Azure AD
- Omogućivanje na zahtjev, "samo u vremenu" Administrativni pristup Microsoft Online Services kao što su Office 365 i Intune
- Izvješća o administratorski pristup povijesti i promjene se pridružuju dodjele administratora
- Primati upozorenja o pristupu povlaštene ulogu

uči više:

- [Upravljanje identitetom povlaštene Azure AD](../active-directory/active-directory-privileged-identity-management-configure.md)
- [Uloga upravljanje identitetom povlaštene Azure AD](../active-directory/active-directory-privileged-identity-management-roles.md)
- [Azure AD povlaštene upravljanje identitetom: Kako dodati ili ukloniti korisnička uloga](../active-directory/active-directory-privileged-identity-management-how-to-add-role-to-user.md)

## <a name="identity-protection"></a>Zaštita identiteta
Azure AD identiteta zaštita je sigurnost servis koji omogućuje konsolidirani prikaz u događaje rizik i potencijalne slabe točke utjecaja identiteta vaše tvrtke ili ustanove. Zaštita identiteta upravlja postojeće Azure Active Directory, značajkom prepoznavanja mogućnosti (dostupno samo putem Azure AD Anomalous aktivnosti izvješća), a predstavlja nove vrste rizika događaja koji se može prepoznati anomalies u stvarnom vremenu.

uči više:

- [Zaštita identiteta Azure Active Directory](../active-directory/active-directory-identityprotection.md)
- [Kanal 9: Azure AD i prikaz identiteta: Pretpregled zaštitu identiteta](https://channel9.msdn.com/Series/Azure-AD-Identity/Azure-AD-and-Identity-Show-Identity-Protection-Preview)

## <a name="hybrid-identity-management"></a>Upravljanje identitetom hibridnog

Pristup Microsoftovu identiteta proteže lokalnog poslužitelja u oblak i, stvaranje jednog korisničkog identiteta za provjeru autentičnosti i ovlaštenja na sve resurse, bez obzira na to mjesto.

uči više:

- [Hibridno identiteta studiju](http://download.microsoft.com/download/D/B/A/DBA9E313-B833-48EE-998A-240AA799A8AB/Hybrid_Identity_White_Paper.pdf)
- [Azure Active Directory](https://azure.microsoft.com/documentation/services/active-directory/)
- [Blog tima za Active Directory](https://blogs.technet.microsoft.com/ad/)
