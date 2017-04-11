<properties
   pageTitle="Razumijevanje tkanina servisa aplikacija i servisa sigurnosne pravilnike | Microsoft Azure"
   description="Pregled uputa za pokretanje aplikacije za servis tkanina u odjeljku sustav i lokalnih sigurnosnih računa, uključujući točke SetupEntry gdje potrebno aplikaciju akciju neke povlaštene prije pokretanja"
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
   ms.date="09/22/2016"
   ms.author="mfussell"/>

# <a name="configure-security-policies-for-your-application"></a>Konfiguriranje sigurnosnih pravilnika za aplikaciju
Azure tkanina servisa omogućuje sigurne aplikacije koje rade u skupini pod različitim korisničke račune. Servis tkanina secures i resursima koje koristi aplikacije u trenutku implementacije u odjeljku korisnički račun kao što su datoteke, mape i certifikati. Ovime pokrenute aplikacije, čak i u zajedničkoj glavnom računalu okruženju sigurne međusobno. 

Prema zadanim postavkama aplikacije servisa tkanina pokrenuti u odjeljku račun koji se pokreće postupak Fabric.exe u odjeljku. Servis tkanina pruža i mogućnost za pokretanje aplikacije u odjeljku lokalni korisnički račun ili račun za lokalni sustav, naveden manifestu aplikacije. Podržani lokalni sustav vrste računa za su **LocalUser**, **mrežna usluga sve**, **LocalService**i **LocalSystem**.

 Kada radi tkanina servisa Windows Server u centru za podatke pomoću samostalne instalacijskog programa, možete koristiti računa domene servisa Active Directory (AD).

Grupe korisnika mogu definirani i stvorili tako da možete dodati jedan ili više korisnika za svaku grupu zajedno upravlja. To je korisno kada postoji više korisnika za različite servisne ulazne točke, a koje su im potrebne da bi se neke uobičajene ovlasti koji su dostupni na razini grupe.

## <a name="configure-the-policy-for-service-setupentrypoint"></a>Konfiguriranje pravilnika za servis SetupEntryPoint

Kao što je opisano u [modelu](service-fabric-application-model.md) **SetupEntryPoint** je točka unosa povlaštene koji se izvodi s istim vjerodajnicama kao tkanina servisa (obično račun *mrežna usluga sve* ) prije Ulazna točka. Izvršna datoteka određen **Ulazna** obično je servis glavnog dugoročnih tako da se pojavljuju zasebnu instalaciju Ulazna točka izbjegava potrebe za pokretanje izvršna glavnog računala servisa s ovlastima visoke za prošireni vremenskog razdoblja. Izvršna datoteka određen **Ulazna** pokreće se nakon **SetupEntryPoint** zatvara uspješno. Dobivene postupak je nadzirati i ponovno pokrenuti, počevši od ponovno **SetupEntryPoint**ako ikad prekida ili ruši.

Slijedi primjer manifesta jednostavne servis koji pokazuje na SetupEntryPoint i glavni ulazna za servis.

~~~
<?xml version="1.0" encoding="utf-8" ?>
<ServiceManifest Name="MyServiceManifest" Version="SvcManifestVersion1" xmlns="http://schemas.microsoft.com/2011/01/fabric" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
  <Description>An example service manifest</Description>
  <ServiceTypes>
    <StatelessServiceType ServiceTypeName="MyServiceType" />
  </ServiceTypes>
  <CodePackage Name="Code" Version="1.0.0">
    <SetupEntryPoint>
      <ExeHost>
        <Program>MySetup.bat</Program>
        <WorkingFolder>CodePackage</WorkingFolder>
      </ExeHost>
    </SetupEntryPoint>
    <EntryPoint>
      <ExeHost>
        <Program>MyServiceHost.exe</Program>
      </ExeHost>
    </EntryPoint>
  </CodePackage>
  <ConfigPackage Name="Config" Version="1.0.0" />
</ServiceManifest>
~~~

### <a name="configure-the-policy-using-a-local-account"></a>Konfiguriranje pravila pomoću lokalni račun

Kada konfigurirate usluga da bi postavljanje Ulazna točka, možete promijeniti sigurnosnih dozvola koja se izvodi u odjeljku programski manifest. Primjer followin objašnjava konfiguriranje servisa za pokretanje u odjeljku korisnik administratorske ovlasti za račun.

~~~
<?xml version="1.0" encoding="utf-8"?>
<ApplicationManifest xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" ApplicationTypeName="MyApplicationType" ApplicationTypeVersion="1.0.0" xmlns="http://schemas.microsoft.com/2011/01/fabric">
   <ServiceManifestImport>
      <ServiceManifestRef ServiceManifestName="MyServiceTypePkg" ServiceManifestVersion="1.0.0" />
      <ConfigOverrides />
      <Policies>
         <RunAsPolicy CodePackageRef="Code" UserRef="SetupAdminUser" EntryPointType="Setup" />
      </Policies>
   </ServiceManifestImport>
   <Principals>
      <Users>
         <User Name="SetupAdminUser">
            <MemberOf>
               <SystemGroup Name="Administrators" />
            </MemberOf>
         </User>
      </Users>
   </Principals>
</ApplicationManifest>
~~~

Prvo, stvorite sekcije **Upravitelji** korisničko ime, kao što su SetupAdminUser. To označava da je korisnik član grupe administratora sustava.

Zatim u odjeljku **ServiceManifestImport** konfigurirati pravila da biste primijenili ovaj glavnicu **SetupEntryPoint**. Ovo govori servisa tkanina da prilikom pokretanja **MySetup.bat** datoteke mora biti RunAs s administratorskim ovlastima. Given da imate *ne* primjenjuje pravilnik točke glavni unos, kod u **MyServiceHost.exe** pokreće se u odjeljku sustav **mrežna usluga sve** računa. To je zadani račun pokrenute sve točke unosa servis kao.

Pogledajmo sada dodajte datoteke MySetup.bat projekta za Visual Studio da biste testirali administratorske ovlasti. U Visual Studio, desnom tipkom miša kliknite na projektu servisa, a zatim dodajte novu datoteku pod nazivom MySetup.bat.

Nakon toga provjerite je li datoteka MySetup.bat uvrštava u paketu za servis. Prema zadanim postavkama nije. Odaberite datoteku, desnom tipkom miša kliknite da biste dobili kontekstnog izbornika i odaberite **Svojstva**. U dijaloškom okviru svojstva provjerite je li da **kopirajte izlaz direktorij** postavljen da biste **kopirali ako novija**. Pogledajte sljedeće snimku zaslona.

![Visual Studio CopyToOutput za SetupEntryPoint batch datoteka][image1]

Sada otvorite datoteku MySetup.bat i dodajte sljedeće naredbe:

~~~
REM Set a system environment variable. This requires administrator privilege
setx -m TestVariable "MyValue"
echo System TestVariable set to > out.txt
echo %TestVariable% >> out.txt

REM To delete this system variable us
REM REG delete "HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Session Manager\Environment" /v TestVariable /f
~~~

Nakon toga Sastavljanje i implementacija rješenja lokalne razvoj klaster.  Kada je servis je počeo, kao što je prikazano u programu Explorer tkanina servisa, vidjet ćete na MySetup.bat uspio na dva načina. Otvorite naredbeni redak PowerShell i upišite:

~~~
PS C:\ [Environment]::GetEnvironmentVariable("TestVariable","Machine")
MyValue
~~~

Zatim, imajte na umu naziv čvor gdje servis je uveden i s radom u programu Explorer tkanina servisa, na primjer, čvor 2. Nakon toga pronađite mapu rad instanci aplikacije da biste pronašli datoteku out.txt koja prikazuje vrijednost **TestVariable**. Primjer ako je taj servis implementiran na čvor 2, a možete otvorite ovaj put za **MyApplicationType**:

~~~
C:\SfDevCluster\Data\_App\Node.2\MyApplicationType_App\work\out.txt
~~~

###  <a name="configure-the-policy-using-local-system-accounts"></a>Konfiguriranje pravila pomoću računa za lokalni sustav
Često se preporučuje da biste pokrenuli skriptu za pokretanje pomoću računa za lokalni sustav umjesto administratori račun kao što je prikazano ispred. Izvodi se RunAs pravila kao administratori obično ne funkcionira dobro jer računalima imati korisnički pristup kontrole (kontrolu korisničkih RAČUNA) po zadanom omogućena. U tim slučajevima **na preporuka je da se pokreće na SetupEntryPoint kao LocalSystem umjesto lokalni korisnik dodan grupe administratora**. Sljedeći primjer prikazuje postavljanje SetupEntryPoint da biste pokrenuli kao LocalSystem.

~~~
<?xml version="1.0" encoding="utf-8"?>
<ApplicationManifest xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" ApplicationTypeName="MyApplicationType" ApplicationTypeVersion="1.0.0" xmlns="http://schemas.microsoft.com/2011/01/fabric">
   <ServiceManifestImport>
      <ServiceManifestRef ServiceManifestName="MyServiceTypePkg" ServiceManifestVersion="1.0.0" />
      <ConfigOverrides />
      <Policies>
         <RunAsPolicy CodePackageRef="Code" UserRef="SetupLocalSystem" EntryPointType="Setup" />
      </Policies>
   </ServiceManifestImport>
   <Principals>
      <Users>
         <User Name="SetupLocalSystem" AccountType="LocalSystem" />
      </Users>
   </Principals>
</ApplicationManifest>
~~~

##  <a name="launch-powershell-commands-from-a-setupentrypoint"></a>Pokretanje naredbe ljuske PowerShell s na SetupEntryPoint
Da biste pokrenuli PowerShell od točke **SetupEntryPoint** , možete pokrenuti **PowerShell.exe** u batch datoteka koja upućuje na datoteku PowerShell. Najprije dodajte PowerShell datoteke u projekt servisa, kao što su **MySetup.ps1**. Imajte na umu da biste postavili svojstvo *kopirati ako novija* tako da datoteku i obuhvaćeno paket servisa. Sljedeći primjer prikazuje ogledne datoteke grupe da biste pokrenuli PowerShell datoteku s nazivom MySetup.ps1 kojim se određuje varijablu okruženja sustava, pod nazivom **TestVariable**.


MySetup.bat da biste pokrenuli PowerShell datoteke.

~~~
powershell.exe -ExecutionPolicy Bypass -Command ".\MySetup.ps1"
~~~

U datoteci PowerShell dodajte sljedeće da biste postavili varijabla okruženja sustava:

~~~
[Environment]::SetEnvironmentVariable("TestVariable", "MyValue", "Machine")
[Environment]::GetEnvironmentVariable("TestVariable","Machine") > out.txt
~~~

**Bilješke:** Prema zadanim postavkama kada se pokrene naredbena datoteka pretražuje aplikacije mapu pod nazivom **rad** za datoteke. U ovom slučaju, kada se pokrene MySetup.bat želimo da biste pronašli na MySetup.ps1 u istoj mapi, što je mapa **kod paketa** aplikacije. Da biste promijenili tu mapu postavite radnu mapu kao što je prikazano u nastavku.

~~~
<SetupEntryPoint>
    <ExeHost>
    <Program>MySetup.bat</Program>
    <WorkingFolder>CodePackage</WorkingFolder>
    </ExeHost>
</SetupEntryPoint>
~~~

## <a name="using-console-redirection-for-local-debugging"></a>Pomoću konzole za preusmjeravanje za lokalni ispravljanje pogrešaka
Ponekad je korisno potražite u članku konzola za izlaz iz pokretanje skripte za ispravljanje pogrešaka svrhe. Da biste mogli provesti to možete postaviti pravila preusmjeravanja konzole koja zapisuje izlaz datoteke. Izlazna datoteka zapisuje aplikacije mapu pod nazivom **zapisnika** na čvor gdje aplikacija je primijenjen i pokrenuti (pročitajte članak gdje možete pronaći to u prethodnom primjeru).

**Bilješke: Nikad ne** pomoću pravila preusmjeravanja konzole u uveden u radnog jer to može utjecati prebacivanje aplikacije aplikaciji. **Samo** ta postavka za lokalni razvoj i ispravljanje pogrešaka potrebe.  

Sljedeći primjer prikazuje postavljanje preusmjeravanja konzole s vrijednošću FileRetentionCount.

~~~
<SetupEntryPoint>
    <ExeHost>
    <Program>MySetup.bat</Program>
    <WorkingFolder>CodePackage</WorkingFolder>
    <ConsoleRedirection FileRetentionCount="10"/>
    </ExeHost>
</SetupEntryPoint>
~~~

Ako mijenjate sada MySetup.ps1 datoteka za pisanje i **Jeka** naredbi, to će pisati Izlazna datoteka za ispravljanje pogrešaka svrhe.

~~~
Echo "Test console redirection which writes to the application log folder on the node that the application is deployed to"
~~~

**Odmah nakon što ste debugged skriptu, uklonite ovo pravilo za preusmjeravanje konzole**

## <a name="configure-policy-for-service-code-packages"></a>Konfiguriranje pravilnika za pakete kod usluge 
U prethodnih koraka prikazivalo upute za primjenu pravila RunAs SetupEntryPoint. Pogledajmo malo dublju u kako stvoriti različite upravitelji koje se mogu primijeniti kao servis pravila.

### <a name="create-local-user-groups"></a>Stvaranje grupa lokalni korisnik
Grupe korisnika mogu definirani i koji omogućuju jednog ili više korisnika potrebno dodati u grupu. To je posebno korisno ako postoji više korisnika za različite servisne ulazne točke, a koje su im potrebne da bi se neke uobičajene ovlasti koji su dostupni na razini grupe. Sljedeći primjer prikazuje lokalne grupe naziva **LocalAdminGroup** s administratorskim ovlastima. Dva korisnika, Customer1 i Customer2, postaju članovi ove lokalne grupe.

~~~
<Principals>
 <Groups>
   <Group Name="LocalAdminGroup">
     <Membership>
       <SystemGroup Name="Administrators"/>
     </Membership>
   </Group>
 </Groups>
  <Users>
     <User Name="Customer1">
        <MemberOf>
           <Group NameRef="LocalAdminGroup" />
        </MemberOf>
     </User>
    <User Name="Customer2">
      <MemberOf>
        <Group NameRef="LocalAdminGroup" />
      </MemberOf>
    </User>
  </Users>
</Principals>
~~~

### <a name="create-local-users"></a>Stvaranje lokalne korisnike
Možete stvoriti lokalni korisnik koje je moguće koristiti radi zaštite servisa u aplikaciji. Kada vrstu računa **LocalUser** je navedena u odjeljku upravitelji manifesta aplikacije, servis tkanina stvara lokalne korisničke račune na računalima na kojima je implementiran aplikacije. Prema zadanom tih računa imaju iste nazive kao i onih koji su navedeni u manifestu aplikacije (na primjer, Customer3 u sljedeće uzorku). Umjesto toga se dinamički generiraju i izravnim lozinke.

~~~
<Principals>
  <Users>
     <User Name="Customer3" AccountType="LocalUser" />
  </Users>
</Principals>
~~~

<!-- If an application requires that the user account and password be same on all machines (for example, to enable NTLM authentication), the cluster manifest must set NTLMAuthenticationEnabled to true. The cluster manifest must also specify an NTLMAuthenticationPasswordSecret that will be used to generate the same password across all machines.

<Section Name="Hosting">
      <Parameter Name="EndpointProviderEnabled" Value="true"/>
      <Parameter Name="NTLMAuthenticationEnabled" Value="true"/>
      <Parameter Name="NTLMAuthenticationPassworkSecret" Value="******" IsEncrypted="true"/>
 </Section>
-->

### <a name="assign-policies-to-the-service-code-packages"></a>Dodjeljivanje pravilnika kod pakete usluga
U odjeljku **RunAsPolicy** **ServiceManifestImport** određuje računa iz odjeljka upravitelji koji želite koristiti da biste pokrenuli kod paket. I povezuje kod paketa iz manifesta servisa s korisničkim računima u odjeljku upravitelji. To možete navesti za postavljanje ili glavni unos točke ili možete odrediti `All` da biste primijenili na obje. Sljedeći primjer prikazuje različita pravila primijenjena:

~~~
<Policies>
<RunAsPolicy CodePackageRef="Code" UserRef="LocalAdmin" EntryPointType="Setup"/>
<RunAsPolicy CodePackageRef="Code" UserRef="Customer3" EntryPointType="Main"/>
</Policies>
~~~

Ako **EntryPointType** nije naveden, zadani je postavljeno na EntryPointType = "Glavne". Odredite **SetupEntryPoint** osobito je korisna kada želite pokrenuti određene postupak postavljanja velikim ovlastima u odjeljku račun sustava. U odjeljku donjem ovlasti račun može se pokrenuti kod stvarnog servisa.

### <a name="apply-a-default-policy-to-all-service-code-packages"></a>Zadani pravilnik Primijeni na sve pakete usluga kod
U odjeljku **DefaultRunAsPolicy** se koristi za određivanje zadani korisnički račun za sve pakete kod koji nemaju određene **RunAsPolicy** definirane. Ako veći dio paketa kod naveden u manifestu servis koristi aplikacija je potrebno za pokretanje u odjeljku korisniku, aplikacija možete definirati samo zadani pravilnik RunAs s tom korisničkom računu. Sljedeći primjer određuje ako paket kod **RunAsPolicy** naveden, paket kod trebale bi funkcionirati u odjeljku **MyDefaultAccount** navedene u odjeljku upravitelji.

~~~
<Policies>
  <DefaultRunAsPolicy UserRef="MyDefaultAccount"/>
</Policies>
~~~
### <a name="using-an-active-directory-domain-group-or-user"></a>Pomoću grupa domene servisa Active Directory ili korisnika
Za servis tkanina instalirano Windows Server samostalne instalacijski program, možete pokrenuti servis u odjeljku vjerodajnice za Active Directory (AD) računa korisnika ili grupe. Napomena: To je Active Directory na lokaciji u vašoj domeni i nije s Azure Active Directory (AAD). Korištenjem domene korisnika ili grupu, pa možete pristupiti na domenu (primjerice, zajedničke datoteke) ostale resurse koji ste dobili dozvole.

Sljedeći primjer prikazuje AD korisnik naziva *TestUser* sa svoje domene lozinke šifriran pomoću certifikata pod nazivom *MyCert*. Možete koristiti u `Invoke-ServiceFabricEncryptText` Powershell naredbu da biste stvorili tajnu snaga tekst. Pročitajte ovaj članak [Upravljanje tajne u aplikacijama servisa tkanina](service-fabric-application-secret-management.md) detalje o tome. Privatni ključ certifikata dešifrirati lozinku mora biti implementirano u lokalnom stroju u način grupiranje (u Azure to je putem upravitelja resursa). Pa kad servisa tkanina uvodi paket servisa s računalom, preporučuje se moći dešifrirati na tajna i uz korisničko ime uspješnoj AD za pokretanje u odjeljku vjerodajnice.

~~~
<Principals>
  <Users>
    <User Name="TestUser" AccountType="DomainUser" AccountName="Domain\User" Password="[Put encrypted password here using MyCert certificate]" PasswordEncrypted="true" />
  </Users>
</Principals>
<Policies>
  <DefaultRunAsPolicy UserRef="TestUser" />
  <SecurityAccessPolicies>
    <SecurityAccessPolicy ResourceRef="MyCert" PrincipalRef="TestUser" GrantRights="Full" ResourceType="Certificate" />
  </SecurityAccessPolicies>
</Policies>
<Certificates>
~~~


## <a name="assign-securityaccesspolicy-for-http-and-https-endpoints"></a>Dodijelite SecurityAccessPolicy za HTTP i HTTPS krajnje točke
Primjena pravila RunAs servisa, a manifest servisa deklarira krajnjoj točki resursi HTTP protokola, morate navesti **SecurityAccessPolicy** da bi bili priključke dodijeliti te krajnje točke pravilno-kontrolu pristupa za korisnički račun RunAs koja se pokreće servis u odjeljku na popisu. U suprotnom **http.sys** imaju pristup servisu i pronađite pogreške s pozivima putem klijentskog programa. Primjer followin primjenjuje račun Customer3 krajnje naziva **ServiceEndpointName**, je pružanje prava puni pristup.

~~~
<Policies>
   <RunAsPolicy CodePackageRef="Code" UserRef="Customer1" />
   <!--SecurityAccessPolicy is needed if RunAsPolicy is defined and the Endpoint is http -->
   <SecurityAccessPolicy ResourceRef="EndpointName" PrincipalRef="Customer1" />
</Policies>
~~~

Za krajnju točku HTTPS imate da biste naznačili naziv certifikata da biste se vratili na klijentu. To možete učiniti pomoću **EndpointBindingPolicy**certifikat koji je definiran u odjeljku potvrde u manifestu aplikacije.

~~~
<Policies>
   <RunAsPolicy CodePackageRef="Code" UserRef="Customer1" />
  <!--SecurityAccessPolicy is needed if RunAsPolicy is defined and the Endpoint is http -->
   <SecurityAccessPolicy ResourceRef="EndpointName" PrincipalRef="Customer1" />
  <!--EndpointBindingPolicy is needed if the EndpointName is secured with https -->
  <EndpointBindingPolicy EndpointRef="EndpointName" CertificateRef="Cert1" />
</Policies
~~~


## <a name="a-complete-application-manifest-example"></a>Primjer za manifesta dovršeno aplikacije
Sljedeće programski manifest prikazuje mnoge različite postavke:

~~~
<?xml version="1.0" encoding="utf-8"?>
<ApplicationManifest xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" ApplicationTypeName="Application3Type" ApplicationTypeVersion="1.0.0" xmlns="http://schemas.microsoft.com/2011/01/fabric">
   <Parameters>
      <Parameter Name="Stateless1_InstanceCount" DefaultValue="-1" />
   </Parameters>
   <ServiceManifestImport>
      <ServiceManifestRef ServiceManifestName="Stateless1Pkg" ServiceManifestVersion="1.0.0" />
      <ConfigOverrides />
      <Policies>
         <RunAsPolicy CodePackageRef="Code" UserRef="Customer1" />
         <RunAsPolicy CodePackageRef="Code" UserRef="LocalAdmin" EntryPointType="Setup" />
        <!--SecurityAccessPolicy is needed if RunAsPolicy is defined and the Endpoint is http -->
         <SecurityAccessPolicy ResourceRef="EndpointName" PrincipalRef="Customer1" />
        <!--EndpointBindingPolicy is needed the EndpointName is secured with https -->
        <EndpointBindingPolicy EndpointRef="EndpointName" CertificateRef="Cert1" />
     </Policies>
   </ServiceManifestImport>
   <DefaultServices>
      <Service Name="Stateless1">
         <StatelessService ServiceTypeName="Stateless1Type" InstanceCount="[Stateless1_InstanceCount]">
            <SingletonPartition />
         </StatelessService>
      </Service>
   </DefaultServices>
   <Principals>
      <Groups>
         <Group Name="LocalAdminGroup">
            <Membership>
               <SystemGroup Name="Administrators" />
            </Membership>
         </Group>
      </Groups>
      <Users>
         <User Name="LocalAdmin">
            <MemberOf>
               <Group NameRef="LocalAdminGroup" />
            </MemberOf>
         </User>
         <!--Customer1 below create a local account that this service runs under -->
         <User Name="Customer1" />
         <User Name="MyDefaultAccount" AccountType="NetworkService" />
      </Users>
   </Principals>
   <Policies>
      <DefaultRunAsPolicy UserRef="LocalAdmin" />
   </Policies>
   <Certificates>
     <EndpointCertificate Name="Cert1" X509FindValue="FF EE E0 TT JJ DD JJ EE EE XX 23 4T 66 "/>
  </Certificates>
</ApplicationManifest>
~~~


<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->
## <a name="next-steps"></a>Daljnji koraci

* [Objašnjenje modela aplikacija](service-fabric-application-model.md)
* [Određivanje resursa u manifest servisa](service-fabric-service-manifest-resources.md)
* [Implementacija aplikacije](service-fabric-deploy-remove-applications.md)

[image1]: ./media/service-fabric-application-runas-security/copy-to-output.png
