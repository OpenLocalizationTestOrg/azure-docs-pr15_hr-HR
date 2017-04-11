<properties
   pageTitle="Implementacija Node.js aplikaciji koja koristi MongoDB | Microsoft Azure"
   description="Vodič za pakiranje više goste izvršne datoteke za implementaciju sustava tkanina servisa Azure klaster"
   services="service-fabric"
   documentationCenter=".net"
   authors="msfussell"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="10/22/2016"
   ms.author="msfussell;mikhegn"/>


# <a name="deploy-multiple-guest-executables"></a>Implementacija više goste izvršne datoteke

U ovom se članku objašnjava pakiranje i implementacija više izvršne datoteke goste tkanina servisa Azure. Za stvaranje i implementacija jedan paket servisa tkanina pročitajte kako [implementirati izvršne datoteke na servis tkanina gost](service-fabric-deploy-existing-app.md).

Dok je ovaj vodič pokazuje kako implementirati aplikaciju s sučelje Node.js koji koristi MongoDB kao spremišta podataka, korake možete primijeniti na bilo koju aplikaciju koja sadrži ovisnosti na neku drugu aplikaciju.   

Koristite Visual Studio čime se dobiva Aplikacijski paket koji sadrži više goste izvršne datoteke. U odjeljku [Pomoću programa Visual Studio pakiranje postojeće aplikacije](service-fabric-deploy-existing-app.md#using-visual-studio-to-package-an-existing-executable). Nakon dodavanja prvog izvršnu datoteku za goste, desnom tipkom miša kliknite projekt aplikacije, a zatim odaberite **Dodaj -> novi servis tkanina servisa** da biste dodali drugi projekt izvršna goste rješenje. Napomena: Ako odlučite da biste se povezali izvor u projekta za Visual Studio, stvaranje rješenja za Visual Studio će provjerite nalazi li se Aplikacijski paket u tijeku s promjenama u izvorišnog web-mjesta. 

## <a name="manually-package-the-multiple-guest-executable-application"></a>Ručno paket više goste Izvršna aplikacija
Umjesto toga možete ručno paket izvršna gosta. Za ručno pakiranje, u ovom se članku koristi alat za pakiranje tkanina servisa koja je dostupna na [http://aka.ms/servicefabricpacktool](http://aka.ms/servicefabricpacktool).

### <a name="packaging-the-nodejs-application"></a>Pakiranje Node.js aplikacije
U ovom se članku pretpostavlja da Node.js nije instaliran na čvorove u skupini tkanina servisa. Sljedeći consequence, morate dodati Node.exe u korijenskom direktoriju aplikacije čvor prije pakiranju. Strukturu direktorija Node.js aplikacije (pomoću Express web framework i Jade predložak modul) trebao bi izgledati slično onome u nastavku:

```
|-- NodeApplication
  	|-- bin
        |-- www
  	|-- node_modules
        |-- .bin
        |-- express
        |-- jade
        |-- etc.
  	|-- public
        |-- images
        |-- etc.
  	|-- routes
        |-- index.js
        |-- users.js
  	|-- views
        |-- index.jade
        |-- etc.
  	|-- app.js
  	|-- package.json
  	|-- node.exe
```

Kao sljedeći korak, stvaranje paketa aplikacije za aplikaciju Node.js. Ispod kod stvara servisa tkanina Aplikacijski paket koji sadrži Node.js aplikacije.

```
.\ServiceFabricAppPackageUtil.exe /source:'[yourdirectory]\MyNodeApplication' /target:'[yourtargetdirectory] /appname:NodeService /exe:'node.exe' /ma:'bin/www' /AppType:NodeAppType
```

U nastavku je opis parametara koji se koriste:

- **/Source** upućuje na imenik aplikacije u kojoj se treba pakirati.
- **/Target** definira direktoriju u kojem treba stvorili paketa. Taj imenik ne može se razlikovati od izvorišni direktorij.
- **/appname** definira naziv aplikacije postojeće aplikacije. Važno je da razumijete to prevodi naziv usluge u manifestu, ali ne naziv aplikacije servisa tkanina.
- **/exe** definira izvršnu datoteku koju tkanina servisa bi trebao da biste pokrenuli, u ovom slučaju `node.exe`.
- **/ma** definira argument koji se koristi za pokretanje izvršnu datoteku. Kao što je Node.js nije instaliran, tkanina servis nije potrebno pokretanje web-poslužitelj Node.js izvršavanjem `node.exe bin/www`.  `/ma:'bin/www'`nalaže pakiranje alat za korištenje `bin/ma` kao argument za node.exe.
- **/AppType** definira upišite naziv aplikacije servisa tkanina.

Ako pregledavate direktorij naveden u parametru /target, moći ćete vidjeti alat je stvorio potpuno funkcionalnu paket tkanina servisa kao što je prikazano u nastavku:

```
|--[yourtargetdirectory]
  	|-- NodeApplication
        |-- C
              |-- bin
              |-- data
              |-- node_modules
              |-- public
              |-- routes
              |-- views
              |-- app.js
              |-- package.json
              |-- node.exe
        |-- config
              |--Settings.xml
        |-- ServiceManifest.xml
  	|-- ApplicationManifest.xml
```
Generirani ServiceManifest.xml sada sadrži odjeljak koji opisuje kako web-poslužitelj Node.js treba pokrene, kao što je prikazano u nastavku isječak koda:

```xml
<CodePackage Name="C" Version="1.0">
    <EntryPoint>
        <ExeHost>
            <Program>node.exe</Program>
            <Arguments>'bin/www'</Arguments>
            <WorkingFolder>CodePackage</WorkingFolder>
        </ExeHost>
    </EntryPoint>
</CodePackage>
```
U ovom primjeru web-poslužitelj Node.js očekuje podatke s priključkom 3000, pa ćete morati ažurirati podatke o krajnjoj točki u datoteci ServiceManifest.xml kao što je prikazano u nastavku.   

```xml
<Resources>
      <Endpoints>
        <Endpoint Name="NodeServiceEndpoint" Protocol="http" Port="3000" Type="Input" />
      </Endpoints>
</Resources>
```
### <a name="packaging-the-mongodb-application"></a>Pakiranje MongoDB aplikacije
Sad kad ste pakirat Node.js aplikacije, možete nastaviti i pakiranje MongoDB. Kao što je rečeno prije sada proći kroz korake nisu specifične za Node.js i MongoDB. Zapravo, one se odnose na sve aplikacije koje se trebaju pakirati zajedno kao jedan tkanina servisa aplikacija.  

Pakiranje MongoDB želite provjerite je li paket Mongod.exe i Mongo.exe. Oba binarne datoteke nalaze se u na `bin` imeničkog imenik MongoDB instalacije. Strukturu direktorija izgleda slično onome u nastavku.

```
|-- MongoDB
  	|-- bin
        |-- mongod.exe
        |-- mongo.exe
        |-- anybinary.exe
```
Servis tkanina treba pokrenuti MongoDB pomoću naredbe koja je slična onoj ispod, pa ćete morati koristiti u `/ma` parametar kada pakiranje MongoDB.

```
mongod.exe --dbpath [path to data]
```
> [AZURE.NOTE] Podaci se koja se sačuvati u slučaju pogreške čvor ako odgoditi direktoriju podataka MongoDB na lokalni direktorij čvor. Trebali biste koristiti durable prostora za pohranu ili implementirati MongoDB replike postaviti da biste spriječili gubitak podataka.  

U ili naredbe ljuske PowerShell, ne možemo pokrenuti alat za pakiranje pomoću sljedećih parametara:

```
.\ServiceFabricAppPackageUtil.exe /source: [yourdirectory]\MongoDB' /target:'[yourtargetdirectory]' /appname:MongoDB /exe:'bin\mongod.exe' /ma:'--dbpath [path to data]' /AppType:NodeAppType
```

Da biste dodali na servis tkanina Aplikacijski paket MongoDB, morate da biste bili sigurni da upućuje na isti direktorija koji već sadrži aplikaciju za parametar /target manifesta uz Node.js aplikacije. Morate i da biste bili sigurni da koristite isti naziv ApplicationType.

Pogledajmo dođite do imenika i pregledajte koji je stvorio alat.

```
|--[yourtargetdirectory]
  	|-- MyNodeApplication
  	|-- MongoDB
        |-- C
            |--bin
                |-- mongod.exe
                |-- mongo.exe
                |-- etc.
        |-- config
            |--Settings.xml
        |-- ServiceManifest.xml
  	|-- ApplicationManifest.xml
```
Kao što vidite, alat za dodati novu mapu, MongoDB, direktorij koji sadrži binarne datoteke MongoDB. Ako otvorite na `ApplicationManifest.xml` datoteku, vidjet ćete da paketa sada sadrži Node.js aplikacije i MongoDB. Kod u nastavku prikazuje se sadržaj manifesta aplikacije.

```xml
<ApplicationManifest xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" ApplicationTypeName="MyNodeApp" ApplicationTypeVersion="1.0" xmlns="http://schemas.microsoft.com/2011/01/fabric">
   <ServiceManifestImport>
      <ServiceManifestRef ServiceManifestName="MongoDB" ServiceManifestVersion="1.0" />
   </ServiceManifestImport>
   <ServiceManifestImport>
      <ServiceManifestRef ServiceManifestName="NodeService" ServiceManifestVersion="1.0" />
   </ServiceManifestImport>
   <DefaultServices>
      <Service Name="MongoDBService">
         <StatelessService ServiceTypeName="MongoDB">
            <SingletonPartition />
         </StatelessService>
      </Service>
      <Service Name="NodeServiceService">
         <StatelessService ServiceTypeName="NodeService">
            <SingletonPartition />
         </StatelessService>
      </Service>
   </DefaultServices>
</ApplicationManifest>  
```

### <a name="publishing-the-application"></a>Aplikacija za objavljivanje
Posljednji korak je za objavljivanje aplikacija lokalne klaster tkanina servisa putem skripti ljuske PowerShell:

```
Connect-ServiceFabricCluster localhost:19000

Write-Host 'Copying application package...'
Copy-ServiceFabricApplicationPackage -ApplicationPackagePath '[yourtargetdirectory]' -ImageStoreConnectionString 'file:C:\SfDevCluster\Data\ImageStoreShare' -ApplicationPackagePathInImageStore 'NodeAppType'

Write-Host 'Registering application type...'
Register-ServiceFabricApplicationType -ApplicationPathInImageStore 'NodeAppType'

New-ServiceFabricApplication -ApplicationName 'fabric:/NodeApp' -ApplicationTypeName 'NodeAppType' -ApplicationTypeVersion 1.0  
```

Kada aplikacija uspješno objavljen lokalne klaster, možete pristupiti Node.js aplikaciju na priključak koji smo ste unijeli u manifestu servisa aplikacije Node.js – na primjer http://localhost:3000.

U ovom ćete praktičnom vodiču ste vidjeti kako jednostavno pakiranje dva postojeće aplikacije kao jedan tkanina servisa aplikacija. Ste naučili i kako implementirati servis tkanina tako da se može koristiti iz neke od značajki tkanina servisa, kao što su visoke dostupnosti i Integracija sa sustavom stanja.

## <a name="next-steps"></a>Daljnji koraci

- Informirajte se o implementaciji spremnika [tkanina servisa i spremnike](service-fabric-containers-overview.md) pregled
