<properties
    pageTitle="Dodavanje Microsoft Translator u aplikacijama logike | Microsoft Azure"
    description="Pregled Microsoft Translator poveznika s parametrima REST API-JA"
    services=""
    suite=""
    documentationCenter="" 
    authors="MandiOhlinger"
    manager="erikre"
    editor=""
    tags="connectors"/>

<tags
   ms.service="multiple"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na" 
   ms.date="08/18/2016"
   ms.author="mandia"/>

# <a name="get-started-with-the-microsoft-translator-connector"></a>Uvod u Microsoft Translator poveznika
Povezivanje s Microsoft Translator za prevođenje teksta, otkriva jezik i drugo. Microsoft Translator omogućuje sljedeće: 

- Sastavljanje vaše tvrtke tijek utemeljenih na podacima koje ste dobili od Microsoft Translator. 
- Korištenje akcija za prevođenje teksta, otkriva jezik i drugo. Ove se radnje dobiti odgovor, a zatim unesite dostupne za ostale akcije izlaz. Na primjer, prilikom stvaranja nove datoteke na servisu Dropbox možete prevesti tekst u datoteku na drugom jeziku koristeći Microsoft Translator.

Da biste dodali operaciju u aplikacijama logike, potražite u članku [Stvaranje logike aplikacije](../app-service-logic/app-service-logic-create-a-logic-app.md).

## <a name="triggers-and-actions"></a>Okidača i akcija
Microsoft Translator obuhvaća sljedeće radnje. Postoje bez okidača.

Okidača | Akcija
--- | ---
Ništa | <ul><li>Prepoznavanje jezika</li><li>Tekst u govor</li><li>Prijevod teksta</li><li>Početak jezika</li><li>Početak govora jezika</li></ul>

Sve poveznike podržava podatke u JSON i XML formatima.


## <a name="create-a-connection-to-microsoft-translator"></a>Povezivanje s Microsoft Translator

>[AZURE.INCLUDE [Steps to create a connection to Microsoft Translator](../../includes/connectors-create-api-microsofttranslator.md)]


## <a name="swagger-rest-api-reference"></a>Referenca swagger REST API-JA
Odnosi se na verziju: 1.0.

### <a name="detect-language"></a>Prepoznavanje jezika    
Otkriva jezik izvor dali tekst.  
```GET: /Detect```

| Ime| Vrsta podataka|Obavezno|Koja se nalazi u|Zadana vrijednost|Opis|
| ---|---|---|---|---|---|
|upit|niz|Da|upit|Ništa |Tekst čiji jezik će biti označena|

#### <a name="response"></a>Odgovor
|Ime|Opis|
|---|---|
|200|ok|
|Zadani|Nije uspjelo.|


### <a name="text-to-speech"></a>Tekst u govor    
Pretvara zadani tekst u govor kao audio prijenos u obliku val.  
```GET: /Speak```

| Ime| Vrsta podataka|Obavezno|Koja se nalazi u|Zadana vrijednost|Opis|
| ---|---|---|---|---|---|
|upit|niz|Da|upit|Ništa |Tekst koji se pretvara|
|jezik|niz|Da|upit|Ništa |Šifra jezika za generiranje govora (primjer: "en-us')|

#### <a name="response"></a>Odgovor
|Ime|Opis|
|---|---|
|200|ok|
|Zadani|Nije uspjelo.|


### <a name="translate-text"></a>Prijevod teksta    
Prevodi tekst u navedenom jeziku koristeći Microsoft Translator.  
```GET: /Translate```

| Ime| Vrsta podataka|Obavezno|Koja se nalazi u|Zadana vrijednost|Opis|
| ---|---|---|---|---|---|
|upit|niz|Da|upit|Ništa |Prijevod teksta|
|languageTo|niz|Da|upit| Ništa|Kod ciljni jezik (primjer: "fr")|
|languageFrom|niz|ne|upit|Ništa |Izvor jeziku; Ako nije naveden, Microsoft Translator će pokušati automatsko prepoznavanje. (primjer: en)|
|Kategorija|niz|ne|upit|Općenito |Prijevod kategorija (zadani: "Općenito")|

#### <a name="response"></a>Odgovor
|Ime|Opis|
|---|---|
|200|ok|
|Zadani|Nije uspjelo.|


### <a name="get-languages"></a>Početak jezika    
Dohvaća sve jezike koje podržava Microsoft Translator.  
```GET: /TranslatableLanguages```

Nema parametara za ovaj poziv. 

#### <a name="response"></a>Odgovor
|Ime|Opis|
|---|---|
|200|ok|
|Zadani|Nije uspjelo.|


### <a name="get-speech-languages"></a>Početak govora jezika    
Dohvaća podatke u odjeljku jezici dostupni za govor synthesis.  
```GET: /SpeakLanguages``` 

Nema parametara za ovaj poziv.

#### <a name="response"></a>Odgovor
|Ime|Opis|
|---|---|
|200|ok|
|Zadani|Nije uspjelo.|

## <a name="object-definitions"></a>Definicija objekta

#### <a name="language-language-model-for-microsoft-translator-translatable-languages"></a>Jezik: modela jezika za Microsoft Translator prevodivog jezika

|Naziv svojstva | Vrsta podataka | Obavezno|
|---|---|---|
|Kod|niz|ne|
|Ime|niz|ne|


## <a name="next-steps"></a>Daljnji koraci

[Stvaranje logike aplikacije](../app-service-logic/app-service-logic-create-a-logic-app.md).

Vratite se na [popisu API-ji](apis-list.md).


<!--References-->
[5]: https://datamarket.azure.com/developer/applications/
[6]: ./media/connectors-create-api-microsofttranslator/register-your-application.png
