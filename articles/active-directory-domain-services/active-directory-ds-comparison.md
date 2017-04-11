<properties
    pageTitle="Azure AD domenske servise: Usporedba domene Azure AD Services da biste kontrolera SAMOSTALNO domena | Microsoft Azure"
    description="Usporedba Azure Active Directory Domain Services s kontrolera SAMOSTALNO domena"
    services="active-directory-ds"
    documentationCenter=""
    authors="mahesh-unnikrishnan"
    manager="stevenpo"
    editor="curtand"/>

<tags
    ms.service="active-directory-ds"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/01/2016"
    ms.author="maheshu"/>

# <a name="how-to-decide-if-azure-ad-domain-services-is-right-for-your-use-case"></a>Kako odlučiti ako Azure servisa Active Directory Domain Services je za korištenje – slučaja
Azure AD domenske servise omogućuje vam za implementaciju sustava radnih opterećenja u Azure infrastrukture servise, a da ne morate brinuti o zaštiti infrastruktura za identitet. Servis za upravljane razlikuje se od standardne implementacije Active Directory za Windows Server koja implementacije i Administracija na vlastitu. Servis je namijenjen olakšani implementaciju, nadzor automatiziranog stanja i potražite alat za i jednostavne identiteta infrastrukture za oblaka. Ne možemo stalno su razvoj servisa da biste dodali podrška za uobičajeni scenariji za implementaciju.

Da biste odlučili želite li koristiti Azure servisa Active Directory Domain Services ili Okretni prema gore i upravljanje vlastitim (samostalno) u Azure AD infrastrukture:

- Pogledajte popis [značajke koje nudi Azure servisa Active Directory Domain Services](active-directory-ds-features.md).

- Pregledajte uobičajeni [scenariji implementaciju za Azure AD domenske servise](active-directory-ds-scenarios.md).

- Na kraju, [usporedite Azure AD domenske servise za samostalno AD mogućnost](active-directory-ds-comparison.md#compare-azure-ad-domain-services-to-diy-ad-domain-in-azure).


## <a name="compare-azure-ad-domain-services-to-diy-ad-domain-in-azure"></a>Usporedba Azure AD domenske servise za SAMOSTALNO AD domene u Azure
U sljedećoj su tablici pomaže vam odabrati pomoću servisa Azure AD domena i upravljanje vlastitim AD infrastrukture u Azure.

|**Značajka**|**Azure AD domenske servise**|**"Samostalno AD u Azure VMs**|
|---|:---:|:---:|
|[**Servis za upravljane**](active-directory-ds-comparison.md#managed-service)|**& #x 2713;**|**& #x 2715;**|
|[**Sigurna implementacije**](active-directory-ds-comparison.md#secure-deployments)|**& #x 2713;**|Administrator vam mora sigurne uvođenje.|
|[**DNS poslužitelj**](active-directory-ds-comparison.md#dns-server)|**& #x 2713;** (servis za upravljane)|**& #x 2713;**|
|[**Domena ili Enterprise administratorske ovlasti**](active-directory-ds-comparison.md#domain-or-enterprise-administrator-privileges)|**& #x 2715;**|**& #x 2713;**|
|[**Uključivanje domene**](active-directory-ds-comparison.md#domain-join)|**& #x 2713;**|**& #x 2713;**|
|[**Provjera autentičnosti domene pomoću NTLM i Kerberos**](active-directory-ds-comparison.md#domain-authentication-using-ntlm-and-kerberos)|**& #x 2713;**|**& #x 2713;**|
|[**Prilagođeni OU strukture**](active-directory-ds-comparison.md#custom-ou-structure)|**& #x 2713;**|**& #x 2713;**|
|[**Proširenja shema**](active-directory-ds-comparison.md#schema-extensions)|**& #x 2715;**|**& #x 2713;**|
|[**Smatra pouzdanima skupa stabala/AD domene**](active-directory-ds-comparison.md#ad-domain-or-forest-trusts)|**& #x 2715;**|**& #x 2713;**|
|[**Čitanje LDAP**](active-directory-ds-comparison.md#ldap-read)|**& #x 2713;**|**& #x 2713;**|
|[**Sigurna LDAP (LDAPS)**](active-directory-ds-comparison.md#secure-ldap)|**& #x 2713;**|**& #x 2713;**|
|[**Pisanje LDAP**](active-directory-ds-comparison.md#ldap-write)|**& #x 2715;**|**& #x 2713;**|
|[**Pravilnik grupe**](active-directory-ds-comparison.md#group-policy)|Jednostavan|Potpuna|
|[**Implementacija distributed za zemlj.**](active-directory-ds-comparison.md#geo-dispersed-deployments)|**& #x 2715;**|**& #x 2713;**|


#### <a name="managed-service"></a>Servis za upravljane
Azure domene servisa Active Directory Domain Services upravlja Microsoft. Ne morate brinuti o zakrpa, ažuriranja, nadzor, kopija, i Provjera dostupnosti domene. Ove zadatke upravljanja onih koje nudi kao servisa Microsoft Azure za upravljane domene.

#### <a name="secure-deployments"></a>Sigurna implementacije
Upravljani domene sigurno zaključan prema dolje po tvrtke Microsoft sigurnost najbolje prakse za AD implementacije. Najbolje prakse skrati s tim proizvoda za AD desetljeća sučelja inženjerska i Implementacija servisa Active Directory za podršku. Za samostalno implementacije, morate poduzeti određene implementaciju da biste zaključali dolje/sigurne implementaciju sustava.

#### <a name="dns-server"></a>DNS poslužitelj
Za Azure AD domene upravljanih domenskih servisa sadrži upravljani DNS servise. Članovi grupe 'AAD Kontroler administratori' možete upravljati DNS-a na upravljanih domeni. Članovi ove grupe ponudit će cijeli DNS administracijske ovlasti za upravljane domenu. Upravljanje DNS-om možete izvršiti pomoću 'konzolu DNS administracije' uključeni u paketu Remote Server Administracija Alati (RSAT).

#### <a name="domain-or-enterprise-administrator-privileges"></a>Ovlasti domene ili Administrator u tvrtki
Ove dodatnim ovlastima se ne nudi programa AAD DS upravljanih domeni. Aplikacije koje zahtijevaju te povećane ovlasti biti instaliran/cilja nije moguće pokrenuti protiv upravljanih domene. Manje podskup administratorskim ovlastima dostupna je za članove grupe za administraciju uz prijenos ovlasti pod nazivom "AAD Kontroler administratori". Ove ovlasti obuhvaćaju ovlasti za konfiguriranje DNS-a, konfiguriranje pravilnika grupe, dobiti administratorske ovlasti na domeni pridruženo itd.

#### <a name="domain-join"></a>Uključivanje domene
Virtualnim strojevima možete se uključiti upravljanih domenu sličnu domeni kako uključiti računala u AD domenu.

#### <a name="domain-authentication-using-ntlm-and-kerberos"></a>Provjera autentičnosti domene pomoću NTLM i Kerberos
Pomoću servisa Azure AD domene možete koristiti tvrtke vjerodajnice za provjeru autentičnosti s upravljanih domenom. Vjerodajnice su se sinkroniziraju s klijentom Azure AD. Sinkronizirane klijenata Azure AD Connect osigurava Azure AD se sinkroniziraju promjene vjerodajnice napravljene na lokalni. S SAMOSTALNO domene postavljanja, morate postaviti odnos pouzdanosti domene s za lokalni račun skupa stabala za korisnike za provjeru autentičnosti pomoću vjerodajnica za tvrtke. Umjesto toga možda morati postaviti AD replikacije da biste bili sigurni da korisničkih lozinki sinkronizirati na virtualnim strojevima kontroler Azure domene.

#### <a name="custom-ou-structure"></a>Prilagođeni OU strukture
Članovi grupe 'AAD Kontroler administratori' mogu stvarati prilagođene organizacijske jedinice unutar upravljanih domene.

#### <a name="schema-extensions"></a>Proširenja shema
Nije moguće proširiti shemi osnovni programa Azure AD domene upravljanih domenskih servisa. Stoga aplikacije koje se temelje na proširenja za AD shema (na primjer, nove atribute u korisničkom objektu) ne može lifted sustavu i pomaknut AAD DS domene.

#### <a name="ad-domain-or-forest-trusts"></a>AD domene ili skup stabala Trusts
Da biste postavili pouzdanost odnosa (dolazni/odlazni) s druge domene ne može konfigurirati upravljane domene. Scenariji kao što su implementacijama skupa stabala resursa ili slučajevi u kojima ne želite sinkronizaciju lozinke Azure AD zbog toga ne možete koristiti Azure AD domenske servise.

#### <a name="ldap-read"></a>Čitanje LDAP
Upravljani domene podržava LDAP čitanje radnih opterećenja. Stoga možete implementirati aplikacije koje izvođenje operacija čitanja LDAP protiv upravljanih domene.

#### <a name="secure-ldap"></a>Sigurna LDAP
Možete konfigurirati Azure AD domenske servise za omogućivanje sigurnog pristupa LDAP upravljanih domene, uključujući putem Interneta.

#### <a name="ldap-write"></a>Pisanje LDAP
Upravljani domena je samo za čitanje za objekte za korisnika. Stoga aplikacije koje izvođenje operacije pisanja LDAP izbaciti atribute objekta korisnik ne rade u upravljanih domene. Uz to, korisničke lozinke nije moguće promijeniti iz unutar upravljanih domene. Drugi primjer bi izmjenu članstva u grupi ili atribute grupu unutar upravljanih domene koji nije dopušten. Međutim, sve promjene korisničke atribute ili lozinke koje ste izvršili u Azure AD (putem portala PowerShell/Azure) ili na lokalnim poslužiteljima AD sinkroniziraju s domenom upravljanih AAD DS.

#### <a name="group-policy"></a>Pravilnik grupe
Konstrukta sofisticirane grupe pravila nisu podržane na domeni upravljanih AAD DS. Na primjer, nije moguće stvaranje i implementacija zasebnom GPO za svaku prilagođenu OU u domeni ili pomoću WMI filtriranja za određivanje GP. Nema ugrađenu GPO svaki za spremnika 'AADDC računala' i "AADDC korisnike", koje možete prilagoditi za konfiguriranje pravilnika grupe.

#### <a name="geo-dispersed-deployments"></a>Implementacija u odnosu na zemlj.
Azure upravljanih domene servisa Active Directory Domain Services dostupne su u jednu virtualne mreže u Azure. Za scenarije koje je potrebno kontrolera domene da bi bio dostupan u više Azure regijama širom svijeta, postavljanju kontrolera Azure IaaS VMs možda je bolji odabir.


## <a name="do-it-yourself-diy-ad-deployment-options"></a>Mogućnosti implementacije "samostalno (DIY) AD
Možda koristite slučajeva za implementaciju koja vam neke mogućnosti koje nudi Windows Server AD instalacije. U tim slučajevima, razmislite o na jedan od sljedećih mogućnosti za samostalno (DIY):

- **Samostalnog oblaka domene:** Možete postaviti samostalnog 'oblaka domena' pomoću Azure virtualnim strojevima podešena kao kontrolera domena. U ovom infrastrukture Integracija s lokalnih AD okruženju. Ova mogućnost je potrebna drugi skup "oblaka vjerodajnica" za prijavu/administriranje VMs u oblaku.

- **Implementaciju skupa stabala resursa:** Možete postaviti domenu u topologiji skupa stabala resursa pomoću Azure virtualnim strojevima konfigurirati kao kontrolera domena. Nakon toga možete konfigurirati odnos pouzdanosti AD s lokalnih AD okruženju. Možete domene spoj računala (Azure VMs) za ovaj skupa stabala resursa u oblaku. Provjera autentičnosti korisnika će se dogoditi iznad bilo VPN-ExpressRoute vezu s lokalnog imenika.

- **Proširivanje lokalne domene za Azure:** Možete se povezati Azure virtualne mreže vaše lokalne mreže pomoću veze s VPN-ExpressRoute tako da se Azure VMs može biti pridruženo lokalnih AD. Drugi druga je da biste povećali razinu replike kontrolera domena lokalne domene u Azure kao na VM. Zatim postavite ga za replikaciju putem veze za VPN-ExpressRoute za lokalnog imenika. Ovaj način implementacije učinkovito prenosi lokalne domene Azure.

> [AZURE.NOTE] Može odrediti da SAMOSTALNO mogućnost je prikladnije za implementaciju koristi-slučajeve. Razmislite o Pridonesite razumjeti što bi pomoći značajke [zajedničkog korištenja povratnih informacija](active-directory-ds-contact-us.md) koje ste odabrali Azure servisa Active Directory Domain Services u budućnosti. U ovom povratne informacije pridonose poslovanja servis da biste bolje odgovara potrebama implementaciju i korištenje slučajeva.

Ne možemo objavljeno [smjernice za implementaciju sustava Windows Server Active Directory virtualnim računalima sustava na Azure](https://msdn.microsoft.com/library/azure/jj156090.aspx) za olakšali SAMOSTALNO instalacije.


## <a name="related-content"></a>Srodni sadržaji
- [Značajke - Azure AD Domain Services](active-directory-ds-features.md)

- [Scenariji za implementaciju - Azure AD domenske servise](active-directory-ds-scenarios.md)

- [Smjernice za implementaciju sustava Windows Server Active Directory na Azure virtualnim strojevima](https://msdn.microsoft.com/library/azure/jj156090.aspx)
