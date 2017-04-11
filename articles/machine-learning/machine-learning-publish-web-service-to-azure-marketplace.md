<properties 
    pageTitle="Objavljivanje strojnog učenja web-servisu Azure Marketplace | Microsoft Azure" 
    description="Kako objaviti na web-servisa Azure strojno učenje Azure Marketplace" 
    services="machine-learning" 
    documentationCenter="" 
    authors="BharathS" 
    manager="jhubbard" 
    editor="cgronlun"/>

<tags 
    ms.service="machine-learning" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/08/2016" 
    ms.author="bharaths"/>

# <a name="publish-azure-machine-learning-web-service-to-the-azure-marketplace"></a>Objavljivanje Azure strojnog učenja web-servisa Azure Marketplace 

Trgovina Azure omogućuje objavljivanje web-servisa Azure strojnog učenja plaćeno ili oslobodite servise za potrošnju po vanjske korisnike. Ovaj članak sadrži pregled tog procesa s vezama na upute koje će vam pri prvim koracima. Pomoću ovog postupka možete učiniti web-servisi dostupnim za druge inženjerima omogućuje korištenje poslovne inteligencije u svojim aplikacijama.


[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

## <a name="overview-of-the-publishing-process"></a>Pregled postupka objavljivanja 

Slijede upute za objavljivanje na web-servisa Azure strojnog učenja Azure Marketplace:

1. Stvaranje i objavljivanje servis strojnog učenja-odgovor na zahtjev (RRS)
2. Implementacija servis radnog i nabavite podatke ključ za API-JA i OData krajnjoj točki.
3. Pomoću URL-objavljenu web-servisa za objavljivanje [Azure](https://publish.windowsazure.com/workspace/) Marketplace (Data Market) 
4. Kad poslan, pregledava se tu ponudu i mora odobriti prije klijentima možete pokrenuti kupnju ga. Objavljivanje postupak može potrajati nekoliko dana tvrtke. 

## <a name="walk-through"></a>Prođite
###<a name="step-1-create-and-publish-a-machine-learning-request-response-service-rrs"></a>Korak 1: Stvaranje i objavljivanje servis strojnog učenja-odgovor na zahtjev (RRS)###
 Ako niste to već, provjerite pogledajte ovaj [voditi kroz](machine-learning-walkthrough-5-publish-web-service.md).

###<a name="step-2-deploy-the-service-to-production-and-obtain-the-api-key-and-odata-endpoint-information"></a>Korak 2: Implementacija servis radnog i dobili ključ za API-JA i informacija o krajnjoj točki OData###
1. [Klasični Portal Azure](http://manage.windowsazure.com)s na lijevoj navigacijskoj traci odaberite mogućnost **STROJNOG UČENJA** , te odaberite radnog prostora. 

2. Na kartici **Web-SERVISI** kliknite pa odaberite web-servisa na koje želite objaviti na tržištu.

    ![Azure Marketplace][workspace]

3. Odaberite krajnju točku želite imati trgovine zauzeti. Ako niste stvorili sve dodatne krajnje točke, možete odabrati **zadano** krajnje točke.

4. Nakon što ste kliknuli na krajnjoj, moći da biste vidjeli **KLJUČ za API -JA**. Morat ćete ovaj podatak o kasnije u koraku 3, tako da napravite njezinu kopiju.

    ![Azure Marketplace][apikey]

5. Kliknite metodu **ZAHTJEVA i odgovora** u ovoj fazi ne podržavamo objavljivanje obradu izvođenja servisa na tržištu. Tako ćete doći na stranici pomoć API-JA za metodu zahtjeva i odgovora.

6. Kopirajte **Adresu krajnjoj točki OData**, koji će vam informacije biti potrebne kasnije u koraku 3.

    ![Azure Marketplace][odata]




Uvođenje servisa u radnog.



###<a name="step-3-use-the-url-of-the-published-web-service-to-publish-to-azure-marketplace-datamarket"></a>Korak 3: Pomoću URL-objavljenu web-servisa za objavljivanje Azure Marketplace (DataMarket)###

1.  Dođite do [Azure Marketplace (Data Market)](http://datamarket.azure.com/home) 
2.  Kliknite na vezu za **Objavljivanje** pri vrhu stranice. To će vas odvesti sustava [Microsoft Azure Portal za objavljivanje](https://publish.windowsazure.com)
3.  Kliknite odjeljak **izdavači** da biste registrirali kao u programu publisher.
4.  Kada stvorite novu ponudu, odaberite **Data Services**, a zatim kliknite **Stvori novi podatkovnog servisa**. 
 
    ![Azure Marketplace][image1]

    <br />


5.  U odjeljku **tarife** pružaju informacije o ponude, uključujući plan cijene. Odlučite ako će ponuditi uslugu besplatni ili plaćeni. Da biste dobili plaćena, unesite podatke o plaćanju kao što su vaše banke i porez podatke.

6.  U odjeljku **marketinške** nalaze informacije o ponuda, kao što su naslov i opis za tu ponudu.

7.  U odjeljku **određivanje cijena** možete postaviti cijenu za planove za određene zemlje ili pustiti da sustav "autoprice" tu ponudu.

8. Na kartici **Podatkovnog servisa** kliknite **Web-servisa** kao **Izvor podataka**.

    ![Azure Marketplace][image2]

9.  Pronađite ključ na web-usluge URL i API Azure klasični, na portalu kao što je opisano u koraku gore 2.

10. U dijaloškom okviru Postavljanje trgovine podatkovnog servisa zalijepite adresu krajnjoj točki OData u tekstni okvir **URL servisa** .

11. Za **provjeru autentičnosti**, odaberite **Zaglavlje** kao **Shemu provjere autentičnosti**.

    - Unesite "Autorizacije" **naziv zaglavlja**.
    - Za **Vrijednost zaglavlja**, unesite "Nošenja" (bez navodnika), kliknite traku **prostora** i zalijepite ključ za API-JA.
    - Uključite potvrdni okvir **ovaj servis je OData** .
    - Kliknite **Testiraj vezu** da biste testirali veze.

12. U odjeljku **kategorije**, provjerite je li odabrana **Strojnog učenja** .

13. Kada završite s unosom metapodataka o ponudi, kliknite **Objavi**, a zatim **automatske pripremna**. Sada, bit ćete obaviješteni preostale problema koje su vam potrebne da biste riješili problem.

14. Nakon što ste ensured dovršenog preostala problema, kliknite **zahtjev za odobrenje da biste na radni**. Objavljivanje postupak može potrajati nekoliko dana tvrtke. 


[image1]:./media/machine-learning-publish-web-service-to-azure-marketplace/image1.png
[image2]:./media/machine-learning-publish-web-service-to-azure-marketplace/image2.png
[workspace]:./media/machine-learning-publish-web-service-to-azure-marketplace/selectworkspace.png
[apikey]:./media/machine-learning-publish-web-service-to-azure-marketplace/apikey.png
[odata]:./media/machine-learning-publish-web-service-to-azure-marketplace/odata.png
 
