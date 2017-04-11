<properties
   pageTitle="Registriranje podataka iz spremišta Lake podataka u katalog podataka Azure | Microsoft Azure"
   description="Registriranje podataka iz spremišta Lake podataka u katalog podataka Azure"
   services="data-lake-store,data-catalog" 
   documentationCenter=""
   authors="nitinme"
   manager="jhubbard"
   editor="cgronlun"/>

<tags
   ms.service="data-lake-store"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="10/28/2016"
   ms.author="nitinme"/>

# <a name="register-data-from-data-lake-store-in-azure-data-catalog"></a>Registriranje podataka iz spremišta Lake podataka u katalog podataka Azure

U ovom članku će Saznajte kako integrirati Azure podataka Lake trgovine Azure katalog podataka radi jednostavnijeg pronalaženja unutar tvrtke ili ustanove podataka putem integracije s kataloga podataka. Dodatne informacije o Katalogiziranje podataka potražite u članku [Azure kataloga podataka](../data-catalog/data-catalog-what-is-data-catalog.md). Da biste razumjeli scenariji u kojima možete koristiti katalog podataka potražite u članku [uobičajeni scenariji Azure kataloga podataka](../data-catalog/data-catalog-common-scenarios.md).

## <a name="prerequisites"></a>Preduvjeti

Prije početka ovog praktičnog vodiča, morate imati sljedeće:

- **Mogući Azure pretplate**. Pogledajte [Početak Azure besplatnu probnu verziju](https://azure.microsoft.com/pricing/free-trial/).

- **Omogućivanje pretplate Azure** javno pretpregled spremište Lake podataka. Pročitajte [upute](data-lake-store-get-started-portal.md#signup).

- **Račun za azure podataka Lake trgovine**. Slijedite upute na [Početak rada s spremišta Lake podataka za Azure pomoću portala za Azure](data-lake-store-get-started-portal.md). Za ovaj vodič Javite nam stvaranje računa spremišta podataka Lake pod nazivom **datacatalogstore**. 

    Kada stvorite račun, prijenos uzorka skupa podataka. Za ovaj vodič Javite nam prenesite sve .csv datoteke u mapi **AmbulanceData** u [Azure podataka Lake brojka spremište](https://github.com/Azure/usql/tree/master/Examples/Samples/Data/AmbulanceData/). Možete koristiti različite klijenata, kao što su [Azure prostora za pohranu Explorer](http://storageexplorer.com/)za prijenos podataka u spremniku blob.

- **Azure podataka kataloga**. Vaša tvrtka ili ustanova već mora imati katalog podataka Azure stvara za tvrtku ili ustanovu. Samo jedan kataloga dopuštena za svaku tvrtku ili ustanovu.

## <a name="register-data-lake-store-as-a-source-for-data-catalog"></a>Spremište Lake REGISTER podataka kao izvor za katalog podataka

>[AZURE.VIDEO adcwithadl] 

1. Idite na `https://azure.microsoft.com/services/data-catalog`, a zatim kliknite **Početak rada**.

2. Prijavite se na portal Azure katalog podataka pa kliknite **Objavi podataka**.

    ![Izvor podataka morate registrirati] (./media/data-lake-store-with-data-catalog/register-data-source.png "Izvor podataka morate registrirati")

3. Na sljedećoj stranici kliknite **Pokreni aplikaciju**. To će se preuzeti aplikaciju manifesta datoteku na računalu. Dvokliknite manifesta datoteku da biste pokrenuli aplikaciju.

4. Na stranici dobrodošlice kliknite **Prijava**, a zatim unesite vjerodajnice.

    ![Zaslon dobrodošlice] (./media/data-lake-store-with-data-catalog/welcome.screen.png "Zaslon dobrodošlice")

5. Na stranici Odabir izvora podataka, odaberite **Lake Azure podataka**, a zatim kliknite **Dalje**.

    ![Odaberite izvor podataka] (./media/data-lake-store-with-data-catalog/select-source.png "Odaberite izvor podataka")

6. Na sljedećoj stranici, navedite naziv računa spremišta Lake podataka koje želite registrirati u katalog podataka. Ostavite ostale mogućnosti kao zadani, a zatim kliknite **Poveži**.

    ![Povezivanje s izvorom podataka] (./media/data-lake-store-with-data-catalog/connect-to-source.png "Povezivanje s izvorom podataka")

7. Na sljedećoj stranici može se podijeliti u sljedeće segmenata.

    na. Okvir **Hijerarhije poslužitelja** predstavlja struktura mapa za pohranu podataka Lake računa. **$Root** predstavlja korijenski računa spremišta Lake podataka, a **AmbulanceData** predstavlja mapu stvorene u korijenskom direktoriju računa spremišta Lake podataka.

    b. Okvir **dostupne objekte** popisuje datoteke i mape u mapi **AmbulanceData** .

    c. **Objekti koje treba registrirani okvira** navedene datoteke i mape koje želite registrirati u katalog podataka Azure.

    ![Prikaz struktura podataka] (./media/data-lake-store-with-data-catalog/view-data-structure.png "Prikaz struktura podataka")

8. Za ovaj vodič mora registrirati sve datoteke u direktoriju. Za to, kliknite gumb (![Premještanje objekata](./media/data-lake-store-with-data-catalog/move-objects.png "Premještanje objekata")) da biste premjestili sve datoteke s **objektima Registriranje** . 

    Podaci će biti registriran u katalog podataka na razini tvrtke ili ustanove, pa je recommened pristup da biste dodali neke metapodatke koji kasnije možete koristiti da biste brzo pronašli podatke. Na primjer, dodajte adresu e-pošte za vlasnik podataka (na primjer, jedan koji se prenosi podatke) ili dodati oznaku da biste odredili podatke. Snimke zaslona ispod prikazuje oznake koje ćemo dodati podatke.

    ![Prikaz strukture podataka] (./media/data-lake-store-with-data-catalog/view-selected-data-structure.png "Prikaz strukture podataka")

    Kliknite **Registracija**.

8. Sljedeće snimke zaslona označava je li podatke uspješno registriran u katalogu podataka.

    ![Registracija dovršeno] (./media/data-lake-store-with-data-catalog/registration-complete.png "Prikaz strukture podataka")

9. Kliknite **Prikaz Portal** vratite se na portal katalog podataka i provjere da sada možete pristupiti registriranih podataka na portalu. Da biste pronašli podatke, možete koristiti oznake koji ste koristili prilikom registracije podatke.

    ![Pretraživanje podataka u katalogu] (./media/data-lake-store-with-data-catalog/search-data-in-catalog.png "Pretraživanje podataka u katalogu")

10. Sada možete izvršavati operacije kao da dodajete Opaske i u dokumentaciji o podacima. Dodatne informacije potražite na sljedećim vezama.
    * [Dodavanje izvora podataka u katalogu podataka](../data-catalog/data-catalog-how-to-annotate.md)
    * [Izvori podataka za dokument u katalog podataka](../data-catalog/data-catalog-how-to-documentation.md)

## <a name="see-also"></a>Vidi također

* [Dodavanje izvora podataka u katalogu podataka](../data-catalog/data-catalog-how-to-annotate.md)
* [Izvori podataka za dokument u katalog podataka](../data-catalog/data-catalog-how-to-documentation.md)
* [Integriranje spremišta Lake podataka s drugih servisa za Azure](data-lake-store-integrate-with-other-services.md)
