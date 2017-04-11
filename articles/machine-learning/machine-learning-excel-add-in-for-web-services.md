<properties
    pageTitle="Excel dodatak za servise strojnog učenja Web | Microsoft Azure"
    description="Kako koristiti Azure strojnog učenja Web services izravno u programu Excel bez pisanja kod."
    services="machine-learning"
    documentationCenter=""
    authors="tedway"
    manager="jhubbard"
    editor="cgronlun"
    tags=""/>

<tags
    ms.service="machine-learning"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="data-services"
    ms.date="10/05/2016"
    ms.author="tedway;garye" />

# <a name="excel-add-in-for-azure-machine-learning-web-services"></a>Dodatak programa Excel za Azure strojnog učenja Web services

Excel olakšava poziva web-servisi izravno bez potrebe pisanja koda.

## <a name="steps-to-use-an-existing-web-service-in-the-workbook"></a>Korake da biste koristili na postojeće Web servis u radnoj knjizi

1. Otvorite [oglednu datoteku programa Excel](http://aka.ms/amlexcel-sample-2), koji sadrži dodatka programa Excel i podatke o passengers na na Titanic.
2. Odaberite web-servisa tako da ga kliknete-"Titanic Survivor Predictor (dodatak programa Excel uzorka) [rezultat]" u ovom primjeru.

    ![Odaberite web-servisa][01]

3. Time ćete doći do odjeljka **Predict** .  Ova radna knjiga već sadrži ogledne podatke, ali za od prazne radne knjige možete odabrati ćelije u programu Excel i kliknite **Korištenje oglednih podataka**.
4. Odaberite podatke sa zaglavljima i kliknite ikonu raspon ulaznih podataka.  Provjerite je li potvrđen okvir "Moji podaci imaju zaglavlja".
5. U odjeljku **Izlazni**unesite broj ćelija na mjesto na kojem želite da se izlazne podatke da bi se, primjerice "H1" u nastavku.
6. Kliknite **predviđanje**.

    ![Predviđanje sekcije][02]

## <a name="steps-to-add-a-new-web-service"></a>Korake za dodavanje novog web-servisa

Implementacija web-servisa ili koristite postojeću web-servisa. Dodatne informacije o implementaciji web-servisa, potražite u članku [vodič korak 5: Implementacija servisa Azure strojnog učenja Web](machine-learning-walkthrough-5-publish-web-service.md).

Pronađite ključ za API-JA za web-servisa. Gdje su vam to ovisi o tome jeste li objavili klasični strojnog učenja web-servisa pružanja usluge za novo računalo učenje web-mjesto.

**Pomoću klasične web-servisa** 

1. U strojnog učenja Studio odjeljku **Web-SERVISI** u lijevom oknu kliknite, a zatim web-servisa.

    ![Odaberite Studio web-servisa][04]

2. Kopirajte ključ za API-JA za web-servisa.

    ![Ključ za API Studio][05]

3. Na kartici **nadzorne PLOČE** za web-servisa kliknite vezu **ZAHTJEVA i odgovora** .
4. Potražite sekciji **URI zahtjev** .  Kopiranje i spremite URL-a.

>[AZURE.NOTE] Sada je moguće prijaviti na portal za [Azure strojnog učenja Web Services](https://services.azureml.net) da biste dobili ključ za API-JA za klasični strojnog učenja web-servisa.

**Korištenje novog web-servisa**

1. Na portalu za [Azure strojnog učenja web-servisi](https://services.azureml.net) kliknite **Web-servisi**, a zatim odaberite web-servisa. 
2. Kliknite **Korištenje**.
3. Potražite u odjeljku **Osnovni potrošnje informacije** . Kopiranje i spremanje **Primarni ključ** i URL ADRESE za **Odgovor na zahtjev** .


## <a name="steps-to-add-a-new-web-service"></a>Korake za dodavanje novog web-servisa

1. Implementacija web-servisa ili koristite postojeću web-servisa. Dodatne informacije o implementaciji web-servisa, potražite u članku [vodič korak 5: Implementacija servisa Azure strojnog učenja Web](machine-learning-walkthrough-5-publish-web-service.md).
2. Kliknite **Korištenje**.
3. Potražite u odjeljku **Osnovni potrošnje informacije** . Kopiranje i spremanje **Primarni ključ** i URL ADRESE za **Odgovor na zahtjev** .
2. U programu Excel, prijeđite na odjeljak **Web Services** (Ako ste u odjeljku **Predict** , kliknite strelicu natrag za povratak na popis web-servisi).

    ![Idite na odabir servisa za Web][03]
    
3. Kliknite **Dodavanje web-servisa**.
4. Zalijepite URL u okvir tekst za dodatak programa Excel s natpisom **URL-a**.
5. Zalijepite API-primarni ključ u tekstni okvir s natpisom **ključ za API-JA**.
6. Kliknite **Dodaj**.

    ![URL i API ključ za klasične web-servisa.][06]

10. Da biste koristili web-servisa, slijedite upute za prethodni, "Korake da biste koristili na postojeće web-servisa".

## <a name="sharing-your-workbook"></a>Zajedničko korištenje radne knjige

Ako spremite radnu knjigu, a zatim API-primarni ključ za web-usluge koje ste dodali također će se spremiti. To znači da treba samo zajedničko korištenje radne knjige s osobama koje vam šalju pouzdane.

Postavite pitanja u sljedećem odjeljku komentara ili na našem [forum](http://go.microsoft.com/fwlink/?LinkID=403669&clcid=0x409).

[01]: ./media/machine-learning-excel-add-in-for-web-services/image1.png
[02]: ./media/machine-learning-excel-add-in-for-web-services/image2.png
[03]: ./media/machine-learning-excel-add-in-for-web-services/image3.png
[04]: ./media/machine-learning-excel-add-in-for-web-services/image4.png
[05]: ./media/machine-learning-excel-add-in-for-web-services/image5.png
[06]: ./media/machine-learning-excel-add-in-for-web-services/image6.png
