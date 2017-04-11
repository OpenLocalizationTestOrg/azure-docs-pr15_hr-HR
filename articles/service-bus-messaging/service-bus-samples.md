<properties 
    pageTitle="Razmjenu poruka servisa Bus uzoraka pregled | Microsoft Azure"
    description="Kategorizira, a zatim u članku se opisuje servisa Bus poruka uzorka s vezama na svakom."
    services="service-bus"
    documentationCenter="na"
    authors="sethmanheim"
    manager="timlt"
    editor="" />
<tags 
    ms.service="service-bus"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="na"
    ms.date="10/07/2016"
    ms.author="sethm" />

# <a name="service-bus-messaging-samples"></a>Bus usluge SMS uzorka

Uzorci razmjenu poruka servisa Bus demonstrirati ključne značajke u [Bus servis za razmjenu poruka](https://azure.microsoft.com/services/service-bus/) (u oblaku) i [Bus servisa za Windows Server](https://msdn.microsoft.com/library/dn282144.aspx). U ovom se članku kategoriziranje i opisuje uzoraka dostupne s vezama na svakom.

>[AZURE.NOTE] Uzorci Bus servisa nisu instalirane s SDK-a. Da biste nabavili uzorcima, posjetite [Azure SDK uzoraka stranice](https://code.msdn.microsoft.com/site/search?query=service%20bus&f%5B0%5D.Value=service%20bus&f%5B0%5D.Type=SearchText&ac=5).
>
>Uz to, postoji ažurirani skup servisa Bus poruka uzoraka [ovdje](https://github.com/Azure-Samples/azure-servicebus-messaging-samples) (od pisanja ovog oni ne opisane u ovom članku).  

Prijenos uzorka, potražite u članku [Bus usluge preusmjeravanja uzorka](../service-bus-relay/service-bus-relay-samples.md).

## <a name="service-bus-messaging"></a>Servis Bus poruka

Sljedeća primjera prikazuju kako napisati aplikacije koje koriste Bus usluge razmjene poruka.

Imajte na umu razmjenu uzoraka potrebno je niz za povezivanje da biste pristupili prostora za naziv Bus servisa.

### <a name="to-obtain-a-connection-string-for-azure-service-bus"></a>Da biste dobili niza za povezivanje za Bus servisa Azure

1. Prijavite se na [portal za Azure](http://portal.azure.com).

1. U stupcu na lijevoj strani kliknite **Bus servisa**.

1. Kliknite naziv svoje prostora za naziv na popisu.

3. U plohu prostor naziva kliknite **pravila za zajedničko korištenje programa access**.

4. U plohu **pravilnike pristup za zajedničko korištenje** kliknite **RootManageSharedAccessKey**.

6. Kopirajte niz za povezivanje u međuspremnik.

### <a name="to-obtain-a-connection-string-for-service-bus-for-windows-server"></a>Da biste dobili niza za povezivanje za Bus servisa za Windows Server

1. Pokrenite sljedeći cmdlet komponente PowerShell:

    ```
    get-sbClientConfiguration
    ```

2. Zalijepite niz za povezivanje u datoteku App.config uzorka.

### <a name="getting-started-samples"></a>Početak rada uzorka

Ta uzorka opisuju osnovne funkcije razmjene poruka.

|Primjer naziva|Opis|Minimalna SDK verzija|Dostupnost|
|---|---|---|---|
|[Početak rada: Poruka sa redovima](http://code.msdn.microsoft.com/Getting-Started-Brokered-aa7a0ac3)|Pokazuje kako koristiti Microsoft Azure servisa Bus za slanje i primanje poruka iz reda.|1.8|Bus servisa Microsoft Azure; Servis Bus za Windows Server|
|[Početak rada: Porukama pomoću teme](http://code.msdn.microsoft.com/Getting-Started-Brokered-614d42e5)|Pokazuje kako koristiti Microsoft Azure servisa Bus za slanje i primanje poruka iz temu s više pretplata.|1.8|Bus servisa Microsoft Azure; Servis Bus za Windows Server|

### <a name="exploring-features"></a>Istraživanje značajke

Sljedeća primjera prikazuju različite značajke Bus servisa.

|Primjer naziva|Opis|Minimalna SDK verzija|Dostupnost|
|---|---|---|---|
|[Davatelji HTTP tokena](http://code.msdn.microsoft.com/Service-Bus-HTTP-Token-38f2cfc5)|Pokazuje različitih načina provjere autentičnosti klijent za HTTP/OSTATKOM s Bus servisa.|2.1|Bus servisa Microsoft Azure; Servis Bus za Windows Server|
|[Servis Bus HTTP klijenta](http://code.msdn.microsoft.com/Service-Bus-HTTP-client-fe7da74a)|Pokazuje kako se poruke za slanje i primanje poruka iz servisa Bus putem HTTP/OSTALE.|2.3|Bus servisa Microsoft Azure; Servis Bus za Windows Server|
|[Autoforwarding Bus servisa](http://code.msdn.microsoft.com/Service-Bus-Autoforwarding-b9df470b)|Pokazuje kako automatsko prosljeđivanje poruka iz reda čekanja, pretplatu ili red deadletter u drugom redu čekanja ili temu. Pokazuje i upute za slanje poruke u redu čekanja ili temu putem reda čekanja za prijenos.|2.3|Bus servisa Microsoft Azure; Servis Bus za Windows Server|
|[Brokered poruka: Ogledna sesiju WCF kanala](http://code.msdn.microsoft.com/Brokered-Messaging-WCF-0a526451)|Pokazuje kako koristiti Bus servisa Microsoft Azure putem kanala Windows Communication Foundation (WCF). Uzorak prikazuje korištenje WCF kanala za slanje i primanje poruka putem servisa Bus reda. Uzorak prikazuje sesije i komunikacije koje nisu sesiju tijekom Bus servisa.|1.8|Bus servisa Microsoft Azure; Servis Bus za Windows Server|
|[Brokered poruka: transakcije](http://code.msdn.microsoft.com/Brokered-Messaging-8cd41d1e)|Atomically prikazano kako koristiti Bus Microsoft Azure usluge razmjene poruka značajke unutar opsega transakcije da bi bili serijama od poruka operacije izvršenja.|1.8|Bus servisa Microsoft Azure; Servis Bus za Windows Server|
|[Brokered poruka: Korištenje ostalih operacija upravljanja](http://code.msdn.microsoft.com/Brokered-Messaging-569cff88)|Pokazuje kako izvesti operacija upravljanja na servis Bus pomoću REST.|1.8|Bus servisa Microsoft Azure; Servis Bus za Windows Server|
|[Davatelja resursa REST API-ji](http://code.msdn.microsoft.com/Service-Bus-Resource-5d887203)|Pokazuje kako koristiti novi servis Bus RDFE REST API-ji za upravljanje prostora naziva i razmjenu entiteti.|1.8|Bus servisa Microsoft Azure; Servis Bus za Windows Server|
|[Brokered poruka: Ogledna sesiju WCF servisa](http://code.msdn.microsoft.com/Brokered-Messaging-WCF-db4262c2)|Pokazuje kako koristiti Bus servisa Microsoft Azure pomoću servisa modela WCF. Uzorak prikazuje korištenje WCF modela servisa za obavljanje temelji na sesiji komunikaciju putem servisa Bus reda.|1.8|Bus servisa Microsoft Azure; Servis Bus za Windows Server|
|[Brokered poruka: Odgovor na zahtjev](http://code.msdn.microsoft.com/Brokered-Messaging-Request-2b4ff5d8)|Pokazuje kako koristiti Bus za servis Microsoft Azure i funkciju zahtjeva i odgovora. Na primjer prikazuje jednostavne klijenata i poslužitelja komunikaciju putem servisa Bus reda.|1.8|Bus servisa Microsoft Azure; Servis Bus za Windows Server|
|[Brokered poruka: red Dead pismo](http://code.msdn.microsoft.com/Brokered-Messaging-Dead-22536dd8)|Pokazuje kako koristiti Microsoft Azure servisa Bus i razmjenu funkcija "reda čekanja dead slovo". Na primjer prikazuje jednostavne pošiljatelja i primatelja komunikaciju putem servisa Bus reda.|1.8|Bus servisa Microsoft Azure; Servis Bus za Windows Server|
|[Brokered poruka: Odgođeni poruke](http://code.msdn.microsoft.com/Brokered-Messaging-ccc4f879)|Pokazuje kako koristiti značajku deferral poruka sustava Microsoft Azure servisa Bus. Na primjer prikazuje jednostavne pošiljatelja i primatelja komunikaciju putem servisa Bus reda.|1.8|Bus servisa Microsoft Azure; Servis Bus za Windows Server|
|[Brokered poruka: Sesiju poruke](http://code.msdn.microsoft.com/Brokered-Messaging-Session-41c43fb4)|Pokazuje kako koristiti Microsoft Azure servisa Bus i funkcije sesiju razmjene poruka. Na primjer prikazuje jednostavne pošiljatelja i primatelja komunikaciju putem servisa Bus reda.|1.8|Bus servisa Microsoft Azure; Servis Bus za Windows Server|
|[Brokered poruka: Tema odgovor za zahtjev za](http://code.msdn.microsoft.com/Brokered-Messaging-Request-6759a36e)|Pokazuje kako implementirati uzorak zahtjeva i odgovora pomoću tema Bus usluge za Microsoft Azure i pretplate. Na primjer prikazuje jednostavne klijenata i poslužitelja komunikaciju putem servisa Bus temu.|1.8|Bus servisa Microsoft Azure; Servis Bus za Windows Server|
|[Brokered poruka: Red čekanja odgovor zahtjeva](http://code.msdn.microsoft.com/Brokered-Messaging-Request-0ce8fcaf)|Pokazuje kako koristiti Microsoft Azure servisa Bus i funkcije zahtjeva i odgovora. Na primjer prikazuje jednostavne klijenata i poslužitelja komunikaciju putem dva reda čekanja Bus servisa.|1.8|Bus servisa Microsoft Azure; Servis Bus za Windows Server|
|[Brokered poruka: Duplikata](http://code.msdn.microsoft.com/Brokered-Messaging-c0acea25)|Pokazuje kako koristiti otkrivanje duplikata poruka Microsoft Azure servisa Bus sa redovima. Stvara dva reda čekanja, sa duplikata omogućeno i druge bez duplikata.|1.8|Bus servisa Microsoft Azure; Servis Bus za Windows Server|
|[Brokered poruka: Poruka je asinkrone](http://code.msdn.microsoft.com/Brokered-Messaging-Async-211c1e74)|Pokazuje kako koristiti Microsoft Azure servisa Bus za slanje i primanje poruka asinkrono iz reda. Red čekanja omogućuje samostalne, asinkronog komunikaciju između pošiljatelja i bilo koji broj primatelja (ovdje jednog tekstnog okvira).|1.8|Bus servisa Microsoft Azure; Servis Bus za Windows Server|
|[Brokered poruka: Napredni filtri](http://code.msdn.microsoft.com/Brokered-Messaging-6b0d2749)|Pokazuje kako koristiti Microsoft Azure servisa Bus objavljivanja/pretplate napredni filtri. On stvara 3 pretplate i teme s različitim filtar definicijama šalje poruke na temu, a prima sve poruke iz pretplate.|1.8|Bus servisa Microsoft Azure; Servis Bus za Windows Server|
|[Brokered poruka: Pretpreuzimanje poruke](http://code.msdn.microsoft.com/Brokered-Messaging-be2dac1d)|Pokazuje kako koristiti značajku za pretpreuzimanje Bus usluge za Microsoft Azure poruke. Pokazuje kako koristiti značajku pretpreuzimanje poruke nakon primanja.|1.8|Bus servisa Microsoft Azure; Servis Bus za Windows Server|

## <a name="service-bus-reference-tools"></a>Alati za servis Bus reference

Sljedeća primjera prikazuju različite značajke servisa.

|Primjer naziva|Opis|Minimalna SDK verzija|Dostupnost|
|---|---|---|---|
|[Servis Bus Explorer](http://code.msdn.microsoft.com/Service-Bus-Explorer-f2abca5a)|Explorer Bus servisa korisnicima omogućuje povezivanje servisa Bus servisa prostor naziva i upravljanje razmjenu entiteti jednostavan način. Alat daje napredne značajke kao što su uvoz/izvoz funkcijama i mogućnost da biste testirali razmjenu entiteti i usluge preusmjeravanja.|1.8|Bus servisa Microsoft Azure; Servis Bus za Windows Server|
|[Autorizacije: SBAzTool](http://code.msdn.microsoft.com/Authorization-SBAzTool-6fd76d93)|Ovaj primjer pokazuje kako stvoriti i upravljanje identiteti servisa Microsoft Azure Active Directory kontrole pristupa (poznat i kao servisa za kontrolu pristupa ili ACS) za korištenje s Bus servisa.|N/D|Bus servisa Microsoft Azure|

## <a name="next-steps"></a>Daljnji koraci

U sljedećim temama konceptualni pregled Bus servisa.

- [Bus usluge SMS pregled](service-bus-messaging-overview.md)
- [Servis Bus arhitekture](service-bus-architecture.md)
- [Osnove Bus servisa](service-bus-fundamentals-hybrid-solutions.md)
