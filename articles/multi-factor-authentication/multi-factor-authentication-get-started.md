<properties
    pageTitle="Azure poslužitelja za dodavanje veze za vanjskih oblaka MFA | Microsoft Azure"
    description="Odaberite rješenje secutiry višestruke provjere autentičnosti koje je desno za vas zatražiti koje prijepodne i pokušaja sigurne i gdje se nalazi korisnike.  Zatim odaberite oblaka, MFA poslužitelja ili AD FS."
    services="multi-factor-authentication"
    documentationCenter=""
    authors="kgremban"
    manager="femila"
    editor="yossib"/>

<tags
    ms.service="multi-factor-authentication"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="10/14/2016"
    ms.author="kgremban"/>

#<a name="choose-the-azure-multi-factor-authentication-solution-for-you"></a>Odaberite rješenja Azure višestruka provjera autentičnosti

Jer postoji nekoliko flavors od Azure višestruke provjere autentičnosti (MFA), ne možemo morate odgovorite na nekoliko pitanja da biste utvrdili koja je verzija proper će se koristiti.  Ta pitanja su:

-   [Što se mogu pokušaja sigurne](#what-am-i-trying-to-secure)
-   [Gdje se nalaze korisnike](#where-are-the-users-located)
- [Koje značajke potrebno?](#what-featured-do-i-need)

U sljedećim se odjeljcima navedene su upute o određivanju svaki od tih odgovore.

## <a name="what-am-i-trying-to-secure"></a>Što se mogu pokušaja sigurne?

Da biste utvrdili rješenja za potvrdu točan dva koraka, najprije ćemo morate odgovoriti na pitanje što koju pokušavate sigurne s druga metoda provjere autentičnosti.  Je li aplikaciju koja se nalazi u Azure?  Ili sustav za daljinski pristup?  Tako da odredite što smo pokušavate sigurne, ne možemo možete odgovoriti na pitanje o kojem višestruka provjera autentičnosti mora biti omogućen.  


Što pokušavate sigurne| Višestruka provjera autentičnosti u oblaku|Višestruka provjera autentičnosti poslužitelja
------------- | :-------------: | :-------------: |
Aplikacija Microsoft prve strane|● |● |
SaaS aplikacije u galeriju aplikacija|● |● |
Aplikacija za IIS objavljene putem Proxy aplikacije za Azure AD|● |● |
IIS aplikacije koje nisu objavljene putem Proxy aplikacije za Azure AD | |● |
Daljinski pristup kao što je VPN, RDG| |● |



## <a name="where-are-the-users-located"></a>Gdje se nalaze korisnike

Nakon toga pogled na kojoj se nalaze naših korisnika pomaže da biste odredili točan rješenja za korištenje u oblaku ili na lokalnim pomoću poslužitelja za MFA.



Korisničke lokacije| Višestruka provjera autentičnosti u oblaku|Višestruka provjera autentičnosti poslužitelja
------------- | :-------------: | :-------------: |
Azure Active Directory|● | |
Azure AD i lokalnog servisa Active Directory pomoću vanjski pristup za AD FS|● |● |
Azure AD i lokalnog servisa Active Directory pomoću alata DirSync, Azure AD sinkronizacije, Azure AD Connect – sinkronizaciju bez lozinke|● |● |
Azure AD i lokalnog servisa Active Directory pomoću alata DirSync, Azure AD sinkronizacije, Azure AD Connect – sinkronizaciju lozinke|● | |
Lokalni Active Directory| |● |

## <a name="what-features-do-i-need"></a>Koje značajke potrebno?

U sljedećoj su tablici je usporedbu značajki koje su dostupne s višestruke provjere autentičnosti u oblak i provjeru autentičnosti poslužitelja višestruke provjere.

 | Višestruka provjera autentičnosti u oblaku | Višestruka provjera autentičnosti poslužitelja
------------- | :-------------: | :-------------: |
Mobilne aplikacije obavijesti kao drugi faktor | ● | ● |
Kod za potvrdu mobilne aplikacije kao drugi faktor | ● | ●
Telefonski poziv kao drugi analiza varijance | ● | ●
Jednosmjerna SMS kao drugi analiza varijance | ● | ●
Dvosmjerna SMS kao drugi analiza varijance |  | ●
Hardverski tokeni kao drugi analiza varijance |  | ●
Aplikacija lozinke za klijente koji ne podržavaju MFA | ● |  
Administrator kontrolu nad metoda provjere autentičnosti | ● | ●
Način PIN-a |  | ●
Upozorenje prijevara | ● | ●
MFA izvješća | ● | ●
Jednokratno zaobilazak |  | ●
Prilagođeni čestitke za telefonske pozive | ● | ●
Prilagodljive Pozivatelja za telefonske pozive | ● | ●
Pouzdani IP-ovi | ● | ●
Imajte na umu MFA pouzdanih uređaja  | ● |  
Uvjetno programa access | ● | ●
Predmemoriju |  | ●

Nakon što smo zaključili hoćete li koristiti oblaka višestruke provjere autentičnosti ili u MFA informacije o lokalnom poslužitelju, ne možemo mogli početi s radom postavljanje i korištenje Azure višestruke provjere autentičnosti. **Odaberite ikonu koja predstavlja scenariju!**

<center>




[![Cloud](./media/multi-factor-authentication-get-started/cloud2.png)](multi-factor-authentication-get-started-cloud.md)&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[![Proofup](./media/multi-factor-authentication-get-started/server2.png)](multi-factor-authentication-get-started-server.md)  &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
</center>
