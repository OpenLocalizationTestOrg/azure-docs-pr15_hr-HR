<properties
   pageTitle="Planiranje za aplikacije servisa tkanina kapaciteta | Microsoft Azure"
   description="U članku se opisuje kako prepoznati broj računalnim čvorove obavezan za usluge tkanina aplikacije"
   services="service-fabric"
   documentationCenter=".net"
   authors="mani-ramaswamy"
   manager="markfuss"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="09/14/2016"
   ms.author="subramar"/>


# <a name="capacity-planning-for-service-fabric-applications"></a>Planiranje za aplikacije servisa tkanina kapaciteta


Ovaj dokument ručica za procjenu količinu resursa (CPU-ovi, RAM-a, disk spremište) morate pokrenuti aplikacije tkanina servisa Azure. Preduvjeti za resurse da biste promijenili vremenom uobičajeno je. Nekoliko resursa obično potrebno razviti i testiranje usluge i zahtijevaju dodatne resurse, kao odlaze proizvodnje te aplikacije poveća popularnosti. Prilikom dizajniranja aplikacije, razmislite kroz Dugoročne preduvjeti i provjerite mogućnosti koje omogućuju usluge za promjenu veličine da bi odgovarao zahtjev visoke klijenta.

 Prilikom stvaranja servisa tkanina klaster odlučiti što vrste virtualnim strojevima (VMs) čine klaster. U sklopu svakog VM tijekom ograničenog vremenskog resursa u obliku CPU-ovi (jezgri i brzinu), propusnost mreže, RAM-a i prostora za pohranu na disku. Kao što je vremenom poveća na servisu, možete nadograditi VMs koje nude veći resursa i/ili dodajte dodatne VMs svoj klaster. Da biste učinili drugu mogućnost, koje morate mijenjanje arhitekture usluge prethodno tako da ga možete iskoristiti nove VMs koje se dinamički dodali klaster.

Neki servisi za upravljanje malo da nema podataka na VMs sami. Zbog toga treba za te servise planiranja kapaciteta fokus prvenstveno na performanse, što znači da odaberete odgovarajući CPU-ovi (jezgri i brzinu) od na VMs. Osim toga, trebali biste propusnost mreže, uključujući način prijenosa mreže su česta i prenose kojom količinom podataka. Ako na servisu mora poduzeti kao povećava korištenje servisa, možete dodati više VMs klaster i učitavanje saldo preko svih VMs zahtjeve za razgovore s mrežom.

Za servise koji upravljaju velikih količina podataka na na VMs, Planiranje kapaciteta trebali biste fokus prvenstveno na veličinu. Dakle, Pažljivo razmotrite kapacitet u VM RAM-a i prostora za pohranu na disku. Upravljanje virtualna memorija sustavu Windows čini prostora na disku izgledati RAM-a kod aplikacije. Uz to, runtime tkanina servisa nudi pametno broja stranica čuvanja samo najnovije podatke u memorije i premještanje Hladna podataka na disk. Aplikacije mogu koristiti stoga dodatnu memoriju od fizički dostupna je na na VM. Imate više RAM-a samo povećava se performanse, budući da u VM možete zadržati dodatnog prostora za pohranu na disku u RAM-a. VM odaberete mora imati dovoljno veliko da biste pohranili podatke koje želite na na VM disk. Isto tako, u VM mora imati dovoljno RAM-a da vam pruži performanse po volji. Ako na servisu podataka s vremenom poveća, možete dodati više VMs klaster i particija podatke preko svih VMs.

## <a name="determine-how-many-nodes-you-need"></a>Odredite koliko čvorove vam je potrebna

Particija na servisu omogućuje vam da biste skalirali podataka i na servisu. Dodatne informacije o particija, potražite u članku [Particija tkanina servisa](service-fabric-concepts-partitioning.md). Svaki particija mora stane u jednom VM, ali na jednom VM možete staviti više particija (mala). Tako, imate više particija small daje veću fleksibilnost od imate nekoliko veće particije. U trade-off je ako imate mnogo particije povećava indirektni tkanina servisa, a ne možete izvršiti operacije transakcije preko particije. Ako se nalazi i dodatne potencijalne mrežni promet kod usluge često treba pristupiti koji žive u različite particije u podatka. Prilikom dizajniranja uslugu, Pažljivo razmotrite ove argumente za i protiv da stigne na stvaranje učinkovite Strategije za stvaranje particija.

Recimo da aplikacije sadrži jednu s praćenjem stanja servisa koji je veličina spremišta očekivana radi povećanja DB_Size GB u godini. Želite dodati više aplikacije (i particije) kao što je sučelje growth izvan te godine.  Replikacija faktor (RF), koji određuje broj replike servisa utječe ukupni DB_Size. Ukupna DB_Size preko svih replike je faktor replikacije pomnožen sa DB_Size.  Node_Size predstavlja disk prostor/RAM-a po čvor koji želite koristiti za uslugu. Ostvarili najbolje performanse, na DB_Size treba uklapaju u memoriji preko klaster, a Node_Size koji je oko RAM-a na VM treba odabrati. Po dodjeljivanje Node_Size veću od kapaciteta RAM-a, koje su potrebe za oslanjanjem na broja stranica nudi runtime tkanina servisa. Dakle, performanse možda neće biti optimalnih čitavog sadržaja smatra se tipkovni (nakon toga podatke je Straničeno/izvan). Međutim, za mnoge servise gdje je tipkovni samo razlomak podatke, nije više učinkovit.

Broj čvorove potrebne za maksimalne performanse možete izračunati na sljedeći način:

```
Number of Nodes = (DB_Size * RF)/Node_Size

```


## <a name="account-for-growth"></a>Račun za growth

Možda želite izračunati broj čvorove na temelju DB_Size koji očekujete uslugu radi povećanja, osim DB_Size koja je s. Nakon toga svakodnevnim broj čvorove usluge rastom tako da ne su previše dodjeljivanje broj čvorove. No broj particije se temelji na broj čvorove koja su potrebna kada naiđete na servisu pri Maksimalna rasta.

Je dobro imati nekoliko dodatnih strojeva dostupne u bilo kojem trenutku tako da možete učiniti bilo neočekivane krivina ili (na primjer, ako nekoliko VMs prelazak prema dolje).  Dodatni kapaciteta treba određen pomoću svoje očekivani krivina, polazište je rezervirati nekoliko dodatnih VMs (5 10 posto dodatni).

Prethodni pretpostavlja jedan s praćenjem stanja servisa. Ako imate više od jedne s praćenjem stanja servisa, morate dodati DB_Size povezan s drugih servisa u jednadžbu. Osim toga, možete izračunati broj čvorove zasebno za svaku uslugu s praćenjem stanja.  Na servisu možda replike ili particije koje nisu raspoređen. Imajte na umu da particije može imati više podataka od drugih. Dodatne informacije o particija potražite u odjeljku [particija članak: najbolje prakse](service-fabric-concepts-partitioning.md). Prethodni jednadžbu je particija i replike agnostic jer je servis tkanina osigurava da na replike su širenje među čvorove optimizirana način.


## <a name="use-a-spreadsheet-for-cost-calculation"></a>Korištenje proračunske tablice za izračun troškova

Sada ćemo staviti neke realnih brojeva u formuli. [Primjer proračunska tablica](https://servicefabricsdkstorage.blob.core.windows.net/publicrelease/SF%20VM%20Cost%20calculator-NEW.xlsx) prikazuje kako planiranje kapaciteta za aplikaciju koja sadrži tri vrste podataka objekte. Za svaki objekt smo aproksimaciju njegove veličine i možemo očekivati da bi koliko objekte. Ne možemo i odaberite koliko replike želimo svake vrste objekta. Proračunska tablica izračunava ukupni iznos memorije pohraniti u klasteru.

Zatim ćemo unesite veličina VM i mjesečni trošak. Ovisno o veličini VM, proračunske tablice obavijestit će vas najmanji broj particije morate koristiti za podjelu podataka da bi se fizički stao na čvorove. Možda radu s većim brojem particije kako bi odgovarao izračuni za određenu aplikaciju sustava i mora mrežni promet. Proračunska tablica prikazuje broj particije koje su Upravljanje korisnički profil objekti sadrži povećava se s jednog šest.

Sada, ovisno o svim tim informacijama, proračunske tablice prikazuje da nije fizički dohvaćanje podataka pomoću željenog particije i replike na 26 čvor klaster. Međutim, ovaj klaster će biti densely zapakirane, da bi se trebali neke dodatne čvorove kako bi odgovarao čvor pogrešaka i nadogradnje. Proračunska tablica prikazuje i da imate više od 57 čvorove nudi nema dodatne vrijednosti jer se promijenile prazan čvorove. Ponovno, preporučujemo vam da otvorite iznad 57 čvorove ipak kako bi odgovarao čvor pogrešaka i nadogradnji. Možete dotjerati proračunsku tablicu tako da odgovara određenim potrebama vaše aplikacije.   

![Proračunske tablice za izračun troškova][Image1]



## <a name="next-steps"></a>Daljnji koraci

Pogledajte [particija tkanina servisa services] [ 10] da biste saznali više o stvaranju particija usluge.



<!--Image references-->
[Image1]: ./media/SF-Cost.png

<!--Link references--In actual articles, you only need a single period before the slash-->
[10]: service-fabric-concepts-partitioning.md
