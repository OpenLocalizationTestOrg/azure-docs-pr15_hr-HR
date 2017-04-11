<properties
pageTitle="Saznajte kako se pomoću poveznika Bus servisa Azure u aplikacijama za logiku | Microsoft Azure"
description="Stvorite logiku aplikacije sa servisom Azure aplikacije. Povezivanje s Bus servisa Azure za slanje i primanje poruka. Možete izvoditi akcije kao što su za slanje red čekanja, slanje temu, dobiti iz reda čekanja i primanje iz pretplate."
services="logic-apps"
documentationCenter=".net,nodejs,java"  
authors="msftman"
manager="erikre"
editor=""
tags="connectors" />

<tags
ms.service="logic-apps"
ms.devlang="multiple"
ms.topic="article"
ms.tgt_pltfrm="na"
ms.workload="integration"
ms.date="08/02/2016"
ms.author="deonhe"/>

# <a name="get-started-with-the-azure-service-bus-connector"></a>Početak rada s Bus servisa Azure poveznika

Povezivanje s Bus servisa Azure za slanje i primanje poruka. Možete izvoditi akcije kao što su za slanje red čekanja, slanje temu, dobiti iz reda čekanja i primanje iz pretplate.

Da biste koristili [neki poveznika](./apis-list.md), najprije morate stvoriti logike aplikaciju. Početak rada stvaranjem [aplikacije logike sada](../app-service-logic/app-service-logic-create-a-logic-app.md).

## <a name="connect-to-service-bus"></a>Povezivanje s Bus servisa

Logiku aplikacije možete pristupiti bilo koji servis, najprije morate stvoriti veze sa servisom. [Veze](./connectors-overview.md) navedene veze između logike aplikacije i drugih servisa.  

>[AZURE.INCLUDE [Steps to create a connection to Azure Service Bus](../../includes/connectors-create-api-servicebus.md)]

## <a name="use-a-service-bus-trigger"></a>Koristite okidač Bus servisa

Okidač je događaja koji se mogu koristiti za pokretanje tijeka rada definirano u aplikaciji logike. [Dodatne informacije o okidača](../app-service-logic/app-service-logic-what-are-logic-apps.md#logic-app-concepts).  

>[AZURE.INCLUDE [Steps to create a Service Bus trigger](../../includes/connectors-create-api-servicebus-trigger.md)]  

## <a name="use-a-service-bus-action"></a>Korištenje servisa Bus akcija

Akciju je postupak provodi definirano u aplikaciji logike tijeka rada. [Dodatne informacije o akcije](../app-service-logic/app-service-logic-what-are-logic-apps.md#logic-app-concepts).

[AZURE.INCLUDE [Steps to create a Service Bus action](../../includes/connectors-create-api-servicebus-action.md)]  

## <a name="technical-details"></a>Tehničke pojedinosti

Evo pojedinosti o okidačima, akcije i odgovore koje podržava tu vezu.

### <a name="service-bus-triggers"></a>Servis Bus okidača

Servis Bus sastoji se od sljedećih okidača:  

|Pokretanje | Opis|
|--- | ---|
|[Kada je poruka primljena u redu čekanja](connectors-create-api-servicebus.md#when-a-message-is-received-in-a-queue)|Ovaj postupak pokrene u tijeku je po primitku poruke u redu.|
|[Kada je poruka primljena u temi pretplate](connectors-create-api-servicebus.md#when-a-message-is-received-in-a-topic-subscription)|Ovaj postupak pokrene u tijeku je po primitku poruke u temi pretplate.|


### <a name="service-bus-actions"></a>Servis Bus akcije

Servis Bus sastoji se od sljedećih akcija:


|Akcija|Opis|
|--- | ---|
|[Slanje poruke](connectors-create-api-servicebus.md#send-message)|Ovaj postupak pošalje poruku u redu čekanja ili temu.|
### <a name="action-and-trigger-details"></a>Detalji o akcija i okidača

Evo detalja za akcije i okidača za ovaj poveznik, zajedno s njihovih odgovora.



#### <a name="send-message"></a>Slanje poruke

|Naziv svojstva| Zaslonsko ime|Opis|
| ---|---|---|
|ContentData *|Sadržaj|Sadržaj poruke.|
|ContentType|Vrste sadržaja|Vrsta sadržaja sadržaja poruke.|
|Svojstva|Svojstva|Parovima ključnih vrijednosti za svaku brokered svojstvo.|
|entityName *|Naziv reda čekanja/teme|Naziv reda čekanja ili temu.|

Te napredne parametre i dostupne su:

|Naziv svojstva| Zaslonsko ime|Opis|
| ---|---|---|
|MessageId|Id poruke|Korisnički definirana vrijednost koja Bus servisa možete koristiti za pronalaženje duplikata poruka, ako je omogućeno.|
|Da biste|Da biste|Adresa da biste poslali.|
|OutboundSMTPServer|Odgovaranje na|Adresa reda čekanja da biste odgovorili.|
|ReplyToSessionId|Odgovor na sesiju ID-a|Identifikator sesiju da biste odgovorili.|
|Oznaka|Oznaka|Oznaka specifičnim aplikacijama.|
|ScheduledEnqueueTimeUtc|ScheduledEnqueueTimeUtc|Datum i vrijeme u UTC-a, kada poruka dodat će se u redu čekanja.|
|ID sesije|Id sesije|Identifikator sesiju.|
|CorrelationId|Id korelacije|Identifikator na korelacije.|
|TimeToLive|Vrijeme uživo|Trajanje, u crtice na osi, vrijedi poruke. Trajanje započinje s kada je poruka poslana Bus servisa.|



Programa * označava da je potrebno svojstvo.


#### <a name="when-a-message-is-received-in-a-queue"></a>Kada je poruka primljena u redu čekanja

|Naziv svojstva| Zaslonsko ime|Opis|
| ---|---|---|
|queueName *|Naziv reda čekanja|Naziv reda čekanja.|


Programa * označava da je potrebno svojstvo.


##### <a name="output-details"></a>Detalji o Izlaz

ServiceBusMessage: Taj objekt ima sadržaja i svojstva servisa Bus poruke.


| Naziv svojstva | Vrsta podataka | Opis |
|---|---|---|
|ContentData|niz|Sadržaj poruke.|
|ContentType|niz|Vrsta sadržaja sadržaja poruke.|
|Svojstva|objekt|Parovima ključnih vrijednosti za svaku brokered svojstvo.|
|MessageId|niz|Korisnički definirana vrijednost koja Bus servisa možete koristiti za pronalaženje duplikata poruka, ako je omogućeno.|
|Da biste|niz|Pošaljite na adresu.|
|OutboundSMTPServer|niz|Adresa reda čekanja da biste odgovorili.|
|ReplyToSessionId|niz|Identifikator sesiju da biste odgovorili.|
|Oznaka|niz|Oznaka specifičnim aplikacijama.|
|ScheduledEnqueueTimeUtc|niz|Datum i vrijeme u UTC-a, kada poruka dodat će se u redu čekanja.|
|ID sesije|niz|Identifikator sesiju.|
|CorrelationId|niz|Identifikator na korelacije.|
|TimeToLive|niz|Trajanje, u crtice na osi, vrijedi poruke. Trajanje započinje s kada je poruka poslana Bus servisa.|




#### <a name="when-a-message-is-received-in-a-topic-subscription"></a>Kada je poruka primljena u temi pretplate

|Naziv svojstva| Zaslonsko ime|Opis|
| ---|---|---|
|topicName *|Naziv teme|Naziv teme.|
|subscriptionName *|Naziv pretplate teme|Naziv pretplate temu.|


Programa * označava da je potrebno svojstvo.


##### <a name="output-details"></a>Detalji o Izlaz

ServiceBusMessage: Taj objekt ima sadržaja i svojstva servisa Bus poruke.


| Naziv svojstva | Vrsta podataka | Opis |
|---|---|---|
|ContentData|niz|Sadržaj poruke.|
|ContentType|niz|Vrsta sadržaja sadržaja poruke.|
|Svojstva|objekt|Parovima ključnih vrijednosti za svaku brokered svojstvo.|
|MessageId|niz|Korisnički definirana vrijednost koja Bus servisa možete koristiti za pronalaženje duplikata poruka, ako je omogućeno.|
|Da biste|niz|Pošaljite na adresu.|
|OutboundSMTPServer|niz|Adresa reda čekanja da biste odgovorili.|
|ReplyToSessionId|niz|Identifikator sesiju da biste odgovorili.|
|Oznaka|niz|Oznaka specifičnim aplikacijama.|
|ScheduledEnqueueTimeUtc|niz|Datum i vrijeme u UTC-a, kada poruka dodat će se u redu čekanja.|
|ID sesije|niz|Identifikator sesiju.|
|CorrelationId|niz|Identifikator na korelacije.|
|TimeToLive|niz|Trajanje, u crtice na osi, vrijedi poruke. Trajanje započinje s kada je poruka poslana Bus servisa.|



### <a name="http-responses"></a>HTTP odgovora

Prethodna akcije i okidača možete se vratiti jedan ili više sljedećih HTTP kodovima stanja:

|Ime|Opis|
|---|---|
|200|ok|
|202|Prihvatili|
|400|Neispravan zahtjev|
|401|Neovlašteno|
|403|Zabranjen|
|404|Nije pronađen|
|500|Interna pogreška poslužitelja. Došlo je do neočekivane pogreške.|
|Zadani|Nije uspjelo.|

## <a name="next-steps"></a>Daljnji koraci
[Stvaranje logike aplikacije](../app-service-logic/app-service-logic-create-a-logic-app.md).
