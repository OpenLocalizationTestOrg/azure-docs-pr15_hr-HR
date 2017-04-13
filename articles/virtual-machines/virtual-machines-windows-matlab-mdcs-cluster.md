<properties
   pageTitle="MATLAB klaster na virtualnim strojevima | Microsoft Azure"
   description="Stvaranje klastere MATLAB raspodijeljenom Computing poslužitelja za pokretanje sustava računalnim ćete morati usko paralelno MATLAB radnih opterećenja pomoću virtualnim računalima sustava Microsoft Azure"
   services="virtual-machines-windows"
   documentationCenter=""
   authors="mscurrell"
   manager="timlt"
   editor=""/>

<tags
   ms.service="virtual-machines-windows"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="Windows"
   ms.workload="infrastructure-services"
   ms.date="05/09/2016"
   ms.author="markscu"/>

# <a name="create-matlab-distributed-computing-server-clusters-on-azure-vms"></a>Stvaranje MATLAB Distributed poslužitelja za računalstvo klastere na Azure VMs 

Da biste stvorili klastere MATLAB raspodijeljenom Computing poslužitelja za pokretanje sustava računalnim ćete morati usko paralelno MATLAB radnih opterećenja pomoću virtualnim računalima sustava Microsoft Azure. Instalacija softvera MATLAB Distributed poslužitelja za računalstvo na VM koristiti kao osnovni sliku i koristiti Azure brzi početak rada predloška ili skriptu Azure PowerShell (dostupno na [GitHub](https://github.com/Azure/azure-quickstart-templates/tree/master/matlab-cluster)) i upravljanje klaster. Nakon implementacije, povežite se s klaster za pokretanje sustava radnih opterećenja. 

## <a name="about-matlab-and-matlab-distributed-computing-server"></a>O MATLAB i MATLAB Distributed računalstvo poslužitelja 

Platforme [MATLAB](http://www.mathworks.com/products/matlab/) optimiziran je za rješavanje problema inženjerskih i znanstvenih. Korisnici MATLAB s veliki simulations i zadatke za obradu podataka mogu koristiti MathWorks paralelno računalstvo proizvoda da biste ubrzali njihove računalnim ćete morati usko radnih opterećenja prihvaćanjem prednost računalnim klastere i servise rešetke. [Paralelni alatnog okvira računalstvo](http://www.mathworks.com/products/parallel-computing/) omogućuje korisnicima MATLAB parallelize aplikacije i iskoristite prednosti više core procesora, prethode, i izračunati klastere. [MATLAB distribuira poslužitelja za računalstvo](http://www.mathworks.com/products/distriben/) MATLAB korisnicima omogućuje da koristi funkciju mnoga računala u računalnim klasterima. 


Pomoću Azure virtualnim strojevima možete stvoriti MATLAB Distributed poslužitelja za računalstvo klastere koje imaju iste mehanizme dostupna slanje paralelno rada kao lokalnog klastere, kao što su interaktivne zadacima, obrade, neovisno zadaci i komunikaciju zadaci. Pomoću Azure zajedno s platformu MATLAB sadrži brojne prednosti u usporedbi s dodjelom resursa i pomoću klasične lokalnog hardver: veličina raspon virtualnog računala, stvaranje klastere osvježavati tako plaćate samo za resurse računalnim koju koristite i mogućnost da biste testirali modela na razini.  

## <a name="prerequisites"></a>Preduvjeti

* **Klijentsko računalo** -, potreban vam je utemeljen na sustavu Windows klijentskog računala možete komunicirati s Azure i klaster MATLAB Distributed poslužitelja za računalstvo nakon implementacije. 

* **Azure PowerShell** – pogledajte [kako instalirati i konfigurirati Azure PowerShell](../powershell-install-configure.md) da biste ga instalirali na klijentskom računalu. 

* **Azure pretplate** – ako nemate pretplatu, možete stvoriti na [slobodno računa](https://azure.microsoft.com/free/) u samo nekoliko minuta. Za veće klastere, razmislite o pay-as-you-go pretplatu ili druge mogućnosti za kupnju. 

* **Kvota jezgri** – možda ćete morati povećati core kvote za implementaciju velikih klaster ili više MATLAB Distributed poslužitelja za računalstvo klaster. Da biste povećali ograničenja, [otvorite zahtjev za mrežna Služba podrške](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/) bez troškova. 

* **MATLAB, paralelnog računalstvo alatnog okvira i licenci MATLAB Distributed poslužitelju za računalstvo** – skripte pretpostavlja [Upravitelj licencama hostira MathWorks](http://www.mathworks.com/products/parallel-computing/mathworks-hosted-license-manager/) služi za sve licence.  

* **Softver MATLAB Distributed poslužitelja za računalstvo** - instalirat će se na VM koja će se koristiti kao osnovni VM sliku za klaster VMs. 


## <a name="high-level-steps"></a>Visoke razine korake

Da biste koristili Azure virtualnim računalima sustava klastere MATLAB distribuira poslužitelja za računalstvo, potrebni su sljedeće više razine. Detaljne upute su u dokumentaciji popratnim brzi početak rada predloška i skripte na [GitHub](https://github.com/Azure/azure-quickstart-templates/tree/master/matlab-cluster).

1. **Stvaranje osnovne VM slike**  
    * Preuzmite i instalirajte softver MATLAB Distributed poslužitelja za računalstvo na ovom VM. 

    >[AZURE.NOTE]Taj postupak može potrajati nekoliko sati, ali imate samo to učiniti kada je za svaku verziju MATLAB koristite.   
    
2. **Stvaranje jednog ili više klastere**  
    * Koristite navedenom skriptu PowerShell ili predložak za brzi početak rada da biste stvorili klaster s osnovnim VM slike.   
    * Upravljanje klastere pomoću navedenom skriptu PowerShell koja vam omogućuje da popis, zadržite pokazivač, nastavite i brisanje klastere. 
 
## <a name="cluster-configurations"></a>Konfiguracija klaster 

Trenutno klaster stvaranja skripte i predložak omogućuju stvaranje jedne topologije MATLAB Distributed poslužitelja za računalstvo. Ako želite, stvorite jedan ili više dodatnih klastere, s svaki klaster pojavljuju različit broj radnih VMs, pomoću različitih veličina VM i tako dalje. 

### <a name="matlab-client-and-cluster-in-azure"></a>MATLAB klijenta i klaster servisu Azure 

Čvor MATLAB klijent, raspored poslova MATLAB čvor i MATLAB Distributed poslužitelja za računalstvo "tempiranja" čvorove sve konfigurirane kao Azure VMs u virtualne mreže, kao što je prikazano na sljedećoj slici. 

![Topologija klaster](./media/virtual-machines-windows-matlab-mdcs-cluster/mdcs_cluster.png)

* Da biste koristili klaster, povezati putem udaljene radne površine čvor klijenta. Čvor klijent pokreće MATLAB klijenta. 

* Klijent čvor ima zajedničke datoteke koja se može pristupiti zaposlenici zaduženi za sve.

* Upravitelj licencama hostira MathWorks koristi se za provjeru licencu za sve programe MATLAB. 

* Prema zadanim postavkama, jedan MATLAB Distributed poslužitelja za računalstvo tempiranja po temeljni je stvoreno radnih VMs, ali možete odrediti bilo koji broj. 


## <a name="use-an-azure-based-cluster"></a>Korištenje programa Azure sustavom klaster 

Kao s drugim vrstama MATLAB Distributed poslužitelja za računalstvo klastere, morate koristiti upravitelj profila klaster u klijentu MATLAB (na klijentskom računalu VM) stvaranje profila klaster raspored poslova MATLAB.

![Upravitelj profila klaster](./media/virtual-machines-windows-matlab-mdcs-cluster/cluster_profile_manager.png)

## <a name="next-steps"></a>Daljnji koraci

* Detaljne upute za upravljanje klastere MATLAB Distributed poslužitelja za računalstvo u Azure i potražite u članku [GitHub](https://github.com/Azure/azure-quickstart-templates/tree/master/matlab-cluster) spremište koja sadrži predloške i skripti. 

* Idite na [web-mjesta MathWorks](http://www.mathworks.com/) za detaljne dokumentaciji MATLAB i MATLAB Distributed poslužitelja za računalstvo.
