<properties
    pageTitle="Uloge web i tempiranja Python s Visual Studio | Microsoft Azure"
    description="Pregled korištenja Python alata za Visual Studio stvaranje servisa Azure oblaka uključujući web uloge i uloge suradnika."
    services="cloud-services"
    documentationCenter="python"
    authors="thraka"
    manager="timlt"
    editor=""/>

<tags
    ms.service="cloud-services"
    ms.workload="tbd"
    ms.tgt_pltfrm="na"
    ms.devlang="python"
    ms.topic="hero-article"
    ms.date="08/03/2016"
    ms.author="adegeo"/>


# <a name="python-web-and-worker-roles-with-python-tools-for-visual-studio"></a>Uloge web i tempiranja Python pomoću alata za Python za Visual Studio

Ovaj članak sadrži pregled korištenja Python web tempiranja uloge i pomoću [Alata za Python za Visual Studio][]. Će Saznajte kako pomoću programa Visual Studio za stvaranje i implementacija neki osnovni servis u Oblaku koja koristi Python.

## <a name="prerequisites"></a>Preduvjeti

 - Visual Studio 2013 ili 2015.
 - [Python alate za Visual Studio][] (PTVS)
 - [Alati za Azure SDK za dodavanje veze za VANJSKIH 2013][] ili [Azure SDK Alati za dodavanje veze za VANJSKIH 2015.][]
 - [Python 2.7 32-bitnu][] ili [Python 3.5 32-bitne][]

[AZURE.INCLUDE [create-account-and-websites-note](../../includes/create-account-and-websites-note.md)]

## <a name="what-are-python-web-and-worker-roles"></a>Što su Python web i tempiranja uloge?

Azure pruža tri izračunati modela za pokretanje aplikacije: [značajka web-aplikacije u aplikacije servisa za Azure][execution model-web sites], [virtualnim računalima sustava Azure][execution model-vms], i [Servise u Oblaku Azure][execution model-cloud services]. Sve tri modeli podržavaju Python. Servisi u oblaku, što obuhvaća web- a tempiranja uloge nude *platformu kao Service (PaaS)*. U oblaku, uloga web daje namjenski web-poslužitelj Internet Information Services (IIS) glavno računalo sučelja web-aplikacije, dok ulogu suradnika možete pokrenuti asinkronog, dugoročnih ili trajna zadataka ovisi o interakcije s korisnikom ili unos.

Dodatne informacije potražite u članku [što je servis u Oblaku?].

> [AZURE.NOTE]*Looking da biste sastavili jednostavne web-mjesto?*
Ako vaš scenarij obuhvaća samo jednostavne web-mjesto sučelja, razmislite o korištenju laganih značajka web-aplikacije u aplikacije servisa za Azure. Možete jednostavno nadograditi na servis u Oblaku kao što je rastom web-mjesta i promjena svojim potrebama. Potražite u <a href="/develop/python/">Centru za razvojne inženjere Python</a> članke koji pokrivaju razvoja značajka web-aplikacije u aplikacije servisa za Azure.
<br />


## <a name="project-creation"></a>Stvaranje projekta

U Visual Studio, možete odabrati **Azure u Oblaku** u dijaloškom okviru **Novi projekt** , u odjeljku **Python**.

![Novi dijaloški okvir projekta](./media/cloud-services-python-ptvs/new-project-cloud-service.png)

U čarobnjaku za Azure u Oblaku, možete stvoriti nove web- a tempiranja uloge.

![Dijaloški okvir servisa Azure oblaka](./media/cloud-services-python-ptvs/new-service-wizard.png)

U sklopu predloška uloge suradnika predloženog šifru da biste povezali račun za Azure prostora za pohranu ili Bus servisa Azure.

![Oblak servis za rješenja](./media/cloud-services-python-ptvs/worker.png)

Uloge weba ili tempiranja možete dodati u postojeću oblaku u bilo kojem trenutku.  Možete odabrati da biste dodali postojeće projekte u rješenje ili stvarati nove.

![Dodavanje naredbe uloga](./media/cloud-services-python-ptvs/add-new-or-existing-role.png)

Servis u oblaku mogu sadržavati uloge primijenjeno na različitim jezicima.  Na primjer, imate ulogu web Python implementirati pomoću Django, s Python ili uloge suradnika C#.  Jednostavno možete započeti komunikaciju između vaša uloga pomoću servisa Bus redova ili reda čekanja za pohranu.

## <a name="install-python-on-the-cloud-service"></a>Instalacija Python na servis u oblaku

>[AZURE.WARNING] Postavljanje skripte koje se instaliraju pomoću Visual Studio (u trenutku u ovom se članku zadnjeg ažuriranja) ne funkcionira. U ovom se odjeljku opisuje zaobilazno rješenje.

Glavni problem s skripte postavljanje su da su instalirali python. Najprije definirati dva [pokretanje zadataka](cloud-services-startup-tasks.md) u datoteci [ServiceDefinition.csdef](cloud-services-model-and-package.md#servicedefinitioncsdef) . Prvi zadatak (**PrepPython.ps1**) preuzima i instalira Python runtime. Drugi zadatak (**PipInstaller.ps1**) pokreće točaka da biste instalirali sve ovisnosti možda imate.

Skripti komponente su pisane ciljanja Python 3.5. Ako želite koristiti verziju 2.x python, postavite **PYTHON2** promjenjiva datoteku da biste **na** dvije stvari pokretanje i izvođenje zadatka: `<Variable name="PYTHON2" value="<mark>on</mark>" />`.


```xml
<Startup>

  <Task executionContext="elevated" taskType="simple" commandLine="bin\ps.cmd PrepPython.ps1">
    <Environment>
      <Variable name="EMULATED">
        <RoleInstanceValue xpath="/RoleEnvironment/Deployment/@emulated" />
      </Variable>
      <Variable name="PYTHON2" value="off" />
    </Environment>
  </Task>

  <Task executionContext="elevated" taskType="simple" commandLine="bin\ps.cmd PipInstaller.ps1">
    <Environment>
      <Variable name="EMULATED">
        <RoleInstanceValue xpath="/RoleEnvironment/Deployment/@emulated" />
      </Variable>
      <Variable name="PYTHON2" value="off" />
    </Environment>
    
  </Task>

</Startup>
```

Varijable **PYTHON2** i **PYPATH** mora dodati zadatak pokretanja tempiranja. Varijabla **PYPATH** koristi se samo ako je varijabla **PYTHON2** postavljeno na **na**.

```xml
<Runtime>
  <Environment>
    <Variable name="EMULATED">
      <RoleInstanceValue xpath="/RoleEnvironment/Deployment/@emulated" />
    </Variable>
    <Variable name="PYTHON2" value="off" />
    <Variable name="PYPATH" value="%SystemDrive%\Python27" />
  </Environment>
  <EntryPoint>
    <ProgramEntryPoint commandLine="bin\ps.cmd LaunchWorker.ps1" setReadyOnProcessStart="true" />
  </EntryPoint>
</Runtime>
```

#### <a name="sample-servicedefinitioncsdef"></a>Ogledni ServiceDefinition.csdef

```xml
<?xml version="1.0" encoding="utf-8"?>
<ServiceDefinition name="AzureCloudServicePython" xmlns="http://schemas.microsoft.com/ServiceHosting/2008/10/ServiceDefinition" schemaVersion="2015-04.2.6">
  <WorkerRole name="WorkerRole1" vmsize="Small">
    <ConfigurationSettings>
      <Setting name="Microsoft.WindowsAzure.Plugins.Diagnostics.ConnectionString" />
      <Setting name="Python2" />
    </ConfigurationSettings>
    <Startup>
      <Task executionContext="elevated" taskType="simple" commandLine="bin\ps.cmd PrepPython.ps1">
        <Environment>
          <Variable name="EMULATED">
            <RoleInstanceValue xpath="/RoleEnvironment/Deployment/@emulated" />
          </Variable>
          <Variable name="PYTHON2" value="off" />
        </Environment>
      </Task>
      <Task executionContext="elevated" taskType="simple" commandLine="bin\ps.cmd PipInstaller.ps1">
        <Environment>
          <Variable name="EMULATED">
            <RoleInstanceValue xpath="/RoleEnvironment/Deployment/@emulated" />
          </Variable>
          <Variable name="PYTHON2" value="off" />
        </Environment>
      </Task>
    </Startup>
    <Runtime>
      <Environment>
        <Variable name="EMULATED">
          <RoleInstanceValue xpath="/RoleEnvironment/Deployment/@emulated" />
        </Variable>
        <Variable name="PYTHON2" value="off" />
        <Variable name="PYPATH" value="%SystemDrive%\Python27" />
      </Environment>
      <EntryPoint>
        <ProgramEntryPoint commandLine="bin\ps.cmd LaunchWorker.ps1" setReadyOnProcessStart="true" />
      </EntryPoint>
    </Runtime>
    <Imports>
      <Import moduleName="RemoteAccess" />
      <Import moduleName="RemoteForwarder" />
    </Imports>
  </WorkerRole>
</ServiceDefinition>
```



Nakon toga stvoriti **PrepPython.ps1** i **PipInstaller.ps1** datoteke u na **. / intervala** mapu vaša uloga.

#### <a name="preppythonps1"></a>PrepPython.ps1

Ova skripta instalira python. Ako je varijabla enviornment **PYTHON2** postavljeno na **na** , a zatim Python 2.7 instalirat će se, u suprotnom Python 3.5 instalirat će se.

```powershell
$is_emulated = $env:EMULATED -eq "true"
$is_python2 = $env:PYTHON2 -eq "on"
$nl = [Environment]::NewLine

if (-not $is_emulated){
    Write-Output "Checking if python is installed...$nl"
    if ($is_python2) {
        & "${env:SystemDrive}\Python27\python.exe"  -V | Out-Null
    }
    else {
        py -V | Out-Null
    }

    if (-not $?) {

        $url = "https://www.python.org/ftp/python/3.5.2/python-3.5.2-amd64.exe"
        $outFile = "${env:TEMP}\python-3.5.2-amd64.exe"

        if ($is_python2) {
            $url = "https://www.python.org/ftp/python/2.7.12/python-2.7.12.amd64.msi"
            $outFile = "${env:TEMP}\python-2.7.12.amd64.msi"
        }
        
        Write-Output "Not found, downloading $url to $outFile$nl"
        Invoke-WebRequest $url -OutFile $outFile
        Write-Output "Installing$nl"

        if ($is_python2) {
            Start-Process msiexec.exe -ArgumentList "/q", "/i", "$outFile", "ALLUSERS=1" -Wait
        }
        else {
            Start-Process "$outFile" -ArgumentList "/quiet", "InstallAllUsers=1" -Wait
        }

        Write-Output "Done$nl"
    }
    else {
        Write-Output "Already installed"
    }
}
```

#### <a name="pipinstallerps1"></a>PipInstaller.ps1

Ova skripta poziva prema gore točaka i instalira sve ovisnosti u datoteci **requirements.txt** . Ako je varijabla enviornment **PYTHON2** postavljeno na **na** , a zatim Python 2.7 koristit će se, u suprotnom će se koristiti Python 3.5.

```powershell
$is_emulated = $env:EMULATED -eq "true"
$is_python2 = $env:PYTHON2 -eq "on"
$nl = [Environment]::NewLine

if (-not $is_emulated){
    Write-Output "Checking if requirements.txt exists$nl"
    if (Test-Path ..\requirements.txt) {
        Write-Output "Found. Processing pip$nl"

        if ($is_python2) {
            & "${env:SystemDrive}\Python27\python.exe" -m pip install -r ..\requirements.txt
        }
        else {
            py -m pip install -r ..\requirements.txt
        }

        Write-Output "Done$nl"
    }
    else {
        Write-Output "Not found$nl"
    }
}
```

#### <a name="modify-launchworkerps1"></a>Izmjena LaunchWorker.ps1

>[AZURE.NOTE] U slučaju projekta **ulogu suradnika** , **LauncherWorker.ps1** datoteka je potreban za izvršiti datoteku za pokretanje. U projektu **web uloge** , datoteke prilikom pokretanja umjesto definirana je u svojstva projekta.

Da biste učinili mnogo avansne rada izvorno stvorena **bin\LaunchWorker.ps1** , ali zaista ne funkcionira. Zamijenite sadržaj u toj datoteci sljedeću skriptu.

Ova skripta poziva **worker.py** datoteku s python projekta. Ako je varijabla enviornment **PYTHON2** postavljeno na **na** , a zatim Python 2.7 koristit će se, u suprotnom će se koristiti Python 3.5.

```powershell
$is_emulated = $env:EMULATED -eq "true"
$is_python2 = $env:PYTHON2 -eq "on"
$nl = [Environment]::NewLine

if (-not $is_emulated)
{
    Write-Output "Running worker.py$nl"

    if ($is_python2) {
        cd..
        iex "$env:PYPATH\python.exe worker.py"
    }
    else {
        cd..
        iex "py worker.py"
    }
}
else
{
    Write-Output "Running (EMULATED) worker.py$nl"

    # Customize to your local dev environment

    if ($is_python2) {
        cd..
        iex "$env:PYPATH\python.exe worker.py"
    }
    else {
        cd..
        iex "py worker.py"
    }
}
```

#### <a name="pscmd"></a>PS.cmd

Predlošci za Visual Studio trebalo stvoriti datoteku **ps.cmd** u na **. / intervala** mapu. U ovom Jezgrena skripta pozive izvan skripti komponente PowerShell omot iznad i njihovi zapisivanje temelji se na nazivu omot PowerShell naziva. Ako datoteka nije stvorena, Evo što mora biti u njoj. 

```bat
@echo off

cd /D %~dp0

if not exist "%DiagnosticStore%\LogFiles" mkdir "%DiagnosticStore%\LogFiles"
%SystemRoot%\System32\WindowsPowerShell\v1.0\powershell.exe -ExecutionPolicy Unrestricted -File %* >> "%DiagnosticStore%\LogFiles\%~n1.txt" 2>> "%DiagnosticStore%\LogFiles\%~n1.err.txt"
```



## <a name="run-locally"></a>Izvodi lokalno

Ako postavite oblaka servisa projekta kao projekt prilikom pokretanja i pritisnite F5, servis u oblaku funkcionirat će u lokalnom Azure emulator.

Iako PTVS podržava neće funkcionirati pokretanje u u emulator, ispravljanje pogrešaka (na primjer, prekidne točke).

Za ispravljanje pogrešaka na web- a tempiranja uloge, možete postaviti projekta uloga kao projekt prilikom pokretanja a za ispravljanje pogrešaka koje umjesto toga.  Možete postaviti i više projekata prilikom pokretanja.  Desnom tipkom miša kliknite rješenje, a zatim odaberite **Postavljanje pokretanja projekata**.

![Svojstva projekta pokretanje rješenja](./media/cloud-services-python-ptvs/startup.png)

## <a name="publish-to-azure"></a>Objavljivanje Azure

Da biste objavili, desnom tipkom miša kliknite projekt servisa oblak u rješenje, a zatim odaberite **Objavi**.

![Microsoft Azure objavljivanje prijava](./media/cloud-services-python-ptvs/publish-sign-in.png)

Pratite upute u čarobnjaku. Ako je potrebno, Omogući udaljenu radnu površinu. Kada je potrebno za ispravljanje pogrešaka nešto korisno je udaljene radne površine.

Kada završite s konfiguracijom postavke, kliknite **Objavi**.

Neki tijek pojavit će se u izlaznom prozoru, a zatim vidjet ćete prozor zapisnik aktivnosti za Microsoft Azure.

![Prozor Evidencija aktivnosti Microsoft Azure](./media/cloud-services-python-ptvs/publish-activity-log.png)

Uvođenje će potrajati nekoliko minuta da biste dovršili, a zatim na webu i/ili radnih uloge će se izvoditi na Azure!

### <a name="investigate-logs"></a>Istražite zapisnika

Kada virtualnog računala servisa oblaka pokretanja i instalira Python, možete pogledati zapisnike da biste pronašli poruke pogreške. Ove zapisnika nalaze se u u **C:\Resources\Directory\\\LogFiles {uloga}** mapu. **PrepPython.err.txt** neće imati barem jedan pogreške u iz kada skripta pokušava otkriti ako je instaliran Python i **PipInstaller.err.txt** možda požali o zastarjelih verziji točaka.

## <a name="next-steps"></a>Daljnji koraci

Detaljnije informacije o radu s web- a tempiranja ulogama u alatima za Python za Visual Studio potražite u dokumentaciji PTVS:

- [Oblak servisa projekata][]

Dodatne informacije o korištenju servisa Azure s vašim ulogama web- a tempiranja, kao što su pomoću Azure prostora za pohranu ili Bus servisa potražite u sljedećim člancima.

- [Servis za blobova platforme][]
- [Servis za tablice][]
- [Usluga reda čekanja][]
- [Servis Bus reda čekanja][]
- [Servis Bus teme][]


<!--Link references-->

[Što je servis u Oblaku?]: cloud-services-choose-me.md
[execution model-web sites]: ../app-service-web/app-service-web-overview.md
[execution model-vms]: ../virtual-machines/virtual-machines-windows-about.md
[execution model-cloud services]: cloud-services-choose-me.md
[Python Developer Center]: /develop/python/

[Servis za blobova platforme]: ../storage/storage-python-how-to-use-blob-storage.md
[Usluga reda čekanja]: ../storage/storage-python-how-to-use-queue-storage.md
[Servis za tablice]: ../storage/storage-python-how-to-use-table-storage.md
[Servis Bus reda čekanja]: ../service-bus-messaging/service-bus-python-how-to-use-queues.md
[Servis Bus teme]: ../service-bus-messaging/service-bus-python-how-to-use-topics-subscriptions.md


<!--External Link references-->

[Python alate za Visual Studio]: http://aka.ms/ptvs
[Python Tools for Visual Studio Documentation]: http://aka.ms/ptvsdocs
[Oblak servisa projekata]: http://go.microsoft.com/fwlink/?LinkId=624028
[Alati za Azure SDK za dodavanje veze za VANJSKIH 2013]: http://go.microsoft.com/fwlink/?LinkId=323510
[Alati za Azure SDK za dodavanje veze za VANJSKIH 2015.]: http://go.microsoft.com/fwlink/?LinkId=518003
[Python 2.7 32-bitne]: https://www.python.org/downloads/
[Python 3.5 32-bitne]: https://www.python.org/downloads/
