<properties 
   pageTitle="Upozorenje upravljanje u programu Microsoft nadzor proizvodi | Microsoft Azure"
   description="Upozorenje upućuje na to neki problem koje je potrebno obratiti pozornost od administratora.  U ovom se članku opisuje razlike u kako stvoriti i upravlja u sustavu centar operacije Manager (SCOM) i prijava analitiku upozorenja i sadrži korisni Savjeti za korištenje Proizvodi za upozorenja upravljanje strategije hibridnog rad." 
   services="operations-management-suite"
   documentationCenter=""
   authors="bwren"
   manager="jwhit"
   editor="tysonn" />
<tags 
   ms.service="operations-management-suite"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="09/06/2016"
   ms.author="bwren" />

# <a name="managing-alerts-with-microsoft-monitoring"></a>Upravljanje upozorenjima s Microsoft nadzora 

Upozorenje upućuje na to neki problem koje je potrebno obratiti pozornost od administratora.  Postoje jasne razlike između sustava centra operacije Manager (SCOM) i prijava analitiku u paketu operacije upravljanja (OMS) kako se stvaraju upozorenja, kako se upravlja i analizirati i kako ćete obavijest da kritičnih problema otkrivena.

## <a name="alerts-in-operations-manager"></a>Upozorenja u Operations Manager
Upozorenja u SCOM generira pojedinačne pravila ili monitora da biste naznačili konkretan problem.  Monitor možete generirati upozorenja kada ulazi u stanju pogreške tijekom pravila može generirati upozorenja da biste naznačili neke ključne problem koji nisu izravno povezani s stanja upravljanog objekta.  Upravljanje paketi obuhvaćaju brojne tijekove rada koji se stvaranje upozorenja za aplikaciju ili servis koji mogu upravljati.  Da biste bili sigurni da ne dobivati viškom upozorenja za probleme koji ne smatrate ključnih je usklađivanje procesa konfiguriranja novi paket za upravljanje.

![SCOM upozorenja prikaz](media/operations-management-suite-monitoring-alerts/scom-alert-view.png)

SCOM omogućuje dovršavanje upozorenja upravljanje upozorenja imate status koje administratori mogu mijenjati tijekom rada da biste riješili taj problem.  Kada je problem riješen, administrator postavlja upozorenje da biste zatvorili koje trenutku se više ne prikazuje u prikazima prikaz aktivne upozorenja.  Upozorenja koje su stvorene iz monitora može automatski riješiti kada monitor vraća dobar stanje.

![Status upozorenja](media/operations-management-suite-monitoring-alerts/scom-alert-status.png)

## <a name="alerts-in-log-analytics"></a>Upozorenja u zapisnik Analytics
Upozorenja u zapisnik analize stvara se iz zapisnika upita koji se automatski pokreću u pravilnim vremenskim razmacima.  Upozorenja pravilo možete stvoriti iz bilo kojeg zapisnika upita.  Ako je upit vraća rezultat koji zadovoljavaju kriterij koji navedete, stvara se upozorenje.  To može biti određene upit koji stvara upozorenja ako je otkriven određeni događaj ili može koristiti više općenitih upit koji se traži bilo koji događaj pogreške vezane uz određenu aplikaciju.

Prijavite se analize upozorenja zapisuju u spremište OMS kao događaja i mogu biti dohvaćeni s upitom zapisnika.  Nemaju status kao što su događaji SCOM tako da možete označiti kad je problem riješen.

![OMS upozorenja](media/operations-management-suite-monitoring-alerts/oms-alert.png)

Kada SCOM se koristi kao izvor podataka za analizu zapisnika, SCOM upozorenja zapisuju se u spremište OMS kao što su stvorene i mijenjati.  

![SCOM upozorenje](media/operations-management-suite-monitoring-alerts/scom-alert.png)

[Rješenje upozorenja upravljanja](http://technet.microsoft.com/library/mt484092.aspx) nudi sažetak active upozorenja i nekoliko uobičajenih upite za dohvaćanje različitih skupova upozorenja.  Ovo vam omogućuje učinkovitije analizu upozorenja od izvješće u SCOM.  Možete pretraživati naniže iz sažetke detaljne podatke i stvarati ad hoc upite za dohvaćanje različitih skupova upozorenja.

![Rješenje upozorenja upravljanja](media/operations-management-suite-monitoring-alerts/alert-management.png)

## <a name="notifications"></a>Obavijesti
Obavijesti u SCOM slati poštu ili tekst u odgovor na upozorenja koji odgovaraju određenom kriteriju.  Možete stvoriti pretplate na različitim obavijesti koji imaju različite osobe obavijest ovisno o takav kriterijima kao objekt prate, težinu upozorenja, a zatim vrstu otkrio problem ili doba dana.

Nekoliko pretplate može se koristiti implementaciju strategije dovršeno obavijesti za veliki broj paketi za upravljanje.

![SCOM upozorenja](media/operations-management-suite-monitoring-alerts/alerts-overview-scom.png)

Prijava analitiku vas može obavijestiti poštom da upozorenja stvorena postavljanjem akciju obavijesti e-pošte na svako [pravilo upozorenja](http://technet.microsoft.com/library/mt614775.aspx).  Nema mogućnost SCOM pretplata na višestruka upozorenja pomoću jedne pravila.  Morate stvoriti upozorenje pravila jer OMS ne nudi bilo unaprijed konfigurirane.

![Prijava analitiku upozorenja](media/operations-management-suite-monitoring-alerts/alerts-overview-oms.png)

Nije moguće u potpunosti upravljati SCOM upozorenja u zapisnik analize kroz jer možete mijenjati samo ih na konzoli operacije.  Prijava analitiku koristan je kao dio upozorenja upravljanje kroz postupka za pružanje alata za analizu te SCOM samostalno nema.

## <a name="alert-remediation"></a>Upozorenja olakšava
[Olakšava](http://technet.microsoft.com/library/mt614775.aspx) se odnosi na pokušaj automatsko ispravljanje problema otkrije upozorenja.
  
SCOM omogućuje vam pokretanje dijagnostiku i Recoveries u odgovoru unos dobro stanje monitor.  To se događa istodobno monitor stvaranje upozorenja.  Za dijagnostiku i recoveries obično implementirana kao skriptu koja se izvršava na agenta.  Na Dijagnostika pokušava prikupite dodatne informacije o otkriven problem dok je oporavak Potraži da biste riješili taj problem.

Prijava analitiku omogućuje vam da biste započeli [Automatizacija Azure runbook](https://azure.microsoft.com/documentation/services/automation/) ili poziva u webhook kao odgovor na prijava analitiku upozorenja.  Runbooks mogu sadržavati složene logike implementirana u ljusci PowerShell.  Skripta pokreće se u Azure i možete pristupiti Azure resursi ni dostupne vanjskim izvorima iz oblaka.  Azure Automatizacija imati mogućnost dopušta izvršavanje runbooks na poslužitelju u vašem lokalnom podatkovnog centra, ali ta značajka nije dostupan prilikom pokretanja na runbook kao odgovor na prijava analitiku upozorenja.

Obje recoveries u SCOM i runbooks u OMS mogu sadržavati skripte komponente PowerShell, ali recoveries teže stvaranje i upravljanje jer mora se nalaziti unutar paket za upravljanje.  Runbooks spremaju se u automatizaciji Azure koje nudi značajke za izradu, testiranje i upravljanje runbooks.

Ako koristite SCOM kao izvor podataka za analizu zapisnika, mogli biste stvoriti upozorenje na prijava analitiku pomoću zapisnika upita za dohvaćanje SCOM upozorenja pohranjene u spremištu OMS.  To omogućuje vam pokretanje programa automatizacije Azure runbook kao odgovor na SCOM upozorenje.  Naravno, budući da u runbook funkcionirat će u Azure, to ne bi izgledna strategija recoveries lokalnog problema.

## <a name="next-steps"></a>Daljnji koraci

- Dodatne pojedinosti [upozorenja u sustavu centar operacije Manager (SCOM)](https://technet.microsoft.com/library/hh212913.aspx).