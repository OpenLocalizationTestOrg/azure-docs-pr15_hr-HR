<properties
    pageTitle="Stvaranje Azure RemoteApp slike na temelju programa Azure VM | Microsoft Azure"
    description="Saznajte kako stvoriti sliku za Azure RemoteApp počevši od Azure virtualnog računala."
    services="remoteapp"
    documentationCenter=""
    authors="lizap"
    manager="mbaldwin" />

<tags
    ms.service="remoteapp"
    ms.workload="compute"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/15/2016" 
    ms.author="elizapo" />



# <a name="create-a-azure-remoteapp-image-based-on-an-azure-virtual-machine"></a>Stvaranje Azure RemoteApp slike na temelju Azure virtualnog računala

> [AZURE.IMPORTANT]
> Ukinute Azure RemoteApp je u tijeku. Čitanje [Objava](https://go.microsoft.com/fwlink/?linkid=821148) detalje.

Azure RemoteApp slike (koji sadrže aplikacije šaljete u sklopu zbirke) možete stvoriti iz Azure virtualnog računala. Nije moguće odabrati i da biste koristili sliku virtualnog računala dodali smo u galeriju Azure VM sliku koja zadovoljava sve preduvjete slika Azure RemoteApp – možete koristiti tu VM sliku kao početnu točku za vlastitu VM po želji. Potražite "Windows Server udaljene radne površine glavnog" slike u biblioteci.

Postoje dva koraka za stvaranje vlastite slike na temelju programa Azure VM – stvaranje slike i prenesite ga iz biblioteke Azure VM za Azure RemoteApp.

## <a name="create-a-custom-image-based-on-an-azure-vm"></a>Stvoriti prilagođenu sliku na temelju VM programa Azure

Da biste stvorili sliku programa Azure VM na temelju poduzmite sljedeće korake.

1. Stvaranje Azure virtualnog računala. Možete koristiti "Windows Server udaljene radne površine voditelju sesije" ili "Windows Server udaljene radne površine sesiju glavno računalo s Microsoft Office 365 ProPlus" slike iz galerije slika Azure virtualnog računala. Na ovoj se slici ispunjava li sve preduvjete slika predloška Azure RemoteApp.

    Detalje potražite u članku [Stvaranje na VM sa sustavom Windows](../virtual-machines/virtual-machines-windows-hero-tutorial.md).

2. Povezivanje s VM i instalirajte i konfigurirajte aplikacije koje želite zajednički koristiti putem RemoteApp. Provjerite je li za izvođenje sve dodatne Windows konfiguracije potrebnih aplikacija.

    Detalje potražite u članku [kako se prijaviti u virtualnog računala izvodi Windows Server](../virtual-machines/virtual-machines-windows-classic-connect-logon.md).

3. Ako koristite neku od slika Windows Server udaljene radne površine glavnog, postoji skripte uključene provjere valjanosti koji će osigurati vaše VM zadovoljava RemoteApp pre-reqs. Da biste pokrenuli skriptu, dvokliknite **ValidateRemoteAppImage** na radnoj površini. Provjerite je li sve pogreške dojavi skriptu rješava prije prelaska na sljedeći korak.

4. SYSPREP generalize i snimiti. Upute potražite u članku [kako snimiti virtualnog računala za Windows koja će služiti kao predložak](../virtual-machines/virtual-machines-windows-classic-capture-image.md) .



## <a name="import-the-image-into-the-azure-remoteapp-image-library"></a>Uvoz slika u biblioteku slika Azure RemoteApp

Da biste uvezli novu sliku u Azure RemoteApp, poduzmite sljedeće korake:

1. Na kartici **Slika predložaka** :
    - Ako imate bez postojeće slike, kliknite **Prenesi ili uvoz predloška slike**.
    - Ako već imate barem jednu sliku, kliknite **+** da biste dodali novu sliku.

2. Odaberite **Uvoz sliku s virtualnim strojevima** biblioteka, a zatim kliknite **Dalje**.

3. Na sljedećoj stranici odaberite prilagođenu sliku s popisa, a zatim potvrdite da ste pratili korake navedene stvaranja sliku. Kliknite **Dalje**.
4. Unesite naziv za novu sliku RemoteApp i odaberite mjesto, a zatim kliknite kvačicu da biste pokrenuli postupak uvoza.

> [AZURE.NOTE] Možete uvesti slike s bilo kojeg mjesta za Azure podržane virtualnim računalima sustava po Azure na bilo koje mjesto za Azure podržava Azure RemoteApp. Ovisno o mjesta uvoz može proći do 25 minuta.

Sada ste spremni za ili stvoriti novu zbirku [oblaka](remoteapp-create-cloud-deployment.md) zbirke ili [hibridnog](remoteapp-create-hybrid-deployment.md), ovisno o vašim potrebama.
