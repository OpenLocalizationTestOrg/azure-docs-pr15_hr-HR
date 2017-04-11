<properties
    pageTitle="Pokrenite alat za planer kapaciteta Hyper-V za oporavak web-mjesta | Microsoft Azure"
    description="Ovaj članak sadrži upute za korištenje alata za planer Hyper-V kapaciteta za oporavak Azure web-mjesta"
    services="site-recovery"
    documentationCenter="na"
    authors="rayne-wiselman"
    manager="jwhit"
    editor="" />
<tags
    ms.service="site-recovery"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="storage-backup-recovery"
    ms.date="07/12/2016"
    ms.author="raynew" />

# <a name="run-the-hyper-v-capacity-planner-tool-for-site-recovery"></a>Pokrenite alat za planer kapaciteta Hyper-V za oporavak web-mjesta

U sklopu Azure oporavak web-mjesta uvođenja morat ćete utvrđivanje replikaciju i uvjeta propusnosti. Alat za planer Hyper-V kapaciteta za oporavak web-mjesta pomaže vam da biste utvrdili replikacije i propusnosti preduvjeti za replikaciju Hyper-V virtualnog računala.


U ovom se članku opisuje kako pokrenuti alat za planer kapaciteta Hyper-V. Ovaj alat treba koristiti zajedno s drugim alatima za planiranje kapaciteta i informacije koje su opisane u [Planiranje kapaciteta za oporavak web-mjesta](site-recovery-capacity-planner.md).


## <a name="before-you-start"></a>Prije početka

Pokrenite alat na Hyper-V poslužitelja ili klaster čvor u primarni web-mjesta. Da biste pokrenuli alat za glavnog računala poslužitelja Hyper-V mora:

- Operacijski sustav: Windows Server® 2012 ili Windows Server® 2012 R2
- Memorija: 20 MB (najmanje)
- Procesor: 5 posto indirektni (najmanje)
- Prostor na disku: 5 MB (najmanje)

Prije pokretanja alata morat ćete pripremiti primarni web-mjesta. Ako ste replikaciju između dva lokalnog web-mjesta i želite provjeriti propusnosti, morat ćete pripremiti poslužiteljem replike.


## <a name="step-1-prepare-the-primary-site"></a>Korak 1: Priprema primarni web-mjesta
1. Na web-mjestu primarni provjerite popis svih virtualnim strojevima Hyper-V želite replicirati i Hyper-V domaćini/klastere na kojem se nalazi. Alat za možete pokrenuti svaki put kada više domaćini samostalne ili jedan klaster, ali ne oba zajedno. Također se mora se izvoditi zasebno za svaki operacijski sustav da biste trebali biste prikupite i zabilježite poslužitelja Hyper-V na sljedeći način:

  - Poslužitelji sustava Windows Server® 2012 samostalni
  - Windows Server® 2012 klastere
  - Windows Server® 2012 R2 samostalne poslužitelja
  - Windows Server® 2012 R2 klaster

3. Omogućivanje daljinskog pristupa s WMI Hyper-V domaćini i klastere. Pokrenite sljedeću naredbu na svakom poslužitelja/klaster da biste bili sigurni da pravila vatrozida i korisničkih dozvola postavljene:

        netsh firewall set service RemoteAdmin enable

5. Omogućite praćenje performansi na poslužiteljima i klastere, na sljedeći način:

  - Otvorite Vatrozid za Windows s **Dodatnom sigurnošću** dodatak, a zatim omogućiti sljedeće ulazna pravila: **COM + mrežom (DCOM-u)** i sva pravila u **grupi daljinsko upravljanje zapisnika događaja**.

## <a name="step-2-prepare-a-replica-server-on-premises-to-on-premises-replication"></a>Korak 2: Priprema poslužitelj replike (lokalni za replikaciju lokalnog)

Ne morate učiniti ako ste replikaciju za Azure.

Preporučujemo da postavite jedno Hyper-V glavno računalo kao poslužitelj za oporavak tako da se taj VM moguće je replicirati na da biste provjerili propusnosti.  To možete preskočiti, ali nećete moći mjerenje propusnosti osim u slučaju da to učinite.

1. Ako želite koristiti klaster čvor kao replike konfiguriranje Hyper-V replike broker:

    - Otvorite **Upravitelj klaster prebacivanje**u **Upravitelj poslužitelja**.
    - Povezivanje s klaster, isticanje klaster naziv i kliknite **Akcije** > **Konfiguriranje uloge** da biste otvorili čarobnjak visoke dostupnosti.
    - **Odaberite ulogu** kliknite **Broker replike Hyper-V**. U čarobnjaku navedite **NetBIOS naziv** i **IP adresu** koja će se koristiti kao točka povezivanja klaster (naziva se klijent pristupnu točku). **Hyper-V replike Broker** će se konfigurirati, rezultirajuća u nazivu točke klijentskog programa access koje Imajte na umu.
    - Provjerite je li ulogu Hyper-V replike Broker uspješno priključuje na mrežu i uspijeva putem između sve čvorove klaster. Da biste to učinili, desnom tipkom miša kliknite željenu ulogu, pokažite na **Premjesti**, a zatim **Odaberite čvor**. Odaberite čvor > **u redu**.
    - Ako koristite provjeru autentičnosti utemeljenu na certifikata, provjerite je li svaki čvor klaster i klijentski pristup točki imaju certifikat koji je instaliran.
2.  Omogućivanje replike poslužitelja:

    - Za klaster otvorite upravitelj klaster pogreške, povezivanje s klaster i kliknite **uloge** > odaberite ulogu > s **Replikacijom postavke**> **Omogući ovaj klaster kao replike poslužitelj**. Imajte na umu da ako koristite klaster kao replike morat ćete imati ulogu Hyper-V replike Broker izlaganje na klasteru primarni web-mjestu kao i.
    - Za poslužitelj samostalne otvorite upravitelj Hyper-V. U oknu **Akcije** kliknite **Hyper-V postavke** poslužitelja koje želite omogućiti, a u **Konfiguraciji replikacije** kliknite **Omogući kao replike poslužitelja**.
3. Postavljanje provjere autentičnosti:

    - U **provjeru autentičnosti i priključaka** odaberite način provjere autentičnosti na primarnom poslužitelju i priključke za provjeru autentičnosti. Ako koristite certifikata kliknite **Odabir potvrde** da biste odabrali jedan. Koristite Kerberos ako Hyper-V domaćini primarni i oporavak u istu domenu ili pouzdanih domena. Pomoću certifikata za različite domene ili radne grupe implementacije.
    - U sekciji **autorizacije i pohranu** dopustiti **bilo koji** čija je autentičnost provjerena (primarni) poslužitelj za slanje podataka replikacije ovaj poslužitelj replike. Kliknite **u redu** ili **Primijeni**.

    ![](./media/site-recovery-capacity-planning-for-hyper-v-replication/image1.png)

    - Pokretanje **netsh http prikaz servicestate** provjerite je li ga slušatelj pokrenut za protokol/priključak koje ste naveli:  
4. Postavljanje vatrozida. Tijekom instalacije značajke Hyper-V pravila vatrozida stvaraju se omogućuje promet na zadanom priključke (HTTPS na 443, Kerberos na 80). Ta pravila omogućiti na sljedeći način:

        - Certificate authentication on cluster (443): **Get-ClusterNode | ForEach-Object {Invoke-command -computername \$\_.name -scriptblock {Enable-Netfirewallrule -displayname "Hyper-V Replica HTTPS Listener (TCP-In)"}}**
        - Kerberos authentication on cluster (80): **Get-ClusterNode | ForEach-Object {Invoke-command -computername \$\_.name -scriptblock {Enable-Netfirewallrule -displayname "Hyper-V Replica HTTP Listener (TCP-In)"}}**
        - Certificate authentication on standalone server: **Enable-Netfirewallrule -displayname "Hyper-V Replica HTTPS Listener (TCP-In)"**
        - Kerberos authentication on standalone server: **Enable-Netfirewallrule -displayname "Hyper-V Replica HTTP Listener (TCP-In)"**

## <a name="step-3-run-the-capacity-planner-tool"></a>Korak 3: Pokretanje alata za planer kapaciteta

Kada pripremite primarni web-mjesta i postavljanje poslužitelja za oporavak možete pokrenuti alat.

1. [Preuzmite](https://www.microsoft.com/download/details.aspx?id=39057) alat iz Microsoft Download Center.
2. Pokretanje alata iz jednog od primarni poslužitelji (ili od čvorove iz primarnog skupine). Desnom tipkom miša kliknite .exe datoteke, a zatim odaberite **Pokreni kao administrator**.
3. U **prije nego što počnete** odredite koliko želite prikupiti podatke. Preporučujemo da pokrenete alat tijekom radnog vremena da biste bili sigurni da je podataka predstavnika. Ako samo želite provjeriti valjanost veza s mrežom, možete prikupiti za samo nekoliko minuta.

    ![](./media/site-recovery-capacity-planning-for-hyper-v-replication/image2.png)

4. U **Detalji primarni web-mjesta** Navedite naziv poslužitelja ili FQDN za glavno računalo samostalne ili za klaster odredite FQDN klijent prihvatili zareza, naziv klaster ili svi čvorovi u klasteru, a zatim kliknite **Dalje**. Alat za automatski otkriva naziv je pokrenut na poslužitelju. Alat za uzima VMs koje je moguće nadzirati za navedene poslužitelje.

    ![](./media/site-recovery-capacity-planning-for-hyper-v-replication/image3.png)

5. **Detalji replike web-mjesta** ako ste replikaciju za Azure ili ako ste replikaciju sekundarnu podatkovnim centrom, a još niste postavili replike poslužitelj, odaberite **Preskoči testova koje obuhvaćaju replike web-mjesta**. Ako su replikaciju sekundarnu podatkovnim centrom i postavite vrsti replike u FQDN samostalni poslužitelj ili pristupnoj točci klijenta za klaster **naziv poslužitelja (**ili) Hyper-V replike Broker Kapaciteta.

    ![](./media/site-recovery-capacity-planning-for-hyper-v-replication/image4.png)

6. U **Prošireni detalja replike** omogućiti **preskočiti testova koje obuhvaćaju prošireni replike web-mjesta**. Oni ne podržava oporavak web-mjesta.
7. U **Odaberite VMs za replikaciju** alata povezuje s poslužiteljem ili klaster i prikazuje VMs i diskova koji se izvodi na poslužitelju primarni, skladu s postavkama navedene na stranici **Detalji primarni web-mjesta** . Imajte na umu da se ne izvode VMs koje su već omogućene za replikaciju ili koji se neće se prikazati. Odaberite VMs za koju želite prikupiti metriku. Odabir u VHDs automatski prikuplja podatke za na VMs previše.
9. Ako ste konfigurirali replike poslužitelja ili klaster, u **mreži informacija** navedite djelomičnog WAN propusnosti mislite da će se koristiti između primarni i replike web-mjesta i odaberite certifikate ako ste konfigurirali provjera autentičnosti potvrda.

    ![](./media/site-recovery-capacity-planning-for-hyper-v-replication/image5.png)

10. U **sažetku** provjerite postavke pa kliknite **Dalje** da biste započeli prikupljanje metriku. Alat za tijek i status se prikazuje na stranici **Izračun kapaciteta** . Kada završi alat za pokretanje kliknite **Prikaz izvješća** da biste prešli na izlaz. Prema zadanim postavkama izvješća i zapisnika pohranjuju se u **%systemdrive%\Users\Public\Documents\Capacity planer**.

    ![](./media/site-recovery-capacity-planning-for-hyper-v-replication/image6.png)


## <a name="step-4-interpret-the-results"></a>Korak 4: Tumačenje rezultata
Slijedi nekoliko važnih metriku. Možete zanemariti mjernih podataka koji nisu ovdje naveden. Oni niste prikladno za oporavak web-mjesta.

### <a name="on-premises-to-on-premises-replication"></a>Lokalni za lokalni replikacije
  - Utjecaj replikacije na primarni host računalnim, memorije
  - Utjecaj replikacije na primarni, prostora za pohranu oporavak domaćini, IOPS
  - Ukupna propusnost potrebnu za replikaciju delta (MB/s)
  - Opažena mrežna propusnost između primarni glavno računalo i oporavak glavnog računala (MB/s)
  - Prijedlog za broj idealna aktivni paralelno prijenosa između dva domaćini/skupina

### <a name="on-premises-to-azure-replication"></a>Lokalni za Azure replikacije
  - Utjecaj replikacije na primarni host računalnim, memorije
  - Utjecaj replikacije primarni host prostora za pohranu prostora na disku, IOPS
  - Ukupna propusnost potrebnu za replikaciju delta (MB/s)

## <a name="more-resources"></a>Dodatni resursi

- Detaljne informacije o alatu za čitanje dokumenta koji se pridružuje preuzimanje alata.
- Pogledajte vodič alat na Keith Mayer [TechNet bloga](http://blogs.technet.com/b/keithmayer/archive/2014/02/27/guided-hands-on-lab-capacity-planner-for-windows-server-2012-hyper-v-replica.aspx).
- [Dobili željene rezultate](site-recovery-performance-and-scaling-testing-on-premises-to-on-premises.md) naš performansi testiranje za lokalni za lokalni Hyper-V replikacije



## <a name="next-steps"></a>Daljnji koraci

Nakon što završite planiranje kapaciteta možete pokrenuti implementacija oporavak web-mjesta:

- [Replicirati Hyper-V VMs u VMM oblaka za Azure](site-recovery-vmm-to-azure.md)
- [Replicirati VMs Hyper-V (bez VMM) za Azure](site-recovery-hyper-v-site-to-azure.md)
- [Replicirati Hyper-V VMs između VMM web-mjesta](site-recovery-vmm-to-vmm.md)
- [Između VMM web-mjesta s SAN replicirati VMs Hyper-V](site-recovery-vmm-san.md)
- [Replicirati hyper-V VMs na jednom VMM poslužitelju](site-recovery-single-vmm.md)