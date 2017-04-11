<properties
    pageTitle="Korištenje Docker VM nastavka za Linux | Microsoft Azure"
    description="U članku se opisuje Docker i proširenja virtualnim računalima sustava Azure te kako stvoriti virtualnim računalima sustava Azure koji su docker hosts pomoću EŽA Azure u modelu klasični implementacije."
    services="virtual-machines-linux"
    documentationCenter=""
    authors="squillace"
    manager="timlt"
    editor="tysonn"
    tags="azure-service-management"/>

<tags
    ms.service="virtual-machines-linux"
    ms.devlang="multiple"
    ms.topic="article"
    ms.tgt_pltfrm="vm-linux"
    ms.workload="infrastructure-services"
    ms.date="05/27/2016"
    ms.author="rasquill"/>


# <a name="using-the-docker-vm-extension-with-the-azure-classic-portal"></a>Proširenje VM Docker pomoću portala za Azure klasični

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]


[Docker](https://www.docker.com/) je jedan od najpopularnijih virtualizacije pristupa koja koristi [Linux spremnika](http://en.wikipedia.org/wiki/LXC) umjesto virtualnim strojevima kao način izoliranja podataka i rad na zajedničkim resursima. Proširenje Docker VM upravlja [Azure Linux Agent] možete koristiti da biste stvorili Docker VM koji hostira bilo koji broj spremnici aplikacija na Azure.

> [AZURE.NOTE] U ovoj se temi opisuje kako stvoriti Docker VM s portala za Azure klasični. Da biste vidjeli kako stvoriti Docker VM u naredbenom retku, potražite u članku [upute za korištenje VM proširenje Docker iz Azure sučelja naredbenog retka (Azure EŽA)]. Da biste vidjeli više razine rasprave spremnika i njihove prednosti, potražite u članku [Docker visoke razine Zaslonska ploča](http://channel9.msdn.com/Blogs/Regular-IT-Guy/Docker-High-Level-Whiteboard).

## <a name="create-a-new-vm-from-the-image-gallery"></a>Stvaranje nove VM iz galerije slika
Prvi korak potreban je VM Azure Linux slike koji podržava VM proširenje Docker korištenja Ubuntu 14.04 LTS slike iz galerije slika kao primjer slike poslužitelja i radna površina 14.04 Ubuntu kao klijenta. Na portalu kliknite **+ Novo** u donjem lijevom kutu da biste stvorili novu instancu VM i odaberite Ubuntu 14.04 LTS sliku iz dostupnih odabira ili dovršeno Galerija slika kao što je prikazano u nastavku.

> [AZURE.NOTE] Trenutno samo Ubuntu 14.04 LTS slika novije od srpanj 2014 podržava VM nastavak Docker.

![Stvaranje nove Ubuntu slike](./media/virtual-machines-linux-classic-portal-use-docker/ChooseUbuntu.png)

## <a name="create-docker-certificates"></a>Stvaranje certifikata za Docker

Nakon stvaranja na VM provjerite je li instaliran Docker na klijentskom računalu. (Detalje potražite u članku [upute za instalaciju Docker](https://docs.docker.com/installation/#installation).)

Stvorite datoteke certifikat i ključ za komunikaciju Docker prema [Pokrenut Docker s https] i postavite ih na **`~/.docker`** imeničkog na klijentskom računalu.

> [AZURE.NOTE] Proširenje VM Docker na portalu trenutno zahtijeva vjerodajnice koje su base64 kodiran.

U naredbenom retku upotrijebite **`base64`** ili neki drugi omiljene kodiranja alat za stvaranje base64 kodirani teme. Taj postupak jednostavne skup certifikat i ključ datoteka može izgledati ovako:

```
 ~/.docker$ ls
 ca-key.pem  ca.pem  cert.pem  key.pem  server-cert.pem  server-key.pem
 ~/.docker$ base64 ca.pem > ca64.pem
 ~/.docker$ base64 server-cert.pem > server-cert64.pem
 ~/.docker$ base64 server-key.pem > server-key64.pem
 ~/.docker$ ls
 ca64.pem    ca.pem    key.pem            server-cert.pem   server-key.pem
 ca-key.pem  cert.pem  server-cert64.pem  server-key64.pem
```

## <a name="add-the-docker-vm-extension"></a>Dodajte VM nastavak Docker
Da biste dodali VM proširenje Docker, pronaći VM instancu koju ste stvorili i pomaknite se do odjeljka **proširenja** i kliknite je da bi se prikazala VM proširenja, kao što je prikazano u nastavku.
> [AZURE.NOTE] Ta je funkcija podržava samo portal za pretpregled: https://portal.azure.com/

![](./media/virtual-machines-linux-classic-portal-use-docker/ClickExtensions.png)
### <a name="add-an-extension"></a>Dodavanje datotečni nastavak
Kliknite **+ Dodaj** da biste prikazali moguće proširenja VM možete dodati u ovom VM.

![](./media/virtual-machines-linux-classic-portal-use-docker/ClickAdd.png)
### <a name="select-the-docker-vm-extension"></a>Odaberite VM proširenje Docker
Odaberite VM proširenje Docker koji unosi opis Docker i važne veze, a zatim kliknite **Stvori** pri dnu da biste započeli postupak instalacije.

![](./media/virtual-machines-linux-classic-portal-use-docker/ChooseDockerExtension.png)

![](./media/virtual-machines-linux-classic-portal-use-docker/CreateButtonFocus.png)
### <a name="add-your-certificate-and-key-files"></a>Dodavanje certifikata i ključa datoteke:

U poljima obrasca unesite verzije base64 kodirani ustanove za Izdavanje certifikata, vaš poslužitelj certifikat i ključ poslužitelja, kao što je prikazano u sljedeća grafika.

![](./media/virtual-machines-linux-classic-portal-use-docker/AddExtensionFormFilled.png)

> [AZURE.NOTE] Imajte na umu da (kao u prethodnom slike) na 2376 je prema zadanim postavkama. Možete unijeti bilo koju krajnje, ali na sljedeći korak će biti otvorite odgovarajući krajnjoj točki. Ako promijenite zadani, provjerite je li otvorite odgovarajući krajnje točke u sljedećem koraku.

## <a name="add-the-docker-communication-endpoint"></a>Dodavanje krajnje komunikacije Docker
Prilikom pregledavanja grupa resursa koje ste stvorili, odaberite sigurnosne grupe mreže povezane s vašeg VM pa kliknite **Ulazna pravila sigurnost** da biste pogledali pravila kao što je prikazano ovdje.

![](./media/virtual-machines-linux-classic-portal-use-docker/AddingEndpoint.png)

Kliknite **+ Dodaj** da biste dodali drugo pravilo, a u slučaju zadane, unesite naziv za krajnju točku (u ovom primjeru **Docker**) i 2376 'odredišni priključak raspon". Postavite vrijednost protokol prikazuje **TCP**, a zatim kliknite **u redu** da biste stvorili pravilo.

![](./media/virtual-machines-linux-classic-portal-use-docker/AddEndpointFormFilledOut.png)


## <a name="test-your-docker-client-and-azure-docker-host"></a>Testiranje Docker klijenta i Azure Docker glavnog računala
Pronađite i kopirajte naziv vaše VM domene, a zatim u naredbenom retku na klijentskom računalu, upišite `docker --tls -H tcp://` *dockerextension* `.cloudapp.net:2376 info` (gdje *dockerextension* zamijenjen je poddomena za vaše VM).

Rezultat trebao izgledati ovako:

```
$ docker --tls -H tcp://dockerextension.cloudapp.net:2376 info
Containers: 0
Images: 0
Storage Driver: devicemapper
 Pool Name: docker-8:1-131214-pool
 Pool Blocksize: 65.54 kB
 Data file: /var/lib/docker/devicemapper/devicemapper/data
 Metadata file: /var/lib/docker/devicemapper/devicemapper/metadata
 Data Space Used: 305.7 MB
 Data Space Total: 107.4 GB
 Metadata Space Used: 729.1 kB
 Metadata Space Total: 2.147 GB
 Library Version: 1.02.82-git (2013-10-04)
Execution Driver: native-0.2
Kernel Version: 3.13.0-36-generic
WARNING: No swap limit support
```

Kada dovršite gore navedene korake, sada imate potpuno funkcionalni Docker glavno računalo sustavom Azure VM, konfigurirano tako da se povezati s udaljenog mjesta s drugim klijentskim programima.

<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->
## <a name="next-steps"></a>Daljnji koraci

Spremni ste da biste se [Docker korisničkom priručniku] i koristiti vaš VM Docker. Ako želite da biste automatizirali stvaranje domaćini Docker na Azure VMs putem sučelja naredbeni redak, potražite u članku [upute za korištenje VM proširenje Docker iz Azure sučelja naredbenog retka (Azure EŽA)]

<!--Anchors-->
[Create a new VM from the Image Gallery]: #createvm
[Create Docker Certificates]: #dockercerts
[Add the Docker VM Extension]: #adddockerextension
[Test Docker Client and Azure Docker Host]: #testclientandserver
[Next steps]: #next-steps

<!--Image references-->
[StartingPoint]: ./media/StartingPoint.png
[StartingPoint]: ./media/StartingPoint.png
[StartingPoint]: ./media/StartingPoint.png
[StartingPoint]: ./media/StartingPoint.png
[StartingPoint]: ./media/StartingPoint.png
[StartingPoint]: ./media/StartingPoint.png
[StartingPoint]: ./media/StartingPoint.png
[StartingPoint]: ./media/StartingPoint.png
[6]: ./media/markdown-template-for-new-articles/pretty49.png
[7]: ./media/markdown-template-for-new-articles/channel-9.png


<!--Link references-->
[Kako koristiti VM proširenje Docker iz Azure sučelja naredbenog retka (Azure EŽA)]: http://azure.microsoft.com/documentation/articles/virtual-machines-docker-with-xplat-cli/
[Agent za Azure Linux]: virtual-machines-linux-agent-user-guide.md
[Link 3 to another azure.microsoft.com documentation topic]: ../storage-whatis-account.md

[Pokretanje Docker s https]: http://docs.docker.com/articles/https/
[Docker korisničkom priručniku]: https://docs.docker.com/userguide/
