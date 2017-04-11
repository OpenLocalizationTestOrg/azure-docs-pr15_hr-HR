<properties 
    pageTitle="Konfiguriranje jedinstvene prijave za aplikacije koje nisu u galeriji aplikacija Azure Active Directory | Microsoft Azure" 
    description="Saznajte kako samostalne povezivanje aplikacije Azure Active Directory pomoću SAML i SSO za-poštu" 
    services="active-directory" 
    authors="asmalser-msft"  
    documentationCenter="na" manager="femila"/>
<tags 
    ms.service="active-directory" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.tgt_pltfrm="na" 
    ms.workload="identity" 
    ms.date="02/09/2016" 
    ms.author="asmalser" />

#<a name="configuring-single-sign-on-to-applications-that-are-not-in-the-azure-active-directory-application-gallery"></a>Konfiguriranje jedinstvene prijave za aplikacije koje nisu u galeriji aplikacije Azure Active Directory

Ovaj je članak o značajki koje omogućuju administratorima da biste konfigurirali jedinstvenu prijavu aplikacijama ne postoji u na Azure Active Directory aplikacije Galerija *bez pisanja koda*. Ova značajka izdavanja iz Tehnički pretpregled u studenom 18th, 2015 i je sve obuhvaćeno [Azure Active Directory Premium](active-directory-editions.md). Ako umjesto vas zanimaju za razvojne inženjere smjernice o integraciji prilagođenih aplikacija pomoću Azure AD putem koda, potražite u članku [Provjera autentičnosti scenariji za Azure AD](active-directory-authentication-scenarios.md).

Galerija aplikacije za Azure Active Directory sadrži popis aplikacija koje se gube se za podršku oblik jedinstvenu prijavu s Azure Active Directory, kao što je opisano u [ovom članku](active-directory-appssoaccess-whatis.md). Kada (kao što je IT stručnjak ili sustav integrator u tvrtki ili ustanovi) pronađete aplikaciju koju želite povezati, koje mogu započeti tako da slijedite detaljne upute navedene na portalu za upravljanje Azure da biste omogućili jedinstvenu prijavu.

Korisnici licenci [Azure Active Directory Premium](active-directory-editions.md) se i sljedeće dodatne mogućnosti:

* Samostalno Integracija s bilo kojeg programa koji podržava davatelji identiteta SAML 2.0 (SP pokrenut ili IdP pokrenut)
* Samostalno integracije s web-aplikacije koja ima programa koji se temelji na HTML stranica za prijavu u koristiti [SSO za-poštu](active-directory-appssoaccess-whatis.md#password-based-single-sign-on)
* Samostalno vezu aplikacije koje koristi protokol SCIM za korisnika dodjele resursa ([što je opisano u nastavku](active-directory-scim-provisioning.md))
* Mogućnost da biste dodali veze na bilo koji program u [pokretač aplikacija za Office 365](https://blogs.office.com/2014/10/16/organize-office-365-new-app-launcher-2/) ili [Azure AD teksta](active-directory-appssoaccess-whatis.md#deploying-azure-ad-integrated-applications-to-users)

To se može obuhvaćati ne samo SaaS aplikacije koje koristite, ali ste nije još nije na-boarded u galeriju aplikacija za Azure AD, ali proizvođača aplikacija koja je vaša tvrtka ili ustanova postavila poslužiteljima određujete u oblaku ili na lokalnim.

Ove mogućnosti, poznata i kao *Predlošci integraciju aplikacija*točkama povezivanja utemeljenih na standardima omogućavaju aplikacije koje podržavaju SAML, SCIM ili provjere autentičnosti utemeljene na obrascima i uključiti dio su fleksibilnih mogućnosti i postavki kompatibilnosti s općenite broj aplikacija. 

##<a name="adding-an-unlisted-application"></a>Dodavanje nenavedenim aplikacije

Povezati zatvaranje pomoću predloška aplikacije programa Integracija prijavite se na portal za upravljanje Azure pomoću računa za administratora Azure Active Directory, a zatim prijeđite na **servisa Active Directory > [direktorija] > aplikacije** sekcije, odaberite **Dodaj**, a zatim **Dodavanje aplikacije iz galerije**. 

![][1]

Aplikaciju nenavedenim pomoću **prilagođene** kategorije na lijevoj strani ili tako da odaberete **Dodaj aplikaciju nenavedenim** vezu prikazane u rezultatima pretraživanja ako nije pronađena željenu aplikaciju možete dodati u galeriju aplikacija. Nakon unosa naziva aplikacije, možete konfigurirati jedan mogućnosti za prijavu i ponašanje. 

**Brz savjet**: Najbolji način pomoću funkcije search Provjerite postoji li već aplikacija u galeriji aplikacije. Ako se nalazi li se aplikacija i njezin opis spominjanja "jedine prijave", a zatim aplikaciju već nije podržana za vanjsko jedinstvenu prijavu. 

![][2]

Dodavanje aplikacije na taj način nudi vrlo slično iskustvo onome koji su dostupni za unaprijed integrirane aplikacije. Da biste započeli, odaberite **Konfigurirati jedinstvenu prijavu**. Na sljedećem zaslonu prikazuju se sljedeće tri mogućnosti za konfiguriranje znak, koje su opisane u sljedećim odjeljcima.

![][3]


##<a name="azure-ad-single-sign-on"></a>Azure AD jedinstvene prijave

Odaberite ovu mogućnost da biste konfigurirali provjere autentičnosti utemeljene na SAML za aplikaciju. To su potrebne da SAML 2.0 podrška za aplikacije, a trebali biste prikupljanje informacija o tome kako koristiti mogućnosti SAML aplikacije prije nastavka. Kad odaberete **Dalje**, zatražit će se da biste unijeli tri različite URL-ovi koji odgovaraju krajnje točke SAML za aplikaciju. 

![][4]
 
To su:

* **Prijava na URL-a (SP pokrenut samo)** – gdje dolazi korisnika da se prijavite u ovoj aplikaciji. Ako aplikacija je konfiguriran za izvođenje servisa davatelja pokrenut znak na, zatim kada korisnik vodi na ovom URL-u, davatelj usluga učinite potrebne preusmjeravanje za Azure AD za provjeru autentičnosti i prijavite se korisnik nalazi. Ako polje se popunjava, pa Azure AD koristi ovaj URL za pokretanje programa iz sustava Office 365 i Azure AD pristup ploča. Ako je polje ommited, a zatim Azure AD umjesto izvođenje davatelja identiteta-pokrenuti prijaviti prilikom pokretanja aplikacije iz sustava Office 365, na ploči za Azure AD Access ili Azure AD jedan prijave URL-a (copiable na kartici nadzorne ploče).

* **Izdavač URL** - izdavač URL-a trebali biste identificirati samo aplikacije za koji znak na koji se konfigurira. To je vrijednost koja Azure AD šalje natrag aplikacije kao parametar **ciljne skupine** SAML tokena, a aplikacija se očekuje da biste provjerili je. Ta vrijednost pojavit će se kao **ID entitet** u bilo kojem metapodacima SAML nudi aplikacije U dokumentaciji aplikacije SAML pojedinosti o tome što je entitet ID-a ili je vrijednost ciljne skupine. Slijedi primjer kako URL publici pojavit će se u SAML token vraćaju u aplikaciju:

```
    <Subject>
    <NameID Format="urn:oasis:names:tc:SAML:2.0:nameid-format:unspecificed">chad.smith@example.com</NameID>
        <SubjectConfirmation Method="urn:oasis:names:tc:SAML:2.0:cm:bearer" />
      </Subject>
      <Conditions NotBefore="2014-12-19T01:03:14.278Z" NotOnOrAfter="2014-12-19T02:03:14.278Z">
        <AudienceRestriction>
          <Audience>https://tenant.example.com</Audience>
        </AudienceRestriction>
      </Conditions>
```

* **Odgovori URL** - odgovor je URL gdje se aplikacija očekuje prima SAML token. To se naziva **URL pridruživanju potrošača servisa (ACS)**. U dokumentaciji aplikacije SAML detalje o što je njegova SAML tokena odgovor URL-a ili ACS URL-a.
 Nakon što ih unesena, kliknite **Dalje** da biste prešli na sljedećem zaslonu. Ovaj zaslon daje informacije o što potrebno je konfigurirati na strani aplikacije da biste omogućili da biste prihvatili SAML tokena iz Azure AD. 

![][5]

Vrijednosti koje su potrebni razlikuju se ovisno o aplikacije, pa u dokumentaciji aplikacije SAML pojedinosti. Za **Prijavu** i **Sign-Out** URL-a i riješili isti krajnjoj, što je zahtjev za obradu krajnje SAML za instanci programa Azure AD za servis. URL izdavač je vrijednost koja će se prikazati kao "Izdavač" unutar SAML token izdala aplikacije. 

Kada aplikacija konfiguriran, kliknite gumb **Dalje** , a zatim **Dovršeno** da biste zatvorili dijaloški okvir. 

##<a name="assigning-users-and-groups-to-your-saml-application"></a>Dodjeljivanje korisnicima i grupama u aplikaciji SAML 

Kada aplikacija konfiguriran za korištenje Azure AD kao davatelja identiteta utemeljene na SAML, zatim gotovo spremna je za testiranje. Kao kontrolu sigurnosti Azure AD nije poslao token im se prijaviti u aplikaciju, osim ako ih je pristup odobren pomoću Azure AD na taj način. Korisnici mogu dobiti pristup izravno ili putem grupu čiji su članovi. 

Da biste dodijelili korisnika ili grupu u aplikaciji, kliknite gumb **Dodijeliti korisnicima** . Odaberite korisnika ili grupu koju želite dodijeliti, a zatim gumb **dodijeliti** . 

![][6]

Dodjela korisnika da bi Azure AD za izdavanje tokena za korisnika, kao i uzrokuje pločicu za ovu aplikaciju da se pojavi na ploči pristupa za korisnika. Na pločicu aplikacije će se prikazivati u pokretaču aplikacija sustava Office 365 ako korisnik koristi Office 365. 

Možete prenijeti logotipa pločica za aplikaciju pomoću gumba **Logotip prenijeli** na kartici **Konfiguriraj** za aplikaciju. 

###<a name="customizing-the-claims-issued-in-the-saml-token"></a>Prilagodba zahtjevima izdavanja u SAML tokena 

Kada korisnik se autentificira servisa na aplikaciju, Azure AD će izdati SAML token aplikaciju koja sadrži podatke (ili zahtjevima) o korisnika koji služi kao jedinstvena identifikacija ih. Prema zadanim postavkama To uključuje korisnika korisničko ime, adresu e-pošte, ime i prezime. 

Mogu pregledavati ili uređivati zahtjevima šalje u token SAML aplikaciju na kartici **atribute** . 

![][7]

Postoje dva moguća razloga zašto možda ćete morati urediti zahtjevima izdavanja u SAML token: •maksimalnu aplikacije napisan obavezne drugačiji skup zahtjeva ji ili vrijednosti zahtjeva •Your aplikacije implementiran na način koji zahtijeva na NameIdentifier zahtjeva za bilo koju drugu osim korisničko ime (AKA korisnikovo Glavno ime) pohranjene u Azure Active Directory. 

Informacije o tome kako dodati i uređivanje zahtjevima za scenarija, pogledajte [članak prilagođavanje zahtjevima](active-directory-saml-claims-customization.md). 

###<a name="testing-the-saml-application"></a>Testiranje SAML aplikacije 

Kada SAML URL-ovi i certifikatu nije konfiguriran u Azure AD i u aplikaciji, korisnike ili grupe su dodijeljene aplikaciju Azure, i na zahtjevima pregledati i urediti ako je potrebno, a zatim je korisnik spremni prijaviti u aplikaciju. 

Da biste testirali, jednostavno znak-u Azure AD pristup ploču https://myapps.microsoft.com putem korisničkog računa koji ste dodijelili aplikacije, a zatim na pločici za aplikaciju da biste pokrenuli proces jedan prijave. Umjesto toga možete pregledavati izravno na URL prijave za aplikacije i prijaviti iz nje. 

Savjeti za ispravljanje pogrešaka, potražite u [članku za ispravljanje pogrešaka utemeljene na SAML jedinstvenu prijavu aplikacijama](active-directory-saml-debugging.md) 

##<a name="password-single-sign-on"></a>Lozinka jedinstvenu prijavu

Odaberite ovu mogućnost da biste konfigurirali [operacijskim sustavom jedinstvenu prijavu](active-directory-appssoaccess-whatis.md) za web-aplikacije koja sadrži HTML prijavu stranicu. Operacijskim sustavom SSO naziva kao što je lozinka vaulting, omogućuje upravljanje korisničkim pristupom i lozinke za web-aplikacije koji ne podržavaju vanjski pristup identitetu. Također je korisno za scenariji kojima više korisnika morati zajedničko korištenje putem jednog računa, kao što su račune aplikacija za društvene mreže vaše tvrtke ili ustanove. 

Kad odaberete **Dalje**, zatražit će se unijeti URL aplikacije koji se temelji na web stranica za prijavu u. Imajte na umu da to mora biti stranicu koja sadrži polja unos korisničkog imena i lozinke. Nakon što ste unijeli, Azure AD pokreće postupak raščlaniti stranici za prijavu za unos korisničko ime i lozinku za unos. Ako se postupak ne uspije, pa ga će vas voditi kroz zamjenski postupak instalacije (zahtijeva Internet Explorer, Chrome i Firefox) proširenje preglednika koji vam omogućuje da ručno snimite polja.

Kada se stranica za prijavu u hvata, korisnici i grupe mogu biti dodijeljena i vjerodajnica pravila možete postaviti baš kao i obične [lozinku SSO aplikacije](active-directory-appssoaccess-whatis.md).

Napomena: Možete prenijeti logotipa pločica za aplikaciju pomoću gumba **Logotip prenijeli** na kartici **Konfiguriraj** za aplikaciju. 

##<a name="existing-single-sign-on"></a>Postojeće jedinstvenu prijavu

Odaberite ovu mogućnost da biste dodali vezu na aplikaciju na portal Azure AD teksta ili Office 365 svoje tvrtke ili ustanove. Možete koristiti sljedeće da biste dodali veze na prilagođenu web-aplikacije, a koji trenutno koristite Azure Active Directory Federation Services (ili drugog servisa za vanjski pristup) umjesto Azure AD za provjeru autentičnosti. Ili možete dodati precizno veze do određene stranice sustava SharePoint ili drugih web-stranica samo će se prikazivati na vaš korisnički pristup ploče. 

Kad odaberete **Dalje**, zatražit će se unijeti URL aplikacije da biste se povezali. Nakon dovršetka, korisnici i grupe mogu biti dodijeljena aplikacije koji uzrokuje računala da se prikazuju u [pokretač aplikacija za Office 365](https://blogs.office.com/2014/10/16/organize-office-365-new-app-launcher-2/) ili [ploča programa access za Azure AD](active-directory-appssoaccess-whatis.md#deploying-azure-ad-integrated-applications-to-users) za tim korisnicima.

Napomena: Možete prenijeti logotipa pločica za aplikaciju pomoću gumba **Logotip prenijeli** na kartici **Konfiguriraj** za aplikaciju.

## <a name="related-articles"></a>Povezani članci

- [Članak indeks za upravljanje aplikacijama servisa Azure Active Directory](active-directory-apps-index.md)
- [Kako prilagoditi zahtjevima izdavanja u tokena SAML za unaprijed integrirane aplikacije](active-directory-saml-claims-customization.md)
- [Otklanjanje poteškoća utemeljene na SAML jedinstvenu prijavu](active-directory-saml-debugging.md)

<!--Image references-->
[1]: ./media/active-directory-saas-custom-apps/customapp1.png
[2]: ./media/active-directory-saas-custom-apps/customapp2.png
[3]: ./media/active-directory-saas-custom-apps/customapp3.png
[4]: ./media/active-directory-saas-custom-apps/customapp4.png
[5]: ./media/active-directory-saas-custom-apps/customapp5.png
[6]: ./media/active-directory-saas-custom-apps/customapp6.png
[7]: ./media/active-directory-saas-custom-apps/customapp7.png
