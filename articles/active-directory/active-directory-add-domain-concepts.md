<properties
    pageTitle="Konceptualni pregled prilagođenih naziva domena u servisu Azure Active Directory | Microsoft Azure"
    description="U članku se objašnjava konceptualni framework za korištenje prilagođenih naziva domena servisa Azure Active directory, uključujući vanjski pristup za jedinstvenu prijavu"
    services="active-directory"
    documentationCenter=""
    authors="jeffsta"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/04/2016"
    ms.author="curtand;jeffsta"/>

# <a name="conceptual-overview-of-custom-domain-names-in-azure-active-directory"></a>Konceptualni pregled prilagođenih naziva domena u servisu Azure Active Directory

Naziv domene je važan dio identifikator za mnoge direktorija resurse: je dio korisničko ime ili e-pošte adrese za korisnika, dio adrese za grupu, i može biti dio aplikacije ID URI za aplikaciju. Resurs u Azure Active Directory (Azure AD) možete uključiti naziv domene koji je već potvrđena da biste se posjeduje direktorij koji sadrži resurs. Samo globalni administrator mogu obavljati zadatke upravljanja domene u Azure AD.

Globalno jedinstvenih nazivima domena u Azure AD. Jedan Azure AD mogu koristiti naziv domene. Ako jedan Azure AD direktorija potvrdio naziv domene, bez Azure AD imenik će možete potvrda ili koristiti taj isti naziv domene.

## <a name="initial-and-custom-domain-names"></a>Nazivi početni i prilagođene domene

Svaki naziv domene u Azure AD je na početni naziv domene ili prilagođeni naziv domene.

Svaki Azure AD isporučuje se s nazivom početnu domenu u contoso.onmicrosoft.com obrasca. Ovaj treći naziv domene razine, u ovom primjeru "contoso.onmicrosoft.com," uspostavljena kada imenika stvorilo, obično administrator koji je stvorio imenika. Početni naziv domene za direktorij ne mogu promijeniti ni izbrisati. Početni naziv domene, dok je potpuno funkcionalni namijenjen prvenstveno će se koristiti kao pokretačkog mehanizam dok je potvrđena prilagođenog naziva domene.

U većini radnog okruženja, direktorij sadrži najmanje jedan provjereno prilagođenu domenu, kao što su "contoso.com,", a zatim je tu prilagođenu domenu koja je vidljiva krajnjim korisnicima. Prilagođeni naziv domene je naziv domene koji posjeduje i koristi taj tvrtka ili ustanova, kao što su "contoso.com," za korisnike, kao što su hostiranja web-mjesta. Ovaj naziv domene je poznato zaposlenicima jer je dio korisničkog imena koje se koriste za prijavu na mreži tvrtke ili za slanje i dohvat e-pošte.

Prije nego što se može koristiti tako da Azure AD, naziv prilagođene domene mora biti dodan u imenik i provjeriti.

## <a name="verified-and-unverified-domain-names"></a>Nazivi domena provjerene i Neprovjereni

Početni naziv domene za direktorij implicitno vrednuje kao provjerio Azure AD. Kad administrator dodaje prilagođenog naziva domene za Azure AD, je prethodno u stanju Neprovjereni. Azure AD neće dopustiti nikakve direktorija resurse da biste koristili Neprovjereni domenu. Na taj način da samo jedan direktorija naziv za određene domene možete koristiti i da se tvrtka ili ustanova koristi naziv domene je doista vlasnik taj naziv domene.

Azure AD potvrđuje vlasništvo nad naziv domene traženjem određene stavke u zone datoteka domene servisa (DNS) naziv za naziv domene. Da biste provjerili vlasništvo nad naziv domene, administrator može vidjeti stavku DNS iz Azure AD će izgledati Azure AD i dodaje tu stavku datoteka zone DNS-a za naziv domene. Datoteka zone DNS-a sastoji se od registrara naziva domena za tu domenu. U članku za [Dodavanje prilagođene domene u imenik Azure AD](active-directory-add-domain.md)prikazane su korake da biste potvrdili domenu.

Dodavanje stavke na DNS datoteku zone za naziv domene utječe na druge domenske servise kao što su e-pošte ili hostiranje web.

## <a name="federated-and-managed-domain-names"></a>Nazivi domena pridruženim i upravljani

Prilagođeni naziv domene u Azure AD moguće je konfigurirati da bi korisnicima omogućili vanjsko znak u sučelje između lokalnog imeničkog servisa Active Directory i Azure AD. Konfiguriranje domena za vanjski pristup zahtijeva ažuriranja povlaštene resursima servisa Azure AD i sustava Windows Server Active Directory. Konfiguriranje pridruženoj domeni morate izvršiti iz Azure AD Connect ili pomoću komponente PowerShell. Na portalu Azure klasični ne može započeti združivanju s prilagođenom domenom. [Pogledajte ovaj videozapis da biste saznali o konfiguriranju AD FS za korisnik, prijavite se Azure AD Connect](http://channel9.msdn.com/Series/Azure-Active-Directory-Videos-Demos/Configuring-AD-FS-for-user-sign-in-with-Azure-AD-Connect).

Domene koje su pridružene ponekad se nazivaju upravljanog domene. Na početnu domenu za u imeniku Azure AD implicitno vrednuje kao upravljanih domene.

## <a name="primary-domain-names"></a>Naziva primarne domene

Naziv primarne domene za direktorij je naziv domene koji je unaprijed odabran kao zadanu vrijednost za 'domena' dio korisničko ime kada administrator stvara novi korisnik u [Azure klasični portal](https://manage.windowsazure.com/) ili drugog portala kao što su portala za administratore sustava Office 365. Direktorij može imati samo jedan naziv primarne domene. Administrator može promijeniti naziv primarne domene se neki provjeriti prilagođene domene nije naveden kao pridruženi, ili na početnu domenu.

## <a name="domain-names-in-azure-ad-and-other-microsoft-online-services"></a>Nazivi domena u Azure AD i drugi Microsoft Online Services

Naziv domene mora se provjeriti u Azure AD prije nego što se može koristiti tako da drugi Microsoftov mrežni servis kao što je Exchange Online, sustava SharePoint Online i Intune. Ove usluge obično potrebno administrator da biste dodali jednu ili više stavki DNS-a koji su određeni servis.

Azure web-aplikaciju programa koristi vlastiti mehanizam potvrđuje vlasništvo nad domenom. Domene mora se provjeriti za Azure AD čak i ako je prethodno provjerila za korištenje aplikacije Azure web pretplate na koji se zasniva na tom Azure AD. Azure web-aplikaciju programa možete koristiti naziv domene koji potvrdio u neki drugi direktorij iz imenika secures web-aplikaciji.

## <a name="managing-domain-names"></a>Upravljanje nazivima domena

Zadatke upravljanja domene možete obaviti iz Azure klasični portal i PowerShell. Mnoge zadatke možete izvršiti pomoću Azure AD grafikonu API-JA (u javnom pretpregled).

-   [Dodavanje i provjera prilagođenog naziva domene](active-directory-add-domain.md)

-   [Upravljanje domenama na portalu za Azure klasični](active-directory-add-manage-domain-names.md)

-   [Upravljanje nazivima domena u Azure AD pomoću komponente PowerShell](https://msdn.microsoft.com/library/azure/e1ef403f-3347-4409-8f46-d72dafa116e0#BKMK_ManageDomains)

-   [Upravljanje nazivima domena u Azure AD pomoću API Azure AD grafikonu](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/domains-operations)
