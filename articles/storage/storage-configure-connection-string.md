<properties 
    pageTitle="Konfiguriranje niza za povezivanje sa spremištem Azure | Microsoft Azure"
    description="Konfiguriranje niza za povezivanje s poslovnim subjektom Azure prostora za pohranu. Niz za povezivanje sadrži podatke koji su potrebni za provjeru autentičnosti programa access s računom za pohranu iz aplikacije tijekom rada."
    services="storage"
    documentationCenter=""
    authors="tamram"
    manager="carmonm"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/18/2016"
    ms.author="tamram"/>

# <a name="configure-azure-storage-connection-strings"></a>Konfiguriranje nizove veze Azure prostora za pohranu

## <a name="overview"></a>Pregled

Niz za povezivanje sadrži podatke za provjeru autentičnosti potrebne za pristup podacima u račun za Azure prostora za pohranu iz aplikacije tijekom rada. Možete konfigurirati niza za povezivanje za:

- Povezivanje s emulator Azure prostora za pohranu.
- Pristup računu za pohranu u Azure.
- Pristup navedenih resursa u Azure putem zajednički pristup potpis (SAS).

[AZURE.INCLUDE [storage-account-key-note-include](../../includes/storage-account-key-note-include.md)]

## <a name="storing-your-connection-string"></a>Spremanje niz za povezivanje

Aplikacija ćete pristupati niz za povezivanje prilikom izvođenja da bi se autentičnost zahtjeva za pokušaj Azure prostora za pohranu. Imate nekoliko različitih mogućnosti za spremanje niz za povezivanje:

- Za za aplikaciju je pokrenut na radnoj površini ili na uređaju, možete spremiti niz za povezivanje u programa `app.config `datoteke ili `web.config` datoteku. Dodavanje niz za povezivanje u odjeljku **AppSettings** .
- Programa koji se izvodi u Azure oblaku, možete spremiti niz za povezivanje u [datoteke shema (.cscfg) konfiguracije Azure servisa](https://msdn.microsoft.com/library/ee758710.aspx). Dodajte niz veze do odjeljka **ConfigurationSettings** konfiguracijska datoteka servisa.
- Niz za povezivanje možete koristiti i izravno u kodu. Za većinu scenarije, no preporučujemo pohranu niz za konfiguraciju u konfiguracijskoj datoteci.

Spremanje niz za povezivanje u datoteci konfiguracije olakšava da biste ažurirali niz za povezivanje da biste se prebacivali između emulator prostora za pohranu i račun za Azure prostora za pohranu u oblaku. Što je potrebno da biste uredili niz za povezivanje na vaše okruženje cilj.

[Upravitelj konfiguracije za Microsoft Azure](https://www.nuget.org/packages/Microsoft.WindowsAzure.ConfigurationManager/) predmete možete koristiti za pristup niz za povezivanje prilikom izvođenja bez obzira na kojem se pokreće program.

## <a name="create-a-connection-string-to-the-storage-emulator"></a>Stvaranje niza za povezivanje za emulator prostora za pohranu

[AZURE.INCLUDE [storage-emulator-connection-string-include](../../includes/storage-emulator-connection-string-include.md)]

Dodatne informacije o emulator prostora za pohranu potražite u članku [korištenje prostora za pohranu Emulator Azure za razvoj i testiranje](storage-use-emulator.md) .

## <a name="create-a-connection-string-to-an-azure-storage-account"></a>Stvaranje niza za povezivanje s poslovnim subjektom Azure prostora za pohranu

Za stvaranje niza za povezivanje s računom Azure prostora za pohranu, koristite ispod oblik niza veze. Označava hoće li želite povezati s računom za pohranu putem HTTP (preporučeno) ili HTTP, a zamijeniti `myAccountName` s nazivom vašeg računa za pohranu i zamijeni `myAccountKey` pomoću svojeg ključa za pristup računu:

    DefaultEndpointsProtocol=[http|https];AccountName=myAccountName;AccountKey=myAccountKey

Na primjer, niz za povezivanje izgledat će slično kao sljedeće ogledne niz za povezivanje:

    DefaultEndpointsProtocol=https;AccountName=storagesample;AccountKey=<account-key>

> [AZURE.NOTE] Azure prostora za pohranu podržava HTTP i HTTPS niza za povezivanje; Međutim, pomoću HTTPS preporučujemo.

## <a name="create-a-connection-string-using-a-shared-access-signature"></a>Stvaranje niza za povezivanje s potpisom zajednički pristup

[AZURE.INCLUDE [storage-use-sas-in-connection-string-include](../../includes/storage-use-sas-in-connection-string-include.md)]

## <a name="creating-a-connection-string-to-an-explicit-storage-endpoint"></a>Stvaranje niza za povezivanje za krajnje eksplicitnih prostora za pohranu

U nizu za povezivanje umjesto korištenja krajnje točke zadani možete odrediti izričito krajnje točke servisa. Da biste stvorili niz koji određuje krajnje eksplicitnih, navedite krajnja točka za dovršavanje servisa za svaki servis, uključujući specifikacija protokola (HTTPS (preporučeno) ili HTTP), u sljedećem obliku:

    DefaultEndpointsProtocol=[http|https];
    BlobEndpoint=myBlobEndpoint;
    QueueEndpoint=myQueueEndpoint;
    TableEndpoint=myTableEndpoint;
    FileEndpoint=myFileEndpoint;
    AccountName=myAccountName;
    AccountKey=myAccountKey

Jedan scenarij možda, gdje želite navesti krajnje izričite je ako ste preslikali vaše krajnje točke spremišta blobova platforme za prilagođenu domenu. U tom slučaju možete navesti prilagođene krajnja točka za spremište blobova platforme u nizu za povezivanje i po želji navedite krajnje točke zadano za tog servisa ako ih se koristi aplikacija.

Evo primjera valjanu vezu nizovi koji odredite eksplicitnih krajnja točka za servis Blob:

    # Blob endpoint only
    DefaultEndpointsProtocol=https;
    BlobEndpoint=www.mydomain.com;
    AccountName=storagesample;
    AccountKey=account-key

    # All service endpoints
    DefaultEndpointsProtocol=https;
    BlobEndpoint=www.mydomain.com;
    FileEndpoint=myaccount.file.core.windows.net;
    QueueEndpoint=myaccount.queue.core.windows.net;
    TableEndpoint=myaccount;
    AccountName=storagesample;
    AccountKey=account-key

Vrijednost krajnju točku koja je navedena u nizu za povezivanje koristi se za sastavljanje zahtjev ji sa servisom Blob i ona određuje obrasca sve ji vraćenih kod.

Imajte na umu da ako odaberete izostaviti krajnje točke usluge iz niza za povezivanje, zatim koje nećete moći koristiti taj niz za povezivanje za pristup podacima u tu uslugu iz koda.

### <a name="creating-a-connection-string-with-an-endpoint-suffix"></a>Stvaranje niza za povezivanje s programa nastavka krajnje točke

Da biste stvorili niza za povezivanje za servis za pohranu u područja ili instance nastavke različite krajnje točke, kao što su za Azure Kini ili Azure upravljanja, koristite sljedeće format niza za povezivanje. Odredite želite li povezivanje s računom za pohranu putem HTTP ili HTTPS, zamijenite `myAccountName` pod nazivom vašeg računa za pohranu, zamijenite `myAccountKey` s tipka za pristup računu i zamijeni `mySuffix` s kraticom URI:


    DefaultEndpointsProtocol=[http|https];
    AccountName=myAccountName;
    AccountKey=myAccountKey;
    EndpointSuffix=mySuffix;


Na primjer, niz za povezivanje bi trebala izgledati ovako na sljedeći niz za povezivanje:

    DefaultEndpointsProtocol=https;
    AccountName=storagesample;
    AccountKey=<account-key>;
    EndpointSuffix=core.chinacloudapi.cn;

## <a name="parsing-a-connection-string"></a>Raščlanjivanje niza za povezivanje

[AZURE.INCLUDE [storage-cloud-configuration-manager-include](../../includes/storage-cloud-configuration-manager-include.md)]


## <a name="next-steps"></a>Daljnji koraci

- [Korištenje Emulator Azure prostora za pohranu za razvoj i testiranje](storage-use-emulator.md)
- [Programu Software Explorer Azure prostora za pohranu](storage-explorers.md)
- [Korištenje potpisa zajednički pristup (SAS)](storage-dotnet-shared-access-signature-part-1.md)
