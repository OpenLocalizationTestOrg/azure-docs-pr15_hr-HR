<properties
 pageTitle="Stvaranje programa paketa HPC glavni čvor u VM programa Azure | Microsoft Azure"
 description="Saznajte kako pomoću portala za Azure i resursima model za uvođenje da biste stvorili paket Microsoft HPC glavni čvor u VM programa Azure."
 services="virtual-machines-windows"
 documentationCenter=""
 authors="dlepow"
 manager="timlt"
 editor=""
 tags="azure-resource-manager,hpc-pack"/>
<tags
ms.service="virtual-machines-windows"
 ms.devlang="na"
 ms.topic="article"
 ms.tgt_pltfrm="vm-windows"
 ms.workload="big-compute"
 ms.date="08/17/2016"
 ms.author="danlep"/>

# <a name="create-the-head-node-of-an-hpc-pack-cluster-in-an-azure-vm-with-a-marketplace-image"></a>Stvorite glavni čvor programa paketa HPC klaster u programa VM Azure Marketplace slikom


Koristite [Microsoft HPC Pack virtualnog računala sliku](https://azure.microsoft.com/marketplace/partners/microsoft/hpcpack2012r2onwindowsserver2012r2/) iz trgovine Azure i Azure portal da biste stvorili čvor glavni programa klasteru HPC. Na ovoj se slici HPC paket VM temelji se na Windows Server 2012 R2 podatkovnog centra i HPC Pack 2012 R2 ažuriranja 3 unaprijed instalirano. Koristite ovaj glavni čvor za dokaz pojam implementacije HPC paketa u Azure. Zatim možete dodati računalnim čvorove klaster da biste pokrenuli radnih opterećenja HPC.



>[AZURE.TIP]Da biste implementirali dovršeno klaster HPC paketa u Azure koja sadrži glavni čvor i računalnim čvorove, preporučujemo da pomoću automatiziranog način. Mogućnosti obuhvaćaju [HPC paket IaaS implementacijsku skriptu](virtual-machines-windows-classic-hpcpack-cluster-powershell-script.md) i resursima predložak [klaster HPC paket za radnih opterećenja sustava Windows](https://azure.microsoft.com/marketplace/partners/microsofthpc/newclusterwindowscn/) . Dodatni predlošci potražite u članku [Mogućnosti klaster HPC paket Azure](virtual-machines-windows-hpcpack-cluster-options.md) . 


## <a name="planning-considerations"></a>Planiranje okolnosti

Kao što je prikazano na sljedećoj slici, morate je implementirati čvor paket HPC glavni u programa domene servisa Active Directory u Azure virtualne mreže.

![Glavni čvor HPC paketa][headnode]

* **Domene servisa active Directory** - u HPC paket glavni čvor mora biti pridruženo programa domene servisa Active Directory u Azure prije nego što počnete HPC usluge na na VM. Kao što je prikazano u ovom članku, dokaz o implementaciji pojam možete promicati VM stvarate za glavni čvor kao kontroler domene prije pokretanja servisa HPC. Druga mogućnost je za implementaciju kontroler zasebnom domene i skup stabala u Azure u koju se pridružite čvor glavni VM.

* **Azure virtualne mreže** – kada koristite model implementacije resursima za implementaciju čvor glavni, navesti ili stvoriti Azure virtualne mreže. Ako vam je potrebna za pridruživanje čvor glavni postojeće domene servisa Active Directory koristite virtualne mreže. Morate kasnije da biste dodali računalnim čvor VMs klaster.

    
## <a name="steps-to-create-the-head-node"></a>Koraci za stvaranje glavni čvor

Slijede više razine korake da biste koristili Azure portal da biste stvorili programa Azure VM za čvor glavni HPC paket pomoću model implementacije Voditelj resursa. 


1. Ako želite stvoriti novi skup stabala servisa Active Directory u Azure s kontrolera zasebnom domene VMs, jedna od mogućnosti jest da biste koristili [predložak Voditelj resursa](https://azure.microsoft.com/documentation/templates/active-directory-new-domain-ha-2-dc/). Za jednostavne dokaz o implementaciji pojam je u redu izostavite ovaj korak i konfigurirali glavni čvor VM sam kao kontroler domene. Ova mogućnost je opisan u nastavku ovog članka.
    
2. Na na [HPC paket 2012 R2 na stranici Windows Server 2012 R2](https://azure.microsoft.com/marketplace/partners/microsoft/hpcpack2012r2onwindowsserver2012r2/) u trgovine Windows Azure, kliknite **Stvaranje virtualnog računala**. 

3. Na portalu, na stranici **HPC paket 2012 R2 na Windows Server 2012 R2** odaberite model implementacije **Voditelj resursa** , a zatim kliknite **Stvori**.

    ![Paket HPC slike][marketplace]

4. Pomoću portala za konfiguriranje postavki i stvarati na VM. Ako ste novi korisnik Azure, slijedite vodič [Stvaranje virtualnog računala za Windows Azure portalu](virtual-machines-windows-hero-tutorial.md). Dokaz pojam implementacije, obično prihvatite zadane ili preporučenih postavki.

    >[AZURE.NOTE]Ako želite pridružiti čvor glavni postojeće domene servisa Active Directory u Azure, provjerite jesu li virtualne mreže za tu domenu pri stvaranju na VM.
       
4. Kad stvorite na VM i na VM traje, [povezati s VM](virtual-machines-windows-connect-logon.md) udaljene radne površine. 

5. Pridružite se VM postojeći skup stabala domene ili stvaranje skupa stabala domene na VM sam.

    * Ako ste stvorili u VM u Azure virtualne mreže s postojećeg skupa stabala domene, pridružite se VM u skup stabala pomoću standardnih alata za Upravitelj poslužitelja ili komponente Windows PowerShell. Zatim ponovno pokrenite.

    * Ako ste stvorili u VM u novi virtualne mreže (bez postojeći skup stabala domene), na VM Promicanje kao kontroler domene. Pomoću standardnih koraka instalirate i konfigurirate uloge servisa Active Directory Domain Services na glavni čvor. Detaljne korake potražite u članku [Instalacija na novi Windows Server 2012 Active Directory skupa stabala](https://technet.microsoft.com/library/jj574166.aspx).

5. Kada u VM pokrenut je, a pridruženo skupa stabala u servisu Active Directory, pokrenite HPC paket servisa na sljedeći način:

    na. Povezivanje s čvor glavni VM pomoću računa za domenu koja je član lokalne grupe administratora. Ako, na primjer, pomoću administratorskog računa postavite stvaranja čvor glavni VM.

    b. Za zadanu konfiguraciju glavni čvor, pokrenite Windows PowerShell kao administrator i upišite sljedeće:

    ```
    & $env:CCP_HOME\bin\HPCHNPrepare.ps1 –DBServerInstance ".\ComputeCluster"
    ```

    Može potrajati nekoliko minuta za servise HPC paket za pokretanje.

    Mogućnosti za konfiguriranje dodatne glavni čvor upišite `get-help HPCHNPrepare.ps1`.


## <a name="next-steps"></a>Daljnji koraci

* Sada možete raditi s čvor glavni svoj klaster HPC paket. Ako, na primjer, pokrenite upravitelj HPC klaster, a dovršiti [Popis zadataka implementacije](https://technet.microsoft.com/library/jj884141.aspx).
* Ako želite povećati klaster izračunati kapaciteta na zahtjev, dodajte [čvorove Azure neprekidnog rada](virtual-machines-windows-classic-hpcpack-cluster-node-burst.md) u oblaku. 

* Pokušajte pokrenuti test radno opterećenje na klaster. Na primjer, pročitajte članak HPC paket [Vodič za početak rada](https://technet.microsoft.com/library/jj884144).

<!--Image references-->
[headnode]: ./media/virtual-machines-windows-hpcpack-cluster-headnode/headnode.png
[marketplace]: ./media/virtual-machines-windows-hpcpack-cluster-headnode/marketplace.png
