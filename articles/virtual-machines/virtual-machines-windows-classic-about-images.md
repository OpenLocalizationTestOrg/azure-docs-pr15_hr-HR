<properties
    pageTitle="O slike za virtualnim strojevima sa sustavom Windows | Microsoft Azure"
    description="Saznajte kako se koriste slike s virtualnim računalima sustava Windows u Azure."
    services="virtual-machines-windows"
    documentationCenter=""
    authors="cynthn"
    manager="timlt"
    editor="tysonn"
    tags="azure-service-management"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/21/2016"
    ms.author="cynthn"/>

# <a name="about-images-for-windows-virtual-machines"></a>O slike za virtualnim strojevima sa sustavom Windows

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]

[AZURE.INCLUDE [virtual-machines-common-classic-about-images](../../includes/virtual-machines-common-classic-about-images.md)]



## <a name="working-with-images"></a>Rad sa slikama

Modul Azure PowerShell možete koristiti da biste upravljali slike koje su dostupne u pretplatu za Azure. Možete koristiti Azure klasični portal za neke zadatke slike, ali u naredbenom retku pruža više mogućnosti.


-   **Dohvaćanje svih slika**:`Get-AzureVMImage`vraća popis svih slika koje su dostupne u trenutne pretplate: slike, kao i one koje ste dobili od Azure ili partnera. To znači da mogla bi vam se veliki popis. Sljedeći primjeri pokazuju kako doći do kraći popis.
-   **Dohvati sliku linije**:`Get-AzureVMImage | select ImageFamily` može vidjeti popis slika linije prikazom nizovi **ImageFamily** svojstvo.
-   **Dohvati sve slike iz određene skupine**:`Get-AzureVMImage | Where-Object {$_.ImageFamily -eq $family}`
-   **Pronalaženje slika VM**: `Get-AzureVMImage | where {(gm –InputObject $_ -Name DataDiskConfigurations) -ne $null} | Select -Property Label, ImageName` to funkcionira filtriranjem DataDiskConfiguration svojstvo koje se odnosi samo na VM slike. U ovom se primjeru filtrira i izlaz samo naziv oznake i slike.
-   **Spremanje generalizirano slike**:`Save-AzureVMImage –ServiceName "myServiceName" –Name "MyVMtoCapture" –OSState "Generalized" –ImageName "MyVmImage" –ImageLabel "This is my generalized image"`
-   **Spremanje specijalizirane slike**:`Save-AzureVMImage –ServiceName "mySvc2" –Name "MyVMToCapture2" –ImageName "myFirstVMImageSP" –OSState "Specialized" -Verbose`
>[Azure.Tip] Ako želite stvoriti VM slika, što obuhvaća diskova podataka, kao i na disku operacijski sustav, potreban je parametar OSState. Ako ne koristite parametar, cmdlet stvara OS sliku. Vrijednost parametra označava hoće li se slike GRG generalizirano ili oblici sadrže, na temelju li sadrži radi ponovnog korištenja pripremili disk operacijski sustav.
-   **Brisanje slika**učinite sljedeće:`Remove-AzureVMImage –ImageName "MyOldVmImage"`


## <a name="next-steps"></a>Daljnji koraci

Možete i [stvoriti Windows računala pomoću portala za klasični](virtual-machines-windows-classic-tutorial.md)

