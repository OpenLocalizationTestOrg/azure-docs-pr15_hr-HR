<properties
   pageTitle="Testiranje tu ponudu podatkovnog servisa za Marketplace | Microsoft Azure"
   description="Objašnjenje kako testirati vaše podatkovnog servisa ponuda za Azure Marketplace."
   services="marketplace-publishing"
   documentationCenter=""
   authors="HannibalSII"
   manager="hascipio"
   editor=""/>

<tags
   ms.service="marketplace"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/26/2016"
   ms.author="hascipio; avikova" />

# <a name="testing-your-data-service-offer-in-staging"></a>Testiranje tu ponudu podatkovnog servisa u pripremna

>[AZURE.IMPORTANT] **Trenutno ne možemo više nisu za uhodavanje sve nove izdavači podatkovnog servisa. Novi dataservices će dobiti odobrene za unos.** Ako imate aplikaciju SaaS tvrtke koje želite objaviti na AppSource možete pronaći dodatne informacije [u nastavku](https://appsource.microsoft.com/partners). Ako imate programa IaaS aplikacije ili servisa za razvojne inženjere koje želite objaviti na Azure Marketplace možete pronaći dodatne informacije [u nastavku](https://azure.microsoft.com/marketplace/programs/certified/).

Nakon dovršetka prva dva koraka [stvaranja računa za Microsoft za razvojne inženjere](marketplace-publishing-accounts-creation-registration.md) i [Stvaranje vaše podatke servisa nude u Portal za objavljivanje](marketplace-publishing-data-service-creation.md) spremni ste za dostupnost tu ponudu u Azure Marketplace. U ovoj se temi će vas voditi kroz prvi, Srednja, korak pod nazivom "Pripremna"

Pripremna znači implementacija tu ponudu u privatnu "s memorijom za testiranje" gdje možete testiranja i provjere njegova funkcionalnost prije margina za radni. Ponudu Pojavit će se pripremna baš kao i to klijentom koji je postavila.

## <a name="step-1-pushing-your-offer-to-staging"></a>Korak 1. Odaberite tu ponudu za pripremna
Odaberite tu ponudu za pripremna omogućuje vam da biste testirali ponudu prije postaje dostupan za buduće pretplatnika.  Vidjet ćete kako tu ponudu će se pojaviti funkcija and za one pretplata na podatke.  

  ![Crtanje](media/marketplace-publishing-data-service-test-in-staging/step-1.1.png)

1.  Prijava na [Portal za objavljivanje](https://publish.windowsazure.com)
2.  Odaberite **Podatkovni servisi** u prozoru lijevom navigacijskom oknu
3.  Odaberite tu ponudu želite automatske pripremna. Prikazat će se iznad zaslona.
4.  Kliknite gumb **Automatske pripremna** .  
5.  Ako postoje problemi s ponudom za koje je potrebno izvršiti prije no što margina pripremna, vidjet ćete prikazuje popis.  Ispravljanje te stavke tako da kliknete na svaku stavku na popisu. Kada sve ispravaka unijeli, ponovno kliknite gumb **automatske pripremna** .

Ako ne postoje problemi s tu ponudu vidjet ćete skočni prozor.  

Ako ne planiranje/ne što ste odobrili da biste ponuditi tu ponudu Azure portalu (trenutno je ograničeno kapaciteta), samo zatvorite skočnog prozora.

Da biste testirali podatkovnog servisa Azure portalu (osim na portalu servisa DataMarket), trebat će vam pretplata ID Azure da biste testirali s.  U ovom ID pretplate će prepoznavanju računa koja će biti dopuštena da biste testirali tu ponudu.  

Izrezivanje i lijepljenje identifikacijskog Broja za pretplatu i kliknite kvačicu da biste nastavili.

  ![Crtanje](media/marketplace-publishing-data-service-test-in-staging/step-1.2.png)

> [AZURE.NOTE] Ove pretplate Azure ID-ovi su samo potrebni za testiranje i pripremna [Portal za upravljanje Azure](https://manage.windowsazure.com). Nije potreban da biste testirali u trgovine Windows Azure.

Na sljedećem zaslonu koji će se prikazati prikazuje da objavljivanje se izvršava u zemlji koja upozorava i prikazivanjem ikonu "u tijeku" istaknuta žuta ispod. Odaberite pripremna vodi između 10 do 15 minuta.  Ako traje dulje, najprije osvježite preglednik (pritisnite F5 u preglednika Internet Explorer).  U slučajevima rijetko gdje tu ponudu i dalje margina pripremna nakon sata, kliknite kontakt nam povezati Javite nam da je došlo do problema.

  ![Crtanje](media/marketplace-publishing-data-service-test-in-staging/step-1.3.png)

Kad Pritisni za pripremna dovršava ikonu "u tijeku" Premještanje tabulatora i status će ažurirat će se u "Postupna".  Sada ste spremni za testiranje tu ponudu.  

## <a name="step-2-test-your-staged-offer-in-datamarket"></a>Korak 2. Testirajte ponude za postupnu u DataMarket

Kliknite vezu iza teksta **"Vide na servisu nude at...."** Prikaz na zaslonu pretplatnika prikazat će se kada tu ponudu odlazak radnog te će se pojaviti u DataMarket.

  ![Crtanje](media/marketplace-publishing-data-service-test-in-staging/step-2.2.png)

Testiranje ili provjerite je li svaki od 12 stavki označena iznad da biste bili sigurni sve logotipa, cijene/transakcije, tekst, slike, dokumentaciju i veze su točni i radi ispravno.  Ovo je dobro vrijeme da biste bili sigurni test vrijednosti koje ste unijeli prilikom stvaranja tu ponudu zamijenjene stvarnih vrijednosti.

1. Logotip ponudu
2. Naziv ponude
3. Publisher naziv/veza na web-mjesto tvrtke
4. Kategorija za pretraživanje za tu ponudu
5. Veza na podršku za tu ponudu pretplatnike kao pomoć
6. Kontekstna opis za tu ponudu
7. Plan za ponudu koji pokazuje detaljima o naplati
8. Veza na implementaciju kod
9. Ogledne slike koji pokazuju korištenje ponudu podataka
10. Ulazni i izlazni metapodataka za svaki servis unutar ponudu
11. Ponuda, Uvjeti korištenja
12. Pretpregled podataka u ponudu


Na kraju, potvrdite okvir servis će rješavati na Datamarket tako da kliknete vezu "ISTRAŽIVANJE ovaj skup podataka".  Otvorit će se novi prozor u alatu smo poziva "Servisa programa Explorer" tako da možete pretpregledati rezultate upita na temelju usluge.  U ovom prozoru možete unijeti parametara potrebne i pregled rezultata prikazuje iz upita za uslugu.   Osim toga, prikazuje se jest URL upita.  

> [AZURE.NOTE] Ne zaboravite pregled tekstnih opis servisa prikazuje na vrhu.  I ako tu ponudu sastoji se od više od jedne usluge poziv, kliknite karticama pri dnu da biste prešli na sljedeći servis za pregled i testiranje.



## <a name="next-step"></a>Sljedeći korak
Ako imate problema, a potrebna pomoć pri otklanjanju ih obratite se [Podršci za Publisher Azure]( http://go.microsoft.com/fwlink/?LinkId=272975).

Ako ste zadovoljni i želite objaviti tu ponudu pročitajte dokumentaciju [Zahtjev za odobrenje na automatske radnog](marketplace-publishing-push-to-production.md) .

## <a name="see-also"></a>Vidi također
- [Početak rada: Kako objaviti ponude Azure Marketplace](marketplace-publishing-getting-started.md)
