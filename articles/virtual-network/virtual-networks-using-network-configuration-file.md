<properties 
    pageTitle="Konfiguriranje virtualne mreže pomoću mreže konfiguracijska datoteka" 
    description="Upute za izvoz i uvoz mreže konfiguracijska datoteka za Portal za upravljanje Azure za stvaranje i izmjena virtualne mreže. " 
    services="virtual-network" 
    documentationCenter="" 
    authors="jimdial" 
    manager="carmonm" 
    editor="tysonn"/>

<tags
    ms.service="virtual-network"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="infrastructure-services" 
    ms.date="03/15/2016"
    ms.author="jdial"/>

# <a name="configure-a-virtual-network-using-a-network-configuration-file"></a>Konfiguriranje virtualne mreže pomoću mreže konfiguracijska datoteka

Virtualne mreže (VNet) možete konfigurirati tako da pomoću portala za upravljanje Azure ili pak putem mreže konfiguracijska datoteka.

## <a name="creating-and-modifying-a-network-configuration-file"></a>Stvaranje i izmjena mreže konfiguracijska datoteka 
Da biste izradili mreže konfiguracijska datoteka najjednostavnije da biste izvezli postavke mreže s postojeće virtualne mrežnoj konfiguraciji izmjena da dokument sadrži postavke koje želite konfigurirati virtualne mreže.

Da biste uredili konfiguracijska datoteka mrežu, možete jednostavno otvorite datoteku, unesite odgovarajuće promjene, a zatim spremite datoteku. *Xml* uređivača možete koristiti da biste promijenili konfiguracijska datoteka mreže. 

Usko slijedite navedene upute za [mrežne konfiguracije datoteke sheme postavke](https://msdn.microsoft.com/library/azure/jj157100.aspx). 

Azure smatra podmreže koja sadrži nešto uveden u nju kao **koristi**. Kada se koristi podmreži, nije moguće mijenjati. Prije no što izmijenite, premjestite sve što ste implementiran na podmreži omogućili drugoj podmreži koji nije onemogućiti izmjenu.   Potražite u članku [Premještanje VM ili uloga instancu da biste drugoj podmreži](virtual-networks-move-vm-role-to-subnet.md).

## <a name="export-and-import-virtual-network-settings-using-the-management-portal"></a>Izvoz i Uvoz postavki virtualne mreže pomoću portala za upravljanje  
Možete uvesti i izvesti mreže konfiguracijske postavke koje se nalaze u konfiguracijskoj datoteci mreže pomoću programa PowerShell ili Portal za upravljanje. Upute u nastavku pomoći će vam izvoz i uvoz pomoću portala za upravljanje. 

### <a name="to-export-your-network-settings"></a>Da biste izvezli postavke mreže
Kada izvezete, sve postavke za virtualne mreže u vašoj pretplati će biti zapisane .xml datoteci. 

1. Prijava na **Portal za upravljanje**.
2. Na portalu za upravljanje, na dnu stranice **mreža** kliknite **Izvoz**. 
3. U prozoru **Izvoz mrežna konfiguracija** provjerite je li odabrana pretplatu za koju želite izvesti postavke mreže. Kliknite kvačicu na donjoj desnoj strani. 
4. Kada se od vas zatraži, spremite datoteku *NetworkConfig.xml* mjesto po izboru.


### <a name="to-import-your-network-settings"></a>Da biste uvezli postavke mreže

1. **Portal za upravljanje**u navigacijskom oknu u donjem lijevom kutu, kliknite **Novo**.
2. Kliknite **mrežnim servisima** -> **virtualne mreže** -> **Uvoz konfiguracije**.
3. Na stranici **Uvoz konfiguracijska datoteka mreže** dođite do datoteke konfiguracije mreže, a zatim strelica **sljedeće** .
4. Na stranici **Stvaranje mrežom** , vidjet ćete podatke na zaslon na kojem je prikazano koje dijelove mrežna konfiguracija će se promijeniti ili stvorili. Ako promjene izgledaju ispravnom, kliknite kvačicu da biste prešli na ažuriranje ili stvaranje virtualne mreže. 