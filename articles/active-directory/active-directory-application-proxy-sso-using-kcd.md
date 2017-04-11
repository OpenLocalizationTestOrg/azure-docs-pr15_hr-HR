<properties
    pageTitle="Jedinstvenu prijavu s Proxy aplikacije | Microsoft Azure"
    description="Opisuje kako sadrže jedinstvene prijave pomoću Proxy aplikacije za Azure AD."
    services="active-directory"
    documentationCenter=""
    authors="kgremban"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/10/2016"
    ms.author="kgremban"/>


# <a name="single-sign-on-with-application-proxy"></a>Jedinstvenu prijavu s Proxy aplikacije

Ključni element proxyja aplikacije za Azure AD je jedinstvenu prijavu. Pruža najbolje korisnički doživljaj sa sljedećim koracima:

1. Korisnik se prijavi u oblak  
2. Sve provjere sigurnost dogoditi u oblaku (prethodne)  
3. Ako se zahtjev pošalje lokalno aplikaciju, Proxy poveznik aplikacija impersonates korisnika. Pozadinska aplikacija misli to je redoviti korisnik koji dolaze iz domene pridruženo uređaj.

![Dijagram programa Access iz krajnjeg korisnika putem Proxy aplikacije korporacijskom mrežom](./media/active-directory-application-proxy-sso-using-kcd/app_proxy_sso_diff_id_diagram.png)

Proxy aplikacije za Azure AD pomaže vam omogućavaju sučelje za jedinstvenu prijavu (SSO). Da biste objavili aplikacije pomoću jedinstvene Prijave, slijedite ove upute:


## <a name="sso-for-on-prem-iwa-apps-using-kcd-with-application-proxy"></a>SSO za lokalno IWA aplikacije KCD pomoću Proxy aplikacije
Možete omogućiti jedinstvenu prijavu svojim aplikacijama pomoću integriranu provjeru autentičnosti sustava Windows (IWA) daju poveznika Proxy aplikacije dozvole u servisu Active Directory oponašati korisnika, i slanje i primanje tokeni u ime.


### <a name="network-diagram"></a>Mrežni dijagram

Ovaj dijagram objašnjava tijeka kada se korisnik pokuša pristupiti lokalno aplikacije koja koristi IWA.

![Dijagram toka za Microsoft AAD provjera autentičnosti](./media/active-directory-application-proxy-sso-using-kcd/AuthDiagram.png)

1. Korisnik unosi URL za izravan pristup aplikaciji lokalno putem Proxy aplikacije.
2. Proxy poslužitelj aplikacije preusmjerava zahtjev servise za provjeru autentičnosti Azure AD za preauthenticate. Sada Azure AD primjenjuje svih primjenjivih provjere autentičnosti i pravila za provjeru autentičnosti, kao što su multifactor provjeru autentičnosti. Ako korisniku se provjerava, Azure AD stvara token i šalje korisniku.
3. Korisnik prosljeđuje token Proxy aplikacije.
4. Proxy poslužitelj aplikacije Provjeri valjanost token dohvaća korisnika Glavno ime (UPN-a) od njega i poveznik kroz konfiguriranom čija je autentičnost provjerena sigurnog kanala šalje zahtjev, UPN-a i na servis glavni naziv (SPN).
5. Poveznik izvodi Pregovori Kerberos ograničeno delegiranje (KCD) s na lokalno AD, oponaša korisnika da biste dobili Kerberos token aplikaciji.
6. Active Directory šalje token Kerberos za aplikaciju poveznik.
7. Poveznik šalje izvorni zahtjev za poslužitelj aplikacije pomoću token Kerberos poslao AD.
8. Aplikacija šalje odgovor poveznik koji se zatim vraća Proxy aplikacije servisa i na kraju korisniku.

### <a name="prerequisites"></a>Preduvjeti

Prije nego što počnete s SSO za Proxy aplikacije, provjerite je li vaše okruženje spreman uz sljedeće postavke i konfiguracije:

- Aplikacija, kao što je SharePoint web-aplikacijama postavljene da biste koristili integrirani provjeru autentičnosti sustava Windows. Dodatne informacije potražite u članku [Omogućivanje podrška za provjeru autentičnosti Kerberos](https://technet.microsoft.com/library/dd759186.aspx)ili sustava SharePoint potražite u članku [Planiranje za provjeru autentičnosti Kerberos u sustavu SharePoint 2013](https://technet.microsoft.com/library/ee806870.aspx).

- Sve aplikacije dodijeljeni su nazivi usluge.

- Poslužitelj koji se izvodi poveznik i poslužitelju koji se izvodi aplikaciju su domene pridruženo dio iste domene ili postavljanju domene. Dodatne informacije o spoj domene potražite u članku [Uključivanje računala s domenom](https://technet.microsoft.com/library/dd807102.aspx).

- Poslužitelj koji se izvodi poveznik ima pristup za čitanje na TokenGroupsGlobalAndUniversal za korisnike. Ovo je zadana postavka koje možda imaju omogućeno utječe sigurnost utvrđivanje okruženje. Dodatna pomoć s ovom u [KB2009157](https://support.microsoft.com/en-us/kb/2009157).

### <a name="active-directory-configuration"></a>Konfiguracije servisa Active Directory

Konfiguracija servisa Active Directory ovisi o poveznik za Proxy aplikacije i objavljene poslužitelja jesu li u istoj domeni ili ne.

#### <a name="connector-and-published-server-in-the-same-domain"></a>Poveznik i objavljene poslužitelja u istoj domeni

1. U servisu Active Directory, odaberite **Alati** > **korisnicima i računalima**.
2. Odaberite poslužitelj koji se izvodi poveznik.
3. Desnom tipkom miša, a zatim odaberite **Svojstva** > **delegiranje**.
4. Odaberite **pouzdanost ovo računalo za delegiranje navedeni servisima** , a zatim u odjeljku **servisa kojoj ovaj račun možete prikazati ovlaštenog vjerodajnice**, dodajte vrijednost za identitet SPN poslužitelja aplikacija.
5. Time se omogućuje Proxy poveznik aplikacija oponašati korisnika u AD protiv aplikacije određene na popisu.

![Snimka zaslona prozora poveznik SVR svojstva](./media/active-directory-application-proxy-sso-using-kcd/Properties.jpg)

#### <a name="connector-and-published-server-in-different-domains"></a>Poveznik i objavljene poslužitelju u različitim domenama

1. Popis preduvjeti za rad s KCD svim domenama, potražite u članku [Kerberos ograničeno delegiranje svim domenama](https://technet.microsoft.com/library/hh831477.aspx).
2. U sustavu Windows 2012 R2 koristiti u `principalsallowedtodelegateto` svojstvo na poslužitelju poveznik da biste omogućili Proxy aplikacije delegatu za poslužitelj poveznika, gdje se nalazi poslužitelj za objavljene `sharepointserviceaccount` te delegiranje poslužitelj `connectormachineaccount`.

        $connector= Get-ADComputer -Identity connectormachineaccount -server dc.connectordomain.com

        Set-ADComputer -Identity sharepointserviceaccount -PrincipalsAllowedToDelegateToAccount $connector

        Get-ADComputer sharepointserviceaccount -Properties PrincipalsAllowedToDelegateToAccount


>[AZURE.NOTE] `sharepointserviceaccount`može biti SPS račun ili račun usluge na kojem se izvodi aplikacije skup SPS.


### <a name="azure-classic-portal-configuration"></a>Azure klasični Konfiguracija portala

1. Objavite svoju aplikaciju prema uputama opisane u [aplikacijama za objavljivanje s Proxy aplikacije](active-directory-application-proxy-publish.md). Provjerite jeste li **Azure Active Directory** kao odaberite **Način Preauthentication**.
2. Kada aplikacija pojavi na popisu aplikacija, odaberite ga i kliknite **Konfiguriraj**.
3. U odjeljku **Svojstva**postavite **Interna metoda provjere autentičnosti** na **Integriranu provjeru autentičnosti sustava Windows**.  
  ![Konfiguracija naprednih aplikacije](./media/active-directory-application-proxy-sso-using-kcd/cwap_auth2.png)  
4. Unesite **Interna SPN računala** poslužitelja aplikacija. U ovom primjeru SPN za naše objavljen je http/lob.contoso.com.  

>[AZURE.IMPORTANT] Ako lokalnog UPN-a i UPN-a servisa Azure Active Directory nisu identične, morat ćete konfigurirati [delegirani identitet za prijavu](#delegated-login-identity) na umu da prethodna rad.

|  |  |
| --- | --- |
| Interna provjere autentičnosti | Ako koristite Azure AD za prethodne, možete postaviti način internu provjere autentičnosti da biste omogućili korisnicima da im jedinstvenu prijavu (SSO) za tu aplikaciju. <br><br> Ako aplikacija koristi IWA i možete konfigurirati Kerberos ograničeno delegiranje (KCD) da biste omogućili SSO za ovu aplikaciju, odaberite **Integriranu provjeru autentičnosti sustava Windows** (IWA). Aplikacije koje koriste IWA mora biti konfigurirana pomoću KCD, u suprotnom Proxy aplikacije nećete moći objaviti te aplikacije. <br><br> Ako aplikacija ne koristi IWA odaberite **ništa** . |
| Interne SPN | Ovo je servis Glavno ime (SPN) aplikacije za interne konfigurirano u na lokalno Azure AD. Na SPN dohvaćaju tokene Kerberos za aplikaciju koja koristi KCD upotrebljava Proxy poveznik aplikacija. |


## <a name="sso-for-non-windows-apps"></a>SSO za aplikacije koje nisu u sustavu Windows
Delegiranje tijek Kerberos u Proxy aplikacije za Azure AD se pokreće kada Azure AD potvrđuje korisnika u oblaku. Kada zahtjev stigne lokalnog Azure AD aplikacije Proxy poveznik Kerberos karata ime korisnika ponovnom instalacijom nakon interakcija s lokalnim servisom Active Directory. Taj postupak naziva se kao Kerberos ograničeno delegiranje (KCD). Sljedeće faze zahtjev se šalje s aplikacijom pozadinskog ovaj karata Kerberos. Mnoga protokola koje odredite kako Pošalji zahtjeve za pretraživanje kao. Većina poslužitelja – Windows očekivan Pregovaraj/SPNego koje sada podržane na Proxy aplikacije za Azure AD.

![Osobe koje nisu Windows SSO dijagrama](./media/active-directory-application-proxy-sso-using-kcd/app_proxy_sso_nonwindows_diagram.png)

### <a name="delegated-login-identity"></a>Delegirana prijava identiteta
Delegirana prijava identiteta pomaže vam rukovati dva scenarija različite prijave:

- Osobe koje nisu Windows aplikacije koje obično se identitet korisnika u obliku korisničko ime ili SAM naziv računa ne adresu e-pošte (username@domain).
- Konfiguracija zamjenski prijava gdje UPN-a u Azure AD i UPN-a servisa Active Directory lokalnog razlikuju.

S Proxy aplikacije, možete odabrati koji identiteta koristite da biste saznali karata Kerberos. Ta je postavka po aplikacije. Neke od tih mogućnosti su prikladna za sustave kojima je prihvatiti oblik adrese e-pošte, drugima dizajnirani za prijavu na drugu.

![Snimka zaslona za parametar identiteta ovlaštenih prijava](./media/active-directory-application-proxy-sso-using-kcd/app_proxy_sso_diff_id_upn.png)

Ako se koristi ovlaštenog prijava identiteta, vrijednost možda neće biti jedinstvena za sve domena ili šuma u tvrtki ili ustanovi. Taj se problem možete izbjeći objavljivanjem te aplikacije dvaput pomoću dvije različite grupe poveznik. Budući da svaku aplikaciju ima drugi korisnik publike, možete se uključiti njegov poveznika na drugu domenu.


## <a name="working-with-sso-when-on-premises-and-cloud-identities-are-not-identical"></a>Rad s SSO kada lokalni i oblaka identiteti nisu jednake
Ako u suprotnom konfigurirali Proxy aplikacije pretpostavlja da korisnici imaju točno istom identiteta u oblak i lokalne. Možete konfigurirati, za svaku aplikaciju koja identiteta treba koristiti prilikom izvršavanja jedinstvenu prijavu.  

Ova mogućnost dopušta mnogo tvrtkama ili ustanovama koje imaju različite identitete lokalno i oblaka da bi SSO iz oblaka lokalno aplikacije bez potrebe korisnika da biste unijeli drugu korisnička imena i lozinke. To obuhvaća tvrtkama ili ustanovama koje:

- Imate više domena interno (joe@us.contoso.com, joe@eu.contoso.com) i jednu domenu u oblaku (joe@contoso.com).

- Imate naziv domene koji nije usmjeravati interno (joe@contoso.usa) i pravne jedan u oblaku.

- Ne koristite interno naziva domena (joe)

- Korištenje različitih pseudonima lokalno i u oblaku. Npr. joe-johns@contoso.comDodavanje veze za vanjskih.joej@contoso.com  

To će i pomoći s aplikacijama koje prihvatiti adrese u obliku adresu e-pošte koja je vrlo uobičajeni scenarij – Windows pozadinskog poslužitelja.


### <a name="setting-sso-for-different-cloud-and-on-prem-identities"></a>Postavljanje SSO za različite identitete oblaka i lokalno

1. Konfiguriranje postavki za Azure AD Connect da bi se glavni identitet bit će adresu e-pošte (pošta). To možete učiniti u sklopu postupka Prilagodba tako da promijenite polja **Korisničko ime načelo** u postavkama sinkronizacije. Napomena da ove postavke i određuju kako korisnici prijavite se u sustav Office 365, Windows10 uređaja, i drugim aplikacijama koje koriste Azure AD kao spremište njihov identitet.  
  ![Označavanje snimka korisnika – padajući izbornik korisnikovo Glavno ime](./media/active-directory-application-proxy-sso-using-kcd/app_proxy_sso_diff_id_connect_settings.png)  
2. U odjeljku postavke konfiguracije aplikacije za aplikaciju koju želite izmijeniti odaberite **Delegirani identitet za prijavu** koja će se koristiti:
  - Korisničko ime načelo:joe@contoso.com  
  - Zamjenski korisničko ime načelo:joed@contoso.local  
  - Korisničko ime dio korisničko ime načelo: joe  
  - Korisničko ime dio zamjensko ime korisnika načelo: joed  
  - Naziv računa SAM lokalnog: zavisno lokalno konfiguracija kontrolera domena

  ![Snimka zaslona padajućeg izbornika za ovlaštenog prijava identiteta](./media/active-directory-application-proxy-sso-using-kcd/app_proxy_sso_diff_id_upn.png)  

### <a name="troubleshooting-sso-for-different-identities"></a>Upute za otklanjanje poteškoća SSO različite identitete
Ako je došlo do pogreške u postupku SSO je pojavit će se u zapisniku događaja na računalu poveznik kao što je opisano u [Otklanjanje poteškoća](active-directory-application-proxy-troubleshoot.md).
No u nekim slučajevima zahtjev uspješno poslat će se aplikacija pozadinskog dok se ova aplikacija će odgovor u različite druge odgovore HTTP. Otklanjanje poteškoća s tim slučajevima potrebno najprije Provjera broj 24029 događaja na računalu poveznika u zapisniku događaja na sesiju Proxy aplikacije. Identitet korisnika koji je korišten za delegiranje pojavit će se u polju "korisnik" unutar detalja o događaju. Da biste uključili zapisnik sesije, odaberite **Pokaži analitički i zapisnika za ispravljanje pogrešaka** u događaja preglednik prikaz izbornika.


## <a name="see-also"></a>Vidi također

- [Objavljivanje aplikacija pomoću Proxy aplikacije](active-directory-application-proxy-publish.md)
- [Otklanjanje poteškoća s Proxy aplikacije](active-directory-application-proxy-troubleshoot.md)
- [Rad s programima koji su svjesni zahtjevima](active-directory-application-proxy-claims-aware-apps.md)
- [Omogućivanje pristupa uvjetno](active-directory-application-proxy-conditional-access.md)

Najnovije vijesti i ažuriranja, potražite u članku [blog Proxy aplikacije](http://blogs.technet.com/b/applicationproxyblog/)


<!--Image references-->
[1]: ./media/active-directory-application-proxy-sso-using-kcd/AuthDiagram.png
[2]: ./media/active-directory-application-proxy-sso-using-kcd/Properties.jpg
