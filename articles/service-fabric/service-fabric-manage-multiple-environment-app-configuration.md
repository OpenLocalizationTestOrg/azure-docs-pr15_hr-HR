<properties
   pageTitle="Upravljanje više okruženja u servis tkanina | Microsoft Azure"
   description="Servis tkanina aplikacije mogu se izvoditi na klastere rasponu veličina s jednog računala na tisuće strojeva. U nekim slučajevima želite konfigurirali aplikaciju drugačije za tih brojeva okruženja. U ovom se članku opisuje kako definiranje parametara drugu aplikaciju po okruženju."
   services="service-fabric"
   documentationCenter=".net"
   authors="seanmck"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotNet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="07/19/2016"
   ms.author="seanmck"/>

# <a name="manage-application-parameters-for-multiple-environments"></a>Upravljanje parametara aplikacije za više okruženja

Tkanina servisa Azure klastere možete stvoriti pomoću bilo kojeg mjesta s mnogo tisućama slika s računala. Dok binarne datoteke iz aplikacije možete pokrenuti bez izmjene preko ovaj širine razni načini okruženja, često ćete željeti Konfiguriranje aplikacije drugačije, ovisno o broju strojeva ste uvođenja.

Preporučuje se kao primjeru jednostavne `InstanceCount` bez praćenja stanja servisa. Ako koristite aplikacije u Azure će obično želite postaviti taj parametar posebno vrijednost broja-"1". Time se osigurava da na svaku čvor u klasteru nije ostalo na servisu. No tu konfiguraciju nije prikladna za na jednom računalu klaster jer ne možete imati više procesa koji se priključuje na istom krajnje točke na jednom računalu. Umjesto toga, koji ćete obično postaviti `InstanceCount` "1".

## <a name="specifying-environment-specific-parameters"></a>Određivanje parametara specifične za okruženje

Rješenje za ovaj problem s konfiguracijom je skup s parametrima zadane servise i datoteka aplikacije parametar koji ispunjavaju te vrijednosti parametara za dani okruženje. Zadane servise i aplikacije parametara nije konfiguriran u manifesti aplikacija i servisa. Definicija sheme za datoteke ServiceManifest.xml i ApplicationManifest.xml instaliran je uz servis tkanina SDK i alate za *C:\Programske datoteke\Microsoft SDKs\Service Fabric\schemas\ServiceFabricServiceModel.xsd*.

### <a name="default-services"></a>Zadane servise

Aplikacije servisa tkanina se sastoje od Zbirka instanci servisa. Dok je moguće stvoriti praznu aplikaciju i dinamičko stvaranje sve instance servisa, većina aplikacija imati skup core services koje treba kreirati uvijek kada je instancirati aplikacije. Ove se nazivaju "zadane servise". Oni određeni su klauzulom programski manifest s rezerviranim mjestima za konfiguraciju po okruženju uključeni u uglatim zagradama:

    <DefaultServices>
        <Service Name="Stateful1">
            <StatefulService
                ServiceTypeName="Stateful1Type"
                TargetReplicaSetSize="[Stateful1_TargetReplicaSetSize]"
                MinReplicaSetSize="[Stateful1_MinReplicaSetSize]">

                <UniformInt64Partition
                    PartitionCount="[Stateful1_PartitionCount]"
                    LowKey="-9223372036854775808"
                    HighKey="9223372036854775807"
                />
        </StatefulService>
    </Service>
  </DefaultServices>

Svaki imenovani parametara potrebno definirati unutar elementa parametara manifesta aplikacije:

    <Parameters>
        <Parameter Name="Stateful1_MinReplicaSetSize" DefaultValue="2" />
        <Parameter Name="Stateful1_PartitionCount" DefaultValue="1" />
        <Parameter Name="Stateful1_TargetReplicaSetSize" DefaultValue="3" />
    </Parameters>

Atribut DefaultValue određuje vrijednost će se koristiti u Izostanak parametar više specifične za dani okruženje.

>[AZURE.NOTE] Sve parametre instanca servisa su prikladna za konfiguraciju po okruženju. U gornjem primjeru LowKey i HighKey vrijednosti za usluge aktiviranja sheme izričito definiranih za sve instance servisa jer je raspon particija funkcija domene podataka, a ne u okruženju.


### <a name="per-environment-service-configuration-settings"></a>Postavke za konfiguriranje servisa po okruženju

U [modelu tkanina servisa](service-fabric-application-model.md) omogućuje servise za uključivanje konfiguracije paketa koje sadrže prilagođene parovima ključnih vrijednosti koje su čitljiv vrijeme izvođenja. Vrijednosti od tih postavki možete također se međusobno razlikuju po okruženju navođenjem na `ConfigOverride` u manifestu aplikacije.

Recimo da imate sljedeće postavke u datoteci Config\Settings.xml za na `Stateful1` servisa:


    <Section Name="MyConfigSection">
      <Parameter Name="MaxQueueSize" Value="25" />
    </Section>

Da biste nadjačali tu vrijednost za određenu aplikaciju/okruženje par, stvorite je `ConfigOverride` prilikom uvoza manifest servisa u manifestu aplikacije.

    <ConfigOverrides>
     <ConfigOverride Name="Config">
        <Settings>
           <Section Name="MyConfigSection">
              <Parameter Name="MaxQueueSize" Value="[Stateful1_MaxQueueSize]" />
           </Section>
        </Settings>
     </ConfigOverride>
  </ConfigOverrides>

Ovaj parametar pa mogu konfigurirati okruženje kao što je prikazano gore. To možete učiniti deklariranje u odjeljku parametara manifesta aplikacije i navođenjem okruženje određene vrijednosti u parametru datoteka aplikacije.

>[AZURE.NOTE] U slučaju konfiguracijske postavke servisa, postoje tri mjesta na kojem možete postaviti vrijednost odgovarajućeg ključa: konfiguracije paketa, programski manifest i parametar datoteke aplikacije. Servis tkanina uvijek će birati parametar datoteke aplikacije prvog (Ako je naveden), zatim programski manifest i na kraju paket konfiguracije.


### <a name="application-parameter-files"></a>Datoteka za parametar aplikacije

Projekt aplikacije servisa tkanina možete uključiti jednu ili više datoteka parametar aplikacije. Svaki od njih određuje određene vrijednosti za parametre koji su definirani u manifestu aplikacije:

    <!-- ApplicationParameters\Local.xml -->

    <Application Name="fabric:/Application1" xmlns="http://schemas.microsoft.com/2011/01/fabric">
        <Parameters>
            <Parameter Name ="Stateful1_MinReplicaSetSize" Value="2" />
            <Parameter Name="Stateful1_PartitionCount" Value="1" />
            <Parameter Name="Stateful1_TargetReplicaSetSize" Value="3" />
        </Parameters>
    </Application>

Prema zadanim postavkama, novu aplikaciju sadrži dvije datoteke parametar aplikacije, pod nazivom Local.xml i Cloud.xml:

![Datoteka aplikacije parametra u pregledniku rješenja][app-parameters-solution-explorer]

Da biste stvorili novu datoteku parametar, jednostavno kopirajte i zalijepite postojećeg i dajte naziv.

## <a name="identifying-environment-specific-parameters-during-deployment"></a>Označavanje parametara specifične za okruženje tijekom implementacije

Uvođenje trenutku ćete morati odaberite odgovarajući parametar datoteka da biste primijenili s aplikacijom. To možete učiniti putem dijaloški okvir Objavi u Visual Studio ili PowerShell.

### <a name="deploy-from-visual-studio"></a>Implementacija iz Visual Studio

Popis parametara dostupnih datoteka možete odabrati Kad objavite svoju aplikaciju u Visual Studio.

![Odaberite datoteku parametar u dijaloškom okviru Objavi][publishdialog]

### <a name="deploy-from-powershell"></a>Implementacija iz komponente PowerShell

Na `Deploy-FabricApplication.ps1` skriptu PowerShell obuhvaćeno predložak za aplikacije project prihvaća profila Objavi kao parametar i na PublishProfile sadrži referencu u datoteku parametara aplikacije.

  ```PowerShell
    ./Deploy-FabricApplication -ApplicationPackagePath <app_package_path> -PublishProfileFile <publishprofile_path>
  ```

## <a name="next-steps"></a>Daljnji koraci

Dodatne informacije o nekim osnovni koncepti koji se spominju u ovoj temi potražite u članku [servis tkanina Tehnički pregled](service-fabric-technical-overview.md). Informacije o mogućnostima upravljanja druge aplikacije koje su dostupne u Visual Studio, potražite u članku [Upravljanje tkanina servisa aplikacija u Visual Studio](service-fabric-manage-application-in-visual-studio.md).

<!-- Image references -->

[publishdialog]: ./media/service-fabric-manage-multiple-environment-app-configuration/publish-dialog-choose-app-config.png
[app-parameters-solution-explorer]:./media/service-fabric-manage-multiple-environment-app-configuration/app-parameters-in-solution-explorer.png
