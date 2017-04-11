<properties
   pageTitle="Kako aplikacije dodaju Azure Active Directory."
   description="U ovom se članku opisuje kako se dodaju aplikacija instancu Azure Active Directory."
   services="active-directory"
   documentationCenter=""
   authors="shoatman"
   manager="kbrint"
   editor=""/>

   <tags
      ms.service="active-directory"
      ms.devlang="na"
      ms.topic="article"
      ms.tgt_pltfrm="na"
      ms.workload="identity"
      ms.date="02/09/2016"
      ms.author="shoatman"/>

# <a name="how-and-why-applications-are-added-to-azure-ad"></a>Kako i zašto aplikacije dodaju se Azure AD

Nešto od prethodno zbunjujućih kada pregledavate popis aplikacija u instanci programa Azure Active Directory je objašnjenje koju ste dobili aplikacije iz i zašto su tamo.  U ovom članku pronaći ćete više razine pregled kako aplikacija prikazani su u direktoriju i ponuditi kontekstu koji će vam pomoći u objašnjenje kako ste dobili aplikacije u direktoriju.

## <a name="what-services-does-azure-ad-provide-to-applications"></a>Koji su servisi Azure AD nudi aplikacijama?

Aplikacija se dodaju Azure AD odražava jedan ili više usluga pruža.  Tih servisa obuhvaćaju sljedeće:

* Provjere autentičnosti za aplikaciju i autorizacije
* Provjera autentičnosti korisnika i autorizacije
* Jedinstvenu prijavu (SSO) pomoću vanjski pristup ili lozinka
* Dodjeljivanje korisnika i sinkronizacije
* Kontrola pristupa na temelju uloga; Korištenje direktorija da biste definirali uloge aplikacije da biste izvršili uloge temelji autorizacije provjere u aplikaciju.
* servisi za provjeru autentičnosti oAuth (koristi Office 365 i drugih aplikacija za Microsoft da biste autorizirali pristup API-resursima.)
* Objavljivanje aplikacija i proxy; Objavljivanje aplikacije iz privatna mreža s Internetom

## <a name="how-are-applications-represented-in-the-directory"></a>Kako aplikacije prikazani su u imeniku?

Aplikacija prikazani su u Azure AD pomoću 2 objekata: objekt aplikacija i objektima upravitelja za servis.  Postoji jedan objekt aplikacija registrira u "kuće" i "vlasnik" ili "Objavljivanje" direktorija i jedan ili više usluga glavni objekata koji predstavlja aplikacije u svakoj direktoriju u kojem se bavi.  

Objekt aplikacije opisuje aplikaciju za Azure AD (servis više klijentu), a može sadržavati bilo što od sljedećeg: (*Napomena*: nije iscrpan popis.)

* Naziva, logotipa i Publisher
* Tajne (simetričnu i/ili asimetričnim tipke provjerava autentičnost na temelju aplikaciju)
* API ovisnosti (oAuth)
* API-ji/resursa/opsega objavljene (oAuth)
* Uloge aplikacije (RBAC)
* SSO metapodataka i konfiguracija (SSO)
* Korisnik dodjeljivanje metapodataka i konfiguracija
* Proxy metapodataka i konfiguracija

Upravitelj servisa je zapis aplikacije u direktoriju svaki gdje aplikacije djeluje uključujući njegove osnovne mape.  Upravitelj servisa:

* Upućuje na objekt aplikacija putem svojstvo id aplikacije
* Lokalni korisnik zapisa i dodjele aplikacija uloge grupa
* Dodijeliti zapisa lokalni korisnik i administrator dozvole za aplikaciju
    * Na primjer: dozvole za aplikaciju da biste pristupili e-pošte određenog korisnika
* Lokalna pravila zapisa uključujući pravila uvjetnog programa access
* Zapisi lokalne zamjenski lokalne postavke za aplikaciju
    * Sporove transformaciju pravila
    * Mapiranja atributa (korisnik dodjele resursa)
    * Smjernice za određenu aplikaciju uloge (Ako je aplikacija podržava prilagođene uloge)
    * Naziv/logotipa

### <a name="a-diagram-of-application-objects-and-service-principals-across-directories"></a>Dijagram objekata aplikacija i servisa upravitelji preko direktorija

![Dijagram ilustrira način na koji aplikacija objekte i upravitelji postojeće s instancama Azure AD za servis.][apps_service_principals_directory]

Kao što možete vidjeti iz dijagrama gore.  Microsoft zadržava dva direktorija interno (na lijevoj strani) koristi da bi se objaviti.

* Jedan za Microsoft Apps (imenik servisa Microsoft)
* Jedan za unaprijed integrirane 3 proizvođača aplikacije (galeriju aplikacija imenik)

Aplikacija izdavači/proizvođači dokumenata koji bi integrirati s Azure AD moraju imati objavljivanje direktorija.  (Neke imenik SAAS).

Aplikacije koje ste sami dodali obuhvaćaju sljedeće:

* Aplikacije koje ste razvili (integriran s AAD)
* Aplikacija povezani za-jedinstvene prijave
* Aplikacija je objavljen pomoću proxy poslužitelj za Azure AD aplikacije.

### <a name="a-couple-of-notes-and-exceptions"></a>Nekoliko bilješke i iznimke

* Sve usluge upravitelji pokažite natrag objekti aplikacije.  Huh? Kada je izvorno gradnje Azure AD usluge aplikacijama su znatno ograničena i glavnicu servis je dovoljno za uspostavljanje identitet aplikacije.  Izvorni servis upravitelja je bliže u obliku račun servisa Active Directory za Windows Server.  Zbog toga je moguće je stvoriti upravitelji servis pomoću Azure AD PowerShell bez stvaranja objekt aplikacija.  API grafikonu zahtijeva objekt aplikacije prije stvaranja servisa glavni.
* Sve informacije na prethodno opisan je trenutno vidljiva programski.  Sljedeće su dostupne samo u korisničkom Sučelju:
    * Sporove transformaciju pravila
    * Mapiranja atributa (korisnik dodjele resursa)
* Dodatne detaljnije informacije na servis glavnicu i objekti aplikacije potražite u dokumentaciji referenca Azure AD grafikonu REST API-JA.  *Savjet*: najsličniji što biste referenca sheme je dokumentacija API na grafikonu Azure AD za Azure AD trenutno nije dostupno.  
    * [Aplikacija](https://msdn.microsoft.com/library/azure/dn151677.aspx)
    * [Usluge](https://msdn.microsoft.com/library/azure/dn194452.aspx)


## <a name="how-are-apps-added-to-my-azure-ad-instance"></a>Kako aplikacija dodaju se moj Azure AD instance?
Azure AD moguće dodati aplikaciju na više načina:

* Dodavanje aplikacije u [Galeriju aplikacija za Azure Active Directory](https://azure.microsoft.com/updates/azure-active-directory-over-1000-apps/)
* Znak gore/u s 3 proizvođača aplikacije integriran s Azure Active Directory (na primjer: [Smartsheet](https://app.smartsheet.com/b/home) ili [DocuSign](https://www.docusign.net/member/MemberLogin.aspx))
    * Tijekom znak gore/korisnika će se zatražiti da biste dodijelili dozvolu za aplikaciju da biste pristupili svoj profil i druge dozvole.  Prvu osobu da bi se dobilo pristanak uzrokuje glavni servis koji predstavlja aplikaciju dodati u direktoriju.
* Prijavite se gore/u Microsoft online services, kao što je [Office 365](http://products.office.com/)
    * Kada pretplata na Office 365 ili započeti probnu verziju jedne ili više usluga upravitelji stvaraju se u direktoriju koji predstavlja različite servise koji se koriste za održavanje sve funkcije povezan sa sustavom Office 365.
    * Neke servisa Office 365 kao što je SharePoint stvorite servisa Upravitelji na osnovi na početak da biste omogućili sigurnu komunikaciju između komponente uključujući tijekova rada.
* Dodavanje aplikacije razvijate u potražite u članku Portal za upravljanje Azure: https://msdn.microsoft.com/library/azure/dn132599.aspx
* Dodavanje aplikacije razvijate pomoću Visual Studio potražite u članku:
    * [Metoda provjere autentičnosti platforme ASP.Net](http://www.asp.net/visual-studio/overview/2013/creating-web-projects-in-visual-studio#orgauthoptions)
    * [Povezani servisi](http://blogs.msdn.com/b/visualstudio/archive/2014/11/19/connecting-to-cloud-services.aspx)
* Dodavanje aplikacije za rad [Proxy aplikacije za Azure AD](https://msdn.microsoft.com/library/azure/dn768219.aspx)
* Povezivanje aplikacije za jedine prijave pomoću SAML ili SSO za lozinke
* Mnoge druge uključujući različite sadržaje za razvojne inženjere u Azure i/u programu explorer API iskustvo na različitim centre za razvojne inženjere

## <a name="who-has-permission-to-add-applications-to-my-azure-ad-instance"></a>Tko ima dozvole za dodavanje aplikacije na moj instancu Azure AD?

Samo globalni administratori mogu:

* Dodavanje aplikacije iz galerije Azure AD aplikacije (unaprijed integrirane 3 proizvođača aplikacije)
* Objavljivanje aplikacije pomoću aplikacije Proxy za AD Azure

Svi korisnici u direktoriju imaju prava za dodavanje aplikacije koje oni su razvoj i diskretni putem koji su zajedničko korištenje/dati pristup da biste svoje podatke iz tvrtke ili ustanove.  *Zapamti za prijavu gore/za aplikaciju i dodjelu dozvola može uzrokovati glavni servis stvoren.*

U ovom možda prethodno zvuk koji se odnosi, ali imajte na umu sljedeće:

* Aplikacija je mogućnost odražava Windows Server Active Directory za provjeru autentičnosti korisnika za mnoge godine bez aplikacija biti registriran/naveden u direktoriju.  Sada u tvrtki ili ustanovi će poboljšani vidljivost točno koliko aplikacije pomoću direktorija i what za.
* Nema potrebe za administratore utemeljenima na postupak objavljivanja/Registracija aplikacije.  S Active Directory Federation Services je vjerojatno da ste administrator da biste dodali aplikaciju kao relying strana ime razvojnim inženjerima.  Sada razvojni inženjeri mogli samostalne.
* Korisnici prijava/najviše pomoću računa tvrtke ili ustanove za potrebe tvrtke je dobro.  Ako se naknadno napuštaju tvrtku ili ustanovu oni izgubit ćete pristup putem računa u aplikaciji se koriste.
* Imate zapis podataka koje je omogućeno zajedničko korištenje s kojom je aplikacija dobro.  Podaci su više daljnjeg nego ikad i pritom Očisti zapis koji je zajednički koristiti podatke s kojim aplikacijama koristan.
* Aplikacije koje koriste Azure AD za oAuth odlučite točno potrebne dozvole koje korisnici mogu odobriti aplikacije i koje dozvole zahtijevaju administrator biste.  Otvorite treba bez pričaju pristajete da možete samo administratori veće opsega i više značajan dozvole.
* Korisnici su Dodavanje i dopuštanje aplikacije da bi se pristupati svojim podacima nadzirati događaja da biste mogli vidjeti izvješća nadzora unutar portala za upravljanje Azure da biste odredili kako aplikaciju dodan u direktoriju.

**Bilješke:** *Microsoft sam sadrži je operacijski pomoću zadanu konfiguraciju za više mjeseci sada.*

Sa sve rečeno je moguće da biste spriječili da korisnici u direktoriju dodavanje aplikacije i iz exercising diskretni putem koje se informacije mogu zajedničko korištenje s aplikacijama izmjenom konfiguracije Directory na portalu za upravljanje Azure.  Sljedeće konfiguracije mogu pristupati unutar portala za upravljanje Azure na kartici vaš imenik "Konfiguriraj".

![Snimka zaslona s korisničkim Sučeljem za konfiguriranje postavki integrirane aplikacije][app_settings]


<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->
## <a name="next-steps"></a>Daljnji koraci

Saznajte više o tome kako dodati aplikacije za Azure AD i upute za konfiguriranje servisa za aplikacije.

* Razvojni inženjeri: [Saznajte kako integrirati aplikacije s AAD](https://msdn.microsoft.com/library/azure/dn151122.aspx)
* Razvojni inženjeri: [Pregled uzorak koda za aplikacije integriran s Azure Active Directory na Github](https://github.com/AzureADSamples)
* Razvojni inženjeri i IT profesionalce: [Pregled Azure grafikonu API za Active Directory potražite u dokumentaciji REST API -JA](https://msdn.microsoft.com/library/azure/hh974478.aspx)
* INFORMATIČKI profesionalci: [Saznajte kako koristiti Azure Active Directory unaprijed integrirane aplikacije iz galerije aplikacije](https://msdn.microsoft.com/library/azure/dn308590.aspx)
* INFORMATIČKI profesionalci: [za konfiguriranje određene unaprijed integrirane aplikacije](https://msdn.microsoft.com/library/azure/dn893637.aspx)
* INFORMATIČKI profesionalci: [Saznajte kako objaviti aplikacije pomoću proxy poslužitelj za Azure Active Directory aplikacije](https://msdn.microsoft.com/library/azure/dn768219.aspx)

## <a name="see-also"></a>Vidi također

- [Članak indeks za upravljanje aplikacijama servisa Azure Active Directory](active-directory-apps-index.md)

<!--Image references-->
[apps_service_principals_directory]:media/active-directory-how-applications-are-added/HowAppsAreAddedToAAD.jpg
[app_settings]:media/active-directory-how-applications-are-added/IntegratedAppSettings.jpg
