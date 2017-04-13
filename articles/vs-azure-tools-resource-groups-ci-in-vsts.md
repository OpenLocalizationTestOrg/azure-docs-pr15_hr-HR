<properties
    pageTitle="Neprekinuti integraciju u Dodavanje veze za VANJSKIH Team Services pomoću Azure resursa grupne projekte | Microsoft Azure"
    description="U članku se opisuje kako postaviti neprekinuti integraciju u Visual Studio Team Services pomoću grupa resursa Azure implementacije projekata u Visual Studio."
    services="visual-studio-online"
    documentationCenter="na"
    authors="mlearned"
    manager="erickson-doug"
    editor="" />

 <tags
    ms.service="azure-resource-manager"
    ms.devlang="multiple"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="na"
    ms.date="08/01/2016"
    ms.author="mlearned" />

# <a name="continuous-integration-in-visual-studio-team-services-using-azure-resource-group-deployment-projects"></a>Neprekinuti integraciju u Visual Studio Team Services pomoću grupa resursa Azure implementacije projekata

Da biste implementirali Azure predložak, morate izvršavanje zadataka prođite kroz različite faze: Sastavi, Test Kopiraj Azure (naziva se i "Pripremna"), i uvođenje predloška.  Postoje dva načina da biste to učinili u Visual Studio Team Services (Dodavanje veze za VANJSKIH Team Services). Obje metode omogućuju iste rezultate, pa odaberite onaj koji najbolje odgovara tijekova rada.

-   Dodajte jedan korak definicije Sastavi koji će se pokrenuti skriptu PowerShell uvrštene u programu project za implementaciju Azure grupa resursa (uvođenja AzureResourceGroup.ps1). Skripta kopira artefakte i uvodi predložak.
-   Dodajte većem broju servisa tima za dodavanje veze za VANJSKIH sastavljanje koraka, svaki od njih izvođenje fazu zadatka.

U ovom se članku objašnjava kako koristiti prvu mogućnost (koristite definiciju Sastavi da biste pokrenuli skriptu komponente PowerShell). Jedna od prednosti tu mogućnost jest da je skriptu koja se koristi za razvojne inženjere u Visual Studio isti skriptu koja koristi Dodavanje veze za VANJSKIH Team Services. Ovaj postupak pretpostavlja da ste već projekt za implementaciju Visual Studio prijavi u Dodavanje veze za VANJSKIH Team Services.

## <a name="copy-artifacts-to-azure"></a>Kopiranje artefakte Azure 

Bez obzira na to scenarij, ako imate sve artefakte koje su vam potrebne za implementaciju predložak, morat ćete omogućiti pristup Voditelj resursa Azure s njima. Te elemente kao što su mogu sadržavati datoteke:

-   Ugniježđene predložaka
-   Konfiguriranje skripte i DSC skripti
-   Binarne datoteke iz aplikacije

### <a name="nested-templates-and-configuration-scripts"></a>Ugniježđene predloške i konfiguracija skripti
Kada koristite predlošci Visual Studio (ili načinjene pomoću Visual Studio isječci), skriptu PowerShell faze ne samo na artefakte, također parameterizes URI za resurse za različite implementacije. Skripta, a zatim kopira u artefakte sigurne spremniku u Azure, stvara SaS tokena za njih, a zatim prosljeđuje te podatke na uvođenje predloška. Potražite u članku [Stvaranje predloška implementaciju](https://msdn.microsoft.com/library/azure/dn790564.aspx) da biste saznali više o predlošcima ugniježđene.

## <a name="set-up-continuous-deployment-in-vs-team-services"></a>Postavljanje neprekinuti implementacije u Dodavanje veze za VANJSKIH Team Services

Da biste pozvali skriptu PowerShell u Dodavanje veze za VANJSKIH Team Services, morate ažurirati definicije Sastavi. Brief, koraci su: 

1.  Uređivanje definicije Sastavi.
1.  Postavljanje Azure autorizaciju u Dodavanje veze za VANJSKIH Team Services.
1.  Dodajte korak Sastavi za Azure PowerShell koji referencira skriptu PowerShell u programu project za implementaciju Azure grupu resursa.
1.  Postavite vrijednost parametra *– ArtifactsStagingDirectory* za rad s projektom ugrađeni Dodavanje veze za VANJSKIH Team Services.

### <a name="detailed-walkthrough"></a>Detaljni vodič

Sljedeći koraci će vas voditi kroz potrebne korake za konfiguriranje neprekinuti implementacije u Dodavanje veze za VANJSKIH Team Services 

1.  Uređivanje definicije Sastavi Dodavanje veze za VANJSKIH Team Services i dodajte je korak Sastavi Azure PowerShell. Odaberite definiciju Sastavi kategoriji **Sastavljanje definicije** , a zatim odaberite **Uredi** vezu.

    ![][0]

1.  Dodavanje novog koraka za sastavljanje **Azure PowerShell** definiciji Sastavi, a zatim odaberite **Dodaj sastavljanje korak...** gumb.

    ![][1]

1.  Odaberite kategoriju **zadatka uvođenja** , odaberite zadatak **Azure PowerShell** , a zatim njegov gumb **Dodaj** .

    ![][2]

1.  Odaberite koraka za sastavljanje **Azure PowerShell** , a zatim ispunite vrijednosti.

    1.  Ako već imate krajnje Azure service dodali Dodavanje veze za VANJSKIH Team Services, odaberite pretplatu u **Pretplatu Azure** padajućeg okvira s popisom, a zatim prijeđite na sljedeći odjeljak. 

        Ako nemate krajnje Azure service Dodavanje veze za VANJSKIH Team Services, morat ćete dodati. U ovom Podsekcija vodi vas kroz postupak. Ako vaš račun za Azure koristi Microsoftova računa (kao što su Hotmail), morat ćete poduzeti sljedeće korake da biste dobili provjere autentičnosti za usluge.

    1.  Odaberite vezu **Upravljanje** uz **Pretplatu Azure** padajući okvir s popisom.

        ![][3]

    1. Odaberite **Azure** **Krajnju točku servisa** padajućeg okvira s popisom.

        ![][4]

    1.  U dijaloškom okviru **Dodavanje Azure pretplate** odaberite mogućnost **Usluge** .

        ![][5]

    1.  Dodavanje informacija o pretplati Azure dijaloški okvir **Dodavanje Azure pretplate** . Morat ćete unijeti sljedeće stavke:
        -   Id pretplate
        -   Naziv pretplate
        -   Id usluge
        -   Ključ za usluge
        -   Id klijenta

    1.  Naziv koji želite dodati u okvir naziv **pretplate** . Ta vrijednost pojavit će se u nastavku **Azure pretplate** padajućeg popisa u Dodavanje veze za VANJSKIH Team Services. 

    1.  Ako ne znate svoj ID Azure pretplatu, možete koristiti jedan od sljedećih naredbi da biste ga.
        
        Da biste postigli skripte komponente PowerShell koristite:

        `Get-AzureRmSubscription`

        Da biste postigli EŽA Azure, koristite:

        `azure account show`
    

    1.  Da biste dobili usluge ID-a, ključ za usluge i ID klijenta, slijedite postupak u [aplikaciju za stvaranje servisa Active Directory i usluge pomoću portala](resource-group-create-service-principal-portal.md) ili [provjere autentičnosti glavni servis pomoću upravitelja za Azure resursa](resource-group-authenticate-service-principal.md).

    1.  Zbrajanje vrijednosti ID usluge i usluge ključ ID klijenta u dijaloški okvir **Dodavanje Azure pretplate** , a zatim odaberite gumb **u redu** .

        Sada imate valjani glavni servis za pokrenuti skriptu Azure PowerShell.

1.  Uređivanje definicije Sastavi, a zatim odaberite koraka za sastavljanje **Azure PowerShell** . Odaberite pretplatu u **Pretplatu Azure** padajućeg okvira s popisom. (Ako pretplate ne prikazuje, odaberite gumb **Osvježi** dalje veza **Upravljaj** .) 

    ![][8]

1.  Unesite put do skriptu PowerShell uvođenja AzureResourceGroup.ps1. Da biste to učinili, odaberite gumb tri točke (...) pokraj okvira **Put skripte** , dođite do skriptu PowerShell uvođenja AzureResourceGroup.ps1 u mapi **skripte** projekta, odaberite ga, a zatim gumb **u redu** . 

    ![][9]

1. Nakon što odaberete skriptu, ažurirajte put skripte tako da se pokreće sa Build.StagingDirectory (isti imenik u *ArtifactsLocation* postavljeno na). To možete učiniti tako da dodate "$(Build.StagingDirectory)/"na početak put skripte.

    ![][10]

1.  U okviru **Skripte argumenti** unesite sljedećih parametara (u jednom retku). Kada pokrenete skriptu u Visual Studio, vidjet ćete kako Dodavanje veze za VANJSKIH koristi parametara u **izlaznom** prozoru. Možete koristiti ovo kao početnu točku za postavljanje vrijednosti parametara u vaš korak Sastavi.

  	| Parametar | Opis|
  	|---|---|
  	| -ResourceGroupLocation           | Vrijednost zemlj mjesto gdje se nalazi, kao što su **eastus** ili **' Istočni sad '**grupu resursa. (Jednostrukim navodnicima dodavanje Ako naziv razmak) Dodatne informacije potražite u članku [Azure područja](https://azure.microsoft.com/en-us/regions/) .|                                                                                                                                                                                                                              |
  	| -ResourceGroupName               | Naziv grupe resursa koji se koristi za ovaj implementacije.|                                                                                                                                                                                                                                                                                                                                                                                                                |
  	| -UploadArtifacts                 | Ovaj parametar, ako postoje, navodi da artefakte moraju prenijeti Azure s lokalnog sustava. Samo morate postaviti taj parametar ako implementaciju sustava predložak zahtijeva dodatni artefakte koju želite faze pomoću skriptu komponente PowerShell (primjerice konfiguracije skripte ili ugniježđene predlošci).                                                                                                                                                                 |
  	| -StorageAccountName              | Naziv računa za pohranu koji se koristi u fazi artefakte ovaj implementacije. Ovaj parametar potreban je samo ako kopirate artefakte Azure. Taj račun za pohranu neće se automatski stvoriti tako da implementaciju, već mora postojati.|                                                                                                                                                                                                                     |
  	| -StorageAccountResourceGroupName | Naziv grupe resursa povezanu s računom za pohranu. Ovaj parametar potreban je samo ako kopirate artefakte Azure.|                                                                                                                                                                                                                                                                                                                               |
  	| -TemplateFile                    | Put do datoteke predloška u programu project za implementaciju Azure grupu resursa. Da biste poboljšali fleksibilnost koristiti put za taj parametar koji se odnosi na mjesto skriptu PowerShell umjesto apsolutni put.|
  	| -TemplateParametersFile          | Put do datoteke parametara u programu project za implementaciju Azure grupu resursa. Da biste poboljšali fleksibilnost koristiti put za taj parametar koji se odnosi na mjesto skriptu PowerShell umjesto apsolutni put.|
  	| -ArtifactStagingDirectory        | Ovaj parametar omogućuje PowerShell skripte informacije u mapu na koju treba kopirali binarne datoteke u projekt. Ta vrijednost nadjačava zadane vrijednosti koje se koriste skriptu PowerShell. Za dodavanje veze za VANJSKIH Team Services koristili, postavite vrijednost na: $(Build.StagingDirectory) - ArtifactStagingDirectory                                                                                                                                                                                              |

    Evo primjera skripte argumente (redak je prekinuta zbog čitljivosti):

    ``` 
    -ResourceGroupName 'MyGroup' -ResourceGroupLocation 'eastus' -TemplateFile '..\templates\azuredeploy.json' 
    -TemplateParametersFile '..\templates\azuredeploy.parameters.json' -UploadArtifacts -StorageAccountName 'mystorageacct' 
    –StorageAccountResourceGroupName 'Default-Storage-EastUS' -ArtifactStagingDirectory '$(Build.StagingDirectory)' 
    ```

    Kada završite, okvir **Argumenti skripte** trebali biste otprilike ovako.

    ![][11]

1.  Kada dodate sve potrebne stavke PowerShell Azure sastavljanje korak, odaberite u **redu čekanja** sastavljanje gumb da biste sastavili projekta. Na zaslonu **Sastavljanje** prikazan Izlaz iz skriptu PowerShell.

## <a name="next-steps"></a>Daljnji koraci

[Voditelj resursa Azure pregled](azure-resource-manager/resource-group-overview.md) da biste saznali više o Azure Voditelj resursa i grupa Azure resursa za čitanje.


[0]: ./media/vs-azure-tools-resource-groups-ci-in-vsts/walkthrough1.png
[1]: ./media/vs-azure-tools-resource-groups-ci-in-vsts/walkthrough2.png
[2]: ./media/vs-azure-tools-resource-groups-ci-in-vsts/walkthrough3.png
[3]: ./media/vs-azure-tools-resource-groups-ci-in-vsts/walkthrough4.png
[4]: ./media/vs-azure-tools-resource-groups-ci-in-vsts/walkthrough5.png
[5]: ./media/vs-azure-tools-resource-groups-ci-in-vsts/walkthrough6.png
[8]: ./media/vs-azure-tools-resource-groups-ci-in-vsts/walkthrough9.png
[9]: ./media/vs-azure-tools-resource-groups-ci-in-vsts/walkthrough10.png
[10]: ./media/vs-azure-tools-resource-groups-ci-in-vsts/walkthrough11b.png
[11]: ./media/vs-azure-tools-resource-groups-ci-in-vsts/walkthrough12.png
