<properties
   pageTitle="Instalirajte .NET na uloge servisa oblaka | Microsoft Azure"
   description="U ovom se članku opisuje kako ručno instalirati .NET framework na Cloud servisa Web i uloge suradnika"
   services="cloud-services"
   documentationCenter=".net"
   authors="thraka"
   manager="timlt"
   editor=""/>

<tags
   ms.service="cloud-services"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/10/2016"
   ms.author="adegeo"/>

# <a name="install-net-on-a-cloud-service-role"></a>Kliknite pločicu .NET uloge servisa oblaka 

U ovom se članku opisuje kako instalirati .NET framework na Cloud servisa Web i uloge suradnika. Možete koristiti sljedeće korake da biste instalirali .NET 4.6.1 na Azure 4 za goste OS obitelj. Najnovije informacije o goste OS izdanja potražite u članku [Azure goste OS izdavanje i SDK kompatibilnosti matrice](cloud-services-guestos-update-matrix.md).

Postupak instalacije .NET na vašim web- a tempiranja ulogama uključuje uključujući instalacijski paket .NET kao dio oblaka projekta i pokretanje instalacijskog programa kao dio željenu ulogu pokretanja zadataka.  

## <a name="add-the-net-installer-to-your-project"></a>Dodavanje instalacijskog programa za .NET u projekt
- Preuzmite na webu instalacijski program za .NET framework želite instalirati
    - [Instalacijski program za web-.NET 4.6.1](http://go.microsoft.com/fwlink/?LinkId=671729)
- Uloge za Web
  1. U **Pregledniku rješenja**, u odjeljku **uloge** u programu project za servis oblaka desnom tipkom miša kliknite uloge i odaberite **Dodaj > nova mapa**. Stvorite mapu pod nazivom *smeće*
  2. Desnom tipkom miša kliknite mapu za **smeće** , a zatim odaberite **Dodaj > postojeće stavke**. Odabir instalacijskog programa za .NET i dodajte ga u mapu za smeće.
- Uloge suradnika
  1. Desnom tipkom miša kliknite uloge i odaberite **Dodaj > postojeće stavke**. Odaberite instalacijskog programa za .NET, a zatim ga dodati u ulogu. 

Datoteke će se dodati taj način za mapu sadržaja uloga bit će automatski dodane u paket servisa oblaka i implementirati dosljedan mjesto na virtualnog računala. Ponovite postupak za sve web- a tempiranja uloge servis u Oblaku da sve uloge imali kopiju instalacijski program.

> [AZURE.NOTE] Instalirajte .NET 4.6.1 na vaša uloga u Oblaku čak i ako se aplikacija pronalaze .NET 4.6. OS za goste Azure obuhvaća ažuriranja [3098779](https://support.microsoft.com/kb/3098779) i [3097997](https://support.microsoft.com/kb/3097997). Instaliranje .NET 4.6 pri vrhu ove ažuriranja može uzrokovati probleme s kada se pokrene .NET aplikacijama da bi se izravno instalirajte .NET 4.6.1 umjesto .NET 4.6. Dodatne informacije potražite u članku [iz baze znanja 3118750](https://support.microsoft.com/kb/3118750).

![Uloga sadržaja s datoteke instalacijskog programa][1]

## <a name="define-startup-tasks-for-your-roles"></a>Definiranje pokretanja zadataka za svoje uloge
Pokretanje zadataka dopustiti izvođenje operacije prije pokretanja uloge. Instaliranje .NET Framework kao dio zadatak pokretanja će li instaliran framework prije pokretanja bilo koji od kodu aplikacije. Dodatne informacije o pokretanju zadataka potražite u članku: [Pokretanje zadaci prilikom pokretanja u Azure](cloud-services-startup-tasks.md). 

1. Dodajte sljedeće *ServiceDefinition.csdef* datoteku u odjeljku čvor **WebRole** ili **WorkerRole** za sve uloge:
    
    ```xml
    <LocalResources>
      <LocalStorage name="NETFXInstall" sizeInMB="1024" cleanOnRoleRecycle="false" />
    </LocalResources>    
    <Startup>
      <Task commandLine="install.cmd" executionContext="elevated" taskType="simple">
        <Environment>
          <Variable name="PathToNETFXInstall">
            <RoleInstanceValue xpath="/RoleEnvironment/CurrentInstance/LocalResources/LocalResource[@name='NETFXInstall']/@path" />
          </Variable>
          <Variable name="ComputeEmulatorRunning">
            <RoleInstanceValue xpath="/RoleEnvironment/Deployment/@emulated" />
          </Variable>
        </Environment>
      </Task>
    </Startup>
    ```

    Iznad konfiguracije će se pokrenuti naredba konzole *install.cmd* s ovlastima administratora da biste mogli instalirati .NET framework. Konfiguraciju i stvara na LocalStorage s nazivom *NETFXInstall*. Skriptu za pokretanje će postaviti mapu temp za korištenje tog resursa lokalno spremište tako da instalacijskog programa za .NET framework će se preuzeti i instalirati s ovog resursa. Važno je da postavite veličinu ovaj resurs na najmanje 1024MB da biste bili sigurni framework instalirat će ispravno. Dodatne informacije o pokretanju zadataka potražite u članku: [Zadaci prilikom pokretanja uobičajenih servis u Oblaku](cloud-services-startup-tasks-common.md) 

2. Stvaranje datoteke **install.cmd** i dodati sve uloge tako da dvokliknite ulogu i odabirom **Dodaj > postojeće stavke...**. Da bi se sve uloge trebala bi se datoteci .NET instalacijskog programa, kao i install.cmd datoteku.
    
    ![Uloga sadržaja sa svim datotekama][2]

    > [AZURE.NOTE] Da biste stvorili ove datoteke koristite jednostavan uređivač teksta kao što je blok za pisanje. Ako koristite Visual Studio da biste stvorili tekstne datoteke i pa ga preimenujte u '.cmd' datoteku i dalje može sadržavati oznaku UTF-8 bajt redoslijed i izvodi u prvom retku skripte uzrokovat će pogrešku. Ako ste pomoću programa Visual Studio stvaranje ostavite datoteke dodati REM (napomenu) u prvom retku datoteke tako da se zanemaruje prilikom pokretanja. 

3. Dodajte sljedeću skriptu **install.cmd** datoteku:

    ```
    REM Set the value of netfx to install appropriate .NET Framework. 
    REM ***** To install .NET 4.5.2 set the variable netfx to "NDP452" *****
    REM ***** To install .NET 4.6 set the variable netfx to "NDP46" *****
    REM ***** To install .NET 4.6.1 set the variable netfx to "NDP461" *****
    REM ***** To install .NET 4.6.2 set the variable netfx to "NDP462" *****
    set netfx="NDP461"
    
    REM ***** Set script start timestamp *****
    set timehour=%time:~0,2%
    set timestamp=%date:~-4,4%%date:~-10,2%%date:~-7,2%-%timehour: =0%%time:~3,2%
    set "log=install.cmd started %timestamp%."
    
    REM ***** Exit script if running in Emulator *****
    if %ComputeEmulatorRunning%=="true" goto exit
    
    REM ***** Needed to correctly install .NET 4.6.1, otherwise you may see an out of disk space error *****
    set TMP=%PathToNETFXInstall%
    set TEMP=%PathToNETFXInstall%
    
    REM ***** Setup .NET filenames and registry keys *****
    if %netfx%=="NDP462" goto NDP462
    if %netfx%=="NDP461" goto NDP461
    if %netfx%=="NDP46" goto NDP46
        set "netfxinstallfile=NDP452-KB2901954-Web.exe"
        set netfxregkey="0x5cbf5"
        goto logtimestamp
    
    :NDP46
    set "netfxinstallfile=NDP46-KB3045560-Web.exe"
    set netfxregkey="0x6004f"
    goto logtimestamp
    
    :NDP461
    set "netfxinstallfile=NDP461-KB3102438-Web.exe"
    set netfxregkey="0x6040e"
    goto logtimestamp
    
    :NDP462
    set "netfxinstallfile=NDP462-KB3151802-Web.exe"
    set netfxregkey="0x60632"
    
    :logtimestamp
    REM ***** Setup LogFile with timestamp *****
    md "%PathToNETFXInstall%\log"
    set startuptasklog="%PathToNETFXInstall%log\startuptasklog-%timestamp%.txt"
    set netfxinstallerlog="%PathToNETFXInstall%log\NetFXInstallerLog-%timestamp%"
    echo %log% >> %startuptasklog%
    echo Logfile generated at: %startuptasklog% >> %startuptasklog%
    echo TMP set to: %TMP% >> %startuptasklog%
    echo TEMP set to: %TEMP% >> %startuptasklog%
    
    REM ***** Check if .NET is installed *****
    echo Checking if .NET (%netfx%) is installed >> %startuptasklog%
    set /A netfxregkeydecimal=%netfxregkey%
    set foundkey=0
    FOR /F "usebackq skip=2 tokens=1,2*" %%A in (`reg query "HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\NET Framework Setup\NDP\v4\Full" /v Release 2^>nul`) do @set /A foundkey=%%C
    echo Minimum required key: %netfxregkeydecimal% -- found key: %foundkey% >> %startuptasklog%
    if %foundkey% GEQ %netfxregkeydecimal% goto installed
    
    REM ***** Installing .NET *****
    echo Installing .NET with commandline: start /wait %~dp0%netfxinstallfile% /q /serialdownload /log %netfxinstallerlog%  /chainingpackage "CloudService Startup Task" >> %startuptasklog%
    start /wait %~dp0%netfxinstallfile% /q /serialdownload /log %netfxinstallerlog% /chainingpackage "CloudService Startup Task" >> %startuptasklog% 2>>&1
    if %ERRORLEVEL%== 0 goto installed
        echo .NET installer exited with code %ERRORLEVEL% >> %startuptasklog%   
        if %ERRORLEVEL%== 3010 goto restart
        if %ERRORLEVEL%== 1641 goto restart
        echo .NET (%netfx%) install failed with Error Code %ERRORLEVEL%. Further logs can be found in %netfxinstallerlog% >> %startuptasklog%
    
    :restart
    echo Restarting to complete .NET (%netfx%) installation >> %startuptasklog%
    EXIT /B %ERRORLEVEL%
    
    :installed
    echo .NET (%netfx%) is installed >> %startuptasklog%
    
    :end
    echo install.cmd completed: %date:~-4,4%%date:~-10,2%%date:~-7,2%-%timehour: =0%%time:~3,2% >> %startuptasklog%
    
    :exit
    EXIT /B 0
    ```
        
    Skripta za instalaciju provjerava je li navedeni .NET framework verzije je instalirao na računalu ispitivanje registra. Ako nije instalirana verzija .NET, onda je za .net Web Installer pokrenuti. Pomoći u rješavanju problema s eventualne probleme s skripta će se prijaviti sve aktivnosti datoteku pod nazivom *startuptasklog-(currentdatetime) .txt* pohranjene u *InstallLogs* lokalno spremište.

    > [AZURE.NOTE] Skripta i dalje prikazuje kako se instalira .NET 4.5.2 ili .NET 4.6 za continuity. Nema potrebe da biste ručno instalirali .NET 4.5.2 kao što je dostupan na Azure goste OS. Umjesto instalacije .NET 4.6 izravno instalirajte .NET 4.6.1 zbog [KB 3118750](https://support.microsoft.com/kb/3118750).
      

## <a name="configure-diagnostics-to-transfer-the-startup-task-logs-to-blob-storage"></a>Konfiguriranje Dijagnostika prenijeti zapisnike pokretanje zadatka da biste bloba prostora za pohranu 
Da biste pojednostavnili otklanjanje problema s bilo kojeg Instaliraj možete konfigurirati Azure dijagnostiku da biste prenijeli sve datoteke zapisnika generira skriptu za pokretanje ili instalacijskog programa za .NET sa spremištem blobova. Na taj se način možete pogledati zapisnike jednostavno preuzimanje datoteke zapisnika iz spremišta blobova umjesto potrebe za udaljene radne površine u ulogu.

Da biste konfigurirali Dijagnostika Otvori *diagnostics.wadcfgx* i dodajte sljedeće u odjeljku čvor **direktorija** : 

```xml 
<DataSources>
 <DirectoryConfiguration containerName="netfx-install">
  <LocalResource name="NETFXInstall" relativePath="log"/>
 </DirectoryConfiguration>
</DataSources>
```

To će konfigurirati azure dijagnostiku da biste prenijeli sve datoteke u direktoriju *zapisnika* u odjeljku *NETFXInstall* resursa u račun za pohranu Dijagnostika u spremniku blob *netfx Instaliraj* .

## <a name="deploying-your-service"></a>Implementacija usluge 
Ako pokrenete usluge zadaci prilikom pokretanja će pokrenuti i instalirajte .NET framework ako već nije instaliran. Vaša uloga bit će se u zauzet stanju dok framework instalira, a možda početi čak i ako je to potrebno framework instalaciju. 

## <a name="additional-resources"></a>Dodatni resursi

- [Instaliranje .NET Framework][]
- [Kako: odredite koje .NET Framework verzije su instalirani][]
- [Otklanjanje poteškoća s .NET Framework instalacije][]

[Kako: odredite koje .NET Framework verzije su instalirani]: https://msdn.microsoft.com/library/hh925568.aspx
[Instaliranje .NET Framework]: https://msdn.microsoft.com/library/5a4x27ek.aspx
[Otklanjanje poteškoća s .NET Framework instalacije]: https://msdn.microsoft.com/library/hh925569.aspx

<!--Image references-->
[1]: ./media/cloud-services-dotnet-install-dotnet/rolecontentwithinstallerfiles.png
[2]: ./media/cloud-services-dotnet-install-dotnet/rolecontentwithallfiles.png

 
