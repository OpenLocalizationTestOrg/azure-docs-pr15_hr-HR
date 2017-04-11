<properties
    pageTitle="Snimite Linux VM da biste koristili kao predložak | Microsoft Azure"
    description="Saznajte kako snimiti i generalize slika utemeljenu na Linux Azure virtualni stroj (VM) stvorene pomoću model implementacije Azure Voditelj resursa."
    services="virtual-machines-linux"
    documentationCenter=""
    authors="iainfoulds"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machines-linux"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/25/2016"
    ms.author="iainfou"/>


# <a name="capture-a-linux-virtual-machine-running-on-azure"></a>Snimite virtualni stroj Linux sustavom Azure

Slijedite korake u ovom članku generalize i snimiti Azure Linux virtualne računalu (VM) u modelu implementacije Voditelj resursa. Kada generalize na VM, uklonite osobnog računa informacije i Priprema VM koja će se koristiti kao sliku. Zatim snimite sliku generalizirano virtualne na tvrdom disku (VHD) za os-a, VHDs za diskova priložene podataka i [resursima predložak](../azure-resource-manager/resource-group-overview.md) za nove VM implementacije. 

Da biste stvorili VMs pomoću slika, postavljanje mrežni resursi za svaku novu VM i implementirati s snimljene slike VHD pomoću predloška (notaciju JavaScript objekt ili JSON, datoteka). Na taj način možete replicirati VM konfiguraciji njegov trenutni softver, slično korištenje slika u Azure Marketplace.

>[AZURE.TIP]Ako želite stvoriti kopiju vaše postojeće VM Linux s stanje specijalizirane za sigurnosno kopiranje ili ispravljanje pogrešaka potražite u članku [Stvaranje kopiju sustavom Azure virtualni stroj Linux](virtual-machines-linux-copy-vm.md). I ako želite prenijeti Linux VHD iz programa VM lokalnog potražite u članku [prijenos i stvaranje Linux VM iz slike prilagođene disk](virtual-machines-linux-upload-vhd.md).  

## <a name="before-you-begin"></a>Prije početka

Provjerite zadovoljavate sljedeće preduvjete:

* **Azure VM stvorene u model implementacije Voditelj resursa** – ako niste stvorili Linux VM, možete koristiti [portal](virtual-machines-linux-quick-create-portal.md), [Azure EŽA](virtual-machines-linux-quick-create-cli.md)i [resursima predložaka](virtual-machines-linux-cli-deploy-templates.md). 

    Konfiguriranje na VM prema potrebi. Ako, na primjer, [Dodavanje diskova podataka](virtual-machines-linux-add-disk.md), primijenite ažuriranja i instalirajte aplikacije. 
* **Azure EŽA** - Instaliraj [Azure EŽA](../xplat-cli-install.md) na lokalnom računalu.

## <a name="step-1-remove-the-azure-linux-agent"></a>Korak 1: Uklanjanje agent Azure Linux

Prvo pokretanje naredbe **waagent** s parametrom **deprovision** na Linux VM. Ta se naredba briše datoteke i podatke u VM Provjerite jeste li spremni za generalizing. Detalje potražite u članku [Vodič za korisnički Azure Linux Agent](virtual-machines-linux-agent-user-guide.md).

1. Povežite se s vašeg VM Linux klijenti SSH.

2. U prozoru SSH upišite sljedeću naredbu:

    ```
    sudo waagent -deprovision+user
    ```
    >[AZURE.NOTE] Samo pokrenite sljedeću naredbu na VM koje namjeravate snimiti kao sliku. Ne jamči da slike isključen sve povjerljivih podataka ili je prikladna radi ponovne distribucije.

3. Upišite **y** da biste nastavili. Možete dodati u **– prisilno** parametar da biste izbjegli taj korak za potvrdu.

4. Po završetku naredbu upišite **izlazak iz njega**. Ovaj korak zatvara SSH klijenta.

    
## <a name="step-2-capture-the-vm"></a>Korak 2: Snimiti na VM

Pomoću EŽA Azure generalize i snimite na VM. U sljedećim primjerima zamijenite primjer Nazivi parametara vlastitih vrijednosti. Nazivi parametara primjer obuhvaćaju **myResourceGroup**, **myVnet**i **myVM**.

5. S lokalnog računala, otvorite EŽA Azure i [prijavite se u pretplatu za Azure](../xplat-cli-connect.md). 

6. Provjerite radite u načinu rada Voditelj resursa.

    ```
    azure config mode arm
    ```

7. Isključivanje VM koji ste već pretplati su uklonjeni resursi pomoću sljedeće naredbe:

    ```
    azure vm deallocate -g MyResourceGroup -n myVM
    ```

8. Generalize VM pomoću sljedeće naredbe:

    ```
    azure vm generalize -g MyResourceGroup -n myVM
    ```

9. Odmah pokrenite naredbu **azure vm snimiti** koje se pohranjuju u VM. U sljedećem primjeru snimaju VHDs slika s nazivima počevši od **MyVHDNamePrefix**, a mogućnost **-t** navodi put do predloška **MyTemplate.json**. 

    ```
    azure vm capture MyResourceGroup MyResourceGroup MyVHDNamePrefix -t MyTemplate.json
    ```

    >[AZURE.IMPORTANT]Prema zadanim postavkama u isti račun za pohranu koji koriste izvornu VM stvaraju slikovne datoteke VHD. Koristite *isti račun za pohranu* za pohranu na VHDs za sve nove VMs stvarate iz slike. 

6. Da biste pronašli mjesto snimljenu sliku, otvorite predložak JSON u uređivaču teksta. U **storageProfile**pronađite **uri** **sliku** koja se nalazi u spremniku **sustava** . Na primjer, URI slika na disku OS slično je`https://xxxxxxxxxxxxxx.blob.core.windows.net/system/Microsoft.Compute/Images/vhds/MyVHDNamePrefix-osDisk.xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx.vhd`

## <a name="step-3-create-a-vm-from-the-captured-image"></a>Korak 3: Stvaranje na VM iz snimljenu sliku
Sada pomoću slika s predloškom da biste stvorili Linux VM. Sljedeći vam koraci prikazuju kako koristiti EŽA Azure i predložak datoteke JSON zabilježene da biste stvorili na VM u novi virtualne mreže.

### <a name="create-network-resources"></a>Stvaranje mrežni resursi

Da biste koristili predložak, najprije morate postaviti virtualne mreže i NIC za svoje nove VM. Preporučujemo Stvorite grupu resursa za resurse na mjestu na kojem je pohranjena VM sliku. Pokretanje naredbe slične nazive sljedeće, zamjenjujući za resurse i odgovarajuće Azure mjesto ("centralus" u te naredbe):

    azure group create MyResourceGroup1 -l "centralus"

    azure network vnet create MyResourceGroup1 myVnet -l "centralus"

    azure network vnet subnet create MyResourceGroup1 myVnet mySubnet

    azure network public-ip create MyResourceGroup1 myIP -l "centralus"

    azure network nic create MyResourceGroup1 myNIC -k mySubnet -m myVnet -p myIP -l "centralus"

### <a name="get-the-id-of-the-nic"></a>Preuzmite Id NIC-a

Da biste implementirali VM iz slike pomoću JSON koje ste spremili tijekom snimanja, morate Id u NIC. Nabavite ponovnim pokretanjem sljedeće naredbe:

    azure network nic show MyResourceGroup1 myNIC

**ID-a** u rezultatu je slična`/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/MyResourceGroup1/providers/Microsoft.Network/networkInterfaces/myNic`



### <a name="create-a-vm"></a>Stvaranje na VM
Odmah pokrenite sljedeću naredbu da biste stvorili vaše VM iz snimljenu sliku VM. Koristite parametar **-f** da biste odredili put do datoteke JSON predložak koji ste spremili.

    azure group deployment create MyResourceGroup1 MyDeployment -f MyTemplate.json

U rezultatu naredbe morat ćete navesti novi naziv VM, administrator korisničko ime i lozinku i Id NIC koju ste prethodno stvorili.

    info:    Executing command group deployment create
    info:    Supply values for the following parameters
    vmName: myNewVM
    adminUserName: myAdminuser
    adminPassword: ********
    networkInterfaceId: /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resource Groups/MyResourceGroup1/providers/Microsoft.Network/networkInterfaces/myNic

Sljedeći primjer prikazuje ono što vidite za uspješno uvođenje:

    + Pokretanje konfiguracije predloška i parametre
    + Stvaranje implementacije informacije: stvorili xxxxxxx uvođenje predloška
    + Čekanje implementaciju da biste dovršili podataka: DeploymentName: MyDeployment podataka: ResourceGroupName: MyResourceGroup1 podataka: ProvisioningState: uspješno podataka: vremenske oznake: xxxxxxx podataka: način rada: rastuće podataka: vrijednost za naziv vrste


    data:    ------------------  ------------  -------------------------------------

    podaci: myNewVM vmName niza


    podaci: vmSize Standard_D1 niza


    podaci: myAdminuser adminUserName niza


    podaci: adminPassword SecureString Nedefinirana


    podaci: networkInterfaceId niz /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/MyResourceGroup1/providers/Microsoft.Network/networkInterfaces/myNic informacije: implementaciju grupiranje Naredba Stvori u redu

### <a name="verify-the-deployment"></a>Provjera uvođenja

Sada SSH da biste virtualnog računala koju ste stvorili da biste provjerili implementacije i počnite koristiti novi VM. Da biste povezali putem SSH, pronađite IP adresu VM koji ste stvorili ponovnim pokretanjem sljedeće naredbe:

    azure network public-ip show MyResourceGroup1 myIP

Na javnu IP adresu nalazi se u rezultatu naredbe. Prema zadanim postavkama povezati VM Linux po SSH na priključak 22.

## <a name="create-additional-vms"></a>Stvaranje dodatnih VMs
Korištenje snimljenu sliku i predložak za implementaciju dodatne VMs pomoću koraka u prethodnom odjeljku. Druge mogućnosti da biste stvorili VMs iz slike obuhvaćaju pomoću predloška za brzo pokretanje ili izvođenje naredbe za **Stvaranje azure vm** .

### <a name="use-the-captured-template"></a>Korištenje snimljenu predloška

Da biste koristili snimljenu sliku i predložak, slijedite korake u nastavku (detaljna u prethodnom odjeljku):

* Provjerite je li VM sliku u isti račun za pohranu koji hostira vaša VM VHD.
* Kopiranje JSON datoteku predloška i Navedite jedinstveni naziv diska OS novi VM VHD (ili VHDs). Ako, na primjer, u **storageProfile**, u odjeljku **vhd**, u **uri**Navedite jedinstveni naziv **osDisk** VHD, slično kao`https://xxxxxxxxxxxxxx.blob.core.windows.net/vhds/MyNewVHDNamePrefix-osDisk.xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx.vhd`
* Stvaranje na NIC u istim ili različitim virtualne mreže.
* Korištenje JSON datoteku izmjene predloška stvorite implementacije u grupi resursa u kojoj postavljate virtualne mreže.

### <a name="use-a-quickstart-template"></a>Korištenje predloška za brzi početak rada

Ako želite da se s mrežom postaviti automatski prilikom stvaranja na VM iz slike, možete odrediti tih resursa u predlošku. Ako, na primjer, potražite [predložak 101 vm-na-korisnika – slike](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-from-user-image) iz GitHub. Ovaj predložak stvara na VM prilagođenu sliku i potrebne virtualne mreže, javnu IP adresa i NIC resursi. Vodič pomoću ovog predloška na portalu za Azure, potražite u članku [Stvaranje virtualnog računala s prilagođenu sliku pomoću predloška Voditelj resursa](http://codeisahighway.com/how-to-create-a-virtual-machine-from-a-custom-image-using-an-arm-template/).

### <a name="use-the-azure-vm-create-command"></a>Korištenje azure vm Naredba Stvori

Obično je najjednostavnije pomoću predloška resursima za stvaranje na VM iz slike. Međutim, možete stvoriti na VM _imperatively_ pomoću naredbe za **Stvaranje azure vm** **-Q** (**– Slika urn**) parametara. Ako koristite taj način, i prenesite parametar **-d** (**--os, na disku i vhd**) da biste odredili mjesto datoteke .vhd OS za nove VM. Datoteka mora biti u spremniku vhds računa za pohranu pohranjuju slikovne datoteke VHD. Naredba na VHD za nove VM automatski kopira u spremnik **vhds** .

Prije pokretanja **azure vm stvaranje** sa slikom, slijedite sljedeće korake:

1.  Stvorite grupu resursa ili prepoznavanje postojeću grupu resursa za implementaciju.

2.  Stvaranje javnog resursa IP adresa i NIC resursa za novog VM. Upute za stvaranje virtualne mreže, javnu IP adresa i NIC korištenjem na EŽA potražite u članku ranije u ovom članku. (**Stvaranje azure vm** možete stvoriti na NIC, ali morate proći dodatne parametre za virtualne mreže i podmreže.)


Zatim pokrenite naredbu koja prosljeđuje ji novu datoteku OS VHD i postojeću sliku. U ovom primjeru veličinu Standard_A1 VM se stvara u području Istočni SAD-a.

    azure vm create MyResourceGroup1 myNewVM eastus Linux -d "https://xxxxxxxxxxxxxx.blob.core.windows.net/vhds/MyNewVHDNamePrefix.vhd" -Q "https://xxxxxxxxxxxxxx.blob.core.windows.net/system/Microsoft.Compute/Images/vhds/MyVHDNamePrefix-osDisk.xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx.vhd" -z Standard_A1 -u myAdminname -p myPassword -f myNIC

Naredba dodatne mogućnosti, pokrenite `azure help vm create`.

## <a name="next-steps"></a>Daljnji koraci

Da biste upravljali vaše VMs s na EŽA, pročitajte članak zadaci u [uvođenje i upravljanje virtualnim strojevima pomoću predložaka Voditelj resursa Azure i EŽA Azure](virtual-machines-linux-cli-deploy-templates.md).
