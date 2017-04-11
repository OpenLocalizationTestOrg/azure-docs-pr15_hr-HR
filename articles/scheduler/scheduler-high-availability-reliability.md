<properties
 pageTitle="Visoku dostupnost raspored i pouzdanosti"
 description="Visoku dostupnost raspored i pouzdanosti"
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
 ms.topic="article"
 ms.date="08/16/2016"
 ms.author="deli"/>


# <a name="scheduler-high-availability-and-reliability"></a>Visoku dostupnost raspored i pouzdanosti

## <a name="azure-scheduler-high-availability"></a>Visoku dostupnost Azure rasporeda

Kao core servisa Azure platforme Azure rasporeda vrlo dostupna je i značajkama implementaciju zemlj suvišnih servisa i replikacije zemlj regionalne posla.

### <a name="geo-redundant-service-deployment"></a>Uvođenje servisa suvišnih zemlj.

Azure raspored je dostupan putem korisničkog Sučelja u regiji gotovo svaki zemlj koji se nalazi u Azure danas. Popis područja koja je dostupna u raspored Azure je [naveden u nastavku](https://azure.microsoft.com/regions/#services). Ako je podatkovnog centra u glavnom računalu regiji prikazivanju nije dostupan, prebacivanje mogućnosti rasporeda Azure su tako da je servis nije dostupan iz drugog podatkovnog centra.

### <a name="geo-regional-job-replication"></a>Zadatak zemlj regionalne replikacije

Ne samo raspored Azure je sučelja za upravljanje zahtjeve, ali vlastiti posao ujedno zemlj replicirati. Kada je do prekida u jednom području, raspored Azure ne uspije putem i provjerava se pokrene posao iz drugog podatkovnog centra u paru regiji.

Ako, na primjer, ako ste stvorili posao u Jug središnje NAM, raspored Azure automatski replicira taj zadatak u Sjevernoj središnje NAM. Kada je došlo u Jug središnje NAM, raspored Azure osigurava se pokrene posao od Sjeverna središnje nas. 

![][1]

Zbog toga Azure raspored osigurava podataka prekorači na istom šira regiji u slučaju programa Azure. Zbog toga ne morate dupliciranje posla samo da biste dodali visoke dostupnosti – Azure raspored automatski pruža visoku dostupnost mogućnosti za svoje zadatke.

## <a name="azure-scheduler-reliability"></a>Azure raspored pouzdanosti

Azure raspored jamčiti vlastitu visoku dostupnost i traje drukčiji pristup da biste stvorili zadatke. Na primjer, vaš posao možda pozivanje HTTP krajnju točku koja nije dostupna. Azure raspored velik ni složen pokušava izvršiti posla uspješno, tako da druge mogućnosti baviti nije uspjelo. Azure raspored to na dva načina:

### <a name="configurable-retry-policy-via-retrypolicy"></a>Prilagodljivo ponovite pravila putem "retryPolicy"

Azure raspored omogućuje vam da biste konfigurirali pravila Ponovi. Prema zadanim postavkama, ako je zadatak neuspješan, raspored pokuša posao ponovno četiri puta intervalima 30 sekundi. Ponovno možete konfigurirati ovog pravila Ponovi biti redovno (na primjer, deset puta intervalima 30 sekundi) ili looser (na primjer, dva puta dnevno intervalima.)

Kao primjer kada to može pomoći, možete stvoriti zadatak koji se izvodi u jednom tjedno a poziva krajnje HTTP. Ako je krajnju točku HTTP prema dolje za nekoliko sati kada se pokrene posla, možda ne želite pričekajte jedan tjedan više za posao da biste ponovno pokrenuli jer čak i Zadana pravila Ponovi neće uspjeti. U tim slučajevima možda konfigurirajte pravila Ponovi standardne pokušati svaka tri sata (na primjer) umjesto svakih 30 sekundi.

Da biste saznali kako konfigurirati pravila Ponovi, pogledajte [retryPolicy](scheduler-concepts-terms.md#retrypolicy).

### <a name="alternate-endpoint-configurability-via-erroraction"></a>Zamjenski Configurability krajnju točku putem "errorAction"

Ako krajnju točku cilj za svoj posao Azure raspored ostane nedostupno, raspored Azure pada natrag zamjenski krajnjoj obradu pogreške nakon njegova pravila Ponovi. Ako je konfiguriran za obradu pogreške krajnje zamjenski, raspored Azure poziva ga. S krajnje zamjenski vlastitim zadacima dostupnih vrlo in the face of nije uspjelo.

Na primjer, u dijagramu u nastavku, raspored Azure slijedi politiku pokušaj pogoditi iz prvog New York web-servisa. Nakon što u ponovne pokušaje neće funkcionirati, on provjerava postoji zamjenske. Zatim prelazi unaprijed i pokreće upućivanje zahtjeva za isključive s ista pravila Ponovi.

![][2]

Imajte na umu da se ista pravila Ponovi primjenjuje na izvornom akcije i akcija zamjenski pogreške. I moguće je da bi vrsta akcije akciju zamjenski pogreška se razlikovati od vrsta akcije glavni akciju. Na primjer, dok krajnje HTTP možda pozivanje glavni akciju, akcije pogreške umjesto toga možda reda čekanja za pohranu, red čekanja bus servisu ili servis bus tema koje akcije ne zapisivanje pogrešaka prilikom.

Da biste saznali kako konfigurirati krajnje zamjenski, pogledajte [errorAction](scheduler-concepts-terms.md#action-and-erroraction).

## <a name="see-also"></a>Vidi također

 [Što je raspored?](scheduler-intro.md)

 [Azure koncepata raspored, terminologija i entitet hijerarhije](scheduler-concepts-terms.md)

 [Početak rada s raspored na portalu za Azure](scheduler-get-started-portal.md)

 [Tarife i naplata u rasporedu Azure](scheduler-plans-billing.md)

 [Kako izraditi složene analize i naprednih ponavljanja s Azure raspored](scheduler-advanced-complexity.md)

 [Referenca za Azure raspored REST API-JA](https://msdn.microsoft.com/library/mt629143)

 [Azure referenca cmdleta ljuske PowerShell za raspored](scheduler-powershell-reference.md)

 [Azure ograničenja raspored, zadanih vrijednosti i šifre pogreške](scheduler-limits-defaults-errors.md)

 [Azure izlaznu provjeru autentičnosti rasporeda](scheduler-outbound-authentication.md)


[1]: ./media/scheduler-high-availability-reliability/scheduler-high-availability-reliability-image1.png

[2]: ./media/scheduler-high-availability-reliability/scheduler-high-availability-reliability-image2.png
