<properties 
    pageTitle="Pomoću Upravitelja veze hibridnog | Microsoft Azure" 
    description="Instaliranje i konfiguriranje Upravitelja veze hibridnog i povezivanje s lokalnim poveznika u aplikacijama logika" 
    services="app-service\logic" 
    documentationCenter=".net,nodejs,java"
    authors="MandiOhlinger" 
    manager="anneta" 
    editor=""/>

<tags 
    ms.service="logic-apps" 
    ms.workload="integration" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/18/2016" 
    ms.author="mandia"/>

# <a name="connect-to-on-premises-connectors-using-the-hybrid-connection-manager"></a>Povezivanje s lokalnim poveznika pomoću Upravitelja veze hibridnog

>[AZURE.NOTE] Ovu verziju članka odnosi logike aplikacije 2014. – 12-01 – pregled shema verziju. Logika aplikacije Općenito dostupan (GA) koristi pristupnik za povezivanje s lokalnim. Dodatne informacije o novog [pristupnika](app-service-logic-gateway-connection.md) i [Logika aplikacije GA](https://azure.microsoft.com/documentation/services/logic-apps/).

Da biste koristili lokalnim sustavom, logike aplikacije koristi Upravitelja veze hibridnog. Neke poveznike možete povezati s lokalnog sustava, kao što je SQL Server, SAP, sustava SharePoint i tako dalje. 

Upravitelj veze hibridnog (HCM) je klika-jednom installer koji je instaliran na poslužitelj za IIS u mreži, iza vatrozida. Korištenje programa preusmjeravanja Bus servisa Azure HCM potvrđuje lokalnog sustava s Connector u Azure. 

> [AZURE.NOTE] Upravitelj veze hibridnog potreban je samo ako se povezujete s programa lokalnog resursa iza vatrozida. Ako se ne povezujete s lokalnim sustavom, ne morate Upravitelja veze hibridnog.

Da bismo započeli, potrebno je:

- Azure Bus usluge preusmjeravanja prostor naziva SAS niz za povezivanje. U odjeljku [Cijena Bus servisa](https://azure.microsoft.com/pricing/details/service-bus/) da biste utvrdili koji razina obuhvaća preusmjeravanje.
- Lokalni sustav prijavu podatke, uključujući korisničko ime i lozinku. Ako, na primjer, ako se povezujete s poslužiteljem SQL lokalnog, morate SQL Server prijava račun i lozinku.
- Lokalni poslužitelj podatke, uključujući port (priključak) broj i naziv poslužitelja. Na primjer, ako se povezujete s poslužiteljem SQL lokalnog, morat ćete naziv SQL Server i broj priključka TCP.

## <a name="get-the-service-bus-connection-string"></a>Dohvaćanje niza za povezivanje servisa Bus

Na portalu Azure kopirajte korijen servisa Bus SAS niz za povezivanje. Niz za povezivanje povezuje poveznik za Azure s lokalnim sustavom. 

1. [Azure klasičnih portala](http://go.microsoft.com/fwlink/p/?LinkID=213885)odaberite mjesto za naziv Bus servisa i odaberite **Podatke o vezi**:

    ![][SB_ConnectInfo]

2. Kopirajte niz za povezivanje SAS:

    ![][SB_SAS]

## <a name="install-the-hybrid-connection-manager"></a>Instalacija Upravitelja veze hibridnog

1. [Portal za Azure](http://go.microsoft.com/fwlink/p/?LinkID=525040)odaberite poveznik koji ste stvorili. Da biste ga otvorili, odaberite **Pregledaj**, odaberite **API aplikacije**, a zatim poveznika ili aplikacije API-JA. 
<br/><br/>
Postavljanje nije **Hibridnog veze**, **Potpuna**:
<br/>
![][2] 

2. Odaberite **vezu za hibridno**. Niz za povezivanje servisa Bus koju ste prethodno unijeli nalazi.
3. Kopirajte **niz primarni konfiguracije**:
<br/>
![][PrimaryConfigString]

4. U odjeljku **Upravitelj veze s lokalnog hibridnog**, možete preuzeti Upravitelj veze hibridnog ili ga instalirati izravno na portalu. 
<br/><br/>
Da biste instalirali izravno na portalu, otvorite s poslužiteljem za IIS lokalnog otvorite portal i odaberite **Preuzimanje i konfiguracija**.
<br/><br/>
Da biste preuzeli Upravitelja veze hibridnog, idite na lokalni poslužitelj za IIS pa kliknite **ClickOnce aplikacije** (http://hybridclickonce.azurewebsites.net/install/Microsoft.Azure.BizTalk.Hybrid.ClickOnce.application). Instalacija automatski se pokreće tako da možete ga pokrenuti.

5. U prozoru **Postavljanje ga Slušatelj** unesite **Primarni niz konfiguracije** koje ste prethodno zalijepili (u koraku 3), a zatim odaberite **Instalacija**.

Kada je dovršena postavljanje, prikazuje sljedeće:
<br/>
![][3] 

Sada kada pregledavate Internet u Connector ponovno, stanje veze hibridnog je **povezan**. Možda ćete morati zatvoriti poveznik i ponovno je otvorite:
<br/>
![][4] 

> [AZURE.NOTE] Da biste prešli na sekundarnom veza_niz, ponovno pokrenite instalacijski program za hibridno vezu, a zatim unesite **Sekundarnu konfiguracije niz**.


## <a name="tcp-ports-and-security"></a>TCP priključci i sigurnost

Prilikom stvaranja hibridnog veze web-mjesta stvara se na poslužitelj za IIS lokalne lokalnog. U na DMZ može biti IIS poslužitelj. TCP priključke na poslužitelju IIS obuhvaćaju sljedeće:

TCP priključak | Zašto
--- | ---
 | Potrebni su dolazne TCP priključci.
9350 - 9354 | Sljedeće priključke koriste se za prijenos podataka. Upravitelj preusmjeravanja servisa Bus probes port 9350 da biste odredili TCP povezivanje dostupna. Ako je dostupan, zatim pretpostavlja se da priključak 9352 dostupan je i. Promet podataka prelazi preko priključak 9352. <br/><br/>Dopusti izlazne veze za priključke.
5671 | Kada priključak 9352 koristi promet podataka, priključak 5671 se koristi kao u kontrolu kanala. <br/><br/>Dopusti izlazne veze s tog priključka. 
80, 443 | Ako su priključke 9352 i 5671 upotrebljivosti, *zatim* priključke 80 i 443 su pričuvni priključke za prijenos podataka i kontrola kanala.<br/><br/>Dopusti izlazne veze za priključke.
Lokalno priključak sustava | Lokalni sustav otvorite priključak koji se koristi za sustav. Ako je, primjerice, SQL Server obično koristi priključak 1433. Otvorite TCP priključak.

[Hostiranje iza vatrozida s Bus servisa](http://msdn.microsoft.com/library/azure/ee706729.aspx)

## <a name="troubleshooting"></a>Otklanjanje poteškoća

![][HCMFlow]

### <a name="on-premises-troubleshooting"></a>Lokalni otklanjanje poteškoća

1. Na poslužitelju IIS, potvrdite je instaliran ulogu IIS web i sve servise za IIS pokrenute.
2. Na poslužitelju IIS potvrdite Upravitelja veze hibridnog je instalirana i pokrenuta:
 - U IIS Manager (inetmgr), ***MicrosoftAzureBizTalkHybridListener*** web-mjesta mora biti naveden i imati. 
 - Ovo web-mjesto koristi ***HybridListenerAppPool*** koji se izvodi kao račun za lokalni korisnik ugrađene *mrežna usluga sve* . Ovaj AppPool mora biti pokrenut.
3. Na poslužitelju IIS potvrdite poveznik je instalirana i pokrenuta: 
 - Web-mjesta stvara sustava poveznik. Na primjer, ako ste stvorili SQL connector, ne postoji ***MicrosoftSqlConnector_nnn*** web-mjesta. U IIS Manager (inetmgr), provjerite to web-mjesto je na popisu i s radom. 
 - Ovo web-mjesto koristi vlastitu aplikacija IIS pod nazivom ***HybridAppPoolnnn***. U ovom AppPool pokreće kao lokalni ugrađene korisnički račun *mrežna usluga sve* . U ovom web-mjesta i AppPool oba počne. 
 - Pronađite lokalne poveznik. Na primjer, ako poveznika web-mjesta koristi priključak 6569, pregledavati se http://localhost:6569. Zadani dokument nije konfiguriran tako da se `HTTP Error 403.14 - Forbidden error` se očekuje.
4. U vatrozid, provjerite je li otvoreno TCP priključci navedena u ovoj temi.
5. Pregled sustava izvor ili odredište:
 - Neki lokalni sustavi zahtijevaju dodatne ovisnost datoteke. Na primjer, ako se povezujete s lokalnim SAP, neke dodatne SAP datoteke mora biti instaliran na poslužitelju za IIS.
 - Provjerite povezivanje sa sustavom pomoću računa za prijavu. Na primjer, TCP priključak koji se koristi za sustav mora biti otvoren, kao što je priključak 1433 za SQL Server. Prijava račun koji ste unijeli na portalu za Azure mora imati pristup sustavu.
6. Na poslužitelju IIS u zapisnicima događaja za sve pogreške. 
7. Čišćenje i ponovna instalacija upravitelja veze hibridnog: 
 - U IIS-ručno izbrisati poveznika web-mjesta i njezin aplikacija. 
 - Ponovno pokrenite upravitelj veze hibridnog i provjerili unosa točan **Primarni konfiguracije niz** za vaše poveznik.



### <a name="in-the-azure-classic-portal"></a>Na portalu za Azure klasični

1. Provjerite je li servis Bus prostora za naziv je u stanju **Aktivno** .
2. Kada stvorite poveznik, unesite niz za povezivanje servisa Bus SAS. Unesite niz za povezivanje ACS.


## <a name="faq"></a>NAJČEŠĆA PITANJA

**PITANJE**: postoje dvije upravitelji hibridnog veze. Koja je razlika? 

**Odgovor**: postoji tehnologija [Hibridnog veze](../biztalk-services/integration-hybrid-connection-overview.md) koja koristi prvenstveno web-aplikacije (stari mu je naziv web-mjesta) i mobilne aplikacije (bivši mobilne usluge) za povezivanje s lokalnim. Ovaj Upravitelj veze hibridnog vlastitu [Postavljanje](../biztalk-services/integration-hybrid-connection-create-manage.md) i koristi na servis BizTalk Azure (u pozadini). Podržava samo protokoli TCP i HTTP-a.

Pomoću poveznika aplikacije servisa za Azure i imamo Upravitelj hibridnog veza.  Ovaj hibridnog Upravitelja veze ne *koristi servisa Azure BizTalk (u pozadini)* i podržava više od protokoli TCP i HTTP. U odjeljku [poveznika i popisa aplikacija API-JA](app-service-logic-connectors-list.md).

Povezivanje s lokalnim sustavom koristiti Bus servisa Azure.

**PITANJE**: kada je li moguće stvoriti prilagođene aplikacije API-JA, možete koristiti Upravitelja veze za hibridno aplikacije servisa za povezivanje s lokalnim? 

**Odgovor**: nije u tradicionalni odgovara. Možete koristiti ugrađeno poveznika, konfigurirati vezu Upravitelj aplikacija hibridnog servisa za povezivanje s lokalnim sustavom. Nakon toga koristite ovaj poveznik s prilagođene aplikacije API-JA, vjerojatno pomoću aplikacije za logika. Trenutno nije moguće razviti ili stvorite vlastitu hibridnog API aplikacije (kao što je SQL poveznika ili poveznik datoteka).

Ako prilagođene API-JA koristi priključak TCP i HTTP, možete koristiti [Hibridno veze](../biztalk-services/integration-hybrid-connection-overview.md) i njegova Upravitelja veze hibridnog. U ovom scenariju programa Azure BizTalk se servis koristi. [Povezivanje s lokalnog sustava SQL Server iz web-aplikacije](../app-service-web/web-sites-hybrid-connection-connect-on-premises-sql-server.md) mogu pomoći.  


## <a name="read-more"></a>Saznajte više

[Praćenje aplikacija u logike](app-service-logic-monitor-your-logic-apps.md)<br/>
[Bus cijene za servis](https://azure.microsoft.com/pricing/details/service-bus/)



[SB_ConnectInfo]: ./media/app-service-logic-hybrid-connection-manager/SB_ConnectInfo.png
[SB_SAS]: ./media/app-service-logic-hybrid-connection-manager/SB_SAS.png
[PrimaryConfigString]: ./media/app-service-logic-hybrid-connection-manager/PrimaryConfigString.png
[HCMFlow]: ./media/app-service-logic-hybrid-connection-manager/HCMFlow.png
[2]: ./media/app-service-logic-hybrid-connection-manager/BrowseSetupIncomplete.jpg
[3]: ./media/app-service-logic-hybrid-connection-manager/HybridSetup.jpg
[4]: ./media/app-service-logic-hybrid-connection-manager/BrowseSetupComplete.jpg

 
