<properties
 pageTitle="Dodavanje čvorove neprekidnog rada programa paketa HPC klaster | Microsoft Azure"
 description="Saznajte kako da biste proširili programa paketa HPC klaster u Azure osvježavati dodavanjem tempiranja uloga instance izvodi u oblaku"
 services="virtual-machines-windows"
 documentationCenter=""
 authors="dlepow"
 manager="timlt"
 editor=""
 tags="azure-service-management,hpc-pack"/>
<tags
ms.service="virtual-machines-windows"
 ms.devlang="na"
 ms.topic="article"
 ms.tgt_pltfrm="vm-multiple"
 ms.workload="big-compute"
 ms.date="10/14/2016"
 ms.author="danlep"/>

# <a name="add-on-demand-burst-nodes-to-an-hpc-pack-cluster-in-azure"></a>Dodavanje čvorove osvježavati "neprekidnog rada" programa paketa HPC klaster servisu Azure



Ako postavite [Paket Microsoft HPC](https://technet.microsoft.com/library/cc514029) klaster u Azure, možda ćete način za brzo skaliranje kapaciteta klaster prema gore ili prema dolje, bez održavanje skup konfiguriranog računalnim čvor VMs. U ovom se članku objašnjava da biste dodali na zahtjev "neprekidnog rada" čvorove (tempiranja uloga instance izvodi u oblaku) kao resurse računalnim na glavni čvor u Azure. 

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]

![Brzi niz čvorovi][burst]

Koraci u ovom članku pomoći brzo dodavanje Azure čvorovi u oblaku HPC paket glavni čvor VM za testiranje ili dokaz pojam implementacije. Više razine koraci su jednaki su koracima za "brzi niz za Azure" da biste dodali oblaka izračunati kapaciteta na lokalni paket HPC klaster. Praktični vodič, potražite u članku [Postavljanje hibridnoj izračunati klaster Microsoft HPC paketom](../cloud-services/cloud-services-setup-hybrid-hpcpack-cluster.md). Detaljne upute i zahtjevi za radni implementacijama potražite u članku [Brzi niz za Azure s Microsoft HPC Pack](https://technet.microsoft.com/library/gg481749.aspx).


## <a name="prerequisites"></a>Preduvjeti

* **Glavni čvor paket HPC uveden u programa Azure VM** – možete koristiti samostalni glavni čvor VM ili onu koja je dio veće klaster. Da biste stvorili samostalni glavni čvor, potražite u članku [uvođenja čvor glave paket HPC u VM programa Azure](virtual-machines-windows-hpcpack-cluster-headnode.md). Automatizirana mogućnosti implementacije klaster HPC paket, potražite u članku [Mogućnosti za stvaranje i upravljanje Windows HPC klaster u Azure s Microsoft HPC Pack](virtual-machines-windows-hpcpack-cluster-options.md).

    >[AZURE.TIP] Ako koristite [HPC paket IaaS implementacijsku skriptu](virtual-machines-windows-classic-hpcpack-cluster-powershell-script.md) da biste stvorili klaster u Azure, možete uključiti čvorove Azure neprekidnog rada u implementaciji sustava automatiziranog. Pogledajte primjere u tom članku.

* **Azure pretplate** – da biste dodali Azure čvorove, možete odabrati iste pretplate na koristi za implementaciju čvor glavni VM ili neku drugu pretplatu (ili pretplate).

* **Kvota jezgri** – možda ćete morati povećati kvote jezgri, osobito ako se odlučite za implementaciju nekoliko Azure čvorove s multicore veličine. Da biste povećali ograničenja, [otvorite zahtjev za mrežna Služba podrške](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/) bez troškova.

## <a name="step-1-create-a-cloud-service-and-a-storage-account-for-the-azure-nodes"></a>Korak 1: Stvaranje servis u oblaku i račun za pohranu za Azure čvorove

Pomoću portala za Azure klasični ili ekvivalentne alate za konfiguriranje u sljedećim resursima koji su potrebni za implementaciju sustava Azure čvorove:

* Novog servisa Azure oblaka
* Novi račun za Azure prostora za pohranu

>[AZURE.NOTE] Ne iskoristite postojeći oblaka servis za pretplatu. 

**Razmatranja**

* Konfiguriranje usluge zasebnom oblaka za svaki predložak Azure čvor koje namjeravate stvoriti. Međutim, možete koristiti isti račun za pohranu za većeg broja predložaka čvor.

* Preporučujemo da pronađete servisa u oblaku i račun za pohranu za implementaciju u istom Azure regiji.




## <a name="step-2-configure-an-azure-management-certificate"></a>Korak 2: Konfiguriranje certifikata Azure upravljanja

Da biste dodali Azure čvorove kao računalnim resursa, potrebno vam je upravljanje certifikat na glavni čvor i prijenos odgovarajući certifikat Azure pretplatu za implementaciju.

U ovom scenariju, možete odabrati **Zadano HPC Azure upravljanja certifikat** koji HPC paket instalira i automatski konfigurira čvor glavni. Taj certifikat je korisno za testiranje namjene i dokaz pojam implementacije. Da biste koristili ovog certifikata, prenesite na datoteku c:\Programske datoteke\Microsoft HPC paket 2012\Bin\hpccert.cer glavni čvorovi VM s pretplatom. Da biste prenijeli certifikat za [Azure klasični portal](https://manage.windowsazure.com), kliknite **Postavke** > **Upravljanja certifikata**.

Dodatne mogućnosti da biste konfigurirali upravljanje certifikatom potražite u članku [scenariji za konfiguriranje certifikata za Azure upravljanja za Azure brzi niz implementacije](http://technet.microsoft.com/library/gg481759.aspx).

## <a name="step-3-deploy-azure-nodes-to-the-cluster"></a>Korak 3: Implementacija Azure čvorove klaster



Korake za dodavanje i počnite Azure čvorove u tom slučaju obično su jednaki su koracima s na lokalni glavni čvor. Dodatne informacije potražite u sljedećim se odjeljcima u [koracima za implementaciju Azure čvorove Microsoft HPC paketom](https://technet.microsoft.com/library/gg481758.aspx):

* Stvaranje predloška Azure čvor

* Dodavanje Azure čvorovi klasteru HPC za Windows

* Pokretanje čvorovi (dodjele) na Azure

Nakon što dodate i pokretanje čvorove, jesu li spremni za korištenje da biste pokrenuli klaster poslove.

Ako naiđete na poteškoće pri implementaciji Azure čvorove, potražite u članku [Otklanjanje poteškoća s implementacijama sustava Azure čvorove Microsoft HPC paketom](http://technet.microsoft.com/library/jj159097.aspx).

## <a name="next-steps"></a>Daljnji koraci

* Da biste koristili računalnim ćete morati usko instancu veličina čvorove neprekidnog rada, potražite u članku pitanja o [H niza i računalnim ćete morati usko VMs odgovora niz](virtual-machines-windows-a8-a9-a10-a11-specs.md).

* Ako želite automatski Povećaj ili Smanji Azure računalno resursi prema radno opterećenje klaster, pročitajte članak [automatski Povećaj i Smanji Azure računalnim resursa u programa paketa HPC klaster](virtual-machines-windows-classic-hpcpack-cluster-node-autogrowshrink.md).

<!--Image references-->
[burst]: ./media/virtual-machines-windows-classic-hpcpack-cluster-node-burst/burst.png
