
<properties
    pageTitle="Povežite zdravlje Azure AD pomoću sinkronizacije | Microsoft Azure"
    description="To je stranica stanja povezivanje Azure AD koji će govori o praćenje Azure AD Connect sinkronizaciju."
    services="active-directory"
    documentationCenter=""
    authors="karavar"
    manager="samueld"
    editor="curtand"/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="10/18/2016"
    ms.author="vakarand"/>

# <a name="using-azure-ad-connect-health-for-sync"></a>Korištenje stanja povezivanje Azure AD za sinkronizaciju
Sljedeća dokumentacija je za Azure AD Connect (Sinkroniziranje) s povežite zdravlje Azure AD za nadzor.  Informacije o AD FS s stanjem povezati Azure AD za nadzor potražite u članku [Korištenje Azure AD povezati stanja AD fs](active-directory-aadconnect-health-adfs.md). Uz to, informacije o Active Directory Domain Services s povežite zdravlje Azure AD za nadzor potražite [Pomoću Azure AD povezivanje stanje s AD DS](active-directory-aadconnect-health-adds.md).

![Azure AD Connect stanje sinkronizacije](./media/active-directory-aadconnect-health-sync/sync-blade.png)

## <a name="alerts-for-azure-ad-connect-health-for-sync"></a>Upozorenja za Azure AD povezivanje stanje sinkronizacije
Odjeljku Azure AD povezivanje stanja upozorenja za sinkronizaciju daje popis aktivni upozorenja. Svakom upozorenju obuhvaća sve relevantne podatke, razlučivost koraka i veze s povezanim dokumentaciju. Odabirom upozorenje aktivna ni riješi vidjet ćete novi plohu s dodatnim informacijama, kao i koraci koje možete poduzeti da biste riješili upozorenja i veze dodatna dokumentacija. Pregled prošli podataka o upozorenja koja je Riješeno u prošlosti.

Odabirom upozorenje koje će se pružati s dodatnim informacijama, kao i koraka možete poduzeti da biste riješili upozorenja i veze dodatna dokumentacija.

![Pogreška pri sinkronizaciji Azure AD Connect](./media/active-directory-aadconnect-health-sync/alert.png)

### <a name="limited-evaluation-of-alerts"></a>Ograničeno vrednovanju upozorenja
Ako Azure AD Connect ne koristi zadanu konfiguraciju (na primjer, ako atribut filtriranje mijenja se iz konfiguracije zadane prilagođene konfiguracije), agent za Azure AD povežite zdravlje će neće prenijeti događaje pogreške vezane uz Azure AD Connect.

Na taj način vrednovanju upozorenja servis. Želite prikazat će se natpis koji označava ovaj uvjet na portalu Azure u odjeljku usluge.

![Azure AD Connect stanje sinkronizacije](./media/active-directory-aadconnect-health-sync/banner.png)

To možete promijeniti tako da kliknete "Postavke" i dopuštanje agent za Azure AD povezivanje stanja da biste prenijeli sve evidencije pogrešaka.

![Azure AD Connect stanje sinkronizacije](./media/active-directory-aadconnect-health-sync/banner2.png)

## <a name="sync-insight"></a>Sinkronizacija uvida
Administratori često želite znati o vrijeme potrebno za sinkronizaciju promjene Azure AD i iznos promjene izvršava. Ta značajka omogućuje jednostavan je način za to vizualizacija pomoću u nastavku grafikona:   

- Latencija operacija sinkronizacije
- Trend promjene u objekt

### <a name="sync-latency"></a>Latencija sinkronizacije
Ova značajka omogućuje grafički trend latencije sinkronizaciju operacije (uvoz, izvoz, itd.) za poveznike.  To omogućuje brz i jednostavan način da biste shvatili ne samo Latencija vaše operacije (veće ako imate veliki skup promjena koje su se pojavile), ali i njihovo otkrivanje anomalies u Latencija koje možda ćete morati dodatno istrage.

![Latencija sinkronizacije](./media/active-directory-aadconnect-health-sync/synclatency02.png)

Prema zadanim postavkama, prikazuje se samo Latencija ' "izvoza za Azure AD poveznik.  Da biste vidjeli dodatne operacije poveznik ili da biste pogledali operacija iz drugih poveznika, desnom tipkom miša kliknite grafikon, odaberite Uređivanje grafikona ili pritisnite gumb "Uređivanje Latencija grafikon", a zatim određene operacije i prikaz na telefonu.

### <a name="sync-object-changes"></a>Sinkronizacija objekt promjene
Ta značajka omogućuje grafički trend broja promjene koje se procjenjuju i izvesti Azure AD.  Danas, pokušaja prikupite te podatke iz zapisnika za sinkronizaciju je teško.  Grafikon će vam dati, ne samo jednostavniji način nadzor broj promjene koje se pojavljuje u svom okruženju, ali i vizualni prikaz pogrešaka koje su koje su se pojavile.

![Latencija sinkronizacije](./media/active-directory-aadconnect-health-sync/syncobjectchanges02.png)

## <a name="object-level-synchronization-error-report-preview"></a>Izvješće o pogrešci sinkronizacije razine objekta (pretpregled)
Ova značajka omogućuje izvješće o pogreškama sinkronizacije koje se mogu pojaviti kada identiteta podaci se sinkroniziraju između Windows Server AD i Azure AD pomoću Azure AD Connect.

- Izvješće pokriva pogreške snimio klijent za sinkronizaciju (Azure AD Connect verziju 1.1.281.0 ili noviji)
- Mora sadržavati došlo je do pogreške zadnja operacija sinkronizacije na modula za sinkroniziranje. ("Izvoz" Azure AD Connector.)
- Azure AD povežite zdravlje agent za sinkronizaciju, morate imati izlazni povezivanje potrebni krajnje točke za izvješće da biste uvrstili najnovije podatke. Potražite u [odjeljku preduvjeti](active-directory-aadconnect-health-agent-install.md#Requirements) detalje.
- Izvješće je **ažurirane nakon svakih 30 minuta** pomoću podataka prenosi povežite zdravlje Azure AD agent za sinkronizaciju.
Pruža sljedeće ključne značajke

    - Kategorizacija pogrešaka
    - Popis objekata uz poruku o pogrešci po kategoriji
    - Sve podatke o pogreškama na jednom mjestu
    - Usporedni usporedbe strani objekata uz poruku o pogrešci zbog sukoba
    - Preuzimanje izvješće o pogrešci kao CVS (uskoro dostupno)

### <a name="categorization-of-errors"></a>Kategorizacija pogrešaka
Izvješće kategorizira postojeće sinkronizacijom u sljedećim kategorijama:

| Kategorija | Opis |
| -------------- | ----------- |
| Dupliciranje atributa | Pogreške prilikom Azure AD Connect pokušava stvorite ili ažurirati dupliciranu vrijednosti atributa jedan ili više objekata u Azure AD mora biti jedinstvena u klijentu, kao što su proxyAddresses, UserPrincipalName. |
| Ne podudaraju se podataka | Pogreške prilikom mekih match ne uspije tako da odgovara objekata koji kao rezultat pogrešaka pri sinkronizaciji. |
| Pogreška pri provjeri valjanosti podataka | Pogreške zbog podataka koji nisu valjani, kao što su nepodržane znakova u ključnih atribute kao što su UserPrincipalName, oblikujte pogrešaka koje ne prođu provjeru prije nego što se pisane Azure AD.|
| Veliki atributa | Pogreške prilikom jedan ili više atributa ih je veće od dopuštene veličine, Duljina ili broj.|
| Drugi | Sve ostale pogreške koji ne stane u gornji kategorije. Na temelju povratnih informacija kategoriju će se podijeliti u kategorijama sub.

![Sinkronizacija pogreške izvješće sažetak](./media/active-directory-aadconnect-health-sync/errorreport01.png)
![sinkronizirati kategorija izvješća o pogrešci](./media/active-directory-aadconnect-health-sync/errorreport02.png)

### <a name="list-of-objects-with-error-per-category"></a>Popis objekata uz poruku o pogrešci po kategoriji
Dubinska analiza svake kategorije dat će se popis objekata koji se pojavljuju pogreške iz te kategorije.
![Popis izvješća o pogrešci za sinkronizaciju](./media/active-directory-aadconnect-health-sync/errorreport03.png)

### <a name="error-details"></a>Detalje o pogrešci
Praćenje podataka dostupna je u detaljan prikaz za svaku pogrešku

- Identifikatori *AD objekt* koji je uključen
- Identifikatori *Azure AD objekt* koji je uključen (kao što je primjenjivo)
- Opis pogreške i kako ispraviti
- Povezani članci

![Detalji izvješća za sinkronizaciju pogreške](./media/active-directory-aadconnect-health-sync/errorreport04.png)

### <a name="download-the-error-report-as-csv"></a>Preuzimanje izvješće o pogrešci kao CSV
Tu mogućnost uskoro dostupno. Ostanite definirati više ažuriranja.



## <a name="related-links"></a>Srodne veze
* [Otklanjanje poteškoća prilikom sinkronizacije](active-directory-aadconnect-troubleshoot-sync-errors.md)
* [Dupliciranje otpornost atributa](active-directory-aadconnectsyncservice-duplicate-attribute-resiliency.md)
* [Azure AD Connect stanja](active-directory-aadconnect-health.md)
* [Stanje Agent za instalaciju za Azure AD Connect](active-directory-aadconnect-health-agent-install.md)
* [Stanje postupci za Azure AD Connect](active-directory-aadconnect-health-operations.md)
* [Korištenje Azure AD povezivanje stanja AD fs](active-directory-aadconnect-health-adfs.md)
* [Korištenje Azure AD povezivanje stanje s AD DS](active-directory-aadconnect-health-adds.md)
* [Najčešća pitanja o stanju za Azure AD Connect](active-directory-aadconnect-health-faq.md)
* [Povijest verzija stanja za Azure AD Connect](active-directory-aadconnect-health-version-history.md)
