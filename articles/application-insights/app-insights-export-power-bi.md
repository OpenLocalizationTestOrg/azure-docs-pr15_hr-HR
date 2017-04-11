<properties 
    pageTitle="Izvoz u Power BI iz aplikacije uvida | Microsoft Azure" 
    description="Analize upita prikazuje se u dodatku Power BI." 
    services="application-insights" 
    documentationCenter=""
    authors="noamben" 
    manager="douge"/>

<tags 
    ms.service="application-insights" 
    ms.workload="tbd" 
    ms.tgt_pltfrm="ibiza" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/18/2016" 
    ms.author="awills"/>

# <a name="feed-power-bi-from-application-insights"></a>Sažetak sadržaja servisa Power BI iz aplikacije uvida

[Power BI](http://www.powerbi.com/) je paket sustava business analytics alatima koji služe analiza podataka i dijelili Saznanja. Nadzorne ploče obogaćeni su dostupne na svim uređajima. Možete kombiniranje podataka iz više izvora, uključujući analize upita s [Uvida aplikacije za Visual Studio](app-insights-overview.md).

Tri su preporučena metode izvoza podataka aplikacije uvida u Power BI. Možete ih koristiti zasebno ili zajedno.

* [**Prilagodnik za power BI**](#power-pi-adapter) – postavljanje dovršeno nadzorne ploče programa telemetrijskih iz aplikacije. Skup grafikoni je unaprijed definirane, ali možete dodati svoje upite iz drugih izvora.
* [**Izvoz analize upita**](#export-analytics-queries) - napišite nešto upita pomoću analize, a zatim izvoz u Power BI. Ovaj upit možete postaviti na nadzornoj ploči zajedno s bilo koje druge podatke.
* [**Izvoz Continous i analize strujanja**](app-insights-export-stream-analytics.md) – To uključuje dodatne rad da biste postavili. Korisno je ako želite zadržati podatke dugo. U suprotnom, preporučuje se druge načine.

## <a name="power-bi-adapter"></a>Power BI prilagodnika

Ovaj postupak za vas stvara dovrši nadzorne ploče programa telemetrijskih. Početni skup podataka je unaprijed definirane, ali na njega možete dodati više podataka.

### <a name="get-the-adapter"></a>Početak prilagodnika

1. Prijavite se na [Power BI](https://app.powerbi.com/).
2. Otvorite **Dohvati podatke**, **servise**ili **aplikacije uvida**

    ![Početak iz izvora podataka za aplikaciju uvida](./media/app-insights-export-power-bi/power-bi-adapter.png)

3. Detalje o vaše aplikacije uvida resursa.

    ![Početak iz izvora podataka za aplikaciju uvida](./media/app-insights-export-power-bi/azure-subscription-resource-group-name.png)

4. Pričekajte minutu ili dvije za podatke koje želite uvesti.

    ![Power BI prilagodnika](./media/app-insights-export-power-bi/010.png)


Na nadzornoj ploči možete urediti kombiniranja grafikona uvida aplikacije s promjenama drugih izvora i analize upita. Postoji galerije vizualizaciju koju možete dobiti više grafikona, a svaki grafikon ima parametre možete postaviti.

Nakon početnog uvoza na nadzornoj ploči i izvješća i dalje da biste ažurirali svakodnevno. Možete odrediti raspored osvježavanja na skupu podataka.


## <a name="export-analytics-queries"></a>Izvoz analize upita

U ovom usmjeravanje omogućuje pisanje bilo koji analize upit koji vam se sviđa, a zatim izvezite koji na nadzornu ploču dodatka Power BI. (Možete dodati na nadzornoj ploči stvorio prilagodnika.)

### <a name="one-time-install-power-bi-desktop"></a>Jedna jedinica vremena: instalirati Power BI Desktop

Da biste uvezli aplikacije uvida upit, pomoću verzije programa Power BI za stolna računala. No, pa je možete objaviti na web-mjestu ili u Power BI oblaka prostor programa Groove. 

Instalirajte [Power BI Desktop](https://powerbi.microsoft.com/en-us/desktop/).

### <a name="export-an-analytics-query"></a>Izvoz upit Analytics

1. [Otvori analize i pisanje upita](app-insights-analytics-tour.md).
2. Testirajte da biste suzili upit dok ne budete zadovoljni s rezultatima.
3. Na izborniku za **Izvoz** odaberite **Power BI (M)**. Spremite tekstnu datoteku.

    ![Izvoz upita dodatka Power BI](./media/app-insights-export-power-bi/analytics-export-power-bi.png)
4. U dodatku Power BI Desktop odaberite **Dohvati podatke, prazan upit** , a zatim u uređivaču upita u odjeljku **Prikaz** odaberite **Napredni uređivač upita**.


    Zalijepite izvezenu skripta jezika M u napredne uređivaču upita.

    ![Uređivač naprednog upita](./media/app-insights-export-power-bi/power-bi-import-analytics-query.png)

5. Možda ćete morati unijeti vjerodajnice da biste omogućili Power BI da biste pristupili Azure. Korištenje "račun tvrtke ili ustanove" da biste se prijavili pomoću Microsoftova računa.

    ![Navedite Azure vjerodajnice da biste omogućili Power BI da biste pokrenuli upit uvida aplikacije](./media/app-insights-export-power-bi/power-bi-import-sign-in.png)

6. Odaberite vizualizaciju upita, a zatim odaberite polja za osi x, y i segmenting dimenzije.

    ![Odaberite vizualizaciju](./media/app-insights-export-power-bi/power-bi-analytics-visualize.png)

7. Objavite izvješća servisa Power BI oblaka radnog prostora. Iz nje, moguće je ugraditi sinkronizirane verzija u drugim web-stranice.

    ![Odaberite vizualizaciju](./media/app-insights-export-power-bi/publish-power-bi.png)
 
8. Ručno osvježavanje izvješća u vremenskim razmacima ili postaviti zakazano osvježavanje na stranici mogućnosti.


## <a name="about-sampling"></a>O uzorkovanje

Ako aplikacija pošalje velike količine podataka, značajku prilagodljivo uzorkovanje može raditi i pošaljite samo postotak vaše telemetrijskih. Isto vrijedi i ako ste ručno uzorkovanje u SDK ili na ingestion. [Saznajte više o uzorkovanje.](app-insights-sampling.md)
 

## <a name="next-steps"></a>Daljnji koraci

* [Power BI – Saznajte](http://www.powerbi.com/learning/)
* [Praktični vodič Analytics](app-insights-analytics-tour.md)
