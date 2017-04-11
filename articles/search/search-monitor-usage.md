<properties 
   pageTitle="Nadzor korištenja te statistike u servis za pretraživanje Azure | Microsoft Azure | Servis za pretraživanje glavnom računalu oblaka" 
   description="Praćenje potrošnje i indeksa veličina resursa za Azure pretraživanje, na glavnom računalu pretraživanja u oblaku na Microsoft Azure." 
   services="search" 
   documentationCenter="" 
   authors="HeidiSteen" 
   manager="jhubbard" 
   editor=""
   tags="azure-portal"/>

<tags
   ms.service="search"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="required" 
   ms.date="05/17/2016"
   ms.author="heidist"/>

# <a name="monitor-usage-and-statistics-in-an-azure-search-service"></a>Nadzor korištenja te statistike u servis za pretraživanje Azure

Rast indekse i veličinu dokumenta za praćenje može vam pomoći pri prilagoditi kapaciteta prije odlazak gornju granicu uspostavite servisa. 

Da biste pratili korištenje resursa, broji te statistike servisa jednostavno prikazuju na [Portal za Azure](https://portal.azure.com), ali možete dobiti informacije programski ako izrađujete alat za administraciju prilagođene servisa. U ovom se članku objašnjava postupak za obje tehnike.

Nova značajka analize promet za pretraživanje možete koristiti i za uvida u aktivnosti na razini indeksa. Posjetite [Pretraživanje promet analize Azure pretraživanja](search-traffic-analytics.md) za početak rada.

##<a name="view-counts-and-metrics-in-the-portal"></a>Prikaz brojanja i metrike na portalu 

1. Prijava na [Portal za Azure](https://portal.azure.com). 

2. Otvorite nadzorna ploča službe za servis za pretraživanje Azure. Pločice za servis možete pronaći na početnoj stranici ili možete pregledavati na servis iz Pregledaj na na JumpBar. Detaljne upute potražite u članku [Stvaranje servisa](search-create-service-portal.md) .

U odjeljku korištenje obuhvaća metar koja kaže da su trenutno koristi koji dio dostupnih resursa.

  ![][1]

Opoziv ima li zajedničke usluge najviše jedan replike i svaki particije. Osim toga, možete samo podržava 10 000 dokumenata u zbroj ili 50 MB podataka, ovisno o tome što dolaze prvi.

##<a name="get-index-statistics-using-the-rest-api"></a>Početak indeksa Statistički podaci o korištenju REST API-JA

Azure pretraživanje REST API-JA i .NET SDK pružaju programski pristup metriku servisa.  Ako koristite [indexers](https://msdn.microsoft.com/library/azure/dn946891.aspx) za učitavanje indeksa s bazom podataka SQL Azure ili DocumentDB, a zatim dodatne API dostupna da biste dobili brojeve koji su vam potrebne. 

  + [Početak Statistika indeksa](https://msdn.microsoft.com/library/azure/dn798942.aspx)
  + [Broj dokumenata](https://msdn.microsoft.com/library/azure/dn798924.aspx)
  + [Dohvatite Status indeksiranje](https://msdn.microsoft.com/library/azure/dn946884.aspx)

## <a name="next-steps"></a>Daljnji koraci

Pregledajte [ograničenja i kapacitet](search-limits-quotas-capacity.md) da biste odredili kombinacije particije i replike morat ćete ako ima kapacitet za postojeće dovoljno. 

[Upravljanje servisa za pretraživanje na Microsoft Azure](search-manage.md) potražite dodatne informacije o za administraciju servisa.

<!--Image references-->
[1]: ./media/search-monitor-usage/AzureSearch-Monitor1.PNG




 
