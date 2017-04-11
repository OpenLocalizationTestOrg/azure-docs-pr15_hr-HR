<properties
    pageTitle=" Stvorite račun servisa Azure Media Services s portala za Azure | Microsoft Azure"
    description="Pomoću ovog praktičnog vodiča vodit će vas kroz korake stvorite račun servisa Azure Media Services s portala za Azure."
    services="media-services"
    documentationCenter=""
    authors="Juliako"
    manager="erikre"
    editor=""/>

<tags
    ms.service="media-services"
    ms.workload="media"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="10/24/2016"
    ms.author="juliako"/>


# <a name="create-an-azure-media-services-account-using-the-azure-portal"></a>Stvorite račun servisa Azure Media Services pomoću portala za Azure

> [AZURE.SELECTOR]
- [Portal](media-services-portal-create-account.md)
- [PowerShell](media-services-manage-with-powershell.md)
- [OSTALE](http://msdn.microsoft.com/library/azure/dn194267.aspx)

> [AZURE.NOTE] Da biste dovršili ovaj Praktični vodič, potreban vam je račun za Azure. Detalje potražite u članku [Azure besplatnu probnu verziju](https://azure.microsoft.com/pricing/free-trial/). 

Portal za Azure omogućuje da biste brzo stvorili račun servisa Azure Media Services (AMS). Koristite svoj račun za pristup koja omogućuje pohranu šifriranje, kodiranje, upravljanje i strujanja medijskih sadržaja u Azure Media Services. Prilikom stvaranja računa Media Services koje i stvorite račun povezan prostora za pohranu (ili koristite postojeću) u istom regiji kao račun Media Services.

U ovom se članku objašnjava neke uobičajene koncepata i pokazuje kako stvoriti račun Media Services s portala za Azure.

## <a name="concepts"></a>Koncepti

Pristup Media Services potrebna dva povezana računa:

- Na račun servisa Media Services. Vaš račun omogućuje vam pristup skup oblaku Media Services koje su dostupne u Azure. Računa za servise za medijske sadržaje pohranite stvarni medijskog sadržaja. Umjesto toga pohranjuje metapodataka o medijske sadržaje i medijske sadržaje obrada zadataka na vašem računu. Prilikom stvaranja računa odaberite dostupne područja za Media Services. Područje odaberete je podatkovnog centra u kojoj su pohranjeni zapisa metapodataka za vaš račun.

    Dostupne područja Media Services (AMS) obuhvaćaju sljedeće: Sjeverna Europa, Zapad Europe, Zapad SAD-a, Istočni SAD-a, Jugoistočne Azije, istočnoazijski, Japan Zapad, Istok Japan. Media Services pomoću afiniteta grupe.
    
    AMS dostupna sada i u centre za sljedeće podatke: Brazil Jug, Indija zapada, Južna Indija i Indija središnje. Portal za Azure sada možete koristiti za stvaranje računa servisa za medijske sadržaje i izvođenje različitih zadataka koje se ovdje opisuju. Međutim, Live kodiranje nije omogućena u tih podataka. Dodatno, sve vrste kodiranja rezervirane jedinice dostupne su u tih podataka.
    
    - Brazil Jug: Standardno i osnovni kodiranje rezervirane jedinice su dostupne samo.
    - Indija Zapad, Južna Indija: 

- Račun za Azure prostora za pohranu. Računi za pohranu moraju nalaziti u istoj regiji kao račun Media Services. Prilikom stvaranja računa za servise za medijske sadržaje, možete odabrati postojeći račun za pohranu u istom području ili stvorite novi račun za pohranu u istom području. Ako izbrišete računa za servise za medijske sadržaje, blob polja na vašem računu povezane prostora za pohranu ne brišu se.

## <a name="create-an-ams-account"></a>Stvorite račun AMS

Koraci u ovom odjeljku pokazati kako stvoriti račun AMS.

1. Prijavite se na [portal za Azure](https://portal.azure.com/).
2. Kliknite **+ Novo** > **Web + Mobile** > **Media Services**.

    ![Stvaranje Media Services](./media/media-services-portal-vod-get-started/media-services-new1.png)

3. U **Stvaranje RAČUNA za SERVISE MEDIA** unesite tražene vrijednosti.

    ![Stvaranje Media Services](./media/media-services-portal-vod-get-started/media-services-new3.png)
    
    1. U **Naziv računa**, unesite naziv novog računa AMS. Naziv računa za Media Services sve malo brojeva ili slova bez razmaka, a 3 i 24 znakova.
    2. U pretplatu, odaberite između različitih Azure pretplate koje imate pristup.
    
    2. U **Grupi resursa**, odaberite novi ili postojeći resurs.  Grupa resursa je skup resursa koji životni ciklus, dozvolama i pravilnicima za zajedničko korištenje. Dodatne informacije [u nastavku](azure-resource-manager/resource-group-overview.md#resource-groups).
    3. **Mjesto**, odaberite regiji koja će se koristiti za pohranu zapisa mediji i metapodataka za vaš račun Media Services. Tom području će se koristiti za obradu i strujanje medijskih sadržaja. Samo dostupne Media Services područja pojavljuju se u okviru padajućeg popisa. 
    
    3. U **Račun za pohranu**, odaberite račun za pohranu možete unijeti blobova medijskog sadržaja s računa Media Services. Možete odabrati postojeći račun za pohranu u istom regiji kao račun Media Services ili možete stvoriti račun za pohranu. U istom području stvaranja novog računa za pohranu. Pravila za nazive računa za pohranu isti su kao računi Media Services.

        Dodatne informacije o prostora za pohranu [u nastavku](storage-introduction.md).

    4. Odaberite **Prikvači na nadzornu ploču** da biste vidjeli tijeku implementacije računa.
    
7. Kliknite **Stvori** pri dnu obrasca.

    Kada je račun uspješno stvorili, status mijenja se u **pokrenut**. 

    ![Postavke Media Services](./media/media-services-portal-vod-get-started/media-services-settings.png)

    Da biste upravljali računa AMS (na primjer, prijenos videozapisa, kodiranje resursi, praćenje napretka posla) pomoću prozora **Postavke** .

## <a name="manage-keys"></a>Upravljanje tipke

Potreban je naziv računa i informacija primarnog ključa za programatski pristup računu Media Services.

1. Na portalu Azure odaberite svoj račun. 

    Pojavit će se prozor **Postavke** na desnoj strani. 

2. U prozoru **Postavke** odaberite **tipke**. 

    **Upravljanje ključevima** windows prikazuje naziv računa i prikazuju se primarnih i sekundarnih ključeva. 
3. Pritisnite gumb Kopiraj da biste kopirali vrijednosti.
    
    ![Media Services tipke](./media/media-services-portal-vod-get-started/media-services-keys.png)

## <a name="next-steps"></a>Daljnji koraci

Sada možete prenijeti datoteke na račun AMS. Dodatne informacije potražite u članku [Prijenos datoteke](media-services-portal-upload-files.md).

## <a name="media-services-learning-paths"></a>Tečajevi za Media Services

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Slanje povratnih informacija

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]


