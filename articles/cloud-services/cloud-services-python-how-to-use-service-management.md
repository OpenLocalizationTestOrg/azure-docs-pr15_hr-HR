<properties
    pageTitle="Kako koristiti Upravljanje servisom API (Python) – vodič za značajku"
    description="Saznajte kako programski iz Python izvoditi uobičajene zadatke upravljanja servisa."
    services="cloud-services"
    documentationCenter="python"
    authors="lmazuel"
    manager="wpickett"
    editor=""/>

<tags
    ms.service="cloud-services"
    ms.workload="tbd"
    ms.tgt_pltfrm="na"
    ms.devlang="python"
    ms.topic="article"
    ms.date="09/06/2016"
    ms.author="lmazuel"/>

# <a name="how-to-use-service-management-from-python"></a>Kako koristiti Upravljanje servisom iz Python

> [AZURE.NOTE] Servis za upravljanje API je zamijenjena s novi resurs upravljanje API-jem, trenutno dostupno u pretpregledu aplikacije.  Potražite u [dokumentaciji Upravljanje resursima Azure](http://azure-sdk-for-python.readthedocs.org/) detalje o korištenju novog API upravljanje resursa iz Python.

Ovaj vodič prikazuje kako programski iz Python izvoditi uobičajene zadatke upravljanja servisa. Klase **ServiceManagementService** u [Azure SDK Python](https://github.com/Azure/azure-sdk-for-python) podržava programatski pristup velik broj usluge vezane uz upravljanje funkcionalnosti koji je dostupan [portal za Azure klasični] [ management-portal] (kao što su **Stvaranje, ažuriranje, i brisanje servise u oblaku, implementacije, upravljanje podacima i virtualnih računala**). Ta je funkcija može biti korisno u stvaranje aplikacija koje je potrebno programski pristup za upravljanje servisom.

## <a name="WhatIs"> </a>Što je Upravljanje servisom
Upravljanje API servisa omogućuje programatski pristup većina funkcija upravljanja za servis putem [Azure klasični portal][management-portal]. Azure SDK Python omogućuje upravljanje servise u oblaku i račune za pohranu.

Da biste koristili upravljanje API servisa, morate [stvoriti račun za Azure](https://azure.microsoft.com/pricing/free-trial/).

## <a name="Concepts"> </a>Koncepti
Azure SDK Python prelama [Upravljanje API -JA servisa Azure][svc-mgmt-rest-api], koji je REST API-JA. Sve operacije API izvršavaju putem SSL i međusobno autentičnost korištenje X.509 v3 certifikata. Servis za upravljanje može pristupiti iz unutar servisa operacijski u Azure ili izravno putem Interneta na bilo koji program koji možete poslati zahtjev HTTP i primati je odgovor HTTPS.

## <a name="Installation"> </a>Instalacije

Sve značajke opisane u ovom članku dostupne su u na `azure-servicemanagement-legacy` paketa koje možete instalirati pomoću točaka. Za dodatne pojedinosti o instalacija (na primjer, ako ste novi korisnik Python), pogledajte članak: [instalacije Python i Azure SDK](../python-how-to-install.md)

## <a name="Connect"> </a>Kako: povezivanje s Upravljanje servisom
Da biste povezali krajnja točka za upravljanje servisom, morate identifikacijskog Broja za Azure pretplate i upravljanje valjani certifikat. Možete dobiti svoj ID pretplate putem [Azure klasični portal][management-portal].

> [AZURE.NOTE] Od SDK Azure Python v0.8.0, sada je moguće koristiti certifikati stvorena pomoću OpenSSL kada se pokrene u sustavu Windows.  Potrebna je Python 2.7.4 ili noviji. Preporučujemo da se korisnicima omogućili korištenje OpenSSL umjesto .pfx, od podrška za .pfx certifikati vjerojatno uklonit će se u budućnosti.

### <a name="management-certificates-on-windowsmaclinux-openssl"></a>Upravljanje certifikata na Windows i Mac i Linux (OpenSSL)
Stvaranje certifikata za upravljanje možete koristiti [OpenSSL](http://www.openssl.org/) .  Zapravo morate stvoriti dvije certifikata, jedan za poslužitelj (u `.cer` datoteka) i jedan za klijentski program (u `.pem` datoteka). Da biste stvorili na `.pem` datoteka, izvršiti:

    openssl req -x509 -nodes -days 365 -newkey rsa:1024 -keyout mycert.pem -out mycert.pem

Da biste stvorili na `.cer` certifikata, izvršiti:

    openssl x509 -inform pem -in mycert.pem -outform der -out mycert.cer

Dodatne informacije o Azure certifikata potražite u članku [Pregled certifikata za servise u Oblaku Azure](./cloud-services-certs-create.md). Za potpuni opis parametara OpenSSL potražite u dokumentaciji pri [http://www.openssl.org/docs/apps/openssl.html](http://www.openssl.org/docs/apps/openssl.html).

Nakon stvaranja te datoteke, morate prenijeti na `.cer` datoteke za Azure putem "Prenesi" Akcije "postavke" [Azure klasični portal][management-portal], i trebali biste zabilježite koju ste spremili na `.pem` datoteku.

Kad dobijete identifikacijskog Broja za pretplatu, stvorili certifikat i prenijeti na `.cer` datoteka za Azure, možete se povezati s krajnju točku Azure upravljanje prosljeđivanjem id pretplate i put do na `.pem` datoteku da biste **ServiceManagementService**:

    from azure import *
    from azure.servicemanagement import *

    subscription_id = '<your_subscription_id>'
    certificate_path = '<path_to_.pem_certificate>'

    sms = ServiceManagementService(subscription_id, certificate_path)

U prethodnom primjeru, `sms` **ServiceManagementService** objekt. Klase **ServiceManagementService** je primarni predmete služi za upravljanje uslugama za Azure.

### <a name="management-certificates-on-windows-makecert"></a>Upravljanje certifikata u sustavu Windows (MakeCert)

Upravljanje samopotpisani certifikat možete stvoriti na računalo pomoću `makecert.exe`.  Otvaranje programa **Visual Studio naredbenog retka** kao **administrator** i koristite sljedeću naredbu zamjena *AzureCertificate* naziv certifikata koji želite koristiti.

    makecert -sky exchange -r -n "CN=AzureCertificate" -pe -a sha1 -len 2048 -ss My "AzureCertificate.cer"

Naredba stvara na `.cer` datoteku, a potom instalira u spremištu **osobnih** certifikata. Dodatne informacije potražite u članku [Pregled certifikata za servise u Oblaku Azure](./cloud-services-certs-create.md).

Kada stvorite certifikata, morate prenijeti na `.cer` datoteke za Azure putem "Prenesi" Akcije "postavke" [Azure klasični portal][management-portal].

Kad dobijete identifikacijskog Broja za pretplatu, stvorili certifikat i prenijeti na `.cer` datoteka za Azure, možete se povezati s krajnju točku Azure upravljanje prosljeđivanjem id pretplate i mjesto certifikat u spremištu **osobnih** certifikata **ServiceManagementService** (ponovno replace *AzureCertificate* pod nazivom certifikat):

    from azure import *
    from azure.servicemanagement import *

    subscription_id = '<your_subscription_id>'
    certificate_path = 'CURRENT_USER\\my\\AzureCertificate'

    sms = ServiceManagementService(subscription_id, certificate_path)

U prethodnom primjeru, `sms` **ServiceManagementService** objekt. Klase **ServiceManagementService** je primarni predmete služi za upravljanje uslugama za Azure.

## <a name="ListAvailableLocations"> </a>Kako: popis dostupnih mjesta

Na popisu mjesta na koje su dostupne za usluge hostiranja, koristite na **popis\_mjesta** metoda:

    from azure import *
    from azure.servicemanagement import *

    sms = ServiceManagementService(subscription_id, certificate_path)

    result = sms.list_locations()
    for location in result:
        print(location.name)

Kada stvorite servis u oblaku ili servis za pohranu, morate upisati valjano mjesto. Na **popis\_mjesta** način uvijek vraća ažurirani popis trenutno dostupna mjesta. Od pisanja ovog mjesta dostupnih su:

- Europa Zapad
- Sjeverna Europa
- Jugoistočne Azije
- Istočnoazijski
- Središnje SAD-a
- Sjeverna središnje SAD-a
- Južna središnje SAD-a
- Zapad SAD-a
- Istočni SAD-a
- Istok Japan
- Japan Zapad
- Južna Brazil
- Istok Australija
- Australija Jugoistok

## <a name="CreateCloudService"> </a>Kako: Stvaranje servis u oblaku

Kada stvorite aplikacije i pokretanje u Azure, kod i konfiguraciji zajedno se nazivaju Azure [servis u oblaku][] (poznatom kao na *hostira servisa* iz prethodnih izdanja Azure). Na **Stvaranje\_hostira\_servisa** način omogućuje vam da biste stvorili novi servis za unosom naziva servis (koji moraju biti jedinstveni u Azure), natpis (automatski kodirana za base64), opis i mjesto.

    from azure import *
    from azure.servicemanagement import *

    sms = ServiceManagementService(subscription_id, certificate_path)

    name = 'myhostedservice'
    label = 'myhostedservice'
    desc = 'my hosted service'
    location = 'West US'

    sms.create_hosted_service(name, label, desc, location)

Čije su smještene usluge za pretplatu na **popis\_hostira\_services** metoda:

    result = sms.list_hosted_services()

    for hosted_service in result:
        print('Service name: ' + hosted_service.service_name)
        print('Management URL: ' + hosted_service.url)
        print('Location: ' + hosted_service.hosted_service_properties.location)
        print('')

Ako želite da biste dobili informacije o servisu određenom glavnom računalu, to možete učiniti prosljeđivanjem naziva servis da biste na **dobiti\_hostira\_servisa\_svojstva** metoda:

    hosted_service = sms.get_hosted_service_properties('myhostedservice')

    print('Service name: ' + hosted_service.service_name)
    print('Management URL: ' + hosted_service.url)
    print('Location: ' + hosted_service.hosted_service_properties.location)

Kada stvorite servis u oblaku, možete implementirati kod za servis pomoću na **Stvaranje\_implementaciju** način.

## <a name="DeleteCloudService"> </a>Kako: brisanje servis u oblaku

Možete izbrisati servise u oblaku prosljeđivanjem naziva servisa da biste na **Brisanje\_hostira\_servisa** metoda:

    sms.delete_hosted_service('myhostedservice')

Da biste izbrisali servisa, sve implementaciji servisa za izbrisati. (U odjeljku [Kako: brisanje implementacije](#DeleteDeployment) detalje.)

## <a name="DeleteDeployment"> </a>Kako: brisanje implementacije

Da biste izbrisali implementacije, koristite na **Brisanje\_implementaciju** način. Sljedeći primjer prikazuje način da biste izbrisali implementacije pod nazivom `v1`.

    from azure import *
    from azure.servicemanagement import *

    sms = ServiceManagementService(subscription_id, certificate_path)

    sms.delete_deployment('myhostedservice', 'v1')

## <a name="CreateStorageService"> </a>Kako: Stvaranje servis za pohranu

[Servis za pohranu](../storage/storage-create-storage-account.md) omogućuje vam pristup Azure [blob-ova](../storage/storage-python-how-to-use-blob-storage.md), [tablice](../storage/storage-python-how-to-use-table-storage.md)i [redova](../storage/storage-python-how-to-use-queue-storage.md). Da biste stvorili servis za pohranu, potrebno vam je naziv servisa (između 3 i 24 malim slovima i jedinstven unutar Azure), opis, natpis (do 100 znakova, automatski kodirana za base64) i mjesto. Sljedeći primjer pokazuje kako stvoriti servis za pohranu navođenjem mjesto.

    from azure import *
    from azure.servicemanagement import *

    sms = ServiceManagementService(subscription_id, certificate_path)

    name = 'mystorageaccount'
    label = 'mystorageaccount'
    location = 'West US'
    desc = 'My storage account description.'

    result = sms.create_storage_account(name, desc, label, location=location)

    operation_result = sms.get_operation_status(result.request_id)
    print('Operation status: ' + operation_result.status)

Bilješke u prethodnom primjeru koji status na **Stvaranje\_prostora za pohranu\_račun** postupak mogu biti dohvaćeni prosljeđivanjem rezultat koji je vratio **Stvaranje\_prostora za pohranu\_račun** da biste na **dobiti\_operacija\_status** način.  

Možete navesti račune za pohranu i njihova svojstva s na **popis\_prostora za pohranu\_računi** metoda:

    from azure import *
    from azure.servicemanagement import *

    sms = ServiceManagementService(subscription_id, certificate_path)

    result = sms.list_storage_accounts()
    for account in result:
        print('Service name: ' + account.service_name)
        print('Location: ' + account.storage_service_properties.location)
        print('')

## <a name="DeleteStorageService"> </a>Kako: brisanje servis za pohranu

Servis za pohranu možete izbrisati tako da naziv servisa za pohranu za na **Brisanje\_prostora za pohranu\_račun** način. Brisanje servis za pohranu briše sve podatke pohranjene u servisu (blob-ova, tablice i redova).

    from azure import *
    from azure.servicemanagement import *

    sms = ServiceManagementService(subscription_id, certificate_path)

    sms.delete_storage_account('mystorageaccount')

## <a name="ListOperatingSystems"> </a>Kako: popis dostupnih operacijski sustavi

Da biste popis operacijskih sustava koji su dostupni za usluge hostiranja, koristite na **popis\_operacijskom\_sustavi** metoda:

    from azure import *
    from azure.servicemanagement import *

    sms = ServiceManagementService(subscription_id, certificate_path)

    result = sms.list_operating_systems()

    for os in result:
        print('OS: ' + os.label)
        print('Family: ' + os.family_label)
        print('Active: ' + str(os.is_active))

Umjesto toga možete upotrijebiti u **popis\_radi\_sustava\_linije** način, grupira operacijski sustavi po obitelji:

    result = sms.list_operating_system_families()

    for family in result:
        print('Family: ' + family.label)
        for os in family.operating_systems:
            if os.is_active:
                print('OS: ' + os.label)
                print('Version: ' + os.version)
        print('')

## <a name="CreateVMImage"> </a>Kako: Stvaranje slike operacijski sustav

Da biste dodali sliku operacijski sustav spremište slike, koristite na **Dodavanje\_os\_slika** metoda:

    from azure import *
    from azure.servicemanagement import *

    sms = ServiceManagementService(subscription_id, certificate_path)

    name = 'mycentos'
    label = 'mycentos'
    os = 'Linux' # Linux or Windows
    media_link = 'url_to_storage_blob_for_source_image_vhd'

    result = sms.add_os_image(label, media_link, name, os)

    operation_result = sms.get_operation_status(result.request_id)
    print('Operation status: ' + operation_result.status)

Da biste popis slika operacijski sustav koji su dostupni, koristite na **popis\_os\_slike** način. Obuhvaća sve slike platforme i slike korisnika:

    result = sms.list_os_images()

    for image in result:
        print('Name: ' + image.name)
        print('Label: ' + image.label)
        print('OS: ' + image.os)
        print('Category: ' + image.category)
        print('Description: ' + image.description)
        print('Location: ' + image.location)
        print('Media link: ' + image.media_link)
        print('')

## <a name="DeleteVMImage"> </a>Kako: brisanje slika operacijski sustav

Da biste izbrisali sliku korisnika, koristite na **Brisanje\_os\_slika** metoda:

    from azure import *
    from azure.servicemanagement import *

    sms = ServiceManagementService(subscription_id, certificate_path)

    result = sms.delete_os_image('mycentos')

    operation_result = sms.get_operation_status(result.request_id)
    print('Operation status: ' + operation_result.status)

## <a name="CreateVM"> </a>Kako: Stvaranje virtualnog računala

Da biste stvorili virtualnog računala, najprije morate stvoriti [u oblaku](#CreateCloudService).  Stvaranje pomoću implementacije virtualnog računala na **Stvaranje\_virtualne\_strojno\_implementaciju** metoda:

    from azure import *
    from azure.servicemanagement import *

    sms = ServiceManagementService(subscription_id, certificate_path)

    name = 'myvm'
    location = 'West US'

    #Set the location
    sms.create_hosted_service(service_name=name,
        label=name,
        location=location)

    # Name of an os image as returned by list_os_images
    image_name = 'OpenLogic__OpenLogic-CentOS-62-20120531-en-us-30GB.vhd'

    # Destination storage account container/blob where the VM disk
    # will be created
    media_link = 'url_to_target_storage_blob_for_vm_hd'

    # Linux VM configuration, you can use WindowsConfigurationSet
    # for a Windows VM instead
    linux_config = LinuxConfigurationSet('myhostname', 'myuser', 'mypassword', True)

    os_hd = OSVirtualHardDisk(image_name, media_link)

    sms.create_virtual_machine_deployment(service_name=name,
        deployment_name=name,
        deployment_slot='production',
        label=name,
        role_name=name,
        system_config=linux_config,
        os_virtual_hard_disk=os_hd,
        role_size='Small')

## <a name="DeleteVM"> </a>Kako: brisanje virtualnog računala

Da biste izbrisali virtualnog računala, prvi put izbrišete pomoću implementacije u **Brisanje\_implementaciju** metoda:

    from azure import *
    from azure.servicemanagement import *

    sms = ServiceManagementService(subscription_id, certificate_path)

    sms.delete_deployment(service_name='myvm',
        deployment_name='myvm')

Servis u oblaku pa moguće je izbrisati pomoću na **Brisanje\_hostira\_servisa** metoda:

    sms.delete_hosted_service(service_name='myvm')

##<a name="how-to-create-a-virtual-machine-from-a-captured-virtual-machine-image"></a>Upute: Stvaranje virtualnog računala iz slike snimljenu virtualnog računala

Da biste snimili sliku VM, najprije pozovete na **snimiti\_vm\_slika** metoda:

    from azure import *
    from azure.servicemanagement import *

    sms = ServiceManagementService(subscription_id, certificate_path)

    # replace the below three parameters with actual values
    hosted_service_name = 'hs1'
    deployment_name = 'dep1'
    vm_name = 'vm1'

    image_name = vm_name + 'image'
    image = CaptureRoleAsVMImage    ('Specialized',
        image_name,
        image_name + 'label',
        image_name + 'description',
        'english',
        'mygroup')

    result = sms.capture_vm_image(
            hosted_service_name,
            deployment_name,
            vm_name,
            image
        )

Zatim, da biste provjerili je li uspješno zabilježene slike, pomoću na **popis\_vm\_slike** API-jem, i provjerite je li vaša slika se prikazuje u rezultatima:

    images = sms.list_vm_images()

Da biste na kraju stvorili virtualnog računala pomoću snimljenu sliku, poslužite se na **Stvaranje\_virtualne\_strojno\_implementaciju** način kao što je prije, ali ovaj put prenesite na vm_image_name umjesto toga

    from azure import *
    from azure.servicemanagement import *

    sms = ServiceManagementService(subscription_id, certificate_path)

    name = 'myvm'
    location = 'West US'

    #Set the location
    sms.create_hosted_service(service_name=name,
        label=name,
        location=location)

    sms.create_virtual_machine_deployment(service_name=name,
        deployment_name=name,
        deployment_slot='production',
        label=name,
        role_name=name,
        system_config=linux_config,
        os_virtual_hard_disk=None,
        role_size='Small',
        vm_image_name = image_name)

Da biste saznali više o tome kako snimiti Linux virtualnog računala, pročitajte članak [kako snimiti Linux virtualnog računala.](../virtual-machines/virtual-machines-linux-classic-capture-image.md)

Da biste saznali više o tome kako snimiti virtualnog računala za Windows, pročitajte članak [kako snimiti virtualnog računala za Windows.](../virtual-machines/virtual-machines-windows-classic-capture-image.md)

## <a name="What's Next"> </a>Sljedeće korake

Sad kad ste naučili osnove Upravljanje servisom, možete pristupiti [Dovršeno API referentnu dokumentaciju za Azure Python SDK](http://azure-sdk-for-python.readthedocs.org/) i izvode složene zadatke jednostavno da biste upravljali python aplikacije.

Dodatne informacije potražite u [Centru za razvojne inženjere Python](/develop/python/).

[What is Service Management]: #WhatIs
[Concepts]: #Concepts
[How to: Connect to service management]: #Connect
[How to: List available locations]: #ListAvailableLocations
[How to: Create a cloud service]: #CreateCloudService
[How to: Delete a cloud service]: #DeleteCloudService
[How to: Create a deployment]: #CreateDeployment
[How to: Update a deployment]: #UpdateDeployment
[How to: Move deployments between staging and production]: #MoveDeployments
[How to: Delete a deployment]: #DeleteDeployment
[How to: Create a storage service]: #CreateStorageService
[How to: Delete a storage service]: #DeleteStorageService
[How to: List available operating systems]: #ListOperatingSystems
[How to: Create an operating system image]: #CreateVMImage
[How to: Delete an operating system image]: #DeleteVMImage
[How to: Create a virtual machine]: #CreateVM
[How to: Delete a virtual machine]: #DeleteVM
[Next Steps]: #NextSteps
[management-portal]: https://manage.windowsazure.com/
[svc-mgmt-rest-api]: http://msdn.microsoft.com/library/windowsazure/ee460799.aspx


[servis u oblaku]:/services/cloud-services/

