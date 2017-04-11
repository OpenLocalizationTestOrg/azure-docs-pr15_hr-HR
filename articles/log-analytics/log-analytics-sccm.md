<properties
    pageTitle="Upravitelj konfiguracije povezati prijava analitiku | Microsoft Azure"
    description="U ovom se članku prikazuje korake za povezivanje Upravitelj konfiguracije zapisnika analize i pokretanje analiza podataka."
    services="log-analytics"
    documentationCenter=""
    authors="bandersmsft"
    manager="jwhit"
    editor=""/>

<tags
    ms.service="log-analytics"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/29/2016"
    ms.author="banders"/>

# <a name="connect-configuration-manager-to-log-analytics"></a>Upravitelj konfiguracijama povezati zapisnika Analytics

Upravitelj konfiguracijama centar sustava možete povezati prijava analitiku u OMS sinkronizaciju uređaja zbirke podataka. To podatke iz implementaciju sustava Upravitelj konfiguracije postaje dostupan u OMS.

Postoji nekoliko koraka potrebnog za povezivanje Upravitelj konfiguracije OMS, pa ovdje brzi rundown cjelokupan procesa:

1. Na portalu za upravljanje Azure registrirajte Upravitelj konfiguracije web-aplikacije i/ili Web API app i bili sigurni da imate ID klijenta i klijent tajnu ključ iz Registracija iz servisa Azure Active Directory. Potražite u članku [Korištenje portala za stvaranje aplikacije servisa Active Directory i glavni koji može pristupiti resursima servisa](../resource-group-create-service-principal-portal.md) detaljne informacije o korištenju dovršili ovaj korak.
2. Azure upravljanja portalu [Upravitelj konfiguracije (registrirani web-aplikaciji) dati dozvolu za pristup OMS](#provide-configuration-manager-with-permissions-to-oms).
3. U Upravitelj konfiguracije, [dodajte vezu pomoću čarobnjaka za dodavanje OMS povezivanje](#add-an-oms-connection-to-configuration-manager).
4. U upravitelju konfiguracije možete [ažurirati svojstva veze](#update-oms-connection-properties) ako lozinku ili klijenta tajnu tipku ikad istječe ili se gube.
5. Podaci s portala sustava OMS, [Preuzmite i instalirajte Microsoft Agent za nadzor](#download-and-install-the-agent) na računalu s operacijskim sustavom veza servisa Upravitelj konfiguracije pokažite ulogu web-mjesta sustava. Agenta šalje podatke Upravitelj konfiguracije OMS.
6. U OMS [Uvoz zbirke od upravitelja konfiguracije](#import-collections) kao grupa računala.
7. U OMS, prikaz podataka iz Upravitelj konfiguracije kao [grupa računala](log-analytics-computer-groups.md).

Dodatne informacije o povezivanju Upravitelj konfiguracije OMS na [sinkroniziranje podataka iz Upravitelj konfiguracije u paketu za upravljanje Microsoft operacije](https://technet.microsoft.com/library/mt757374.aspx).



## <a name="provide-configuration-manager-with-permissions-to-oms"></a>Omogućuje Upravitelj konfiguracije s dozvolama OMS

Sljedeći se postupak sastoji Portal za upravljanje Azure s dozvolama za pristup OMS. Konkretno, morate dodijeliti *ulogu suradnika* za korisnika u grupu resursa. U nizu, koji omogućuje Portal za upravljanje Azure povezati Upravitelj konfiguracije OMS.

>[AZURE.NOTE] Morate navesti dozvole za OMS za Upravitelj konfiguracije. U suprotnom, primit ćete poruku o pogrešci prilikom korištenja čarobnjaka za konfiguraciju u upravitelju konfiguracije.


1. Otvorite [portal za Azure](https://portal.azure.com/) , a zatim kliknite **Pregledaj** > **Prijava analitiku (OMS)** da biste otvorili plohu prijava analitiku (OMS).  
2. Na plohu **Prijava analitiku (OMS)** kliknite **Dodaj** da biste otvorili plohu **OMS radnog prostora** .  
  ![OMS plohu](./media/log-analytics-sccm/sccm-azure01.png)
3. Na plohu **Radnog prostora OMS** navedite sljedeće podatke, a zatim kliknite **u redu**.
  - **OMS radnog prostora**
  - **Pretplate**
  - **Grupa resursa**
  - **Mjesto**
  - **Cijene sloju**  
    ![OMS plohu](./media/log-analytics-sccm/sccm-azure02.png)  

    >[AZURE.NOTE] Gornji primjer stvara novu grupu resursa. Grupa resursa koristi se samo za Upravitelj konfiguracije dati dozvole za radni prostor OMS u ovom primjeru.

4. Kliknite **Pregledaj** > **grupa resursa** da biste otvorili plohu **grupa resursa** .
5. U plohu **resursa grupa** kliknite grupu resursa koji ste stvorili iznad da biste otvorili u &lt;naziv grupe resursa&gt; plohu postavke.  
  ![plohu postavke za grupu resursa](./media/log-analytics-sccm/sccm-azure03.png)
6. U na &lt;naziv grupe resursa&gt; plohu postavke, kliknite kontrolu pristupa (IAM) da biste otvorili u &lt;naziv grupe resursa&gt; plohu korisnika.  
  ![resurs grupe korisnika plohu](./media/log-analytics-sccm/sccm-azure04.png)  
7. U na &lt;naziv grupe resursa&gt; plohu korisnika, kliknite **Dodaj** da biste otvorili plohu **Dodaj pristup** .
8. U plohu **Dodaj access** kliknite **Odaberi uloge**, a zatim ulogu **suradnika** .  
  ![Odaberite ulogu](./media/log-analytics-sccm/sccm-azure05.png)  
9. Kliknite **Dodavanje korisnika**, odaberite Upravitelj konfiguracije korisnika, kliknite **Odaberi**, a zatim **u redu**.  
  ![Dodavanje korisnika](./media/log-analytics-sccm/sccm-azure06.png)  


## <a name="add-an-oms-connection-to-configuration-manager"></a>Dodaj vezu s OMS upravitelju konfiguracije

Da biste dodali vezu na OMS, okruženje za Upravitelj konfiguracije morate imati [veza servisa pokažite](https://technet.microsoft.com/library/mt627781.aspx) konfigurirana za mrežni način rada.

1. U prostoru za **administraciju** Upravitelj konfiguracije odaberite **OMS poveznik**. Otvara se **Čarobnjak za dodavanje veze OMS**. Odaberite **Dalje**.

2. Na zaslonu **Općenito** potvrdite da ste napravili sljedeće radnje i detalje za svaku stavku, zatim odaberite **Dalje**.
  1. Na portalu za upravljanje Azure ste registriran Upravitelj konfiguracije kao web-aplikacije i/ili Web API app, a imate [ID klijenta s registracije](../active-directory/active-directory-integrating-applications.md).
  2. Na portalu za upravljanje Azure ste stvorili tajnu ključa aplikacije registrirani aplikacije servisa Azure Active Directory.  
  3. Na portalu za upravljanje Azure koji ste naveli registrirani web-aplikacije s dozvolom pristupa OMS.  
  ![Povezivanje OMS čarobnjak Općenita stranica](./media/log-analytics-sccm/sccm-console-general01.png)

3. Na zaslonu **Azure Active Directory** konfigurirao postavke veze za OMS unosom vaš **klijent** , **ID klijenta** i **Klijent tajna ključ** , a zatim odaberite **Dalje**.  
  ![Veza na stranicu OMS Čarobnjak za Azure Active Directory](./media/log-analytics-sccm/sccm-wizard-tenant-filled03.png)

4. Ako je uspješno napraviti sve ostale procedure, zatim podatke na zaslonu **Konfiguraciju OMS veze** automatski će se pojaviti na ovoj stranici. Informacije o postavke veze prikazivati za **Azure pretplatu** , **grupa Azure resursa** i **Prostoru paket za upravljanje operacije**.  
  ![Veza na stranicu OMS čarobnjak OMS vezi](./media/log-analytics-sccm/sccm-wizard-configure04.png)

5. Čarobnjak povezuje sa servisom OMS pomoću informacija koje ste unos. Odaberite uređaj zbirke koju želite sinkronizirati s OMS, a zatim kliknite **Dodaj**.  
  ![Odaberite zbirke](./media/log-analytics-sccm/sccm-wizard-add-collections05.png)

6. Provjerite postavke veze na zaslonu **Sažetak** , a zatim odaberite **Dalje**. Na zaslonu **tijeku** prikazuje stanje veze, a zatim treba **Dovršeno**.

>[AZURE.NOTE] OMS morate povezati s vrha sloja web-mjesta u hijerarhiju. Ako OMS povezati samostalne primarni web-mjesta, a zatim dodajte web-mjestu središnje administracije u okruženju sustava, morat ćete izbrisati, a zatim ponovno stvorite vezu OMS unutar nove hijerarhije.

Nakon što ste povezali Upravitelj konfiguracije OMS, možete dodati ili ukloniti zbirke i pogledali svojstva veze OMS.

## <a name="update-oms-connection-properties"></a>Ažuriranje OMS svojstva veze

Ako lozinku ili klijenta tajnu ključa ikad istječe ili nestaje, morate ručno ažurirati OMS svojstva veze.

1. U upravitelju konfiguracije idite na **Servise u Oblaku** , a zatim odaberite **OMS poveznik** da biste otvorili stranicu **OMS svojstva veze** .
2. Na ovoj stranici, kliknite karticu **Azure Active Directory** da biste vidjeli **klijentu** **ID klijenta** **klijent tajnu ključa isteka**. **Potvrda** **klijent tajnu ključ** ako je istekao.


## <a name="download-and-install-the-agent"></a>Preuzmite i instalirajte agenta

1. OMS portalu [Preuzimanje datoteke instalacijski program agent iz OMS](log-analytics-windows-agents.md#download-the-agent-setup-file-from-oms).
2. Primijenite jednu od sljedećih načina instaliranja i konfiguriranja agenta na računalu pokrenut na Upravitelj konfiguracije servisa točke web-mjesta sustava uloge veze:
  - [Instalirajte agent pomoću instalacijskog programa](log-analytics-windows-agents.md#install-the-agent-using-setup)
  - [Instalirajte agent pomoću naredbenog retka](log-analytics-windows-agents.md#install-the-agent-using-the-command-line)
  - [Instalirajte agent pomoću DSC u automatizaciji Azure](log-analytics-windows-agents.md#install-the-agent-using-dsc-in-azure-automation)


## <a name="import-collections"></a>Uvoz zbirke

Kada ste dodali vezu na OMS upravitelju konfiguracije i instalirali agenta na računalu s operacijskim sustavom veza servisa Upravitelj konfiguracije pokažite ulogu sustava web-mjesta, sljedeći je korak da biste uvezli zbirke od upravitelja konfiguracije u OMS kao grupa računala.

Kada je omogućen uvoz, informacija o članstvu zbirke dohvaća svaki 3 sata za ažuriranje članstva zbirke. Možete odabrati da biste onemogućili pristojbi u bilo kojem trenutku.

1. Na portalu OMS kliknite **Postavke**.
2. Kliknite karticu **Računala grupe** , a zatim kliknite karticu **SCCM** .
3. Odaberite **Upravitelj konfiguracije uvoz zbirke članstva** , a zatim kliknite **Spremi**.  
  ![Grupa računala - SCCM kartica](./media/log-analytics-sccm/sccm-computer-groups01.png)

## <a name="view-data-from-configuration-manager"></a>Prikaz podataka iz Upravitelj konfiguracije

Nakon što ste dodali vezu na OMS upravitelju konfiguracije i agenta na računalu instaliran pokrenuti Upravitelj konfiguracije servisa veze točke web-mjesta sustava ulogu, OMS slanje podataka agenta. U OMS, Upravitelj konfiguracije zbirke pojavljuju kao [grupe računala](log-analytics-computer-groups.md). Grupe na stranici **Upravitelja konfiguracije** u odjeljku **Grupe računala** možete pogledati u **postavkama**.

Nakon uvoza zbirke možete vidjeti koliko računala s članstvima zbirke otkrivena. Možete vidjeti i broj zbirki koji su uvezeni.

![Grupa računala - SCCM kartica](./media/log-analytics-sccm/sccm-computer-groups02.png)

Kada kliknete neki jednu, otvorit će se pretraživanja prikazuje ili sve od uvezene grupe ili sva računala koje pripadaju za svaku grupu. Pomoću [Zapisnika pretraživanja](log-analytics-log-searches.md), možete pokrenuti temeljitije analize podataka Upravitelj konfiguracije.

## <a name="next-steps"></a>Daljnji koraci

- Pomoću [Zapisnika pretraživanja](log-analytics-log-searches.md) da biste pogledali detaljne informacije o vašim podacima Upravitelj konfiguracije.
