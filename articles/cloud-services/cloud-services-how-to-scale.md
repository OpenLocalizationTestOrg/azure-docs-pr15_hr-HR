<properties
    pageTitle="Automatsko skaliranje servis u oblaku na portalu | Microsoft Azure"
    description="(classic) Saznajte kako koristiti klasične portal za konfiguriranje pravila skaliranje automatski oblaka servisa web uloga ili uloga suradnika u Azure."
    services="cloud-services"
    documentationCenter=""
    authors="Thraka"
    manager="timlt"
    editor=""/>

<tags
    ms.service="cloud-services"
    ms.workload="tbd"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/06/2016"
    ms.author="adegeo"/>


# <a name="how-to-auto-scale-a-cloud-service"></a>Upute za automatsko skaliranje servis u oblaku

> [AZURE.SELECTOR]
- [Portal za Azure](cloud-services-how-to-scale-portal.md)
- [Azure klasični portal](cloud-services-how-to-scale.md)

Na stranici portala za Azure klasični skaliranje ručno skaliranje ulogu suradnika ili uloga web ili Omogući automatsko skaliranje na temelju opterećenje procesora ili red čekanja za poruke.

>[AZURE.NOTE] U ovom se članku usredotočuje se na servis u Oblaku web- a tempiranja uloge. Kada stvorite virtualnog računala (klasični) izravno, nalaziti u oblaku. Neke od tih informacija odnosi se na sljedeće vrste virtualnih računala. Skaliranje dostupnosti skupa virtualnim strojevima je zaista samo isključite ih uključiti i isključiti na temelju pravila skaliranje konfigurirate. Dodatne informacije o virtualnim strojevima i skupova dostupnost potražite u članku [Upravljanje strojeva za dostupnost virtualne](../virtual-machines/virtual-machines-windows-classic-configure-availability.md)

Prije no što konfigurirate skaliranje za aplikaciju, razmotrite sljedeće informacije:

- Promjena veličine utječe core korištenje. Veće instance uloga pomoću više jezgri. Zatvaranje samo unutar ograničenja jezgri mogu mijenjati veličinu za vašu pretplatu. Na primjer, pretplate ima ograničenje od dvadeset jezgri i pokretanje aplikacija s dva srednje veličine oblaka services (ukupno četiri jezgri), koje možete samo proširenja druge implementaciji servisa oblak u svoju pretplatu tako da šesnaest jezgri. Dodatne informacije o veličinama potražite u članku [Oblaka servisa veličine](cloud-services-sizes-specs.md) .

- Morate stvoriti red i povezati s ulogom prije nego što se mogu mijenjati veličinu aplikacije na temelju poruke praga. Dodatne informacije potražite u članku [kako koristiti servis za pohranu red](../storage/storage-dotnet-how-to-use-queues.md).

- Mogu mijenjati veličinu resursi koji su povezani s servis u oblaku. Dodatne informacije o povezivanju resursima potražite u članku [Kako: veza resursa na neki servis u oblaku](cloud-services-how-to-manage.md#how-to-link-a-resource-to-a-cloud-service).

- Da biste omogućili visoke dostupnosti aplikacije, potrebno je provjeriti je li implementiran s dva ili više instanci uloge. Dodatne informacije potražite u članku [Ugovore o razini usluge](https://azure.microsoft.com/support/legal/sla/).



## <a name="schedule-scaling"></a>Zakazivanje skaliranja

Po zadanom sve uloge slijedite određeni raspored. Zbog toga promijeniti postavke odnose na cijelo vrijeme i sve dana u godini. Ako želite, možete postaviti ručno i automatsko skaliranje za:

- Dane u tjednu
- Vikenda
- Noći tjedna
- Mornings dan
- Određenog datuma
- Raspon datuma

Ovo je conigured [Azure klasični portal](https://manage.windowsazure.com/) u  
**Servisi u oblaku** > **\[servis u oblaku\]** > **mjerilo** > **\[proizvodnje ili pripremna\] ** stranice.

Kliknite gumb **Postavljanje zakazivanje vremena** za svaku ulogu želite promijeniti.

![Servis za automatsko skaliranje na temelju rasporeda u oblaku][scale_schedules]



## <a name="manual-scale"></a>Ručna promjena veličine

Na stranici **Skaliranje** ručno možete povećati ili smanjiti broj pokrenute instance u oblaku. Ako niste stvorili raspored to je konfiguriran za svaki raspored koji ste stvorili ili sve vrijeme.

1. [Azure klasični portal](https://manage.windowsazure.com/)kliknite **Servise u Oblaku**, a zatim naziv servisa u oblaku da biste otvorili na nadzornoj ploči.

    > [AZURE.TIP] Ako ne vidite servis u oblaku, možda ćete morati promijeniti iz **radnog** **pripremna** ili obrnuto.

2. Kliknite **Prilagodi**.

3. Odaberite raspored koji želite promijeniti mogućnosti skaliranja. Zadane postavke da biste *bez zakazano vrijeme* ako imate bez rasporede definirani.

4. Pronađite odjeljak **mjerilo po metrika** , a zatim odaberite **None (nema)**. Ovo je zadana postavka za sve uloge.

5. Svaka uloga u servis u oblaku ima klizač za promjenu broj instanci da biste koristili.

    ![Ručna promjena veličine uloge servisa oblaka][manual_scale]

    Ako trebate više instanci, možda ćete morati promjena [oblaka servisa virtualnog računala veličina](cloud-services-sizes-specs.md).

6. Kliknite **Spremi**.  
Uloga instance će se dodati ili ukloniti ovisno o odabire.

>[AZURE.TIP] Svaki put kada se prikaže ![][tip_icon] prijelaz mišem, a možete mu pristupiti pomoći za koje određene postavke ne.


## <a name="automatic-scale---cpu"></a>Automatska promjena veličine - procesora

To mijenja veličinu ako average postotak središnjeg procesora koristi dolazi iznad ili ispod navedeni pragovi; Uloga instance je stvorila ili izbrisati.

1. [Azure klasični portal](https://manage.windowsazure.com/)kliknite **Servise u Oblaku**, a zatim naziv servisa u oblaku da biste otvorili na nadzornoj ploči.

    > [AZURE.TIP] Ako ne vidite servis u oblaku, možda ćete morati promijeniti iz **radnog** **pripremna** ili obrnuto.

2. Kliknite **Prilagodi**.

3. Odaberite raspored koji želite promijeniti mogućnosti skaliranja. Zadane postavke da biste *bez zakazano vrijeme* ako imate bez rasporede definirani.

4. Pronađite odjeljak **mjerilo po metrika** , a zatim odaberite **procesora**.

5. Sada možete konfigurirati minimalne i maksimalne raspon instance uloge, cilja procesora (da biste pokrenuli skalu gore), a koliko je instanci skaliranje gore i dolje po.

![Promjena veličine uloge servisa oblak tako da opterećenje procesora][cpu_scale]

>[AZURE.TIP] Svaki put kada se prikaže ![][tip_icon] prijelaz mišem, a možete mu pristupiti pomoći za koje određene postavke ne.





## <a name="automatic-scale---queue"></a>Automatska promjena veličine - reda čekanja

To automatski mijenja veličinu ako se broj poruka u redu čekanja dolazi iznad ili ispod određeni prag; Uloga instance je stvorila ili izbrisati.

1. [Azure klasični portal](https://manage.windowsazure.com/)kliknite **Servise u Oblaku**, a zatim naziv servisa u oblaku da biste otvorili na nadzornoj ploči.

    > [AZURE.TIP] Ako ne vidite servis u oblaku, možda ćete morati promijeniti iz **radnog** **pripremna** ili obrnuto.

2. Kliknite **Prilagodi**.

3. Pronađite odjeljak **mjerilo po metrika** , a zatim odaberite **procesora**.

4. Sada možete konfigurirati raspon uloge instance reda čekanja i iznos poruka reda čekanja za obradu za svaku instancu i koliko je instanci skaliranje gore i dolje po minimum i maksimum.

![Promjena veličine uloge servisa oblak tako da red čekanja za poruke][queue_scale]

>[AZURE.TIP] Svaki put kada se prikaže ![][tip_icon] prijelaz mišem, a možete mu pristupiti pomoći za koje određene postavke ne.


## <a name="scale-linked-resources"></a>Promjena veličine povezani resursi

Često kada skaliranje uloge, dobro je skalirali bazu podataka koju aplikaciju i korištenje. Ako bazu podataka veze na servis u oblaku, skaliranja postavke resursa možete pristupiti tako da kliknete na odgovarajuću vezu.

1. [Azure klasični portal](https://manage.windowsazure.com/)kliknite **Servise u Oblaku**, a zatim naziv servisa u oblaku da biste otvorili na nadzornoj ploči.

    > [AZURE.TIP] Ako ne vidite servis u oblaku, možda ćete morati promijeniti iz **radnog** **pripremna** ili obrnuto.

2. Kliknite **Prilagodi**.

3. Pronađite odjeljak **povezani resursi** i kliknuli **Upravljanje**skalu za tu bazu podataka.

    > [AZURE.NOTE] Ako ne vidite odjeljak **povezani resursi** , vjerojatno nemate sve povezane resurse.

![][linked_resource]


[manual_scale]: ./media/cloud-services-how-to-scale/manual-scale.png
[queue_scale]: ./media/cloud-services-how-to-scale/queue-scale.png
[cpu_scale]: ./media/cloud-services-how-to-scale/cpu-scale.png
[tip_icon]: ./media/cloud-services-how-to-scale/tip.png
[scale_schedules]: ./media/cloud-services-how-to-scale/schedules.png
[scale_popup]: ./media/cloud-services-how-to-scale/schedules-dialog.png
[linked_resource]: ./media/cloud-services-how-to-scale/linked-resources.png
