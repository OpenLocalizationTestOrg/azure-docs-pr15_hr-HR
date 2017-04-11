<properties
   pageTitle="Početak rada s SQL podataka skladištu prijetnju otkrivanje"
   description="Upute za početak rada s prijetnju otkrivanje"
   services="sql-data-warehouse"
   documentationCenter=""
   authors="lodipalm"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="09/24/2016"
   ms.author="lodipalm;sonyama;barbkess"/>


# <a name="get-started-with-threat-detection"></a>Početak rada s prijetnju otkrivanje

> [AZURE.SELECTOR]
- [Nadzor](sql-data-warehouse-auditing-overview.md)
- [Otkrivanje prijetnju](sql-data-warehouse-security-threat-detection.md)

## <a name="overview"></a>Pregled

Otkrivanje prijetnju otkriva aktivnosti anomalous baze podataka koji označava potencijalne sigurnosnih prijetnji u bazu podataka. Otkrivanje prijetnju se pretpregled i nije podržana za SQL Data Warehouse.

Otkrivanje prijetnju nudi novu razinu zaštite, koja omogućuje klijentima za otkrivanje i odgovaranje na potencijalnih prijetnji kako nastaju unosom sigurnosnih upozorenja na anomalous aktivnosti. Korisnici mogu Istražite sumnjiva događaja da biste odredili ako oni rezultat pokušaj pristup, krši ili izrabljuje podataka u skladištu podataka pomoću [Azure SQL podataka skladištu nadzor](sql-data-warehouse-auditing-overview.md) .
Otkrivanje prijetnju olakšava jednostavne njegovu adresu potencijalne skladištu podataka ne moraju biti vrijednosnicu expert ili upravljanje dodatnom sigurnošću sustavi za nadzor.

Ako, na primjer, otkrivanje prijetnju otkriva određene aktivnosti anomalous baze podataka koji označava potencijalne prilikom pokušaja unos SQL. Unos SQL jedan je od uobičajenih Web aplikacije sigurnosnim problemima na Internetu, za napasti aplikacija utemeljenih na podacima. Attackers iskoristiti slabe točke aplikacije da biste Ubaci zlonamjerni SQL naredbe u aplikaciji polja za unos za kršenju ili mijenjati podatke u bazi podataka.


## <a name="set-up-threat-detection-for-your-database"></a>Postavljanje prijetnju otkrivanje za bazu podataka

1. Pokrenite Azure Portal na [https://portal.azure.com](https://portal.azure.com).

2. Dođite do plohu konfiguracija sustava SQL Data Warehouse želite nadzirati. U plohu postavke odaberite **nadzor i otkrivanje prijetnju**.

    ![Navigacijsko okno][1]

3. U konfiguraciji plohu **nadzor i otkrivanje prijetnju** pretvoriti **Uključeno** nadzor, čime ćete prikazati postavke otkrivanja prijetnju.

    ![Navigacijsko okno][2]

4. Uključivanje otkrivanja prijetnju **Uključeno** .

5. Konfiguriranje na popisu poruka e-pošte koji će primati sigurnosna upozorenja o otkrivanje aktivnosti u skladištu anomalous podataka.

6. Kliknite **Spremi** u plohu konfiguracije **nadzor i prijetnju otkrivanje** da biste spremili novi ili ažurirani nadzora i prijetnju otkrivanje pravila.

    ![Navigacijsko okno][3]


## <a name="explore-anomalous-data-warehouse-activities-upon-detection-of-a-suspicious-event"></a>Istražite aktivnosti u skladištu anomalous podataka nakon Otkrivanje sumnjivih događaja

1. Primit ćete obavijest e-poštom nakon otkrivanje aktivnosti anomalous baze podataka. <br/>
E-pošte pronaći ćete informacije o sumnjiva sigurnosnog događaja uključujući prirode anomalous aktivnosti, naziv baze podataka, naziv poslužitelja i vrijeme događaja. Osim toga, on pronaći ćete informacije o mogućim uzrocima i preporučene akcije da biste istražili i prevladavanje potencijalne prijetnju u bazu podataka.<br/>

    ![Navigacijsko okno][4]

2. U e-pošte, kliknite vezu **Zapisnika nadzora za SQL Azure** koja će pokretanje portalu klasični Azure te se navode relevantne zapise nadzor oko vrijeme sumnjiva događaja.

    ![Navigacijsko okno][5]

3. Kliknite na nadzora zapisa da biste vidjeli dodatne detalje o aktivnosti sumnjiva bazu podataka kao što je SQL naredbe IP pogreška razlog i klijenta.

    ![Navigacijsko okno][6]

4. U plohu nadzor zapisa kliknite **Otvori u programu Excel** da biste otvorili unaprijed konfigurirana predložak za uvoz i pokretanje dublju analizu zapisnika nadzora u doba sumnjiva događaja u programu excel.<br/>
**Bilješke:** U programu Excel 2010 ili noviju verziju, Power Query i **Brzo kombiniranje** postavka potreban je

    ![Navigacijsko okno][7]

5. Konfiguriranje postavke **Brzo kombiniranje** – na kartici vrpce **POWER QUERY** , odaberite **Mogućnosti** da biste prikazali dijaloški okvir mogućnosti. Odaberite sekciju izjave o zaštiti privatnosti, a zatim odaberite drugu mogućnost - "Zanemari razine zaštite privatnosti i potencijalno poboljšati performanse":

    ![Navigacijsko okno][8]

6. Da biste učitali SQL zapisnike nadzora, provjerite parametara u postavkama kartica pravilno postavljen i odaberite vrpci "Podaci" i kliknite gumb "Osvježi sve".

    ![Navigacijsko okno][9]

7. Rezultati prikazuju u **SQL zapisnike nadzora** lista koji omogućuje pokretanje dublju analizu anomalous aktivnosti koji su pronađeni a prevladavanje utjecaj sigurnosnog događaja u aplikaciji.


<!--Image references-->
[1]: ./media/sql-data-warehouse-security-threat-detection/1_td_click_on_settings.png
[2]: ./media/sql-data-warehouse-security-threat-detection/2_td_turn_on_auditing.png
[3]: ./media/sql-data-warehouse-security-threat-detection/3_td_turn_on_threat_detection.png
[4]: ./media/sql-data-warehouse-security-threat-detection/4_td_email.png
[5]: ./media/sql-data-warehouse-security-threat-detection/5_td_audit_records.png
[6]: ./media/sql-data-warehouse-security-threat-detection/6_td_audit_record_details.png
[7]: ./media/sql-data-warehouse-security-threat-detection/7_td_audit_records_open_excel.png
[8]: ./media/sql-data-warehouse-security-threat-detection/8_td_excel_fast_combine.png
[9]: ./media/sql-data-warehouse-security-threat-detection/9_td_excel_parameters.png
