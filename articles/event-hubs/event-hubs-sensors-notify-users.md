<properties 
   pageTitle="Obavještavanje korisnika o podatke od senzori ili drugi sustavi | Microsoft Azure"
   description="U članku se opisuje kako koristiti događaj koncentratora za obavještavanje korisnika o senzor podataka."
   services="event-hubs"
   documentationCenter="na"
   authors="spyrossak"
   manager="timlt"
   editor="" />
<tags 
   ms.service="event-hubs"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/25/2016"
   ms.author="spyros;sethm" />

# <a name="notify-users-of-data-received-from-sensors-or-other-systems"></a>Obavještavanje korisnika o podatke od senzori ili drugi sustavi

Recimo da imate program koji nadzire podatke u stvarnom vremenu ili daje izvješća na raspored. Ako pogledate web-mjestu na kojem se prikazuju te u stvarnom vremenu grafikona ili izvješća, možda će se prikazati nešto što zahtijeva akcija. Što se događa ako morate biti obaviješteni tim slučajevima, umjesto oslanjate na plaćanju na web-stranici? Zamislite da imate Povećaj svijetlo u na greenhouse i odmah znati ako osnovnu vodi. Jedan od načina koji bi se uz osnovnu senzora u greenhouse, raspoređivanje slati poruke e-pošte ako je isključena osnovnu.

![][1]

U drugom slučaju zamislite da pokrenete pet boarding funkcijom i morate pojavi upozorenje razine opskrbu niskog zaliha. Ako, na primjer, možda rasporedite slati tekstnu poruku iz sustava ERP ako skladištu od psa Hrana sadrži fallen ključnih razinu. 

![][2]

Problem se objašnjava kako se ključnih informacija prilikom određene uvjete, ne kada se prikaže za odjavljivanje statične izvješća. Ako koristite [Koncentrator Azure događaj][] ili [Azure IoT koncentrator][] primaju podatke s uređaja ili aplikacijama kao što su [Dynamics AX][], imate nekoliko mogućnosti za način obrade ih. Možete pogledati na web-mjestu, možete analizirati ih, koji vam omogućuje pohranu i pomoću njih možete pokrenuti naredbi na nešto. Da biste to učinili, možete koristiti napredne alate kao što su [Azure web-mjesta][], [SQL Azure][], [HDInsight][], [Cortana obavještavanje paket][], [IoT paket][], [Logike aplikacije][]ili [Koncentratora Azure obavijesti][]. No ponekad želite učiniti je da biste poslali osoba koja ima najmanje indirektni tih podataka. Da bi se prikazala kako da to učinite uz samo malo koda, ne možemo ste naveli novi uzorka, [AppToNotifyUsers][]. Mogućnosti koje se nude su e-pošte (SMTP), SMS i telefon.

## <a name="application-structure"></a>Strukturu aplikacije

Aplikacija je napisan C# i datoteke readme u uzorku sadrži sve informacije potrebne za izmjenu, stvaranje i objavljivanje aplikacija. Sljedeći odjeljci sadrže više razine pregled funkcija aplikacije.

Ne možemo počinju pretpostavci da imate kritične događaje koji se pomiču koncentrator Azure događaj ili IoT koncentratora. Neki koncentratoru neće imati kao pristup i znate niz za povezivanje.

Ako već nemate programa koncentratora za događaj ili IoT koncentrator, jednostavno možete postaviti test Uloži programa štita Arduino i Raspberry Pi, slijedite upute u programu project za [Povezivanje u točkama](https://github.com/Azure/connectthedots) . Osnovna senzora na štita Arduino šalje osnovnu razine kroz vrijednost Pi programa [Azure događaj koncentrator][] (**ehdevices**), a programa [Azure strujanje analize](https://azure.microsoft.com/services/stream-analytics/) posao ih gura upozorenja na drugi događaj koncentrator (**ehalerts**) ako osnovna razine primili nalaziti ispod određene razine.

Kada se pokrene **AppToNotify** , čita konfiguracijska datoteka (App.config) da biste dobili URL-a i vjerodajnice za događaj koncentrator primati upozorenja. Zatim se spawns procesa neprestano praćenje da događaj koncentrator za sve poruke koje prolazi kroz – pod uvjetom da imate možete pristupiti URL-a za događaj koncentrator ili IoT koncentratora i valjanih se vjerodajnica, kod čitač događaj koncentratora će neprestano čitati uskoro. Prilikom pokretanja, aplikacije i čita URL-a i vjerodajnice za razmjenu poruka servisa (e-pošte, SMS, telefon) koji želite koristiti, a naziv/adresa pošiljatelja i na popis primatelja.

Kada monitor događaj koncentrator otkrije poruke, pokreće postupak koji šalje poruke na način naveden u konfiguracijskoj datoteci. Imajte na umu da se šalje svaku poruku otkrije. Ako ste postavili monitor pokazivati koncentrator za događaj koji prima deset poruke sekundi, pošiljatelj će poslati deset poruke sekundi – deset poruke e-pošte u sekundi, deset SMS poruke u sekundi, deset telefonske pozive u sekundi. Zbog toga, provjerite je li vam praćenje koncentratora za događaj koji prima samo upozorenja na koja treba poslati odgovor nije programa koncentrator događaj koji prima sirovim podacima iz senzori ili aplikacije.

## <a name="applicability"></a>Primjenjivošću kako bi

Kod u ovom primjeru prikazuje se samo praćenje događaja koncentratora i da biste pozvali vanjske servise za razmjenu za slučaj da želite dodati ta je funkcija u aplikaciji. Imajte na umu da ova rješenja DIY za razvojne inženjere odnosila primjer samo. Ga ne adresu preduvjeti za enterprise kao što su zalihosti, Neuspjelo postavite pokazivač, ponovno pokrenite nakon pogreška, itd. Više potpun i radnih rješenja, potražite u sljedećim člancima:

- Pomoću poveznika ili slanje obavijesti putem servisa [Azure logike aplikacije](../app-service-logic/app-service-logic-connectors-list.md) .
- Pomoću [Azure obavijesti koncentratora](https://msdn.microsoft.com/library/azure/jj927170.aspx), kao što je opisano blog [emitiranje automatske obavijesti da biste milijune mobilnim uređajima pomoću koncentratora Azure obavijesti](http://weblogs.asp.net/scottgu/broadcast-push-notifications-to-millions-of-mobile-devices-using-windows-azure-notification-hubs). 

## <a name="next-steps"></a>Daljnji koraci

To je jasan da biste stvorili servis za jednostavne obavijesti koja šalje poruke e-pošte ili tekstnih poruka primatelji ili poziva, prijenos podataka primio koncentratora za događaj ili IoT koncentratora. Da biste implementirali rješenje obavijestiti korisnike na temelju podataka primio te koncentratora, posjetite [AppToNotifyUsers][].

Dodatne informacije o ove koncentratora potražite u sljedećim člancima:

- [Koncentratorima Azure događaja]
- [Azure IoT koncentratora]
- Početak rada s [vodič koncentratora za događaj].
- U Dovršeno [Ogledni program koji koristi događaj koncentratora].

Da biste implementirali rješenje za obavještavanje korisnika koji se temelje na podacima primio te koncentratora, posjetite:

- [AppToNotifyUsers][]

[Praktični vodič koncentratora događaja]: event-hubs-csharp-ephcs-getstarted.md
[Azure IoT koncentratora]: https://azure.microsoft.com/services/iot-hub/
[Koncentratorima Azure događaja]: https://azure.microsoft.com/services/event-hubs/
[Koncentrator Azure događaja]: https://azure.microsoft.com/services/event-hubs/
[primjer aplikacije koja koristi događaj koncentratora]: https://code.msdn.microsoft.com/Service-Bus-Event-Hub-286fd097
[AppToNotifyUsers]: https://github.com/Azure-Samples/event-hubs-dotnet-user-notifications
[Dynamics AX za Outlook]: http://www.microsoft.com/dynamics/erp-ax-overview.aspx
[Azure web-mjesta]: https://azure.microsoft.com/services/app-service/web/
[SQL Azure]: https://azure.microsoft.com/services/sql-database/
[HDInsight]: https://azure.microsoft.com/services/hdinsight/
[Cortana obavještavanje paketa]: http://www.microsoft.com/server-cloud/cortana-analytics-suite/Overview.aspx?WT.srch=1&WT.mc_ID=SEM_lLFwOJm3&bknode=BlueKai
[Paket IoT]: https://azure.microsoft.com/solutions/iot-suite/
[Logika aplikacije]: https://azure.microsoft.com/services/app-service/logic/
[Azure obavijesti koncentratora]: https://azure.microsoft.com/services/notification-hubs/
[Azure Stream Analytics]: https://azure.microsoft.com/services/stream-analytics/
 
[1]: ./media/event-hubs-sensors-notify-users/event-hubs-sensor-alert.png
[2]: ./media/event-hubs-sensors-notify-users/event-hubs-erp-alert.png