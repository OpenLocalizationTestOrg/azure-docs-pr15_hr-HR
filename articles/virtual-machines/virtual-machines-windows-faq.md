<properties
    pageTitle="Najčešća pitanja o Aplikaciji Windows VMs | Microsoft Azure"
    description="Navedeni odgovori na neka najčešća pitanja o Windows virtualnim strojevima stvorene pomoću modela Voditelj resursa."
    services="virtual-machines-windows"
    documentationCenter=""
    authors="cynthn"
    manager="timlt"
    editor=""
    tags="azure-resource-management"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/16/2016"
    ms.author="cynthn"/>

# <a name="frequently-asked-question-about-windows-virtual-machines"></a>Najčešća pitanja o Windows virtualnim strojevima 


U ovom se članku adrese neka od čestih pitanja o Windows virtualnim strojevima stvorene u Azure pomoću modela implementacije Voditelj resursa. Linux verziju ove teme potražite u članku [Najčešća pitanja o Linux virtualnim strojevima](virtual-machines-linux-faq.md)

## <a name="what-can-i-run-on-an-azure-vm"></a>Što možete pokrenuti na VM programa Azure?

Sve pretplatnike možete pokrenuti poslužiteljski softver na Azure virtualnog računala. Informacije o pravilniku podrške za pokretanje Microsoft poslužiteljski softver u Azure potražite u članku [Microsoft poslužiteljski softver podržava virtualnim računalima sustava za Azure](https://support.microsoft.com/kb/2721672)

Određene verzije sustava Windows 7 i Windows 8.1 dostupni su za pretplatnike na MSDN Azure prednosti i MSDN razvojni i testiranje Pay-As-You-Go pretplatnike za razvoj i testiranje zadatke. Detalje o, uključujući upute i ograničenja, potražite u članku [slike Windows klijent za pretplatnike na MSDN](http://azure.microsoft.com/blog/2014/05/29/windows-client-images-on-azure/). 


## <a name="how-much-storage-can-i-use-with-a-virtual-machine"></a>Koliko prostora za pohranu možete koristiti s virtualnog računala?

Svaki podatkovni disk može biti do 1 TB. Broj diskova podataka možete koristiti ovisi o veličini virtualnog računala. Detalje potražite u članku [veličine za virtualnih računala](virtual-machines-windows-sizes.md).

Račun za Azure prostora za pohranu omogućuje spremanje disk operacijski sustav i sve podatke diskova. Svaki disk je .vhd datoteke spremljene kao blob stranice. Cijene detalje potražite u članku [Pojedinosti cijene za pohranu](https://azure.microsoft.com/pricing/details/storage/).


## <a name="how-can-i-access-my-virtual-machine"></a>Kako pristupiti Moje virtualnog računala?

Uspostavite udaljenu pomoću veze udaljene radne površine (RDP) za Windows VM. Upute potražite u članku [Povezivanje i prijavite se na Azure virtualnog računala sa sustavom Windows](virtual-machines-windows-connect-logon.md). Maksimalno dvije istovremene veze podržane su, osim ako je poslužitelj konfiguriran kao glavno računalo sesiju udaljene radne površine.  


Ako imate problema s udaljene radne površine, potražite u članku [Otklanjanje poteškoća s veze udaljene radne površine da biste je utemeljen na sustavu Windows Azure virtualnog računala](virtual-machines-windows-troubleshoot-rdp-connection.md). 

Ako ste upoznati s Hyper-V, možda gledate za alat koja je slična VMConnect. Azure ne nude sličnog alata jer ne podržava konzole za pristup virtualnog računala.

## <a name="can-i-use-the-temporary-disk-the-d-drive-by-default-to-store-data"></a>Mogu li koristiti privremene disk (pogon D: po zadanom) da biste pohranili podatke?

Nemojte koristiti privremene disk radi pohrane podataka. Moguće je samo privremeni prostor za pohranu, tako da bi rizikom gubitka podataka koji nije moguć. Gubitka podataka ubuduće se može dogoditi kada premješta virtualnog računala na drugo glavno računalo. Promjena veličine virtualnog računala, su ažuriranja glavno računalo ili kvara hardvera na računalu koje hostira neki od razloga zašto možda premještanje virtualnog računala.

Ako imate aplikaciju koju je potrebno koristiti slovo D:, možete ponovno dodijeliti slova pogona tako da koristi privremene disk brojem koji nije D:. Upute potražite u članku [Promjena slovo privremene disketa za Windows](virtual-machines-windows-classic-change-drive-letter.md).

## <a name="how-can-i-change-the-drive-letter-of-the-temporary-disk"></a>Kako promijeniti slovo privremene diska?

Slovo možete promijeniti tako da premještanje datoteka stranice i dodjelom slova pogona, ali morate obavezno učinite korake određenim redoslijedom. Upute potražite u članku [Promjena slovo privremene disketa za Windows](virtual-machines-windows-classic-change-drive-letter.md).

## <a name="can-i-add-an-existing-vm-to-an-availability-set"></a>Kako dodati postojeći VM u skupu dostupnost?

ne. Ako želite da se vaša VM kao dio skupa dostupnost, morate stvoriti VM unutar skupa. Trenutno ne postoji način da biste dodali na VM raspoloživost postavljanje kada je stvorena.

## <a name="can-i-upload-a-virtual-machine-to-azure"></a>Mogu li prenijeti virtualnog računala Azure?

Da. Upute potražite u članku [prijenos na sliku Windows VM Azure](virtual-machines-windows-upload-image.md)

## <a name="can-i-resize-the-os-disk"></a>Mogu li promijeniti veličinu na disku OS?

Da. Upute potražite u članku [Da biste proširili pogon OS virtualnog računala u grupi resursa Azure](virtual-machines-windows-expand-os-disk.md).

## <a name="can-i-copy-or-clone-an-existing-azure-vm"></a>Kako kopirati-Kloniraj postojeće VM za Azure?

Da. Upute potražite u članku [kako stvoriti kopiju virtualnog računala za Windows u modelu implementacije Voditelj resursa](virtual-machines-windows-vhd-copy.md).

## <a name="why-am-i-not-seeing-canada-central-and-canada-east-regions-through-azure-resource-manager"></a>Zašto se ne prikazuju Kanada središnje i Kanada Istok područja putem upravitelja za Azure resursa?

Dva nova područja Kanada središnje i Kanada Istok nisu automatski registrirani za stvaranje virtualnog računala za postojeće Azure pretplate. U ovom Registracija obavlja se automatski kada je virtualnog računala implementiran putem portala za Azure do druge regije pomoću upravitelja resursa Azure. Kada je virtualnog računala implementiran na druga Azure regija, nova područja mora biti dostupno za sljedeće virtualnim računalima.

## <a name="does-azure-support-linux-vms"></a>Podržava li Azure Linux VMs?

Da. Da biste brzo stvorili Linux VM da biste isprobali, potražite u članku [Stvaranje Linux VM na Azure pomoću portala za](virtual-machines-linux-quick-create-portal.md).

## <a name="can-i-add-a-nic-to-my-vm-after-its-created"></a>Mogu li dodati na NIC Moje VM nakon stvaranja?

ne. Dodavanje u NIC mogu izvršiti samo na vrijeme stvaranja.

## <a name="are-there-any-computer-name-requirements"></a>Postoje li sve preduvjete naziv računala?

Da. Naziv računala može biti najviše 15 znakova. Dodatne informacije o oko imenovanja resursa potražite u članku [smjernice za imenovanja infrastrukture](virtual-machines-windows-infrastructure-naming-guidelines.md) .

## <a name="what-are-the-username-requirements-when-creating-a-vm"></a>Preduvjeti za korisničko ime pri stvaranju na VM?

Korisnička imena može biti najviše 20 znakova i ne može završavati točkom ("."). 

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
        <td style="text-align:center">sigurnosno kopiranje </td><td style="text-align:center"> Konzola </td><td style="text-align:center"> Neven </td><td style="text-align:center"> gost</td>
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

Lozinke mora biti 8 123 znakova i zadovoljavaju 3 iz sljedećih 4 složenosti:

- Imati donjem znakova
- Imati gornjem znakova
- Imate znamenka
- Sadrže posebne znakove (Regex odgovaraju [\W_])

Nije dopušteno lozinke za sljedeće:

<table>
    <tr>
        <td style="text-align:center">abc@123</td><td style="text-align:center">P@$$w0rd</td><td style="text-align:center">P@ssw0rd</td><td style="text-align:center">P@ssword123</td><td style="text-align:center">Word $ stranicu</td>
    </tr>
    <tr>
        <td style="text-align:center">pass@word1</td><td style="text-align:center">Lozinka!</td><td style="text-align:center">Lozinke1</td><td style="text-align:center">Password22</td><td style="text-align:center">ILOVEYOU!</td>
    </tr>
</table>
