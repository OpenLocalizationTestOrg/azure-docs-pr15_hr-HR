<properties
    pageTitle="Provjera autentičnosti identiteta bez lozinke kroz Microsoft Passport | Microsoft Azure"
    description="Na implementacija Microsoft Passport TechNet sadrži pregled Microsoft Passport i dodatne informacije."
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

# <a name="authenticating-identities-without-passwords-through-microsoft-passport"></a>Provjera autentičnosti identiteta bez lozinke kroz Microsoft Passport

Trenutni načina provjere autentičnosti lozinkama samostalno nisu potrebne da bi korisnici sigurno. Korisnici ponovno korištenje i zaboravite lozinku. U lozinkama se breachable, phishable, podložni pukotina i guessable. Mogu se i zatražite teško je zapamtiti i podložni napada kao što su "[prolaz raspršivanje](https://technet.microsoft.com/dn785092.aspx)".

## <a name="about-microsoft-passport"></a>O Microsoft Passport
Microsoft Passport je privatno/javni ključ ili pristup provjere autentičnosti utemeljene na certifikat za tvrtke ili ustanove i korisnici koji prelazi lozinke. U ovom obrascu provjere autentičnosti ovisi o ključ vjerodajnice pomoću kojih možete zamijeniti lozinke i otporan breaches, thefts i krađa identiteta.

 Microsoft Passport omogućuje korisniku provjeru autentičnosti Microsoftov račun, računa za Windows Server Active Directory, račun za Microsoft Azure Active Directory (Azure AD) ili servisa drugih proizvođača koji podržava provjeru autentičnosti brzo identiteta Online (FIDO). Nakon početnog dva koraka potvrdu tijekom registraciju Microsoft Passport, Microsoft Passport postavljen je na uređaju korisnika, a korisnik postavlja gesta koje mogu biti Windows pozdrav ili PIN-a. Korisnik unese gesta da biste provjerili njihov identitet. Windows zatim koristi Microsoft Passport provjere autentičnosti korisnika i pomoć dopustiti pristup zaštićene resurse i usluge.

Privatni ključ postane dostupna isključivo putem "korisnik gesta" kao PIN-a, biometrija ili udaljenog uređaja kao što su pametne kartice koje korisnik koristi za prijavu na uređaj. Ove informacije je povezana s certifikat ili asymmetrical ključa par. Privatni ključ je hardver attested ako je uređaj na čip pouzdana Platform Module (TPM). Privatni ključ nikad ne ostavlja uređaj.

Javni ključ je registriran za Azure Active Directory i Windows Server Active Directory (za lokalni). Davatelji identiteta (IDPs) provjeru korisnika mapiranjem javni ključ korisnika na privatni ključ i unesite podatke prijavu putem jednog lozinku vrijeme (OTP), PhoneFactor ili mehanizam za različite obavijesti.

## <a name="why-enterprises-should-adopt-microsoft-passport"></a>Zašto korporacije treba prihvaćaju Microsoft Passport

Omogućivanjem Microsoft Passport korporacije možete zaštititi svoje resursi čak i tako da:

* Postavljanje Microsoft Passport mogućnošću hardver Preferirani. To znači da tipke bit će načinjena TPM 1.2 ili TPM 2.0 kada su dostupni. Kada TPM nije dostupan, softver generirat će tipku.

* Definiranje složenosti i duljinu PIN, i korištenje pozdrav je li omogućeno u tvrtki ili ustanovi.

* Konfiguriranje Microsoft Passport za podržava pametna kartica nalik scenarije pomoću certifikat utemeljen pouzdanost.

## <a name="how-microsoft-passport-works"></a>Kako funkcionira Microsoft Passport
1. Strelicama generira hardverske TPM ili softvera. Proširi imaju ugrađene TPM čip koji secures hardver po integriranje šifriranja tipke u uređaja. TPM 1.2 ili TPM 2.0 generira tipke ili certifikate koji se stvaraju iz generirani tipke.

2. TPM attests ključeve hardver granica.

3. Gesta za jednu otključaj ćete otključati uređaj. Gesta omogućuje pristup višestrukih resursa ako je uređaj domene pridruženo ili Azure AD pridružili.

## <a name="how-the-microsoft-passport-lifecycle-works"></a>Kako funkcionira Microsoft Passport životni ciklus

![Microsoft Passport životni ciklus](./media/active-directory-azureadjoin/active-directory-azureadjoin-microsoft-passport.png)

Prethodni dijagram pokazuje par ključa privatno/javno i provjere valjanosti od davatelja identiteta. Svaki od ovih koraka objašnjeni detaljno ovdje:

1. Korisnik dokazuje svoj identitet kroz više ugrađene načina za jezičnu provjeru (geste, fizičke pametne kartice, višestruka provjera autentičnosti) i šalje te podatke da biste je identiteta davatelja (IDP) kao što je Azure Active Directory ili lokalnog servisa Active Directory.

2. Uređaj pa stvara tipku, attests tipku, vodi javno dio ovog ključa, pridružuje pomoću naredbe stanice, prijavi i šalje IDP da biste registrirali tipku.

4. Čim se na IDP registrira dijelu javni ključ, u IDP challenges uređaj da biste se odjavili s dijelu privatni ključ.

5. Na IDP zatim Provjeri valjanost i problemi token provjere autentičnosti koja omogućuje korisnik i pristup putem uređaja zaštićeni resursi. IDPs možete napisati različite platforme aplikacije ili pomoću podrške za preglednike (putem JavaScript/Webcrypto API-ji) možete stvarati i koristiti Microsoft Passport vjerodajnice za svoje korisnike.

## <a name="the-deployment-requirements-for-microsoft-passport"></a>Preduvjeti za implementaciju za Microsoft Passport
### <a name="at-the-enterprise-level"></a>Na razini tvrtke

* Tvrtki ima Azure pretplate.

### <a name="at-the-user-level"></a>Na razini korisnika

* Korisnikovo računalo pokreće Windows 10 Professional ili Enterprise.

Detaljne upute, potražite u članku [Omogućivanje Microsoft Passport za rad u tvrtki ili ustanovi](active-directory-azureadjoin-passport-deployment.md).


## <a name="additional-information"></a>Dodatne informacije

* [Windows 10 za tvrtke: načina korištenja uređaji za posao](active-directory-azureadjoin-windows10-devices-overview.md)
* [Proširivanje mogućnosti oblak na uređajima Windows 10 do Azure Active Directory uključivanje](active-directory-azureadjoin-user-upgrade.md)
* [Saznajte više o korištenju scenariji za uključivanje za Azure AD](active-directory-azureadjoin-deployment-aadjoindirect.md)
* [Povezivanje domene pridruženo uređaji Azure AD za Windows 10 sučelja](active-directory-azureadjoin-devices-group-policy.md)
* [Postavljanje uključivanje za Azure AD](active-directory-azureadjoin-setup.md)
