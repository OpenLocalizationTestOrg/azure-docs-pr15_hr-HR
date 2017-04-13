<properties
    pageTitle="Najčešća Pitanja o Linux VMs | Microsoft Azure"
    description="Navedeni odgovori na neka najčešća pitanja o Linux virtualnim strojevima stvorene pomoću modela Voditelj resursa."
    services="virtual-machines-linux"
    documentationCenter=""
    authors="cynthn"
    manager="timlt"
    editor=""
    tags="azure-resource-management"/>

<tags
    ms.service="virtual-machines-linux"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/16/2016"
    ms.author="cynthn"/>

# <a name="frequently-asked-question-about-linux-virtual-machines"></a>Najčešća pitanja o Linux virtualnim strojevima

U ovom se članku adrese neka od čestih pitanja o Linux virtualnim strojevima stvorene u Azure pomoću modela implementacije Voditelj resursa. Verziji sustava Windows u ovoj se temi potražite u članku [Najčešća pitanja o Windows virtualnim strojevima](virtual-machines-windows-faq.md)

## <a name="what-can-i-run-on-an-azure-vm"></a>Što možete pokrenuti na VM programa Azure?

Sve pretplatnike možete pokrenuti poslužiteljski softver na Azure virtualnog računala. Dodatne informacije potražite u članku [Linux na Azure-Endorsed distribucija](virtual-machines-linux-endorsed-distros.md)


## <a name="how-much-storage-can-i-use-with-a-virtual-machine"></a>Koliko prostora za pohranu možete koristiti s virtualnog računala?

Svaki podatkovni disk može biti do 1 TB. Broj diskova podataka možete koristiti ovisi o veličini virtualnog računala. Detalje potražite u članku [veličine za virtualnih računala](virtual-machines-linux-sizes.md).

Račun za Azure prostora za pohranu omogućuje spremanje disk operacijski sustav i sve podatke diskova. Svaki disk je .vhd datoteke spremljene kao blob stranice. Cijene detalje potražite u članku [Pojedinosti cijene za pohranu](https://azure.microsoft.com/pricing/details/storage/).


## <a name="how-can-i-access-my-virtual-machine"></a>Kako pristupiti Moje virtualnog računala?

Uspostavite udaljenu za prijavu na virtualnog računala pomoću sigurne ljuske (SSH). Pogledajte upute za povezivanje [iz sustava Windows](virtual-machines-linux-ssh-from-windows.md) ili [Linux i Mac](virtual-machines-linux-mac-create-ssh-keys.md). Prema zadanim postavkama SSH omogućuje najviše 10 Istodobni veze. Taj broj možete povećati uređivanjem konfiguracijskoj datoteci.


Ako imate poteškoća, pogledajte [veze za otklanjanje poteškoća s sigurne ljuske (SSH)](virtual-machines-linux-troubleshoot-ssh-connection.md).


## <a name="can-i-use-the-temporary-disk-devsdb1-to-store-data"></a>Mogu li koristiti privremeni disk (/ razvojni/sdb1) da biste pohranili podatke?

Nemojte koristiti privremene disk (/ razvojni/sdb1) radi pohrane podataka. Je li samo privremeno spremište. Rizika gubitka podataka koji nije moguć.


## <a name="can-i-copy-or-clone-an-existing-azure-vm"></a>Kako kopirati-Kloniraj postojeće VM za Azure?

Da. Upute potražite u članku [kako stvoriti kopiju Linux virtualnog računala u modelu implementacije Voditelj resursa](virtual-machines-linux-copy-vm.md).


## <a name="why-am-i-not-seeing-canada-central-and-canada-east-regions-through-azure-resource-manager"></a>Zašto se ne prikazuju Kanada središnje i Kanada Istok područja putem upravitelja za Azure resursa?

Dva nova područja Kanada središnje i Kanada Istok nisu automatski registrirani za stvaranje virtualnog računala za postojeće Azure pretplate. U ovom Registracija obavlja se automatski kada je virtualnog računala implementiran putem portala za Azure do druge regije pomoću upravitelja resursa Azure. Kada je virtualnog računala implementiran na druga Azure regija, nova područja mora biti dostupno za sljedeće virtualnim računalima.


## <a name="can-i-add-a-nic-to-my-vm-after-its-created"></a>Mogu li dodati na NIC Moje VM nakon stvaranja?

ne. Dodavanje u NIC mogu izvršiti samo na vrijeme stvaranja.


## <a name="are-there-any-computer-name-requirements"></a>Postoje li sve preduvjete naziv računala?

Da. Naziv računala može biti najviše 64 znaka. Dodatne informacije o oko imenovanja resursa potražite u članku [smjernice za imenovanja infrastrukture](virtual-machines-linux-infrastructure-naming-guidelines.md) .


## <a name="what-are-the-username-requirements-when-creating-a-vm"></a>Preduvjeti za korisničko ime pri stvaranju na VM?

Korisnička imena mora biti 1 do 64 znaka.

Nije dopušteno sljedeće korisnička imena:

<table>
    <tr>
        <td style="text-align:center">administrator </td><td style="text-align:center"> administrator </td><td style="text-align:center"> korisnik </td><td style="text-align:center"> Korisnik1</td>
    </tr>
    <tr>
        <td style="text-align:center">Test </td><td style="text-align:center"> korisnika2 </td><td style="text-align:center"> ispit1 </td><td style="text-align:center"> Korisnika3</td>
    </tr>
    <tr>
        <td style="text-align:center">admin1 </td><td style="text-align:center"> 1 </td><td style="text-align:center"> 123 </td><td style="text-align:center"> na</td>
    </tr>
    <tr>
        <td style="text-align:center">actuser  </td><td style="text-align:center"> Admin </td><td style="text-align:center"> admin2 </td><td style="text-align:center"> aspnet</td>
    </tr>
    <tr>
        <td style="text-align:center">sigurnosno kopiranje </td><td style="text-align:center"> Konzola </td><td style="text-align:center"> Neven </td><td style="text-align:center"> goste</td>
    </tr>
    <tr>
        <td style="text-align:center">Nevena </td><td style="text-align:center"> vlasnik </td><td style="text-align:center"> Korijenska </td><td style="text-align:center"> poslužitelj</td>
    </tr>
    <tr>
        <td style="text-align:center">SQL </td><td style="text-align:center"> podrška </td><td style="text-align:center"> support_388945a0 </td><td style="text-align:center"> sistemskim</td>
    </tr>
    <tr>
        <td style="text-align:center">ispit2 </td><td style="text-align:center"> test3 </td><td style="text-align:center"> Korisnik4 </td><td style="text-align:center"> user5</td>
    </tr>
</table>


## <a name="what-are-the-password-requirements-when-creating-a-vm"></a>Preduvjeti za lozinku prilikom pisanja na VM?

Lozinke mora biti 6 72 znakova i zadovoljavaju 3 iz sljedećih 4 složenosti:

- Imati donjem znakova
- Imati gornjem znakova
- Imate znamenka
- Sadrže posebne znakove (Regex odgovaraju [\W_])

Nije dopušteno lozinke za sljedeće:

<table>
    <tr>
        <td style="text-align:center">abc@123</td>
        <td style="text-align:center">P@$$w0rd</td>
        <td style="text-align:center">P@ssw0rd</td>
        <td style="text-align:center">P@ssword123</td>
        <td style="text-align:center">Word $ stranicu</td>
    </tr>
    <tr>
        <td style="text-align:center">pass@word1</td>
        <td style="text-align:center">Lozinka!</td>
        <td style="text-align:center">Lozinke1</td>
        <td style="text-align:center">Password22</td>
        <td style="text-align:center">ILOVEYOU!</td>
    </tr>
</table>
