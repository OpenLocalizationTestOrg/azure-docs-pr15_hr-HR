<properties
   pageTitle="Implementacija postojeće izvršnu datoteku tkanina servisa Azure | Microsoft Azure"
   description="Vodič za pakiranje postojeće aplikacije kao gost izvršni, tako da mogu biti implementirano u servis tkanina klaster"
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
   ms.workload="na"
   ms.date="10/22/2016"
   ms.author="msfussell;mikhegn"/>

# <a name="deploy-a-guest-executable-to-service-fabric"></a>Implementacija gost izvršna tkanina servisa

Bilo koje vrste aplikacija, kao što su node.js, jezika Java ili izvornim aplikacijama možete pokrenuti u tkanina servisa Azure. Servis tkanina se odnosi na ove vrste aplikacija kao gost izvršne datoteke.
Izvršne datoteke goste tretira se tkanina servisa kao što su bez praćenja stanja servisa. Zbog toga ih smještaju se na čvorove klasteru, ovisno o dostupnosti i druge metriku. U ovom se članku opisuje kako pakiranje i uvođenje gost izvršne datoteke na servis tkanina klaster pomoću Visual Studio ili naredbenog retka za.

U ovom se članku ćemo objasniti korake za pakiranje gost izvršna i Implementacija servisa tkanina.  

## <a name="benefits-of-running-a-guest-executable-in-service-fabric"></a>Pogodnosti gost izvršnu u tkanina servisa

Nekoliko je prednosti koje se isporučuju uz bavljenje gost izvršna klasteru tkanina servisa:

- Visoke dostupnosti. Programi koji se izvode na servis tkanina su dostupne visoko. Servis tkanina osigurava da se izvode instance programa.
- Nadzor stanja. Praćenje stanja servisa tkanina otkriva Ako aplikaciju radi i pruža informacije Dijagnostika ako postoji pogreška.   
- Upravljanje aplikacijama životni ciklus. Osim kojoj nadogradnje bez nedostupnost, tkanina servisa omogućuje automatsko vraćanje na prethodnu verziju ako postoji neispravni stanja događaja prijavljenih tijekom nadogradnje.    
- Gustoća. Više aplikacija možete pokrenuti u skupini koji nema potrebe za svaku aplikaciju da biste pokrenuli na vlastitu hardveru.


## <a name="overview-of-application-and-service-manifest-files"></a>Pregled aplikacija i servisa manifesta datoteke

Implementacija izvršna gost, dio je korisno za razumijevanje pakiranje tkanina servisa i model implementacije kao što je opisano [modelu](service-fabric-application-model.md). Pakiranje modela servisa tkanina ovisi o dva XML datoteke: manifesti aplikacija i servisa. Definicija sheme za datoteke ApplicationManifest.xml i ServiceManifest.xml instaliran je uz tkanina SDK servisa u *C:\Programske datoteke\Microsoft SDKs\Service Fabric\schemas\ServiceFabricServiceModel.xsd*.

* **Manifestu aplikacije** Programski manifest se opisuju aplikacije. Popisuje servise koje sastavite i drugi parametri koji se koriste za definiranje kako jedne ili više usluga mora biti implementirano, kao što su broj instanci.

  U tkanina servisa aplikacija je jedinica i nadogradnja. Aplikacije mogu biti nadograđeni kao jedna jedinica gdje se upravlja potencijalne pogreške i potencijalne vraćanja na staro stanje. Servis tkanina jamčiti da je postupak nadogradnje bilo uspješno, ili, ako je nadogradnja ne uspije, ne ostavite aplikacije u stanju Nepoznato nestabilan.

* **Servis manifesta** Servis manifest opisuju komponente servisa. Obuhvaća podatke, primjerice naziv i vrstu servisa, i njegov kod, konfiguriranje i podataka. Servis manifest sadrži i neke dodatne parametre koje je moguće koristiti za konfiguriranje servisa kada je u upotrebi.


## <a name="application-package-file-structure"></a>Struktura datoteke paketa aplikacije
Da biste implementirali aplikaciju servisa tkanina, aplikaciju treba slijedite strukturu unaprijed definirane direktorija. U sljedećem primjeru te strukture.

```
|-- ApplicationPackageRoot
  	|-- GuestService1Pkg
        |-- Code
            |-- existingapp.exe
        |-- Config
            |-- Settings.xml
        |-- Data
        |-- ServiceManifest.xml
  	|-- ApplicationManifest.xml
```

U ApplicationPackageRoot s datotekom ApplicationManifest.xml koji definira aplikacija. Poddirektorij za svaki servis uključeni u aplikaciji za sadrže sve na artefakte koju ovaj servis zahtijeva – u ServiceManifest.xml, i obično sljedeća tri direktorija:

- *Kod*. Taj direktorij sadrži kod usluge.
- *Konfiguracija*. Taj direktorij sadrži datoteku Settings.xml (i druge datoteke ako je potrebno) koji servis možete pristupiti u vrijeme izvođenja dohvatiti određene konfiguracijske postavke.
- *Podatke*. Ovo je dodatne direktorija za pohranu dodatne lokalnih podataka koji servis možda će biti potrebno. Napomena: Podataka trebali biste koristiti da biste pohranili samo efemerni podatke. Servis tkanina ne Kopiraj/replicirati promjene u direktoriju podataka ako servis mora biti je premještena – na primjer, tijekom prebacivanje.

Napomena: Ne morate stvoriti u `config` i `data` direktorija ako vam nisu potrebni.

## <a name="packaging-an-existing-executable"></a>Pakiranje postojeće izvršna datoteka

Kada pakiranje izvršnu datoteku za goste, možete odabrati bilo da biste koristili predložak projekta za Visual Studio ili [ručno stvaranje paketa aplikacije](#manually). Koristite Visual Studio, strukture paketa aplikacije i datotekama manifesta stvaraju u čarobnjak za novi projekt umjesto vas.

>[AZURE.NOTE] Najjednostavniji način za pakiranje na postojeće izvršna u servisa Windows je pomoću programa Visual Studio.

## <a name="using-visual-studio-to-package-an-existing-executable"></a>Pakiranje postojeće izvršne datoteke pomoću programa Visual Studio

Visual Studio nudi predloška servisa tkanina servisa za pomoć prilikom implementacije gost izvršne datoteke na servis tkanina klaster.

Otvorite kroz sljedeće korake da biste dovršili objavljivanje:

1. Odaberite Datoteka -> novi projekt i stvaranje servisne aplikacije za tkanina.
2. Odaberite goste izvršnu datoteku kao predložak servisa.
3. Kliknite Pregledaj da biste odaberite mapu s vašeg izvršnu datoteku, a zatim ispunite ostatak parametara da biste stvorili servis.
    - *Ponašanje kod paket* možete postaviti da biste kopirali sav sadržaj mape u Visual Studio projekt, koji je korisno ako ne mijenja izvršnu datoteku. Ako očekujete izvršnu datoteku da biste promijenili te želite mogućnost dinamički obraditi novi izgradi, možete odabrati da biste se povezali mapu umjesto toga. Imajte na umu da povezane mape možete koristiti prilikom stvaranja aplikacije projekta u Visual Studio. Ova veza na izvorno mjesto iz u projektu, što omogućuje ažuriranje goste izvršne datoteke u odredište izvora pojavljuju te ažuriranja postaju dio Aplikacijski paket na Sastavi.
    - *Program* – odaberite izvršnu datoteku koju trebale bi funkcionirati pokrenuti servis.
    - *Argumenti* - navesti argumente koji treba proslijediti izvršnu datoteku. Možda ćete popis parametara uz argumente.
    - *WorkingFolder* - određuje radni direktorij za postupak koji će se pokrenuti. Možete navesti tri vrijednosti:
        - `CodeBase`Određuje radni direktorij će biti postavljena na imenik kod u Aplikacijski paket (`Code` imeničkog u strukturi datoteka prikazano ispred).
        - `CodePackage`Određuje radni direktorij će biti postavljena na korijenu Aplikacijski paket (`GuestService1Pkg` u strukturi datoteka prikazano ispred).
        - `Work`Određuje da datoteke spremaju se u poddirektorij pod nazivom rad
4. Imenujte uslugu pa kliknite u redu.
5. Ako na servisu mora krajnje točke za komunikaciju, sada možete dodati protokol, priključcima i vrsta ServiceManifest.xml datoteku. e.g. `<Endpoint Name="NodeAppTypeEndpoint" Protocol="http" Port="3000" UriScheme="http" PathSuffix="myapp/" Type="Input" />`.
6. Sada možete pomoću značajke pakiranja i objaviti akcija na temelju vaše lokalne klaster tako što ćete rješenja u Visual Studio za ispravljanje pogrešaka. Kada budete spremni možete objaviti aplikacije na udaljenom klaster ili rješenje izvora kontrole za prijavu.
7. Idite na kraju ovog članka da biste vidjeli kako prikazati goste izvršnim usluge pokrenut u programu Explorer tkanina servisa.

<a id="manually"></a>
## <a name="manually-packaging-and-deploying-an-existing-executable"></a>Ručno pakiranje i implementacija postojeće izvršna datoteka
Postupak ručno pakiranje izvršnu datoteku goste temelji se na sljedeće korake:

1. Stvorite strukturu direktorija paketa.
2. Dodavanje aplikacije web-mjesto kod i u okvir za konfiguraciju datoteka.
3. Uređivanje datoteke manifesta servisa.
4. Uređivanje datoteke manifesta aplikacije.

<!--
>[AZURE.NOTE] We do provide a packaging tool that allows you to create the ApplicationPackage automatically. The tool is currently in preview. You can download it from [here](http://aka.ms/servicefabricpacktool).
-->

### <a name="create-the-package-directory-structure"></a>Stvaranje strukture direktorija paketa
Možete početi stvaranjem strukturu direktorija, kao što je opisano ranije.

### <a name="add-the-applications-code-and-configuration-files"></a>Dodavanje aplikacije web-mjesto kod i u okvir za konfiguraciju datoteka
Kada stvorite strukturu direktorija, možete dodati kod aplikacije i konfiguracija datoteka u odjeljku kod i config direktorija. Možete stvoriti i dodatne direktorija ili direktorijima u odjeljku kod ili config direktorija.

Servis tkanina ne programa xcopy sadržaja korijenski direktorij aplikacije pa nema unaprijed definiranu strukturu da biste koristili osim stvaranje dva gornji direktorija, kod i postavke. (Možete odabrati različite nazive po želji. Dodatne pojedinosti nalaze se u sljedećem odjeljku.)

>[AZURE.NOTE] Provjerite jeste li uključili sve datoteke/ovisnosti koju je potrebno aplikacije. Servis tkanina kopira sadržaj Aplikacijski paket na sve čvorove u skupini gdje se premještaju u aplikacijske usluge uvesti. Paket mora sadržavati kod koji je aplikacija potrebno za pokretanje. Ne preporučujemo uz pretpostavku da je već instalirana ovisnosti.

### <a name="edit-the-service-manifest-file"></a>Uređivanje datoteke manifesta servisa
Sljedeći je korak da biste uredili datoteku manifesta servisa da biste uključili sljedeće informacije:

- Naziv vrste usluge. To je ID-a koji servis tkanina koristi za prepoznavanje servisa.
- Naredba za pokretanje aplikacije (ExeHost).
- Bilo koji skriptu koju je potrebno za pokretanje da biste postavili gore/Konfiguriranje aplikacije (SetupEntrypoint).

Slijedi primjer na `ServiceManifest.xml` datoteke:

```xml
<?xml version="1.0" encoding="utf-8"?>
<ServiceManifest xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" Name="NodeApp" Version="1.0.0.0" xmlns="http://schemas.microsoft.com/2011/01/fabric">
   <ServiceTypes>
      <StatelessServiceType ServiceTypeName="NodeApp" UseImplicitHost="true"/>
   </ServiceTypes>
   <CodePackage Name="code" Version="1.0.0.0">
      <SetupEntryPoint>
         <ExeHost>
             <Program>scripts\launchConfig.cmd</Program>
         </ExeHost>
      </SetupEntryPoint>
      <EntryPoint>
         <ExeHost>
            <Program>node.exe</Program>
            <Arguments>bin/www</Arguments>
            <WorkingFolder>CodePackage</WorkingFolder>
         </ExeHost>
      </EntryPoint>
   </CodePackage>
   <Resources>
      <Endpoints>
         <Endpoint Name="NodeAppTypeEndpoint" Protocol="http" Port="3000" Type="Input" />
      </Endpoints>
   </Resources>
</ServiceManifest>
```

Prođimo kroz različite dijelove datoteke koje su vam potrebne da biste ažurirali:

#### <a name="updating-the-servicetypes"></a>Ažuriranje na ServiceTypes

```xml
<ServiceTypes>
  <StatelessServiceType ServiceTypeName="NodeApp" UseImplicitHost="true" />
</ServiceTypes>
```

- Možete odabrati bilo koji naziv koji želite za `ServiceTypeName`. Vrijednost se koristi u na `ApplicationManifest.xml` datoteke za otkrivanje servisa.
- Morate navesti `UseImplicitHost="true"`. Taj atribut govori tkanina servis koji servis temelji se na samostalnih aplikacije, pa je sve tkanina servisa treba učiniti za pokretanje kao proces i nadzor njegov stanja.

#### <a name="updating-the-codepackage"></a>Ažuriranje na CodePackage
CodePackage element određuje mjesto (i verzija) kod usluge.

```xml
<CodePackage Name="Code" Version="1.0.0.0">
```

Na `Name` element se koristi za određivanje naziv direktorija u Aplikacijski paket koji sadrži kod usluge. `CodePackage`ima u `version` atribut. Može se koristiti za određivanje verzije kod – i potencijalno i može se koristiti za Nadogradnja kod usluge pomoću servisa tkanina aplikacije životni ciklus upravljanje infrastrukture.
#### <a name="optional-updating-the-setupentrypoint"></a>Neobavezno: Ažuriranje na SetupEntrypoint

```xml
<SetupEntryPoint>
   <ExeHost>
       <Program>scripts\launchConfig.cmd</Program>
   </ExeHost>
</SetupEntryPoint>
```
SetupEntrypoint element se koristi za određivanje izvršne datoteke ili grupe datoteka koje je potrebno izvršiti prije pokretanja servisa kod. Neobavezan korak je da ga ne morate uvrstiti ako nema Inicijalizacija/postavljati. Na SetupEntryPoint izvršava se svaki put kada je ponovno pokrenuti servis.

Postoji samo jedan SetupEntrypoint da skripte instalacije i konfiguracije morate grupirane u datoteci jedne grupe ako zahtijeva više skripta za instalaciju config aplikacije. Na SetupEntrypoint možete izvršiti bilo koju vrstu datoteka – izvršne datoteke, datoteke grupe i cmdleta ljuske PowerShell. Dodatne informacije potražite u članku [Konfiguriranje SetupEntryPoint](service-fabric-application-runas-security.md).

U prethodnom primjeru, u SetupEntrypoint će se pokrenuti skupnu datoteku pod nazivom `LaunchConfig.cmd` koji se nalazi u na `scripts` njegov kod direktorija (pod pretpostavkom WorkingFolder element postavljen na CodeBase).

#### <a name="updating-the-entrypoint"></a>Ažuriranje Ulazna

```xml
<EntryPoint>
  <ExeHost>
    <Program>node.exe</Program>
    <Arguments>bin/www</Arguments>
    <WorkingFolder>CodeBase</WorkingFolder>
  </ExeHost>
</EntryPoint>
```

Na `Entrypoint` element u datoteci manifesta servis koristi da biste odredili kako pokrenuti servis. Na `ExeHost` element određuje izvršne datoteke (i argumente) koji želite koristiti da biste pokrenuli servis.

- `Program`određuje naziv izvršnu datoteku koju je potrebno izvršiti pokrenuti servis.
- `Arguments`Određuje argumenti koji treba proslijediti izvršnu datoteku. Možda ćete popis parametara uz argumente.
- `WorkingFolder`Određuje radni direktorij za postupak koji će se pokrenuti. Možete navesti tri vrijednosti:
    - `CodeBase`Određuje radni direktorij će biti postavljena na imenik kod u Aplikacijski paket (`Code` imeničkog u prethodnom struktura datoteke).
    - `CodePackage`Određuje radni direktorij će biti postavljena na korijenu Aplikacijski paket (`GuestService1Pkg` u prethodnom struktura datoteke).
  - `Work`Određuje da datoteke spremaju se u poddirektorij pod nazivom rad

Na WorkingFolder koristan je da biste postavili ispravnu radni direktorij tako da se aplikacija ili pokretanje skripte mogu koristiti relativni putovi.

#### <a name="updating-the-endpoints-and-registering-with-naming-service-for-communication"></a>Ažuriranje krajnjih točaka i registrirate za servis dodjele naziva za komunikaciju

```xml
<Endpoints>
   <Endpoint Name="NodeAppTypeEndpoint" Protocol="http" Port="3000" Type="Input" />
</Endpoints>

```
U prethodnom primjeru u `Endpoint` element određuje krajnje točke koje možete osluškuju aplikacije. U ovom primjeru aplikacije Node.js očekuje podatke http na priključak 3000.

Osim toga možete zatražiti tkanina servisa da biste objavili ovaj krajnje točke na servis dodjele naziva tako da druge servise mogu otkriti adresu krajnja točka za taj servis. To vam omogućuje da moći komunikaciju između servisa koji su izvršne datoteke za goste.
Adrese objavljenog krajnje je obrasca `UriScheme://IPAddressOrFQDN:Port/PathSuffix`. `UriScheme`i `PathSuffix` su neobavezno atribute. `IPAddressOrFQDN`je IPAdresa ili potpuno kvalificirani naziv domene čvora ovaj izvršnu datoteku postavlja se na i izračunati umjesto vas.

U sljedećem primjeru kada je servis implementiran, u programu Explorer tkanina servisa vidite krajnje slično `http://10.1.4.92:3000/myapp/` objavljene instanca servisa ili ako je na lokalno računalo prikazat `http://localhost:3000/myapp/`. 

```xml
<Endpoints>
   <Endpoint Name="NodeAppTypeEndpoint" Protocol="http" Port="3000"  UriScheme="http" PathSuffix="myapp/" Type="Input" />
</Endpoints>
```
Možete koristiti te adrese s [povratnog proxy](service-fabric-reverseproxy.md) za komunikaciju između servisa.

### <a name="edit-the-application-manifest-file"></a>Uređivanje datoteke manifesta aplikacije

Nakon što ste konfigurirali u `Servicemanifest.xml` datoteku, morate izvršiti neke promjene da biste na `ApplicationManifest.xml` datoteku da biste bili sigurni da se koriste vrsta točan servisa i naziv.

```xml
<?xml version="1.0" encoding="utf-8"?>
<ApplicationManifest xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" ApplicationTypeName="NodeAppType" ApplicationTypeVersion="1.0" xmlns="http://schemas.microsoft.com/2011/01/fabric">
   <ServiceManifestImport>
      <ServiceManifestRef ServiceManifestName="NodeApp" ServiceManifestVersion="1.0.0.0" />
   </ServiceManifestImport>
</ApplicationManifest>
```

#### <a name="servicemanifestimport"></a>ServiceManifestImport

U na `ServiceManifestImport` element, možete odrediti jedan ili više usluga koje želite uvrstiti u aplikaciji. Usluge referencirani s `ServiceManifestName`, koji određuje naziv direktorija gdje u `ServiceManifest.xml` se nalazi datoteka.

```xml
<ServiceManifestImport>
  <ServiceManifestRef ServiceManifestName="NodeApp" ServiceManifestVersion="1.0.0.0" />
</ServiceManifestImport>
```

## <a name="set-up-logging"></a>Postavljanje zapisivanje
Za goste izvršne datoteke, je korisno moći vidjeti konzole zapisnika da biste saznali jesu li skripte aplikacije i konfiguraciji navedene sve pogreške.
Preusmjeravanje konzole moguće je konfigurirati u na `ServiceManifest.xml` datoteku pomoću na `ConsoleRedirection` element.

```xml
<EntryPoint>
  <ExeHost>
    <Program>node.exe</Program>
    <Arguments>bin/www</Arguments>
    <WorkingFolder>CodeBase</WorkingFolder>
    <ConsoleRedirection FileRetentionCount="5" FileMaxSizeInKb="2048"/>
  </ExeHost>
</EntryPoint>
```

* `ConsoleRedirection`možete se koristi prilikom preusmjeravanja konzole izlazna (stdout i stderr) radni direktorij pa se poslužite da biste potvrdili da postoje pogreške tijekom postavljanja ili izvođenja aplikacije klaster tkanina servisa.

    * `FileRetentionCount`određuje koliko se datoteke spremaju u imeniku rad. Vrijednosti 5, ako, primjerice, znači da zapisničke datoteke za prethodni pet izvršavanja pohranjuju u imeniku rad.
    * `FileMaxSizeInKb`određuje veličinu Maks datoteka zapisnika.

Zapisničke datoteke spremaju se u jednom od direktorija rad na servis. Da biste odredili gdje se nalaze datoteke, morate koristiti servis tkanina Explorer da biste utvrdili koji čvor servis pokrenut, a koristi se koji radni imenik. Ovaj postupak opisana su u nastavku ovog članka.

## <a name="deployment"></a>Uvođenje
Posljednji korak je za implementaciju aplikacije. Sljedeću skriptu komponente PowerShell pokazuje kako implementirati aplikaciju klaster lokalne razvoj i pokretanje novog servisa tkanina servisa.

```PowerShell

Connect-ServiceFabricCluster localhost:19000

Write-Host 'Copying application package...'
Copy-ServiceFabricApplicationPackage -ApplicationPackagePath 'C:\Dev\MultipleApplications' -ImageStoreConnectionString 'file:C:\SfDevCluster\Data\ImageStoreShare' -ApplicationPackagePathInImageStore 'nodeapp'

Write-Host 'Registering application type...'
Register-ServiceFabricApplicationType -ApplicationPathInImageStore 'nodeapp'

New-ServiceFabricApplication -ApplicationName 'fabric:/nodeapp' -ApplicationTypeName 'NodeAppType' -ApplicationTypeVersion 1.0

New-ServiceFabricService -ApplicationName 'fabric:/nodeapp' -ServiceName 'fabric:/nodeapp/nodeappservice' -ServiceTypeName 'NodeApp' -Stateless -PartitionSchemeSingleton -InstanceCount 1

```
Servis za servis tkanina može uvesti u različitim "konfiguracijama." Ako, primjerice, može se uvesti kao jedan ili više instanci ili može uvesti na način da postoji jedna instanca servisa na svakom čvor klaster tkanina servisa.

Na `InstanceCount` parametar u `New-ServiceFabricService` cmdlet se koristi da biste odredili koliko je instanci servisa treba moguće pokrenuti servis tkanina klasteru. Možete postaviti na `InstanceCount` vrijednost, ovisno o vrsti aplikacije koje su implementacija. Dva najčešće scenarija:

* `InstanceCount = "1"`. U ovom slučaju samo jedna instanca servisa je uveden u klasteru. Raspored servisa tkanina određuje koje čvor će biti implementirano na servis.

* `InstanceCount ="-1"`. U ovom slučaju pojava servisa je implementiran na svakoj čvor u skupini tkanina servisa. Rezultat ima jedan (i samo jedan) instanca servisa za svaki čvor u klasteru.

Ovo je korisno konfiguracije za sučelja aplikacije (na primjer, krajnje REST) jer morate "povezivanje" na bilo koju od čvorove u skupini da biste koristili krajnju točku klijentske aplikacije. Konfiguraciju i može se koristiti kada, na primjer, sve čvorove klaster tkanina servisa povezani s raspoređivača opterećenja tako promet klijenta možete raspodijeliti servis koji se izvodi na sve čvorove u klasteru.

## <a name="check-your-running-application"></a>Provjera pokrenuti program

U programu Explorer tkanina servisa, odredite čvor na kojem se pokreće servis. U ovom primjeru se izvršava na Node1:

![Čvor u kojem se pokreće servis](./media/service-fabric-deploy-existing-app/nodeappinsfx.png)

Ako otvorite čvor i pronađite aplikaciju, vidjet ćete ključna čvor podatke, uključujući mjesta na disku.

![Mjesto na disku](./media/service-fabric-deploy-existing-app/locationondisk2.png)

Ako Pregledavanje imenika pomoću programa Explorer Server, možete pronaći radni direktorij i mapa na servis zapisnika kao u sljedećem slike.

![Mjesto zapisnika](./media/service-fabric-deploy-existing-app/loglocation.png)


## <a name="next-steps"></a>Daljnji koraci
U ovom se članku ste naučili kako pakiranje izvršnu datoteku za goste i Implementacija servisa tkanina. Kao sljedeći korak, preporučujemo da pročitate dodatni sadržaj ove teme.

- [Uzorka za pakiranje i implementacija gost izvršnu na GitHub](https://github.com/Azure-Samples/service-fabric-dotnet-getting-started/tree/master/GuestExe/SimpleApplication), uključujući veze na predizdanja alata za pakiranje
- [Implementacija više goste izvršne datoteke](service-fabric-deploy-multiple-apps.md)
- [Stvaranje prve aplikacije servisa tkanina pomoću Visual Studio](service-fabric-create-your-first-application-in-visual-studio.md)
