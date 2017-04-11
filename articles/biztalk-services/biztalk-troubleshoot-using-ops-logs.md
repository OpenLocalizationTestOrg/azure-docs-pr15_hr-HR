<properties 
    pageTitle="Otklanjanje poteškoća s BizTalk servisa pomoću zapisnika operacija | Microsoft Azure" 
    description="Otklanjanje poteškoća s BizTalk servisa pomoću postupka zapisnika. MABS, WABS" 
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
    ms.topic="article" 
    ms.date="08/15/2016" 
    ms.author="mandia"/>


# <a name="biztalk-services-troubleshoot-using-operation-logs"></a>Servisi za BizTalk: Rješavanje problema s korištenjem zapisnik postupka

## <a name="what-are-the-operation-logs"></a>Što su zapisnike operacija
Zapisnik postupka je značajka Management Services dostupna Azure klasični portalu koji vam omogućuje da zapisnicima povijesne operacije obavljene Azure Services, uključujući BizTalk usluge. To vam omogućuje da vidite povijesne podatke vezane uz upravljanje operacije pretplate BizTalk servis as far back as 180 dana.

> [AZURE.NOTE]Značajka samo snima zapisnika za upravljanje operacije na BizTalk servisa, kao što su kada je pokrenuta servis sigurnosno prema gore, i tako dalje. Operacije evidentiraju se bez obzira je li se izvode na portalu Azure klasični ili pomoću [BizTalk servis REST API -ji](http://msdn.microsoft.com/library/azure/dn232347.aspx). Popis svih operacije koje prate pomoću Management Services potražite u članku [Operacije evidentirane pomoću servisa Azure Management Services](#bizops).<br/><br/>
To snimite u zapisnicima aktivnosti povezanih izvođenje servisa BizTalk (kao što su poruka obradili bridges itd.). Da biste vidjeli ove zapisnike, koristite prikaz praćenje na portalu servisa BizTalk. Dodatne informacije potražite u članku [Praćenje poruka](http://msdn.microsoft.com/library/azure/hh949805.aspx).

## <a name="view-biztalk-services-operation-logs"></a>Prikaz BizTalk Services zapisnik postupka
1. Azure klasični portalu odaberite **Management Services**, a zatim na kartici **Zapisnici operacija** .
2. Možete filtrirati zapisnike koji se temelji na različitim parametara kao što su pretplate, raspon datuma, Vrsta usluge (primjerice BizTalk Services), naziv servisa ili stanje operacije (je uspio, nije uspjelo).
3. Odaberite kvačicu da biste prikazali popis filtrirane. Sljedeća slika prikazuje aktivnosti povezanih testbiztalkservice:  ![zapisnicima operacije][ViewLogs] 
4. Da biste pogledali dodatne informacije o određene operacije, odaberite redak, a zatim kliknite **Detalji** u programske trake pri dnu.


## <a name="bizops"></a>Operacije prate pomoću servisa Azure upravljanja
U sljedećoj su tablici navedeni operacije koje prate pomoću servisa Azure upravljanja:

Naziv operacije | Zadatak
--- | ---
CreateBizTalkService | Postupak da biste stvorili novi BizTalk servis
DeleteBizTalkService | Postupak da biste izbrisali BizTalk servisa
RestartBizTalkService | Ponovno pokrenite servis za BizTalk operacije
StartBizTalkService | Postupak za pokretanje servisa BizTalk
StopBizTalkService | Operaciju koja će se zaustaviti servis za BizTalk
DisableBizTalkService | Postupak onemogućivanja BizTalk servisa
EnableBizTalkService | Postupak da biste omogućili BizTalk servisa
BackupBizTalkService | Postupak za sigurnosno kopiranje BizTalk servisa
RestoreBizTalkService | Operaciju koja će se vratiti na servis BizTalk navedeni sigurnosne kopije
SuspendBizTalkService | Postupak za privremeno obustavljanje BizTalk servisa
ResumeBizTalkService | Postupak da biste nastavili BizTalk servisa
ScaleBizTalkService | Postupak za promjenu veličine na servis BizTalk prema gore ili dolje
ConfigUpdateBizTalkService | Postupak ažuriranja konfiguracija BizTalk servisa
ServiceUpdateBizTalkService | Postupak nadogradnje ili prijeći na nižu BizTalk servis za neku drugu verziju
PurgeBackupBizTalkService | Operacije čišćenja sigurnosne kopije servisa BizTalk izvan razdoblje zadržavanja


## <a name="see-also"></a>Vidi također
- [Sigurnosno kopiranje BizTalk servisa](http://go.microsoft.com/fwlink/p/?LinkID=325584)
- [Vraćanje servisa BizTalk iz sigurnosne kopije](http://go.microsoft.com/fwlink/p/?LinkID=325582)
- [Servisi za BizTalk: Za razvojne inženjere, Basic, standardna i Premium izdanja grafikona](http://go.microsoft.com/fwlink/p/?LinkID=302279)
- [BizTalk servisa: Azure pomoću portala za klasični dodjele resursa](http://go.microsoft.com/fwlink/p/?LinkID=302280)
- [Servisi za BizTalk: Dodjeljivanje Status grafikona](http://go.microsoft.com/fwlink/p/?LinkID=329870)
- [BizTalk servisa: Kartica nadzorne ploče, Monitor i skaliranje](http://go.microsoft.com/fwlink/p/?LinkID=302281)
- [BizTalk usluge: ograničavanje](http://go.microsoft.com/fwlink/p/?LinkID=302282)
- [BizTalk servisa: Naziv izdavača i izdavatelj ključ](http://go.microsoft.com/fwlink/p/?LinkID=303941)
- [Kako pokrenuti pomoću Azure BizTalk Services SDK](http://go.microsoft.com/fwlink/p/?LinkID=302335)

[ViewLogs]: ./media/biztalk-troubleshoot-using-ops-logs/Operation-Logs.png
 
