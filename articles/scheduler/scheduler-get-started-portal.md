<properties
 pageTitle="Početak rada s Azure raspored Azure portalu | Microsoft Azure"
 description="Početak rada s Azure raspored Azure portalu"
 services="scheduler"
 documentationCenter=".NET"
 authors="derek1ee"
 manager="kevinlam1"
 editor=""/>
<tags
 ms.service="scheduler"
 ms.workload="infrastructure-services"
 ms.tgt_pltfrm="na"
 ms.devlang="dotnet"
 ms.topic="hero-article"
 ms.date="08/10/2016"
 ms.author="deli"/>

# <a name="get-started-with-azure-scheduler-in-azure-portal"></a>Početak rada s Azure raspored Azure portalu

Jednostavno je stvoriti Zakazani zadaci u rasporedu Azure. U ovom ćete praktičnom vodiču ćete saznati kako stvoriti posao. Ćete saznati i nadzor i upravljanje mogućnosti rasporeda.

## <a name="create-a-job"></a>Stvaranje zadatka

1.  Prijavite se na [portal za Azure](https://portal.azure.com/).  

2.  Kliknite **+ Novo** > u okvir za pretraživanje upišite _raspored_ > Odabir **rasporeda** u rezultatima > kliknite **Stvori**.

     ![][marketplace-create]

3.  Stvaranje zadatka koji se jednostavno dodirne http://www.microsoft.com/ sa zahtjevom GET. Na zaslonu za **Posao raspored** unesite sljedeće podatke:

    1.  **Naziv:**`getmicrosoft`  

    2.  **Pretplate:** Pretplate za Azure   

    3.  **Posla zbirke:** Odaberite postojeću zbirku posao ili kliknite **Stvori novo** > unesite naziv.

4.  Zatim u odjeljku **Postavke akcije**definiranje sljedeće vrijednosti:

    1.  **Vrsta akcije:**` HTTP`  

    2.  **Metoda:**`GET`  

    3.  **URL:**` http://www.microsoft.com`  

      ![][action-settings]

5.  Na kraju, recimo definirati raspored. Zadatak se definira kao jednokratni zadatak, ali ćemo odaberite ponavljanje rasporeda:

    1. **Ponavljanje**:`Recurring`

    2. **Pokretanje**: današnji datum

    3. **Ponavljanja svaki**:`12 Hours`

    4. **Kraj po**: dva dana od danas je datum  

      ![][recurrence-schedule]

6.  Kliknite **Stvori**

## <a name="manage-and-monitor-jobs"></a>Upravljanje i praćenje zadataka

Nakon stvaranja posla, pojavljuje se u glavnom Azure nadzorne ploče. Kliknite zadatak, a zatim otvorit će se novi prozor sa sljedećim karticama:

1.  Svojstva  

2.  Postavke akcija  

3.  Raspored  

4.  Povijest

5.  Korisnici

    ![][job-overview]

### <a name="properties"></a>Svojstva

Svojstva samo za čitanje opisuju upravljanje metapodataka za posao raspored.

   ![][job-properties]


### <a name="action-settings"></a>Postavke akcija

Klik na poslu na zaslonu **Poslovi** omogućuje vam da biste konfigurirali taj zadatak. Možete konfigurirati dodatne postavke omogućuje ako ih niste konfigurirali za brzo stvaranje čarobnjak.

Za sve vrste akcija, možete promijeniti pravila Ponovi i akciju pogreške.

Za HTTP i HTTPS vrste akcija posla, možete promijeniti način da biste sve dopuštene glagolski HTTP. Možete i dodati, brisanje ili promjena zaglavlja i podaci za osnovnu provjeru autentičnosti.

Za vrste akcija reda čekanja za pohranu, možete promijeniti račun za pohranu, Naziv reda čekanja, SAS token i tijelo.

Za vrste akcija bus usluga, možete promijeniti prostor naziva, put tema/red, postavke provjere autentičnosti, Vrsta prijenosa, svojstva poruke i tijelo poruke.

   ![][job-action-settings]

### <a name="schedule"></a>Raspored

To vam omogućuje da ponovno konfigurira rasporedu, ako želite da biste promijenili raspored koji ste stvorili u na brzo stvaranje čarobnjak.

Ovo je priliku izgradnje [složene analize i naprednih ponavljanje u vaš posao](scheduler-advanced-complexity.md)

Možete promijeniti početni datum i vrijeme, ponavljanje raspored i kraju datuma i vremena (Ako je zadatak ponavlja.)

   ![][job-schedule]


### <a name="history"></a>Povijest

Na kartici **Povijest** prikazuje odabrane metriku za svaki zadatak izvođenja u sustavu za odabrani zadatak. Ove metriku unesite u stvarnom vremenu vrijednosti o stanju sustava rasporeda:

1.  Status  

2.  Pojedinosti  

3.  Ponovnim pokušajima

4.  Ponavljanje: 1st, 2nd, 3, itd.

5.  Početno vrijeme izvođenja  

6.  Vrijeme završetka izvođenja

   ![][job-history]

Klikom na Pokreni da biste vidjeli njegove **Pojedinosti povijest**, uključujući cijeli odgovor za svaki izvršavanja. Taj se dijaloški okvir omogućuje i kopirati ga u međuspremnik.

   ![][job-history-details]

### <a name="users"></a>Korisnici

Azure na temelju uloga pristup kontrola (RBAC) omogućuje upravljanje preciznije pristup za Azure raspored. Da biste saznali kako koristiti i karticu korisnici, pogledajte [Azure_Role-Based kontrola pristupa](../active-directory/role-based-access-control-configure.md)


## <a name="see-also"></a>Vidi također

 [Što je raspored?](scheduler-intro.md)

 [Raspored koncepti, terminologija i entitet hijerarhije](scheduler-concepts-terms.md)

 [Tarife i naplata u rasporedu Azure](scheduler-plans-billing.md)

 [Kako izraditi složene analize i naprednih ponavljanja s Azure raspored](scheduler-advanced-complexity.md)

 [Referenca raspored REST API-JA](https://msdn.microsoft.com/library/mt629143)

 [Raspored PowerShell cmdleti za pregled](scheduler-powershell-reference.md)

 [Visoku dostupnost raspored i pouzdanosti](scheduler-high-availability-reliability.md)

 [Ograničenja raspored, zadanih vrijednosti i šifre pogreške](scheduler-limits-defaults-errors.md)

 [Raspored izlaznu provjeru autentičnosti](scheduler-outbound-authentication.md)


[marketplace-create]: ./media/scheduler-get-started-portal/scheduler-v2-portal-marketplace-create.png
[action-settings]: ./media/scheduler-get-started-portal/scheduler-v2-portal-action-settings.png
[recurrence-schedule]: ./media/scheduler-get-started-portal/scheduler-v2-portal-recurrence-schedule.png
[job-properties]: ./media/scheduler-get-started-portal/scheduler-v2-portal-job-properties.png
[job-overview]: ./media/scheduler-get-started-portal/scheduler-v2-portal-job-overview-1.png
[job-action-settings]: ./media/scheduler-get-started-portal/scheduler-v2-portal-job-action-settings.png
[job-schedule]: ./media/scheduler-get-started-portal/scheduler-v2-portal-job-schedule.png
[job-history]: ./media/scheduler-get-started-portal/scheduler-v2-portal-job-history.png
[job-history-details]: ./media/scheduler-get-started-portal/scheduler-v2-portal-job-history-details.png


[1]: ./media/scheduler-get-started-portal/scheduler-get-started-portal001.png
[2]: ./media/scheduler-get-started-portal/scheduler-get-started-portal002.png
[3]: ./media/scheduler-get-started-portal/scheduler-get-started-portal003.png
[4]: ./media/scheduler-get-started-portal/scheduler-get-started-portal004.png
[5]: ./media/scheduler-get-started-portal/scheduler-get-started-portal005.png
[6]: ./media/scheduler-get-started-portal/scheduler-get-started-portal006.png
[7]: ./media/scheduler-get-started-portal/scheduler-get-started-portal007.png
[8]: ./media/scheduler-get-started-portal/scheduler-get-started-portal008.png
[9]: ./media/scheduler-get-started-portal/scheduler-get-started-portal009.png
[10]: ./media/scheduler-get-started-portal/scheduler-get-started-portal010.png
[11]: ./media/scheduler-get-started-portal/scheduler-get-started-portal011.png
[12]: ./media/scheduler-get-started-portal/scheduler-get-started-portal012.png
[13]: ./media/scheduler-get-started-portal/scheduler-get-started-portal013.png
[14]: ./media/scheduler-get-started-portal/scheduler-get-started-portal014.png
[15]: ./media/scheduler-get-started-portal/scheduler-get-started-portal015.png
