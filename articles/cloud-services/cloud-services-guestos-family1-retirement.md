<properties
   pageTitle="OS za goste obitelji 1 umirovljenje obratite pozornost na to | Microsoft Azure"
   description="Pruža informacije o kada se dogodilo umirovljenje Azure goste OS obitelji 1 i kako odrediti ako postoji pogreška"
   services="cloud-services"
   documentationCenter="na"
   authors="raiye"
   manager="timlt"
   editor=""/>

<tags
   ms.service="cloud-services"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="tbd"
   ms.date="10/24/2016"
   ms.author="raiye"/>



# <a name="guest-os-family-1-retirement-notice"></a>Obavijest o umirovljenje goste OS obitelji 1

Umirovljenje OS obitelji 1 je najprije objaviti na lipnja 1, 2013.

**2. rujan 2014.** Operacijski sustav Azure goste (goste OS) obitelji 1.x koji se temelji na operacijskom sustavu Windows Server 2008, povučena je službeno iz upotrebe. Poruka o pogrešci koja vas obavještava da je povučena obitelji 1 za goste s operacijskim Sustavom neće uspjeti sve pokušaje za implementaciju nove servise ili nadogradnja postojeće usluge korištenja obitelji 1.

**3. studenom 2014.** Dovršen produljena podrška za goste OS obitelji 1 i potpuno povučena. Sve servise i dalje na 1 obitelji neće utjecati. Ne možemo može prestati tih servisa u bilo kojem trenutku. Nema jamstva servisa će se i dalje se izvoditi dok ručno nadogradnje ih sami.

Ako imate dodatna pitanja, posjetite [Forume oblaka Services](http://social.msdn.microsoft.com/Forums/home?forum=windowsazuredevelopment&filter=alltypes&sort=lastpostdesc) ili [obratite se podršci za Azure](https://azure.microsoft.com/support/options/).




## <a name="are-you-affected"></a>Koje utječe?

Utječe na servise u Oblaku ako se odnosi nešto od sljedećeg:

1. Imaju vrijednost "osFamily ="1"izričito navedeno u datoteci ServiceConfiguration.cscfg za servis u Oblaku.
2. Nemaju vrijednost za osFamily izričito navedeno u datoteci ServiceConfiguration.cscfg za servis u Oblaku. Trenutno sustav koristi zadanu vrijednost od "1" u ovom slučaju.
3. Azure klasični portal navodi goste operacijski sustav obitelji vrijednosti kao "Windows Server 2008".

Da biste pronašli koje servise u oblaku koristite koje obitelji OS, možete pokrenuti skriptu ispod u ljusci PowerShell Azure, iako se najprije morate [postaviti Azure PowerShell](../powershell-install-configure.md) . Dodatne informacije o skriptu potražite u članku [Azure goste OS obitelji 1 završetka od vijek: 2014 lipnja](http://blogs.msdn.com/b/ryberry/archive/2014/04/02/azure-guest-os-family-1-end-of-life-june-2014.aspx). 

```Powershell
foreach($subscription in Get-AzureSubscription) {
    Select-AzureSubscription -SubscriptionName $subscription.SubscriptionName

    $deployments=get-azureService | get-azureDeployment -ErrorAction Ignore | where {$_.SdkVersion -NE ""}

    $deployments | ft @{Name="SubscriptionName";Expression={$subscription.SubscriptionName}}, ServiceName, SdkVersion, Slot, @{Name="osFamily";Expression={(select-xml -content $_.configuration -xpath "/ns:ServiceConfiguration/@osFamily" -namespace $namespace).node.value }}, osVersion, Status, URL
}
```

Servisi u oblaku će biti utječe umirovljenje OS obitelji 1 ako stupac osFamily u rezultatu skripte je prazna ili sadrži "1".

## <a name="recommendations-if-you-are-affected"></a>Preporuke ako vam

Preporučujemo da migrirati vašim ulogama u Oblaku na neki od podržanih linije OS goste:

**Goste OS obitelji 4.x** – Windows Server 2012 R2 *(preporučeno)*

1. Provjerite je li vaša aplikacija je pomoću SDK 2.1 ili noviji s .NET framework 4.0, 4,5 ili 4.5.1.
2. Postavite atribut osFamily na "4" u datoteci ServiceConfiguration.cscfg i ponovno implementirate servis u oblaku.


**Goste OS obitelji 3.x** – Windows Server 2012

1. Provjerite je li vaša aplikacija je pomoću SDK 1.8 ili noviji s .NET framework 4.0 ili 4,5.
2. Postavite atribut osFamily "3" u datoteci ServiceConfiguration.cscfg i ponovno implementirate servis u oblaku.


**Goste OS obitelji 2.x** – Windows Server 2008 R2

1. Provjerite je li aplikacija koristi SDK 1.3 i iznad s .NET framework 3.5 ili 4.0.
2. Postavite atribut osFamily "2" u datoteci ServiceConfiguration.cscfg i ponovno implementirate servis u oblaku.


## <a name="extended-support-for-guest-os-family-1-ended-nov-3-2014"></a>Prošireni podrška za goste OS obitelji 1 završen riječi studeni 3, 2014.
Servisi u oblaku na OS goste obitelji 1 više nisu podržane. Provjerite migrirati isključivanje obitelji 1 čim da bi se izbjeglo prekidu servisa.  

## <a name="next-steps"></a>Daljnji koraci
Pregledajte najnovija [izdanja goste OS](cloud-services-guestos-update-matrix.md).
