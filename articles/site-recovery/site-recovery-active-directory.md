<properties
    pageTitle="Zaštita Active Directory i DNS-a s oporavak Azure web-mjesta | Microsoft Azure"
    description="U ovom se članku opisuje kako implementirati rješenja za oporavak Izrada za Active Directory pomoću oporavak Azure web-mjesta."
    services="site-recovery"
    documentationCenter=""
    authors="prateek9us"
    manager="abhiag"
    editor=""/>

<tags
    ms.service="site-recovery"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="storage-backup-recovery"
    ms.date="08/31/2016"
    ms.author="pratshar"/>

# <a name="protect-active-directory-and-dns-with-azure-site-recovery"></a>Zaštita Active Directory i DNS-a s oporavak Azure web-mjesta

Aplikacijama kao što je SharePoint, Dynamics AX i SAP proizvod ovise o Active Directory i DNS infrastrukture pravilno funkcionirati. Kada stvarate rješenje oporavak Izrada aplikacije, važno je zapamtiti da morate Zaštita i oporavak servisa Active Directory i DNS- a prije druge aplikacije komponente, da biste bili sigurni da stvari pravilno funkcionirati kada dođe do Izrada.

Oporavak web-mjesta je Azure servis koji omogućuje oporavak Izrada po orchestrating replikacije, prebacivanje i oporavak virtualnih računala. Oporavak web-mjesta podržava broj replikacije scenariji dosljedno zaštitu, i jednostavno prebacivanje virtualnim strojevima i aplikacijama privatno, javno ili za hostiranje oblaka.

Korištenje oporavak web-mjesta, možete stvoriti plan za oporavak dovršeno automatiziranog Izrada za Active Directory. Kada se dogodi to izbjeglo, možete započeti s pomoćnim sekundi s bilo kojeg mjesta i servisa Active Directory Tren oka pripremiti za nekoliko minuta. Ako ste uvesti servisa Active Directory za više aplikacija kao što je SharePoint i SAP proizvod u primarni web-mjesta, a želite neće uspjeti putem dovršeno web-mjesta, možete uspjeti putem servisa Active Directory najprije koristi oporavak web-mjesta, a zatim neće uspjeti iznad druge aplikacije pomoću tarife za oporavak specifičnim aplikacijama.

U ovom se članku objašnjava kako stvoriti Izrada oporavak rješenje za Active Directory, kako izvoditi planiranog, neplanirano i failovers Probno korištenje plan oporavak jednim klikom, podržanih konfiguracija i preduvjeti.  Morate biti upoznati sa servisom Active Directory i oporavak Azure web-mjesta prije nego što počnete.

Postoje dvije preporučene mogućnosti koji se temelji na složenosti vaše okruženje.

### <a name="option-1"></a>Mogućnost 1

Ako imate mali broj aplikacije i kontroler jedne domene, a želite neće uspjeti preko cijelog web-mjesta, pa preporučujemo korištenje oporavak web-mjesta za replikaciju kontrolerom domene na sekundarnom web-mjesto (li vam se ne uspijeva putem Azure ili sekundarnog web-mjesta). Isti repliciranu virtualnog računala može se koristiti za testiranje prebacivanje previše.

### <a name="option-2"></a>Mogućnost 2

Ako imate velik broj aplikacije i u okruženju postoji više kontroler domene ili ako planirate neće uspjeti preko nekoliko aplikacije, preporučujemo da osim replikaciju domene kontroler virtualnog računala s oporavak web-mjesta i postavit ćete je kontroler dodatnu domenu ciljnog web-mjesta (Azure ili podatkovnog centra za sekundarne lokalnog).

>[AZURE.NOTE] Čak i ako ste implementacijom mogućnost 2, za testiranje prebacivanje i dalje morate za replikaciju kontrolerom domene pomoću oporavak web-mjesta. Pročitajte [testiranje prebacivanje pitanja vezana uz](#considerations-for-test-failover) dodatne informacije.


Sljedeći koraci objašnjavaju kako omogućiti zaštitu kontroler domene u oporavak web-mjesta i upute za postavljanje kontroler domene u Azure.


## <a name="prerequisites"></a>Preduvjeti

- Lokalne implementacije servisa Active Directory i DNS poslužitelja.
- Azure web-mjesta oporavak servisa sigurnog pretplate na Microsoft Azure.
- Ako vam se replikaciju Azure Pokreni alat za procjenu za pripremu Azure virtualnog računala na VMs da biste bili sigurni one se kompatibilan s Azure VMs i servise za oporavak Azure web-mjesta.


## <a name="enable-protection-using-site-recovery"></a>Omogući zaštitu pomoću oporavak web-mjesta


### <a name="protect-the-virtual-machine"></a>Zaštita virtualnog računala

Omogući zaštitu od virtualnog računala kontroler/DNS za domenu u oporavak web-mjesta. Konfiguriranje postavki za oporavak web-mjesta ovise o vrsti virtualnog računala (Hyper-V ili VMware). Preporučujemo da se učestalost dosljedan replikacije rušenje za 15 minuta.

###<a name="configure-virtual-machine-network-settings"></a>Konfiguriranje postavki mreže virtualnog računala

Za virtualnog računala kontroler/DNS za domenu konfigurirati postavke mreže u oporavak web-mjesta tako da na VM bit će priložene desnom mrežu nakon prebacivanje. Ako, na primjer, ako ste replikaciju Hyper-V VMs za Azure možete odabrati na VM u oblaku VMM ili u grupi zaštita da biste konfigurirali postavke mreže kao što je prikazano u nastavku

![Postavke mreže VM](./media/site-recovery-active-directory/VM-Network-Settings.png)

## <a name="protect-active-directory-with-active-directory-replication"></a>Zaštita servisa Active Directory s replikacijom servisa Active Directory

### <a name="site-to-site-protection"></a>Zaštita web-mjesto

Stvaranje kontroler domene na sekundarnog web-mjesta i navedite naziv iste domene koje se koristi na web-mjestu primarni kada Promicanje poslužitelja u ulogu kontroler domene. **Active Directory web-mjesta i usluge** dodatak možete koristiti da biste konfigurirali postavke na objekt veze web-mjesta na kojem se dodaju web-mjesta. Konfiguriranjem postavki na vezu za web-mjesta, možete odrediti kada dođe do replikacije između dva ili više web-mjesta, a učestalosti. Dodatne informacije potražite u članku [Planiranje replikacije između web-mjesta](https://technet.microsoft.com/library/cc731862.aspx) .

###<a name="site-to-azure-protection"></a>Zaštita Azure web-mjesta

Slijedite upute da biste [stvorili kontroler domene u Azure virtualne mreže](../active-directory/active-directory-install-replica-active-directory-domain-controller.md). Kada Promicanje poslužitelja u ulogu kontroler domene Navedite naziv domene koji se koristi na web-mjestu primarni.

Zatim [konfigurirajte DNS poslužitelj za virtualne mreže](../active-directory/active-directory-install-replica-active-directory-domain-controller.md#reconfigure-dns-server-for-the-virtual-network), da biste koristili DNS poslužitelja u Azure.

![Azure mreže](./media/site-recovery-active-directory/azure-network.png)

## <a name="test-failover-considerations"></a>Razmatranja prebacivanje test

Prebacivanje test se pojavljuje u mreži s kojom je Izolirani s mreže proizvodnje tako da postoji bez utjecaja na radnih opterećenja radnog.

Većina aplikacije potreban je i prisutnosti kontroler domena i DNS poslužitelj funkcionirati, pa prije aplikacije nije uspjelo putem kontroler domene potrebne će biti stvoren u mreži odvajanja koja će se koristiti za testiranje prebacivanje. Najjednostavniji način za to je da biste omogućili zaštite virtualnog računala kontroler/DNS za domenu s oporavak web-mjesta i pokrenite test prebacivanje od tog virtualnog računala prije pokretanja test prebacivanje oporavak plana za aplikaciju. Evo kako to učiniti:

1. Omogućivanje zaštiti u oporavak web-mjesta za domenu kontroler/DNS virtualnog računala.
2. Stvoriti Izolirani mrežu. Virtualna mreže stvorene u Azure po zadanom je Izolirani s drugim mrežama. Preporučujemo da je isti kao mrežom radnog rasponu IP adresa za ovu mrežu. Povezivanje web-mjesto na ovoj mreži ne omogućite.
3. Navedite DNS IP adresu u mreži stvoreni kao IP adresa koju ste i očekivali DNS virtualnog računala da biste dobili. Ako ste replikaciju za Azure, navedite IP adresa za VM koji će se koristiti u prebacivanje u **Ciljne IP** postavka svojstva VM. Ako ste replikaciju u drugu lokalnog web-mjesta i koristite DHCP slijedite upute za [Postavljanje DNS-a i DHCP za prebacivanje test](site-recovery-failover.md#prepare-dhcp)

>[AZURE.NOTE] IP adresa dodijeliti virtualnog računala tijekom testiranja prebacivanje je isti kao IP adresa želite dobiti tijekom planiranog ili neplanirano prebacivanje, ako je dostupan u mreži prebacivanje test IP adresa. Ako nije, virtualnog računala prima drugu IP adresu koja je dostupna u mreži prebacivanje test.

4. Na domeni kontroler virtualnog računala pokrenite test prebacivanje je Izolirani mreže. Koristite najnovije dostupne aplikacije dosljedan oporavak točku virtualnog računala kontroler domene za prebacivanje test. 
5. Pokrenite test prebacivanje za oporavak plan aplikacije.
6. Po dovršetku testiranje označite zadatak prebacivanje test domene kontroler virtualnog računala i tarifa za oporavak "Dovršeno" na kartici **Zadaci** na portalu za oporavak web-mjesta.

### <a name="dns-and-domain-controller-on-different-machines"></a>DNS-a i domene kontroler na različitim računalima

Ako se DNS-a ne nalazi na istom računalu virtualne kao kontrolerom domene morat ćete stvoriti VM DNS-a za prebacivanje test. Ako takvi stupci na istom VM, možete preskočiti ovaj odjeljak.

Možete koristiti Osvježi DNS poslužitelj i stvaranje potrebnih zone. Ako, na primjer, ako je vaša domena servisa Active Directory contoso.com, možete stvoriti DNS zone s contoso.com naziv. Stavke koje odgovaraju sa servisom Active Directory mora se ažurirati u DNS-a, na sljedeći način:

1. Provjerite je li te postavke će se prikazati prije nego što se pojavi bilo koje druge virtualnog računala u planu oporavak:

    - Zone morate nosi naziv korijenske skupa stabala.
    - Zone mora biti datoteka koje se sigurnosno.
    - Zone mora biti omogućen za sigurne i nesigurne ažuriranja.
    - Razrješavanje virtualnog računala kontroler domene moraju pokazivati IP adresu DNS virtualnog računala.

2. Na domeni kontroler virtualnog računala, pokrenite sljedeću naredbu:

    `nltest /dsregdns`

3. Dodavanje u zonu na DNS poslužitelj, omogućili nesigurne ažuriranja i dodavanje unosa za nju DNS:

        dnscmd /zoneadd contoso.com  /Primary
        dnscmd /recordadd contoso.com  contoso.com. SOA %computername%.contoso.com. hostmaster. 1 15 10 1 1
        dnscmd /recordadd contoso.com %computername%  A <IP_OF_DNS_VM>
        dnscmd /config contoso.com /allowupdate 1


## <a name="next-steps"></a>Daljnji koraci

Čitanje [koje radnih opterećenja možete zaštititi?](../site-recovery/site-recovery-workload.md) da biste saznali više o zaštiti radnih opterećenja enterprise uz oporavak Azure web-mjesta.
