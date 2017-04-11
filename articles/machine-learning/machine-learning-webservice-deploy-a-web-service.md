<properties
   pageTitle="Implementacija nove web-usluge"
   description="Tijek rada implementacije u OBLAK temelji web-servisa"
   services="machine-learning"
   documentationCenter=""
   authors="vDonGlover"
   manager="raymondl"
   editor=""/>

<tags
    ms.service="machine-learning"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/04/2016"
    ms.author="v-donglo"/>

# <a name="deploy-a-new-web-service"></a>Implementacija nove web-usluge

Microsoft Azure strojnog učenja sada sadrži web-servisi koji se temelje na [Voditelj resursa Azure](../azure-resource-manager/resource-group-overview.md) čime se omogućuje nove mogućnosti tarife za naplatu i implementacija web-servisa na više područja.

Općenito tijek rada za implementaciju web-servisa pomoću web-servisi za Microsoft Azure strojnog učenja je:

* Stvaranje predvidljivu eksperiment
* njegove implementacije
* Konfiguriranje njegov naziv
* plan za naplatu
* Testiranje je
* korištenje.

Sljedeća grafika prikazuje tijek rada.

![Tijek rada za implementaciju servisa za web][1]
 
## <a name="deploy-web-service-from-studio"></a>Implementacija web-servisa iz Studio 

Za implementaciju pokusa kao novog web-servisa. Prijavite se u učenje Studio stroj, a zatim stvorite novi predvidljivu web-servisa. 

**Napomena**: Ako ste već implementiran pokusa kao klasične web-servisa nije moguće je uvesti kao novog web-servisa.
 
Kliknite **Pokreni** pri dnu područja crtanja eksperiment, a zatim **Implementacija web-servisa** i **Implementacija web-servisa [novi]**. Otvorit će se stranica implementacije upravitelja strojnog učenja web-servisa.

## <a name="machine-learning-web-service-manager-deploy-experiment-page"></a>Upravitelj servisa za strojno učenje Web implementacija eksperiment stranice
Na stranici uvođenje eksperiment unesite naziv web-servisa.
Odaberite plan cijene. Ako imate postojeću cijene za tarifu možete je odabrati, u suprotnom morate stvoriti novi cijena plan za servis. 

1.  **Planiranje cijena** padajući popis, odaberite postojeći plan ili odaberite mogućnost **Odaberite novu tarifu** .
2.  U odjeljak **Naziv plana**upišite naziv koji će prepoznavanje plan na računu.
3.  Odaberite jednu od **Mjesečni Plan razine**. Imajte na umu da su zadane razine plan na tarife za zadane regije i web-servisa implementiran na to područje.

Kliknite **uvođenja** i stranicu za brzi početak rada za web usluge će se otvoriti.

## <a name="quickstart-page"></a>Stranica za brzi početak rada
Web-stranicu za brzo pokretanje servisa omogućuje pristup i upute na uobičajene zadatke koje će se izvršiti nakon stvaranja novog web-servisa. Na tom mjestu možete jednostavno pristupiti **probna** stranica i stranica **utrošak** .

## <a name="testing-your-web-service"></a>Testiranje web-servisa

Na stranici za brzo pokretanje kliknite servis za testiranje web u odjeljku uobičajeni zadaci.   

Da biste testirali web-servisa kao na odgovor na zahtjev servisa (RRS):

* Na traci izbornika kliknite **Test** .
* Kliknite **Odgovor na zahtjev**.
* Unesite odgovarajuće vrijednosti za unos stupce vaše eksperimenta.
* Kliknite Testiraj **Odgovor na zahtjev**.

Koje rezultati će se prikazivati na desnoj strani stranice.

Da biste testirali web-servisa za obradu izvođenja servisa (BES), koristite CSV datoteke:

* Na traci izbornika kliknite **Test** .
* Kliknite **grupe**.
* U odjeljku unos, kliknite Pregledaj, a zatim dođite do svoje datoteke s oglednim podacima.
* Kliknite **Testiraj**.

U odjeljku **Testiranje obrade**prikazuje se status testiranju.

## <a name="consuming-your-web-service"></a>Koristi web-servisa

Kada je uveden kao web-servisa, Azure strojnog učenja eksperimenata pružaju REST API-JA koje možete troše širok raspon uređaja i platformama. To je zato jednostavnih REST API-JA priznaje i Odgovori s JSON oblikovane poruke. Portal za Azure strojnog učenja predstavlja kod koji se mogu koristiti da biste nazvali web-servisa u R, C# i Python.
 
Na stranici potrošnja možete pronaći:

* Ključ za API-JA i URI korisnika za koje koriste web-servisa u aplikacijama.
* Excel web app predloške i da biste pokrenuli pokrenuti proces potrošnje.
* Ogledni kod u C#, python i R za početak.

Dodatne informacije o drugim web-servisi, potražite u članku [upute za korištenje u web-servisa Azure strojnog učenja koji je uveden iz strojnog učenja eksperiment](machine-learning-consume-web-services.md).

## <a name="next-steps"></a>Daljnji koraci

Dodatne informacije o drugim web-servisi, potražite u članku:

[Upute za korištenje programa Azure strojnog učenja web-servisa koji je uveden iz strojnog učenja eksperiment](machine-learning-consume-web-services.md)

[Azure strojnog učenja web-servisi: Uvođenje i potrošnje](machine-learning-deploy-consume-web-service-guide.md)

<!--Image references-->
[1]: ./media/machine-learning-webservice-deploy-a-web-service/armdeploymentworkflow.png


<!--links-->
