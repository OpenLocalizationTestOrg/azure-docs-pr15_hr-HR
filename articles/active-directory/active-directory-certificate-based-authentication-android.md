<properties 
    pageTitle="Početak rada s provjerom autentičnosti certifikat koji se temelji na sustavu Android | Microsoft Azure" 
    description="Upute za konfiguriranje provjere autentičnosti certifikat koji se temelji na rješenja uz uređaje sa sustavom Android" 
    services="active-directory" 
    authors="markusvi"  
    documentationCenter="na" 
    manager="femila"/>
<tags 
    ms.service="active-directory" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.tgt_pltfrm="na" 
    ms.workload="identity" 
    ms.date="10/10/2016" 
    ms.author="markvi" />



# <a name="get-started-with-certificate-based-authentication-on-android---public-preview"></a>Početak rada s provjerom autentičnosti certifikat koji se temelji na sustavu Android – javno pretpregled  

> [AZURE.SELECTOR]
- [iOS](active-directory-certificate-based-authentication-ios.md)
- [Android](active-directory-certificate-based-authentication-android.md)


U ovoj se temi objašnjava konfiguriranje i korištenje certifikat prema provjere autentičnosti (CBA) na uređaju sa sustavom Android za korisnike od drugih korisnika u tarifama za Office 365 Enterprise, Business i obrazovne ustanove. 

CBA omogućuje moguće provjeriti autentičnost po Azure Active Directory pomoću certifikata klijenta na uređaju sa sustavom Android ili iOS prilikom povezivanja svoj račun sustava Exchange online da biste: 

- Mobilne aplikacije sustava Office kao što je Microsoft Outlook i Microsoft Word   
- Klijenti za Exchange ActiveSync (EAS) 

Konfiguriranje značajka se uklanja morati unijeti kombinaciju korisničko ime i lozinku u određene mail i aplikacijama Microsoft Office na mobilnom uređaju. 
 

## <a name="supported-scenarios-and-requirements"></a>Podržani scenariji i preduvjeti  



### <a name="general-requirements"></a>Općeniti preduvjeti 


Za sve scenarije u ovoj temi, potrebni su sljedeće zadatke:  

- Pristup authority(s) certifikat za izdavanje certifikata klijent.  

- Certifikati authority(s) mora biti konfigurirano u servisu Azure Active Directory. Možete pronaći detaljne upute o tome da biste dovršili konfiguraciju u odjeljku [Početak rada](#getting-started) .  

- Korijenska ustanova za izdavanje potvrda i sve ustanovama za izdavanje certifikata Srednja mora biti konfigurirano u servisu Azure Active Directory.  

- Svaki ustanove za izdavanje certifikata, morate imati popisi opozvanih certifikata (CRL-ove) koji se pozivati putem Interneta nasuprotne URL-a.  

- Klijentski certifikat mora izdati za provjeru autentičnosti klijenta.  


- Za Exchange ActiveSync klijente samo klijentski certifikat mora imati korisničke adrese e-pošte usmjeravati u sustavu Exchange online Glavno ime ili vrijednost RFC822 naziv polja Naziv zamjenski predmet. Azure Active Directory mapira RFC822 vrijednost atributa Proxy adresom u direktoriju.  



### <a name="office-mobile-applications-support"></a>Podrška za mobilne aplikacije za Office 

| Aplikacija                      | Podrška      |
| ---                       | ---          |
| Word / u programu Excel i PowerPoint | ![Provjera][1]  |
| OneNote                   | dolazim brzo  |
| OneDrive                  | ![Provjera][1]  |
| Outlook                   | ![Provjera][1]  |
| Na servisu Yammer                    | ![Provjera][1]  |
| Skype za tvrtke        | ![Provjera][1]  |


### <a name="requirements"></a>Preduvjeti  

Mora biti verzija uređaj s operacijskim Sustavom Android 5.0 (Lollipop) i veće. 

Mora biti konfiguriran na vanjskom poslužitelju.  


ADFS token Azure Active Directory da biste opozvali potvrdu klijenta mora imati zahtjevima za sljedeće:  

  - `http://schemas.microsoft.com/ws/2008/06/identity/claims/<serialnumber>`  
(Serijski broj certifikat klijenta) 

  - `http://schemas.microsoft.com/2012/12/certificatecontext/field/<issuer>`  
(Niz za izdavatelj certifikata klijenta) 

Azure Active Directory dodaje te zahtjevima za osvježavanje token ako su dostupne u ADFS token (ili SAML token). Kad token osvježavanja treba provjeriti, ti podaci se koristi da biste provjerili opoziv. 

Kao preporučenim načinom rada, trebali biste ažurirati stranica s pogreškom ADFS s uputama o tome kako doći do potvrda korisnika. 

Dodatne informacije potražite u članku [Prilagodba AD FS prijavu stranica](https://technet.microsoft.com/library/dn280950.aspx).  



### <a name="exchange-activesync-clients-support"></a>Podrška za klijente sustava Exchange ActiveSync 


Podržane su određene aplikacije za Exchange ActiveSync na Android 5.0 (Lollipop) ili noviji. Da biste utvrdili ako aplikacija e-pošte podržava tu značajku, obratite se vaš razvojni inženjer. 



## <a name="getting-started"></a>Početak rada 


Da bismo započeli, morate konfigurirati ustanovama za izdavanje certifikata u servisu Azure Active Directory. Za svaki ustanove za izdavanje certifikata, prenesite sljedeće: 

- Javni dio certifikat u obliku *.cer* 

- Internet nasuprotne URL-ovi gdje nalaze se popisi opozvanih certifikata (CRL-ovi)
 

U nastavku je sheme za izdavanje certifikata: 

    class TrustedCAsForPasswordlessAuth 
    { 
       CertificateAuthorityInformation[] certificateAuthorities;    
    } 

    class CertificateAuthorityInformation 

    { 
        CertAuthorityType authorityType; 
        X509Certificate trustedCertificate; 
        string crlDistributionPoint; 
        string deltaCrlDistributionPoint; 
        string trustedIssuer; 
        string trustedIssuerSKI; 
    }                

    enum CertAuthorityType 
    { 
        RootAuthority = 0, 
        IntermediateAuthority = 1 
    } 


Da biste prenijeli podatke, možete koristiti modul Azure AD putem komponente Windows PowerShell.  
Slijede primjeri za dodavanje, uklanjanje ili izmjena ustanove za izdavanje certifikata. 



### <a name="configuring-your-azure-ad-tenant-for-certificate-based-authentication"></a>Konfiguriranje klijenta sustava Azure AD za potvrdu provjeru autentičnosti utemeljenu na 

1. Pokrenite Windows PowerShell s administratorskim ovlastima. 

2. Instalirajte modul Azure AD. Morate instalirati verziju [1.1.143.0](http://www.powershellgallery.com/packages/AzureADPreview/1.1.143.0) ili noviji.  

        Install-Module -Name AzureADPreview –RequiredVersion 1.1.143.0 

3. Povezivanje s ciljnom klijentu: 

        Connect-AzureAD 

### <a name="adding-a-new-certificate-authority"></a>Dodavanje novog ustanova za izdavanje certifikata

1. Postavljanje svojstava za različite od ustanove za izdavanje certifikata i dodajte ga Azure Active Directory: 

        $cert=Get-Content -Encoding byte "[LOCATION OF THE CER FILE]" 
        $new_ca=New-Object -TypeName Microsoft.Open.AzureAD.Model.CertificateAuthorityInformation 
        $new_ca.AuthorityType=0 
        $new_ca.TrustedCertificate=$cert 
        New-AzureADTrustedCertificateAuthority -CertificateAuthorityInformation $new_ca 

5. Pojavljuje se ustanovama za izdavanje certifikata: 

        Get-AzureADTrustedCertificateAuthority 


### <a name="retrieving-the-list-certificate-authorities"></a>Dohvaćanje popisa ustanovama za izdavanje certifikata

Dohvaćanje ustanovama za izdavanje certifikata koji je trenutno spremljene u Azure Active Directory za vaš klijent: 

        Get-AzureADTrustedCertificateAuthority 


### <a name="removing-a-certificate-authority"></a>Uklanjanje ustanove za izdavanje certifikata

1.  Dohvaćanje ustanovama za izdavanje certifikata: 

        $c=Get-AzureADTrustedCertificateAuthority 


2. Uklanjanje certifikata za izdavanje potvrda: 

        Remove-AzureADTrustedCertificateAuthority -CertificateAuthorityInformation $c[2] 


### <a name="modfiying-a-certificate-authority"></a>Modfiying ustanove za izdavanje certifikata 

1.  Dohvaćanje ustanovama za izdavanje certifikata: 

        $c=Get-AzureADTrustedCertificateAuthority 


2. Izmijenite svojstva na ustanove za izdavanje certifikata: 

        $c[0].AuthorityType=1 

3. Postavite **ustanove za izdavanje certifikata**: 

        Set-AzureADTrustedCertificateAuthority -CertificateAuthorityInformation $c[0] 




## <a name="testing-office-mobile-applications"></a>Testiranje mobilne aplikacije sustava Office  

Da biste testirali provjeru autentičnosti certifikat na svoje mobilne aplikacije sustava Office: 

1.  Na uređaju test instalirajte mobilnih aplikacija sustava Office (npr. OneDrive) iz trgovine Google Play.

2.  Provjerite je li korisnički certifikat sadrži dodijeljeni resursi za testiranje uređaj. 

3.  Pokrenite aplikaciju. 

4.  Unesite svoje korisničko ime, a zatim odaberite korisnički certifikat koji želite koristiti. 

Mora biti uspješno prijavili ste. 





## <a name="testing-exchange-activesync-client-applications"></a>Testiranje klijentskim aplikacijama sustava Exchange ActiveSync

Da biste pristupili Exchange ActiveSync putem certifikat prema provjeru autentičnosti, profil programa EAS koja sadrži certifikat klijenta mora biti čini dostupnima aplikaciji. EAS profil mora sadržavati sljedeće informacije:

- Potvrda korisnika će se koristiti za provjeru autentičnosti 

- Krajnja točka EAS mora biti outlook.office365.com (kao što je značajka trenutno je podržano samo u Exchange online više klijentu okruženju)

Profil programa EAS možete konfigurirati i smjestiti na uređaj kroz Upotreba MDM u kao što je Intune ili ručno postavite potvrdu u profilu EAS na uređaju.  


### <a name="testing-eas-client-applications-on-android"></a>Testiranje EAS klijentske aplikacije na sustavu Android 

Da biste testirali provjera autentičnosti potvrda s aplikaciju na Android 5.0 (Lollipop) ili noviji, poduzmite korake u nastavku:  

1. Konfiguriranje profila programa EAS u aplikaciji koja zadovoljava preduvjete iznad.  


2. Kada profil pravilno konfigurirani, otvorite željenu aplikaciju pa provjerite je li sinkroniziranje pošte. 




## <a name="revocation"></a>Opoziv

Da biste opozvali potvrdu klijenta, Azure Active Directory dohvaćanja popisi opozvanih certifikata (CRL-ove) iz URL-ova prenijeli kao dio informacije za izdavanje certifikata i predmemorira ga. Objavljivanje posljednji da biste bili sigurni da je još uvijek valjan CRL-a koristi se vremenska oznaka (**Valjano** svojstvo) u CRL-a. Da biste opozvali pristup certifikate koji su dio popisa povremeno poziva CRL-a.

Ako je više izravne opoziva obavezno (na primjer, ako se korisnik prekine uređaj), ovlaštenja korisnika možete nevažeći. Polje **StsRefreshTokenValidFrom** mora biti postavljeno za određenog korisnika pomoću komponente Windows PowerShell za onemogućavanje token za autorizaciju. Morate ažurirati polje **StsRefreshTokenValidFrom** za svakog korisnika želite opozvati pristup.
 
Da biste bili sigurni da opoziv ne riješi, morate postaviti **Valjano** CRL-a na datum nakon vrijednost odredio **StsRefreshTokenValidFrom** i provjerite je li certifikat u pitanju je CRL-a.
 
Sljedeći koraci strukture postupak ažuriranja i poništavanja token autorizacije postavljanjem **StsRefreshTokenValidFrom** polja. 

1. Povezivanje s administratorske vjerodajnice sa servisom MSOL: 

        $msolcred = get-credential 
        connect-msolservice -credential $msolcred 

1.  Dohvaćanje trenutnu vrijednost StsRefreshTokensValidFrom za korisnike: 

        $user = Get-MsolUser -UserPrincipalName test@yourdomain.com` 
        $user.StsRefreshTokensValidFrom 


1.  Konfiguriranje novu StsRefreshTokensValidFrom vrijednost za korisnika jednako trenutnu vremensku oznaku: 

        Set-MsolUser -UserPrincipalName test@yourdomain.com -StsRefreshTokensValidFrom ("03/05/2016")


Datum koji ste postavili mora biti u budućnosti. Ako datum nije u budućnosti, svojstvo **StsRefreshTokensValidFrom** nije postavljen. Ako je datum u budućnosti, **StsRefreshTokensValidFrom** postavljen je na trenutno vrijeme (ne datum označena naredba skup-MsolUser). 


<!--Image references-->
[1]: ./media/active-directory-certificate-based-authentication-android/ic195031.png