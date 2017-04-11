<properties
pageTitle="Korištenje podataka iz lokalne baze podataka sustava SQL Server u strojnog učenja | Azure"
description="Pomoću podataka iz lokalne baze podataka SQL Server da biste izvršili naprednom analitikom s Azure strojnog učenja."
services="machine-learning"
documentationCenter=""
authors="garyericson"
manager="jhubbard"
editor="cgronlun"/>

<tags
ms.service="machine-learning"
ms.workload="data-services"
ms.tgt_pltfrm="na"
ms.devlang="na"
ms.topic="article"
ms.date="09/16/2016"
ms.author="garye;krishnan"/>

# <a name="perform-advanced-analytics-with-azure-machine-learning-using-data-from-an-on-premises-sql-server-database"></a>Izvođenje naprednom analitikom s Azure strojnog učenja pomoću podataka iz lokalne baze podataka sustava SQL Server


Često velike tvrtke koje funkcioniraju s lokalnim podacima želite da iskoristite prednost skale i agility oblaka za svoje strojnog učenja radnih opterećenja. No žele poremetiti svoje trenutne poslovnih procesa i tijekove rada pomicanjem njihove lokalnih podataka s oblakom. Azure strojnog učenja sada podržava čitanje podataka iz lokalne baze podataka SQL Server i zatim obuka i bilježenje rezultata model s tim podacima. Više ne morate ručno kopirali i sinkroniziranje podataka između u oblak i lokalnog poslužitelja. Umjesto toga modul za **Uvoz podataka** u Azure strojnog učenja Studio sada vidite izravno iz lokalne baze podataka SQL Server za vaše obuka i bilježenje rezultata zadatke. 

Ovaj članak sadrži pregled načina ingress lokalnim podacima sustava SQL server u Azure strojnog učenja. Pretpostavlja se da ste upoznati s Azure strojnog učenja koncepte kao što su *radni prostori, module, skupova podataka, eksperimenata itd*...

> [AZURE.NOTE] Ta značajka nije dostupna za besplatne radne prostore. Dodatne informacije o strojnog učenja cijene i razine potražite u članku [Cijene za Azure strojnog učenja](https://azure.microsoft.com/pricing/details/machine-learning/).

<!-- --> 

[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]


## <a name="install-the-microsoft-data-management-gateway"></a>Instalirati pristupnik za upravljanje podacima u programu Microsoft

Da biste pristupili lokalnog sustava SQL Server baze podataka programa Azure strojnog učenja ćete morati preuzeti i instalirati Microsoftova pristupnika za upravljanje podacima. Kada konfigurirate pristupnik veze u strojnog učenja Studio imat ćete mogućnost preuzmite i instalirajte pristupnika pomoću dijaloškog okvira **Preuzimanje i register podataka pristupnika** opisanim.

Također možete instalirati pristupnik za upravljanje podacima na vrijeme preuzimanja i pokretanjem instalacijski paket MSI iz [Microsoftova centra za preuzimanje](https://www.microsoft.com/download/details.aspx?id=39717). Odaberite najnoviju verziju, odabir 32-bitne ili 64-bitni po potrebi za vaše računalo. Na MSI može se koristiti i da biste nadogradili na postojeće pristupnik za upravljanje podacima na najnoviju verziju s sve postavke sačuvati.

Pristupnik sadrži sljedeće preduvjete:

- Podržani verzija operacijskog sustava Windows su Windows 7, Windows 8/8.1, Windows 10, Windows Server 2008 R2, Windows Server 2012 i Windows Server 2012 R2.
- Preporučena konfiguracija za pristupnom računalu je barem GHz 2, 4 jezgri, 8 GB RAM-a i 80 GB diska.
- Ako glavno računalo hibernira, pristupnika nećete moći odgovora na zahtjeve za podatke. Stoga konfigurirajte odgovarajuće napajanja na računalu prije instalacije pristupnika. Instalacija pristupnika prikazuje poruku ako računalo nije konfiguriran tako da hibernacije.
- Jer Kopiraj aktivnosti pojavljuje se na određene učestalost, na korištenje resursa (CPU-a, memorije) na računalu slijedi isti uzorak s Vršna i vremena neaktivnosti. Upotreba resursa i ovisi o intenzivnog količinu podataka koja se premješta. Kada više kopija zadataka u tijeku ćete pridržavajte se pomaknuli prema gore tijekom vremena Vršna korištenje resursa. Dok je minimalne konfiguraciju naveden Tehnički dovoljno, trebali biste imati konfiguraciju s više resursa od minimalne konfiguraciju ovisno o tome na određene opterećenje za premještanje podataka.

Razmotrite sljedeće kada postavljanje i korištenje pristupnik za upravljanje podacima:

- Samo jedna pojava pristupnik za upravljanje podacima možete instalirati na jednom računalu.
- Možete koristiti jedan pristupnik za više lokalnih izvora podataka.
- Više pristupnika na različitim računalima možete povezati isti lokalni izvor podataka.
- Konfigurirati pristupnik za samo jedan radni prostor odjednom. Pristupnika nije moguće zajednički koristiti s radnim prostorima trenutno.
- Možete konfigurirati više pristupnika za jedan radni prostor. Ako, na primjer, možda želite koristiti pristupnika koji je povezan s test izvore tijekom razvoja i pristupnika radnog kada ste spremni operationalize.
- Pristupnik moraju biti na istom računalu kao izvor podataka, ali zadržavanje bliže s izvorom podataka skraćuje vrijeme za pristupnik za povezivanje s izvorom podataka. Preporučujemo da instalirate pristupnika na računalu koje se razlikuje od onog koji hostira lokalni izvor podataka da bi se na pristupnika i izvor podataka ne se natječu za resurse.
- Ako već imate pristupnik instaliran na vašem računalu posluživanje Power BI ili Azure podataka tvorničke scenariji, instalirajte zasebnom pristupnik za Azure strojnog učenja na drugom računalu. 

    > [AZURE.NOTE] Pristupnik za upravljanje podacima i Power BI pristupnik nije moguće pokrenuti na istom računalu.

- Morate koristiti pristupnik za upravljanje podacima za Azure strojnog učenja čak i ako koristite Azure ExpressRoute za druge podatke. Izvor podataka mora Smatraj na lokalni izvor podataka (u kojoj se nalazi iza vatrozida) čak i kada koristite ExpressRoute, a koristite pristupnik za upravljanje podacima da biste uspostavili veze između strojnog učenja i izvora podataka. 

Detaljne informacije o preduvjetima za instalaciju, korake za instalaciju i savjete za otklanjanje poteškoća možete pronaći u članku [Premještanje podataka između lokalnih izvora i u oblaku s pristupnik za upravljanje podacima](../data-factory/data-factory-move-data-between-onprem-and-cloud.md#considerations-for-using-data-management-gateway), počevši od odjeljku [okolnosti pri korištenju pristupnik za upravljanje podacima](../data-factory/data-factory-move-data-between-onprem-and-cloud.md#considerations-for-using-data-management-gateway).

## <a name="span-idusing-the-data-gateway-step-by-step-walk-classanchorspan-idtoc450838866-classanchorspanspaningress-data-from-your-on-premises-sql-server-database-into-azure-machine-learning"></a><span id="using-the-data-gateway-step-by-step-walk" class="anchor"><span id="_Toc450838866" class="anchor"></span></span>Ingress podatke iz vaše lokalne baze podataka SQL Server u Azure strojnog učenja


U ovom vodiču će postavljanje pristupnika za upravljanje podacima u radnom prostoru programa Azure strojnog učenja, konfigurirati i pročitajte podatke iz lokalne baze podataka sustava SQL Server.

> [AZURE.TIP] Prije nego što počnete, onemogućivanje preglednika blokator skočnih prozora za `studio.azureml.net`. Ako koristite Google Chrome preglednik, preuzmite i instalirajte jedan od nekoliko dodataka dostupne na web-Google Chrome spremišta [Kliknite jednom aplikacije nastavak](https://chrome.google.com/webstore/search/clickonce?_category=extensions).

### <a name="step-1-create-a-gateway"></a>Korak 1: Stvaranje pristupnika

Prvi korak je za stvaranje i postavljanje pristupnika za pristup lokalnog SQL baze podataka.

1. Prijavite se na [Azure strojnog učenja Studio](https://studio.azureml.net/Home/) i odaberite radnog prostora koji želite raditi.

2. Kliknite plohu **Postavke** na lijevoj strani, a zatim karticu **PRISTUPNICI podataka** pri vrhu.

3. Kliknite **Novi PRISTUPNIK PODACIMA** pri dnu zaslona.

    ![](media/machine-learning-use-data-from-an-on-premises-sql-server/new-data-gateway-button.png)

4. U dijaloškom okviru **Novi pristupnik podacima** unesite **Naziv pristupnika** i po želji dodajte **Opis**. Kliknite strelicu u donjem kutu desnoj da biste prešli na sljedeći korak konfiguracije.

    ![](media/machine-learning-use-data-from-an-on-premises-sql-server/new-data-gateway-dialog-enter-name.png)

5. U preuzimanje i register dijaloški okvir podaci pristupnika, kopirajte ključa za REGISTRACIJU PRISTUPNIKA u međuspremnik.

    ![](media/machine-learning-use-data-from-an-on-premises-sql-server/download-and-register-data-gateway.png)

6. <span id="note-1" class="anchor"></span>Ako još nije ste preuzeli i instalirali Microsoftova pristupnika za upravljanje podacima, kliknite za **Preuzimanje pristupnika za upravljanje podacima**. Tako ćete doći do Microsoft Download Center gdje možete odabrati verzija pristupnika vam je potrebna, preuzmite ga i instalirajte ga. U odjeljcima početak članka [Premještanje podataka između lokalnih izvora i u oblaku s pristupnik za upravljanje podacima](../data-factory/data-factory-move-data-between-onprem-and-cloud.md)možete pronaći detaljne informacije o preduvjetima za instalaciju, korake za instalaciju i savjete za otklanjanje poteškoća.

7. Kada je instaliran pristupnik, otvorit će se upravitelj konfiguracijama pristupnika za upravljanje podacima i prikazuje se dijaloški okvir **registrirati pristupnik** . Zalijepite **Ključ pristupnika registraciju** koju ste kopirali u međuspremnik, a zatim kliknite **Registracija**.

8. Ako već imate instaliran pristupnik, pokrenite upravitelj konfiguracijama pristupnika za upravljanje podacima, kliknite **Promjena ključa**, zalijepite  **Ključ pristupnika registraciju** koju ste kopirali u međuspremnik i kliknite **u redu**.

9. Nakon dovršetka instalacije prikazuje se **registrirati pristupnik** dijaloški okvir za Microsoft Management upravitelja konfiguracije pristupnika podacima. Zalijepite KLJUČ PRISTUPNIKA REGISTRACIJU koju ste kopirali u međuspremnik iznad, a zatim kliknite **Registracija**.

    ![](media/machine-learning-use-data-from-an-on-premises-sql-server/data-gateway-configuration-manager-register-gateway.png)

10. Konfiguraciju pristupnika dovršetka sljedeće vrijednosti postavljene na kartici **Polazno** u programu Microsoft Management upravitelja konfiguracije pristupnika podacima:

    - **Naziv pristupnika** i **naziv Instance** postavljene su na naziv pristupnika.

    - **Registracija** postavljen na **registriran**.

    - **Status** je postavljeno na **pokrenut**.

    - Na traci stanja pri dnu prikazuje **povezan na servis u Oblaku pristupnik za upravljanje podacima** uz Zelena kvačica.

     ![](media/machine-learning-use-data-from-an-on-premises-sql-server/data-gateway-configuration-manager-registered.png)

     Azure Studio strojnog učenja i ažurira prilikom registracije je uspješan.

    ![](media\machine-learning-use-data-from-an-on-premises-sql-server\gateway-registered.png)

11. U dijaloškom okviru **Preuzimanje i registrirati pristupnik podataka** kliknite potvrdite okvir da biste dovršili postavljanje. Na stranici **Postavke** prikazuje stanje pristupnika kao "Online". U desnom oknu pronaći ćete statusa i druge korisne informacije.

    ![](media\machine-learning-use-data-from-an-on-premises-sql-server\gateway-status.png)

12. U Microsoft upravitelja konfiguracije pristupnika za upravljanje podacima prijelaz na karticu **certifikata** . Potvrda navedena na ovoj kartici koristi se za šifriranje/dešifriranje vjerodajnica za lokalni spremišta podataka koje ste odredili na portalu. Ovo je zadana certifikata koje generira. Microsoft preporučuje ta promjena vlastiti certifikat koji ste sigurnosno kopirali u sustavu za upravljanje certifikata. Kliknite **Promijeni** namijenjenu vlastiti certifikat.

    ![](media\machine-learning-use-data-from-an-on-premises-sql-server\data-gateway-configuration-manager-certificate.png)

13. (neobavezno) Ako želite da biste omogućili opširno zapisivanje radi otklanjanja poteškoća s pristupnika, u programu Microsoft podataka upravljanja upravitelju konfiguracije pristupnika prijeđite na karticu **Dijagnostika** i potvrdite mogućnost **Omogući opširno zapisivanje za rješavanje problema** . Zapisivanje informacija pronaći ćete u preglednik događaja sustava Windows u odjeljcima **Zapisnici programa i servisa**  - &gt; čvor **Pristupnik za upravljanje podacima** . Da biste testirali veza pomoću pristupnika na lokalni izvor podataka možete koristiti i karticu **Dijagnostika** .

    ![](media\machine-learning-use-data-from-an-on-premises-sql-server\data-gateway-configuration-manager-verbose-logging.png)

Time se dovršava pristupnika postaviti postupak u Azure strojnog učenja.
Sada ste spremni za korištenje lokalne podatke.

Možete stvoriti i postavili više pristupnika u Studio za svaki radni prostor. Ako, na primjer, možda pristupnik koji se želite povezati s izvorima podataka na test tijekom razvoj, a drugi pristupnik za vaše izvore podataka radnog. Azure strojnog učenja pruža fleksibilnost da biste postavili više pristupnika ovisno o korporacijskom okruženju. Trenutno ne možete zajednički koristiti pristupnika između radnih prostora i samo jedan pristupnik moguće je instalirati na jednom računalu. Dodatne napomene prilikom instalacije pristupnika, potražite u članku [pitanja o korištenju pristupnik za upravljanje podacima](../data-factory/data-factory-move-data-between-onprem-and-cloud.md#considerations-for-using-data-management-gateway) u članku [Premještanje podataka između lokalnih izvora i u oblaku s pristupnik za upravljanje podacima](../data-factory/data-factory-move-data-between-onprem-and-cloud.md).

### <a name="step-2-use-the-gateway-to-read-data-from-an-on-premises-data-source"></a>Korak 2: Pomoću pristupnika za čitanje podataka iz lokalnih izvora podataka

Nakon postavljanja pristupnika možete dodati modul za **Uvoz podataka** pokusa ulazi podataka iz baze podataka SQL Server na lokalni.

1.  U strojnog učenja Studio, odaberite karticu **EKSPERIMENATA** , kliknite **+ NOVO** u donjem lijevom kutu i odaberite **Prazan eksperiment** (ili odaberite jednu od nekoliko oglednih eksperimenata dostupna).

2.  Pronađite i povucite modul za **Uvoz podataka** u područje crtanja eksperiment.

3.  Kliknite **Spremi kao** ispod područje crtanja. Unesite "Azure strojno učenje lokalnog SQL Server praktičnom vodiču" naziv eksperiment, odaberite radni prostor i kliknite kvačicu **u redu** .

    ![](media\machine-learning-use-data-from-an-on-premises-sql-server\experiment-save-as.png)

4.  Kliknite modul za **Uvoz podataka** da biste je odabrali, a zatim u oknu **Svojstva** s desne strane područje crtanja "Baze podataka SQL lokalnog" na padajućem popisu **izvora podataka** .

5.  Odaberite **pristupnik podacima** instaliran i registriran. Drugi pristupnika možete postaviti tako da odaberete "(Dodavanje novog podataka pristupnika...)".

    ![](media\machine-learning-use-data-from-an-on-premises-sql-server\import-data-select-on-premises-data-source.png)

6.  Unesite **naziv poslužitelja baze podataka** za SQL i **Naziv baze podataka**, zajedno sa SQL **upita baze podataka** koju želite izvesti.

7.  Pritisnite **Enter vrijednosti** u odjeljku **korisničko ime i lozinku** , a zatim unesite vjerodajnice za bazu podataka. Možete koristiti integriranu provjeru autentičnosti sustava Windows ili SQL Server ovisno o tome kako je konfigurirano vaše lokalnog sustava SQL Server.

    ![](media\machine-learning-use-data-from-an-on-premises-sql-server\database-credentials.png)
    
    Poruka "vrijednosti zahtijeva" će se promijeniti "skup vrijednosti" s Zelena kvačica. Samo morati unijeti vjerodajnice jednom osim u slučaju promjene podatke iz baze podataka ili lozinku. Azure strojnog učenja koristi certifikat koji ste naveli prilikom instaliran pristupnik za šifriranje vjerodajnica u oblaku. Azure nikad se pohranjuju vjerodajnica za lokalni bez šifriranja.

    ![](media\machine-learning-use-data-from-an-on-premises-sql-server\import-data-properties-entered.png)

8.  Kliknite **POKRENI** da biste pokrenuli pokusa.

Kada završi pokusa izvodi koje možete vizualizacija podataka koju ste uvezli iz baze podataka tako da kliknete izlazni priključak modula za **Uvoz podataka** , a zatim odaberete **Vizualiziraj**.

Kada završite razvoj sustava eksperiment, možete uvesti i operationalize modela. Putem servisa za obradu izvođenja, podaci iz lokalne baze podataka SQL Server konfiguriran u modulu za **Uvoz podataka** će za čitanje i koristiti za bilježenje rezultata. Tijekom korištenja odgovor servisnog zahtjeva za bilježenje rezultata lokalnih podataka, Microsoft preporučuje korištenje u [programu Excel programski dodatak](machine-learning-excel-add-in-for-web-services.md) umjesto toga. Trenutno pisanja sustava SQL Server lokalne baze podataka putem **Izvoz podataka** nije podržan eksperimenata ili objavljenu web-servisi.

