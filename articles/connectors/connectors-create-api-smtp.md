<properties
pageTitle="SMTP | Microsoft Azure"
description="Stvorite logiku aplikacije sa servisom Azure aplikacije. Povezivanje s SMTP slanje e-pošte."
services="logic-apps"   
documentationCenter=".net,nodejs,java"  
authors="msftman"   
manager="erikre"    
editor=""
tags="connectors" />

<tags
ms.service="app-service-logic"
ms.devlang="multiple"
ms.topic="article"
ms.tgt_pltfrm="na"
ms.workload="integration"
ms.date="07/15/2016"
ms.author="deonhe"/>

# <a name="get-started-with-the-smtp-connector"></a>Početak rada s SMTP poveznika

Povezivanje s SMTP slanje e-pošte.

Da biste koristili [neki poveznika](./apis-list.md), najprije morate stvoriti logike aplikaciju. Početak rada stvaranjem [aplikacije logike sada](../app-service-logic/app-service-logic-create-a-logic-app.md).

## <a name="connect-to-smtp"></a>Povezivanje s SMTP

Logika aplikacije možete pristupiti bilo koji servis, najprije morate stvoriti *veze* sa servisom. [Veze](./connectors-overview.md) navedene veze između logike aplikacije i drugih servisa. Na primjer, da biste se povezali s SMTP, najprije morate SMTP *veze*. Da biste stvorili vezu, morate da unesete vjerodajnice obično koristite za pristup servisu koji se želite povezati. Tako, u ovom primjeru SMTP koji su vam potrebni vjerodajnice naziv veze, adresa SMTP poslužitelja i podatke za prijavu korisnika da biste stvorili vezu s SMTP. [Dodatne informacije o vezama]()  

### <a name="create-a-connection-to-smtp"></a>Povezivanje s SMTP

>[AZURE.INCLUDE [Steps to create a connection to SMTP](../../includes/connectors-create-api-smtp.md)]

## <a name="use-an-smtp-trigger"></a>Pomoću okidača SMTP

Okidač je događaja koji se mogu koristiti za pokretanje tijeka rada definirano u aplikaciji logike. [Dodatne informacije o okidača](../app-service-logic/app-service-logic-what-are-logic-apps.md#logic-app-concepts).

U ovom primjeru jer SMTP imaju okidača vlastitog, ćemo koristiti okidača **Salesforce – stvaranja objekta** . Taj se okidač će aktivirati prilikom stvaranja novog objekta u Salesforce. Naš, primjerice, ne možemo ćete ga postaviti tako da se prilikom svakog stvaranja novog potencijalnog klijenta u Salesforce akcija za *Slanje e-pošte* obavlja putem poveznika SMTP s obavijesti o stvaranja novog potencijalnog klijenta.

1. U okvir za pretraživanje unesite *salesforce* u dizajneru logike aplikacije, a zatim odaberite okidača **Salesforce – stvaranja objekta** .  
 ![](../../includes/media/connectors-create-api-salesforce/trigger-1.png)  

2. Prikazat će se kontrola **stvaranja objekta** .
 ![](../../includes/media/connectors-create-api-salesforce/trigger-2.png)  

3. Odaberite **Vrstu objekta** , a zatim odaberite *potencijalnog klijenta* s popisa objekata. U ovom ćete koraku koje su koja označava da stvarate okidač će obavijestiti logike aplikacije prilikom svakog stvaranja novog potencijalnog klijenta u Salesforce.  
 ![](../../includes/media/connectors-create-api-salesforce/trigger3.png)  

4. Stvoreno je okidača.  
 ![](../../includes/media/connectors-create-api-salesforce/trigger-4.png)  

## <a name="use-an-smtp-action"></a>Akcijom SMTP

Akciju je postupak provodi definirano u aplikaciji logike tijeka rada. [Dodatne informacije o akcije](../app-service-logic/app-service-logic-what-are-logic-apps.md#logic-app-concepts).

Sad kad okidača dodana, slijedite ove korake da biste dodali akciju SMTP koja će se pojaviti prilikom stvaranja novog potencijalnog klijenta u Salesforce.

1. Odaberite **+ Novi korak** da biste dodali akciju koju želite poduzeti prilikom stvaranja novog potencijalnog klijenta.  
 ![](../../includes/media/connectors-create-api-salesforce/trigger4.png)  

2. Odaberite **Dodaj akciju**. U ovom otvara se okvir za pretraživanje pretraživanja za svaku akciju koju želite preuzeti.  
 ![](../../includes/media/connectors-create-api-smtp/using-smtp-action-2.png)  

3. Unesite *smtp* da biste potražili akcije koje su vezane uz SMTP.  

4. Odaberite **SMTP - Pošalji e-poštu** kao akcija koja se izvodi prilikom stvaranja novog potencijalnog klijenta. Otvorit će se Kontrolni blok akcija. Morat ćete uspostaviti vezu smtp dizajnera blok ako niste učinili prethodno.  
 ![](../../includes/media/connectors-create-api-smtp/smtp-2.png)    

5. Unos podataka za željenu e-pošte u bloku **SMTP - Pošalji e-poštu** .  
 ![](../../includes/media/connectors-create-api-smtp/using-smtp-action-4.PNG)  

6. Spremanje rezultata rada da biste aktivirali svoj tijek rada.  

## <a name="technical-details"></a>Tehničke pojedinosti

Evo detalja o okidačima, akcije i odgovore koje podržava tu vezu:

## <a name="smtp-triggers"></a>SMTP okidača

SMTP ima bez okidača. 

## <a name="smtp-actions"></a>SMTP akcije

SMTP sastoji se od sljedećih akcija:


|Akcija|Opis|
|--- | ---|
|[Slanje e-pošte](connectors-create-api-smtp.md#send-email)|Ovaj postupak šalje poruku e-pošte za jednog ili više primatelja.|

### <a name="action-details"></a>Detalji o akcija

Evo detalja za akciju poveznika, zajedno s odgovor:


### <a name="send-email"></a>Slanje e-pošte
Ovaj postupak šalje poruku e-pošte za jednog ili više primatelja. 


|Naziv svojstva| Zaslonsko ime|Opis|
| ---|---|---|
|Da biste|Da biste|Odredite razdvojenih točkom sa zarezom kao što su adrese e-pošterecipient1@domain.com;recipient2@domain.com|
|KOPIJA|kopija|Navedite razdvojenih točkom sa zarezom kao što su adrese e-pošterecipient1@domain.com;recipient2@domain.com|
|Predmet|Predmet|Predmet e-pošte|
|Tijelo|Tijelo|Tijelo e-pošte|
|Iz|Iz|Adresa e-pošte pošiljatelja kao što susender@domain.com|
|IsHtml|Upozorenje|Slanje e-pošte u obliku HTML (true i false)|
|Skrivena kopija|Skrivena kopija|Odredite razdvojenih točkom sa zarezom kao što su adrese e-pošterecipient1@domain.com;recipient2@domain.com|
|Važnost|Važnost|Važnost e-pošte (visoka, normalno ili najniža)|
|ContentData|Privici sadržaja podataka|Sadržaja podataka (base64 kodirani strujanja i kao-je za niz)|
|ContentType|Vrste sadržaja privitaka|Vrste sadržaja|
|ContentTransferEncoding|Prijenos sadržaja privitke kodiranja|Sadržaja prijenos kodiranje (base64 ili ništa)|
|Naziv datoteke|Naziv datoteke privitaka|Naziv datoteke|
|ContentId|ID sadržaja privitaka|Id sadržaja|

Programa * označava da je svojstvo potrebno


## <a name="http-responses"></a>HTTP odgovora

Akcije i okidača iznad možete se vratiti jedan ili više sljedećih HTTP kodovima stanja: 

|Ime|Opis|
|---|---|
|200|ok|
|202|Prihvaćen|
|400|Neispravan zahtjev|
|401|Neovlašteno|
|403|Zabranjen|
|404|Nije pronađen|
|500|Interna pogreška poslužitelja. Došlo je do neočekivane pogreške.|
|Zadani|Nije uspjelo.|

## <a name="next-steps"></a>Daljnji koraci
[Stvaranje logike aplikacije](../app-service-logic/app-service-logic-create-a-logic-app.md)