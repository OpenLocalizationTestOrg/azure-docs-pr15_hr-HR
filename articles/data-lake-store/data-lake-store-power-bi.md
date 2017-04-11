<properties
   pageTitle="Analiza podataka u spremištu Lake podataka pomoću dodatka Power BI | Microsoft Azure"
   description="Korištenje servisa Power BI da biste analizirali podatke pohranjene u spremištu Lake podataka za Azure"
   services="data-lake-store" 
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
   ms.date="10/05/2016"
   ms.author="nitinme"/>

# <a name="analyze-data-in-data-lake-store-by-using-power-bi"></a>Analiza podataka u spremištu Lake podataka pomoću dodatka Power BI

U ovom članku će Saznajte kako koristiti Power BI Desktop za analizu i Vizualizirajte podatke pohranjene u spremištu Lake za Azure podataka.

## <a name="prerequisites"></a>Preduvjeti

Prije početka ovog praktičnog vodiča, morate imati sljedeće:

- **Mogući Azure pretplate**. Pogledajte [Početak Azure besplatnu probnu verziju](https://azure.microsoft.com/pricing/free-trial/).

- **Račun za azure podataka Lake trgovine**. Slijedite upute na [Početak rada s spremišta Lake podataka za Azure pomoću portala za Azure](data-lake-store-get-started-portal.md). U ovom se članku pretpostavlja da ste već stvorili račun spremišta Lake podataka, pod nazivom **mybidatalakestore**i prenijeli datoteku s oglednim podacima (**Drivers.txt**). U ovom oglednu datoteku dostupna je za preuzimanje [Azure podataka Lake brojka](https://github.com/Azure/usql/tree/master/Examples/Samples/Data/AmbulanceData/Drivers.txt)spremištu.

- **Power BI Desktop**. To možete preuzeti iz [Microsoftova centra za preuzimanje](https://www.microsoft.com/en-us/download/details.aspx?id=45331). 


## <a name="create-a-report-in-power-bi-desktop"></a>Stvaranje izvješća u Power BI Desktop

1. Pokrenite Power BI Desktop na vašem računalu.

2. S vrpce **Polazno** kliknite **Dohvati podatke**, a zatim kliknite više. U dijaloškom okviru **Dohvaćanje podataka** kliknite **Azure**, kliknite **Azure podataka Lake trgovina**i zatim kliknite **Poveži**.

    ![Povezivanje sa spremištem Lake podataka] (./media/data-lake-store-power-bi/get-data-lake-store-account.png "Povezivanje sa spremištem Lake podataka")

3. Ako se prikaže dijaloški okvir o connector uvijek u fazi razvoja, odabrati da biste nastavili.

4. U dijaloškom okviru **Spremište Lake sustava Microsoft Azure podataka** Navedite URL-a na račun za spremište Lake podataka, a zatim **u redu**.

    ![URL za spremište Lake podataka] (./media/data-lake-store-power-bi/get-data-lake-store-account-url.png "URL za spremište Lake podataka")

5. U sljedećem dijaloškom okviru kliknite **Prijava** da biste se prijavili u obzir spremišta Lake podataka. Će biti preusmjereni na vaše tvrtke ili ustanove stranicom za prijavu. Slijedite upute da biste se prijavili na račun.

    ![Prijavite se u spremištu Lake podataka] (./media/data-lake-store-power-bi/get-data-lake-store-account-signin.png "Prijavite se u spremištu Lake podataka")

6. Kada uspješno ste se prijavili u, kliknite **Poveži**.

    ![Povezivanje sa spremištem Lake podataka] (./media/data-lake-store-power-bi/get-data-lake-store-account-connect.png "Povezivanje sa spremištem Lake podataka")

7. U sljedećem dijaloškom okviru prikazuje datoteku koju ste prenijeli na račun za spremište Lake podataka. Provjerite informacije, a zatim kliknite **Učitaj**.

    ![Učitavanja podataka iz spremišta Lake podataka] (./media/data-lake-store-power-bi/get-data-lake-store-account-load.png "Učitavanja podataka iz spremišta Lake podataka")

8. Kada podatke uspješno je učitana u Power BI, vidjet ćete sljedeća polja na kartici **polja** .

    ![Uvezeno polja] (./media/data-lake-store-power-bi/imported-fields.png "Uvezeno polja")

    Međutim, vizualizacija i analizirati podatke, ne možemo radije podataka da bi bio dostupan po sljedeća polja

    ![Desired polja] (./media/data-lake-store-power-bi/desired-fields.png "Desired polja")

    U sljedećim koracima ne možemo ažurirat će se upit za pretvaranje uvezenih podataka u željenom obliku.

9. Na vrpci **Početna stranica** kliknite **Uređivanje upita**.

    ![Uređivanje upita] (./media/data-lake-store-power-bi/edit-queries.png "Uređivanje upita")

10. U uređivaču upita, u stupcu **sadržaja** kliknite **Binarni**.

    ![Uređivanje upita] (./media/data-lake-store-power-bi/convert-query1.png "Uređivanje upita")

11. Prikazat će se ikona datoteke koja predstavlja **Drivers.txt** datoteku koju ste prenijeli. Desnom tipkom miša kliknite datoteku, a zatim kliknite **CSV**.  

    ![Uređivanje upita] (./media/data-lake-store-power-bi/convert-query2.png "Uređivanje upita")

12. Prikazat će se izlazni kao što je prikazano u nastavku. Podaci su dostupne u obliku koji možete koristiti za stvaranje vizualizacija.

    ![Uređivanje upita] (./media/data-lake-store-power-bi/convert-query3.png "Uređivanje upita")

13. S vrpce **Polazno** kliknite **Zatvori i Primijeni**, a zatim **zatvorite, a zatim primijenite**.

    ![Uređivanje upita] (./media/data-lake-store-power-bi/load-edited-query.png "Uređivanje upita")

14. Kada se ažurira se upit, na kartici **polja** prikazat će se nova polja za vizualizaciju.

    ![Ažuriranje polja] (./media/data-lake-store-power-bi/updated-query-fields.png "Ažuriranje polja")

15. Javite nam stvaranje tortnog grafikona za predstavljanje upravljačke programe u svaki grad navedene države. Da biste to učinili, provjerite sljedeće.

    1. Na kartici vizualizacije kliknite simbol za tortni grafikon.

        ![Stvaranje tortnog grafikona] (./media/data-lake-store-power-bi/create-pie-chart.png "Stvaranje tortnog grafikona")

    2. Stupci koji ne možemo namjeravate koristiti su **stupac 4** (naziv grada) i **stupca 7** (naziv države). Povucite ti stupci s kartice **polja** na karticu **vizualizacije** kao što je prikazano u nastavku.

        ![Stvaranje vizualizacije] (./media/data-lake-store-power-bi/create-visualizations.png "Stvaranje vizualizacije")

    3. Tortni grafikon moraju sada oblik sličan poput prikazano u nastavku.

        ![Tortni grafikon] (./media/data-lake-store-power-bi/pie-chart.png "Stvaranje vizualizacije")

16. Tako da odaberete određenoj državi filtri razine stranice, sada možete vidjeti broj upravljačke programe u svaki grad odabrane države. Na primjer, na kartici **vizualizacije** , u odjeljku **Filtri razini stranice**, odaberite **Brazil**.

    ![Odaberite državu] (./media/data-lake-store-power-bi/select-country.png "Odaberite državu")

17. Tortni grafikon automatski ažurira da bi se prikazao upravljačke programe u gradova Brazil.

    ![Upravljačke programe u države] (./media/data-lake-store-power-bi/driver-per-country.png "Upravljačke programe po državi")

18. Na izborniku **datoteka** kliknite **Spremi** da biste spremili vizualizacije kao datoteku Power BI Desktop.

## <a name="publish-report-to-power-bi-service"></a>Objava izvješća servisa Power BI

Nakon stvaranja vizualizacije u dodatku Power BI Desktop, možete je dijeliti s drugim korisnicima tako da ga objavite na servis Power BI. Upute o tome kako to učiniti potražite u članku [objavljivanje iz dodatka Power BI Desktop](https://powerbi.microsoft.com/documentation/powerbi-desktop-upload-desktop-files/).

## <a name="see-also"></a>Vidi također

* [Analiza podataka u spremištu Lake podataka pomoću analize podataka Lake](../data-lake-analytics/data-lake-analytics-get-started-portal.md)
