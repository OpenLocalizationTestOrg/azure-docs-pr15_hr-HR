<properties
   pageTitle="Korištenje ga Slušatelj HTTP i poveznika u aplikacijama logike | Aplikacije servisa za Microsoft Azure "
   description="Kako stvoriti i konfigurirati ga slušatelj HTTP i aplikacije poveznika ili API HTTP akcije i koristiti u aplikaciji logike u aplikacije servisa za Azure"
   services="logic-apps"
   documentationCenter=".net,nodejs,java"
   authors="anuragdalmia"
   manager="erikre"
   editor=""/>

<tags
   ms.service="logic-apps"
   ms.devlang="multiple"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="integration"
   ms.date="08/31/2016"
   ms.author="prkumar"/>


# <a name="get-started-with-the-http-listener-and-http-action-and-add-it-to-your-logic-app"></a>Početak rada s ga slušatelj HTTP i HTTP akcije i njezino dodavanje logike aplikacije

> [AZURE.NOTE]Ne možemo su završni podrška za ovaj poveznik jer njegova funkcionalnost sada uključen po zadanom kao **ručno pokretanje** prilikom stvaranja novih logike aplikacija.  Preporučujemo da nadogradnje svih aplikacija logike koji koriste ovaj poveznik.  
> Ovu verziju članka odnosi logike aplikacije 2014. – 12-01 – pregled shema verziju.

Izravno povezivanje HTTP resursa za slušanje HTTP zahtjeva za i konfiguriranje HTTP zahtjevima za web. Postoje nekim scenarijima kojima ćete morati raditi izravno HTTP veze, uključujući:

1.  Za razvoj logike aplikacije s podrškom za web ili mobilni korisnik interaktivne sučelje.
2.  Da biste dobili i obrada podataka iz web-servisa koji nema za vrijeme odsutnosti okvir poveznika.
3.  Izvedite akcije koje su već prikazanog kao web-servisa, ali nije dostupan kao aplikaciju API-JA.

Sljedećim scenarijima postoje dvije mogućnosti:

1. **HTTP ga slušatelj**: ovaj poveznik ponaša se kao okidač i prati HTTP zahtjeva na konfigurirani krajnjoj točki. Poziv primitku na konfigurirani krajnje točke pokreće novu instancu tijeka i prosljeđuje primljene u zahtjev za tijek za obradu podataka. Moguće je konfigurirati i automatski odgovoriti na zahtjev za dolazne kada tijeka rada, ili omogućuju vam sastavljanje odgovor na temelju izvođenja tijeka i slati odgovor s pozivateljem.
2. **HTTP akcija**: to možete konfigurirati web-zahtjeva za sve krajnje točke dostupne na Internetu, dobije odgovor i on postaje dostupan za dodatne akcije u tijeku trošiti.

Možete pokrenuti logike aplikacije koji se temelji na raznim izvorima podataka i ponuda poveznici za dohvat i obrada podataka u sklopu tijeka. Poveznik za HTTP možete dodati tijek rada i proces podataka tvrtke kao dio ovaj tijek rada u aplikaciji za logiku. 

## <a name="creating-an-http-listener-for-your-logic-app"></a>Stvaranje programa ga slušatelj HTTP logike aplikacije
Poveznik mogu stvoriti iz logike aplikacije ili stvoriti izravno iz trgovine Azure Marketplace. Da biste stvorili poveznik iz trgovine:  

1. Azure startboard odaberite **trgovine**.
2. Traženje "HTTP", odaberite ga Slušatelj HTTP pa odaberite **Stvori**.
3.  Konfigurirajte ga slušatelj HTTP na sljedeći način:  
![][1]

4.  Kada postavite postavke paketa, vidjet ćete sljedeću mogućnost na li ga slušatelj treba odgovoriti automatski ili potrebno da biste poslali eksplicitnih odgovor. To postavite na **False** da biste poslali svoj odgovor:  
![][2]

5.  Kliknite **u redu** da biste stvorili.
6.  Nakon stvaranja aplikacije instancu API-JA, otvorite postavke da biste konfigurirali sigurnost. HTTP ga slušatelj trenutno podržava osnovnu provjeru autentičnosti. Možete konfigurirati to pomoću mogućnosti sigurnosti prilikom otvaranja ga slušatelj HTTP:  
![][3]
  
    **Poznati problemi** *Na sigurnosne postavke prikaz "Nema" kao zadanu vrijednost međutim nije definirana. Morate promijeniti postavku Basic i ponovno ništa prije spremanja da biste bili sigurni da ga Slušatelj HTTP ispravno konfigurirano.*  

7. Na kraju, postavljanje sigurnosnih postavki aplikacije API-JA u javnu (anonimno) da biste omogućili vanjski klijenti da biste pristupili krajnju točku. Ta je postavka dostupna u odjeljku "sve postavke > postavke aplikacije" HTTP ga Slušatelj API aplikacije:![][10]

Nakon toga, sada možete stvoriti aplikaciju logike da biste koristili ga slušatelj HTTP.

## <a name="using-the-http-listener-in-your-logic-app"></a>Korištenje ga slušatelj HTTP u logike aplikacije
Nakon stvaranja aplikacije API-JA, sada možete koristiti ga slušatelj HTTP kao okidač logike aplikacije. Da biste to učinili, morate:

4.  Stvorite novu aplikaciju logike.
5.  Otvorite "Okidača i akcije" da biste otvorili dizajner logike aplikacije i konfiguracija vašeg tijeka. HTTP ga Slušatelj nalazi se u galeriji. Odaberite ga.
6.  Sada možete postaviti HTTP metoda i relativni URL na kojem su vam potrebne ga slušatelj pokretanje tijeka:  
![][4]  
![][5]

7.  Da biste dobili potpuni URI, dvokliknite ga Slušatelj HTTP da biste pogledali postavke konfiguracije i kopirajte URL za "Glavno računalo" API aplikacije:  
![][6]
8.  Sada možete koristiti podatke primljene u HTTP zahtjev u druge akcije u tijeku na sljedeći način:  
![][7]  
![][8]
9.  Na kraju, da biste poslali odgovor, dodajte drugi ga Slušatelj HTTP i odaberite akciju slanje HTTP odgovor. Postavite ID zahtjev za RequestID dobivenog ga Slušatelj HTTP i popunjavanje tijela odgovor i HTTP status koji želite vratiti natrag:  
![][9]

## <a name="using-the-http-action"></a>Pomoću akcije HTTP
Akciju HTTP nativno podržava logike aplikacije, a ne zahtijeva API aplikacije za stvaranje prve da biste mogli koristiti. Možete umetnuti akciju HTTP bilo kojem trenutku u svojoj aplikaciji logiku i odaberite URI, zaglavlja i tijela za poziv.
Akcija HTTP podržava više mogućnosti za sigurnost strani klijenta. Potražite u članku [klijent strani sigurnosne mogućnosti](../scheduler/scheduler-outbound-authentication.md).

Izlaz iz akciju HTTP je zaglavlja i tijela koje se mogu koristiti dodatno slijedu u tijeku sličan način potrošnje Izlaz iz druge akcije i poveznika.

## <a name="do-more-with-your-connector"></a>Bolje iskorištavanje vaše poveznika
Sad kad je poveznik stvorili, možete ga dodati u tijeku rada poslovne logike aplikacija. U odjeljku [koje su aplikacije logike?](app-service-logic-what-are-logic-apps.md).

Prikaz reference Swagger REST API-JA na [poveznika i API aplikacije Reference](http://go.microsoft.com/fwlink/p/?LinkId=529766).

Možete pregledati i performanse Statistika i kontrola sigurnost da biste poveznik. Potražite u članku [Upravljanje i nadzirati ugrađene aplikacije API -JA i poveznike](app-service-logic-monitor-your-connectors.md).

> [AZURE.NOTE] Ako želite započeti s Azure logike aplikacije prije registracije za račun za Azure, idite na [Pokušajte logike aplikacije](https://tryappservice.azure.com/?appservice=logic). Odmah možete stvoriti aplikaciju za logiku short-lived starter u aplikacije servisa. Nema kreditne kartice potrebna; Nema preuzete obveze.

<!--Image references-->
[1]: ./media/app-service-logic-connector-http/1.png
[2]: ./media/app-service-logic-connector-http/2.png
[3]: ./media/app-service-logic-connector-http/3.png
[4]: ./media/app-service-logic-connector-http/4.png
[5]: ./media/app-service-logic-connector-http/5.png
[6]: ./media/app-service-logic-connector-http/6.png
[7]: ./media/app-service-logic-connector-http/7.png
[8]: ./media/app-service-logic-connector-http/8.png
[9]: ./media/app-service-logic-connector-http/9.png
[10]: ./media/app-service-logic-connector-http/10.png
