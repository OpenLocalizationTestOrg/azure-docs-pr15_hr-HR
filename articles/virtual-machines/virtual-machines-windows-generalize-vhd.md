<properties
    pageTitle="Generalize Windows VHD | Microsoft Azure"
    description="Saznajte kako koristiti Sysprep da biste generalize VM Windows će se koristiti za implementaciju modela Voditelj resursa."
    services="virtual-machines-windows"
    documentationCenter=""
    authors="cynthn"
    manager="timlt"
    editor="tysonn"
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/20/2016"
    ms.author="cynthn"/>
    
    
    
    
# <a name="generalize-a-windows-virtual-machine-using-sysprep"></a>Generalize virtualni stroj Windows koristi Sysprep

U ovom se odjeljku objašnjava generalize Windows virtualnog računala za korištenje kao sliku. Sysprep uklanja sve osobnih podataka o računu, između ostalog, i Priprema računala koja će se koristiti kao sliku. Detalje o Sysprep potražite u članku [upute za korištenje Sysprep: programa Uvod](http://technet.microsoft.com/library/bb457073.aspx).

Provjerite je li Sysprep podržanog uloge poslužitelja koji se izvode na računalu. Dodatne informacije potražite u članku [Sysprep podrška za uloge poslužitelja](https://msdn.microsoft.com/windows/hardware/commercialize/manufacture/desktop/sysprep-support-for-server-roles)

>[AZURE.IMPORTANT] Ako koristite Sysprep prije prijenosa na VHD za Azure prvo, provjerite imate [pripremljeni vaše VM](virtual-machines-windows-prepare-for-upload-vhd-image.md) prije pokretanja Sysprep. 

1. Prijavite se na Windows virtualnog računala.

2. Otvorite prozor naredbenog retka kao administrator. Promijeniti direktorij **%windir%\system32\sysprep**, a zatim pokrenite `sysprep.exe`.

3. U dijaloškom okviru **Alat za pripremu sustava** odaberite **sučelje – okvir unesite sustava (OOBE)**pa provjerite je li potvrđen okvir **konf** .

4. U odjeljku **Mogućnosti isključivanja**odaberite **isključivanje**.

5. Kliknite **u redu**.

    ![Pokrenite Sysprep](./media/virtual-machines-windows-upload-image/sysprepgeneral.png)

6. Kada se dovrši Sysprep, zatvara ga virtualnog računala. 

## <a name="next-steps"></a>Daljnji koraci

- Ako je na VM lokalnog, možete ga odmah [prenijeti VHD za Azure](virtual-machines-windows-upload-image.md).
- Ako se na VM već Azure, možete ga sada [stvoriti sliku s generalizirano VM](virtual-machines-windows-capture-image.md).