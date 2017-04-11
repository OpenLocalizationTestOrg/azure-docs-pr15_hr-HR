<properties
   pageTitle="Uvod u SQL baze podataka dinamične podatke Masking (klasični Portal Azure)"
   description="Upute za početak rada s SQL baza podataka dinamične podatke Masking na portalu klasični Azure"
   services="sql-database"
   documentationCenter=""
   authors="ronitr"
   manager="jhubbard"
   editor=""/>

<tags
   ms.service="sql-database"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="07/10/2016"
   ms.author="ronitr; ronmat; v-romcal; sstein"/>

# <a name="get-started-with-sql-database-dynamic-data-masking-azure-classic-portal"></a>Uvod u SQL baze podataka dinamične podatke Masking (klasični Portal Azure)

> [AZURE.SELECTOR]
- [Dinamičke podatke Masking – Portal za Azure](sql-database-dynamic-data-masking-get-started.md)

## <a name="overview"></a>Pregled

SQL baza podataka dinamične podatke Masking ograničenjima izlaganje povjerljive podatke prema masking korisnicima velikim ovlastima. Dinamičke podatke masking nije podržana za V12 verziju baze podataka SQL Azure.

Masking dinamičke podatke pomaže u sprečavanju neovlaštenog pristupa osjetljivim podacima tako da omogućite klijentima da biste odredili koliko povjerljive podatke da bi se pojavila s minimalnim utjecajem na aplikacijskom sloju. To je značajka pravila temeljenu na sakriven osjetljive podatke u skupu rezultata upita iznad polja određenu bazu podataka, dok se ne mijenja podataka u bazi podataka.

Na primjer, službe poziva centru možda prepoznavanja pozivatelji nekoliko znamenki nad OIB ili broj kreditne kartice, ali te stavke podataka mora biti potpuno izložen predstavnika službe za korisnike. Pravilo masking može se definirati da maske sve, ali posljednje četiri znamenke OIB ni broj kreditne kartice u rezultatu skup bilo koji upit. Kao drugi primjer maske odgovarajuće podatke može se definirati za zaštitu podataka za osobnu identifikaciju osobe (PII) tako da je razvojni inženjer upit možete poslati radnog okruženja za rješavanje problema bez Narušavanje propisa usklađenosti.

## <a name="sql-database-dynamic-data-masking-basics"></a>SQL baza podataka dinamične podatke Masking osnove

Postavljanje dinamičke podatke masking pravila na portalu za klasični Azure-a na kartici nadzor i sigurnost za bazu podataka.


> [AZURE.NOTE] Da biste postavili dinamičke podatke masking na portalu za Azure, potražite u članku [Uvod u SQL baze podataka dinamične podatke Masking (Portal za Azure)](sql-database-dynamic-data-masking-get-started.md).


### <a name="dynamic-data-masking-permissions"></a>Dozvole masking dinamičke podatke

Masking dinamičke podatke moguće je konfigurirati administratore baze podataka Azure, administrator poslužitelja ili officer uloge.

### <a name="dynamic-data-masking-policy"></a>Pravila masking dinamičke podatke

* **Korisnici SQL izuzeti iz masking** - skup korisnika SQL ili AAD identiteta koji će dobiti unmasked podataka u rezultatima upita SQL. Imajte na umu uvijek treba izostaviti iz masking korisnika s administratorskim ovlastima, a prikazat će se izvorne podatke bez sve maske za unos.

* **Masking pravila** - skupa pravila koja definira određenu polja da biste se maskirana i funkciju masking koja će se koristiti. Polja za određenu može se definirati pomoću naziv sheme baze podataka, naziv tablice i naziv stupca.

* **Masking funkcije** - skup metode koje kontroliraju izlaganje podataka za različitim scenarijima.

| Masking (funkcija) | Logika masking |
|----------|---------------|
| **Zadani**  |**Potpuna masking prema vrste podataka za određenu polja**<br/><br/>• Korištenje XXXX ili manje o. Ako je veličina polja manje od 4 znaka za vrste podataka niz (nchar, ntext, nvarchar).<br/>• Pomoću vrijednost nula za vrste numeričkih podataka (bigint, bitne, decimalne, int, novac, numeričke, smallint, smallmoney, tinyint, plutati, realni).<br/>• Pomoću 01 01 1900 za vrste podataka datuma/vremena (datum, datetime2, datumom i vremenom, datetimeoffset, smalldatetime, vrijeme).<br/>• SQL varijantu, zadana vrijednost trenutne vrste koristi.<br/>• Za XML dokument <masked/> koristi.<br/>• Koristite praznu vrijednost za vrste podataka posebno (Vremenska oznaka tablice, hierarchyid, GUID, binarni, slike, varbinary prostorno vrste).
| **Kreditna kartica** |**Masking način na koji se izlaže posljednje četiri znamenke određenu polja** te dodaje konstanta niza kao prefiks u obliku kreditne kartice.<br/><br/>XXXX-XXXX-XXXX-1234|
| **OIB** |**Masking način na koji se izlaže posljednje četiri znamenke određenu polja** te dodaje konstanta niza kao prefiks u obliku programa American OIB.<br/><br/>XXX XX 1234 |
| **E-pošte** | **Način na koji se izlaže prvo slovo i zamjenjuje domene u sustavu XXX.com masking** pomoću prefiks konstanta niza u obliku adresu e-pošte.<br/><br/>aXX@XXXX.com |
| **Slučajni broj** | **Masking metoda koje generira slučajni broj** prema odabrane ograničenja i vrste stvarnih podataka. Ako jednake granice onu koja je označena funkcija masking bit će konstante broj.<br/><br/>![Navigacijsko okno](./media/sql-database-dynamic-data-masking-get-started-portal/1_DDM_Random_number.png) |
| **Prilagođeni tekst** | **Masking način na koji se izlaže imena i prezimena znakova** i dodati prilagođene udaljenosti od ruba niza u sredini. Ako je izvorni niz kraće prikazanog prefiks i sufiks, koristit će se samo u nizu ispune.<br/>nastavak prefiks [udaljenosti od ruba]<br/><br/>![Navigacijsko okno](./media/sql-database-dynamic-data-masking-get-started-portal/2_DDM_Custom_text.png) |


<a name="Anchor1"></a>

## <a name="set-up-dynamic-data-masking-for-your-database-using-the-azure-classic-portal"></a>Postavljanje masking dinamičke podatke za bazu podataka pomoću portala za klasični Azure

1. Pokrenite Azure klasični Portal na [https://manage.windowsazure.com](https://manage.windowsazure.com).

2. Kliknite bazu podataka koju želite maski, a zatim karticu **NADZOR i SIGURNOST** .

3. U odjeljku **dinamičke podatke masking**kliknite **OMOGUĆENO** da biste omogućili dinamičke podatke masking značajku.  

4. Upišite SQL korisnika ili AAD identiteta koji treba izuzeti iz masking, a imate pristup unmasked povjerljive podatke. Trebali biste odvojenih zarezom popisa korisnika. Imajte na umu da korisnici s administratorskim ovlastima uvijek imati pristup unmasked izvorne podatke.

    >[AZURE.TIP] Da biste ga da bi aplikacijskom sloju mogao prikazati povjerljive podatke za korisnike aplikacije ovlasti, dodajte SQL korisnika ili AAD identiteta aplikacija koristi upita baze podataka. Preporučuje se da ovaj popis sadrži Minimalni broj povlaštene korisnike da biste minimizirali izlaganje povjerljive podatke.

    ![Navigacijsko okno](./media/sql-database-dynamic-data-masking-get-started-portal/4_ddm_policy_classic_portal.png)

5. Pri dnu stranice na traci izbornika kliknite **Dodavanje MASKE** da biste otvorili u masking prozor konfiguracije pravila.

6. Na raspolaganju **shemu**, **tablica** i **stupaca** na padajućem popisu da biste definirali određenu polja koja će biti maskirana.

7. Na popisu povjerljive podatke masking kategorije odaberite **MASKING (opis funkcije)** .

    ![Navigacijsko okno](./media/sql-database-dynamic-data-masking-get-started-portal/5_DDM_Add_Masking_Rule_Classic_Portal.png)

8. Kliknite **u redu** u podacima masking prozor pravila da biste ažurirali skup masking pravila u dinamičke podatke masking pravila.

9. Kliknite **SPREMI** da biste spremili novi ili ažurirani masking pravila.


## <a name="set-up-dynamic-data-masking-for-your-database-using-transact-sql-statements"></a>Postavljanje dinamičke podatke masking za bazu podataka pomoću Transact-SQL naredbe

Pogledajte [dinamičke podatke Masking](https://msdn.microsoft.com/library/mt130841.aspx).

## <a name="set-up-dynamic-data-masking-for-your-database-using-powershell-cmdlets"></a>Postavljanje dinamičke podatke masking za bazu podataka pomoću cmdleta ljuske Powershell

Potražite u članku [cmdleti za baze podataka Azure SQL](https://msdn.microsoft.com/library/azure/mt574084.aspx).

## <a name="set-up-dynamic-data-masking-for-your-database-using-rest-api"></a>Postavljanje masking dinamičke podatke za bazu podataka pomoću REST API-JA

U odjeljku [operacija za baze podataka Azure SQL](https://msdn.microsoft.com/library/dn505719.aspx).
