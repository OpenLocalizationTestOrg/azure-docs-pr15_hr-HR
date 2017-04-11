<properties 
    pageTitle="Novu verziju sheme 2016 06 01 | Microsoft Azure" 
    description="Saznajte kako napisati definiciju JSON za najnoviju verziju aplikacije logike" 
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
    ms.date="07/25/2016"
    ms.author="jehollan"/>
    
# <a name="new-schema-version-2016-06-01"></a>Novu verziju sheme 2016 06 01

Nove sheme i API verzija za aplikacije logike ima nekoliko poboljšanja koji poboljšanje pouzdanosti i jednostavnog od-korištenje logike aplikacija. Postoje 3 ključne razlike:

1. Zbrajanje opsega akcije koje sadrži skup akcija.
1. Uvjeti i petlje su jednostavno prva liga akcije
1. Redoslijed izvršavanja više opširno putem `runAfter` svojstvo (koji zamjenjuje `dependsOn`)

Informacije o ažuriranju logike aplikacije iz sheme 2015-08-01 – pregled shema 2016-06-01 [potražite u članku Nadogradnja odjeljku u nastavku.](#upgrading-to-2016-06-01-schema)


## <a name="1-scopes"></a>1. opsezi

Jedna od najvećih promjene u ovu shemu je dodavanja opsega i mogućnost ugnijezditi akcija unutar drugog.  To je korisno kada zajedno grupiranja skup akcija ili kada je potrebno ugnijezditi akcije unutar drugoga (na primjer uvjeta može sadržavati drugi uvjet).  Dodatne informacije o sintaksi opseg mogu biti pronađene [ovdje](app-service-logic-loops-and-scopes.md), ali jednostavne opseg primjer može se pronaći ispod:


```
{
    "actions": {
        "My_Scope": {
            "type": "scope",
            "actions": {                
                "Http": {
                    "inputs": {
                        "method": "GET",
                        "uri": "http://www.bing.com"
                    },
                    "runAfter": {},
                    "type": "Http"
                }
            }
        }
    }
}
```

## <a name="2-conditions-and-loops-changes"></a>2. uvjeta i petlje promjene

U prethodnim verzijama sheme uvjeta i petlji su parametri povezane jednu akciju.  To ograničenje je bio lifted u ovu shemu, a sada uvjeta i petlji prikazuju kao vrstu akcije.  Dodatne informacije pronaći ćete [u ovom članku](app-service-logic-loops-and-scopes.md)i jednostavnog primjera uvjet akcije prikazano u nastavku:

```
{
    "If_trigger_is_foo": {
        "type": "If",
        "expression": "@equals(triggerBody(), 'foo')",
        "runAfter": { },
        "actions": {
            "Http_2": {
                "inputs": {
                    "method": "GET",
                    "uri": "http://www.bing.com"
                },
                "runAfter": {},
                "type": "Http"
            }
        },
        "else": 
        {
            "if_trigger_is_bar": "..."
        }      
    }
}
```

## <a name="3-runafter-property"></a>3. svojstvo RunAfter

Novi `runAfter` zamjenjuje svojstvo `dependsOn` da biste lakše dopustiti veća preciznost u izvođenja redoslijed.  `dependsOn`je sinonim za "akciju pokrenuli i je bio uspješan," no više puta morate izvršiti akciju ako prethodne radnje ne uspije, nije uspjelo ili preskočeno.  `runAfter`omogućuje za taj fleksibilnost.  Postoji objekt koji određuje sva imena akcija će se pokrenuti nakon i definira polja statusa korisnika koji su vam prihvatljivi za pokretanje iz.  Za ako želite pokrenuti nakon odgovora nije uspjela i korak B, primjerice nije uspjelo ili nije uspjela, želite sastaviti sljedeće `runAfter` svojstvo:

```
{
    "...",
    "runAfter": {
        "A": ["Succeeded"],
        "B": ["Succeeded", "Failed"]
    }
}
```

## <a name="upgrading-to-2016-06-01-schema"></a>Nadogradnja 2016-06-01 shemu

Samo nadogradnje na novu shemu 2016-06-01 svega nekoliko koraka.  Detalje o promjenama iz sheme pronaći ćete [u ovom članku](app-service-logic-schema-2016-04-01.md).  Postupak nadogradnje obuhvaća pokretanje nadogradnje skripte, spremite datoteku u obliku nove aplikacije logiku i potencijalno prebrisivanje staru aplikaciju logike prema potrebi.

1. Otvorite aplikaciju za trenutni logike.
1. Kliknite gumb **Sheme ažuriranje** na alatnoj traci
   
    ![][1]
   
    Vratit će se nadograđene definicija.  Koje nije moguće kopirati i zalijepiti to u definiciju resursa, ako je potrebno, ali ne možemo **preporučujemo** korištenje gumb **Spremi kao** da biste bili sigurni sve veze reference vrijede u aplikaciji nadograđena logika.
1. Kliknite gumb **Spremi kao** na alatnoj traci sustava nadogradnje plohu.
1. Ispunite aplikacije status naziv i logiku i kliknite **Stvori** za implementaciju nadogradnje logike aplikacije.
1. Provjerite je li aplikacije nadograđena logika radi prema očekivanjima.

    >[AZURE.NOTE] Ako koristite okidač ručno ili zahtjev za URL povratnog promijenit će imati novu aplikciju logike.  Koristite novi URL da biste provjerili funkcionira završetka do kraja, a mogu Kloniraj preko postojećeg logike aplikacije da biste sačuvali prethodne URL-ova.

1. *Neobavezno* Pomoću gumba **Kloniraj** na alatnoj traci (susjedne **Ažuriranje sheme** ikona na gornjoj slici) da biste prebrisali prethodno logike aplikacije s novom verzijom sheme.  To je potrebno samo ako želite zadržati isti ID resursa za podršku ili zahtjev okidača URL logike aplikacije.

### <a name="upgrade-tool-notes"></a>Alat za nadogradnju bilješke

#### <a name="condition-mapping"></a>Mapiranje uvjet

Alat će postati najbolje trud da biste grupirali akcije true i false granu zajedno u opsegu u definiciji nadograđena.  Konkretno dizajnera uzorak `@equals(actions('a').status, 'Skipped')` treba prikazuju kao programa `else` akcija.  No ako alat otkrije uzoraka ne prepoznaje potencijalno stvoriti zasebnu uvjete za na true i false grani.  Akcije može biti ponovno mapiranih objavljuju nadogradnje po potrebi.

#### <a name="foreach-with-condition"></a>ForEach s uvjetom
  
U novu shemu s akcijom filtra moguće je replicirati prethodnog uzorka petlje foreach s uvjetom po stavku.  To treba dogoditi automatski pri nadogradnji.  Uvjet postane akciju filtar prije petlje foreach (da biste vratili samo polja stavke koje zadovoljavaju uvjet), a taj polja se prenosi u akciji foreach.  Možete pogledati primjera ovog [članka](app-service-logic-loops-and-scopes.md)

#### <a name="resource-tags"></a>Oznaka resursa

Resurs oznake uklonit će se pri nadogradnji i morat ćete ih ponovno postaviti nadograđene tijeka rada.

## <a name="other-changes"></a>Ostale promjene

### <a name="manual-trigger-renamed-to-request-trigger"></a>Ručno pokretanje preimenovano u zahtjev za pokretanje

Vrsta `manual` ukinuta i preimenovana `request` s vrstu od `http`.  Ovo je dosljedan s vrstom uzorak okidača koristi za sastavljanje.

### <a name="new-filter-action"></a>Nove akcije "Filtar"

Ako radite s velikim polja i treba li vam da biste filtrirali prema dolje do manji skup stavki, možete koristiti novu vrstu "Filtar".  Prihvaća polja i uvjeta i bit će rezultirati uvjet za svaku stavku i vratite polja stavki koje ispunjavaju uvjet.

### <a name="foreach-and-until-action-restrictions"></a>ForEach i do ograničenja akcija

U foreach i do petlja su ograničeni na jednu akciju.

### <a name="trackedproperties-on-actions"></a>TrackedProperties na Akcije

Akcije sada možete imati dodatnog svojstva (iste razine da biste `runAfter` i `type`) pod nazivom `trackedProperties`.  Je objekt koji navodi određene akcije unosa ili izlaze da budu obuhvaćeni Azure dijagnostičkih telemetrijskih koji je čuje u sklopu tijeka rada.  Ako, na primjer:

```
{                
    "Http": {
        "inputs": {
            "method": "GET",
            "uri": "http://www.bing.com"
        },
        "runAfter": {},
        "type": "Http",
        "trackedProperties": {
            "responseCode": "@action().outputs.statusCode",
            "uri": "@action().inputs.uri"
        }
    }
}
```

## <a name="next-steps"></a>Daljnji koraci
- [Korištenje definiciju logike aplikacije tijeka rada](app-service-logic-author-definitions.md)
- [Stvaranje predloška za implementaciju aplikacije logike](app-service-logic-create-deploy-template.md)


<!-- Image references -->
[1]: ./media/app-service-logic-schema-2016-04-01/upgradeButton.png
