<properties
   pageTitle="Pomoću funkcije Azure s aplikacijama logike | Microsoft Azure"
   description="Informirajte se o korištenju funkcije Azure s aplikacijama logike"
   services="logic-apps,functions"
   documentationCenter=".net,nodejs,java"
   authors="jeffhollan"
   manager="dwrede"
   editor=""/>

<tags
   ms.service="logic-apps"
   ms.devlang="multiple"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="integration"
   ms.date="10/18/2016"
   ms.author="jehollan"/>

# <a name="using-azure-functions-with-logic-apps"></a>Korištenje funkcija Azure s aplikacijama logike

Prilagođeni isječaka C# ili node.js možete pokrenuti pomoću funkcija Azure s unutar logike aplikacije.  [Funkcija Azure](../azure-functions/functions-overview.md) nudi poslužitelj slobodno računalstvo u Microsoft Azure. To je korisno za izvođenje sljedećih zadataka:

* Napredno oblikovanje ili računalnim polja unutar logike aplikacije
* Računanje unutar tijeka rada
* Proširivanje funkcionalnosti aplikacije logike s funkcijama koje su podržane u C# ili node.js

## <a name="create-a-function-for-logic-apps"></a>Stvaranje funkcije za logike aplikacije

Preporučujemo vam da stvorite nove funkcije na portalu funkcije Azure pomoću predložaka **Generički Webhook - čvor** ili **Generički Webhook - C#** . To automatski-popunjava predložak koji prihvaća `application/json` iz logike aplikacije.  Ako odaberete karticu **Integrate** u funkcijama Azure moraju imati **načinu rada** postavljen na **Webhook** i **Webhook vrsta** **Generički JSON**.  Funkcije koje koriste predloške tih automatski se pronađu i naveden u dizajneru logike aplikacije u odjeljku **funkcije Azure u moj regiji.**

Funkcija Webhook prihvaćanje zahtjeva i prosljeđivanja u način putem programa `data` varijablu. Svojstva vaše tereta mogu pristupati pomoću notacija kao što su `data.foo`.  Ako, na primjer, jednostavno JavaScript funkciju koja pretvara vrijednost datuma i vremena u nizu datuma izgleda kao u sljedećem primjeru:

```
function start(req, res){
    var data = req.body;
    res = {
        body: data.date.ToDateString();
    }
}
```

## <a name="call-azure-functions-from-a-logic-app"></a>Pozivati funkcije Azure iz logike aplikacije

U dizajneru ako kliknete izbornik **Akcije** , možete odabrati **Funkcije Azure u moj regiji**.  Popisi spremnika u svoju pretplatu i omogućuje vam da odaberete funkciju koju želite pozvati.  

Kada odaberete funkciju, morat ćete navesti unos tereta objekt. Ovo je poruka koje šalje aplikaciju logiku funkcije, a mora biti JSON objekta. Na primjer, ako želite proslijediti u datum **Zadnje izmjene** iz Salesforce okidač opseg funkcija može izgledati ovako:

![Datum posljednjeg modfied][1]

## <a name="trigger-logic-apps-from-a-function"></a>Pokretanje aplikacije logiku funkcije

Također je moguće pokrenuti logike aplikacije iz unutar funkcije.  Da biste to učinili, jednostavno stvaranje logike aplikacije s ručno pokretanje. Dodatne informacije potražite u članku [logike aplikacije kao callable krajnje točke](app-service-logic-http-endpoint.md).  Nakon toga unutar funkcije, generirati HTTP POST na URL ručno pokretanje s opseg koju želite poslati logike aplikacije.

### <a name="create-a-function-from-the-designer"></a>Stvaranje funkcije iz dizajnera

Možete stvoriti i funkcije node.js webhook iz unutar dizajnera. Najprije odaberite **Azure funkcije u moj regiji** , a zatim odaberite spremnik za funkcija.  Ako još nemate spremniku, morate stvoriti s [portala za Azure funkcije](https://functions.azure.com/signin). Zatim odaberite **Stvori novo**.  

Da biste generirali predloška na temelju podataka koje želite izračunati, navedite Kontekstni objekt koje namjeravate ulaze u funkciji. To mora biti JSON objekta. Ako, na primjer, ako u sadržaju datoteke iz akciju FTP, opseg kontekst izgledat će ovako:

![Kontekst tereta][2]

>[AZURE.NOTE] Budući da taj objekt nije oblikuje kao niz, sadržaj će se dodati izravno JSON opseg. Međutim, ga će pogreške out ako ona još nije JSON token (to jest, niz ili JSON objekt/polja). Da biste oblikuje kao niz, jednostavno dodajte ponude kao što je prikazano na prvom slici u ovom članku.

Dizajner zatim generira predloška (funkcija), možete stvoriti u istoj razini. Varijable unaprijed stvaraju se ovisno o kontekstu koji planirate ulaze u funkciji.




<!--Image references-->
[1]: ./media/app-service-logic-azure-functions/callFunction.png
[2]: ./media/app-service-logic-azure-functions/createFunction.png
