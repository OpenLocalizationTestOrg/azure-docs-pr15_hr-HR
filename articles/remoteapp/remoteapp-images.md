<properties
    pageTitle="Što je slika predložaka Azure RemoteApp? | Microsoft Azure"
    description="Saznajte više o slika predložaka u sklopu Azure RemoteApp."
    services="remoteapp"
    documentationCenter=""
    authors="lizap"
    manager="mbaldwin" />

<tags
    ms.service="remoteapp"
    ms.workload="compute"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="08/15/2016"
    ms.author="elizapo" />

# <a name="what-is-in-the-azure-remoteapp-template-images"></a>Što je slika predložaka Azure RemoteApp?

> [AZURE.IMPORTANT]
> Ukinute Azure RemoteApp je u tijeku. Čitanje [Objava](https://go.microsoft.com/fwlink/?linkid=821148) detalje.

Vaša pretplata Azure RemoteApp obuhvaća tri slika predložaka:


- Windows Server 2012.
- Microsoft Office 365 ProPlus (pretplatu na Office 365 obavezno)
- Microsoft Office 2013 Professional Plus (samo za probnu verziju)

> [AZURE.IMPORTANT]Vaša pretplata Azure RemoteApp daje pristup softver na slikama, osim sustava Office 365 ProPlus, koji su potrebni posebnu pretplatu i Office 2013 ne može se koristiti u radni. To znači da možete zajednički koristiti programe ili aplikacija na slika predložaka s korisnicima. Na primjer, ako stvorite zbirka koja koristi Windows Server 2012 R2 sliku, možete objaviti Zaštita na krajnjoj točki centar sustava za korisnike koji žele pristupiti putem RemoteApp.
>
> Pogledajte [detalje za licenciranje RemoteApp](remoteapp-licensing.md) dodatne informacije. I informacije o licenci za [Korištenje sustava Office s Azure RemoteApp](remoteapp-o365.md) za Office.

Nastavite čitati pojedinosti o što sadrži svaku sliku.

## <a name="windows-server-2012-r2--the-vanilla-image"></a>Windows Server 2012 R2 ("vanilla slike")
Na ovoj se slici se temelji na sustavu Microsoft Windows Server 2012 R2 Standard operacijski sustav, a sadrži sljedeće uloge i značajke instalirane na zadovoljava preduvjete za Azure RemoteApp slika predložaka:


- .NET framework 4,5, 3.5.1, 3.5
- Doživljaj radne površine
- Rukopis i servise rukopisa
- Temelji medijskih sadržaja
- Glavno računalo sesiji udaljene radne površine
- Komponente Windows PowerShell 4.0
- Windows Očisti filtar
- Podrška za WoW64

Ova slika je i instalirane sljedeće aplikacije:

- Adobe Flash Player
- Microsoft Silverlight
- Zaštita na krajnjoj točki Microsoft sustava centra 2012.
- Microsoft Windows Media Player


## <a name="microsoft-office-365-proplus-subscription-required"></a>Microsoft Office 365 ProPlus (potrebna je pretplata)
Office 365 je aplikacija za većinu tražene, pa ćemo stvoriti "Prilagođeno" slike za rad s.

Ova slika je datotečni nastavak vanilla sliku, a sadrži sljedeće komponente sustava Microsoft Office 365 ProPlus instaliran osim komponente opisane slike u sustavu Windows Server 2012 R2:


- Pristup
- Excel
- Lync
- OneNote
- OneDrive za tvrtke (Imajte na umu agent za sinkronizaciju nije podržana za korištenje s Azure RemoteApp)
- Outlook
- PowerPoint
- Word
- Alati za jezičnu provjeru za Microsoft Office

Slika sadrži i Visio Pro i Project Pro.

I sljedeće aplikacije, kao i:

- SQL nativni klijent
- ODBC upravljački program
- SQL Server dubinsko klijenta
- MasterDataServices klijenta
- Microsoft Publisher
- PowerQuery
- PowerMap


Omogućite sve funkcije sustava Office 365 ProPlus aplikacija dostupna je samo za korisnike koji imaju tarifu za Office 365 ProPlus. Dodatne informacije o sustavu Office 365 pretplatničkim tarifama potražite u članku [servis tarife za Office 365](http://technet.microsoft.com/library/office-365-plan-options.aspx). Niste pronašli odgovor? Pogledajte informacije o [sustavu Office 365 + RemoteApp](remoteapp-o365.md) . Pogledajte novi članak [kako koristiti pretplatu na Office 365 s Azure RemoteApp](remoteapp-officesubscription.md).

Imajte na umu da ćete morati zasebno - licencu sustava Office 365 ProPlus, Visio Pro i Project Pro svaki imaju vlastite licence.

## <a name="microsoft-office-2013-professional-plus-trial-only"></a>Microsoft Office 2013 Professional Plus (samo za probnu verziju)
Tijekom besplatnog probnog razdoblja, možete testirati i servisa sustava Office 2013 sliku.

Ova slika je datotečni nastavak vanilla sliku, a sadrži sljedeće komponente sustava Microsoft Office 2013 Professional Plus instaliran osim komponente opisane slike u sustavu Windows Server 2012 R2:


- Pristup
- Excel
- Lync
- OneNote
- OneDrive za tvrtke (Imajte na umu agent za sinkronizaciju nije podržana za korištenje s Azure RemoteApp)
- Outlook
- PowerPoint
- Projekt
- Visio
- Word
- Alati za jezičnu provjeru za Microsoft Office

> [AZURE.IMPORTANT]**Pravne informacije:** Na ovoj se slici ne obuhvaća licencu za Microsoft Office i *ne može se koristiti za proizvodnju*. Slika namijenjen samo Probno korištenje sustava Office 2013 Professional Plus. Ako želite koristiti aplikacije sustava Office u Azure RemoteApp za proizvodnju trebate koristiti Office 365 ProPlus slike. Dodatne informacije o licenciranju sustava Office potražite u članku [korištenje sustava Office 365 s Azure RemoteApp](remoteapp-o365.md)
