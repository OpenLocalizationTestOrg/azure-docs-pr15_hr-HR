<properties
   pageTitle="Otklanjanje poteškoća klijentskog Docker u sustavu Windows pomoću Visual Studio | Microsoft Azure"
   description="Otklanjanje poteškoća naići prilikom korištenja Visual Studio za stvaranje i implementacija web-aplikacije pomoću Visual Studio Docker u sustavu Windows."
   services="azure-container-service"
   documentationCenter="na"
   authors="mlearned"
   manager="douge"
   editor="" />
<tags
   ms.service="multiple"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="multiple"
   ms.date="06/08/2016"
   ms.author="allclark" />

# <a name="troubleshooting-visual-studio-docker-development"></a>Otklanjanje poteškoća s razvoj Docker Visual Studio

Rad s Visual Studio Tools za pretpregled Docker, mogu se pojaviti neki problemi zbog prirode pretpregled.
Slijedi nekoliko uobičajenih problema i rješenja.


## <a name="unable-to-validate-volume-mapping"></a>Nije moguće provjeriti valjanost mapiranje glasnoće
Mapiranje glasnoću potrebna je za zajedničko korištenje izvornog koda i binarne datoteke aplikacije s mapom aplikacije u spremniku.  Mapiranja određene glasnoću nalazi se unutar datoteke docker compose.dev.debug.yml i docker compose.dev.release.yml. Kao što je datoteka mijenjaju se na glavno računalo, spremnike odražavaju promjene u slične struktura mapa.

Da biste omogućili mapiranje glasnoće, otvorite **postavke...** iz palete "moby" Docker za Windows, a zatim odaberite karticu **Zajedničko korištenje pogone** .  Provjerite da su slovo koje hostira projekta, kao i slovo u kojoj se nalazi % korisničkog PROFILA zajednički koriste provjeru ih, a zatim kliknite **Primijeni**.

Da biste testirali ako glasnoću mapiranje funkcionira, kada se (diskovima) je omogućeno zajedničko korištenje, izvođenje i F5 iz programa Visual Studio ili pokušajte sljedeće iz naredbe upit:

*U naredbeni redak za Windows*

*[Bilješka: pretpostavlja da se mape korisnika nalazi se na disku "C", a da je omogućeno zajedničko korištenje.  Po potrebi ažurirajte ako ste omogućili zajedničko korištenje na drugi pogon]*
```
docker run -it -v /c/Users/Public:/wormhole busybox
```

*U spremniku Linux*

```
/ # ls
```

Trebali biste vidjeti direktorij stavku s korisnika/javne mape.
Ako prikazuju se datoteke i mape /c/Users/Public nije prazno, glasnoću mapiranje nije ispravno konfiguriran. 

```
bin       etc       proc      sys       usr       wormhole
dev       home      root      tmp       var
```

Promjena u direktoriju wormhole da biste vidjeli sadržaj na `/c/Users/Public` direktorija:

```
/ # cd wormhole/
/wormhole # ls
AccountPictures  Downloads        Music            Videos
Desktop          Host             NuGet.Config     desktop.ini
Documents        Libraries        Pictures
/wormhole #
```

**Bilješke:** *Kada radite s Linux VMs, datotečni sustav spremnik nije velika i mala slova.*

##<a name="build--prepareforbuild-task-failed-unexpectedly"></a>Sastavljanje: "PrepareForBuild" zadatka neočekivano nije uspjelo.

Microsoft.DotNet.Docker.CommandLine.ClientException: Pojavila se pogreška povezivanja:

Provjerite je li pokrenuto glavno računalo za docker zadani. Otvorite naredbeni redak, a zatim izvršiti:

```
docker info
```

Ako je to vraća pogrešku pa pokušajte pokrenuti aplikaciju za stolna računala **Docker za Windows** .  Ako se aplikacija za stolna računala izvodi pa ikonu **moby** iz palete trebala biti vidljiva. Desnom tipkom miša kliknite ikonu palete i otvorite **Postavke**.  Kliknite **Ponovno postavi** karticu i zatim **ponovno pokretanje Docker..**.

##<a name="manually-upgrading-from-version-031-to-040"></a>Ručno nadogradnje s verzije 0.31 i 0.40


1. Sigurnosno kopiranje projekta
1. Izbrišite sljedeće datoteke u projektu:

    ```
      Dockerfile
      Dockerfile.debug
      DockerTask.ps1
      docker-compose-yml
      docker-compose.debug.yml
      .dockerignore
      Properties\Docker.props
      Properties\Docker.targets
    ```

1. Zatvorite rješenje i uklonite sljedeće retke iz datoteke .xproj:

    ```
      <DockerToolsMinVersion>0.xx</DockerToolsMinVersion>
      <Import Project="Properties\Docker.props" />
      <Import Project="Properties\Docker.targets" />
    ```

1. Ponovno otvorite rješenja
1. Uklanjanje sljedeće retke iz datoteke Properties\launchSettings.json:

    ```
      "Docker": {
        "executablePath": "%WINDIR%\\System32\\WindowsPowerShell\\v1.0\\powershell.exe",
        "commandLineArgs": "-ExecutionPolicy RemoteSigned .\\DockerTask.ps1 -Run -Environment $(Configuration) -Machine '$(DockerMachineName)'"
      }
    ```

1. Uklanjanje sljedeće datoteke koja se odnose na Docker project.json u na publishOptions:

    ```
    "publishOptions": {
      "include": [
        ...
        "docker-compose.yml",
        "docker-compose.debug.yml",
        "Dockerfile.debug",
        "Dockerfile",
        ".dockerignore"
      ]
    },
    ```

1. Deinstalirati prethodnu verziju i alati Docker 0.40, a zatim **Dodaj -> podrška Docker** ponovno instalirajte na kontekstnom izborniku za ASP.Net osnovne Web ili aplikacije konzole. Ovo će dodati nove potrebne Docker artefakte natrag na projektu. 

## <a name="an-error-dialog-occurs-when-attempting-to-add-docker-support-or-debug-f5-an-aspnet-core-application-in-a-container"></a>Dijalog pogreške prilikom pokušaja **Dodaj -> Docker podršku** ili ASP.NET osnovne aplikacija u spremniku za ispravljanje pogrešaka (F5)

Ne možemo katkad pojavljuje deinstalacije i instalacije proširenja predmemoriju za Visual Studio MEF (upravlja proširenja Framework) možete oštećena. Kada se to dogodi može uzrokovati razne pogreške dijalozi prilikom dodavanja Docker podršku i/ili pokuša pokrenuti ili ASP.NET aplikacija za Core ispravljanje pogrešaka (F5). Kao zaobilazno privremene izvršiti sljedeće korake da biste izbrisali i Obnovi predmemoriju MEF.

1. Zatvorite sve instance programa Visual Studio
1. Otvorite %USERPROFILE%\AppData\Local\Microsoft\VisualStudio\14.0\
1. Izbrišite sljedeće mape
     ```
       ComponentModelCache
       Extensions
       MEFCacheBackup
    ```
1. Otvori Visual Studio
1. Ponovno pokušajte scenarija 
