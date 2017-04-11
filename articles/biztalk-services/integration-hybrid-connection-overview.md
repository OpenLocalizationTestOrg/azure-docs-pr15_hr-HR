<properties
    pageTitle="Pregled veza hibridnog | Microsoft Azure"
    description="Saznajte više o hibridnog veze, sigurnost, TCP priključci i podržanim konfiguracije. MABS, WABS."
    services="biztalk-services"
    documentationCenter=""
    authors="MandiOhlinger"
    manager="erikre"
    editor=""/>

<tags
    ms.service="biztalk-services"
    ms.workload="integration"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="10/18/2016"
    ms.author="ccompy"/>


# <a name="hybrid-connections-overview"></a>Pregled hibridnog veza
Uvod u veze hibridnog popisa podržanih konfiguracija i popise potrebne TCP priključci.


## <a name="what-is-a-hybrid-connection"></a>Što je hibridno vezu

Hibridno veze su značajke servisa Azure BizTalk. Hibridno veze pružaju lako i jednostavan način značajke web-aplikacije u servisu Azure aplikacije (stari mu je naziv web-mjesta) i na mobilne aplikacije u servisu Azure aplikacije (bivši Mobile Services) s lokalnog resursa iza vatrozida.

![Hibridno veze][HCImage]

Prednosti veze hibridnog obuhvaćaju sljedeće:

- Web-aplikacije i mobilne aplikacije mogu pristupiti postojeće lokalnih podataka i servise sigurno.
- Više web-aplikacijama ili mobilne aplikacije možete zajednički koristiti hibridno veze za pristup resursu za lokalni.
- Minimalna TCP priključci su potrebne za pristup mreži.
- Aplikacije koje koriste veze hibridnog pristup samo određenim lokalnim resurs koji se objavljuje putem veze za hibridno.
- Možete povezati s bilo kojeg lokalnog resursa koji koristi statički TCP priključak, kao što su SQL Server, MySQL, HTTP Web API-ji i većina prilagođene web-servisa.

    > [AZURE.NOTE] Usluge utemeljene na TCP koje koriste dinamičku priključke (kao što su FTP pasivni načinu ili prošireni pasivni) trenutno nije podržano. LDAP i nije podržana. LDAP koristi statički TCP priključak, ali ga nije moguće koristiti UDP. Zbog toga nije podržan.

- Možete koristiti u sklopu svi okviri podržavaju Web Apps (.NET, PHP, Java, Python, Node.js) i mobilne aplikacije (Node.js, .NET).
- Web-aplikacije i mobilne aplikacije mogu pristupiti lokalnog resursa na isti način kao da je argument weba ili mobilnu aplikaciju nalazi se u lokalnoj mreži. Ako, na primjer, na isti veze niz koji se koristi lokalni možete koristit i Azure.


Veza za hibridno omogućuju administratorima u tvrtki s kontrola i uvid u tvrtke resursima pristupati hibridnog aplikacije, uključujući:

- Postavke pravilnika grupe za korištenje, administratori mogu Dopusti hibridno veze na mreži i i odrediti resursa koji mogu pristupiti hibridnog aplikacije.
- Događaj i nadzornog zapisnika na mreži tvrtke nude uvid u resursima pristupati hibridnog veze.


## <a name="example-scenarios"></a>Ogledni scenariji

Povezivanje hibridnog podržava sljedeće kombinacije framework i aplikacije:

- .NET framework pristup sustava SQL Server
- .NET framework pristup servisima HTTP/HTTPS s WebClient
- Pristup PHP MySQL SQL Server
- Pristup Java SQL Server, MySQL i Oracle
- Pristup Java HTTP/HTTPS servisima

Prilikom korištenja hibridnog veze za pristup lokalnog sustava SQL Server, imajte na umu sljedeće:

- Da biste koristili statične priključke, morate ga konfigurirati SQL Express instance pod nazivom. Prema zadanim postavkama SQL Express pod nazivom instance dinamički priključke za korištenje.
- SQL Express instance zadani koristiti statične port (priključak), ali TCP mora biti omogućen. Prema zadanim postavkama, TCP nije omogućen.
- Prilikom korištenja Clustering ili grupe dostupnosti u `MultiSubnetFailover=true` načinu rada trenutno nije podržano.
- Na `ApplicationIntent=ReadOnly` trenutno nije podržano.
- SQL provjeru autentičnosti može biti obavezno metodu završetka do kraja autorizacije podržava Azure aplikacije i lokalnog sustava SQL server.


## <a name="security-and-ports"></a>Sigurnost i priključci

Hibridno veze pomoću zajednički pristup potpis (SAS) autorizacije sigurne veze iz Azure aplikacije i Upravitelja veze za hibridno lokalnog hibridnog vezu. Tipke zasebnu vezu se stvaraju za aplikaciju i upravitelju lokalnog hibridnog veze. Ove veze tipke možete poslednjeg i povučen zasebno.

Hibridno veze omogućavaju objedinjenog i sigurne distribucija tipke za aplikacije i upravitelju lokalnog hibridnog veze.

Potražite u članku [Stvaranje i upravljanje vezama hibridnog](integration-hybrid-connection-create-manage.md).

*Aplikacije autorizacije razlikuje se od hibridnog veze*. Bilo koji način prikladne autorizacije može se koristiti. Način autorizacije ovisi o metode završetka do kraja autorizacije podržane putem oblaka Azure i lokalnih komponente. Na primjer, Azure aplikacije pristupa programa lokalnog sustava SQL Server. U ovom scenariju SQL autorizacije možda autorizacije metoda koja je podržana završetka do kraja.

#### <a name="tcp-ports"></a>TCP priključci
Veze hibridnog zahtijevaju samo izlazni TCP i HTTP povezivanje s mrežom privatno. Ne morate otvarati priključaka u vatrozidu ili mijenjati mrežna konfiguracija opseg da biste omogućili sve ulaznog povezivanja u vašoj mreži.

Hibridno veze koristi sljedeće TCP priključke:

Priključak | Zašto je potrebna
--- | ---
9350 - 9354 | Sljedeće priključke koriste se za prijenos podataka. Upravitelj preusmjeravanja servisa Bus probes port 9350 da biste odredili TCP povezivanje dostupna. Ako je dostupan, zatim pretpostavlja se da priključak 9352 dostupan je i. Promet podataka prelazi preko priključak 9352. <br/><br/>Dopusti izlazne veze za priključke.
5671 | Kad se koristi priključak 9352 za promet podataka, priključak 5671 koristi se kao kontrola kanala. <br/><br/>Dopusti izlazne veze s tog priključka.
80, 443 | Priključke koriste se za neke podatke zahtjevima za Azure. Osim toga, ako priključke 9352 i 5671 ne mogu koristiti, *zatim* priključke 80 i 443 su pričuvni priključke za prijenos podataka i kontrolu kanala.<br/><br/>Dopusti izlazne veze za priključke. <br/><br/>**Napomena** Ne preporučuje se koristiti kao pričuvni priključke umjesto u TCP priključci. HTTP/WebSocket služi kao protokol umjesto nativni TCP kanala podataka. To može rezultirati u donjem performansi.



## <a name="next-steps"></a>Daljnji koraci

[Stvaranje i upravljanje vezama hibridnog](integration-hybrid-connection-create-manage.md)<br/>
[Povezivanje Azure web-aplikacijama programa lokalnog resursa](../app-service-web/web-sites-hybrid-connection-get-started.md)<br/>
[Povezivanje s lokalnog sustava SQL Server Azure web-aplikacije programa](../app-service-web/web-sites-hybrid-connection-connect-on-premises-sql-server.md)<br/>


## <a name="see-also"></a>Vidi također

[REST API-JA za upravljanje uslugama BizTalk na Microsoft Azure](http://msdn.microsoft.com/library/azure/dn232347.aspx)
[BizTalk usluge: izdanja grafikona](biztalk-editions-feature-chart.md)<br/>
[Stvaranje BizTalk servisa pomoću portala za Azure](biztalk-provision-services.md)<br/>
[BizTalk servisa: Kartica nadzorne ploče, Monitor i skaliranje](biztalk-dashboard-monitor-scale-tabs.md)<br/>

[HCImage]: ./media/integration-hybrid-connection-overview/WABS_HybridConnectionImage.png
[HybridConnectionTab]: ./media/integration-hybrid-connection-overview/WABS_HybridConnectionTab.png
[HCOnPremSetup]: ./media/integration-hybrid-connection-overview/WABS_HybridConnectionOnPremSetup.png
[HCManageConnection]: ./media/integration-hybrid-connection-overview/WABS_HybridConnectionManageConn.png
