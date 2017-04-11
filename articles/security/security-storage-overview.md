<properties
   pageTitle="Pregled sigurnosti Azure prostora za pohranu | Microsoft Azure"
   description=" Azure prostora za pohranu je rješenje za pohranu oblaka za Moderna aplikacije koje se temelje na rok trajanja, dostupnost i skalabilnost potrebama svoje kupce. Ovaj članak sadrži pregled temeljni Azure sigurnosne značajke koje se mogu koristiti u sklopu Azure prostora za pohranu. "
   services="security"
   documentationCenter="na"
   authors="TerryLanfear"
   manager="MBaldwin"
   editor="TomSh"/>

<tags
   ms.service="security"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="09/16/2016"
   ms.author="terrylan"/>

# <a name="azure-storage-security-overview"></a>Pregled sigurnosti Azure prostora za pohranu

Azure prostora za pohranu je rješenje za pohranu oblaka za Moderna aplikacije koje se temelje na rok trajanja, dostupnost i skalabilnost potrebama svoje kupce. Azure prostora za pohranu daje opsežan skup sigurnosne mogućnosti:

- Račun za pohranu mogu zaštiti pomoću na temelju uloga kontrola pristupa i Azure Active Directory.
- Podatke možete zaštićenim na putu između aplikacije i Azure pomoću klijentsko šifriranja, HTTPS ili SMB 3.0.
- Podatke možete postaviti automatski šifriranje kada zapisan Azure prostora za pohranu šifriranjem servis za pohranu.
- OS i podataka koristi virtualnim strojevima diskova moguće je postaviti šifriranje pomoću šifriranja Azure Disk.
- Delegirana pristup objektima podataka u spremište Azure se mogu dodijeliti korištenje potpisa za zajedničko korištenje programa Access.
- Način provjere autentičnosti koristi netko prilikom pristupa za pohranu možete pratiti pomoću analize prostora za pohranu.

Detaljniji pogled na sigurnost u Azure prostora za pohranu potražite u članku [Vodič za sigurnost Azure prostora za pohranu](../storage/storage-security-guide.md). Ovaj vodič sadrži na niže dive u sigurnosnim značajkama programa Azure prostora za pohranu kao što su tipke račun za pohranu, šifriranje podataka na putu i na ostale i analize prostora za pohranu.

Ovaj članak sadrži pregled Azure sigurnosne značajke koje se mogu koristiti u sklopu Azure prostora za pohranu. Veze su navedene na članke koji daju pojedinosti svake značajke mogu saznati više.

Ovo su osnovne funkcije da biste se prekriveno u ovom članku:

- Kontrola pristupa na temelju uloga
- Delegirana pristup objektima prostora za pohranu
- Šifriranje na putu
- Šifriranje na ostale/prostora za pohranu servisa šifriranje
- Azure šifriranje
- Azure sigurnog ključa

## <a name="role-based-access-control-rbac"></a>Kontrola pristupa na temelju uloga (RBAC)

Možete zaštititi svoj račun za pohranu na temelju uloga kontrole pristupa (RBAC). Ograničavanje pristupa na temelju sigurnost načela [informacije](https://en.wikipedia.org/wiki/Need_to_know) i [najmanjih ovlasti](https://en.wikipedia.org/wiki/Principle_of_least_privilege) je izuzetne tvrtkama ili ustanovama koje želite da biste nametnuli sigurnosna pravila za pristup podacima. Ta prava pristupa odobravaju dodjeljivanjem odgovarajuću ulogu RBAC grupe i aplikacije, na određene opseg. [Ugrađene RBAC uloge](../active-directory/role-based-access-built-in-roles.md), kao što su suradnik račun za pohranu, možete koristiti da biste korisnicima dodijelili ovlasti.

uči više:

- [Kontrola pristupa na temelju uloga Azure Active Directory](../active-directory/role-based-access-control-configure.md)

## <a name="delegated-access-to-storage-objects"></a>Delegirana pristup objektima prostora za pohranu

Zajednički pristup potpis (SAS) omogućuje prijenos ovlasti pristup resursa u račun za pohranu. Na SAS, to znači da možete dodijeliti klijent ograničene dozvole za objekte na vašem računu za pohranu za određeno vrijeme i određeni skup dozvola. Možete dodijeliti te ograničene dozvole bez potrebe za zajedničko korištenje ključeva za pristup računu. Na SAS je URI koji obuhvaća parametara upita sve podatke koje su potrebne za čija je autentičnost provjerena pristup resursu za pohranu. Da biste pristupili resursi za pohranu na SAS, klijent samo treba prenesite na SAS odgovarajući Graditelj ili način.

uči više:

- [Osnove SAS modela](../storage/storage-dotnet-shared-access-signature-part-1.md)
- [Stvaranje i korištenje SAS s spremište blobova platforme](../storage/storage-dotnet-shared-access-signature-part-2.md)

## <a name="encryption-in-transit"></a>Šifriranje na putu
Šifriranje na putu je mehanizam zaštite podataka kada se nesmetano putem mreže. S Azure pohranom možete zaštititi pomoću podataka:

- [Šifriranje prijenosa razine](../storage/storage-security-guide.md#encryption-in-transit), kao što su HTTPS kada prijenos podataka pojavljivanje ili iščezavanje Azure prostora za pohranu.
- [Šifriranje žičani](../storage/storage-security-guide.md#using-encryption-during-transit-with-azure-file-shares), kao što su SMB 3.0 šifriranje za Azure zajedničke datoteke.
- [Šifriranje klijentsko](../storage/storage-security-guide.md#using-client-side-encryption-to-secure-data-that-you-send-to-storage)za šifriranje podataka prije nego što se prenosi u prostor za pohranu i dešifrirati podatke nakon prenose se više mjesta za pohranu.

Dodatne informacije o klijentsko šifriranja:

- [Šifriranje klijentsko za pohranu Microsoft Azure](https://blogs.msdn.microsoft.com/windowsazurestorage/2015/04/28/client-side-encryption-for-microsoft-azure-storage-preview/)
- [Oblak sigurnosne kontrole niz: šifriranje podataka na putu](http://blogs.microsoft.com/cybertrust/2015/08/10/cloud-security-controls-series-encrypting-data-in-transit/)

## <a name="encryption-at-rest"></a>Šifriranje na ostale

Za mnoge organizacije [šifriranje podataka na ostale](https://blogs.microsoft.com/cybertrust/2015/09/10/cloud-security-controls-series-encrypting-data-at-rest/) je obavezna korak prema podataka o zaštiti privatnosti i usklađenost samostalnosti podataka. Postoje tri Azure značajke koje omogućuju šifriranje podataka koji je "na ostale":

- [Šifriranje servis za pohranu](../storage/storage-security-guide.md#encryption-at-rest) omogućuje vam da biste zatražili da servis za pohranu automatski šifrirali podataka prilikom pisanja Azure za pohranu.
- [Šifriranje klijentsko](../storage/storage-security-guide.md#client-side-encryption) omogućuje značajka šifriranja na ostale.
- [Šifriranje Azure](../storage/storage-security-guide.md#using-azure-disk-encryption-to-encrypt-disks-used-by-your-virtual-machines) omogućuje vam da biste šifrirali OS diskova i diskova podataka koristi se IaaS virtualnog računala.

Dodatne informacije o prostora za pohranu servisa šifriranja:

- [Šifriranje servis za pohranu za Azure](https://azure.microsoft.com/services/storage/) dostupna je za [Spremište blobova platforme Azure](https://azure.microsoft.com/services/storage/blobs/). Informacije o drugim vrstama Azure prostora za pohranu potražite u članku [datoteke](https://azure.microsoft.com/services/storage/files/), [Disk (Premium spremište)](https://azure.microsoft.com/services/storage/premium-storage/), [tablice](https://azure.microsoft.com/services/storage/tables/)i [red](https://azure.microsoft.com/services/storage/queues/).
- [Šifriranje servisa Azure prostora za pohranu za podatke na ostale](../storage/storage-service-encryption.md)

## <a name="azure-disk-encryption"></a>Azure šifriranje

Azure šifriranje diskovnih virtualnim strojevima (VMs) pomaže vam adresu tvrtke ili ustanove sigurnost i zahtjeve za usklađenosti šifrira vaše VM diskova (uključujući operacijskih sustava i podataka diskova) pomoću tipke i pravila kontrolu u [Sigurnog ključ Azure](https://azure.microsoft.com/services/key-vault/).

Šifriranje diskovnih VMs radi operacijski sustavi Linux i Windows. Također koristi sigurnog ključ da biste zaštitili, upravljanje i nadzor korištenje ključeva za šifriranje diska. Svi podaci u vašem VM diskova šifriran na ostale pomoću tehnologiju šifriranja standardni u svoje račune Azure prostora za pohranu. Šifriranje rješenja za Windows temelji se na [Microsoft BitLocker šifriranje pogona](https://technet.microsoft.com/library/cc732774.aspx), a rješenje Linux temelji se na [dm crypt](https://en.wikipedia.org/wiki/Dm-crypt).

uči više:

- [Azure šifriranje za Windows i Linux IaaS virtualnim strojevima](https://gallery.technet.microsoft.com/Azure-Disk-Encryption-for-a0018eb0)

## <a name="azure-key-vault"></a>Azure sigurnog ključa

Azure šifriranje koristi [Azure ključ sigurnog](https://azure.microsoft.com/services/key-vault/) će vam pomoći odrediti i upravljanje ključeva za šifriranje diska i tajne u pretplatu ključa sigurnog uz jamstvo da se svi podaci u diskova virtualnog računala šifrirane na ostale u Azure prostora za pohranu. Trebate koristiti sigurnog ključ za nadzor ključeva i korištenju pravila.

uči više:

- [Što je sigurnog ključ Azure?](../key-vault/key-vault-whatis.md)
- [Početak rada s sigurnog ključ Azure](../key-vault/key-vault-get-started.md)
