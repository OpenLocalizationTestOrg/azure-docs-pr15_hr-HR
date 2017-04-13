<properties 
   pageTitle="Upravljanje DNS poslužitelji koriste virtualne mreže (VNet)"
   description="Saznajte kako dodavati i uklanjati DNS poslužitelji u virtualne mreže (vnet)"
   services="virtual-network"
   documentationCenter="na"
   authors="jimdial"
   manager="carmonm"
   editor="tysonn" />
<tags 
   ms.service="virtual-network"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="03/15/2016"
   ms.author="jdial" />

# <a name="manage-dns-servers-used-by-a-virtual-network-vnet"></a>Upravljanje DNS poslužitelji koriste virtualne mreže (VNet)

Možete upravljati na popisu DNS poslužitelja u VNet na portalu za upravljanje ili u konfiguracijskoj datoteci mreže. Možete dodati do 12 DNS poslužitelji za svaku VNet. Prilikom određivanja DNS poslužitelji, važno je da biste provjerili sastavite popis DNS poslužitelji ispravnim redoslijedom okruženju sustava. Popisi DNS poslužitelj ne funkcioniraju kružnog. Se koriste u redoslijedu koje su navedeni. Ako je prvi DNS poslužitelj na popisu moći proslijediti poziv, klijent će koristiti taj DNS poslužitelj bez obzira na to jesu li DNS poslužitelj funkcionira ispravno ili ne. Da biste promijenili redoslijed DNS poslužitelja za virtualne mreže, DNS poslužitelji uklonili s popisa, i dodajte ih ponovno redoslijedom kojim želite.

>[AZURE.WARNING] Kada je ažuriran popisu DNS, morate ponovno pokrenuti virtualnih računala koja se nalazi u vašoj mreži virtualne tako da se obraditi nove postavke DNS poslužitelja. Virtualnim strojevima će nastaviti koristiti svoje trenutne konfiguracije dok ih ponovno pokrenete.

## <a name="edit-a-dns-server-list-for-a-virtual-network-using-the-management-portal"></a>Uredite popis DNS poslužitelja za virtualne mreže pomoću portala za upravljanje

1. Prijavite se na **Portal za upravljanje**.

1. U navigacijskom oknu kliknite **mreža**, a zatim naziv virtualne mreže u stupcu **naziv** .

1. Kliknite **Konfiguriraj**.

1. U **DNS poslužitelji**možete konfigurirati sljedeće:

    - **Da biste registrirali (Dodaj) novi DNS poslužitelj –** Jednostavno upišite naziv i IP adresu u okvire. Dodati DNS poslužitelj virtualne mreže popis DNS poslužitelji i i registrira DNS poslužitelj s Azure.

    - **Da biste dodali DNS poslužitelja koji je već bila registrirana –** Ako ste već registrirali DNS poslužitelj s Azure, možete je odaberite s popisa unaprijed unesene.

    - **Da biste uklonili DNS poslužitelj s mrežom virtualne –** Kliknite X pokraj poslužitelja koji želite ukloniti. Imajte na umu da to uklanja samo poslužitelj s ovog popisa virtualne mreže. DNS poslužitelj ostaje registriranih u Azure za druge virtualne mreže da biste koristili. Da biste izbrisali DNS poslužitelj iz pretplate, otvorite stranicu **mreža -> DNS poslužitelji** .

    - **Da biste ponovno naručiti DNS poslužitelji –** Ukloni sve DNS poslužitelje koje su navedene, a zatim ih dodati ponovno redoslijedom kojim želite. Imajte na umu da to nije na popisu DNS-kružnog.

    - **Da biste preimenovali DNS poslužitelj –** DNS poslužitelj na popisu, a zatim upišite novi naziv. To će registrirati novi DNS poslužitelj Azure, kao i dodali na popis DNS poslužitelji za virtualne mreže. Stari DNS poslužitelj i IP adrese ostat će registrirani s Azure. Možete je izbrisati na stranici **DNS poslužitelji** ako ne koristite ga za druge virtualne mreže.

1. Kliknite **Spremi** pri dnu stranice da biste spremili novi DNS poslužitelji konfiguracijom.

1. Ponovno pokrenite virtualnim strojevima koji se nalazi u virtualne mreže im omogućuju da nabavite nove postavke DNS-a.

## <a name="edit-a-dns-server-list-using-a-network-configuration-file"></a>Uređivanje popisa DNS poslužitelja pomoću mreže konfiguracijska datoteka

Da biste uredili popis DNS poslužitelja putem mreže konfiguracijska datoteka, prvo ćete izvoz postavki konfiguriranje iz Portal za upravljanje. Zatim će uređivanje konfiguracijska datoteka mreže i uvezite ga unatrag kroz Portal za upravljanje. Slijedi popis više razine korake da biste dovršili postupak.

1. Izvoz postavki virtualne mreže u mreži konfiguracijska datoteka. Dodatne informacije i koraci za izvoz konfiguracije postavke mreže potražite u članku [Izvoz virtualne postavke mreže mreže konfiguracijska datoteka](virtual-networks-using-network-configuration-file.md).

1. Navođenje podataka za DNS poslužitelja za virtualne mreže. Dodatne informacije o DNS poslužitelj potražite u članku [Određivanje DNS poslužitelja u virtualne mreže konfiguracijskoj datoteci](virtual-networks-specifying-a-dns-settings-in-a-virtual-network-configuration-file.md). Dodatne informacije o mrežnim datotekama konfiguracije potražite u članku [Azure virtualne mreže konfiguracije sheme](https://msdn.microsoft.com/library/azure/jj157100.aspx) i [Konfiguriranje virtualne mreže pomoću mreže konfiguracijska datoteka](virtual-networks-using-network-configuration-file.md).

1. Uvezete datoteku konfiguracije mreže. Dodatne informacije i koraci za uvoz konfiguracijskoj datoteci mreže potražite u članku [uvezete datoteku konfiguracije mreže](virtual-networks-using-network-configuration-file.md).

1. Ponovno pokrenite virtualnim strojevima koji se nalazi u virtualne mreže im omogućuju da nabavite nove postavke DNS-a.
