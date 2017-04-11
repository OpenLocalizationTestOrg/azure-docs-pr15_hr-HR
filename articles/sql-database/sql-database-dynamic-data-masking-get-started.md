<properties
   pageTitle="Uvod u SQL baze podataka dinamične podatke Masking (Portal za Azure)"
   description="Upute za početak rada s SQL baza podataka dinamične podatke Masking na portalu za Azure"
   services="sql-database"
   documentationCenter=""
   authors="ronitr"
   manager="jhubbard"
   editor="v-romcal"/>

<tags
   ms.service="sql-database"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="07/10/2016"
   ms.author="ronitr; ronmat; v-romcal; sstein"/>


# <a name="get-started-with-sql-database-dynamic-data-masking-azure-portal"></a>Uvod u SQL baze podataka dinamične podatke Masking (Portal za Azure)

> [AZURE.SELECTOR]
- [Dinamičke podatke Masking - Azure klasični Portal](sql-database-dynamic-data-masking-get-started-portal.md)

## <a name="overview"></a>Pregled

SQL baza podataka dinamične podatke Masking ograničenjima izlaganje povjerljive podatke prema masking korisnicima velikim ovlastima. Dinamičke podatke masking nije podržana za V12 verziju baze podataka SQL Azure.

Dinamičke podatke masking sprječava neovlašten pristup osjetljivim podacima tako da omogućite klijentima da biste odredili koliko povjerljive podatke da bi se pojavila s minimalnim utjecaj na aplikacijskom sloju. To je značajka pravila temeljenu na sakriven osjetljive podatke u skupu rezultata upita poljima određenu bazu podataka dok je promjene podataka u bazi podataka.

Na primjer, službe poziva centru možda prepoznavanja pozivatelji nekoliko znamenki nad OIB ili broj kreditne kartice, ali te stavke podataka mora biti potpuno izložen predstavnika službe za korisnike. Pravilo masking može se definirati da maske sve, ali posljednje četiri znamenke OIB ni broj kreditne kartice u rezultatu skup bilo koji upit. Kao drugi primjer maske odgovarajuće podatke može se definirati za zaštitu podataka za osobnu identifikaciju osobe (PII) tako da je razvojni inženjer upit možete poslati radnog okruženja za rješavanje problema bez Narušavanje propisa usklađenosti.

## <a name="sql-database-dynamic-data-masking-basics"></a>SQL baza podataka dinamične podatke Masking osnove

Postavljanje dinamičke podatke masking pravila na portalu za Azure tako da odaberete postupka dinamičke Masking podataka u plohu konfiguracije SQL baze podataka ili plohu postavke.


### <a name="dynamic-data-masking-permissions"></a>Dozvole masking dinamičke podatke

Masking dinamičke podatke moguće je konfigurirati administratore baze podataka Azure, administrator poslužitelja ili officer uloge.

### <a name="dynamic-data-masking-policy"></a>Pravila masking dinamičke podatke

* **Korisnici SQL izuzeti iz masking** - skup korisnika SQL ili AAD identiteta koji će dobiti unmasked podataka u rezultatima upita SQL. Imajte na umu uvijek treba izostaviti iz masking korisnika s administratorskim ovlastima, a prikazat će se izvorne podatke bez sve maske za unos.

* **Masking pravila** - skupa pravila koja definira određenu polja da biste se maskirana i funkciju masking koja će se koristiti. Polja za određenu može se definirati pomoću naziv sheme baze podataka, naziv tablice i naziv stupca.

* **Masking funkcije** - skup metode koje kontroliraju izlaganje podataka za različitim scenarijima.

| Masking (funkcija) | Logika masking |
|----------|---------------|
| **Zadani**  |**Potpuna masking prema vrste podataka za određenu polja**<br/><br/>• Korištenje XXXX ili manje o. Ako je veličina polja manje od 4 znaka za vrste podataka niz (nchar, ntext, nvarchar).<br/>• Pomoću vrijednost nula za vrste numeričkih podataka (bigint, bitne, decimalne, int, novca, numeričke, smallint, smallmoney, tinyint, plutati, realni).<br/>• Pomoću 01 01 1900 za vrste podataka datuma/vremena (datum, datetime2, datumom i vremenom, datetimeoffset, smalldatetime, vrijeme).<br/>• SQL varijantu, zadana vrijednost trenutne vrste koristi.<br/>• Za XML dokument <masked/> koristi.<br/>• Koristite praznu vrijednost za vrste podataka posebno (Vremenska oznaka tablice, hierarchyid, GUID, binarni, slike, varbinary prostorno vrste).
| **Kreditna kartica** |**Masking način na koji se izlaže posljednje četiri znamenke određenu polja** te dodaje konstanta niza kao prefiks u obliku kreditne kartice.<br/><br/>XXXX-XXXX-XXXX-1234|
| **OIB** |**Masking način na koji se izlaže posljednje četiri znamenke određenu polja** i dodati konstanta niza kao prefiks u obliku programa American OIB.<br/><br/>XXX XX 1234 |
| **E-pošte** | **Način na koji se izlaže prvo slovo i zamjenjuje domene u sustavu XXX.com masking** pomoću prefiks konstanta niza u obliku adresu e-pošte.<br/><br/>aXX@XXXX.com |
| **Slučajni broj** | **Masking metoda koje generira slučajni broj** prema odabrane ograničenja i vrste stvarnih podataka. Ako jednake granice onu koja je označena funkcija masking bit će konstante broj.<br/><br/>![Navigacijsko okno](./media/sql-database-dynamic-data-masking-get-started/1_DDM_Random_number.png) |
| **Prilagođeni tekst** | **Masking način na koji se izlaže imena i prezimena znakova** i dodati prilagođene udaljenosti od ruba niza u sredini. Ako je izvorni niz kraće prikazanog prefiks i sufiks, koristit će se samo u nizu ispune. <br/>nastavak prefiks [udaljenosti od ruba]<br/><br/>![Navigacijsko okno](./media/sql-database-dynamic-data-masking-get-started/2_DDM_Custom_text.png) |


<a name="Anchor1"></a>
### <a name="recommended-fields-to-mask"></a>Preporučena polja za

Modul za preporuke DDM zastavicama određenih polja iz baze podataka kao povjerljivih polja koje se mogu prikladna za masking. U plohu dinamičke Masking podataka na portalu vidjet ćete preporučene stupaca za bazu podataka. Sve što trebate napraviti je kliknite **Dodavanje maske** za jedan ili više stupaca, a zatim **Spremi** da biste primijenili maska za ta polja.

## <a name="set-up-dynamic-data-masking-for-your-database-using-the-azure-portal"></a>Postavljanje masking dinamičke podatke za bazu podataka pomoću portala za Azure

1. Pokrenite Azure Portal na [https://portal.azure.com](https://portal.azure.com).

2. Pomaknite se do postavki plohu baze podataka koja sadrži povjerljive podatke koje želite maski.

3. Kliknite pločicu **Dinamičke Masking podataka** koji se pokreće konfiguracije plohu **Dinamičke Masking podataka** .

    * Osim toga, pomaknite se prema dolje do odjeljka **operacije** i kliknite **Dinamičke Masking podataka**.

    ![Navigacijsko okno](./media/sql-database-dynamic-data-masking-get-started/4_ddm_settings_tile.png)<br/><br/>


4. U konfiguraciji plohu **Dinamičke Masking podataka** vidjet ćete neke stupce baze podataka koje modul preporuke je zastavicom za masking. Da biste prihvatili preporuke, samo kliknite **Dodavanje maske** za jedan ili više stupaca i masku stvorit će se ovisno o Zadana vrsta stupca. Funkcija masking možete promijeniti tako da kliknete na pravilo masking i uređivanje u masking oblik polja u drugi oblik po izboru. Ne zaboravite kliknite **Spremi** da biste spremili postavke.

    ![Navigacijsko okno](./media/sql-database-dynamic-data-masking-get-started/5_ddm_recommendations.png)<br/><br/>


5. Da biste dodali maska za bilo koji stupac u bazi podataka, pri vrhu konfiguracije plohu **Dinamičke Masking podataka** kliknite **Dodavanje maske** da biste otvorili konfiguracije plohu **Dodavanje Masking pravila**

    ![Navigacijsko okno](./media/sql-database-dynamic-data-masking-get-started/6_ddm_add_mask.png)<br/><br/>

6. Odaberite **shemu**, **tablice** i **stupca** da biste definirali određenu polje koje će biti maskirana.

7. Da biste odabrali **Masking oblik polja** s popisa povjerljive podatke masking kategorije.

    ![Navigacijsko okno](./media/sql-database-dynamic-data-masking-get-started/7_ddm_mask_field_format.png)<br/><br/>     

8. Kliknite **Spremi** u podacima masking plohu pravilo da biste ažurirali skup masking pravila u dinamičke podatke masking pravila.

9. Upišite SQL korisnika ili AAD identiteta koji treba izuzeti iz masking, a imate pristup unmasked povjerljive podatke. Trebali biste razdvojenim zarezom popisa korisnika. Imajte na umu da korisnici s administratorskim ovlastima uvijek imati pristup unmasked izvorne podatke.

    ![Navigacijsko okno](./media/sql-database-dynamic-data-masking-get-started/8_ddm_excluded_users.png)

    >[AZURE.TIP] Da biste ga da bi aplikacijskom sloju mogao prikazati povjerljive podatke za korisnike aplikacije ovlasti, dodajte SQL korisnika ili AAD identiteta aplikacija koristi upita baze podataka. Preporučuje se da ovaj popis sadrži Minimalni broj povlaštene korisnike da biste minimizirali izlaganje povjerljive podatke.

10. Kliknite **Spremi** u podacima masking plohu konfiguracije da biste spremili novi ili ažurirani masking pravila.

## <a name="set-up-dynamic-data-masking-for-your-database-using-powershell-cmdlets"></a>Postavljanje dinamičke podatke masking za bazu podataka pomoću cmdleta ljuske Powershell

Potražite u članku [cmdleti za baze podataka Azure SQL](https://msdn.microsoft.com/library/azure/mt574084.aspx).


## <a name="set-up-dynamic-data-masking-for-your-database-using-rest-api"></a>Postavljanje masking dinamičke podatke za bazu podataka pomoću REST API-JA

U odjeljku [operacija za baze podataka Azure SQL](https://msdn.microsoft.com/library/dn505719.aspx).
