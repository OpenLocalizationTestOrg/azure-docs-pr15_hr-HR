<properties
   pageTitle="Promjena veličine web-servisa | Microsoft Azure"
   description="Saznajte kako skaliranje web-servisa povećanje istodobnosti i dodavanjem nove krajnje točke."
   services="machine-learning"
   documentationCenter=""
   authors="neerajkh"
   manager="srikants"
   editor="cgronlun"
   keywords="Azure strojnog učenja web-servisi operationalization, skaliranja krajnje točke, istodobnosti"
   />
<tags
   ms.service="machine-learning"
   ms.devlang="NA"
   ms.workload="data-services"
   ms.tgt_pltfrm="na"
   ms.topic="article"
   ms.date="10/05/2016"
   ms.author="neerajkh"/>

# <a name="scaling-a-web-service"></a>Promjena veličine web-servisa

>[AZURE.NOTE] U ovoj se temi opisuju tehnike odnosi se na klasični strojnog učenja web-servisa. 

Prema zadanim postavkama, svaki objavljenu web-servisa nije konfiguriran za podršku 20 istovremeni zahtjevi i mogu biti najviša 200 istovremeni zahtjevi. Tijekom portala za Azure klasični omogućuje tu vrijednost postavite, Azure strojnog učenja automatski optimizira postavku za pružanje najbolje performanse za web-servisa i portala vrijednost zanemaruje. 

Ako će podržavati plan da biste uputili poziv API-JA s opterećenjem veća od vrijednosti maksimum Istodobni pozive 200, potrebno je stvoriti više krajnje točke na istom web-servisa. Možete zatim slučajno distribuirati vaše učitavanja preko svih od njih.

## <a name="add-new-endpoints-for-same-web-service"></a>Dodavanje novog krajnje točke za istu web-servisa

Promjena veličine web-servisa nije uobičajenih zadataka. Razlozi za promjenu veličine su podržava više od 200 istovremeni zahtjevi, povećati dostupnost kroz više krajnje točke ili nudi zaseban krajnje točke za web-servisa. Možete povećati skale dodavanjem dodatnih krajnje točke za istu web-servisa [Azure klasični portal](https://manage.windowsazure.com/) ili portala za [Azure strojnog učenja web-servisa](https://services.azureml.net/) .

Dodatne informacije o dodavanju novog krajnje točke potražite u članku [Stvaranje krajnje točke](machine-learning-create-endpoint.md).

Imajte na umu da pomoću visoke istodobnosti broj može biti detrimental ako ne zovete API-JA s artiklima visoka stopa. Možete vidjeti povremeni gubitak vremenska ograničenja i/ili krivina u latenciju ako odgoditi relativno premalo opterećenje na API konfigurirana za visoke Učitaj.

Slika API-ji obično se koriste u slučajevima gdje željenu niske latencije. Latencija ovdje podrazumijeva vrijeme potrebno za API da biste dovršili jedan zahtjev, a ne računa za sve kašnjenja mreže. Recimo da imate API-JA s 50 ms Latencija. Da biste potpuno zauzeti dostupan kapacitet razinom ograničenja visoke i Max Istodobni pozive = 20, morate nazvati API 20 * 1000 / 50 = 400 puta sekundi. Proširivanje to Dodatno, Max Istodobni pozive 200 omogućuje upućivanje poziva puta API 4000 sekundi, pod pretpostavkom 50 ms Latencija.

<!--Image references-->
[1]: ./media/machine-learning-scaling-webservice/machlearn-1.png
[2]: ./media/machine-learning-scaling-webservice/machlearn-2.png
