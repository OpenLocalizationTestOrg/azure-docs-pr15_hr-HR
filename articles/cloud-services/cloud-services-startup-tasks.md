<properties 
pageTitle="Pokretanje pokretanje zadataka u servise u Oblaku Azure | Microsoft Azure" 
description="Pokretanje zadaci pomoći pripremu okruženja servisa oblaka za aplikacije. To prilike koji prikazuje kako funkcionira zadaci prilikom pokretanja i kako ih" 
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



# <a name="how-to-configure-and-run-startup-tasks-for-a-cloud-service"></a>Kako konfigurirati i pokrenuti prilikom pokretanja zadataka za servise u oblaku

Pokretanje zadataka možete koristiti za izvođenje operacija prije pokretanja uloge. Operacije koje želite izvesti obuhvaćaju instaliranje komponente, Registracija COM komponente, postavku registra ili pokretanje procesa dugo izvodi.

>[AZURE.NOTE] Pokretanje zadaci nisu primjenjivi virtualnim strojevima, samo oblak servisa Web i uloge suradnika.

## <a name="how-startup-tasks-work"></a>Kako funkcionira prilikom pokretanja zadataka

Pokretanje zadaci trebaju akcije koje su poduzeti da bi se vaša uloga početka i su definirani u datoteci [ServiceDefinition.csdef] pomoću element [zadatka] unutar elementa [prilikom pokretanja] . Često su zadaci prilikom pokretanja datoteke grupe, ali mogu biti konzole za programe ili datoteke grupe koji se pokreću skripte komponente PowerShell.

Varijable okruženja prenositi informacije u pokretanje zadatka, a lokalno spremište koje se može koristiti za prosljeđivanje podataka iz zadatak pokretanja. Ako, na primjer, varijabla okruženja možete navesti put do program koji želite instalirati, a datoteke mogu biti snimljene lokalno spremište koja se zatim može čitati kasnije svoje uloge.

Pokretanje zadatka možete se prijaviti informacije i pogrešaka direktorija određen varijablu okruženja **TEMP** . Tijekom zadatka prilikom pokretanja varijablu okruženja **TEMP** razrješava *C:\\resursi\\temp\\[guid]. [ Naziv uloge]\\RoleTemp* direktorija kada se pokrene u oblak.

Pokretanje zadaci možete također može izvršiti nekoliko puta između ponovna. Na primjer, zadatak pokretanja će pokrenuti svaki put recycles ulogu i recycles uloga može obuhvaćati uvijek ponovno pokretanje. Pokretanje zadaci moraju biti napisani na način koji omogućuje da pokreću nekoliko puta bez poteškoća.

Pokretanje zadatke mora završavati programa **errorlevel** (ili izlazni kod) nula za pokretanje postupka da biste dovršili. Ako je zadatak pokretanja završava nastavkom od nule **errorlevel**, uloge neće se pokrenuti.


## <a name="role-startup-order"></a>Redoslijed pokretanja uloga

Slijedi popis postupak pokretanja uloga u Azure:

1. Instancu označen kao **Početni** i primanje promet.

2. Svi zadaci prilikom pokretanja se izvode prema svojim atribut **taskType** .
    - Zadaci koje ćete **jednostavno** se provode sinkrono, jedan po jedan.
    - **Pozadine** i **planu** zadatke započinju asinkrono, paralelni zadatak pokretanja.  
       
    > [AZURE.WARNING] IIS možda nije potpuno konfiguriran tijekom faze zadatak pokretanja u postupka pokretanja tako da se podaci specijalizirane možda neće biti dostupne. Pokretanje zadatke potrebne podatke za specijalizirane trebali biste koristiti [Microsoft.WindowsAzure.ServiceRuntime.RoleEntryPoint.OnStart](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.onstart.aspx).

3. Pokreće postupak ulogu glavnog računala i web-mjesto će se stvoriti u IIS-u.

4. Način [Microsoft.WindowsAzure.ServiceRuntime.RoleEntryPoint.OnStart](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.onstart.aspx) naziva.

5. Instancu je označena kao **spreman** i promet usmjeravanja instanci.

6. Način [Microsoft.WindowsAzure.ServiceRuntime.RoleEntryPoint.Run](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx) naziva.


## <a name="example-of-a-startup-task"></a>Primjer pokretanje zadatka

Pokretanje zadataka su definirani u datoteci [ServiceDefinition.csdef] element **zadatka** . Atribut **naredbenog retka** određuje naziv i parametre pri pokretanju batch datoteka ili konzole naredbe, atribut **ExecutionContextu** određuje Razina ovlasti pokretanje zadatka, a atribut **taskType** određuje koliko će se izvršavati zadatak.

U ovom se primjeru varijabla okruženja, **MyVersionNumber**se stvara za zadatak pokretanja i postavite vrijednost "**1.0.0.0**".

**ServiceDefinition.csdef**:

```xml
<Startup>
    <Task commandLine="Startup.cmd" executionContext="limited" taskType="simple" >
        <Environment>
            <Variable name="MyVersionNumber" value="1.0.0.0" />
        </Environment>
    </Task>
</Startup>
```

U sljedećem primjeru batch datoteka **Startup.cmd** piše u retku "trenutnu verziju je 1.0.0.0" StartupLog.txt datoteku u imeniku određen varijablu okruženja TEMP. Na `EXIT /B 0` redak osigurava zadatak pokretanja završava **errorlevel** nula.

```cmd
ECHO The current version is %MyVersionNumber% >> "%TEMP%\StartupLog.txt" 2>&1
EXIT /B 0
```

> [AZURE.NOTE] U Visual Studio, **kopirajte direktorij izlaz** svojstvo datoteke grupe za pokretanje treba biti postavljeno na **Uvijek Kopiraj** da biste provjerili je li vaš pokretanje batch datoteka pravilno implementirana u projekt na Azure (**approot\\smeće** za Web uloge i uloge suradnika na **approot** ).

## <a name="description-of-task-attributes"></a>Opis zadatka atributa

Atributi element **zadatka** u datoteci [ServiceDefinition.csdef] opisuje:

**naredbeni redak** – Time se određuje naredbeni redak za pokretanje zadatka:

- Naredbe, s parametrima neobavezno naredbenog retka koji počinje zadatak pokretanja.
- Često je naziv .cmd ili .bat batch datoteka.
- Zadatak je u odnosu na AppRoot\\smeće mapu za implementaciju. Prilikom određivanja put i datoteka na zadatak koji nije proširuju varijable okruženja. Ako je potrebno proširenja okruženju, možete stvoriti small .cmd skriptu koja se poziva pokretanje zadatka.
- Može biti aplikacije konzole ili batch datoteka koji će se pokrenuti [skriptu PowerShell](cloud-services-startup-tasks-common.md#create-a-powershell-startup-task).

**ExecutionContextu** - određuje Razina ovlasti za zadatak pokretanja. Razina ovlasti može biti ograničeno ili razinom:

- **Ograničeno**  
Zadatak pokretanja pokreće se s iste ovlasti kao uloga. Kada atribut **ExecutionContextu** za [vrijeme izvođenja] element nije **ograničena**, koristi se korisničke ovlasti.

- **povećani**  
Zadatak pokretanja pokreće se s administratorskim ovlastima. Time se omogućuje pokretanje zadaci koje ćete morati instalirati programe, unesite promjene konfiguracije IIS, izvođenje promjena u registru i druge administrator razine zadatke bez povećanju Razina ovlasti uloge sam.  

> [AZURE.NOTE] Razina ovlasti pokretanje zadatka ne moraju biti isti kao uloga sam.

**taskType** - određuje način na koji se izvršava zadatak pokretanja.

- **jednostavan**  
Zadaci se provode sinkronizirano, jedan po jedan, redoslijed naveden u datoteci [ServiceDefinition.csdef] . Kada jedan zadatak pokretanja **jednostavne** završava nastavkom **errorlevel** nula, izvršava se sljedeći **jednostavne** zadatak pokretanja. Ako nema više **jednostavne** pokretanja zadataka za izvršavanje, zatim uloga sam pokrenut će se.   

    > [AZURE.NOTE] **Jednostavan** zadatak završava nastavkom od nule **errorlevel**, instanci će se blokirati. Sljedeće **jednostavne** prilikom pokretanja zadataka i uloga sam neće se pokrenuti.

    Da biste osigurali naredbena datoteka završava **errorlevel** nula, izvršiti naredbu `EXIT /B 0` na kraju proces datoteke grupe.

- **pozadine**  
Zadaci se provode asinkrono, zajedno s početnom uloge.

- **planu**  
Zadaci se provode asinkrono, zajedno s početnom uloge. Ključne razlike između **prednji plan** i **Pozadina** zadatak je zadatak **planu** sprječava je uloga recikliranje ili isključuje dok je završen zadatak. **Pozadinske** zadatke ste to ograničenje.

## <a name="environment-variables"></a>Varijable okruženja

Varijable okruženja su način za prosljeđivanje podataka zadatak pokretanja. Na primjer, možete staviti put blob koji sadrži program da biste instalirali, ili brojevi priključka koji će koristiti vaša uloga ili postavke značajke kontrole zadatak pokretanja.

Postoje dvije vrste varijabli okruženja za pokretanje zadatke varijable statični okruženja i članovi klase [RoleEnvironment] na temelju varijabli okruženja. Obje su u odjeljku [okruženje] [ServiceDefinition.csdef] datoteke, a koristiti [varijable] atribut element i **naziv** .

Varijable statične okruženja koristi **vrijednost** atributa [varijable] element. Gornji primjer stvara varijabla okruženja **MyVersionNumber** koja ima statična vrijednost "**1.0.0.0**". Drugi primjer bio bi da biste stvorili varijabla okruženja **StagingOrProduction** koje možete postaviti i ručno vrijednosti "**pripremna**" ili "**radni**" da biste izvršili različite pokretanje akcije na temelju vrijednosti varijabla okruženja **StagingOrProduction** .

Članovi klase RoleEnvironment na temelju varijabli okruženja pomoću atribut **value** [varijable] elementa. Umjesto toga podređeni element [RoleInstanceValue] s odgovarajućim **XPath** atributa vrijednosti se koriste za stvaranje na temelju određenog člana klase [RoleEnvironment] varijabla okruženja. Vrijednosti za atribut **XPath** pristupa različite vrijednosti [RoleEnvironment] mogu pronaći [ovdje](cloud-services-role-config-xpath.md).



Ako, na primjer, da biste stvorili varijabla okruženja koja je "**true**" kada je instanci pokrenut u računalnim emulator i "**false**" kada se pokrene u oblak, koristite sljedeće elemente [varijabla] i [RoleInstanceValue] :

```xml
<Startup>
    <Task commandLine="Startup.cmd" executionContext="limited" taskType="simple">
        <Environment>
    
            <!-- Create the environment variable that informs the startup task whether it is running
                in the Compute Emulator or in the cloud. "%ComputeEmulatorRunning%"=="true" when
                running in the Compute Emulator, "%ComputeEmulatorRunning%"=="false" when running
                in the cloud. -->
    
            <Variable name="ComputeEmulatorRunning">
                <RoleInstanceValue xpath="/RoleEnvironment/Deployment/@emulated" />
            </Variable>
    
        </Environment>
    </Task>
</Startup>
```

## <a name="next-steps"></a>Daljnji koraci
Saznajte kako izvođenja nekih [uobičajenih zadataka pokretanje](cloud-services-startup-tasks-common.md) s servisom u Oblaku.

[Paket](cloud-services-model-and-package.md) servis u Oblaku.  


[ServiceDefinition.csdef]: cloud-services-model-and-package.md#csdef
[Zadatak]: https://msdn.microsoft.com/library/azure/gg557552.aspx#Task
[Pokretanje]: https://msdn.microsoft.com/library/azure/gg557552.aspx#Startup
[Prilikom izvođenja]: https://msdn.microsoft.com/library/azure/gg557552.aspx#Runtime
[Okruženje]: https://msdn.microsoft.com/library/azure/gg557552.aspx#Environment
[Varijabla]: https://msdn.microsoft.com/library/azure/gg557552.aspx#Variable
[RoleInstanceValue]: https://msdn.microsoft.com/library/azure/gg557552.aspx#RoleInstanceValue
[RoleEnvironment]: https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleenvironment.aspx