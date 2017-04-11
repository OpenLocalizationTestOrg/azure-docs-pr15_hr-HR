<properties
    pageTitle="Azure AD Connect više domena"
    description="U ovom dokumentu opisuje postavljanju i konfiguriranju više domena najviše razine s O365 i Azure AD."
    services="active-directory"
    documentationCenter=""
    authors="billmath"
    manager="femila"
    editor="curtand"/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/08/2016"
    ms.author="billmath"/>

# <a name="multiple-domain-support-for-federating-with-azure-ad"></a>Podrška za više domena za združivanju s s Azure AD
Sljedeća dokumentacija sadrži upute o tome kako koristiti više domena najviše razine i poddomene kad združivanju s s web-mjesto sustava Office 365 ili u okvir za Azure AD domene.

## <a name="multiple-top-level-domain-support"></a>Podrška za više domena najviše razine
Združivanju s više, domena najviše razine s Azure AD zahtijeva neke dodatne konfiguracijske koja nije potrebna kada združivanju s s jednog domena najviše razine.

Kada je domene pridružene s Azure AD, nekoliko svojstava postavljene su na domeni u Azure.  Jedan važno je IssuerUri.  Ovo je URI koji koriste Azure AD za prepoznavanje domene koji je pridružen token.  URI nije potrebno raščlanjuje bilo što ali mora biti valjane URI.  Prema zadanim postavkama Azure AD postavlja to vrijednost identifikator usluge vanjski pristup vašem lokalnom sustavu AD FS konfiguracije.

>[AZURE.NOTE]Identifikator usluge vanjski pristup je URI koji služi kao jedinstvena identifikacija servis za vanjski pristup.  Servis za vanjski pristup je instanca servisa AD FS koje služi kao servis sigurnosnih tokena. 

Možete prikazati IssuerUri pomoću naredbe ljuske PowerShell `Get-MsolDomainFederationSettings - DomainName <your domain>`.

![Get-MsolDomainFederationSettings](./media/active-directory-multiple-domains/MsolDomainFederationSettings.png)

Problem nastaje kada želite dodati više domena najviše razine.  Na primjer, recimo da imate postavljanje vanjskog pristupa između Azure AD i lokalnog okruženja.  Za taj dokument koristim bmcontoso.com.  Sada koje su dodane domenu drugu, najviše razine, bmfabrikam.com.

![Domena](./media/active-directory-multiple-domains/domains.png)

Kada pokušaju možemo pretvoriti naše bmfabrikam.com domene da biste se vanjskog, ne možemo primiti pogrešku.  Razlog za to je Azure AD sadrži constraint koji omogućuju svojstvo IssuerUri imati iste vrijednosti za više od jedne domene.  
  

![Vanjski pristup pogreške](./media/active-directory-multiple-domains/error.png)

### <a name="supportmultipledomain-parameter"></a>Parametar SupportMultipleDomain

Da biste zaobilazno rješenje, moramo da biste dodali drugu IssuerUri koje možete izvršiti pomoću na `-SupportMultipleDomain` parametar.  Taj parametar koristi se sa sljedećim cmdletima:
    
- `New-MsolFederatedDomain`
- `Convert-MsolDomaintoFederated`
- `Update-MsolFederatedDomain`

Ovaj parametar čini Azure AD konfigurirati na IssuerUri tako da se temelji na naziv domene.  To će biti jedinstveni preko direktorija Azure AD.  Parametar omogućuje naredbu PowerShell da biste uspješno dovršena.

![Vanjski pristup pogreške](./media/active-directory-multiple-domains/convert.png)

Pogledate postavke naš novu domenu bmfabrikam.com koje možete vidjeti sljedeće:

![Vanjski pristup pogreške](./media/active-directory-multiple-domains/settings.png)

Imajte na umu da `-SupportMultipleDomain` promjena drugih krajnjih točaka koji i dalje konfiguriran tako da pokazuje na našem vanjski pristup servisu na adfs.bmcontoso.com.

Druge stvari koje `-SupportMultipleDomain` ne je da ga osigurava da sustava AD FS sadrži odgovarajuću vrijednost izdavač tokeni izdavanja za Azure AD. To čini poduzimanja domene dio korisnici UPN-a i to postavljanjem kao domena u IssuerUri, odnosno https://{upn sufiks} / services/adfs/pouzdanost. 

Stoga tijekom provjere autentičnosti za Azure AD ili Office 365, IssuerUri element u korisnikovu token se koristi da biste pronašli domene u Azure AD.  Ako rezultat nije moguće pronaći provjeru autentičnosti neće uspjeti. 

Na primjer, ako korisnikovo UPN je bsimon@bmcontoso.com, IssuerUri element u probleme token AD FS će biti postavljena na http://bmcontoso.com/adfs/services/trust. To odgovara konfiguraciji Azure AD i provjeru autentičnosti neće uspjeti.

Slijedi pravila za prilagođene zahtjeva koji implementira ovaj logika:

    c:[Type == "http://schemas.xmlsoap.org/claims/UPN"] => issue(Type =   "http://schemas.microsoft.com/ws/2008/06/identity/claims/issuerid", Value = regexreplace(c.Value, ".+@(?<domain>.+)", "http://${domain}/adfs/services/trust/"));


>[AZURE.IMPORTANT]Da biste mogli koristiti parametar - SupportMultipleDomain prilikom pokušaja da biste dodali novi ili pretvaranje već dodavanja domene, morate koristiti postavljanje vanjskog pouzdanosti ih podržava izvorno.  


## <a name="how-to-update-the-trust-between-ad-fs-and-azure-ad"></a>Kako ažurirati pouzdanosti između AD FS i Azure AD
Ako niste nije postavljen pridruženim pouzdanosti između AD FS i instanci programa Azure AD, možda ćete morati ponovno stvoriti ovu pouzdanost.  Razlog je tome što kada je izvorno postavljanje bez u `-SupportMultipleDomain` , u IssuerUri je parametar postavljen sa zadanom vrijednošću.  Snimka ispod koje možete vidjeti na IssuerUri postavljen na https://adfs.bmcontoso.com/adfs/services/trust.

Stoga now, ako ne možemo na portalu za Azure AD uspješno dodati novu domenu, a zatim pokušati ga pretvoriti `Convert-MsolDomaintoFederated -DomainName <your domain>`, ne možemo se sljedeća pogreška.

![Vanjski pristup pogreške](./media/active-directory-multiple-domains/trust1.png)

Ako pokušate dodati u `-SupportMultipleDomain` promjenu smo će primiti sljedeću pogrešku:

![Vanjski pristup pogreške](./media/active-directory-multiple-domains/trust2.png)

Jednostavno pokušaja pokretanja `Update-MsolFederatedDomain -DomainName <your domain> -SupportMultipleDomain` na izvornu domenu i uzrokovat će pogrešku.

![Vanjski pristup pogreške](./media/active-directory-multiple-domains/trust3.png)

Slijedeći korake u nastavku da biste dodali dodatne domena najviše razine.  Ako ste već dodali domenu, a niste koristili u `-SupportMultipleDomain` parametar počinju upute za uklanjanje i ažuriranje izvornu domenu.  Ako niste dodali domenu najviše razine još možete početi s korake za dodavanje domene pomoću komponente PowerShell Azure AD Connect.

Poduzmite sljedeće korake da biste uklonili pouzdanost za Microsoft Online i ažurirati izvornu domenu.

2.  Na poslužitelj vanjski pristup AD FS Otvori **upravljanja AD FS.** 
2.  Na lijevoj strani proširite **Pouzdanost odnose** i **Potrebe za oslanjanjem smatra pouzdanima strana**
3.  Na desnoj strani izbrisati stavku **Platformu identiteta za Microsoft Office 365** .
![Uklanjanje Microsoft Online](./media/active-directory-multiple-domains/trust4.png)
1.  Na računalu koje je na njemu instalirano [Azure Active Directory Module za Windows PowerShell](https://msdn.microsoft.com/library/azure/jj151815.aspx) pokrenite na sljedeći način: `$cred=Get-Credential`.  
2.  Unesite korisničko ime i lozinku za globalni administrator za domenu Azure AD združivanju s s
2.  U ljusci PowerShell unesite`Connect-MsolService -Credential $cred`
4.  U ljusci PowerShell unesite `Update-MSOLFederatedDomain -DomainName <Federated Domain Name> -SupportMultipleDomain`.  To je izvorna domene.  Pomoću gore domene bi:`Update-MsolFederatedDomain -DomainName bmcontoso.com -SupportMultipleDomain`


Poduzmite sljedeće korake da biste dodali novi domena najviše razine pomoću komponente PowerShell

1.  Na računalu koje je na njemu instalirano [Azure Active Directory Module za Windows PowerShell](https://msdn.microsoft.com/library/azure/jj151815.aspx) pokrenite na sljedeći način: `$cred=Get-Credential`.  
2.  Unesite korisničko ime i lozinku za globalni administrator za domenu Azure AD združivanju s s
2.  U ljusci PowerShell unesite`Connect-MsolService -Credential $cred`
3.  U ljusci PowerShell unesite`New-MsolFederatedDomain –SupportMultipleDomain –DomainName`

Poduzmite sljedeće korake da biste dodali novi domena najviše razine pomoću Azure AD Connect.

1.  Pokretanje Azure AD Connect s radne površine ili izbornik start
2.  Odaberite "Dodaj dodatnu domenu Azure AD" ![dodavanje dodatnih Azure AD domene](./media/active-directory-multiple-domains/add1.png)
3.  Unesite Azure AD i vjerodajnice za Active Directory
4.  Odaberite drugu domenu koju želite konfigurirati za vanjski pristup.
![Dodavanje dodatnih Azure AD domene](./media/active-directory-multiple-domains/add2.png)
5.  Kliknite Instaliraj


### <a name="verify-the-new-top-level-domain"></a>Provjerite je li nova domena najviše razine
Pomoću naredbe ljuske PowerShell `Get-MsolDomainFederationSettings - DomainName <your domain>`možete pogledati ažurirane IssuerUri.  Snimka zaslona koja se nalazi ispod prikazuje na vanjski pristup postavke su ažurirani na našem izvorne http://bmcontoso.com/adfs/services/trust domene

![Get-MsolDomainFederationSettings](./media/active-directory-multiple-domains/MsolDomainFederationSettings.png)

I IssuerUri na našem novu domenu postavljen je na https://bmfabrikam.com/adfs/services/trust

![Get-MsolDomainFederationSettings](./media/active-directory-multiple-domains/settings2.png)


##<a name="support-for-sub-domains"></a>Podrška za poddomene
Kada dodate poddomenu, zbog način Azure AD rukuje domena, će nasljeđuju postavke od nadređenog.  To znači da se IssuerUri mora se podudarati s roditeljima.

Tako da omogućuje recimo, primjerice da ste bmcontoso.com i dodavanje corp.bmcontoso.com.  To znači da će se IssuerUri za korisnika iz corp.bmcontoso.com moraju biti **http://bmcontoso.com/adfs/services/trust.**  No standardne pravilo implementirana iznad za Azure AD generirat će token s je izdavač kao **http://corp.bmcontoso.com/adfs/services/trust.** koji ne odgovaraju domenu obavezne vrijednost, a zatim provjera autentičnosti neće uspjeti.

### <a name="how-to-enable-support-for-sub-domains"></a>Kako omogućiti podršku za poddomene
Da bi se to može zaobići AD FS potrebe za oslanjanjem strana pouzdanost za Microsoft Online mora se ažurirati.  Da biste to učinili, morate konfigurirati pravila za prilagođene zahtjeva tako da je pruge isključiti sve poddomene iz korisnikove UPN nastavka tijekom izgradnje prilagođene vrijednosti izdavač. 

Sljedeće zahtjeva učinite sljedeće:

    c:[Type == "http://schemas.xmlsoap.org/claims/UPN"] => issue(Type = "http://schemas.microsoft.com/ws/2008/06/identity/claims/issuerid", Value = regexreplace(c.Value, "^((.*)([.|@]))?(?<domain>[^.]*[.].*)$", "http://${domain}/adfs/services/trust/"));

Poduzmite sljedeće korake da biste dodali prilagođenu zahtjeva za podršku poddomene.

1.  Upravljanje Otvori AD FS
2.  Desnom tipkom miša kliknite Microsoft Online to pouzdanost, a zatim odaberite Uređivanje zahtjeva pravila
3.  Odaberite treće pravilo zahtjeva i zamijeni ![zahtjeva za uređivanje](./media/active-directory-multiple-domains/sub1.png)
4.  Zamijenite trenutnu zahtjev:
    
        c:[Type == "http://schemas.xmlsoap.org/claims/UPN"] => issue(Type = "http://schemas.microsoft.com/ws/2008/06/identity/claims/issuerid", Value = regexreplace(c.Value, ".+@(?<domain>.+)","http://${domain}/adfs/services/trust/"));
        
    s
    
        `c:[Type == "http://schemas.xmlsoap.org/claims/UPN"] => issue(Type = "http://schemas.microsoft.com/ws/2008/06/identity/claims/issuerid", Value = regexreplace(c.Value, "^((.*)([.|@]))?(?<domain>[^.]*[.].*)$", "http://${domain}/adfs/services/trust/"));`
    
![Zamjena zahtjeva](./media/active-directory-multiple-domains/sub2.png)
5.  Kliknite u redu.  Kliknite Primijeni.  Kliknite u redu.  Zatvorite upravljanje AD FS.

