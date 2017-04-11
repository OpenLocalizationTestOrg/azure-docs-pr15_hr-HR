<properties
    pageTitle="Dodavanje unosa podataka na vaše poslove strujanje analize | Microsoft Azure"
    description="Saznajte kako priključiti izvora podataka za vaš posao strujanje analize kao strujanje unosa podataka iz koncentratora za događaj ili referenca podataka iz spremišta bloga."
    keywords="Podaci za unos, strujanja podataka"
    documentationCenter=""
    services="stream-analytics"
    authors="jeffstokes72"
    manager="jhubbard"
    editor="cgronlun"
/>

<tags
    ms.service="stream-analytics"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="data-services"
    ms.date="09/26/2016"
    ms.author="jeffstok"
/>


# <a name="add-a-streaming-data-input-or-reference-data-to-a-stream-analytics-job"></a>Dodavanje strujanja podataka unos ili referenca podataka analize toka posla

Saznajte kako priključiti izvora podataka za vaš posao strujanje analize kao strujanje unosa podataka iz koncentratora za događaj ili referenca podataka iz spremišta blobova.

Azure strujanje analize poslove moguće je povezati unosa podataka jedan ili više njih, od kojih svaka definiranje veze u postojeći izvor podataka. Kako se podaci šalju u tom izvoru podataka, je troše posao strujanje analize i obrađuju u stvarnom vremenu kao strujanja podataka. Analitički strujanje ima prve klase Integracija sa [Azure događaj koncentratora](https://azure.microsoft.com/services/event-hubs/) i [spremište blobova platforme Azure](../storage/storage-dotnet-how-to-use-blobs.md) unutar i izvan nje omogućiti posla pretplate.

U ovom se članku je korak u [strujanje analize tečaj](/documentation/learning-paths/stream-analytics/).

## <a name="data-input-streaming-data-and-reference-data"></a>Unos podataka: strujanja podataka i referentnih podataka

Postoje dvije različite vrste unosa u strujanje analize: strujanja podataka i referentnih podataka.

- **Strujanja podataka**: poslove strujanje analize mora sadržavati barem jedan podatkovni strujanje unos utrošiti i transformacije posla. Azure blobova i Azure događaj koncentratora podržani su kao strujanje unos izvore. Azure koncentratora događaj se koriste za prikupljanje tokove događaj iz povezanih uređaja, servisa i aplikacije. Azure blobova moguće je koristiti kao izvor unos za ingesting skupno podataka kao tok.  
- **Referentnih podataka**: strujanje analize podržava druga vrsta podataka pomoćnih unos naziva reference.  Umjesto podataka u pokretu, ti podaci je statične ili usporavanja promjene.  Se obično koristi za izvođenje look-ups i korelacija s strujanja podataka da biste stvorili bogatiji skupa podataka.  Azure blobova trenutno samo podržani ulazni izvor za referentnih podataka.  

Da biste dodali unos posla strujanje analize:

1. Na portalu za Azure kliknite **unosa** , a zatim **Dodaj ulazni** u vaš posao strujanje analize.

    ![Azure klasični portala - dodavanje ulazni.](./media/stream-analytics-add-inputs/1-stream-analytics-add-inputs.png)  

    Na portalu za Azure kliknite pločicu **unose** u vaš posao strujanje analize.  

    ![Portal za Azure – Dodavanje unosa podataka.](./media/stream-analytics-add-inputs/7-stream-analytics-add-inputs.png)  

2. Određivanje vrste unos: **strujanja podataka** ili **referentnih podataka**.

    ![Dodavanje točne podatke za unos, strujanjem ili referenca](./media/stream-analytics-add-inputs/2-stream-analytics-add-inputs.png)  

    ![Dodavanje točne podatke za unos, strujanjem ili referenca](./media/stream-analytics-add-inputs/8-stream-analytics-add-inputs.png)  

3. Ako stvorite strujanja podataka unos, navedite vrsta izvora za unos.  Tijekom stvaranja referentnih podataka kao samo blobova platforme za pohranu podržana je trenutno mogu se preskočiti ovaj korak.

    ![Dodavanje unosa podataka toka podataka](./media/stream-analytics-add-inputs/3-stream-analytics-add-inputs.png)  

    ![Dodavanje unosa podataka toka podataka](./media/stream-analytics-add-inputs/9-stream-analytics-add-inputs.png)  

4. Navedite neslužbeni naziv za taj unos u okvir ulazni pseudonim.  Ovaj naziv će se koristiti u upitu vaš posao kasnije da biste se pozvali na unos.

    Unesite ostatak svojstva potrebna veze za povezivanje s izvorom podataka. Ta polja razlikuju se po vrsti vrste unos i izvora i definiraju detaljno [ovdje](stream-analytics-create-a-job.md).  

    ![Dodavanje unosa podataka koncentrator za događaj](./media/stream-analytics-add-inputs/4-stream-analytics-add-inputs.png)  

5. Određivanje postavki serijalizacije za unos podataka:
    - Da biste bili sigurni upitima funkcionira kao što ste očekivali, navedite **Događaj serijaliziranog oblika** ulazne podatke.  Oblici serijalizacije podržani su JSON, CSV i Avro.
    - Provjerite je li **kodiranje** za podatke.  UTF-8 trenutno samo podržani oblik kodiranja.

    ![Postavke serijalizacije podataka za unos podataka](./media/stream-analytics-add-inputs/5-stream-analytics-add-inputs.png)  

    ![Postavke serijalizacije podataka za unos podataka](./media/stream-analytics-add-inputs/10-stream-analytics-add-inputs.png)  

6. Nakon dovršetka unos stvaranja strujanje analize ćete provjeriti da ga možete povezati s izvorom unosa.  U središtu obavijesti možete prikazati stanje operacije Testiraj vezu.

    ![Testiranje veze strujanja podataka za unos](./media/stream-analytics-add-inputs/6-stream-analytics-add-inputs.png)  

    ![Testiranje veze strujanja podataka za unos](./media/stream-analytics-add-inputs/11-stream-analytics-add-inputs.png)  

## <a name="get-help-with-streaming-data-inputs"></a>Pronađite pomoć vezanu uz strujanje unosa podataka
Dodatnu pomoć, pokušajte našem [forumu Azure strujanje Analytics](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics)

## <a name="next-steps"></a>Daljnji koraci

- [Uvod u Azure strujanje Analytics](stream-analytics-introduction.md)
- [Prvi koraci pri korištenju Azure strujanje Analytics](stream-analytics-get-started.md)
- [Promjena veličine Azure strujanje analize poslova](stream-analytics-scale-jobs.md)
- [Azure strujanje analize upita jezična referenca](https://msdn.microsoft.com/library/azure/dn834998.aspx)
- [Azure strujanje analize upravljanje REST API Reference](https://msdn.microsoft.com/library/azure/dn835031.aspx)
