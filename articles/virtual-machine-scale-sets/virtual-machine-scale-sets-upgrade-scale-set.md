<properties
    pageTitle="Implementacija aplikacije na skupovima skaliranje virtualnog računala | Microsoft Azure"
    description="Implementacija aplikacije na virtualnog računala skaliranje skupovima"
    services="virtual-machine-scale-sets"
    documentationCenter=""
    authors="gbowerman"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machine-scale-sets"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/13/2016"
    ms.author="guybo"/>


# <a name="upgrade-a-virtual-machine-scale-set"></a>Nadogradnja skupu skaliranje virtualnog računala

U ovom se članku opisuje kako ažuriranje OS možete distribucija programa Azure virtualnog računala skaliranje postaviti bez sve isključiti. U ovom kontekstu OS ažuriranje obuhvaća promjeni verzija ili SKU OS-a ili promjeni URI prilagođenu sliku. Ažuriranje bez nedostupnost znači da je ažuriranje virtualnim strojevima jedan po jedan ili u grupe (primjerice jednu domenu kvara istovremeno) umjesto odjednom. Na taj način, možete zadržati izvodi se nadograđuju virtualnim računalima.

Da biste izbjegli dvosmislenosti, recimo razlikovanje tri vrste OS ažuriranje želite izvesti:

- Promjena verzije ili SKU platformu slike. Na primjer, promjena Ubuntu 14.04.2-LTS verziju s 14.04.201506100 14.04.201507060, ili promjena Ubuntu 15.10/najnoviji SKU 16.04.0-LTS/latest. Ovaj scenarij opisana u ovom članku.

- Promjena URI koja upućuje na novu verziju prilagođenu sliku u komponenti (**Svojstva** > **virtualMachineProfile** > **storageProfile** > **osDisk** > **Slika** > **URI-ja**). Ovaj scenarij opisana u ovom članku.

- Zakrpa OS iz unutar virtualnog računala (to Primjeri instalacije zakrpa za sigurnost i pokretanje servisa Windows Update). Ovaj scenarij je podržano, no ne obuhvaća u ovom članku.

Prve dvije mogućnosti preduvjeti podržani prekriveni ovog članka. Morat ćete stvoriti skalu novi postaviti za izvršavanje treću mogućnost.

Skupovi skaliranje virtualnog računala koje su uvedene kao dio sustava klaster [Tkanina servisa Azure](https://azure.microsoft.com/services/service-fabric/) ne možete pronaći u ovdje.

Osnovni redoslijed mijenja se OS verzija/SKU platformu slike ili URI prilagođenu sliku izgleda na sljedeći način:

1. Pronađite modela postavljanje skaliranje virtualnog računala.

2. Promjena verzije, SKU ili URI vrijednost u modelu.

3. Ažurirajte modela.

4. Učinite poziv *manualUpgrade* virtualnim strojevima u skupu mjerilo. Ovaj korak vrijedi samo ako *upgradePolicy* postavljena na **ručni** u vašem skupu mjerilo. Ako je postavljen na **Automatsko**, sve virtualnim strojevima su nadograditi odjednom, što uzrokuje isključiti.


Pomoću tih informacija pozadine na umu Pogledajmo kako može ažurirati verziju skalu postavljanje u ljusci PowerShell i pomoću REST API-JA. U ovim se primjerima prolaženja kutije platformu slike, ali u ovom se članku daje dovoljno informacija za prilagodbu ovaj postupak da biste prilagođenu sliku.

## <a name="powershell"></a>PowerShell##

U ovom se primjeru ažurira skalu Windows virtualnog računala koja je postavljena na novu verziju 4.0.20160229. Nakon ažuriranja model ne instancu virtualnog računala jedan za ažuriranje odjednom.

```powershell
$rgname = "myrg"
$vmssname = "myvmss"
$newversion = "4.0.20160229"
$instanceid = "1"

# get the VMSS model
$vmss = Get-AzureRmVmss -ResourceGroupName $rgname -VMScaleSetName $vmssname

# set the new version in the model data
$vmss.virtualMachineProfile.storageProfile.imageReference.version = $newversion

# update the virtual machine scale set model
Update-AzureRmVmss -ResourceGroupName $rgname -Name $vmssname -VirtualMachineScaleSet $vmss

# now start updating instances
Update-AzureRmVmssInstance -ResourceGroupName $rgname -VMScaleSetName $vmssname -InstanceId $instanceId
```

Ako ažurirate URI za prilagođenu sliku umjesto promjene slike verzije platforme, Zamijeni redak "Postavljanje novu verziju" otprilike ovako:

```powershell
# set the new version in the model data
$vmss.virtualMachineProfile.storageProfile.osDisk.image.uri= $newURI
```


## <a name="the-rest-api"></a>REST API-JA

Evo nekoliko primjera Python koje koriste Azure REST API-JA distribucija ažuriranu verziju OS. Oba koristiti biblioteku laganih [azurerm](https://pypi.python.org/pypi/azurerm) REST API-JA Azure omot funkcija DOHVATI na skup model mjerilo, a slijedi STAVI s ažuriranim modelom. Izgledaju i na prikaze instance virtualnog računala da biste odredili virtualnim strojevima po ažuriranje domene.

### <a name="vmssupgrade"></a>Vmssupgrade

 [Vmssupgrade](https://github.com/gbowerman/vmsstools) je Python skriptu koja se koristi za nadogradnju OS uvođenju izvodi skaliranje virtualnog računala postavili jednu domenu ažuriranje odjednom.

![Vmssupgrade skripta za odabir virtualnim strojevima ili ažuriranje domene](./media/virtual-machine-scale-sets-upgrade-scale-set/vmssupgrade-screenshot.png)

Ova skripta omogućuje vam da odaberete određenu virtualnih računala da biste ažurirali ili odredite ažuriranje domene. Podržava promjeni verzije platforme slike ili promjeni URI prilagođenu sliku.

### <a name="vmsseditor"></a>Vmsseditor

[Vmsseditor](https://github.com/gbowerman/vmssdashboard) je općenite namjene za skupove skaliranje virtualnog računala koje prikazuje status virtualnog računala kao u heatmap pri čemu je jedan redak predstavlja jednu ažuriranje domene. Između ostalog, možete ažurirati modela za skup skala s novom verzijom, SKU ili prilagođenu sliku URI i odaberite kvara domene za nadogradnju. Kada to učinite, sve virtualnim strojevima tu domenu ažuriranje nadogradit će se novi model. Osim toga, možete učiniti rolling nadogradnje ovisno o veličini obradu po izboru.  

Sljedeće snimka zaslona prikazuje model skalu za Ubuntu 14.04-2LTS verzije 14.04.201507060. Mnogo više mogućnosti su dodani u ovaj alat jer je snimljena snimka zaslona.

![Model Vmsseditor skalu za Ubuntu 14.04 2LTS](./media/virtual-machine-scale-sets-upgrade-scale-set/vmssEditor1.png)

Nakon što kliknete **nadogradnje** , a zatim **Dohvati detalje**, virtualnim strojevima u UD 0 pokrenite da biste ažurirali.

![Ažuriranje prikazuje Vmsseditor u tijeku](./media/virtual-machine-scale-sets-upgrade-scale-set/vmssEditor2.png)
