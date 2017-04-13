<properties 
  pageTitle="Implementacija vlastite registra Docker privatno na Azure | Microsoft Azure"
  description="U članku se opisuje kako možete koristiti registar Docker za hostiranje spremnik slike na usluzi spremište blobova platforme Azure."
  services="virtual-machines-linux"
  documentationCenter="virtual-machines"
  authors="ahmetalpbalkan"
  editor="squillace"
  manager="timlt"
  tags="azure-service-management,azure-resource-manager" />

<tags
  ms.service="virtual-machines-linux"
  ms.devlang="multiple"
  ms.topic="article"
  ms.tgt_pltfrm="vm-linux"
  ms.workload="infrastructure-services"
  ms.date="09/27/2016" 
  ms.author="ahmetb" />

# <a name="deploying-your-own-private-docker-registry-on-azure"></a>Implementacija vlastite registra Docker privatno na Azure

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-both-include.md)]



U ovom dokumentu opisuje koje je Docker privatne registra je i prikazuje kako možete implementirati spremnika sliku Docker registra 2.0 Docker privatne registar na Microsoft Azure pomoću spremište blobova platforme Azure.

Ovaj dokument pretpostavlja:

1. Saznajte kako koristiti Docker i imaju Docker slike za pohranu. (Ne? [Dodatne informacije o Docker](https://www.docker.com))
2. Imate poslužitelju na kojem je modul Docker instaliran. (Ne? [Brzo obaviti na Azure.](https://azure.microsoft.com/documentation/templates/docker-simple-on-ubuntu/))


## <a name="what-is-a-private-docker-registry"></a>Što je privatna registra Docker?

Da bi se isporučuju containerized aplikacija u oblak, slaganje spremnik sliku Docker i spremite ga negdje tako da ga možete koristiti po sebi i drugim korisnicima. 

Dok je jednostavno stvaranje spremnika slike i dostavu u oblak je test za pouzdano spremanje generirani slike. Zbog toga Docker nudi središnje servisa naziva [Koncentrator Docker] [ docker-hub] za pohranu kontejner slike u oblak i omogućuje stvaranje spremnika u bilo kojem trenutku pomoću tim slikama.

Iako [Koncentrator Docker] [ docker-hub] plaćenu servis za pohranu na spremnik privatne aplikacije slike Docker običnoj razvojnim inženjerima potrebama i nudi Otvori izvor skup alata za spremanje slike u vlastiti privatne registra Docker iza vatrozida ili lokalni bez odlazak javnog Interneta.
Budući da je spremište blobova platforme Azure olakšava zaštitu, stvaranje i korištenje privatne registra Docker u Azure koja sami određuju brzo možete koristiti ga.

## <a name="why-should-you-host-a-docker-registry-on-azure"></a>Zašto koji hostira Docker registra na Azure?

Hostiranje na instancu Docker registra na Microsoft Azure i spremanje slike u spremište blobova platforme Azure, imate nekoliko prednosti:

**Sigurnost:** Slika Docker ostavite Azure podatkovnim centrima tako da ih ne Unakrsna javnog Interneta kao i ako ste koristili Docker koncentratora.
  
**Performanse:** Slika Docker spremaju se unutar istog podatkovnog centra ili regije kao aplikacija. To znači da slike će se povlače brže i više pouzdano usporedbi Docker koncentrator.

**Pouzdanosti:** Pomoću blobova platforme Microsoft Azure olakšavaju korištenje mnogo prostora za pohranu svojstva kao što su visoke dostupnosti, zalihosti, premium prostora za pohranu (SSDs) i tako dalje.

## <a name="configuring-docker-registry-to-use-azure-blob-storage"></a>Konfiguriranje Docker registar da biste koristili spremište blobova platforme Azure

(Preporučuje se pročitajte [dokumentaciju Docker registra 2.0][registra dokumente] prije nastavka.)

Možete [konfigurirati] [ registry-config] registra Docker na dva načina.
možeš:

1. Korištenje programa `config.yml` datoteku. U ovom slučaju morate stvoriti zasebnu slikovnu Docker iznad `registry` slike.
2. Zanemarivanje zadana datoteka konfiguracije kroz varijable okruženja: će to učiniti bez stvaranja i održavanja zasebnu slikovnu Docker.

Radi jednostavnosti, u ovoj se temi slijedi mogućnost 2, pomoću varijabli okruženja.

Da biste pokrenuli Docker registra instancu koju:

* Koristi račun za Azure prostora za pohranu za pohranu slika
* očekuje podatke virtualnog računala priključak 5000
* nije konfiguriran provjere autentičnosti (ne preporučuje se, pogledajte napomenu u nastavku)

morate pokrenite sljedeću naredbu Docker u vašem terminal tulumu (zamjena `<storage-account>` i `<storage-key>` s vjerodajnice):

```sh
$ docker run -d -p 5000:5000 \
     -e REGISTRY_STORAGE=azure \
     -e REGISTRY_STORAGE_AZURE_ACCOUNTNAME="<storage-account>" \
     -e REGISTRY_STORAGE_AZURE_ACCOUNTKEY="<storage-key>" \
     -e REGISTRY_STORAGE_AZURE_CONTAINER="registry" \
     --name=registry \
     registry:2
```

Kada naredbe Zatvori, možete vidjeti spremnik hostiranje vaše privatne instancu registra Docker tako da pokrenete u `docker ps` naredbe na vašem računalu koje hostira:

```sh
$ docker ps
CONTAINER ID        IMAGE               COMMAND                CREATED             STATUS              PORTS                    NAMES
3698ddfebc6f        registry:2          "registry cmd/regist   2 seconds ago       Up 1 seconds        0.0.0.0:5000->5000/tcp   registry
```

> [AZURE.IMPORTANT] Konfiguriranje sigurnosnih za registra Docker nije obuhvaćeno ovaj dokument, a registra bit će dostupan svakome bez provjere autentičnosti po zadanom ako otvorite port (priključak) u registru priključak na krajnjoj točki virtualnog računala ili učitati opterećenja ako upotrijebite naredbu za implementaciju iznad.
>
> Pročitajte [Konfiguriranje registra Docker] [ registry-config] dokumentaciju da biste saznali kako instancu registra i slike.

## <a name="next-steps"></a>Daljnji koraci

Nakon što dodate registra postavljanje, vrijeme je da biste se neki više ga koristiti. Započnite s docker [registra dokumente]. 

[docker-hub]: https://hub.docker.com/
[registry]: https://github.com/docker/distribution
[registra dokumenti]: http://docs.docker.com/registry/
[registry-config]: http://docs.docker.com/registry/configuration/
 
