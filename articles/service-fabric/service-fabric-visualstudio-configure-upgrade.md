<properties
   pageTitle="Konfiguriranje nadogradnju aplikacije servisa tkanina | Microsoft Azure"
   description="Saznajte kako konfigurirati postavke za nadogradnju s aplikacijom servisa tkanina pomoću Microsoft Visual Studio."
   services="service-fabric"
   documentationCenter="na"
   authors="cawaMS"
   manager="paulyuk"
   editor="tglee" />
<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="multiple"
   ms.date="07/29/2016"
   ms.author="cawa" />

# <a name="configure-the-upgrade-of-a-service-fabric-application-in-visual-studio"></a>Konfiguriranje nadogradnju aplikacije servisa tkanina u Visual Studio

Visual Studio tools za tkanina servisa Azure podržavaju nadogradnje za objavljivanje na lokalnom ili udaljenom klastere. Postoje dvije prednosti nadogradnje aplikacije na noviju verziju umjesto zamjene aplikacije tijekom testiranja i ispravljanja pogrešaka:

- Tijekom nadogradnje neće biti izgubljene podataka aplikacije.
- Dostupnost ostaje visoke tako da neće biti prekida servisa tijekom nadogradnje, ako postoje dovoljno instanci servisa proširite preko nadogradnje domene.

Testira mogu se izvoditi na temelju aplikacije dok je nadograđen.

## <a name="parameters-needed-to-upgrade"></a>Parametri potrebne za nadogradnju

Možete odabrati dvije vrste implementacije: običnog ili nadogradnje. Uobičajeni implementacije briše prethodne implementacije podatke, a podatke na klaster, tijekom nadogradnje implementacije zadržava ga. Nakon nadogradnje aplikacije servisa tkanina u Visual Studio, morate unijeti parametara za nadogradnju aplikacije i stanje provjerite pravila. Parametri za nadogradnju aplikacije spriječiti nadogradnje dok pravila provjere stanja određivanje hoće li je nadogradnja uspješno. Potražite u članku [nadogradnju aplikacije servisa tkanina: Nadogradnja parametara](service-fabric-application-upgrade-parameters.md) više pojedinosti.

Postoje tri načina za nadogradnju: *nadziranih*, *UnmonitoredAuto*i *UnmonitoredManual*.

  - Nadziranih nadogradnje automatizira nadogradnje i provjera stanja računala.

  - Nadogradnja UnmonitoredAuto automatizira nadogradnje, ali preskače Provjera stanja računala.

  - Nakon nadogradnje UnmonitoredManual, morate ručno nadograditi sve nadogradnje domene.

Način svake nadogradnje zahtijeva različite skupove parametara. Pogledajte [aplikacije nadograditi parametara](service-fabric-application-upgrade-parameters.md) da biste saznali više o dostupnim mogućnostima nadogradnje.

## <a name="upgrade-a-service-fabric-application-in-visual-studio"></a>Nadogradnja aplikacije servisa tkanina u Visual Studio

Ako koristite Alati za Visual Studio servisa tkanina za nadogradnju s aplikacijom servisa tkanina, možete odrediti postupak objavljivanja treba nadogradnju umjesto obične implementacije tako da poništite potvrdni okvir **nadogradnju aplikacije** .

### <a name="to-configure-the-upgrade-parameters"></a>Da biste konfigurirali nadogradnje parametre

1. Kliknite gumb **Postavke** pokraj potvrdnog okvira. Pojavit će se dijaloški okvir **Uređivanje parametara za nadogradnju** . U dijaloški okvir **Uređivanje nadograditi parametar** podržava načini nadogradnje nadziranih, UnmonitoredAuto i UnmonitoredManual.

2. Odaberite način nadogradnje koji želite koristiti, a zatim ispunite rešetki parametar.

    Svaki parametar sadrži zadane vrijednosti. Neobavezan parametar *DefaultServiceTypeHealthPolicy* vodi unos za tablicu raspršivanje. Evo primjera raspršivanje oblik unos tablice za *DefaultServiceTypeHealthPolicy*:

    ```
    @{ ConsiderWarningAsError = "false"; MaxPercentUnhealthyDeployedApplications = 0; MaxPercentUnhealthyServices = 0; MaxPercentUnhealthyPartitionsPerService = 0; MaxPercentUnhealthyReplicasPerPartition = 0 }
    ```

    *ServiceTypeHealthPolicyMap* je drugi neobavezan parametar koji vodi unos za tablicu raspršivanje u sljedećem obliku:

    ```    
    @ {"ServiceTypeName" : "MaxPercentUnhealthyPartitionsPerService,MaxPercentUnhealthyReplicasPerPartition,MaxPercentUnhealthyServices"}
    ```

    Evo primjera stvarnih:

    ```
    @{ "ServiceTypeName01" = "5,10,5"; "ServiceTypeName02" = "5,5,5" }
    ```

3. Ako odaberete načina nadogradnje UnmonitoredManual, morate ručno pokrenuti konzole za PowerShell da biste nastavili i završi postupak nadogradnje. Pogledajte [nadogradnju aplikacije servisa tkanina: napredne teme](service-fabric-application-upgrade-advanced.md) da biste saznali kako ručno nadogradnje radi.

## <a name="upgrade-an-application-by-using-powershell"></a>Nadogradnja aplikacije pomoću komponente PowerShell

Možete koristiti PowerShell cmdleti za nadogradnju servisa tkanina aplikacije. Detaljne informacije potražite u [aplikaciju servisa tkanina nadograditi Praktični vodič](service-fabric-application-upgrade-tutorial.md) i [Početak ServiceFabricApplicationUpgrade](https://msdn.microsoft.com/library/mt125975.aspx) .

## <a name="specify-a-health-check-policy-in-the-application-manifest-file"></a>Navedite pravilo provjere stanja u datoteci manifesta aplikacije

Svaku uslugu u aplikaciji servisa tkanina može imati vlastitu parametara pravila stanja koji nadjačavaju zadane vrijednosti. Možete unijeti sljedeće vrijednosti parametara u datoteci manifesta aplikacije.

Sljedeći primjer prikazuje način za primjenu pravila potvrdite jedinstveni stanja za svaku uslugu u manifestu aplikacije.

```
<Policies>
    <HealthPolicy ConsiderWarningAsError="false" MaxPercentUnhealthyDeployedApplications="20">
        <DefaultServiceTypeHealthPolicy MaxPercentUnhealthyServices="20"               
                MaxPercentUnhealthyPartitionsPerService="20"
                MaxPercentUnhealthyReplicasPerPartition="20" />
        <ServiceTypeHealthPolicy ServiceTypeName="ServiceTypeName1"
                MaxPercentUnhealthyServices="20"
                MaxPercentUnhealthyPartitionsPerService="20"
                MaxPercentUnhealthyReplicasPerPartition="20" />      
    </HealthPolicy>
</Policies>
```
## <a name="next-steps"></a>Daljnji koraci
Dodatne informacije o implementaciji aplikacije potražite u članku [uvođenja postojeće aplikacije u tkanina servisa Azure](service-fabric-deploy-existing-app.md).
