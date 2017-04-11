<properties
   pageTitle="Veličina mape TEMP zadani je presitan za uloge | Microsoft Azure"
   description="Uloge servisa oblaka ima tijekom ograničenog vremenskog prostora za mape TEMP. Ovaj članak sadrži nekoliko prijedloga o tome kako izbjeći pokretanje nema dovoljno prostora."
   services="cloud-services"
   documentationCenter=""
   authors="simonxjx"
   manager="felixwu"
   editor=""
   tags="top-support-issue"/>
<tags
   ms.service="cloud-services"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="tbd"
   ms.date="10/12/2016"
   ms.author="v-six" />

# <a name="default-temp-folder-size-is-too-small-on-a-cloud-service-webworker-role"></a>Veličina mape TEMP zadani je presitan na web-tempiranja uloge servisa oblaka

Zadani privremeni direktorij ulogu oblaka servisa tempiranja ili web-ima maksimalne veličine 100 MB, što može postati cijelog trenutku. U ovom se članku opisuje kako izbjeći ponestati prostora za privremeni direktorij.

[AZURE.INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

## <a name="why-do-i-run-out-of-space"></a>Zašto ponestati mjesta?

Standardni Windows varijable okruženja TEMP i TMP dostupni su za kod koji se izvodi u aplikaciji. TEMP i TMP pokažite na jednu direktoriju maksimalne veličine 100 MB. Sve podatke koji su pohranjenu u taj imenik je ista i preko životnim ciklusom servisa u oblaku; Ako su instance uloga u oblaku brisanja, očistiti je imenika.

## <a name="suggestion-to-fix-the-problem"></a>Da biste riješili problem

Implementacija jednu od sljedećih mogućnosti:

- Konfiguriranje lokalno spremište resursa i pristupiti izravno umjesto korištenja TEMP ili TMP. Da biste pristupili lokalno spremište resursa iz kod koji se izvodi u aplikaciji, nazovite metodu [RoleEnvironment.GetLocalResource](https://msdn.microsoft.com/library/microsoft.windowsazure.serviceruntime.roleenvironment.getlocalresource.aspx) . 

- Konfiguriranje lokalno spremište resursa i pokažite direktorija TEMP i TMP tako da pokazuje na put resursa lokalno spremište. Ova izmjena treba izvršiti unutar metodu [RoleEntryPoint.OnStart](https://msdn.microsoft.com/library/microsoft.windowsazure.serviceruntime.roleentrypoint.onstart.aspx) .

Sljedeći primjer koda prikazuje kako promijeniti direktorija cilj TEMP i TMP iz unutar OnStart metoda:


```csharp
using System;
using Microsoft.WindowsAzure.ServiceRuntime;

namespace WorkerRole1
{
    public class WorkerRole : RoleEntryPoint
    {
        public override bool OnStart()
        {
            // The local resource declaration must have been added to the
            // service definition file for the role named WorkerRole1:
            //
            // <LocalResources>
            //    <LocalStorage name="CustomTempLocalStore"
            //                  cleanOnRoleRecycle="false"
            //                  sizeInMB="1024" />
            // </LocalResources>

            string customTempLocalResourcePath =
            RoleEnvironment.GetLocalResource("CustomTempLocalStore").RootPath;
            Environment.SetEnvironmentVariable("TMP", customTempLocalResourcePath);
            Environment.SetEnvironmentVariable("TEMP", customTempLocalResourcePath);

            // The rest of your startup code goes here…

            return base.OnStart();
        }
    }
}
```

## <a name="next-steps"></a>Daljnji koraci

Pročitajte blog na kojem se objašnjava [kako povećati veličinu Azure web-uloga ASP.NET privremene mape](http://blogs.msdn.com/b/kwill/archive/2011/07/18/how-to-increase-the-size-of-the-windows-azure-web-role-asp-net-temporary-folder.aspx).

Prikaz više [članaka za otklanjanje poteškoća](/?tag=top-support-issue&product=cloud-services) za servise u oblaku.

Upute za otklanjanje poteškoća s oblaka servisa uloga pomoću Azure PaaS računalo Dijagnostika podataka, prikaz [nizu blogova Kevin Williamson](http://blogs.msdn.com/b/kwill/archive/2013/08/09/windows-azure-paas-compute-diagnostics-data.aspx).
