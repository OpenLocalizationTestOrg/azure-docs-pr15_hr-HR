<properties
   pageTitle="Pristupnik lokalnim podacima | Microsoft Azure"
   description="Pristupnik za lokalni uključeno je potrebno ako na poslužitelju komponente Analysis Services u Azure će se povezati s lokalnim izvorima podataka."
   services="analysis-services"
   documentationCenter=""
   authors="minewiskan"
   manager="erikre"
   editor=""
   tags=""/>
<tags
   ms.service="analysis-services"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="na"
   ms.date="10/24/2016"
   ms.author="owend"/>

# <a name="on-premises-data-gateway"></a>Pristupnik lokalnim podacima

Pristupnik za lokalnim podacima ponaša se kao most, koja omogućuje prijenos sigurne podataka između lokalnog izvora podataka i poslužitelj za Azure Analysis Services u oblaku.

Pristupnik je instaliran na računalo u vašoj mreži. Za svaki poslužitelj za Azure Analysis Services u pretplatu za Azure mora biti instaliran jedan pristupnik. Na primjer, ako imate dva poslužitelja u pretplatu za Azure povezivanje s lokalnim izvorima podataka, pristupnik mora biti instaliran na dva zasebna računala u vašoj mreži.

## <a name="requirements"></a>Preduvjeti

**Minimalni preduvjeti:**

- 4,5 .NET framework
- 64-bitnu verziju sustava Windows 7 i Windows Server 2008 R2 (ili noviji)

**Preporučuje se:**

- 8 core procesora
- 8 GB memorije
- 64-bitnu verziju sustava Windows 2012 R2 (ili noviji)

**Važne stavke:**

- Pristupnik nije moguće instalirati na kontroleru domene.

- Samo jedan pristupnik moguće je instalirati na jednom računalu.

- Pristupnika možete instalirati na računalo na kojem će ostati uključeni i prelazi u stanje mirovanja. Ako računalo nije na poslužitelj za Azure Analysis Services ne može povezati s lokalnim izvorima podataka da biste osvježili podatke.

- Instalirajte pristupnika na računalu bežično povezani s mrežom. Performanse mogu biti smanjiti.

- Da biste promijenili naziv poslužitelja za pristupnik za koje već konfigurirali, morate ponovno instalirati i konfigurirati pristupnik za nove.

- U nekim slučajevima tabličnim modelima povezivanje s izvorima podataka pomoću nativnog davatelja usluge kao što je SQL Server nativni klijent (SQLNCLI11) može vratiti pogrešku. Da biste saznali više, pogledajte [veze za izvor podataka](analysis-services-datasource.md).

## <a name="supported-on-premises-data-sources"></a>Podržani lokalnim izvorima podataka
Za pretpregled pristupnika podržava veze između poslužitelj za Azure Analysis Services i sljedeće lokalnim izvorima podataka:

- SQL Server
- SQL Data Warehouse
- WEB-APLIKACIJE APPS
- Oracle
- Teradata


## <a name="download"></a>Preuzimanje
 [Preuzimanje pristupnika](https://aka.ms/azureasgateway)


## <a name="install-and-configure"></a>Instaliranje i konfiguracija

1. Pokrenite instalaciju.

2. Odaberite mjesto za instalaciju i prihvatite licencne odredbe.

3. Prijavite se u Azure.

4. Navedite naziv poslužitelja za analizu Azure. Možete navesti samo jedan poslužitelj po pristupnika. Kliknite **Konfiguriraj** i spremni ste za slanje.

    ![Prijavite se u azure](./media\analysis-services-gateway\aas-gateway-configure-server.png)


## <a name="how-it-works"></a>Kako funkcionira
Pristupnik pokreće kao Windows usluga, **pristupnik lokalnim podacima**na računalu u mreži vaše tvrtke ili ustanove. Pristupnik instalirate za Azure Analysis Services temelji se na istom Pristupniku koristiti za druge servise kao što je Power BI, ali uz neke razlike u korištenju je konfiguriran.

![Kako funkcionira](./media/analysis-services-gateway/aas-gateway-how-it-works.png)

Upitima i podacima tijek rada ovako:

1.  Upit je stvorio servisa u oblaku s šifrirane vjerodajnica za lokalni izvor podataka. Zatim se šalje red za pristupnik za obradu.

2.  Servis u oblaku pristupnika analizira upita i ih gura zahtjev za [Bus servisa Azure](https://azure.microsoft.com/documentation/services/service-bus/).

3.  Pristupnik za lokalnim podacima polls na Azure Bus servisa za, na čekanju zahtjeva.

4.  Pristupnik dobiti upit, dešifrira vjerodajnice i povezuje s izvorima podataka pomoću tih vjerodajnica.

5.  Pristupnik šalje upit izvora podataka za izvršavanje.

6.  Rezultati se šalju iz izvora podataka natrag pristupnika, a zatim na servis u oblaku.

## <a name="windows-service-account"></a>Račun servisa Windows

Pristupnik za lokalnim podacima je konfiguriran za korištenje *NT SERVICE\PBIEgwService* za vjerodajnica za prijavu servisa Windows. Prema zadanim postavkama ima s desne strane prijava kao usluga; u kontekstu koje instalirate pristupnika na računalu. U ovom vjerodajnica nije isti račun koji se koristi za povezivanje s lokalnim izvorima podataka ili račun za Azure.  

Ako naiđete na probleme s proxy poslužiteljem zbog provjeru autentičnosti, preporučujemo vam da biste promijenili račun servisa Windows korisniku domene ili upravlja račun servisa.

## <a name="ports"></a>Priključci

Pristupnik stvara izlazne veze da biste Bus servisa Azure. Interes na izlazni priključke: TCP 443 (zadano), 5671, 5672, 9350 do 9354.  Pristupnik ne zahtijeva ulaznog priključci.

Preporučuje vam whitelist na IP adrese za vašu regiju podataka u vatrozida. [Microsoft Azure podatkovnog centra IP popisa](https://www.microsoft.com/download/details.aspx?id=41653)možete preuzeti. Ovaj popis tjednih će se ažurirati.

> [AZURE.NOTE]  IP adrese naveden na popisu Azure podatkovnog centra IP su CIDR notacijom. Na primjer, 10.0.0.0/24 znači 10.0.0.0 kroz 10.0.0.24. Dodatne informacije o [CIDR notacije](http://whatismyipaddress.com/cidr).

Slijede na potpuno kvalificiranih naziva domena pristupnik koristi.

|Nazivi domena|Izlazni priključci|Opis|
|---|---|---|
|*. powerbi.com|80|HTTP koji se koristi za preuzimanje instalacijskog programa.|
|*. powerbi.com|443|HTTPS|
|*. analysis.windows.net|443|HTTPS|
|*. login.windows.net|443|HTTPS|
|*. servicebus.windows.net|5671 5672|Napredne poruke stavljanje Protocol (AMQP)|
|*. servicebus.windows.net|443, 9350 9354|Slušače na servis preusmjeravanja Bus putem TCP (zahtijeva 443 za kontrolu pristupa tokena acquisition)|
|*. frontend.clouddatahub.net|443|HTTPS|
|*. core.windows.net|443|HTTPS|
|login.microsoftonline.com|443|HTTPS|
|*. msftncsi.com|443|Služi za testiranje internetska veza ako pristupnik nije dostupan servis Power BI.|
|*.microsoftonline p.com|443|Koristi se za provjeru autentičnosti ovisno o konfiguraciji.|


### <a name="forcing-https-communication-with-azure-service-bus"></a>Prisilno HTTPS komunikaciju s Bus servisa Azure

Možete nametnuti pristupnika možete komunicirati s Azure servisa Bus pomoću HTTPS umjesto Izravni TCP; No to možete znatno smanjiti performanse. Ćete morati izmijeniti *Microsoft.PowerBI.DataMovement.Pipeline.GatewayCore.dll.config* datoteku. Promijenite vrijednost iz `AutoDetect` da biste `Https`. U ovom se datoteka nalazi po zadanom pristupnika *C:\Program Files\On udruživanje s lokalnim podacima*.

```
<setting name="ServiceBusSystemConnectivityModeString" serializeAs="String">
    <value>Https</value>
</setting>
```


## <a name="troubleshooting"></a>Otklanjanje poteškoća
U odjeljku Napredne postavke, pristupnik za lokalnim podacima koristi za povezivanje servisa Azure Analysis Services s lokalnog izvora podataka je istom Pristupniku koristiti Power bi.

Ako imate poteškoća pri instalaciji i konfiguriranju pristupnika, obavezno potražite u članku [Otklanjanje poteškoća pristupnika za Power BI](https://powerbi.microsoft.com/documentation/powerbi-gateway-onprem-tshoot/). Ako mislite da imate problema s vatrozidom, potražite u odjeljcima vatrozid ili proxy poslužitelj.

Ako mislite da se pojavili proxy probleme s pristupnika, potražite u članku [Konfiguriranje postavki proxy poslužitelja za Power BI pristupnika](https://powerbi.microsoft.com/documentation/powerbi-gateway-proxy.md).

## <a name="next-steps"></a>Daljnji koraci
- [Upravljanje Analysis Services](analysis-services-manage.md)
- [Dohvaćanje podataka iz komponente Analysis Services Azure](analysis-services-connect.md)
