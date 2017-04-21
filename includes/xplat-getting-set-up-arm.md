<properties services="virtual-machines" title="Using Azure CLI with Azure Resource Manager" authors="squillace" solutions="" manager="timlt" editor="tysonn" />

<tags
   ms.service="virtual-machine"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="linux"
   ms.workload="infrastructure"
   ms.date="04/13/2015"
   ms.author="rasquill" />

## <a name="using-azure-cli-with-azure-resource-manager-arm"></a>Korištenje Azure EŽA s Azure Voditelj resursa (ARM)

Prije korištenja EŽA Azure s resursima naredbe i predloške za implementaciju Azure resurse i radnih opterećenja pomoću grupa resursa, morate račun s Azure (naravno). Ako nemate račun, možete dobiti [Azure probna ovdje](https://azure.microsoft.com/pricing/free-trial/).

> [AZURE.NOTE] Ako još nemate račun za Azure, ali niste pretplaćeni na MSDN pretplatu, možete dobiti besplatne Azure kredita aktiviranjem na [MSDN pretplatnika pogodnosti ovdje](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) –, ili pomoću besplatnog računa. Ili funkcionirat će za Azure pristup.

### <a name="step-1-verify-the-azure-cli-version"></a>Korak 1: Provjerite je li verzija EŽA Azure

Da biste koristili Azure EŽA izuzetne naredbe i ARM predloške, morate imati barem verzija 0.8.17. Da biste provjerili je li vaša verzija, upišite `azure --version`. Trebali biste vidjeti otprilike ovako:

    $ azure --version
    0.8.17 (node: 0.10.25)

Ako je potrebno ažurirati verziju Azure EŽA potražite u članku [EŽA Azure](https://github.com/Azure/azure-xplat-cli).

### <a name="step-2-verify-you-are-using-a-work-or-school-identity-with-azure"></a>Korak 2: Potvrda koristite tvrtke ili obrazovne ustanove identiteta s Azure

Način naredbe ARM možete koristiti samo ako koristite [Azure Active Directory klijenta](https://msdn.microsoft.com/library/azure/jj573650.aspx#BKMK_WhatIsAnAzureADTenant) ili [Glavno ime usluge](https://msdn.microsoft.com/library/azure/dn132633.aspx). (Zovu se *organizacijske ID-ova*.)

Da biste saznali je li nešto, prijavite se u tako da upišete `azure login` i korištenje tvrtke ili obrazovne ustanove korisničko ime i lozinku kada se to od vas zatraži. Ako nemate, trebali biste vidjeti otprilike ovako:

    $ azure login
    info:    Executing command login
    warn:    Please note that currently you can login only via Microsoft organizational account or service principal. For instructions on how to set them up, please read http://aka.ms/Dhf67j.
    Username: ahmet@contoso.onmicrosoft.com
    Password: *********
  	|info:    Added subscription Visual Studio Ultimate with MSDN
    info:    Setting subscription Visual Studio Ultimate with MSDN as default
    info:    Added subscription Azure Free Trial
    +
    info:    login command OK

Ako ne vidite, morate stvoriti novi klijent (ili servis glavni) s vašeg identiteta Microsoftova računa. (To je često u slučaju osobnih pretplate na MSDN ili besplatne probne pretplate.) Da biste stvorili tvrtke ili obrazovne ustanove ID-a s Azure stvorene pomoću id Microsoftova računa, potražite u članku [Povezivanje Azure AD Directory s novu pretplatu Azure](https://msdn.microsoft.com/library/azure/jj573650.aspx#BKMK_WhatIsAnAzureADTenant). Ako mislite da već mora imati id tvrtke ili ustanove, možda ćete morati obratite se osobi koja je stvorena s računom za.

### <a name="step-3-choose-your-azure-subscription"></a>Korak 3: Odaberite pretplatu Azure

Ako imate samo jedan pretplatu u račun za Azure, Azure EŽA poveže s tom pretplatu prema zadanim postavkama. Ako imate više pretplata, morate odaberite pretplatu koju želite koristiti tako da upišete `azure account set <subscription id or name> true` gdje _id pretplate ili naziv_ je id pretplate ili naziv pretplate koju želite raditi u trenutnoj sesiji.

Trebali biste vidjeti otprilike ovako:

    $ azure account set "Azure Free Trial" true
    info:    Executing command account set
    info:    Setting subscription to "Azure Free Trial" with id "2lskd82-434-4730-9df9-akd83lsk92sa".
    info:    Changes saved
    info:    account set command OK

### <a name="step-4-place-your-azure-cli-in-the-arm-mode"></a>Korak 4: Postavite svoje Azure EŽA u načinu rada za ARM

Da biste koristili način upravljanja Azure resursa (ARM) s EŽA Azure, upišite `azure config mode arm`. Trebali biste vidjeti otprilike ovako:

    $ azure config mode arm
    info:    New mode is arm

> [AZURE.NOTE] Prijeđete u naredbama servisa Azure upravljanja tako da upišete `azure config mode asm`.
