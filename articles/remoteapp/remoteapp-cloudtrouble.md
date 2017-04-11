
<properties
    pageTitle="Otklanjanje poteškoća s RemoteApp oblaka zbirke – stvaranje | Microsoft Azure"
    description="Saznajte kako otkloniti RemoteApp oblaka zbirke stvaranje poteškoće s"
    services="remoteapp"
    documentationCenter=""
    authors="vkbucha"
    manager="mbaldwin" />

<tags
    ms.service="remoteapp"
    ms.workload="tbd"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/15/2016"
    ms.author="elizapo" />



# <a name="troubleshoot-creating-remoteapp-cloud-collections"></a>Otklanjanje poteškoća prilikom stvaranja zbirke RemoteApp oblaka

> [AZURE.IMPORTANT]
> Ukinute Azure RemoteApp je u tijeku. Čitanje [Objava](https://go.microsoft.com/fwlink/?linkid=821148) detalje.

Ako imate poteškoća s stvaranje zbirku oblaka, pogledajte sljedeće informacije.

## <a name="your-image-is-invalid"></a>Sliku nije valjan ##
Ako se prikaže poruka kao "GoldImageInvalid" kada čekate Azure Dodjela vaše zbirke, to znači da sliku predložak ne zadovoljava [definirani preduvjeti slike](remoteapp-imagereqs.md). Tako, otvorite pročitajte [preduvjete](remoteapp-imagereqs.md), rješavanje sliku i pokušate stvaraju vašu zbirku.

## <a name="common-errors-seen-in-the-azure-management-portal"></a>Uobičajene pogreške vidjeti na portalu za upravljanje Azure

    DNS server could not be reached
    ProvisioningTimeout

Često Cloud zbirke nije uspjelo tijekom stvaranja zbog koje koriste prilagođenu slike.  Ako vam se prikazuje jedna od gore pogrešaka i koristite prilagođenu sliku da biste stvorili zbirke, provjerite sljedeće:

- Provjerite ispunjava li prilagođenu sliku koju ste prenijeli slika preduvjete.
- Najčešće uobičajenih problema je da slika nije ispravno syspreped.  
- Provjerite je li slika možete pokretanje unutar Hyper-V ili pokušajte stvoriti programa IAAS VM izravno u pretplatu Azure pomoću sliku. Ako na VM ne uspije pokrenuti i ne želite pokrenuti, zatim obično znači da prilagođenu sliku nije ispravno pripremljeni.  Provjerite je li prilagođenu sliku gradnje prate kako da biste stvorili prilagođeni predložak slika za RemoteApp

Ako koristite neki dio vaše pretplate na Microsoft slika, pokušajte stvoriti u zbirci. Ako se problem pojavljuje zatim obratite se Microsoftovoj službi za podršku.

    PlatformImageTrialModeOnly

Ako se prikaže Ova pogreška to obično znači da koja nadograđena plaćenu račun, ali se pokušavate koristiti Microsoft dobili sliku koja je valjan samo tijekom probne načinu rada servisa. U ovom slučaju, pokušajte stvaraju vašu zbirku oblaka ponovno, ali ne zaboravite da biste odredili točan slike.
