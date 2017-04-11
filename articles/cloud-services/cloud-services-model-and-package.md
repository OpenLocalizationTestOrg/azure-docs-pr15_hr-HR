<properties
    pageTitle="Što je servis u Oblaku modela i paket | Microsoft Azure"
    description="Opisuje oblaka servisa model (.csdef, .cscfg) i paket (.cspkg) u Azure"
    services="cloud-services"
    documentationCenter=""
    authors="Thraka"
    manager="timlt"
    editor=""/>
<tags
    ms.service="cloud-services"
    ms.workload="tbd"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/06/2016"
    ms.author="adegeo"/>

# <a name="what-is-the-cloud-service-model-and-how-do-i-package-it"></a>Što je model servis u Oblaku i kako je paket?
Servis u oblaku stvara se od tri komponente, definicija servisa _(.csdef)_, config servisa _(.cscfg)_i paket servisa _(.cspkg)_. **ServiceDefinition.csdef** i **ServiceConfig.cscfg** koji se temelji na XML ili datoteka opisuju strukturu servis u oblaku i kako je konfiguriran; Skupno naziva modela. **ServicePackage.cspkg** je zip datoteka koju je generiran iz **ServiceDefinition.csdef** i između ostalog, sadrži sve potrebne ovisnosti u binarni. Azure stvara neki servis u oblaku **ServicePackage.cspkg** i **ServiceConfig.cscfg**.

Kada servis u oblaku radi u Azure, možete je konfigurirajte pomoću datoteke **ServiceConfig.cscfg** , ali ne možete mijenjati definiciju.

## <a name="what-would-you-like-to-know-more-about"></a>Što želite Saznajte više o?

* Želim saznati više o [ServiceDefinition.csdef](#csdef) i [ServiceConfig.cscfg](#cscfg) datoteke.
* Već informirati, Daj mi [primjere](#next-steps) na što se može konfigurirati.
* Želite li stvoriti [ServicePackage.cspkg](#cspkg).
* Koristim Visual Studio i želim...
    * [Stvaranje nove servise u oblaku][vs_create]
    * [Konfigurirajte postojeće oblaku][vs_reconfigure]
    * [Implementacija projekta u Oblaku][vs_deploy]
    * [Udaljena radna površina u instanca servisa u oblaku][remotedesktop]

<a name="csdef"></a>
## <a name="servicedefinitioncsdef"></a>ServiceDefinition.csdef
Datoteka **ServiceDefinition.csdef** određuje postavke koje koriste Azure da biste konfigurirali servis u oblaku. [Azure servisa definicija sheme (.csdef datoteka)](https://msdn.microsoft.com/library/azure/ee758711.aspx) nudi dozvoljenu oblik datoteka za definiciju servisa. Sljedeći primjer prikazuje postavke koje se može se definirati za Web, a tempiranja uloge:

```xml
<?xml version="1.0" encoding="utf-8"?>
<ServiceDefinition name="MyServiceName" xmlns="http://schemas.microsoft.com/ServiceHosting/2008/10/ServiceDefinition">
  <WebRole name="WebRole1" vmsize="Medium">
    <Sites>
      <Site name="Web">
        <Bindings>
          <Binding name="HttpIn" endpointName="HttpIn" />
        </Bindings>
      </Site>
    </Sites>
    <Endpoints>
      <InputEndpoint name="HttpIn" protocol="http" port="80" />
      <InternalEndpoint name="InternalHttpIn" protocol="http" />
    </Endpoints>
    <Certificates>
      <Certificate name="Certificate1" storeLocation="LocalMachine" storeName="My" />
    </Certificates>
    <Imports>
      <Import moduleName="Connect" />
      <Import moduleName="Diagnostics" />
      <Import moduleName="RemoteAccess" />
      <Import moduleName="RemoteForwarder" />
    </Imports>
    <LocalResources>
      <LocalStorage name="localStoreOne" sizeInMB="10" />
      <LocalStorage name="localStoreTwo" sizeInMB="10" cleanOnRoleRecycle="false" />
    </LocalResources>
    <Startup>
      <Task commandLine="Startup.cmd" executionContext="limited" taskType="simple" />
    </Startup>
  </WebRole>

  <WorkerRole name="WorkerRole1">
    <ConfigurationSettings>
      <Setting name="DiagnosticsConnectionString" />
    </ConfigurationSettings>
    <Imports>
      <Import moduleName="RemoteAccess" />
      <Import moduleName="RemoteForwarder" />
    </Imports>
    <Endpoints>
      <InputEndpoint name="Endpoint1" protocol="tcp" port="10000" />
      <InternalEndpoint name="Endpoint2" protocol="tcp" />
    </Endpoints>
  </WorkerRole>
</ServiceDefinition>
```

Može se odnositi na [] [servisa definicija sheme] za bolje razumjeli XML shema koja se koristi u nastavku, no Evo brzog objašnjenje nekih elemenata:

**Web-mjesta**  
Sadrži definicije za web-mjesta ili web-aplikacije koje se nalaze u IIS7.

**InputEndpoints**  
Sadrži definicije za krajnje točke koji se koriste za kontakt servisa u oblaku.

**InternalEndpoints**  
Sadrži definicije za krajnje točke koje koriste uloga instance možete komunicirati s međusobno povezani.

**ConfigurationSettings**  
Sadrži definicije postavku za značajke određene uloge.

**Certifikati**  
Sadrži definicije za potvrde koje su vam potrebne za uloge. U prethodnom primjeru kod prikazuje certifikat koji se koristi za konfiguraciju Azure povezivanje.

**LocalResources**  
Sadrži definicije za resurse lokalno spremište. Lokalno spremište resurs je rezervirane direktorija u datotečnom sustavu sustava virtualnog računala u kojem se izvodi instance komponente uloge.

**Uvozi**  
Sadrži definicije za uvezene module. U prethodnom primjeru kod prikazuje module za udaljene radne površine i povezivanje Azure.

**Pokretanje**  
Sadrži zadataka koji se izvode kada se pokrene ulogu. Zadaci se definiraju .cmd ili izvršne datoteke.



<a name="cscfg"></a>
## <a name="serviceconfigurationcscfg"></a>ServiceConfiguration.cscfg
Konfiguriranje postavki za servis u oblaku ovisi o vrijednosti u datoteci **ServiceConfiguration.cscfg** . Navedite koliko je instanci koje želite uvesti za svaku ulogu u ovoj datoteci. Konfiguracijska datoteka servisa dodaje vrijednosti za konfiguracijske postavke koje ste definirali u datoteci definicije servisa. Thumbprints za sve upravljanje certifikate koji su povezani sa servisom cloud dodaju u datoteku. [Azure servisa konfiguracije shema (.cscfg datoteka)](https://msdn.microsoft.com/library/azure/ee758710.aspx) nudi dozvoljenu oblik datoteke za konfiguraciju servisa.

Konfiguracijska datoteka servisa je isporučen aplikacije, ali se prenosi Azure kao zasebne datoteke i služi za konfiguriranje servisa u oblaku. Nova datoteka konfiguracije servisa možete prenijeti, a da pritom ne redeploying servis u oblaku. Konfiguriranje vrijednosti za servis u oblaku se mogu mijenjati dok se izvodi servis u oblaku. Sljedeći primjer prikazuje konfiguracijske postavke koje se može se definirati za Web, a tempiranja uloge:

```xml
<?xml version="1.0"?>
<ServiceConfiguration serviceName="MyServiceName" xmlns="http://schemas.microsoft.com/ServiceHosting/2008/10/ServiceConfiguration">
  <Role name="WebRole1">
    <Instances count="2" />
    <ConfigurationSettings>
      <Setting name="SettingName" value="SettingValue" />
    </ConfigurationSettings>

    <Certificates>
      <Certificate name="CertificateName" thumbprint="CertThumbprint" thumbprintAlgorithm="sha1" />
      <Certificate name="Microsoft.WindowsAzure.Plugins.RemoteAccess.PasswordEncryption"
         thumbprint="CertThumbprint" thumbprintAlgorithm="sha1" />
    </Certificates>
  </Role>
</ServiceConfiguration>
```

Može se odnositi na [Shemu konfiguracija servisa](https://msdn.microsoft.com/library/azure/ee758710.aspx) za bolje razumijevanje XML shema koja se koristi u nastavku, no Evo brzog objašnjenje elemenata:

**Instance**  
Konfigurira broj pokrenute instance uloge. Da biste spriječili potencijalno tijekom nadogradnje postaje dostupan servis u oblaku, nije preporučeno implementacije više instanci uloga na web-mjesto. Time se adhering smjernice u na [Azure izračunati servisa razinu ugovor (SLA)](http://azure.microsoft.com/support/legal/sla/), koje jamčiti 99.95% vanjskih povezivanje za mjesto na Internetu uloge kada dva ili više instanci uloga uvode se za uslugu.

**ConfigurationSettings**  
Konfigurira postavke pokrenute instance za uloge. Naziv u `<Setting>` elemenata moraju se podudarati definicije postavku u datoteci definicije servisa.

**Certifikati**  
Konfigurira certifikate koji se koriste servis. U prethodnom primjeru kod prikazuje kako definirati certifikata za modul za daljinski pristup. Vrijednost atributa *otisak prsta* mora biti postavljeno na otisak prsta potvrde da biste koristili.

<p/>

 >[AZURE.NOTE] Otisak prsta certifikata mogu dodati konfiguracijska datoteka pomoću programa za uređivanje teksta ili vrijednosti koje se mogu dodati na kartici **potvrde** na stranici **Svojstva** uloga u Visual Studio.



## <a name="defining-ports-for-role-instances"></a>Definiranje priključke za slučajeve uloga
Azure omogućuje samo jedna stavka točke u ulogu web. To znači da pojavljuje li se sve promet po jednu IP adresu. Možete konfigurirati web-mjesta da biste zajednički koristili priključak konfiguriranjem zaglavlje glavno računalo za usmjeravanje zahtjev na odgovarajuće mjesto. Možete konfigurirati i aplikacije da biste preslušali poznati priključaka na IP adresa.

Sljedeći primjer prikazuje konfiguracije web uloge s web-mjesta i web-aplikacije. Na web-mjesto je konfiguriran kao zadano mjesto unosa na priključak 80, a na web-aplikacije konfigurirani tako da prima zahtjeve iz programa zamjensko glavno računalo zaglavlja koja se zove "mail.mysite.cloudapp.net".

```xml
<WebRole>
  <ConfigurationSettings>
    <Setting name="DiagnosticsConnectionString" />
  </ConfigurationSettings>
  <Endpoints>
    <InputEndpoint name="HttpIn" protocol="http" <mark>port="80"</mark> />
    <InputEndpoint name="Https" protocol="https" port="443" certificate="SSL"/>
    <InputEndpoint name="NetTcp" protocol="tcp" port="808" certificate="SSL"/>
  </Endpoints>
  <LocalResources>
    <LocalStorage name="Sites" cleanOnRoleRecycle="true" sizeInMB="100" />
  </LocalResources>
  <Site name="Mysite" packageDir="Sites\Mysite">
    <Bindings>
      <Binding name="http" endpointName="HttpIn" />
      <Binding name="https" endpointName="Https" />
      <Binding name="tcp" endpointName="NetTcp" />
    </Bindings>
  </Site>
  <Site name="MailSite" packageDir="MailSite">
    <Bindings>
      <Binding name="mail" endpointName="HttpIn" <mark>hostheader="mail.mysite.cloudapp.net"</mark> />
    </Bindings>
    <VirtualDirectory name="artifacts" />
    <VirtualApplication name="storageproxy">
      <VirtualDirectory name="packages" packageDir="Sites\storageProxy\packages"/>
    </VirtualApplication>
  </Site>
</WebRole>
```


## <a name="changing-the-configuration-of-a-role"></a>Promjena konfiguracije uloga
Konfiguriranje servis u oblaku možete ažurirati dok se izvodi u Azure, bez poduzimanja servis izvanmrežno. Da biste promijenili podatke o konfiguraciji, možete prenijeti nove datoteke za konfiguraciju ili uređivanje konfiguracijska datoteka na mjestu i primijenite ga na servisu izvodi. Sljedeće promjene može unijeti konfiguraciju servisa:

- **Promjenom vrijednosti postavke konfiguracije**  
Kada konfiguracije promjene postavki, uloga instance možete odabrati da biste primijenili promjene dok je instanca na Internetu ili koša za instancu bez poteškoća i primjenu promjena tijekom instanci je izvan mreže.

- **Promjena topologije servisne instanci uloga**  
Promjene topologije ne utječe na pokrenute instance, osim gdje se uklanja instance. Sve preostale instance obično ne moraju biti brisanja; Međutim, možete odabrati koša za slučajeve uloga u odgovoru promjena topologije.

- **Promjena otisak prsta certifikata**  
Certifikat možete ažurirati samo kad je izvan mreže uloga instance. Certifikat je dodati, izbrisati ili promijeniti dok je instanca komponente uloga u mreži, Azure obavljanje vodi instanci izvanmrežno ažurirati certifikat, a zatim ga donesete mrežni način nakon promjene.

### <a name="handling-configuration-changes-with-service-runtime-events"></a>Promjene konfiguracije rukovanja s servisa Runtime događajima
[Biblioteka za vrijeme izvođenja Azure](https://msdn.microsoft.com/library/azure/mt419365.aspx) sadrži prostor naziva [Microsoft.WindowsAzure.ServiceRuntime](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.aspx) , koji pruža klase za interakciju okruženje za Azure s kod koji se izvodi u instance komponente uloge. Klase [RoleEnvironment](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleenvironment.aspx) definira sljedeće događaje koje su potenciju prije i poslije promjene konfiguracije:

- **[Promjena](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleenvironment.changing.aspx) događaja**  
To se događa prije primjene promjena konfiguracije na navedeni instancu uloge dodjeljivanja moći zapisivati instance uloga po potrebi.
- **[Promijenjeni](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleenvironment.changed.aspx) događaja**  
Pojavljuje se nakon promjene konfiguracije primjenjuje se na navedeni pojavljivanja uloge.

> [AZURE.NOTE] Jer certifikat promjene uvijek izvanmrežni pojavljivanja uloge, oni podići RoleEnvironment.Changing ili RoleEnvironment.Changed događaja.

<a name="cspkg"></a>
## <a name="servicepackagecspkg"></a>ServicePackage.cspkg
Za implementaciju aplikacije kao neki servis u oblaku u Azure, najprije morate paketa aplikacije u odgovarajućem obliku. Da biste stvorili datoteku paketa kao zamjena za Visual Studio možete koristiti alat naredbenog retka **CSPack** (koje se instaliraju pomoću [Azure SDK](https://azure.microsoft.com/downloads/)).

**CSPack** koristi sadržaj datoteka za definiciju usluge i usluge konfiguracijska datoteka da biste definirali sadržaj paketa. **CSPack** generira aplikacije paketa datoteku (.cspkg) koje možete prenijeti na Azure pomoću [portala za Azure](cloud-services-how-to-create-deploy-portal.md#create-and-deploy). Prema zadanim postavkama, značajke pakiranja pod nazivom `[ServiceDefinitionFileName].cspkg`, ali možete odrediti drugi naziv pomoću na `/out` mogućnost **CSPack**.

**CSPack** se obično nalazi u  
`C:\Program Files\Microsoft SDKs\Azure\.NET SDK\[sdk-version]\bin\`

>[AZURE.NOTE]
CSPack.exe (u sustavu windows) dostupna tako da pokrenete prečac za **Microsoft Azure naredbeni redak** koje se instaliraju pomoću SDK-a.  
>  
>Pokrenite CSPack.exe program tako da potražite u dokumentaciji o sve moguće parametara i naredbi.

<p />

>[AZURE.TIP]
Lokalno pokrenuti servis u oblaku u **Microsoft Azure izračunati Emulator**, upotrijebite mogućnost **/copyonly** tu mogućnost kopira binarne datoteke za aplikaciju rasporeda direktorija iz kojeg se može pokrenuti u emulator računalnim.

### <a name="example-command-to-package-a-cloud-service"></a>Primjer naredbe za pakiranje servis u oblaku
Sljedeći primjer stvara Aplikacijski paket koji sadrži informacije o ulogama za web. Naredba određuje datoteku definicije servisa da biste koristili, direktorija gdje binarne datoteke možete pronaći, a naziv datoteke paketa.

    cspack [DirectoryName]\[ServiceDefinition]
           /role:[RoleName];[RoleBinariesDirectory]
           /sites:[RoleName];[VirtualPath];[PhysicalPath]
           /out:[OutputFileName]

Ako aplikacija sadrži web uloge i uloge suradnika, koristit će se sljedeću naredbu:

    cspack [DirectoryName]\[ServiceDefinition]
           /out:[OutputFileName]
           /role:[RoleName];[RoleBinariesDirectory]
           /sites:[RoleName];[VirtualPath];[PhysicalPath]
           /role:[RoleName];[RoleBinariesDirectory];[RoleAssemblyName]

Gdje varijabli su definirana na sljedeći način:

| Varijabla                  | Vrijednost |
| ------------------------- | ----- |
| \[Direktorija\]         | Poddirektorij u korijenskom direktoriju projekta s datotekom .csdef Azure projekta.|
| \[ServiceDefinition\]     | Naziv datoteke definicije servisa. Prema zadanim postavkama, tu datoteku pod nazivom ServiceDefinition.csdef.  |
| \[OutputFileName\]        | Naziv datoteke generirani paketa. Obično je to postavljeno na naziv aplikacije. Ako je naveden naziv datoteke, Aplikacijski paket stvara kao \[ApplicationName\].cspkg.|
| \[Naziv uloge\]              | Naziv uloge onako kako su definirana u datoteci definicije servisa.|
| \[RoleBinariesDirectory] | Mjesto binarne datoteke za ulogu.|
| \[VirtualPath\]           | Fizička direktorija za svaki virtualnog puta definirano u odjeljku web-mjesta servisa definicije.|
| \[PhysicalPath\]          | Fizička direktorija sadržaja za svaku virtualnog puta definirano u čvor web-mjesta servisa definicije.|
| \[RoleAssemblyName\]      | Naziv binarne datoteke za ulogu.| 


## <a name="next-steps"></a>Daljnji koraci

Mogu se stvaranje paketa u oblak i želim...

* [Postavljanje udaljene radne površine za instancu servisa u oblaku][remotedesktop]
* [Implementacija projekta u Oblaku][deploy]

Koristim Visual Studio i želim...

* [Stvaranje nove servise u oblaku][vs_create]
* [Konfigurirajte postojeće oblaku][vs_reconfigure]
* [Implementacija projekta u Oblaku][vs_deploy]
* [Postavljanje udaljene radne površine za instancu servisa u oblaku][vs_remote]

[deploy]: cloud-services-how-to-create-deploy-portal.md
[remotedesktop]: cloud-services-role-enable-remote-desktop.md
[vs_remote]: ../vs-azure-tools-remote-desktop-roles.md
[vs_deploy]: ../vs-azure-tools-cloud-service-publish-set-up-required-services-in-visual-studio.md
[vs_reconfigure]: ../vs-azure-tools-configure-roles-for-cloud-service.md
[vs_create]: ../vs-azure-tools-azure-project-create.md
