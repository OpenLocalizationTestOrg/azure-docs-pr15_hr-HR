
<properties
    pageTitle="Oporavak Azure web-mjesta: Repliciranje Hyper-V virtualnim strojevima na jednom poslužitelju VMM | Microsoft Azure"
    description="U ovom se članku opisuje kako replicirati Hyper-V virtualnim strojevima kada imate samo jedan VMM poslužitelja."
    services="site-recovery"
    documentationCenter=""
    authors="rayne-wiselman"
    manager="jwhit"
    editor=""/>

<tags
    ms.service="site-recovery"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="backup-recovery"
    ms.date="08/24/2016"
    ms.author="raynew"/>

#  <a name="replicate-hyper-v-virtual-machines-on-a-single-vmm-server"></a>Replicirati Hyper-V virtualnim strojevima na jednom VMM poslužitelju

Ovaj članak da biste saznali kako replicirati Hyper-V virtualnih računala koja se nalazi na poslužitelj Hyper-V u VMM oblaka kada imate samo jedan VMM poslužitelja u implementaciji sustava.

Azure sadrži dvije različite [implementacije modela](../resource-manager-deployment-model.md) za stvaranje i rad s resursima: Voditelj resursa Azure i classic. Azure ima dvije portalima – Azure klasični portala koji podržava model klasični implementacije i portalu Azure s podrškom za oba modele implementacije. Ovaj članak sadrži upute za postavljanje replikacije na portalu za Azure.


Ako imate pitanja kad pročitate članak, objavite ih u komentarima Disqus pri dnu ovog članka ili na [forumu servisa Azure oporavak](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).

## <a name="overview"></a>Pregled

Ako želite replicirati Hyper-V VMs koja se nalazi na Hyper-V domaćini u VMM, a imate samo jedan VMM poslužitelj, možete ga [replicirati Azure](site-recovery-vmm-to-azure.md), ili između oblaka na jednom VMM poslužitelju.

Preporučujemo da vam replicirati za Azure jer prebacivanje i oporavak nisu objedinjenog kada replikaciju između oblaka i broj Ručni koraci nisu potrebni. Ako želite replicirati pomoću samo poslužitelj VMM, možete učiniti sljedeće:

- Replicirati s poslužiteljem VMM jedan samostalni
- Replicirati s poslužiteljem VMM jedan implementiran prošireni klasteru sustava Windows


## <a name="replicate-across-sites-with-a-single-standalone-vmm-server"></a>Replicirati na web-mjesta s poslužiteljem VMM jedan samostalni

![Samostalne virtualnog VMM poslužitelja](./media/site-recovery-single-vmm/single-vmm-standalone.png)

U ovom scenariju implementacija pojedinačni poslužitelj VMM kao virtualnog računala u primarni web-mjesta, a replicirati ovaj VM sekundarne web-mjesto koristeći oporavak web-mjesta i Hyper-V replike.

1. Postavljanje VMM na Hyper-V VM. Predlažemo da instalirate instancu sustava SQL Server koristi VMM na istom VM za uštedu vremena. Ako želite koristiti udaljene instancu sustava SQL Server i do prekida pojavljuje, morate vratiti tu instancu prije nego što se može oporaviti VMM.
2. Provjerite ima li poslužitelj VMM najmanje dva oblaka konfiguriran. Jedan oblaka sadrži na VMs želite replicirati, a u oblak služi kao mjesto na sekundarnom. Trebali biste dobiti oblaka koja sadrži VMs koji želite zaštititi:

    - Jednu ili više VMM glavno računalo grupa koja sadrži jedan ili više poslužitelja Hyper-V glavno računalo u svakoj grupi glavnog računala.
    - Barem jedan Hyper-V virtualnog računala na svakom poslužitelju Hyper-V glavnog računala.

3. Stvaranje oporavak servisa sigurnog, stvaranje i preuzimanje ključa za registraciju sigurnog i registraciju VMM poslužitelja u na sigurnog. Tijekom registracije instalirajte davatelja za oporavak Azure web-mjesta na poslužitelju VMM.
4. Postavljanje jednog ili više oblaka na VMM VM i dodajte domaćini Hyper-V tako da biste te oblaka.
3. Konfiguriranje postavki zaštite za oblaka. Navedite naziv poslužitelja za jednu VMM kao lokacije izvorišno i odredišno. Da biste konfigurirali preslikavanje, mapirati na mreži VM oblaka s VMs koji želite zaštititi mrežu VM za replikaciju spremanja.
4. Omogućivanje početne replikacije za VMs koji želite zaštititi putem mreže jer oba oblaka nalaze se na istom poslužitelju.
4. Na konzoli za Upravitelj Hyper-V Omogućivanje Hyper-V replike na glavnom računalu Hyper-V koja sadrži VMM VM i omogućiti replikaciju na na VM. Provjerite je li VMM VM Nemoj dodati na bilo kojem oblaka zaštićene oporavak web-mjesta. Na taj način Hyper-V replike postavke nisu nadjačati oporavak web-mjesta.
5. Ako želite stvoriti tarife za oporavak, navedite na isti poslužitelj VMM za izvorišno i odredišno.

Slijedite upute iz [članka](site-recovery-vmm-to-vmm.md) stvaranje na sigurnog, registraciju poslužitelja i postavljanje protection.

### <a name="what-to-do-in-an-outage"></a>Što učiniti u do prekida

Ako potpuni nestalo i morate raditi s sekundarnog web-mjesta, učinite sljedeće:

1.  Na konzoli Hyper-V upravitelja sekundarnog web-mjesta pokrenuti neplanirano prebacivanje uvoza putem VM VMM iz primarnih sekundarne web-mjesto.
2.  Provjerite je li VMM VM s radom u sekundarnog web-mjesta.
3.  U sigurnog oporavak Services pokrenuti neplanirano prebacivanje uvoza putem povećavaju VMs iz primarnih za sekundarne oblaka. Da biste dovršili neplanirano prebacivanje od na VMs, provođenja za prebacivanje ili odaberite drugi oporavak točke po potrebi.
4.  Po dovršetku neplanirano prebacivanje korisnici mogu pristupati radno opterećenje resursa u sekundarnog web-mjesta.

Kada primarni web-mjesta radi normalno ponovno, učinite sljedeće:

1.  Na konzoli za Upravitelj Hyper-V omogućiti obrnutim replikaciju za VM VMM, da biste pokrenuli replikaciju iz sekundarne za primarni.
2.  Na konzoli za Upravitelj Hyper-V pokrenuti planiranog prebacivanje uvoza natrag VM VMM primarni web-mjesto. Izvršavanje prebacivanje na dovršavanje. Zatim omogućiti obrnutim replikacije da biste pokrenuli replikaciju u VMM iz primarnog za sekundarne.
3.  U sigurnog servise za oporavak omogućiti obrnutim replikacije za radno opterećenje VMs da biste pokrenuli replikaciju ih iz sekundarne za primarni.
4.  U sigurnog oporavak Services pokrenuti planiranog prebacivanje uvoza natrag radno opterećenje VMs primarni web-mjesto. Izvršavanje prebacivanje na dovršavanje. Zatim omogućiti obrnutim replikacije da biste pokrenuli replikaciju radno opterećenje VMs iz primarnog za sekundarne.



## <a name="replicate-across-sites-with-a-single-vmm-server-in-a-stretched-cluster"></a>Replicirati na web-mjesta s poslužiteljem jedan VMM prošireni klasteru

![Grupirani virtualnog VMM poslužitelja](./media/site-recovery-single-vmm/single-vmm-cluster.png)

Umjesto implementirate samostalni poslužitelj VMM kao VM koji umnaža sekundarnog web-mjesta, možete učiniti VMM iznimno dostupnim uvođenjem kao VM prebacivanje klasteru sustava Windows. To radno opterećenje resilience i nudi zaštitu od kvara hardvera. Za implementaciju s oporavak web-mjesta VMM VM mora biti implementirano rastezanje klasteru geografski zasebnom web-mjesta. Da biste to učinili:

1. Instalirajte VMM na virtualnog računala prebacivanje klasteru sustava Windows pa odaberite željenu mogućnost da biste pokrenuli poslužitelju kao vrlo dostupne tijekom postavljanja.
2. Instancu sustava SQL Server koja se koristi VMM potrebno je replicirati s grupe dostupnosti SQL Server AlwaysOn tako da postoji kopiju baze podataka u sekundarnog web-mjesta.
3. Slijedite upute iz [članka](site-recovery-vmm-to-vmm.md) stvaranje na sigurnog, registraciju poslužitelja i postavljanje protection. Morate registrirati svakome VMM u skupini u sigurnog servise za oporavak. Da biste to učinili, instalirajte davatelja usluge na aktivni čvor i registraciju VMM poslužitelja. Zatim instalirajte davatelja usluge na ostale čvorove.

### <a name="what-to-do-in-an-outage"></a>Što učiniti u do prekida

Kada se pojavi se prekida, VMM poslužitelj i njegov odgovarajuće baze podataka SQL Server nije uspjela tijekom te im pristupiti s sekundarnog web-mjesta.


## <a name="next-steps"></a>Daljnji koraci

[Saznajte više](site-recovery-vmm-to-vmm.md) o detaljne implementaciji oporavak web-mjesta za VMM za replikaciju VMM.
