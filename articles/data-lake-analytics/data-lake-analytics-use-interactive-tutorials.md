<properties 
   pageTitle="Naučite analize podataka Lake te U SQL Azure Portal interaktivni vodiči za korištenje | Azure" 
   description="Brzi početak rada s učenje analize podataka Lake i U SQL. " 
   services="data-lake-analytics" 
   documentationCenter="" 
   authors="edmacauley" 
   manager="jhubbard" 
   editor="cgronlun"/>
 
<tags
   ms.service="data-lake-analytics"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data" 
   ms.date="05/16/2016"
   ms.author="edmaca"/>


# <a name="use-azure-data-lake-analytics-interactive-tutorials"></a>Interaktivni vodiči za analize korištenja Azure podataka Lake

Portal za Azure predstavlja interaktivni vodič za početak rada s podacima Lake analize. Članci objašnjava prođite kroz vodič za analizu zapisnika web-mjesta.


>[AZURE.NOTE]Ako želite prođite kroz isti praktičnom vodiču pomoću Visual Studio, potražite u članku [Analiza pomoću analize podataka Lake zapisnika za web-mjesta](data-lake-analytics-analyze-weblogs.md).
>Više interaktivni vodiči za koja će se dodati na portal.


Ostale praktične vodiče, potražite u članku:

- [Početak rada s podacima Lake analize pomoću portala za Azure](data-lake-analytics-get-started-portal.md)
- [Početak rada s podacima Lake analize pomoću komponente PowerShell Azure](data-lake-analytics-get-started-powershell.md)
- [Početak rada s podacima Lake analize pomoću .NET SDK](data-lake-analytics-get-started-net-sdk.md)
- [Razvoj U SQL skripte pomoću alata za Lake podataka za Visual Studio](data-lake-analytics-data-lake-tools-get-started.md) 

**Preduvjeti**

Prije početka ovog praktičnog vodiča, morate imati sljedeće:

- **A podataka Lake analize računa**.  Potražite u članku [Početak rada s analize Lake podataka za Azure pomoću portala za Azure](data-lake-analytics-get-started-portal.md).

##<a name="create-data-lake-analytics-account"></a>Stvaranje računa analize podataka Lake 

Morate imati račun analize podataka Lake prije pokretanja sve zadatke.

Svaki račun analize podataka Lake ima ovisnost računa [Spremišta Lake za Azure podataka](../data-lake-store/data-lake-store-overview.md) .  Taj račun se nazivaju zadanog računa za spremište Lake podataka.  Možete stvoriti račun spremišta podataka Lake prije toga ili prilikom stvaranja računa analize podataka Lake. U ovom ćete praktičnom vodiču ćete stvoriti računa spremišta Lake podataka s računom Analytics

**Da biste stvorili analize podataka Lake račun**

1. Prijavite se [Portal za Azure](https://portal.azure.com/signin/index/?Microsoft_Azure_Kona=true&Microsoft_Azure_DataLake=true&hubsExtension_ItemHideKey=AzureDataLake_BigStorage%2cAzureKona_BigCompute).
2. Kliknite **Microsoft Azure** u gornjem lijevom kutu da biste otvorili u StartBoard.
3. Kliknite pločicu **trgovine** .  
3. Upišite **Azure podataka Lake analize** u okvir za pretraživanje na plohu **sve** , a zatim pritisnite **ENTER**. **Azure podataka Lake analize** moraju se prikazuje na popisu.
4. Na popisu kliknite **Azure podataka Lake analize** .
5. Kliknite **Stvori** na dnu u plohu.
6. Upišite ili odaberite sljedeće:

    ![Azure podataka Lake analize portala plohu](./media/data-lake-analytics-get-started-portal/data-lake-analytics-portal-create-adla.png)

    - **Naziv**: naziv računa analize.
    - **Pohrana podataka Lake**: račun za svakog analize podataka Lake sadrži zavisne računa spremišta Lake podataka. Račun analize podataka Lake i o njima ovisne računa spremišta podataka Lake moraju nalaziti u istoj Azure podatkovnog centra. Pratite upute za stvaranje novog računa spremišta Lake podataka ili odaberite postojeći.
    - **Pretplate**: Odaberite Azure pretplatu koristi za račun analize.
    - **Grupa resursa**. Odaberite postojeću grupu resursa Azure ili stvorite novi. Aplikacija obično se sastoje od više komponente, primjerice web-aplikacijama, baze podataka, poslužitelj baze podataka, za pohranu i 3 davatelja usluga. Azure upravitelj resursa (ARM) omogućuje rad s resursima u aplikaciji grupe se nazivaju i grupu resursa Azure. Možete uvesti, ažuriranje, praćenje i brisanje svi resursi aplikacije u jednom, usklađenih postupak. Pomoću predloška za implementaciju i taj predložak možete iskoristiti različitim okruženjima kao što su testiranje pripremna i proizvodnje. Pregledom kumulativne troškove za cijelu grupu možete pojašnjavaju naplate za tvrtku ili ustanovu. Dodatne informacije potražite u članku [Pregled upravljanja resursima Azure](azure-resource-manager/resource-group-overview.md). 
    - **Mjesto**. Odaberite centar za Azure podataka za račun analize podataka Lake. 
7. Odaberite **Prikvači na Startboard**. Ovo je obavezan za praćenje ovog praktičnog vodiča.
8. Kliknite **Stvori**. Ga vodi vas na portalu StartBoard. Novu pločicu dodaje se na početnu stranicu s natpisom prikazuje "Implementacija Azure podataka Lake analize". Potrebno stvoriti račun analize podataka Lake trenutak. Prilikom stvaranja računa portalu otvorit će se račun na novu plohu.

    ![Azure podataka Lake analize portala plohu](./media/data-lake-analytics-get-started-portal/data-lake-analytics-portal-blade.png)

##<a name="run-website-log-analysis-interactive-tutorial"></a>Pokretanje interaktivni vodič za analizu zapisnika web-mjesta

**Da biste otvorili interaktivni vodič analize zapisnik web-mjesta**

1. S portala sustava kliknite **Microsoft Azure** na lijevom izborniku da biste otvorili u StartBoard.
2. Kliknite pločicu koja je povezana s računom analize podataka Lake.
3. Kliknite **interaktivni vodiči za istraživanje** s trake **Essentials** .

    ![Interaktivni vodiči za analize podataka Lake](./media/data-lake-analytics-use-interactive-tutorials/data-lake-analytics-explore-interactive-tutorials.png)

4. Ako vidite narančaste sljedeću za upozorenja "uzoraka nije postavljeno, kliknite...", kliknite **Kopiraj ogledne podatke** da biste kopirali ogledne podatke zadanog računa za spremište Lake podataka. Interaktivni vodič mora podatke koje želite pokrenuti.
5. Plohu **Interaktivni vodiči za** kliknite **Analitika zapisnik web-mjesta**. Na portalu vodič se otvara u novi portala plohu.
5. Kliknite **1 Uvod** , a zatim slijedite upute

##<a name="see-also"></a>Vidi također

- [Pregled analize podataka Lake za Microsoft Azure](data-lake-analytics-overview.md)
- [Početak rada s podacima Lake analize pomoću portala za Azure](data-lake-analytics-get-started-portal.md)
- [Početak rada s podacima Lake analize pomoću komponente PowerShell Azure](data-lake-analytics-get-started-powershell.md)
- [Razvoj U SQL skripte pomoću alata za Lake podataka za Visual Studio](data-lake-analytics-data-lake-tools-get-started.md)
- [Analiza zapisnika web-mjesta pomoću Lake analize podataka za Azure](data-lake-analytics-analyze-weblogs.md)
