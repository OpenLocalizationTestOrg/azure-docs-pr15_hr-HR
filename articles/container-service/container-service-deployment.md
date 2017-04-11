<properties
   pageTitle="Implementacija programa servisa Azure spremnik klaster | Microsoft Azure"
   description="Implementacija programa klaster servis spremnik Azure pomoću portala za Azure, Azure EŽA ili PowerShell."
   services="container-service"
   documentationCenter=""
   authors="rgardler"
   manager="timlt"
   editor=""
   tags="acs, azure-container-service"
   keywords="Docker, spremnika, Micro-servisima, Mesos, Azure"/>

<tags
   ms.service="container-service"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="09/13/2016"
   ms.author="rogardle"/>

# <a name="deploy-an-azure-container-service-cluster"></a>Implementacija programa servisa Azure spremnik klaster

Azure Service spremnik nudi brzog uvođenja spremnika popularne Otvori izvor Klasteriranje i djelovanje rješenja. Pomoću servisa Azure spremnik možete implementirati Kontroler/OS i Docker Swarm klastere s predlošcima Voditelj resursa Azure ili Azure portal. Implementacija te klastere pomoću Azure virtualnog računala skaliranje skupova, a skupina iskoristite prednost Azure ponude mreže i prostora za pohranu. Da biste pristupili servisa Azure spremnik, potrebno vam je Azure pretplate. Ako ga nemate, pa možete se prijaviti za [besplatnu probnu verziju](http://azure.microsoft.com/pricing/free-trial/?WT.mc_id=AA4C1C935).

Ovaj dokument vodit će vas kroz implementacija programa klaster servis spremnik Azure pomoću [portala za Azure](#creating-a-service-using-the-azure-portal), [Azure sučelje naredbenog retka (EŽA)](#creating-a-service-using-the-azure-cli)i [Modul Azure PowerShell](#creating-a-service-using-powershell).  

## <a name="create-a-service-by-using-the-azure-portal"></a>Stvaranje servisa pomoću portala za Azure

Prijavite se na portal za Azure, odaberite **Novo**i potražite trgovine Windows Azure servisa **Azure kontejner**.

![Stvaranje implementacije 1](media/acs-portal1.png)  <br />

Odaberite **Servis spremnik Azure**pa kliknite **Stvori**.

![Stvaranje implementacije 2](media/acs-portal2.png)  <br />

Unesite sljedeće podatke:

- **Korisničko ime**: to je korisničko ime koje će se koristiti za račun na svakom virtualnim strojevima i skaliranje virtualnog računala postavlja u skupini servisa Azure kontejner.
- **Pretplate**: Odaberite Azure pretplatu.
- **Grupa resursa**: Odaberite postojeću grupu resursa ili stvorite novi.
- **Lokacija**: Odaberite Azure regiju servisa Azure spremnik implementacije.
- **Javni ključ SSH**: dodavanje javni ključ koji će se koristiti za provjeru autentičnosti protiv servisa Azure spremnik virtualnih računala. Vrlo je važno da taj ključ sadrži bez prijeloma retka, a uključuje "ssh-rsa" prefiks i 'username@domain' postfix. Ga izgleda otprilike ovako: **ssh-rsa AAAAB3Nz... <> …... UcyupgH azureuser@linuxvm **. Upute o stvaranju tipke sigurne ljuske (SSH), pročitajte članke [Linux]( https://azure.microsoft.com/documentation/articles/virtual-machines-linux-ssh-from-linux/) i [Windows]( https://azure.microsoft.com/documentation/articles/virtual-machines-linux-ssh-from-windows/) .

Kada budete spremni za nastavak, kliknite **u redu** .

![Stvaranje implementacije 3](media/acs-portal3.png)  <br />

Odaberite vrstu djelovanje. Su sljedeće mogućnosti:

- **Kontroler/OS**: uvodi klaster Kontroler/OS.
- **Swarm**: uvodi klaster Docker Swarm.

Kada budete spremni za nastavak, kliknite **u redu** .

![Stvaranje implementacije 4](media/acs-portal4.png)  <br />

Unesite sljedeće podatke:

- **Brojanje matrica**: broj matrica u klasteru.
- **Agent za brojanje**: za Docker Swarm, dogodit će se početni broj agenata u skupu skaliranje agent. Za Kontroler/OS, to će se početni broj agenata u skupu privatnih mjerilo. Uz to, skup javno skaliranje stvaranja, koji sadrži unaprijed određenim broj agenata. Broj agenata u tom skupu javno skaliranje određuje po koliko matrica je stvorena u klaster – jedan javno agent za jednu matricu i dva javna agente za tri ili pet matrica.
- **Agent virtualnog računala veličina**: veličina virtualnim strojevima agent.
- **DNS prefiks**: svijeta jedinstveni naziv koji će se koristiti za prefiks glavne dijelove na potpuno kvalificiranih naziva domena za servis.

Kada budete spremni za nastavak, kliknite **u redu** .

![Stvaranje implementacije 5](media/acs-portal5.png)  <br />

Nakon dovršetka usluge provjere valjanosti, kliknite **u redu** .

![Stvaranje implementacije 6](media/acs-portal6.png)  <br />

Kliknite **Stvori** da biste pokrenuli postupak implementacije.

![Stvaranje implementacije 7](media/acs-portal7.png)  <br />

Ako ste odabrali da biste prikvačili implementacije Azure portalu, možete vidjeti status implementacije.

![Stvaranje implementacije 8](media/acs-portal8.png)  <br />

Uvođenje dovrši, klaster servisa Azure spremnik spremna je za korištenje.

## <a name="create-a-service-by-using-the-azure-cli"></a>Stvaranje servisa pomoću EŽA Azure

Da biste stvorili instanca servisa Azure spremnik pomoću naredbenog retka, morate Azure pretplate. Ako ga nemate, pa možete se prijaviti za [besplatnu probnu verziju](http://azure.microsoft.com/pricing/free-trial/?WT.mc_id=AA4C1C935). Morate koristiti [instalacije](../xplat-cli-install.md) i [konfiguracije](../xplat-cli-connect.md) EŽA Azure.

Da biste implementirali Kontroler/OS ili Docker Swarm klaster, odaberite jednu od sljedećih predložaka iz GitHub. Imajte na umu da i predloške tih isti, osim zadani odabir orchestrator.

* [Predložak Kontroler/OS](https://github.com/Azure/azure-quickstart-templates/tree/master/101-acs-dcos)
* [Swarm predloška](https://github.com/Azure/azure-quickstart-templates/tree/master/101-acs-swarm)

Nakon toga provjerite je li povezani da EŽA Azure Azure pretplate. To možete učiniti pomoću sljedeće naredbe:

```bash
azure account show
```
Ako je račun za Azure se vraća, koristite sljedeću naredbu da biste se prijavili u EŽA Azure.

```bash
azure login -u user@domain.com
```

Nakon toga konfigurirati Alati za Azure EŽA da biste koristili Azure Voditelj resursa.

```bash
azure config mode arm
```

Stvaranje grupe Azure resursa i usluge spremnik klaster uz sljedeću naredbu, gdje:

- **RESOURCE_GROUP** je naziv grupe resursa koje želite koristiti za taj servis.
- **Mjesto** je Azure područje u kojem grupa resursa i Implementacija servisa Azure spremnik će se stvoriti.
- **TEMPLATE_URI** je mjesto datoteke za implementaciju. Imajte na umu da to mora biti neobrađenog datoteka ne pokazivač GitHub korisničkog Sučelja. Da biste pronašli ovaj URL, odaberite datoteku azuredeploy.json u GitHub, a zatim kliknite gumb **Raw** .

> [AZURE.NOTE] Prilikom pokretanja naredbe ljuske će vas za implementaciju vrijednosti parametara.

```bash
azure group create -n RESOURCE_GROUP DEPLOYMENT_NAME -l LOCATION --template-uri TEMPLATE_URI
```

### <a name="provide-template-parameters"></a>Navedite parametri predložaka

Ova verzija naredbu zahtijeva interaktivno definiranje parametara. Ako želite navesti parametra, kao što je niz JSON oblikovani, to možete učiniti pomoću na `-p` prijelaz. Ako, na primjer:

 ```bash
azure group deployment create RESOURCE_GROUP DEPLOYMENT_NAME --template-uri TEMPLATE_URI -p '{ "param1": "value1" … }'
```

Osim toga, možete unijeti JSON oblikovani parametara datoteke pomoću na `-e` prebaciti:

```bash
azure group deployment create RESOURCE_GROUP DEPLOYMENT_NAME --template-uri TEMPLATE_URI -e PATH/FILE.JSON
```

Da biste vidjeli primjera parametara datoteke pod nazivom `azuredeploy.parameters.json`, potražite ga s predlošcima servisa Azure kontejner u GitHub.

## <a name="create-a-service-by-using-powershell"></a>Stvaranje servisa pomoću komponente PowerShell

Možete uvesti i programa servisa Azure spremnik klaster sa servisom PowerShell. Ovaj dokument se temelji na verziju 1.0 [Modul Azure PowerShell](https://azure.microsoft.com/blog/azps-1-0/).

Da biste implementirali Kontroler/OS ili Docker Swarm klaster, odaberite jednu od sljedećih predložaka. Imajte na umu da i predloške tih isti, osim zadani odabir orchestrator.

* [Predložak Kontroler/OS](https://github.com/Azure/azure-quickstart-templates/tree/master/101-acs-dcos)
* [Swarm predloška](https://github.com/Azure/azure-quickstart-templates/tree/master/101-acs-swarm)

Prije stvaranja klaster za Azure pretplatu, provjerite je li da sesiju ljuske PowerShell digitalno potpisanu za Azure. To možete učiniti s na `Get-AzureRMSubscription` naredba:

```powershell
Get-AzureRmSubscription
```

Ako vam je potrebna prijava u Azure, koristite na `Login-AzureRMAccount` naredba:

```powershell
Login-AzureRmAccount
```

Ako ste implementacija u novu grupu resursa, najprije morate stvoriti grupu resursa. Da biste stvorili novu grupu resursa, koristite na `New-AzureRmResourceGroup` naredbu, a zatim navedite resursa grupe naziv i odredišni regiju:

```powershell
New-AzureRmResourceGroup -Name GROUP_NAME -Location REGION
```

Kada stvorite grupu resursa, možete stvoriti svoj klaster pomoću sljedeće naredbe. URI željeni predložak će biti navedeni za na `-TemplateUri` parametar. Prilikom pokretanja naredbe ljuske PowerShell će vas za implementaciju vrijednosti parametara.

```powershell
New-AzureRmResourceGroupDeployment -Name DEPLOYMENT_NAME -ResourceGroupName RESOURCE_GROUP_NAME -TemplateUri TEMPLATE_URI
```

### <a name="provide-template-parameters"></a>Navedite parametri predložaka

Ako ste upoznati s PowerShell, znate da možete rotirati dostupnih parametara za na cmdlet tako da upišete znak minus (-), a zatim pritisnite tipku TAB. Ta je funkcija isti funkcionira i sa parametre koje sami definirate u predlošku. Čim upišite naziv predloška, cmdlet dohvaćanja predložak, raščlanjuje parametre i dinamički dodaje parametri predložaka naredbu. To olakšava vrlo da biste odredili vrijednosti parametara predložak. A ako zaboravite vrijednost parametra potrebna PowerShell traži vrijednost.

U nastavku je cijelom naredbom s parametrima uključena. Možete odrediti vlastite vrijednosti za nazive resursa.

```powershell
New-AzureRmResourceGroupDeployment -ResourceGroupName RESOURCE_GROUP_NAME-TemplateURI TEMPLATE_URI -adminuser value1 -adminpassword value2 ....
```

## <a name="next-steps"></a>Daljnji koraci

Sad kad ste funkcionira klaster te dokumente za povezivanje i upravljanje detalje potražite u članku:

- [Povezivanje programa servisa Azure spremnik klaster](container-service-connect.md)
- [Rad sa servisa Azure spremnik i Kontroler/OS](container-service-mesos-marathon-rest.md)
- [Rad sa servisa Azure spremnik i Docker Swarm](container-service-docker-swarm.md)
