<properties
   pageTitle="Izvoz zapisnika analize podataka u Power BI | Microsoft Azure"
   description="Power BI je servis analytics tvrtke u oblaku tvrtke Microsoft koji omogućuje obogaćenim vizualizacijama i izvješća za analizu skupove podataka.  Prijava analitiku neprestano podatke možete izvesti u spremištu OMS u Power BI tako da omogućuje korištenje njegove vizualizacije i alata za analizu.  U ovom se članku objašnjava kako konfigurirati upita u zapisnik analize automatski izvesti Power bi u pravilnim vremenskim razmacima."
   services="log-analytics"
   documentationCenter=""
   authors="bwren"
   manager="jwhit"
   editor="tysonn" />
<tags
   ms.service="log-analytics"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/18/2016"
   ms.author="bwren" />

# <a name="export-log-analytics-data-to-power-bi"></a>Izvoz zapisnika analize podataka u Power BI

[Power BI](https://powerbi.microsoft.com/documentation/powerbi-service-get-started/) je servis analytics tvrtke u oblaku tvrtke Microsoft koji omogućuje obogaćenim vizualizacijama i izvješća za analizu skupove podataka.  Prijava analitiku automatski podatke možete izvesti u spremištu OMS u Power BI tako da omogućuje korištenje njegove vizualizacije i alata za analizu.

Kada konfigurirate Power BI s prijava analitiku, stvorite zapisnika upita koji izvoz njihove rezultate u odgovarajuće skupove podataka u dodatku Power BI.  Upita i izvoz nastavlja se automatski pokrenuti raspored koju ste definirali za skup podataka Ostanite u tijeku s najnovijim podacima koji se prikupljaju prijava analitiku.

![Prijava analitiku u Power BI](media/log-analytics-powerbi/overview.png)

## <a name="power-bi-schedules"></a>Power BI rasporeda

*Power BI raspored* obuhvaća zapisnika pretraživanja koja izvozi skupa podataka u spremištu OMS odgovarajući skup podataka u Power BI i raspored koji definira Učestalost pokretanja ovog pretraživanja za ažuriranje skupu podataka.

Polja u skupu podataka će odgovarati svojstva zapisa rezultata pretraživanja zapisnika.  Ako pretraživanje vraća zapise različitih vrsta zatim skupu podataka neće sadržavati stavku sva svojstva iz svake vrste zapisa uključene.  

> [AZURE.NOTE] Je najbolje koristiti upit za pretraživanje zapisnika koja vraća sirovim podacima umjesto izvođenje bilo kojeg konsolidacije pomoću naredbi kao što je [mjera](log-analytics-search-reference.md#measure).  Bilo koji zbrajanja i izračuni u dodatku Power BI možete izvršiti sa sirovim podacima.

## <a name="connecting-oms-workspace-to-power-bi"></a>Povezivanje radnog prostora OMS Power BI

Power bi je mogli izvesti iz zapisnika analize, morate se povezati OMS radnog prostora s računom servisa Power BI pomoću sljedećeg postupka.  

1. Na konzoli OMS kliknite pločicu **Postavke** .
2. Odaberite **računi**.
3. U odjeljku **Informacije o radnom prostoru** kliknite **Poveži se s računom za Power BI**.
4. Unesite vjerodajnice za vaš račun servisa Power BI.

## <a name="create-a-power-bi-schedule"></a>Stvaranje rasporeda Power BI

Stvaranje rasporeda Power BI za svaki skup podataka pomoću sljedećeg postupka.

1. Na konzoli OMS kliknite pločicu **Zapisnika pretraživanja** .
2. Upišite novi upit ili odaberite spremljeno pretraživanje koji vraća podatke koje želite izvesti **Power**bi.  
3. Kliknite gumb **Power BI** pri vrhu stranice da biste otvorili dijaloški okvir **Power BI** .
4. Unesite podatke u tablici u nastavku, a zatim kliknite **Spremi**.

| Svojstvo | Opis |
|:--|:--|
| Ime | Naziv radi prepoznavanja rasporeda kada pregledavate popis rasporede Power BI. |
| Spremljeno pretraživanje | Pretraživanje zapisnika radi.  Možete odabrati trenutni upit ili u okviru padajućeg izbornika odaberite postojeći spremljeno pretraživanje. |
| Raspored | Kako se često Pokreni spremljeno pretraživanje, a zatim izvoz u skupu podataka za Power BI.  Vrijednost mora biti između 15 minuta i 24 sata. |
| Naziv skupa podataka | Naziv skupa podataka u dodatku Power BI.  Bit će stvoren ako ne postoji i ažurirati ako postoji. |

## <a name="viewing-and-removing-power-bi-schedules"></a>Prikaz i uklanjanje rasporede Power BI

Prikaz popisa postojeće rasporeda Power BI pomoću sljedećeg postupka.

1. Na konzoli OMS kliknite pločicu **Postavke** .
2. Odaberite **Power BI**.

Osim detalje raspored, prikazuju se broj puta namijenjeni rasporeda tijekom proteklog tjedna i status zadnje sinkronizacije.  Ako sinkroniziranje je naišao na pogreške, možete kliknuti vezu da biste pokrenuli zapisnika Traženje zapisa s detaljima o pogrešci.

Raspored možete ukloniti tako da kliknete na **X** u **Uklanjanje stupca**.  Raspored možete onemogućiti tako da odaberete **Isključeno**.  Da biste izmijenili raspored ga ukloniti i ponovno ga stvorite nove postavke.

![Power BI rasporeda](media/log-analytics-powerbi/schedules.png)

## <a name="sample-walkthrough"></a>Vodič za uzorak
U sljedećoj sekciji vodi kroz primjera stvaranje rasporeda Power BI i Stvaranje jednostavnog izvješća pomoću njegov skup podataka.  U ovom primjeru svih podataka o performansama za skup računala izvozi se na Power BI, a zatim stvaranja linijski grafikon da biste prikazali procesor Upotreba.

### <a name="create-log-search"></a>Stvaranje zapisnika pretraživanja
Najprije ćemo stvaranju zapisnika pretraživanja za podatke koji ne možemo želite poslati u skupu podataka.  U ovom primjeru ćemo koristiti upit koji vraća svih podataka o performansama računala čiji naziv počinje *srv*.  

![Power BI rasporeda](media/log-analytics-powerbi/walkthrough-query.png)

### <a name="create-power-bi-search"></a>Stvaranje Power BI pretraživanja
Ne možemo kliknite gumb **Power BI** da biste otvorili dijaloški okvir Power BI i unesite potrebne podatke.  Želimo ovo pretraživanje da biste pokrenuli svaki sat i stvaranje dataset pod nazivom *Contoso mjerača performansi*.  Budući da već imamo pretraživanja otvorite koji stvara podatke želimo, ne možemo zadržati zadanom *Koristi trenutni upit za pretraživanje* za **Spremljeno pretraživanje**.

![Power BI pretraživanja](media/log-analytics-powerbi/walkthrough-schedule.png)

### <a name="verify-power-bi-search"></a>Provjerite je li Power BI pretraživanja
Da biste potvrdili da ne možemo li pravilno stvorili raspored, možemo li vidjeti popis Power BI pretraživanja u odjeljku pločice **Postavke** na nadzornoj ploči OMS.  Ne možemo Pričekajte nekoliko minuta i osvježite prikaz dok je javlja da je sinkronizacija pokrenuta.

![Power BI pretraživanja](media/log-analytics-powerbi/walkthrough-schedules.png)

### <a name="verify-the-dataset-in-power-bi"></a>Provjerite je li skup podataka u dodatku Power BI
Ne možemo prijavite se u našem račun na [powerbi.microsoft.com](http://powerbi.microsoft.com/) i pomaknite se do **skupove podataka** u donjem lijevom oknu.  Vidimo da je dataset *Tvrtke Contoso mjerača performansi* nalazi kojoj stoji da je uspješno pokrenuti naš izvoz.

![Skup podataka za Power BI](media/log-analytics-powerbi/walkthrough-datasets.png)

### <a name="create-report-based-on-dataset"></a>Stvaranje izvješća na temelju skup podataka
Ne možemo se odaberite skup podataka **Mjerača performansi Contoso** , a zatim kliknite na **rezultate** u oknu **polja** na desnoj strani da biste pogledali polja koja su dio ovaj skup podataka.  Da biste stvorili linijski grafikon prikazuje procesor Upotreba za svako računalo, ne možemo izvoditi sljedeće radnje.

1. Odaberite vizualizaciju linijski grafikon.
2. Povucite **ObjectName** **razine filtar izvješća** i provjerite **procesor**.
3. Povucite **CounterName** **razine filtar izvješća** i provjerite **% procesor vremena**.
4. Povucite **CounterValue** **vrijednosti**.
5. Povucite **računalo** **legende**.
6. Povucite **TimeGenerated** **osi**.

Vidimo prikazuje li se stvorenom grafikonu redak s podacima iz naših skup podataka.

![Power BI linijski grafikon](media/log-analytics-powerbi/walkthrough-linegraph.png)

### <a name="save-the-report"></a>Spremite izvješće
Ne možemo spremite izvješće, i to klikom na gumb Spremi pri vrhu zaslona i provjeriti da ona se sada nalazi u odjeljku Reports u lijevom oknu.

![Izvješća značajke Power BI](media/log-analytics-powerbi/walkthrough-report.png)

## <a name="next-steps"></a>Daljnji koraci

- Informirajte se o [zapisniku pretraživanja](log-analytics-log-searches.md) da biste sastavili upite za koje je moguće je izvesti Power bi.
- Dodatne informacije o [Dodatku Power BI](http://powerbi.microsoft.com) da biste sastavili viizualizacije na temelju prijava analitiku izvozi.
