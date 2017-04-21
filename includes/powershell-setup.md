<properties services="virtual-machines" title="Setting up PowerShell" authors="JoeDavies-MSFT" solutions="" manager="timlt" editor="tysonn" />

<tags
   ms.service="virtual-machines"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm=""
   ms.workload="infrastructure"
   ms.date="05/12/2015"
   ms.author="rasquill" />

## <a name="setting-up-powershell"></a>Postavljanje PowerShell

Da biste koristili Azure PowerShell, slijedite ove korake.

### <a name="verify-powershell-versions"></a>Provjerite je li verzija programa PowerShell

Da biste koristili Windows PowerShell, morate imati komponente Windows PowerShell, verzije 3.0 ili 4.0. Da biste pronašli verziju sustava Windows PowerShell, upišite sljedeću naredbu komponente Windows PowerShell naredbeni redak.

    $PSVersionTable

Trebali biste vidjeti otprilike ovako.

    Name                           Value
    ----                           -----
    PSVersion                      3.0
    WSManStackVersion              3.0
    SerializationVersion           1.1.0.1
    CLRVersion                     4.0.30319.18444
    BuildVersion                   6.2.9200.16481
    PSCompatibleVersions           {1.0, 2.0, 3.0}
    PSRemotingProtocolVersion      2.2

Provjerite je li vrijednost **PSVersion** 3.0 ili 4.0. Da biste instalirali kompatibilna verzija, potražite u članku [Windows Management Framework 3.0](http://www.microsoft.com/download/details.aspx?id=34595) ili [Windows Management Framework 4.0](http://www.microsoft.com/download/details.aspx?id=40855).

Trebali biste dobiti i Azure PowerShell verzije 0.8.0 ili noviji. Možete provjeriti verziju Azure PowerShell koji ste instalirali tu naredbu u naredbenom retku Azure PowerShell.

    Get-Module azure | format-table version

Trebali biste vidjeti otprilike ovako.

    Version
    -------
    0.8.16.1

Upute i veze na najnoviju verziju, potražite [u](powershell-install-configure.md)članku instalacija i konfiguriranje Azure PowerShell.


### <a name="set-your-azure-account-and-subscription"></a>Postavite račun za Azure i pretplate

Ako već nemate Azure pretplatu, možete aktivirati [MSDN pretplatnika pogodnosti](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) ili znak prema gore za [besplatnu probnu verziju](https://azure.microsoft.com/pricing/free-trial/).

Otvorite Azure PowerShell naredbeni redak i prijavite se u Azure pomoću ove naredbe.

    Add-AzureAccount

Ako imate više pretplata Azure, može prikazati pretplate Azure pomoću ove naredbe.

    Get-AzureSubscription

Primit ćete sljedeću vrstu informacija:

    SubscriptionId            : fd22919d-eaca-4f2b-841a-e4ac6770g92e
    SubscriptionName          : Visual Studio Ultimate with MSDN
    Environment               : AzureCloud
    SupportedModes            : AzureServiceManagement,AzureResourceManager
    DefaultAccount            : johndoe@contoso.com
    Accounts                  : {johndoe@contoso.com}
    IsDefault                 : True
    IsCurrent                 : True
    CurrentStorageAccountName : 
    TenantId                  : 32fa88b4-86f1-419f-93ab-2d7ce016dba7

Trenutne Azure pretplate možete postaviti tako da pokrenete te naredbe u naredbenom retku Azure PowerShell. Zamijenite sve unutar navodnika, uključujući na < i > znakova s točan naziv.

    $subscr="<SubscriptionName from the display of Get-AzureSubscription>"
    Select-AzureSubscription -SubscriptionName $subscr -Current 

Dodatne informacije o Azure pretplate i računa potražite u članku [Kako: povezivanje s pretplate](powershell-install-configure.md#Connect).
