<properties 
    pageTitle="Vodič za razvojne inženjere Azure državne" 
    description="Ovo omogućuje comparision značajki i upute na razvoj aplikacija za državne ustanove Azure" 
    services="" 
    cloud="gov"
    documentationCenter="" 
    authors="Joharve2" 
    manager="Chrisnie" 
    editor=""/>

<tags 
    ms.service="multiple" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.tgt_pltfrm="na" 
    ms.workload="azure-government" 
    ms.date="10/29/2015" 
    ms.author="jharve"/>


#  <a name="microsoft-azure-government-developer-guide"></a>Vodič za razvojne inženjere za državne ustanove Microsoft Azure 

<p> Microsoft Azure državne je na fizički i mreže Izolirani instanci programa Microsoft Azure.  Vodič za razvojne inženjere će dati pojedinosti o razlikama između tog razvojnim inženjerima i administratori potrebni za interakciju i rad s ove zasebna područja Azure.

<!--Table of contents for topic, the words in brackets must match the heading wording exactly-->


## <a name="in-this-topic"></a>U ovoj temi


+ [Pregled](#Overview)
+ [Upute za razvojne inženjere](#Guidance)
+ [Značajke koje su trenutno dostupne u programu Microsoft Azure državne](#Features)
+ [Mapiranje krajnje točke](#Endpoint)
+ [Daljnji koraci](#next)


## <a name="Overview"></a>Pregled

Microsoft Azure državne je zasebna instanca servisa Microsoft Azure adresiranja sigurnost i usklađenost potrebama Savezna agencije SAD-a, stanja i lokalne vlade i njihova rješenja davatelje usluga. Azure državne nudi fizičke i mreže odvajanja s implementacijama vlade SAD-a i njihovi screened osoblje SAD-a. 

Microsoft pruža velik broj alata za stvaranje i implementaciju aplikacija oblaka globalni servisa Microsoft Azure ("globalni servis") i Microsoft Azure državne usluge tvrtke Microsoft.

Prilikom stvaranja i implementacija aplikacije za državne ustanove servisa Azure, umjesto servis globalni razvojnim inženjerima morate znati ključne razlike dvaju servisa.  Konkretno oko postavljanju i konfiguriranju svoje okruženje za programiranje, konfiguriranje krajnje točke, aplikacije za pisanje i implementacija ih kao servisa Azure državne.

Informacije u ovom dokumentu nalaze te razlike i nadopunjuju informacije dostupne na [Državne Azure](http://www.azure.com/gov "Azure državne") web-mjesta i sustava [Microsoft Azure biblioteka tehničkih podataka o](http://msdn.microsoft.com/cloud-app-development-msdn "MSDN") na MSDN-u. Službeni informacije mogu biti dostupne i više mjesta kao što su na [Microsoft Azure Trust Center] (https://azure.microsoft.com/support/trust-center/ "Microsoft Azure Trust Center" /), [Centar dokumentaciju za Azure](https://azure.microsoft.com/documentation/) i [Azure blogovi] (https://azure.microsoft.com/blog/ "Azure blogovi" /). 

Ovaj sadržaj namijenjen partnera i razvojni inženjeri koji su za implementaciju u Microsoft Azure državne.



## <a name="Guidance"></a>Upute za razvojne inženjere
Budući da se većina tehničke sadržaja koji je dostupan trenutno pretpostavlja da aplikacije su razvijaju servisa za globalnu umjesto za Microsoft Azure Government, važno je za vas da bi bili razvojnim inženjerima svjesni ključne razlike aplikacija projektirane tako da se nalaziti u državne Azure.

- Najprije postoje servisa i razlike u značajkama, to znači da određene značajke koje nisu u određene dijelove globalni servis možda neće biti dostupni u državne Azure.

- Drugo, za značajke koje se nude na državne Azure, postoje razlike konfiguracije globalnih servisa.  Dakle, trebali biste Pregledajte uzorak koda, konfiguracije i korake da biste bili sigurni da su Sastavljanje i izvršava unutar okruženje za servise u Oblaku državne Azure.


## <a name="Features"></a>Značajke koje su trenutno dostupne u programu Microsoft Azure državne
Azure državne trenutno sadrži sljedeći servisi u NAM GOV IOWA i VIRGINIA NAM GOV regijama:

- Virtualnim strojevima
- Servisi u oblaku
- Prostor za pohranu
- Active Directory
- Raspored
- Virtualne mreže
- SQL baze podataka
- Azure datoteke
- Media Services.
- Upravitelj promet
- Servis Bus
- StorSimple
- Redis predmemorije
- Azure sigurnosnog kopiranja
- Automatizacija
- ExpressRoute
- itd.

Dostupne su druge servise i druge usluge dodat će se na temelju neprekinuti.  Najnoviji popis servisa potražite u članku [područja stranice](https://azure.microsoft.com/regions/#services) na kojoj će istaknuti svaku regiju dostupne i njihovih usluga.  

Trenutno NAM Iowa GOV i NAM Virginia GOV su podrške Azure državne centre za podatke.  Pogledajte područja stranice iznad za trenutni centre za podatke i usluge dostupne.

## <a name="Endpoint"></a>Mapiranje krajnje točke

U sljedećoj su tablici pomoću će vas voditi kroz prilikom mapiranja javno web-mjesto Microsoft Azure i u okvir za baze podataka SQL krajnje točke za Azure državne određene krajnje točke.


Vrsta servisa|Azure javnosti|Azure državne ustanove
---|---|---
Portal za upravljanje|Manage.windowsazure.com|Manage.windowsazure.us
Općenito|*. windows.net|*. usgovcloudapi.net
Temeljni|*. core.windows.net|*. core.usgovcloudapi.net
Izračun|*. cloudapp.net|*. usgovcloudapp.net
Spremište blobova platforme|*. blob.core.windows.net|   *. blob.core.usgovcloudapi.net
Red čekanja za pohranu|*. queue.core.windows.net|*. queue.core.usgovcloudapi.net
Spremište tablica|*. table.core.windows.net|*. table.core.usgovcloudapi.net
Upravljanje servisom|Management.Core.Windows.NET|Management.Core.usgovcloudapi.NET
SQL baze podataka|*. database.windows.net|*. database.usgovcloudapi.net
Učitavanje ARM raspoređen krajnje točke|https://Management.Windows.NET|https://Management.usgovcloudapi.NET  

* Za ARM provjeru autentičnosti putem Azure AD provjerite upućuju [Provjere autentičnosti zahtjeva resursima za Azure](https://msdn.microsoft.com/library/azure/dn790557.aspx)

## <a name="next"></a>Daljnji koraci

Ako vas zanima učenje dodatne informacije i o Azure državne provjerite pod utjecajem neke veze u nastavku.

- **[Prijavite se za probnu verziju](https://azuregov.microsoft.com/trial/azuregovtrial)**

- **[Dobivanje i pristupanje državne Azure](http://azure.com/gov)**

- **[Pregled Azure državne ustanove](/azure-government-overview)**

- **[Azure državne bloga](http://blogs.msdn.com/b/azuregov/)**

- **[Azure usklađenosti](https://azure.microsoft.com/support/trust-center/compliance/)**

<!--Anchors-->



<!-- Images. -->

[1]: ./media/azure-government-developer-guide/publisherguide.png


<!--Link references-->
[Link 1 to another azure.microsoft.com documentation topic]: virtual-machines-windows-hero-tutorial.md
[Link 2 to another azure.microsoft.com documentation topic]: web-sites-custom-domain-name.md
[Link 3 to another azure.microsoft.com documentation topic]: storage-whatis-account.md
