<properties
    pageTitle="Azure DevTest Labs najčešća pitanja vezana uz | Microsoft Azure"
    description="Pronađite odgovore na najčešća pitanja Azure DevTest Labs"
    services="devtest-lab,virtual-machines"
    documentationCenter="na"
    authors="tomarcher"
    manager="douge"
    editor=""/>

<tags
    ms.service="devtest-lab"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/13/2016"
    ms.author="tarcher"/>

# <a name="azure-devtest-labs-faq"></a>Azure DevTest Labs najčešća Pitanja

U ovom se članku navedeni odgovori na neka od najčešćih pitanja o Azure DevTest Labs.

[AZURE.INCLUDE [devtest-lab-try-it-out](../../includes/devtest-lab-try-it-out.md)]

## <a name="general"></a>Općenito
- [Što ako moje pitanje niste pronašli odgovor ovdje?](#what-if-my-question-isnt-answered-here)
- [Zašto koristiti Azure DevTest Labs?](#why-should-i-use-azure-devtest-labs) 
- [Što znači "nemojte brinuti bez, samostalno"?](#what-does-quotworry-free-self-servicequot-mean)
- [Kako koristiti Azure DevTest Labs?](#how-can-i-use-azure-devtest-labs) 
- [Kako se naplatiti za Azure DevTest Labs?](#how-am-i-billed-for-azure-devtest-labs) 
 
## <a name="security"></a>Sigurnost 
- [Što su na različitim razinama sigurnosti u Azure DevTest Labs?](#what-are-the-different-security-levels-in-azure-devtest-labs) 
- [Kako stvoriti uloge da biste korisnicima omogućili obavljanje određenog zadatka?](#how-do-i-create-a-role-to-allow-users-to-perform-a-specific-task) 
 
## <a name="cicd-integration--automation"></a>Integracija CI/CD-a i automatizacije 
 
- [Ne Azure DevTest Labs integrirati s Moje toolchain CI/CD-a?](#does-azure-devtest-labs-integrate-with-my-cicd-toolchain) 
 
## <a name="virtual-machines"></a>Virtualnim strojevima 
 
- [Zašto ne mogu vidjeti neke VMs u virtualnim računalima sustava Azure plohu koji se prikazuje unutar Azure DevTest Labs?](#why-cant-i-see-certain-vms-in-the-azure-virtual-machines-blade-that-i-see-within-azure-devtest-labs) 
- [Koja je razlika između prilagođene slike i formula?](#what-is-the-difference-between-custom-images-and-formulas) 
- [Kako stvoriti više VMs od istog predloška odjednom?](#how-do-i-create-multiple-vms-from-the-same-template-at-once) 
- [Kretanja Moje postojeće Azure VMs u moj Laboratorija Azure DevTest Labs](#how-do-i-move-my-existing-azure-vms-into-my-azure-devtest-labs-lab) 
- [Mogu li priložiti više diskova Moje VMs?](#can-i-attach-multiple-disks-to-my-vms) 
- [Kako automatizirati postupak prijenos datoteka VHD da biste stvorili prilagođeni slike?](#how-do-i-automate-the-process-of-uploading-vhd-files-to-create-custom-images) 
- [Kako automatizirati postupak brisanja VMs u moj Laboratorija?](#how-can-i-automate-the-process-of-deleting-all-the-vms-in-my-lab)
 
## <a name="artifacts"></a>Artefakte 
 
- [Što su artefakte?](#what-are-artifacts) 
 
## <a name="lab-configuration"></a>Konfiguriranje Laboratorija 
 
- [Kako stvoriti na Laboratorija iz predloška Azure Voditelj resursa?](#how-do-i-create-a-lab-from-an-azure-resource-manager-template) 
- [Zašto se moj VMs stvaraju u grupama različite resursa proizvoljne nazivima? Možete preimenovati i izmjenu te grupe resursa?](#why-are-my-vms-created-in-different-resource-groups-with-arbitrary-names-can-i-rename-or-modify-these-resource-groups) 
- [Koliko labs možete stvoriti u odjeljku iste pretplate?](#how-many-labs-can-i-create-under-the-same-subscription)
- [Koliko VMs možete stvoriti po Laboratorija?](#how-many-vms-can-i-create-per-lab)
- [Kako se zajednički koriste izravnu vezu na moj Laboratorija?](#how-do-i-share-a-direct-link-to-my-lab)
- [Što je Microsoftov račun?](#what-is-a-microsoft-account)
 
## <a name="troubleshooting"></a>Otklanjanje poteškoća 
 
- [Moje artefakt nije uspjela tijekom stvaranja VM. Kako s njim?](#my-artifact-failed-during-vm-creation-how-do-i-troubleshoot-it) 
- [Zašto nije Moje postojeće virtualne mreže spremanja pravilno?](#why-isnt-my-existing-virtual-network-saving-properly)  

### <a name="what-if-my-question-isnt-answered-here"></a>Što ako moje pitanje niste pronašli odgovor ovdje?
Ako svoje pitanje nije ovdje navedeno, Javite nam i pomoći ćemo vam pronašli odgovor.

- Postavite pitanje u [Disqus niti](#comments) na kraju u ovim najčešćim Pitanjima i sudjelovati s tim Azure predmemorije i ostali članovi zajednice o ovog članka.
- Da biste doseći širu publiku, postavite pitanje na [forumu Azure DevTest Labs MSDN](https://social.msdn.microsoft.com/Forums/azure/home?forum=AzureDevTestLabs)i sudjelovati s tim Azure DevTest Labs i ostali članovi zajednice.
- Da bi značajka zahtjev, pošaljite zahtjeve i ideje [Azure DevTest Labs korisnika glas](https://feedback.azure.com/forums/320373-azure-devtest-labs).

### <a name="why-should-i-use-azure-devtest-labs"></a>Zašto koristiti Azure DevTest Labs? 
Azure DevTest Labs možete spremiti tima vremena i novca. Razvojni inženjeri možete stvoriti vlastite okruženja pomoću nekoliko različitih baza i pomoću artefakte možete brzo implementirati i konfigurirati aplikacije. Pomoću prilagođenih slike i formule, virtualnih računala mogu spremiti kao predloške i jednostavno reproducirati. Osim toga, labs nude nekoliko konfigurirati pravila koja administratorima Laboratorija smanjiti otpad i upravljanje njima u tim okruženjima. Ta pravila sadrže automatski-isključivanja, Prag trošak Maksimalna VMs po korisniku i maksimalne veličine VM. Za detaljnije objašnjenje Azure DevTest Labs, pročitajte [Pregled](devtest-lab-overview.md) ili pogledajte [Uvodni videozapis](/documentation/videos/videos/what-is-azure-devtest-labs). 

### <a name="what-does-worry-free-self-service-mean"></a>Što znači "nemojte brinuti bez, samostalno"?
"Nemojte brinuti slobodno, samostalne" znači da razvojnim inženjerima i mogućnost stvaranja vlastitog okruženja potrebnih pa administratori imaju sigurnost zna Azure DevTest Labs pomaže smanjiti zauzimati i kontrolirati trošak. Administratori možete odrediti koje veličina VM dopušteno, maksimalni broj VMs, i kada su VMs rada i isključiti. Azure DevTest Labs i olakšava praćenje troškova i postavljanje upozorenja pratite kako se koriste resursa u na Laboratorija. 

### <a name="how-can-i-use-azure-devtest-labs"></a>Kako koristiti Azure DevTest Labs? 
Azure DevTest Labs je korisna kad god zahtijevaju razvojni ili test okruženja, a želite ponoviti brzo i/ili upravljajte trošak spremanje pravila. 

Evo nekih scenarija koji korisnici koriste Azure DevTest Labs za: 

- Upravljanje razvojni i testiranje okruženja na jednom mjestu, korištenja pravila da biste smanjili trošak i prilagođene slike da biste zajednički koristili sastavlja preko tima.
- Razvoj aplikacija pomoću prilagođenih slike da biste spremili na disku stanje tijekom faze razvoja.
- Praćenje troškova odnosu napredak. 
- Stvaranje okruženja masovno test za testiranje Jamstvo kvalitete.
- Konfiguriranje i reproduciranje u aplikaciju na različitim okruženjima pomoću artefakte i formule. 
- Distribucija VMs za hackathons (suradnička razvojni ili test rada), a zatim jednostavno Poništi dodjelu resursa ih kada istekne događaj. 

### <a name="how-am-i-billed-for-azure-devtest-labs"></a>Kako se naplatiti za Azure DevTest Labs? 
Azure DevTest Labs je besplatna usluga, što znači da labs za stvaranje i konfiguriranje pravilnika, predložaka i artefakte je besplatno. Plaćate samo za Azure resursa koji se koriste u labs, kao što su virtualnim strojevima, račune za pohranu i virtualne mreže. Dodatne informacije o trošak Laboratorija resursa, Saznajte više o [Azure DevTest Labs cijene](https://azure.microsoft.com/pricing/details/devtest-lab/). 

### <a name="what-are-the-different-security-levels-in-azure-devtest-labs"></a>Što su na različitim razinama sigurnosti u Azure DevTest Labs?  
[Kontrola pristupa na Azure Role-Based (RBAC)](../active-directory/role-based-access-built-in-roles.md)ovisi o sigurnosti programa access. Da biste shvatili funkcioniranje programa access, lakše je razlike između dozvole, uloge i opseg kako je definirano parametrom RBAC.

- **Dozvole** – dozvole je definirani pristup određenu akciju. Na primjer, dozvole može biti čitanje za sve virtualnog računala. 
- **Uloga** - uloge je skup dozvola koje se mogu grupirati i Dodijeljeno korisniku. Na primjer, "vlasnik pretplate" ima pristup svim resursima unutar pretplatu. 
- **Opseg** - opseg je razine u hijerarhiji Azure resursa. Na primjer, opseg može biti grupu resursa ili jedan Laboratorija ili cijelu pretplatu. 
 
Unutar opseg Azure DevTest Labs, postoje dvije vrste uloga da biste definirali korisničkih dozvola: Laboratorija vlasnik i korisnik Laboratorija.

- **Vlasnik Laboratorija** - Laboratorija vlasnik ima pristup bilo koje resursima unutar na Laboratorija. Zbog toga mogu izmjena pravila, čitati i pisanje sve VMs, promjena virtualne mreže, i tako dalje. 
- **Korisnik Laboratorija** - korisnik Laboratorija možete prikazati sve Laboratorija resurse, kao što su VMs, pravila i virtualne mreže, ali ne možete mijenjati pravila ili bilo koju VMs stvorili drugi korisnici. I moguće je da biste stvorili prilagođene uloge u Azure DevTest Labs, a možete saznati kako se u članku [Dodjela korisničkih dozvola na određene Laboratorija pravila](devtest-lab-grant-user-permissions-to-specific-lab-policies.md). 
 
Budući da korisnik ima dozvole u određenim dosegu su hijerarhijski, rasponi, oni se automatski dobiti te dozvole u svakoj dosegu niže razine encompassed. Ako, primjerice, ako se korisniku Dodijeli ulogu vlasnik pretplate, zatim imaju pristup svim resursima u pretplatu. Ove resurse obuhvaćaju sve virtualnim strojevima, sve virtualne mreže i sve labs. Dakle, vlasnik pretplate automatski nasljeđuje ulogu Laboratorija vlasnik. Međutim, suprotno nije ispunjen. Vlasnik Laboratorija ima pristup Laboratorija koji je opseg manju od razinu pretplate. Stoga Laboratorija vlasnik nije možete vidjeti virtualnim strojevima ili virtualne mreže ili bilo kojeg resursa koje su izvan na Laboratorija. 

### <a name="how-do-i-create-a-role-to-allow-users-to-perform-a-specific-task"></a>Kako stvoriti uloge da biste korisnicima omogućili obavljanje određenog zadatka?
Ovdje se nalazi iscrpan članak o stvaranju prilagođene uloge i dodijelite dozvole te uloge. Evo primjera skriptu koja stvara uloga "DevTest Labs napredne korisnik", koja ima dozvole za pokretanje i zaustavljanje sve VMs u na Laboratorija:
 
    $policyRoleDef = Get-AzureRmRoleDefinition "DevTest Labs User" 
    $policyRoleDef.Actions.Remove('Microsoft.DevTestLab/Environments/*') 
    $policyRoleDef.Id = $null 
    $policyRoleDef.Name = "DevTest Labs Advance User" 
    $policyRoleDef.IsCustom = $true 
    $policyRoleDef.AssignableScopes.Clear() 
    $policyRoleDef.AssignableScopes.Add("subscriptions/<subscription Id>") 
    $policyRoleDef.Actions.Add("Microsoft.DevTestLab/labs/virtualMachines/Start/action") 
    $policyRoleDef.Actions.Add("Microsoft.DevTestLab/labs/virtualMachines/Stop/action") 
    $policyRoleDef = New-AzureRmRoleDefinition -Role $policyRoleDef  
 
### <a name="does-azure-devtest-labs-integrate-with-my-cicd-toolchain"></a>Ne Azure DevTest Labs integrirati s Moje toolchain CI/CD-a? 
Ako koristite VSTS, postoji [proširenje Azure DevTest Labs zadaci](https://marketplace.visualstudio.com/items?itemName=ms-azuredevtestlabs.tasks) koji omogućuje vam da biste automatizirali vaše izdanje kanal u Azure DevTest Labs. Koristi ovaj kućni broj obuhvaćaju sljedeće:

- Stvaranje i implementacija u VM automatski i konfiguriranje s ažuran pomoću kopiranja datoteka Azure ili PowerShell VSTS zadataka. 
- Automatski hvatanje stanje na VM nakon testiranje ponovno proizvesti pogrešku na isti VM za daljnje istrage. 
- Brisanje VM na kraju kanal izdanje kada više nije potrebna. 

Sljedeće članaka za blog sadrže upute i informacije o korištenju VSTS nastavak:
 
- [Azure Labs DevTest – VSTS proširenja](https://blogs.msdn.microsoft.com/devtestlab/2016/06/15/azure-devtest-labs-vsts-extension/) 
- [Implementacija novi VM u postojeće AzureDevTestLab iz VSTS](http://www.visualstudiogeeks.com/blog/DevOps/Deploy-New-VM-To-Existing-AzureDevTestLab-From-VSTS) 
- [Pomoću upravljanja izdanje VSTS za neprekinuti implementacijama AzureDevTestLabs](http://www.visualstudiogeeks.com/blog/DevOps/Use-VSTS-ReleaseManagement-to-Deploy-and-Test-in-AzureDevTestLabs) 
 
Za ostale toolchains CI/CD sve prethodno spomenuta Scenariji koji mogu postići putem proširenja za zadatke VSTS mogu se na sličan način postići kroz implementacija [predložaka Voditelj resursa Azure](https://github.com/Azure/azure-devtestlab/tree/master/ARMTemplates) pomoću [cmdleta Azure PowerShell](../resource-group-template-deploy.md) i [.NET SDK-ovi](https://www.nuget.org/packages/Microsoft.Azure.Management.DevTestLabs/). [REST API -ji za DevTest Labs](http://aka.ms/dtlrestapis) možete koristiti i za integraciju s vašeg toolchain.  

### <a name="why-cant-i-see-certain-vms-in-the-azure-virtual-machines-blade-that-i-see-within-azure-devtest-labs"></a>Zašto ne mogu vidjeti neke VMs u virtualnim računalima sustava Azure plohu koji se prikazuje unutar Azure DevTest Labs?
Na VM stvaranja u Azure DevTest Labs dozvola dobiva pristup tom VM. Prikazuje vam se za prikaz u web-mjesto plohu labs i u okvir za plohu **virtualnih računala** . Korisnici u ulozi DevTest Labs mogu vidjeti sve virtualnim strojevima stvorene u na Laboratorija kroz plohu **sve virtualnim računalima** sustava Laboratorija. Međutim, korisnici u ulozi DevTest Labs ne automatski odobravaju čitanje VM resursima koje drugi stvorili. Stoga te VMs se ne prikazuju u plohu **virtualnih računala** . 

### <a name="what-is-the-difference-between-custom-images-and-formulas"></a>Koja je razlika između prilagođene slike i formula? 
Prilagođenu sliku je VHD (virtualne tvrdi disk), dok je formula u sliku koju možete konfigurirati dodatne postavke koje možete spremiti i ponoviti. Prilagođenu sliku možda preporučuje ako želite brzo stvoriti nekoliko okruženja iste osnovne, immutable sliku. Formula možda bolje ako želite ponoviti konfiguraciji vašeg VM s najnovijim bitova, virtualne/podmreži mreže ili određene veličine. Za detaljnije objašnjenje, potražite u članku, [Comparing prilagođene slike i formula u DevTest Labs](devtest-lab-comparing-vm-base-image-types.md). 
 
### <a name="how-do-i-create-multiple-vms-from-the-same-template-at-once"></a>Kako stvoriti više VMs od istog predloška odjednom? 
Možete koristiti [VSTS zadaci proširenje](https://marketplace.visualstudio.com/items?itemName=ms-azuredevtestlabs.tasks) ili [Generiranje predložak Voditelj resursa Azure](devtest-lab-add-vm-with-artifacts.md#save-arm-template) prilikom stvaranja VM i [uvođenje predloška Azure Voditelj resursa iz komponente Windows PowerShell](../resource-group-template-deploy.md). 
 
### <a name="how-do-i-move-my-existing-azure-vms-into-my-azure-devtest-labs-lab"></a>Kretanja Moje postojeće Azure VMs u moj Laboratorija Azure DevTest Labs 
Ne možemo dizajnirate rješenje prijeđite izravno VMs Azure DevTest Labs, ali trenutno možete kopirati vaše postojeće VMs Azure DevTest Labs na sljedeći način: 

1. Kopiranje datoteka VHD vaše postojeće VM pomoću [skripte komponente Windows PowerShell](https://github.com/Azure/azure-devtestlab/blob/master/Scripts/CopyVHDFromVMToLab.ps1) 
1. [Stvaranje prilagođenih slike](devtest-lab-create-template.md) unutar vaše Laboratorija Azure DevTest Labs. 
1. Stvaranje na VM u Laboratorija iz prilagođenu sliku 
 
### <a name="can-i-attach-multiple-disks-to-my-vms"></a>Mogu li priložiti više diskova Moje VMs? 
Prilaganje više diskova VMs podržana.  
 
### <a name="how-do-i-automate-the-process-of-uploading-vhd-files-to-create-custom-images"></a>Kako automatizirati postupak prijenos datoteka VHD da biste stvorili prilagođeni slike? 
Postoje dvije mogućnosti:

- Da biste kopirali ili prijenos datoteka VHD na račun za pohranu koji je povezan s na Laboratorija mogu se [Azure AzCopy](../storage/storage-use-azcopy.md#blob-upload) .
- [Microsoft Azure prostora za pohranu Explorer](../vs-azure-tools-storage-manage-with-storage-explorer.md) je samostalnu aplikaciju koja se izvršava na Windows, OSX i Linux.   
 
Da biste pronašli račun za pohranu odredište koji je povezan s vašeg Laboratorija, slijedite ove korake:

1. Prijavite se na [portal za Azure](http://go.microsoft.com/fwlink/p/?LinkID=525040). 
1. U lijevom oknu odaberite **Grupe resursa** . 
1. Pronađite i odaberite grupu resursa povezan s vašeg Laboratorija. 
1. Plohu **Pregled** odaberite neku računa za pohranu. 
1. Odaberite **blob-ova**.
1. Potražite prijenosima na popisu. Ako nema, vratite se u koraku 4 i pokušajte s drugim računom za pohranu.
1. Pomoću **URL-a** kao odredištu u AzCopy naredbu.


### <a name="how-can-i-automate-the-process-of-deleting-all-the-vms-in-my-lab"></a>Kako automatizirati postupak brisanja VMs u moj Laboratorija?

Osim brisanje VMs iz vaše Laboratorija na portalu za Azure, možete izbrisati sve na VMs u vašem Laboratorija pomoću skriptu PowerShell. U sljedećem primjeru, jednostavno izmijeniti vrijednosti parametara u odjeljku **vrijednosti da biste promijenili** komentar. Možete dohvatiti na `subscriptionId`, `labResourceGroup`, a `labName` vrijednosti iz plohu Laboratorija na portalu za Azure. 


    # Delete all the VMs in a lab
    
    # Values to change
    $subscriptionId = "<Enter Azure subscription ID here>"
    $labResourceGroup = "<Enter lab's resource group here>"
    $labName = "<Enter lab name here>"

    # Login to your Azure account
    Login-AzureRmAccount
    
    # Select the Azure subscription that contains the lab. This step is optional
    # if you have only one subscription.
    Select-AzureRmSubscription -SubscriptionId $subscriptionId
    
    # Get the lab that contains the VMs to delete.
    $lab = Get-AzureRmResource -ResourceId ('subscriptions/' + $subscriptionId + '/resourceGroups/' + $labResourceGroup + '/providers/Microsoft.DevTestLab/labs/' + $labName)
    
    # Get the VMs from that lab.
    $labVMs = Get-AzureRmResource | Where-Object { 
              $_.ResourceType -eq 'microsoft.devtestlab/labs/virtualmachines' -and
              $_.ResourceName -like "$($lab.ResourceName)/*"}
    
    # Delete the VMs.
    foreach($labVM in $labVMs)
    {
        Remove-AzureRmResource -ResourceId $labVM.ResourceId -Force
    }




### <a name="what-are-artifacts"></a>Što su artefakte? 
Artefakte su prilagodljivi elemente koji se mogu koristiti za implementaciju sustava najnovije bitova ili Alati za razvojni premjestite na VM. Su priložene vaše VM tijekom stvaranja samo nekoliko klikova jednostavne, a kada je na VM dodjeli, u artefakte implementirati i konfigurirati svoje VM. Postoje razni postojećih artefakte u našem [javno spremište Github](https://github.com/Azure/azure-devtestlab/tree/master/Artifacts), ali to možete jednostavno [Stvaranje vlastite artefakte](devtest-lab-artifact-author.md). 

### <a name="how-do-i-create-a-lab-from-an-azure-resource-manager-template"></a>Kako stvoriti na Laboratorija iz predloška Azure Voditelj resursa? 
Ne možemo ste naveli [Github spremište Laboratorija Voditelj resursa Azure predloške](https://github.com/Azure/azure-devtestlab/tree/master/ARMTemplates) koje možete implementirati kao- i izmijeniti da biste stvorili prilagođene predloške za vaše labs. Svaki od tih predložaka ima vezu koju možete kliknuti da bi se implementacija Laboratorija kao-je u odjeljku vlastitu Azure pretplatu ili možete prilagoditi predložak i [Implementacija pomoću programa PowerShell ili Azure EŽA](../resource-group-template-deploy.md).
 
### <a name="why-are-my-vms-created-in-different-resource-groups-with-arbitrary-names-can-i-rename-or-modify-these-resource-groups"></a>Zašto se moj VMs stvaraju u grupama različite resursa proizvoljne nazivima? Možete preimenovati i izmjenu te grupe resursa? 
Grupa resursa se stvaraju na taj način da bi Azure DevTest Labs Upravljanje korisničkim dozvolama i pristup virtualnim računalima. Dok je u VM možete premjestiti u drugu grupu resursa uz željeno ime, time se ne preporučuje. Radimo na poboljšanju ovog problema omogućuju veću fleksibilnost.   
 
### <a name="how-many-labs-can-i-create-under-the-same-subscription"></a>Koliko labs možete stvoriti u odjeljku iste pretplate? 
Nema ograničenja određeni broj labs koje je moguće stvoriti po pretplati. Međutim, resursa koji se koriste ograničena su po pretplati. Saznajte o [ograničenja i kvotama zadana Azure pretplate](../azure-subscription-service-limits.md) te [kako povećati ta ograničenja](https://azure.microsoft.com/blog/azure-limits-quotas-increase-requests). 
 
### <a name="how-many-vms-can-i-create-per-lab"></a>Koliko VMs možete stvoriti po Laboratorija? 
Nema ograničenja određeni broj VMs koje je moguće stvoriti po Laboratorija. Međutim, trenutno u Laboratorija podržava samo o 40 VMs koji se izvodi u isto vrijeme u standardnom prostor za pohranu i 25 VMs istovremeno radi pohrane premium u. Ne možemo trenutno radite povećanje ta ograničenja. 

### <a name="how-do-i-share-a-direct-link-to-my-lab"></a>Kako se zajednički koriste izravnu vezu na moj Laboratorija?

Da biste zajednički koristili izravnu vezu Laboratorija korisnicima možete izvesti u nastavku.

1. Pronađite Laboratorija na portalu za Azure.
2. Kopirajte URL Laboratorija iz preglednika te je zajednički koristite s korisnicima Laboratorija. 

>[AZURE.NOTE] Ako korisnici Laboratorija je vanjskih korisnika s [MSA račun](#what-is-a-microsoft-account) , a ne pripadaju Active directory vaše tvrtke, mogu primiti obavijest o pogrešci kada se krećete navedene veze na. Ako se pojavljuje pogreška, uputite ih kliknite svoje ime u gornjem desnom kutu Azure portal i odaberite imenik tamo gdje postoje na Laboratorija iz **imenika** dijelu izbornika.

### <a name="what-is-a-microsoft-account"></a>Što je Microsoftov račun?

Microsoftov je račun koristiti za gotovo sve što činite s uređaja Microsoft i servisima. Je adresa e-pošte i lozinku koje koristite za prijavu u Skype, Outlook.com, OneDrive, Windows Phone i Xbox LIVE – i to znači da se datoteka, fotografije, kontaktima i postavke možete vas prate na bilo kojem uređaju. 

>[AZURE.NOTE] Microsoftov račun koristiti za zove "Windows Live ID".
 
### <a name="my-artifact-failed-during-vm-creation-how-do-i-troubleshoot-it"></a>Moje artefakt nije uspjela tijekom stvaranja VM. Kako s njim? 
Pogledajte članak na blogu [kako otkloniti poteškoće neuspješnih artefakte u AzureDevTestLabs](http://www.visualstudiogeeks.com/blog/DevOps/How-to-troubleshoot-failing-artifacts-in-AzureDevTestLabs) - napisao nešto naš MVP - da biste saznali kako nabaviti zapisnika vezane uz vaše nije uspjelo artefakt. 
 
### <a name="why-isnt-my-existing-virtual-network-saving-properly"></a>Zašto nije Moje postojeće virtualne mreže spremanja pravilno?  
Jedan moguće je i sadrži li naziv virtualne mreže razdoblja. Ako je tako, pokušajte ukloniti razdoblja ili zamjenjuje crtice, a zatim pokušajte ponovno spremiti virtualne mreže.
