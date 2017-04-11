<properties
    pageTitle="Premještanje podataka i iz Azure prostora za pohranu | Microsoft Azure"
    description="Ovaj članak sadrži pregled različite načine za premještanje podataka i iz Azure prostora za pohranu."
    services="storage"
    documentationCenter=""
    authors="micurd"
    manager="jahogg"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/21/2016"
    ms.author="micurd"/>

# <a name="moving-data-to-and-from-azure-storage"></a>Premještanje podataka i iz spremišta Azure

Ako želite premjestiti lokalne podatke sa spremištem Azure (ili obratno), postoje na različite načine da biste to učinili. Pristup koji vam najviše odgovara ovisi o scenariju. U ovom članku pronaći ćete Kratak pregled scenarija i odgovarajuće ponude za svaku grupu.

## <a name="building-applications"></a>Sastavni aplikacije

Ako ste stvaranje aplikacije, razvoj protiv REST API-JA ili nešto naš mnogo biblioteka klijentski sjajan je način da biste premjestili sadržaj i iz spremišta Azure.

Azure prostora za pohranu nudi biblioteke obogaćenog klijenta za .NET, iOS, Java, Android, univerzalni platforme Windows (UWP), Xamarin, C++, Node.JS, PHP, Ruby i Python. Biblioteka klijentski nude naprednih mogućnosti kao što su pokušaj logike, zapisivanje i paralelno prijenosa. Možete razviti i izravno u odnosu na REST API-JA, koji možete pozvati bilo koji jezik koji vam se čini zahtjeva za HTTP/HTTPS.

Pročitajte članak [Početak rada s spremište blobova platforme Azure](storage-dotnet-how-to-use-blobs.md) da biste saznali više.

Uz to, također nudimo [Azure prostora za pohranu podataka premještanje biblioteke](https://www.nuget.org/packages/Microsoft.Azure.Storage.DataMovement) koja je biblioteka namijenjen visokih performansi kopiranje podataka i iz Azure. Pogledajte naš biblioteka premještanje podataka [dokumentaciju](https://github.com/Azure/azure-storage-net-data-movement) da biste saznali više. 

## <a name="quickly-viewinginteracting-with-your-data"></a>Brzi pregled/interakcija s podacima

Ako želite jednostavan način za prikaz podataka za pohranu Azure prilikom i imate mogućnost prijenos i preuzimanje podataka, pa preporučujemo da koristite za pohranu Explorer Azure.

Pogledajte naš popis [Azure prostora za pohranu programu Software Explorer](storage-explorers.md) da biste saznali više.

## <a name="system-administration"></a>Administriranje sustava

Ako je potrebno ili više upoznati s naredbenog retka utility (npr. za administratore sustava), Evo nekoliko mogućnosti koje treba uzeti u obzir:

### <a name="azcopy"></a>AzCopy

AzCopy je Windows naredbenog retka utility namijenjen visokih performansi kopiranje podataka i iz Azure prostora za pohranu. Možete i kopirati podatke u račun za pohranu ili između različitih prostora za pohranu računa.

Potražite u članku [prijenos podataka s AzCopy naredbenog retka uslužni](storage-use-azcopy.md) da biste saznali više.

### <a name="azure-powershell"></a>Azure PowerShell

Azure PowerShell je modul koji sadrži cmdleti za upravljanje servisima na Azure. Je utemeljenoj na zadacima naredbenog retka ljuske i skriptnog jezika dizajniran posebno za administraciju sustava.

[Pomoću ljuske Azure s Azure pohranom](storage-powershell-guide-full.md) dodatne informacije potražite u članku.

### <a name="azure-cli"></a>Azure EŽA

Azure EŽA pruža skup Otvori izvor različite platforme naredbi za rad sa servisa Azure. Azure EŽA je dostupna u sustavu Windows, OSX i Linux.

Pogledajte odjeljak [Korištenje EŽA Azure s pohranom Azure](storage-azure-cli.md) da biste saznali više.

## <a name="moving-large-amounts-of-data-with-a-slow-network"></a>Premještanje velikih količina podataka s kada je mreža spora

Jedna od najvećih izazove povezan s premještanjem velikih količina podataka je vrijeme prijenosa. Ako želite da biste podatke iz spremišta Azure bez brige o troškovima mreže ili pisanja koda, uvoz/izvoz Azure je odgovarajući rješenja.

Potražite u članku [Azure uvoz/izvoz](storage-import-export-service.md) da biste saznali više.

## <a name="backing-up-your-data"></a>Sigurnosno kopiranje podataka

Ako morate samo sigurnosne kopije podataka za pohranu Azure, Azure sigurnosne kopije je način da biste se. Ovo je Napredna rješenje za sigurnosno kopiranje lokalnih podataka i Azure VMs.

[Sigurnosno kopiranje Azure](../backup/backup-introduction-to-azure-backup.md) dodatne informacije potražite u članku.

## <a name="accessing-your-data-on-premises-and-from-the-cloud"></a>Pristup podacima lokalnih i iz oblaka

Ako vam je potrebna rješenje za pristup podacima lokalnih i iz oblaka, zatim razmotrite korištenje Azure, hibridnog oblaka za pohranu rješenja, StorSimple. Rješenje se sastoji od fizički uređaj StorSimple da energija trgovine često koristi podatke na SSDs, povremeno koristili podatke o HDDs te Neaktivni/sigurnosne kopije/arhiviranje podataka na Azure prostora za pohranu.

[StorSimple](../storsimple/storsimple-overview.md) dodatne informacije potražite u članku.

## <a name="recovering-your-data"></a>Oporavak podataka

Ako imate lokalni radnih opterećenja i aplikacije, morat ćete rješenja koja omogućuje tvrtki da biste nastavili pokretanje slučaju na Izrada. Oporavak Azure web-mjesta rukuje replikacije, prebacivanje i oporavak virtualnim strojevima i fizičke poslužiteljima. Repliciranu podaci se pohranjuju u Azure prostor za pohranu, omogućujući vam da biste uklonili potrebe za sekundarne on-site podatkovnog centra.

Pročitajte članak [Oporavak Azure web-mjesta](../site-recovery/site-recovery-overview.md) da biste saznali više.
