<properties
    pageTitle="Korištenje web-servisa za strojno učenje s predloškom web app | Microsoft Azure"
    description="Pomoću web-aplikacije predložak u Azure Marketplace zauzeti predvidljivu web-servisa u Azure strojnog učenja."
    keywords="web-servisa, operationalization, REST API-JA, računala učenje"
    services="machine-learning"
    documentationCenter=""
    authors="garyericson"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="machine-learning"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/10/2016"
    ms.author="garye;raymondl"/>

# <a name="consume-an-azure-machine-learning-web-service-with-a-web-app-template"></a>Korištenje Azure strojnog učenja web-servisa s predloškom web app

>[AZURE.NOTE] U ovoj se temi opisuju tehnike odnosi se na klasične web-servisa. 

Kad ste razvio predvidljivu modela i implementirati kao Azure web servis pomoću strojnog učenja Studio ili pomoću alata kao što su R ili Python, možete pristupiti operationalized modela pomoću REST API-JA.

Postoji nekoliko načina za korištenje REST API-JA i pristup web-servisa. Ako, na primjer, možete pisati aplikaciju C#, R ili Python pomoću koda za uzorka za koje generira kada implementiran (dostupno na API stranici pomoć nadzornoj ploči web servisa u strojnog učenja Studio) web-servisa. Ili možete koristiti oglednoj radnoj knjizi programu Microsoft Excel stvara (dostupna nadzornoj ploči web servisa u Studio).

Dok je najbrži i Najlakši način za pristup web-servisa putem predlošci aplikacija za Web u [Azure Web App trgovine](https://azure.microsoft.com/marketplace/web-applications/all/).

[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

## <a name="the-azure-machine-learning-web-app-templates"></a>Azure strojnog učenja web-aplikacije predložaka

Web app predložaka dostupnih u Azure Marketplace možete izraditi prilagođene web-lokacije koje zna ulaznih podataka i očekivane rezultate web-servisa. Sve što trebate napraviti je dati web app pristup web-servisa i podataka, a predložak ne ostale.

Dostupne su dvije predložaka:

- [Predložak za aplikaciju Web servisa Azure ML zahtjeva i odgovora](https://azure.microsoft.com/marketplace/partners/microsoft/azuremlaspnettemplateforrrs/)
- [Grupe za Azure ML izvođenja servisa web-aplikacije predloška](https://azure.microsoft.com/marketplace/partners/microsoft/azuremlbeswebapptemplate/)

Svaki predložak stvoriti aplikaciju uzorka ASP.NET, pomoću URI API-JA i ključ za web-servisa i uvodi kao web-mjesto za Azure. Predložak za odgovor na zahtjev servisa (RRS) stvara web-aplikacije koje vam omogućuje slanje jedan redak podataka na web-uslugu da biste dobili jedan rezultat. Predložak za obradu izvođenja servisa (BES) stvara web-aplikacije koje vam omogućuje slanje više redaka podataka da biste dobili više rezultata.

Nema kodiranje potreban je za korištenje ove predloške. Samo dohvaćati URI API-JA i ključ i predložak sastavlja aplikacije za vas.

## <a name="how-to-use-the-request-response-service-rrs-template"></a>Kako koristiti predložak za odgovor na zahtjev servisa (RRS)

Kada ste implementiran web-servisa, možete slijedite korake u nastavku da biste koristili predložak RRS web app, kao što je prikazano na sljedećem su dijagramu.

![Postupak da biste pomoću predloška web-RRS][image1]

1. U strojnog učenja Studio, otvorite karticu **Web-servisi** , a zatim otvorite web-servisa na kojoj želite pristupiti. Kopiranje ključa na popisu u odjeljku **ključ za API-JA** i spremite ga.

    ![Ključ za API-JA][image3]

2. Otvorite stranicu pomoć **ZAHTJEVA i odgovora** API-JA. Pri vrhu stranice pomoći, u odjeljku **zahtjev**, kopirajte vrijednost **Zahtjev URI** i spremite ga. Ta vrijednost izgledat će ovako:

        https://ussouthcentral.services.azureml.net/workspaces/<workspace-id>/services/<service-id>/execute?api-version=2.0&details=true

    ![URI zahtjeva][image4]

3. Idite na [portal za Azure](https://portal.azure.com), **prijavu**, kliknite **Novo**, traženje i odabir **Azure ML odgovor na zahtjev servisa web-aplikacije**, a zatim kliknite **Stvori**. 

    - Dajte web-aplikaciju programa jedinstven naziv. URL web-aplikaciji bit će taj naziv slijedi `.azurewebsites.net.` , na primjer,`http://carprediction.azurewebsites.net.`

    - Odaberite Azure pretplate i usluge na kojem se izvodi web-servisa.

    - Kliknite **Stvori**.

    ![Stvaranje web-aplikacije][image5]

4. Po završetku Azure implementacija web-aplikaciju, kliknite **URL** koji se na stranici Postavke web-aplikacije u Azure ili unesite URL web-preglednika. Na primjer,`http://carprediction.azurewebsites.net.`

5. Prilikom prvog pokretanja web-aplikaciji je zatražit će **URL ADRESU objave API -JA** i **Ključ za API -JA**.
Unesite vrijednosti koju ste ranije spremili:
    - **Zahtjev za URI** na stranici pomoć API-JA za **objavu API URL-a**
    - **Ključ za API** servisa nadzornoj ploči web **Ključ za API -JA**.

    Kliknite **Pošalji**.

    ![Unesite objavu URI i ključ za API-JA][image6]

6. Web-aplikaciji prikazuje njezinu stranicu **Web-aplikacije konfiguracija** s trenutne postavke web-servisa. Ovdje možete unijeti promjene postavki koriste web-aplikaciji.

    > [AZURE.NOTE] Promjena postavki ovdje samo mijenja ih za ovo web-aplikacije. Ga ne promijenite zadane postavke web-servisa. Ako, na primjer, ako promijenite **Opis** ga ne mijenja opis koji se prikazuju na nadzornoj ploči servis za web u Studio strojnog učenja.

    Kada završite, kliknite **Spremi promjene**, a zatim kliknite **Idi na početnu stranicu**.

7. Na početnoj stranici možete unijeti vrijednosti da biste poslali web-servisa kliknite **Pošalji**, a vratit će se rezultat.

Ako želite da biste se vratili na stranicu **Konfiguriranje** , idite na `setting.aspx` stranice web-aplikacije. Na primjer: `http://carprediction.azurewebsites.net/setting.aspx.` koji će se zatražiti da ponovno unesite ključ za API - morate koji da biste pristupili stranici i ažurirajte postavke.

Možete zaustaviti, ponovno pokrenite i brisanje web-aplikacije na portalu za Azure kao što je web app. Sve dok se izvodi možete pregledavati na početnom web-adresu i unesite nove vrijednosti.

## <a name="how-to-use-the-batch-execution-service-bes-template"></a>Kako koristiti predložak grupe za izvršavanje usluge (BES)

Možete koristiti BES web-aplikacije predloška na isti način kao predložak RRS osim što web-aplikacije koje se stvara omogućuje više redaka podataka za slanje i primanje više rezultata.

Rezultati iz serije izvođenja web-servisa spremaju se u spremniku programa Azure prostora za pohranu; unos vrijednosti mogu potjecati iz Azure prostora za pohranu ili lokalna datoteka.
Tako, morat ćete je spremnik Azure prostora za pohranu da biste držite rezultata vratio web-aplikaciji, a morat ćete pripremiti ulaznih podataka.

![Postupak da biste pomoću predloška web-BES][image2]

1. Slijedite isti postupak da biste stvorili web-aplikaciji BES kao predložak RRS osim:
    - Pronađite **Zahtjev URI** iz **Serije IZVOĐENJA** API stranici pomoć za web-servisa.
    - Idite na [Azure ML obradu izvođenja servisa web-aplikacije predložak](https://azure.microsoft.com/marketplace/partners/microsoft/azuremlbeswebapptemplate/) da biste otvorili predložak BES na Azure Marketplace i kliknite **Stvaranje web-aplikacija**.

2. Da biste odredili mjesto na kojem želite da se rezultati spremljena, unesite informacije spremnik odredište na početnoj stranici web app. Navesti gdje web-aplikaciji pronaći ulazne vrijednosti, u lokalnu datoteku ili je spremnik Azure prostora za pohranu.
Kliknite **Pošalji**.

    ![Informacija za pohranu][image7]

Web-aplikaciji prikazat će se stranica sa statusom zadatka.
Kada je zadatak dovršen će biti zadano mjesto rezultate u spremište blobova platforme Azure. Imate mogućnost preuzimanja rezultate u lokalnu datoteku.

## <a name="for-more-information"></a>Dodatne informacije

Da biste saznali više o...

- Stvaranje strojnog učenja eksperiment s strojnog učenja Studio, potražite u članku [Stvaranje vaš prvi eksperiment u Azure strojnog učenja Studio](machine-learning-create-experiment.md)

- Kako implementirati učenje strojno Poigrajte se kao web-servisa potražite u članku [uvođenja web-servisa Azure strojnog učenja](machine-learning-publish-a-machine-learning-web-service.md)

- drugi načini pristupa web-servisa potražite [u](machine-learning-consume-web-services.md) članku korištenje u web-servisa Azure strojnog učenja


[image1]: media\machine-learning-consume-web-service-with-web-app-template\rrs-web-template-flow.png
[image2]: media\machine-learning-consume-web-service-with-web-app-template\bes-web-template-flow.png
[image3]: media\machine-learning-consume-web-service-with-web-app-template\api-key.png
[image4]: media\machine-learning-consume-web-service-with-web-app-template\post-uri.png
[image5]: media\machine-learning-consume-web-service-with-web-app-template\create-web-app.png
[image6]: media\machine-learning-consume-web-service-with-web-app-template\web-service-info.png
[image7]: media\machine-learning-consume-web-service-with-web-app-template\storage.png
