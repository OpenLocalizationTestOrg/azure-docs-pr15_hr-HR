<properties
   pageTitle="Nadogradnja programa klaster tkanina samostalne usluge na poslužitelju Windows | Microsoft Azure"
   description="Nadogradnja kod usluge tkanina i/ili konfiguracije koji se izvodi klaster samostalne tkanina servisa, uključujući postavljanje način ažuriranja klaster"
   services="service-fabric"
   documentationCenter=".net"
   authors="ChackDan"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="10/10/2016"
   ms.author="chackdan"/>


# <a name="upgrade-your-standalone-service-fabric-cluster-on-windows-server"></a>Nadogradnja samostalne klaster za tkanina servisa u sustavu Windows Server

> [AZURE.SELECTOR]
- [Azure klaster](service-fabric-cluster-upgrade.md)
- [Samostalne klaster](service-fabric-cluster-upgrade-windows-server.md)

Dizajniranje za upgradability modernom sustavu je ključ postizanja Dugoročne uspjeh proizvoda. Servis tkanina klaster je resurs da ste vlasnik. U ovom se članku opisuje kako možete biti sigurni da klaster uvijek pokreće podržane verzije servisa tkanina kod i konfiguracija.

## <a name="controlling-the-fabric-version-that-runs-on-your-cluster"></a>Kontroliranje tkanina verziju koja se izvršava na svoj klaster

Možete postaviti svoj klaster preuzimanje tkanina ažuriranja servisa kada Microsoft izda nova verzija ili odaberite da biste odabrali tkanina podržanu verziju želite svoj klaster biti uključeno. 

To se postavljanjem klaster konfiguracija "fabricClusterAutoupgradeEnabled" true ili false.


>[AZURE.NOTE] Provjerite jeste li zadržati svoj klaster uvijek pokrenuti podržanu verziju tkanina servisa. Kao i kada ne možemo objava izdanje novu verziju servisa tkanina prethodne verzije označen za kraj podršku nakon najmanje 60 dana od datuma. Nova izdanja najavljena se [na blogu tima tkanina servisa](https://blogs.msdn.microsoft.com/azureservicefabric/ ). Novo izdanje dostupna pa odaberite. 


Možete nadograditi svoj klaster novu verziju samo ako koristite konfiguraciju čvor radnog stil, gdje svaki servis tkanina čvor dodijeljeno na zasebnom fizičke ili virtualnog računala. Ako imate klaster razvoj, gdje se nalaze više od jedne usluge tkanina čvorove na jednom fizičke ili virtualnog računala, tear dolje svoj klaster i ponovno ga stvorite pomoću nove verzije.


Postoje dva različita tijekovi rada za nadogradnju svoj klaster najnoviji ili na podržanom servisu tkanina verziju. Jedan za klastere koje imaju povezivanje da biste automatski preuzeli najnoviju verziju, a drugu za klastere koje su bez povezivanje da biste preuzeli najnoviju verziju servisa tkanina.

### <a name="upgrade-the-clusters-with-connectivity-to-download-the-latest-code-and-configuration"></a>Nadogradnja skupina s povezivanjem da biste preuzeli najnovije kod i konfiguracija 

Pomoću ovih koraka za nadogradnju svoj klaster podržanu verziju ako vaš čvorove klaster imate internetsku vezu s [http://download.microsoft.com](http://download.microsoft.com) 

Za klastere koje povezanost s [http://download.microsoft.com](http://download.microsoft.com)ćemo povremeno potražiti dostupnost nove verzije tkanina servisa.


Kada je nova verzija tkanina servis nije dostupan, paket je preuzeti lokalno klaster i resursi za nadogradnju. Uz to obavještavate kupac ova novu verziju sustava postavlja se upozorenje stanja eksplicitnih klaster sličnu ovoj:

"Trenutno klaster verzija [verzija #] podršku završava [Datum].", nakon klaster traje najnoviju verziju, upozorenje nestaje.


#### <a name="cluster-upgrade-workflow"></a>Nadogradnja tijeka rada s klaster.
 
Kada se prikaže upozorenje o stanju klaster, trebali biste učiniti sljedeće:

1. Povezivanje s klaster iz bilo kojeg uređaja koji ima pristup svim strojevima koji su navedeni kao čvorovi u klasteru administratora. Na računalu koje se izvršava Ova skripta ne mora biti dio klaster

    ```powershell

    ###### connect to the secure cluster using certs
    $ClusterName= "mysecurecluster.something.com:19000"
    $CertThumbprint= "70EF5E22ADB649799DA3C8B6A6BF7FG2D630F8F3" 
    Connect-serviceFabricCluster -ConnectionEndpoint $ClusterName -KeepAliveIntervalInSec 10 `
        -X509Credential `
        -ServerCertThumbprint $CertThumbprint  `
        -FindType FindByThumbprint `
        -FindValue $CertThumbprint `
        -StoreLocation CurrentUser `
        -StoreName My
    ```

2. Dobiti popis servisa tkanina verzije koje možete nadograditi

    ```powershell

    ###### Get the list of available service fabric versions 
    Get-ServiceFabricRegisteredClusterCodeVersion
    ```

    za izlazni moraju se sličnu ovoj:

    ![Početak tkanina verzije][getfabversions]

3. Izbaciti klaster nadogradnju na neki od verzije koja je dostupna pomoću [komponente PowerShell Start ServiceFabricClusterUpgrade cmd](https://msdn.microsoft.com/library/mt125872.aspx)

    ```Powershell

    Start-ServiceFabricClusterUpgrade -Code -CodePackageVersion <codeversion#> -Monitored -FailureAction Rollback

    ###### Here is a filled out example

    Start-ServiceFabricClusterUpgrade -Code -CodePackageVersion 5.3.301.9590 -Monitored -FailureAction Rollback
    
    ```
Možete pratiti napredak nadogradnje servisa tkanina explorer ili ponovnim pokretanjem sljedeće naredbe ljuske power

    ```powershell

    Get-ServiceFabricClusterUpgrade
    ```

    If the cluster health policies are not met, the upgrade is rolled back. You can specify custom health policies at the time for the Start-ServiceFabricClusterUpgrade command refer to [this document](https://msdn.microsoft.com/library/mt125872.aspx) for details. 

Nakon što ste otkloniti probleme koja je rezultirala vraćanje, morate ponovno pokrenuti nadogradnju slijedeći iste korake kao prije.


### <a name="upgrade-the-clusters-with-uno-connectivityu-to-download-the-latest-code-and-configuration"></a>Nadogradnja klastere s <U>nema povezivanja</u> da biste preuzeli najnovije kod i konfiguracija

Pomoću ovih koraka za nadogradnju svoj klaster podržanu verziju ako vaš klaster čvorove **imate** internetsku vezu s [http://download.microsoft.com](http://download.microsoft.com) 


>[AZURE.NOTE]Ako su pokrenuti klaster koji nije internet povezani, morat ćete praćenje tkanina blog tima za servis za primanje obavijesti o novim izdanjima. Sustav **ne** postavite sve klaster stanja upozorenje upozorenje se.  

1. Izmijenite konfiguraciju klaster da biste postavili svojstvo sljedeće FALSE.

        "fabricClusterAutoupgradeEnabled": false,

i izbaciti konfiguracije nadogradnju. Pogledajte [Početak ServiceFabricClusterUpgrade PS cmd](https://msdn.microsoft.com/library/mt125872.aspx) za informacije o korištenju. Verzija manifesta klaster je verzija koje imate u clusterConfig.JSON. Provjerite jeste li ažurirati prije nego što započnite isključivanje nadogradnje konfiguracije.

```powershell

    Start-ServiceFabricClusterUpgrade [-Config] [-ClusterConfigVersion] -FailureAction Rollback -Monitored 

```

#### <a name="cluster-upgrade-workflow"></a>Nadogradnja tijek rada s klaster.
 


1. Preuzmite najnoviju verziju paketa iz dokumenta za [Stvaranje servisa tkanina klaster za windows server](service-fabric-cluster-creation-for-windows-server.md) 


1. Povezivanje s klaster iz bilo kojeg uređaja koji ima pristup svim strojevima koji su navedeni kao čvorovi u klasteru administratora. Na računalu koje se izvršava Ova skripta ne mora biti dio klaster 

    ```powershell

    ###### connect to the cluster
    $ClusterName= "mysecurecluster.something.com:19000"
    $CertThumbprint= "70EF5E22ADB649799DA3C8B6A6BF7FG2D630F8F3" 
    Connect-serviceFabricCluster -ConnectionEndpoint $ClusterName -KeepAliveIntervalInSec 10 `
        -X509Credential `
        -ServerCertThumbprint $CertThumbprint  `
        -FindType FindByThumbprint `
        -FindValue $CertThumbprint `
        -StoreLocation CurrentUser `
        -StoreName My
    ```

2. Kopirajte preuzetu paketa u trgovini klaster slika.

    ```powershell

    ###### Get the list of available service fabric versions 
    Copy-ServiceFabricClusterPackage -Code -CodePackagePath <name of the .cab file including the path to it> -ImageStoreConnectionString "fabric:ImageStore"

    ###### Here is a filled out example
    Copy-ServiceFabricClusterPackage -Code -CodePackagePath .\MicrosoftAzureServiceFabric.5.3.301.9590.cab -ImageStoreConnectionString "fabric:ImageStore"


    ```

2. Registrirajte se kopirani paketa 

    ```powershell

    ###### Get the list of available service fabric versions 
    Register-ServiceFabricClusterPackage -Code -CodePackagePath <name of the .cab file> 

    ###### Here is a filled out example
    Register-ServiceFabricClusterPackage -Code -CodePackagePath MicrosoftAzureServiceFabric.5.3.301.9590.cab

     ```


3. Izbaciti klaster nadogradnju na neki od verzije koja je dostupna. 

    ```Powershell

    Start-ServiceFabricClusterUpgrade -Code -CodePackageVersion <codeversion#> -Monitored -FailureAction Rollback

    ###### Here is a filled out example
    Start-ServiceFabricClusterUpgrade -Code -CodePackageVersion 5.3.301.9590 -Monitored -FailureAction Rollback
    
    ```
Možete pratiti napredak nadogradnje servisa tkanina explorer ili ponovnim pokretanjem sljedeće naredbe ljuske power

    ```powershell

    Get-ServiceFabricClusterUpgrade
    ```

    If the cluster health policies are not met, the upgrade is rolled back. You can specify custom health policies at the time for the start-serviceFabricClusterUpgrade command refer to [this document](https://msdn.microsoft.com/library/mt125872.aspx) for details. 

Nakon što ste otkloniti probleme koja je rezultirala vraćanje, morate ponovno pokrenuti nadogradnju slijedeći iste korake kao prije.



## <a name="next-steps"></a>Daljnji koraci
- Saznajte kako prilagoditi neke [Postavke servisa tkanina za tkanina klaster](service-fabric-cluster-fabric-settings.md)
- Saznajte kako [i smanjivati skaliranje svoj klaster](service-fabric-cluster-scale-up-down.md)
- Dodatne informacije o [nadogradnji aplikacije](service-fabric-application-upgrade.md)

<!--Image references-->
[getfabversions]: ./media/service-fabric-cluster-upgrade-windows-server/getfabversions.PNG
