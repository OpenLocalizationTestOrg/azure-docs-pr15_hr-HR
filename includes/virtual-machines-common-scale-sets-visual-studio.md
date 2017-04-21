

U ovom se članku objašnjava za implementaciju sustava Azure virtualnog računala skaliranje skupa pomoću Visual Studio resursa grupe implementacije.


[Azure virtualnog računala skaliranje skupovi](https://azure.microsoft.com/blog/azure-vm-scale-sets-public-preview/) su resursa i upravljanje skupovima slične virtualnim strojevima jednostavno integrirane mogućnosti za automatsko skaliranje i uravnoteženje opterećenja za Azure izračunati. Možete Dodjela resursa i implementirati VM skaliranje skupove pomoću [predložaka Azure resursa Manager (ARM)](https://github.com/Azure/azure-quickstart-templates). Predlošci ARM može uvesti pomoću REST Azure EŽA, komponente PowerShell i izravno iz Visual Studio. Visual Studio pruža skup primjer predloške koje možete uvesti kao dio Azure resursa grupe implementacije projekta.

Grupa resursa Azure implementacijama su način grupirati i objavljivanje skup povezanih Azure resursa u operaciji jedan implementacije. Dodatne informacije o njima ovdje: [Stvaranje i implementacija grupe Azure resursa kroz Visual Studio](../vs-azure-tools-resource-groups-deployment-projects-create-deploy/).

## <a name="pre-requisites"></a>Prije requisites

Da biste započeli implementacija VM skaliranje skupova u Visual Studio potrebno je sljedeće:

- Visual Studio 2013 ili 2015.
- Azure SDK 2.7 ili 2,8

Napomena: Ove upute pretpostavlja da koristite Visual Studio 2015 sa [2,8 SDK Azure](https://azure.microsoft.com/blog/announcing-the-azure-sdk-2-8-for-net/).

## <a name="creating-a-project"></a>Stvaranje projekta

1. Stvorite novi projekt u Visual Studio 2015 tako da odaberete **datoteku | Novi | Project**.

    ![Datoteka novo][file_new]

2. U odjeljku **Visual C# | Oblak**, odaberite **Upravitelj resursa Azure** Stvaranje projekta za implementaciju predložak OKVIRA.

    ![Stvaranje projekta][create_project]

3.  Na popisu predložaka odaberite Linux ili Windows virtualnog računala skaliranje postavljanje predloška.

    ![Odaberite predložak][select_Template]

4. Nakon stvaranja projekta vidjet ćete skripte komponente PowerShell implementaciju, predložak resursima Azure i parametar datoteke za postavljanje skaliranje virtualnog računala.

    ![Preglednik rješenja][solution_explorer]

## <a name="customize-your-project"></a>Prilagodba projekta

Sada možete urediti predložak tako da ga prilagoditi potrebama vaše aplikacije, kao što su svojstva VM proširenje Dodavanje ili uređivanje pravila za ujednačavanje opterećenja. Prema zadanim postavkama VM skaliranje postavljanje predložaka konfigurirane za implementaciju AzureDiagnostics nastavak koji olakšava dodati pravila za automatsko skaliranje. Također uvodi opterećenja javnu IP adresom, s ulazna NAT pravila koja omogućuju povezati instance VM SSH (Linux) ili RDP (Windows) – raspon priključaka sučelje započinje 50000, što znači da se slučaju Linux, ako ste SSH priključak 50000 javnu IP adrese (ili naziv domene) koji će usmjeriti priključak 22 prvi VM u skupu mjerilo. Povezivanje s priključkom 50001 će biti proslijeđene priključak 22 drugi VM i tako dalje.

 Dobar način za uređivanje predložaka s Visual Studio je da biste koristili strukturu JSON da biste organizirali parametre, varijable i resursa. Poznavati sheme Visual Studio možete pokažite pogreški u predlošku prije njegove implementacije kod.

![JSON Explorer][json_explorer]

## <a name="deploy-the-project"></a>Implementacija projekta

6. Uvođenje predloška OKVIRA za Azure da biste stvorili resursa postavite skaliranje VM. Desnom tipkom miša kliknite čvor projekta, odaberite **uvođenja | Novi implementaciju**.

    ![Uvođenje predloška][5deploy_Template]

7. Odaberite pretplatu u dijaloškom okviru "Implementacija u grupu resursa".

    ![Uvođenje predloška][6deploy_Template]

8. Na tom mjestu možete stvoriti i novu grupu resursa Azure uvođenje predloška.

    ![Nova grupa resursa][new_resource]

9. Sljedeće odaberite gumb **Uređivanje parametara** unesite parametre koji će se proslijediti predlošku određene vrijednosti kao što su korisničko ime i lozinku za os-a su potrebne za stvaranje uvođenje.

    ![Uređivanje parametara][edit_parameters]

10. Sada kliknite **Implementiraj**. U **izlaznom** prozoru vode tijeku implementacije. Imajte na umu da se skripte **Uvođenja AzureResourceGroup.ps1** izvršava akciju.

    ![Izlaznom prozoru][output_window]

## <a name="exploring-your-vm-scale-set"></a>Istraživanje skupu VM skala

Kada se dovrši implementaciju, novi skup za VM skaliranje možete pogledati u Visual Studio **Oblaka Explorer** (osvježite popis). Oblak Explorer omogućuje upravljanje Azure resursa u Visual Studio pri razvoju aplikacija. Postavljanje VM skaliranje možete pogledati i u Azure Portal i Explorer Azure resursa.

![Oblak Explorer][cloud_explorer]

 Portal sustava predstavlja najbolji način vizualno upravljanje preduvjete Azure infrastrukture s web-preglednik, dok Azure resursa Explorer omogućuje jednostavnu explorer i ispravljanje pogrešaka Azure resursa, dodjeljivanja prozora u "prikaz instance" i i prikaz naredbe ljuske PowerShell za resurse gledate. Dok su skupovi skaliranje VM u pretpregledu, Explorer resursa prikazat će Većina detalje za svoje skupove VM mjerilo.

## <a name="next-steps"></a>Daljnji koraci

Nakon što ste uspješno implementiran VM skaliranje skupove kroz Visual Studio možete dodatno prilagoditi projekta u skladu s potrebama aplikacije. Na primjer postavljanje automatsko skaliranje dodavanjem do uvida resursa, dodavanje infrastrukture predložak kao samostalni VMs, ili implementacija aplikacije koje koriste nastavak prilagođene skripte. Dobar izvor predlošci primjera pronaći ćete u spremištu GitHub [Azure predložaka za brzi početak rada](https://github.com/Azure/azure-quickstart-templates) (traženje "vmss").

[file_new]: ./media/virtual-machines-common-scale-sets-visual-studio/1-FileNew.png
[create_project]: ./media/virtual-machines-common-scale-sets-visual-studio/2-CreateProject.png
[select_Template]: ./media/virtual-machines-common-scale-sets-visual-studio/3b-SelectTemplateLin.png
[solution_explorer]: ./media/virtual-machines-common-scale-sets-visual-studio/4-SolutionExplorer.png
[json_explorer]: ./media/virtual-machines-common-scale-sets-visual-studio/10-JsonExplorer.png
[5deploy_Template]: ./media/virtual-machines-common-scale-sets-visual-studio/5-DeployTemplate.png
[6deploy_Template]: ./media/virtual-machines-common-scale-sets-visual-studio/6-DeployTemplate.png
[new_resource]: ./media/virtual-machines-common-scale-sets-visual-studio/7-NewResourceGroup.png
[edit_parameters]: ./media/virtual-machines-common-scale-sets-visual-studio/8-EditParameter.png
[output_window]: ./media/virtual-machines-common-scale-sets-visual-studio/9-Output.png
[cloud_explorer]: ./media/virtual-machines-common-scale-sets-visual-studio/12-CloudExplorer.png