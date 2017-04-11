<properties
    pageTitle="Analiza medijskih sadržaja pomoću portala za Azure | Microsoft Azure"
    description="U ovoj se temi opisuje način obrade medijskih sadržaja s pomoću portala za Azure Media analize media procesora (GPP)."
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
    ms.topic="article"
    ms.date="10/24/2016"
    ms.author="juliako"/>


# <a name="analyze-your-media-using-the-azure-portal"></a>Analiza medijskih sadržaja pomoću portala za Azure

> [AZURE.NOTE] Da biste dovršili ovaj Praktični vodič, potreban vam je račun za Azure. Detalje potražite u članku [Azure besplatnu probnu verziju](https://azure.microsoft.com/pricing/free-trial/). 

## <a name="overview"></a>Pregled

Azure Media Services analize je zbirka govora i vidom komponente (na skaliranje enterprise, usklađenost, sigurnost i globalni razgovor) koje olakšavaju za tvrtke ili ustanove i velike tvrtke za dobivanje glavne s akcijama uvida s svoje videodatoteke. Detaljnije pregled Azure Media Services analize potražite u članku [u ovoj](media-services-analytics-overview.md) se temi. 

U ovoj se temi opisuje način obrade medijskih sadržaja s pomoću portala za Azure Media analize media procesora (GPP). Medijske sadržaje analize GPP proizvesti MP4 datoteke ili datoteke JSON. Ako media procesor proizvodi datoteku MP4, progresivno mogu preuzeti datoteku. Ako media procesor proizvodi JSON datoteke, možete preuzeti datoteku iz spremišta blobova platforme Azure. 

## <a name="choose-an-asset-that-you-want-to-analyze"></a>Odaberite sredstvo koje želite analizirati 
 
1. [Portal za Azure](https://portal.azure.com/)odaberite svoj račun servisa Azure Media Services.
2. U prozoru **Postavke** odaberite **Resursi**.  
.
    ![Analiza videozapisa](./media/media-services-portal-analyze/media-services-portal-analyze001.png)

2. Odaberite resursa koje želite analizirati i pritisnite gumb **Analiza** .
        
    ![Analiza videozapisa](./media/media-services-portal-analyze/media-services-portal-analyze002.png)

3. U prozoru **medijski sadržaj procesa s analize medijskog sadržaja** odaberite procesor. 

    Ostale programe iz sustava potražite u članku se objašnjava zašto i način korištenja svake procesor. 
   
4. Pritisnite **Stvori** na početak posao.

## <a name="azure-media-indexer"></a>Azure Media indeksiranje

Procesor medijskih sadržaja **Za Azure Media indeksiranje** omogućuje vam da bi medijske datoteke i sadržaj koji se može pretraživati, kao i generiranje zatvoreni skrivene titlove prati. Ova sekcija omogućuje neke pojedinosti o mogućnostima koje navedete za ovaj MP.

![Analiza videozapisa](./media/media-services-portal-analyze/media-services-portal-analyze003.png)

### <a name="language"></a>Jezik

Prirodnim jezikom se prepoznaju u multimedijske datoteke. Na primjer, engleskom ili španjolskom jeziku. 

### <a name="captions"></a>Opisi

Možete odabrati oblik opis koji će biti stvorene iz sadržaja. Indeksiranje posao možete generirati titlova datoteke sljedećih oblika:  

- **sami**
- **TTML**
- **WebVTT**

Nepravilan oblik zatvoren opisa (CC) datoteke u tim oblicima može se koristiti za audio i videodatoteka čine pristupačnim osobe oštećena sluha pročita.

### <a name="aib-file"></a>AIB datoteka

Tu mogućnost odaberite ako želite da biste generirali Blob indeksa Audio datoteka za korištenje s prilagođene IFilter SQL Server. Dodatne informacije potražite u članku [ovom](https://azure.microsoft.com/blog/using-aib-files-with-azure-media-indexer-and-sql-server/) blogu.

### <a name="keywords"></a>Ključne riječi

Tu mogućnost odaberite ako želite da biste generirali XML datoteku za ključne riječi. Datoteka sadrži ključne riječi dobivenih iz sadržaja govora s učestalost i offset informacije.

### <a name="job-name"></a>Naziv zadatka

Neslužbeni naziv koji će vam prepoznavanje posao. [U ovom](media-services-portal-check-job-progress.md) se članku opisuje kako možete pratiti napredak posla. 

### <a name="output-file"></a>Izlazna datoteka

Neslužbeni naziv koji omogućuje označavanje sadržaja izlaz. 

## <a name="azure-media-hyperlapse"></a>Azure Media Hyperlapse

Azure Media Hyperlapse je programa MP koji stvara izglađenim vrijeme lapsed videozapisi iz prve osobe ili akcija kamere sadržaj.  Dodatne informacije potražite u članku [u ovoj](media-services-hyperlapse-content.md) se temi. Ova sekcija omogućuje neke pojedinosti o mogućnostima koje navedete za ovaj MP.

![Analiza videozapisa](./media/media-services-portal-analyze/media-services-portal-analyze004.png)

### <a name="speed"></a>Brzina 

Navedite Brzina kojom da biste ubrzali unos videozapis. Rezultat je stabilized i vrijeme lapsed vizualizacije unosa videozapisa.

### <a name="job-name"></a>Naziv zadatka

Neslužbeni naziv koji će vam prepoznavanje posao. [U ovom](media-services-portal-check-job-progress.md) se članku opisuje kako možete pratiti napredak posla. 

### <a name="output-file"></a>Izlazna datoteka

Neslužbeni naziv koji omogućuje označavanje sadržaja izlaz. 

## <a name="azure-media-face-detector"></a>Azure Media nominalne otkrivatelja

Procesor medijskih sadržaja za **Azure Media nominalne otkrivatelja** (MP) omogućuje vam Brojanje, praćenje premještanja, a čak i procijeni sudjelovanje ciljne skupine i izvršavanju putem putem izraza. Taj servis sadrži dvije značajke: 

- **Otkrivanje nominalne**

    Otkrivanje nominalne pronalazi i prati Ljudski smješka unutar videozapisa. Više smješka vidljivo i naknadno pratiti kao što su pomicati, s vremenom i mjestom metapodacima vraća u datoteci JSON. Tijekom praćenje će pokušati dati dosljedan ID isti nominalne dok je osoba koja je kretanje na zaslonu, čak i ako se mogu narušiti ili nakratko ostavite okvir.

    >[AZURE.NOTE]Ovaj services izvršiti putem prepoznavanja. Osobe koja ostavlja okvira ili postaje mogu narušiti za predugo će dobiti novi ID kada se vratite.

- **Otkrivanje emocije**
    
    Otkrivanje emocije je neobavezna komponenta procesora nominalne otkrivanje medijske sadržaje koji vraća analizu na više emocionalne atributa iz smješka otkrije, uključujući sreća, sadness, problema, anger i više. 

![Analiza videozapisa](./media/media-services-portal-analyze/media-services-portal-analyze005.png)

### <a name="detection-mode"></a>Način za otkrivanje

Jedan od sljedećih načina mogu koristiti procesor:

- Otkrivanje nominalne
- po nominalne emocije otkrivanje
- Otkrivanje zbrajanja emocije

### <a name="job-name"></a>Naziv zadatka

Neslužbeni naziv koji će vam prepoznavanje posao. [U ovom](media-services-portal-check-job-progress.md) se članku opisuje kako možete pratiti napredak posla. 

### <a name="output-file"></a>Izlazna datoteka

Neslužbeni naziv koji omogućuje označavanje sadržaja izlaz. 

## <a name="azure-media-motion-detector"></a>Azure Media kretanja otkrivatelja

Procesor medijskih sadržaja za **Azure Media kretanja otkrivatelja** (MP) omogućuje učinkovito prepoznavanje sekcije koje vas zanimaju unutar u suprotnom dugi i uneventful videozapis. Otkrivanje kretanja se može koristiti na materijal statične kameru da biste odredili sekcije videozapisa u kojima se pojavljuje kretanja. Generira JSON datoteku koja sadrži metapodatke s vremenske oznake i opisanog regije gdje došlo je do događaja.

Namijenjena sigurnost videozapisa sažetke sadržaja, tehnologije je Kategorizirajte kretanja u odgovarajuću događaja, a false positives kao što su sjene, a promjene osvjetljenja. Omogućuje stvaranje sigurnosnih upozorenja iz kamere sažetaka sadržaja bez povećavate s neograničene nevažnih događaje, uz mogućnost da biste izdvojili uvodnih kamata iznimno dugo surveillance videozapisi.

![Analiza videozapisa](./media/media-services-portal-analyze/media-services-portal-analyze006.png)

## <a name="azure-media-video-thumbnails"></a>Azure Media Video minijature

Ovog procesora omogućuju stvaranje sažetaka dugo videozapisa odabirom automatski zanimljive isječci iz izvora videozapisa. To je korisno kada želite li omogućiti Kratak pregled što mogu očekivati dugo videozapisa. Detaljne informacije i primjeri potražite u članku [Korištenje Azure Media Video minijature da biste stvorili videozapis Summarization](media-services-video-summarization.md)

![Analiza videozapisa](./media/media-services-portal-analyze/media-services-portal-analyze008.png)

### <a name="job-name"></a>Naziv zadatka

Neslužbeni naziv koji će vam prepoznavanje posao. [U ovom](media-services-portal-check-job-progress.md) se članku opisuje kako možete pratiti napredak posla. 

### <a name="output-file"></a>Izlazna datoteka

Neslužbeni naziv koji omogućuje označavanje sadržaja izlaz. 


##<a name="next-steps"></a>Daljnji koraci

Prikaz Media Services Tečajevi.

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Slanje povratnih informacija

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]


