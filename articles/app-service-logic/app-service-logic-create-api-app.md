<properties 
    pageTitle="Stvaranje API za logike aplikacije" 
    description="Stvaranje prilagođene API-JA za uporabu logike aplikacije" 
    authors="jeffhollan" 
    manager="dwrede" 
    editor="" 
    services="logic-apps" 
    documentationCenter=""/>

<tags
    ms.service="logic-apps"
    ms.workload="integration"
    ms.tgt_pltfrm="na"
    ms.devlang="na" 
    ms.topic="article"
    ms.date="10/18/2016"
    ms.author="jehollan"/>
    
# <a name="creating-a-custom-api-to-use-with-logic-apps"></a>Stvaranje prilagođene API-JA za uporabu logike aplikacije

Ako želite da biste proširili platformu logike aplikacije, načina mnogo da biste uputili poziv u API-ji ili sustavima koji nisu dostupni u nekom od naše mnogo Izlaz u-tvorničke poveznika.  Jedna od tih načina stvaranja aplikacije API-JA možete nazvati unutar aplikacije logike tijeka rada.

## <a name="helpful-tools"></a>Korisnim alatima

API-ji najbolje funkcioniraju s aplikacijama logike, preporučujemo generiranje [swagger](http://swagger.io) dokument dovršenog podržani operacije i parametara za vaše API-JA.  Nema više biblioteke (kao što je [Swashbuckle](https://github.com/domaindrivendev/Swashbuckle)) automatsko generiranje swagger umjesto vas.  [TRex](https://github.com/nihaue/TRex) možete koristiti i da bi unos opaski u njih swagger za rad s logike aplikacije (Prikaži nazive, svojstva vrste itd.).  Za neke uzorke API aplikacije za aplikacije logike svakako pogledajte naše [GitHub spremište](http://github.com/logicappsio) ili [blog](http://aka.ms/logicappsblog).

## <a name="actions"></a>Akcija

Osnovni akcija za aplikaciju logike je kontroler koji će prihvaćanje zahtjeva za razmjenu HTTP i povratna odgovor (obično 200).  No postoje razne uzorke možete pratiti da biste proširili akcije na temelju vašim potrebama.

Prema zadanim postavkama modul logike aplikacija će vremenskog ograničenja zahtjeva nakon 1 minute.  Međutim, imate API izvršiti na Akcije koje dulje i imaju modul čekanja za dovršetak, slijedeći asinkrone ili webhook uzorak detaljne ispod.

Za standardne Akcije, jednostavno upišite način HTTP zahtjev za vaše API koji je izložen putem swagger.  Vidjet ćete uzoraka API aplikacije koje funkcioniraju s aplikacijama logike u našem [GitHub spremište](https://github.com/logicappsio).  U nastavku načina da biste izvršili uobičajene uzorke s prilagođenog poveznika.

### <a name="long-running-actions---async-pattern"></a>Dugo izvodi akcije - asinkrone uzorak

Ako je pokrenut dugo korak ili zadatka, prvo što biste trebali učiniti je provjerite je li modul zna niste isteklo. Morate komunikaciju modul kako znati kada završite sa zadatkom i na kraju, vam je potrebno da biste se vratili relevantnih podataka modul tako da ga možete nastaviti s tijekom rada. Dovršite koji putem API pomoću navedenih u nastavku tijek. Korake u nastavku su iz točke-od-prikaza prilagođene API-JA:

1. Primljena zahtjev, odmah vratite odgovor (prije se radi). Bit će odgovor na `202 ACCEPTED` odgovor, a zatim da modul znate imate podatke, prihvatili opseg, a sada obrade. 202 odgovor mora sadržavati sljedeće zaglavlja: 
 * `location`Zaglavlje (obavezno): to je apsolutni put do logike aplikacije URL možete koristiti da biste provjerili status zadatka.
 * `retry-after`(nije obavezno, će zadane postavke i 20 za akcije). To je broj sekundi modul treba čekati prije provjere zaglavlja URL mjesta da biste provjerili status.

2. Prilikom provjere stanja zadatka izvedite sljedeće provjere: 
 * Ako je obavite posao: vratite na `200 OK` odgovor, odgovor opseg.
 * Ako obrađuju posao: vratili drugi `202 ACCEPTED` odgovor s istom zaglavlja kao početnu odgovor

Ovaj uzorak omogućuje pokretanje iznimno dugo zadataka u niti prilagođene API-JA, ali Zadrži aktivnu vezu aktivnosti sa modul logike aplikacije tako da ga ne vremenskog ograničenja ili nastaviti prije dovršetka posla. Prilikom dodavanja to u aplikaciju za logiku, važno je obratiti pažnju na nije potrebno ništa u definicije logike aplikacije da biste nastavili s ankete i provjera stanja. Čim modul vidi 202 PRIHVAĆENA odgovor sa zaglavljem valjano mjesto, poštovati asinkrone uzorak će se i dalje ankete zaglavlje mjesto dok se vraća u ne-202.

Vidjet ćete uzorak ovaj uzorak u GitHub [ovdje](https://github.com/jeffhollan/LogicAppsAsyncResponseSample)

### <a name="webhook-actions"></a>Webhook akcije

Tijekom tijekova rada, imate aplikaciju logike zadržite pokazivač i pričekajte da "povratnog" da biste nastavili.  U ovom povratnog dolazi u obliku HTTP POST.  Da biste implementirati ovaj uzorak, morate unijeti dva krajnje točke na upravljaču: pretplatiti i otkazivanje pretplate.

Na 'pretplata' aplikaciju logike će stvoriti i morate registrirati povratnog URL koji se mogu pohraniti na API-JA i povratnog spremna kao HTTP POST.  Bilo koji sadržaj/zaglavlja će se proslijediti u aplikaciju logiku i mogu se koristiti u ostatak tijeka rada.  Modul logike aplikacija će pozivanje točke pretplata na izvođenja čim ga dodirne taj korak.

Ako je otkazan Pokreni, modul logike aplikacija će uputiti poziv krajnjoj "otkazivanje pretplate".  Vaš API-JA možete zatim odjaviti povratnog URL-a prema potrebi.

Trenutno dizajner logike aplikacije ne podržava otkrivaju krajnje webhook putem swagger, tako da biste koristili ovu vrstu akcije morate dodati akciju "Webhook" i navedite URL-a, zaglavlja i tijela zahtjev.  Možete koristiti u `@listCallbackUrl()` funkcija tijeka rada u bilo koju od tih polja po potrebi za prosljeđivanje u povratnog URL-u.

Vidjet ćete uzorak uzorkom webhook u GitHub [ovdje](https://github.com/jeffhollan/LogicAppTriggersExample/blob/master/LogicAppTriggers/Controllers/WebhookTriggerController.cs)

## <a name="triggers"></a>Okidača

Uz Akcije, imate prilagođene API zakon o kao okidač logike aplikaciju.  Postoje dvije uzoraka možete pratiti ispod da biste pokrenuli aplikaciju za logika:

### <a name="polling-triggers"></a>Ankete okidača

Ankete okidača ponašaju vrlo nalik iznad dugo izvodi asinkrone akcije.  Modul logike aplikacije podići krajnju točku okidača nakon isteka određenog vremena (ovisno o tome SKU 15 sekundi za Premium 1 minute za Standard, a 1 sat za besplatno).

Ako nema podataka dostupna, okidača vraća na `202 ACCEPTED` odgovor, s je `location` i `retry-after` zaglavlje.  Međutim, okidača preporučuje se `location` Zaglavlje sadrži parametar upita `triggerState`.  Ovo je neke identifikator za API-JA znati kada zadnji put aplikaciju logike je pokrenulo.  Ako nema podataka dostupna, okidača vraća na `200 OK` odgovor sadržaja opseg.  To će pokrenuti aplikaciju logike.

Na primjer, ako mi je provjere jesu li datoteke nije dostupan, nije moguće Sastavi okidača ankete koja učinite sljedeće:

* Ako zahtjev za primljena s bez triggerState vratiti na API na `202 ACCEPTED` s na `location` zaglavlja koja ima triggerState stavke trenutno vrijeme, a `retry-after` od 15.
* Ako je zahtjev primili s na triggerState:
 * Provjerite da biste vidjeli sve datoteke dodani nakon triggerState datuma i vremena. 
  * Ako je 1 datoteka, vratite na `200 OK` odgovor sadržaja opseg povećali triggerState na datum i vrijeme datoteke koje vraćaju i postavite na `retry-after` 15.
  * Ako postoji više datoteka, koje je moguće vratiti 1 odjednom, uz na `200 OK`, povećali Moje triggerState u na `location` zaglavlje i postavljanje `retry-after` 0.  To će vam dopustiti da modul je dostupno više podataka i ona će odmah zatražiti na na `location` zaglavlje naveden.
  * Ako nema datoteka, vratiti na `202 ACCEPTED` odgovor i ostavite na `location` triggerState isti.  Postavljanje `retry-after` 15.

Vidjet ćete uzorak okidač ankete u GitHub [ovdje](https://github.com/jeffhollan/LogicAppTriggersExample/tree/master/LogicAppTriggers)

### <a name="webhook-triggers"></a>Webhook okidača

Webhook okidača ponašaju vrlo nalik Webhook akcije iznad.  Modul logike aplikacije podići krajnjoj točki 'pretplata kad god okidač webhook dodaje i spremiti.  Vaš API-JA možete registrirati webhook URL-a i nazovite ga putem HTTP POST kad god se podaci su dostupni.  U aplikaciju logike pokrenuti će se proslijediti sadržaja tereta i zaglavlja.

Ako okidač webhook ikad izbrisali (logike aplikacije potpuno ili samo okidača webhook), modul će upućivanje poziva na URL "otkazivanje pretplate" gdje možete poništiti registraciju vaše API povratnog URL-a i zaustavljanje sve procese prema potrebi.

Trenutno dizajner logike aplikacije ne podržava otkrivaju okidač webhook putem swagger, tako da biste koristili ovu vrstu akcije morate dodati okidača "Webhook" i navedite URL-a, zaglavlja i tijela zahtjev.  Možete koristiti u `@listCallbackUrl()` funkcija tijeka rada u bilo koju od tih polja po potrebi za prosljeđivanje u povratnog URL-u.

Vidjet ćete uzorak okidač webhook u GitHub [ovdje](https://github.com/jeffhollan/LogicAppTriggersExample/tree/master/LogicAppTriggers)