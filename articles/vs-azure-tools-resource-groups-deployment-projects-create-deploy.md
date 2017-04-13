<properties
   pageTitle="Azure resursa grupe Visual Studio projekti | Microsoft Azure"
   description="Da biste stvorili grupni projekt Azure resursa i implementirati resurse za Azure pomoću programa Visual Studio."
   services="azure-resource-manager"
   documentationCenter="na"
   authors="tfitzmac"
   manager="timlt"
   editor="tysonn" />
<tags
   ms.service="azure-resource-manager"
   ms.devlang="multiple"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="09/20/2016"
   ms.author="tomfitz" />

# <a name="creating-and-deploying-azure-resource-groups-through-visual-studio"></a>Stvaranje i implementacija grupe Azure resursa kroz Visual Studio

S Visual Studio i [Azure SDK](https://azure.microsoft.com/downloads/), možete stvoriti projekt koji implementira infrastrukturu i kod Azure. Na primjer, možete definirati glavno računalo web, web-mjesta i baze podataka za aplikaciju i implementacija te infrastrukture uz kod. Ili možete definirati virtualnog računala, virtualne mreže i račun za pohranu i implementirati taj infrastrukture uz skriptu koja se izvršava na virtualnog računala. Projekt implementacije **Grupa resursa Azure** omogućuje vam za implementaciju sve potrebne resurse u jednom, kontrolirani postupak. Dodatne informacije o implementaciji i upravljanje resursa potražite u članku [Pregled upravljanja resursima Azure](azure-resource-manager/resource-group-overview.md).

Azure resursa grupne projekte sadrže Azure resursima JSON predložaka koji određuju resursa koji implementirati Azure. Dodatne informacije o elementima resursima predložak, potražite u članku [Upravitelj resursa za Azure za izradu predložaka](resource-group-authoring-templates.md). Visual Studio omogućuje vam uređivanje tih predložaka i omogućuje alate koji Pojednostavnite radu s predlošcima.

U ovoj se temi implementacija web app i SQL baze podataka. Međutim, koraci su gotovo bilo koju vrstu resursa. Možete kao jednostavno implementirati virtualnog računala i njegov povezani resursi. Visual Studio sadrži mnogo različitih starter predložaka za implementaciju uobičajeni scenariji.

U ovom se članku prikazuje Visual Studio 2015 ažuriranje 2 i Microsoft Azure SDK za .NET 2.9. Ako koristite Visual Studio 2013 s Azure SDK 2.9, iskustva uglavnom ista je. Možete koristiti verzije SDK Azure s 2,6 ili kasnije; Međutim, iskustva korisničkog sučelja programa mogu se razlikovati od korisničkog sučelja koji se prikazuju u ovom članku. Preporučujemo da instalirate najnoviju verziju programa [Azure SDK](https://azure.microsoft.com/downloads/) prije pokretanja postupka. 

## <a name="create-azure-resource-group-project"></a>Grupa resursa za Azure Stvaranje projekta

U ovom postupku pomoću **web-aplikacije + SQL** predloška stvorite projekta programa Azure grupu resursa.

1. U Visual Studio, odaberite **datoteku**, **Novi projekt**, odaberite **C#** ili **Visual Basic**. Odaberite **oblaka**, a zatim odaberite **Grupu resursa Azure** projekta.

    ![Oblak implementacije projekta](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/create-project.png)

1. Odaberite predložak koji želite uvesti za Azure Voditelj resursa. Obratite pozornost na to ima mnogo različitih mogućnosti temelje se na vrstu projekta želite uvesti. Odaberite predložak **web-aplikacije + SQL** ove teme.

    ![Odabir predloška](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/select-project.png)

    Predložak koji ste odabrali je početna točka; možete dodati i ukloniti resurse za ispunjavanje scenariju.

    >[AZURE.NOTE] Visual Studio dohvaća se popis dostupnih predložaka putem Interneta. Na popisu mogu se promijeniti.

    Visual Studio stvara projekt implementacije grupu resursa za web-aplikacije i SQL baze podataka.

1. Da biste vidjeli što ste napravili, proširite čvorove u programu project za implementaciju.

    ![Prikaži čvorove](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/show-items.png)

    Budući da ne možemo odabrali Web app + SQL predložak u ovom primjeru, vidjet ćete sljedeće datoteke: 

  	|Naziv datoteke|Opis|
  	|---|---|
  	|Implementacija AzureResourceGroup.ps1|PowerShell skriptu koja se poziva naredbe ljuske PowerShell za implementaciju za Azure Voditelj resursa.<br />**Napomena** Visual Studio koristi ovu skriptu PowerShell uvođenje predloška. Sve promjene koje napravite u ovom skriptu utječe na implementacije u Visual Studio, pa budite oprezni.|
  	|WebSiteSQLDatabase.json|Voditelj resursa predložak koji definira Infrastruktura želite implementirati Azure i parametre možete unijeti tijekom implementacije. Određuje i međuzavisnosti resursi kako Voditelj resursa uvodi resursa u točno određenim redoslijedom.|
  	|WebSiteSQLDatabase.parameters.json|Parametri datoteka koja sadrži vrijednosti koje su potrebne za predložak. Prenesite u vrijednosti parametara da biste prilagodili svaki implementacije.|

    Svi projekti implementacije grupu resursa sadrži ove osnovne datoteke. Projekata može sadržavati dodatne datoteke za podršku druge funkcije.

## <a name="customize-the-resource-manager-template"></a>Prilagođavanje predložaka Voditelj resursa

Uvođenje projekta možete prilagoditi izmjenom JSON predloške koje opisuju resursa koje želite uvesti. JSON označava za označavanje objekta JavaScript, a serijaliziranog podataka oblik koji je stvorio je lako raditi. Datoteka JSON pomoću sheme koje upućuju na vrhu svake datoteke. Ako želite da biste shvatili shemu, možete preuzeti i analizirati. Shema definira elemente koje su valjane, vrste i oblici polja, moguće vrijednosti numeriranog vrijednosti i tako dalje. Dodatne informacije o elementima resursima predložak, potražite u članku [Upravitelj resursa za Azure za izradu predložaka](resource-group-authoring-templates.md).

Da biste radili na vašem predlošku, otvorite **WebSiteSQLDatabase.json**.

Uređivač Visual Studio nudi alate radi jednostavnijeg uređivanja predložak Voditelj resursa. Prozor **Struktura JSON** olakšava da biste vidjeli elemente definirane u predlošku.

![Prikaz strukture JSON](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/show-json-outline.png)

Odabir svih elemenata u strukturi vodi vas na taj dio predloška web-mjesta i ističe odgovarajućih JSON.

![Pronađite JSON](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/navigate-json.png)

Resursa možete dodati odabirom ili gumb **Dodavanje resursa** pri vrhu prozora JSON strukture ili desnom tipkom miša kliknete **resurse** i odabirom **Dodajte novi resurs**.

![Dodavanje resursa](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/add-resource.png)

U ovom ćete praktičnom vodiču odaberite **Račun za pohranu** i dajte naziv. Navedite naziv koji je više od 11 znakova i sadrži samo brojeve i mala slova.

![Dodavanje prostora za pohranu](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/add-storage.png)

Uočite da ne samo je resurs koji je dodan, ali i parametar vrsta računa za pohranu i varijablu za naziv računa za pohranu.

![Prikaz strukture](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/show-new-items.png)

Parametar **storageType** je unaprijed definiranih s dopuštene vrste i zadanu vrstu. Možete ostaviti te vrijednosti ili ih uredite za vašu situaciju. Ako ne želite svima uvesti na račun za pohranu **Premium_LRS** pomoću ovog predloška, uklonite ga s dopuštene vrste. 

    "storageType": {
      "type": "string",
      "defaultValue": "Standard_LRS",
      "allowedValues": [
        "Standard_LRS",
        "Standard_ZRS",
        "Standard_GRS",
        "Standard_RAGRS"
      ]
    }

Visual Studio omogućuje intellisense pomoću kojih se objašnjava što svojstva su dostupne prilikom uređivanja predložak. Ako, na primjer, da biste uredili svojstva aplikacije servisa za tarifa, dođite do **HostingPlan** resursa i dodavanje vrijednosti za **Svojstva**. Obratite pozornost toj intellisense prikazuje dostupne vrijednosti, a sadrži opis tu vrijednost.

![Prikaži intellisense](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/show-intellisense.png)

**NumberOfWorkers** postavite na 1.

    "properties": {
      "name": "[parameters('hostingPlanName')]",
      "numberOfWorkers": 1
    }

## <a name="deploy-the-resource-group-project-to-azure"></a>Grupa resursa projekta implementirati Azure

Sada ste spremni za implementaciju projekta. Ako pokrenete projekta programa Azure grupu resursa, morate je implementirati u grupu Azure resursa. Grupa resursa je logički grupiranja resursa koji zajednički koristite uobičajenih životni ciklus.

1. Na izborniku prečacu čvor projekta implementacije odaberite **uvođenja** > **Novu implementaciju**.

    ![Implementirajte novu implementaciju stavka izbornika](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/deploy.png)

    Pojavit će se dijaloški okvir **uvođenja u grupu resursa** .

    ![Implementacija dijaloški okvir grupa resursa](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/show-deployment.png)

1. U okvir padajući popis **grupa resursa** odaberite postojeću grupu resursa ili stvorite novi. Da biste stvorili grupu resursa, otvorite okvir padajući popis **Grupa resursa** , pa odaberite **Stvori novo**.

    ![Implementacija dijaloški okvir grupa resursa](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/create-new-group.png)

    Pojavit će se dijaloški okvir **Stvori grupu resursa** . Dati grupi naziv i lokaciju, a zatim odaberite gumb **Stvori** .

    ![Grupa resursa dijaloški okvir stvaranje](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/create-resource-group.png)
   
1. Uređivanje parametara za implementaciju tako da odaberete gumb **Uređivanje parametara** .

    ![Slika gumba parametara](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/edit-parameters.png)

1. Unesite vrijednosti za parametre prazan, a zatim odaberite gumb **Spremi** . Prazan Parametri se **hostingPlanName**, **administratorLogin**, **administratorLoginPassword**i **ImeBazePodataka**.

    **hostingPlanName** određuje naziv [aplikacije servisa za plan](./app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md) da biste stvorili. 
    
    **administratorLogin** određuje korisničko ime za administratora sustava SQL Server. Nemojte koristiti uobičajena imena administrator kao što su **Pacifička** ili **administrator**. 
    
    **AdministratorLoginPassword** određuje lozinke za administratore sustava SQL Server. Mogućnost **Spremi lozinke u obliku običnog teksta u datoteci s parametrima** nije sigurna; Dakle, odaberite ovu mogućnost. Budući da lozinku neće spremiti kao običan tekst, morat ćete unijeti lozinku ponovno tijekom implementacije. 
    
    **ImeBazePodataka** određuje naziv baze podataka da biste stvorili. 

    ![Parametri dijaloški okvir Uređivanje](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/provide-parameters.png)
    
1. Odaberite gumb **uvođenja** za implementaciju projekta za Azure. Otvorit će se konzole za PowerShell izvan instancu Visual Studio. Na konzoli PowerShell kada se to od vas zatraži, unesite administratorsku lozinku sustava SQL Server. **Konzole za PowerShell je možda skriven iza ostalih stavki ili je minimiziran na programskoj traci.** Pronađite konzole i odaberite je lozinka.

    >[AZURE.NOTE] Visual Studio može od vas tražiti da biste instalirali cmdleta Azure PowerShell. Potreban vam je Azure PowerShell cmdleti za uspješno uvođenje grupa resursa. Ako se to od vas zatraži, instalirajte ih.
    
1. Uvođenje može potrajati nekoliko minuta. U sustavu windows **Izlaz** vidjeti status implementacije. Po završetku implementaciju, zadnju poruku označava uspješno uvođenje s nešto slično kao:

        ... 
        18:00:58 - Successfully deployed template 'c:\users\user\documents\visual studio 2015\projects\azureresourcegroup1\azureresourcegroup1\templates\websitesqldatabase.json' to resource group 'DemoSiteGroup'.


1. U pregledniku otvorite [portal za Azure](https://portal.azure.com/) i prijavite se na račun. Da biste vidjeli grupu resursa, odaberite **grupe resursa** i implementiran na grupu resursa.

    ![Odaberite grupu](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/select-group.png)

1. Pogledajte sve distribuiranih resurse. Primijetite da naziv računa spremišta nije potpuno što ste naveli prilikom dodavanja resursa. Račun za pohranu mora biti jedinstvena. Predložak automatski dodaje niz znakova ime koje ste naveli Navedite jedinstveni naziv. 

    ![prikaz resursa](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/show-deployed-resources.png)

1. Ako promjene, a želite implementirati projekta, odaberite postojeću grupu resursa na izborniku prečaca Azure resursa grupe projekta. Na izborniku prečacu odaberite **uvođenja**, a zatim odaberite grupu resursa koji je implementiran.

    ![Grupa Azure resursa implementiran](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/redeploy.png)

## <a name="deploy-code-with-your-infrastructure"></a>Implementacija kod s sustavu

Sada ste implementiran infrastrukturu za aplikaciju, ali nema stvarni šifre uvode se sa projekta. U ovoj se temi objašnjava implementacija web-aplikacijama i tablice baze podataka SQL tijekom implementacije. Ako implementirate virtualnog računala umjesto web-aplikacijama, koju želite pokrenuti neke kod na računalu kao dio implementacije. Postupak za implementaciju kod za web-aplikacije ili za postavljanje virtualnog računala je gotovo.

1. Dodajte projekta za Visual Studio rješenje. Desnom tipkom miša kliknite rješenje, a zatim odaberite **Dodaj** > **Novi projekt**.

    ![Dodavanje projekta](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/add-project.png)

1. Dodajte **ASP.NET web-aplikacije**. 

    ![Dodavanje web-aplikacije](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/add-app.png)
    
1. Odaberite **MVC** i poništite polje za **glavnog računala u oblaku** jer grupni projekt resursa izvodi taj zadatak.

    ![Odaberite MVC](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/select-mvc.png)
    
1. Nakon što Visual Studio Stvori web-aplikaciju programa, vidjet ćete i projektima u rješenje.

    ![Prikaz projekata](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/show-projects.png)

1. Sada ćete morati provjerite nalazi li se u grupi projekt resursa svjesni novi projekt. Vratite se u grupi projekt resursa (AzureResourceGroup1). Desnom tipkom miša kliknite **reference** , a zatim odaberite **Dodaj referencu**.

    ![Dodavanje reference](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/add-new-reference.png)

1. Odaberite project web app koji ste stvorili.

    ![Dodavanje reference](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/add-reference.png)
    
    Dodavanjem referenca veza aplikacije project web na grupnom projektu resursa i automatski postavlja tri svojstvima ključa. Vidjet ćete tih svojstava u prozoru **Svojstva** za referencu.

      ![potražite u članku referenca](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/see-reference.png)
    
    Svojstva su:

    - **Dodatna svojstva** sadrži paketa za implementaciju web pripremna mjesto na kojem se pomiču se Azure za pohranu. Imajte na umu mape (ExampleApp) i datoteke (package.zip). Će odrediti te vrijednosti kao parametar kada implementacija aplikacije. 
    - **Uvrstite put do datoteke** sadrži put na kojem je stvorena paketa. **Uključiti ciljnih web-mjesta** sadrži naredbu koja se izvršava implementacije. 
    - Sastavljanje zadanu vrijednost od **; Paket** omogućuje implementacije omogućuje stvaranje i stvaranje paketa za implementaciju web (package.zip).  
    
    Ne morate profil Objavi kao implementacijskih dohvaća potrebne informacije iz svojstva da biste stvorili paketa.
      
1. Dodavanje resursa u predlošku.

    ![Dodavanje resursa](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/add-resource-2.png)

1. Ovaj put odaberite **Web implementacija web-aplikacijama**. 

    ![Dodavanje web implementacije](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/add-web-deploy.png)
    
1. Ponovno implementirate projekta grupu resursa u grupu resursa. Trenutno postoje neki nove parametre. Ne morate unesite vrijednosti za **_artifactsLocation** ili **_artifactsLocationSasToken** jer Visual Studio automatski generira te se vrijednosti. Međutim, morati postaviti mapu i naziv datoteke da biste put koji sadrži paketa za implementaciju (prikazanom kao **ExampleAppPackageFolder** i **ExampleAppPackageFileName** na sljedećoj slici). Sadrže vrijednosti koje se prikazivalo u svojstva reference (**ExampleApp** i **package.zip**).

    ![Dodavanje web implementacije](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/set-new-parameters.png)
    
    Za **račun artefakt prostora za pohranu**, odaberite onaj koji je implementiran s ovu grupu resursa.
    
1. Nakon implementacijskih je završio, odaberite web-aplikaciju programa na portalu. Odaberite URL-a da biste pronašli web-mjesta.

    ![pregledavanje web-mjesta](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/browse-site.png)

1. Obratite pozornost na to uspješno implementiran zadane aplikacije ASP.NET.

    ![Prikaži distribuiranih aplikacije](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/show-deployed-app.png)

## <a name="next-steps"></a>Daljnji koraci

- Informacije o upravljanju resurse putem portala sustava potražite u članku [pomoću portala za Azure upravljanja Azure resurse](./azure-portal/resource-group-portal.md).
- Dodatne informacije o predlošcima potražite u članku [Predlošci Voditelj resursa za Azure autorizaciju](resource-group-authoring-templates.md).
