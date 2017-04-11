<properties 
   pageTitle="Prikaz i upravljanje zadacima StorSimple virtualne polja | Microsoft Azure"
   description="U članku se opisuje stranici StorSimple Upravitelj servisa zadacima i kako ga koristiti za praćenje nedavne i trenutni za polja virtualne StorSimple."
   services="storsimple"
   documentationCenter="NA"
   authors="alkohli"
   manager="carmonm"
   editor=""/>
<tags 
   ms.service="storsimple"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="na"
   ms.date="06/07/2016"
   ms.author="alkohli" />

# <a name="use-the-storsimple-manager-service-to-view-jobs-for-the-storsimple-virtual-array"></a>Prikaz zadataka za polja virtualne StorSimple pomoću servisa StorSimple Manager

## <a name="overview"></a>Pregled

Stranica za **poslove** sadrži jedan središnje portal za prikaz i upravljanje pokrenute na virtualne polja (poznat i kao lokalne virtualne uređaji) koji su povezani s usluge StorSimple Upravitelj zadataka. Možete pogledati izvodi, dovršeni i neuspjelih poslova više virtualne uređaja. Rezultati se prikazuju u tabličnom obliku. 

![Zadataka](./media/storsimple-ova-manage-jobs/ovajobs1.png)

Možete brzo pronaći zadatke koji su zainteresirani filtriranjem prema polja kao što su:

- **Status** – možete pretraživati za sve zadatke pokrenut, Dovršeno ili nije uspjelo.
- **Od i do** – poslove moguće je filtrirati prema rasponu datuma i vremena.
- **Vrsta** – vrsta posla se Vrati sve, sigurnosno kopirati, prebacivanje, preuzimanje ažuriranja ili instaliraj ažuriranja.
- **Uređaji** – poslove su pokrenuti na određeni uređaj povezan s na servisu. Filtrirani poslove pa su pozivu na temelju sljedećim atributima:

    - **Vrsta** – vrsta posla se Vrati sve, sigurnosno kopirati, prebacivanje, preuzimanje ažuriranja ili instaliraj ažuriranja.

    - **Status** – poslove može biti sve pokrenut, Dovršeno ili nije uspjelo.

    - **Entitet** – poslove mogu se pridružiti glasnoće, zajedničko korištenje ili uređaj. 

    - **Uređaj** – naziv uređaja na kojem je posao započet.

    - Početak **rada na** – vrijeme pokretanja posla.

    - **Tijeku** – postotak dovršenog izvodi posla. Za dovršenog posla, to mora uvijek biti 100%.

Na popisu zadataka Osvježi svakih 30 sekundi.

## <a name="view-job-details"></a>Prikaz detalja o posla

Izvršite sljedeće korake da biste vidjeli detalje bilo koji zadatak.

#### <a name="to-view-job-details"></a>Da biste vidjeli detalje posla

1. Na stranici **Poslovi** prikazati od poslova su zainteresirani tako da pokrenete upit s odgovarajućim filtrima. Možete tražiti dovršene ili izvodi zadatke.

2. Odaberite zadatak s tablični popis zadataka.

3. Pri dnu stranice kliknite **Detalji**.

4. U dijaloškom okviru **Detalji** možete pogledati status, pojedinosti te statistike vremena. Sljedeća ilustracija prikazuje primjer dijaloškog okvira **Sigurnosne kopije zadatka pojedinosti** .
 
    ![Stranica s detaljima poslova](./media/storsimple-ova-manage-jobs/ovajobs2.png)

#### <a name="job-failures-when-the-virtual-machine-is-paused-in-the-hypervisor"></a>Neuspjelim kada pauzirano je na hypervisor virtualnog računala

Kada je zadatak u tijeku na vašem StorSimple virtualne polja i uređaja (dodijeljena hypervisor virtualnog računala) pauzirano je za veće od 15 minuta, posao neće uspjeti. To je zbog vremenom StorSimple virtualne polja koja se sinkronizirani s vremenom Microsoft Azure. Primjer pogreška za posao vraćanja prikazuju se u sljedećim snimku zaslona.

![Vraćanje nije uspjelo posla](./media/storsimple-ova-manage-jobs/restorejobfailure.png)

Ove pogreške će se primijeniti na sigurnosnog kopiranja, vraćanja, ažuriranje i Prebacivanje zadataka. Ako je Hyper-V dodijeljena virtualnog računala, na računalu naposljetku sinkronizirati vrijeme s vaše hypervisor. Kada se to dogodi, možete ponovno pokrenite posla. 

## <a name="next-steps"></a>Daljnji koraci

[Saznajte kako koristiti lokalne web korisničkog Sučelja za administraciju sustava StorSimple virtualne polja](storsimple-ova-web-ui-admin.md).
