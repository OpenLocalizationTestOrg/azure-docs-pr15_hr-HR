<properties
    pageTitle="Servisa Azure državne | Microsoft Azure"
    description="Informacije o upravljanju pretplate u državne Azure"
    services="Azure-Government"
    cloud="gov" 
    documentationCenter=""
    authors="zakramer"
    manager="liki"
    editor="" />

<tags
    ms.service="multiple"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="azure-government"
    ms.date="10/21/2016"
    ms.author="zakramer" />


#  <a name="managing-and-connecting-to-your-subscription-in-azure-government"></a>Upravljanje i povezivanje s pretplate u državne Azure

Azure državne ima jedinstvene URL-ovi i krajnje točke za upravljanje vaše okruženje. Važno je da koristi desnom veze da biste upravljali vaše okruženje putem portala sustava i PowerShell. Nakon uspostave okruženje Azure državne normalni postupke za upravljanje servisa funkcionira ako je postavila komponentu.

## <a name="connecting-via-the-portal"></a>Povezivanje putem portala
Portal je primarni način na koji većina ljudi povezati državne Azure.  Da biste povezali, otvorite portal na [https://portal.azure.us](https://portal.azure.us).  Stari verziju Azure portal možete pristupiti putem [https://manage.windowsazure.us](https://manage.windowsazure.us).

Povezivanjem [https://account.windowsazure.us](https://account.windowsazure.us)možete moguće pretplate za vaš račun.

## <a name="connecting-via-powershell"></a>Povezivanje putem komponente PowerShell

Je li koristite Azure PowerShell za upravljanje velikim pretplati putem skripte ili access značajke koje se trenutno nisu dostupne na portalu Azure morate se povezati s Azure državne umjesto Azure javno.  Ako ste koristili PowerShell u javnom Azure, je uglavnom ista.  Razlike u Azure državne su:

+ Povezivanje računa
+ Nazivi područja

>[AZURE.NOTE] Ako još niste koristili PowerShell, pogledajte [Uvod u Azure PowerShell](../powershell-install-configure.md).

Kada počnete PowerShell, imate reći PowerShell Azure povezati Azure državne tako da navedete parametar u okruženju.  Parametar osigurava povezivanje PowerShell točan krajnje točke.  Skup krajnje točke određuje kada se povežete Prijava na račun.  Drugi API-ji zahtijevaju različite verzije sustava parametar okruženja:

Vrsta veze | Naredba
---|----
[Upravljanje servisom](https://msdn.microsoft.com/library/dn708504.aspx) naredbe | `Add-AzureAccount -Environment AzureUSGovernment`
[Upravljanje resursima](https://msdn.microsoft.com/library/mt125356.aspx) naredbe | `Add-AzureRmAccount -EnvironmentName AzureUSGovernment`
Naredbe za [Azure Active Directory](https://msdn.microsoft.com/library/azure/jj151815.aspx) | `Connect-MsolService -AzureEnvironment UsGovernment`
[V2 naredbu za Azure Active Directory](https://msdn.microsoft.com/library/azure/mt757189.aspx) | `Connect-AzureAD -AzureEnvironmentName AzureUSGovernment`

Možete koristiti parametar okruženje prilikom povezivanja s računom za pohranu pomoću novo AzureStorageContext i navedite AzureUSGovernment.

### <a name="determining-region"></a>Određivanje područja

Kada ste povezani, postoji jedan dodatni razlika – područja za ciljnu servisa.  Svaki Azure oblaka ima različite regije.  Vidjet ćete ih naveden na stranici dostupnost servisa.  Obično pomoću područja u parametru mjesto za naredbu.

Postoji jedan zamka.  Azure državne regija nije potrebno oblikovani različito od njihova uobičajena imena učinite sljedeće:

Uobičajeni naziv | Naredba
---|----
SAD Gov Virginia | USGov Virginia
SAD Gov Iowa | USGov Iowa

>[AZURE.NOTE] Postoji bez razmaka između SAD-a i Gov prilikom korištenja parametar mjesto.

Ako ne želite provjeriti valjanost dostupna regijama državne Azure, možete pokrenuti sljedeće naredbe i ispišite trenutni popis:

    Get-AzureLocation

Ako vas zanima dostupna okruženja preko Azure, možete pokrenuti:

    Get-AzureEnvironment

## <a name="next-steps"></a>Daljnji koraci

Ako vas zanimaju dodatne informacije, pročitajte članak:

+ [PowerShell dokumente na GitHub](https://github.com/Azure/azure-powershell)
+ [Detaljne upute o povezivanju s Upravljanje resursima](https://blogs.msdn.microsoft.com/azuregov/2015/10/08/configuring-arm-on-azure-gc/)
+ [Azure PowerShell dokumente na MSDN-u](https://msdn.microsoft.com/library/mt619274.aspx)

Dodatne informacije i ažuriranja pretplata [Microsoft Azure državne bloga] (https://blogs.msdn.microsoft.com/azuregov/)
