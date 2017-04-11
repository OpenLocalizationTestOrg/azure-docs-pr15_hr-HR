<properties
   pageTitle="Nadzor resursa u operacije upravljanja paket sigurnost i rješenje nadzora | Microsoft Azure"
   description="Ovaj dokument omogućuje korištenje OMS sigurnost i nadzor mogućnosti i praćenje resurse prepoznavanje sigurnosnim problemima."
   services="operations-management-suite"
   documentationCenter="na"
   authors="YuriDio"
   manager="swadhwa"
   editor=""/>

<tags
   ms.service="operations-management-suite"
   ms.topic="article" 
   ms.devlang="na"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="10/18/2016"
   ms.author="yurid"/>

# <a name="monitoring-resources-in-operations-management-suite-security-and-audit-solution"></a>Nadzor resursa u operacije upravljanja paket sigurnost i rješenje nadzora

Ovaj dokument omogućuje vam pomoću mogućnosti nadzora i sigurnost OMS identificirati sigurnosnim problemima i praćenje resurse.

## <a name="what-is-oms"></a>Što je OMS?

Microsoft operacije upravljanja paket (OMS) je tvrtke Microsoft cloud koji se temelje IT rješenja upravljanja koji olakšava upravljanje i zaštita lokalnih i infrastrukture u oblaku. Dodatne informacije o OMS potražite u članku [Paket za upravljanje operacije](https://technet.microsoft.com/library/mt484091.aspx).

## <a name="monitoring-resources"></a>Nadzor resursa

Kad god je to moguće, preporučujemo da biste spriječili događa lakim sigurnošću. Međutim, nije moguće da biste spriječili sve sigurnošću. Kada se dogodi incident vezan uz sigurnost, morate da biste bili sigurni da je minimiziran njegov utjecaja.  Postoje tri ključna preporuke koje je moguće koristiti da biste minimizirali broj i utjecaj sigurnošću:

- Često procijenite slabe točke u svom okruženju.
- Često provjerite sve sustavi za računala i mrežne uređaje da biste bili sigurni da su sve najnovije zakrpa.
- Često provjerite sve zapisnika i Mehanizmi zapisivanje, uključujući zapisnika događaja operacijski sustav, određene zapisnika aplikacije i zapisnika sustava za otkrivanje podataka.

Sigurnost OMS i nadzora rješenja omogućuje IT aktivno praćenje svih resursa, čime se olakšava minimiziranje utjecaj sigurnošću. Sigurnost OMS i nadzora je sigurnost domene koje je moguće koristiti za nadzor resursa. Sigurnost domene omogućuje brz pristup mogućnosti, za sigurnost nadzor sljedećim domenama će obuhvaćeno dodatne detalje:

- Procjena od zlonamjernog softvera
- Ažuriranje procjenu
- Identitet i pristup

> [AZURE.NOTE] Pregled tih mogućnosti, pročitajte [Uvod u sigurnost paket za upravljanje operacije i rješenje nadzora](oms-security-getting-started.md).

### <a name="monitoring-system-protection"></a>Zaštita sustava za nadzor

U obrane u pristup dubinu, svaki sloj zaštite je važno da cjelokupan stanje sigurnosti vaše resursa. Računalima s otkriven prijetnji i računalima s nedovoljno zaštitu prikazani su na vrednovanje zlonamjernog softvera pločici u odjeljku Sigurnost domene. Pomoću informacija na vrednovanje zlonamjernog softvera možete prepoznati plan da biste primijenili zaštitu s poslužiteljima koji je potreban. Da biste pristupili tu mogućnost, slijedite korake u nastavku:

1. U **Paketu za upravljanje operacije Microsoft** glavna nadzorna ploča kliknite **Sigurnost i nadzora** pločica.

    ![Sigurnost i nadzora](./media/oms-security-responding-alerts/oms-security-responding-alerts-fig1.png)

2. **Sigurnost i nadzora** nadzornoj ploči kliknite **Antimalware procjenu** u odjeljku **Sigurnost domene**. Nadzorna ploča za **Procjenu Antimalware** pojavit će se kao što je prikazano u nastavku:

![Procjena od zlonamjernog softvera](./media/oms-security-monitoring-resources/oms-security-monitoring-resources-fig2-ga.png)

Nadzorna ploča za **Procjenu zlonamjerni softver** možete koristiti da biste odredili sljedeća pitanja zaštite:

- **Aktivni prijetnji**: računala na kojima su ugrožena, a imate aktivni prijetnji u sustavu.
- **Remediated prijetnji**: računala na kojima su ugrožena, ali u prijetnji su remediated.
- **Potpis ažuran**: računala koja su omogućena zaštita od zlonamjernog softvera, ali potpis je istekao rok.
- **Nema zaštite u stvarnom vremenu**: računala koje nemaju antimalware instaliran.

### <a name="monitoring-updates"></a>Praćenje ažuriranja 

Primjena najnovija sigurnosna ažuriranja je sigurnost preporučenim načinom rada, a trebali biste ugrađeni u strategije upravljanja ažuriranja. Servis za Microsoft Agent nadzor (HealthService.exe) čita informacije o ažuriranju s nadziranim računala i šalje te ažurirane podatke OMS servis u oblaku za obradu. Servis Microsoft Agent nadzor je konfiguriran kao servis za automatsko, a zatim ga trebao bi biti uvijek pokrenut u ciljnom računalu.

![Praćenje ažuriranja](./media/oms-security-monitoring-resources/oms-security-monitoring-resources-fig3.png)

Logika primjenjuje se ažuriranja podataka i servisa u oblaku zapisa podataka. Ako se pronađe ažuriranja koja nedostaju, se ne prikažu na nadzornoj ploči za **ažuriranja** . Rad s ažuriranja koja nedostaju i razvoj plan da biste ih primijeniti na poslužiteljima na kojima su potrebni možete koristiti na nadzornoj ploči **ažuriranja** . Slijedite korake u nastavku da biste pristupili na nadzornoj ploči **ažuriranja** :

1. U **Paketu za upravljanje operacije Microsoft** glavna nadzorna ploča kliknite **Sigurnost i nadzora** pločica.
2. Nadzornoj ploči **Sigurnost i nadzora** u odjeljku **Sigurnost domene**kliknite **Ažuriraj procjenu** . Nadzorna ploča za ažuriranje pojavit će se kao što je prikazano u nastavku:

![Ažuriranje procjenu](./media/oms-security-monitoring-resources/oms-security-monitoring-resources-fig4.png)

U ovoj nadzornoj ploči možete izvršiti procjenu ažuriranje da biste razumjeli trenutnog stanja računala i adresa najvažnija prijetnji. Pomoću pločicu **od ključne važnosti ili sigurnosna ažuriranja** IT administratorima će moći pristupiti detaljne informacije o ažuriranjima koja nedostaju kao što je prikazano u nastavku:

![rezultat pretraživanja](./media/oms-security-monitoring-resources/oms-security-monitoring-resources-fig5.png)

Izvješće obuhvaćaju ključnih informacija koje je moguće koristiti da biste odredili vrstu prijetnju sustav je podložno, koji sadrži Microsoft KB članaka vezanih uz sigurnosnog ažuriranja i MS Bulletin koja sadrži više detalja o na slabe.

### <a name="monitoring-identity-and-access"></a>Nadzor identiteta i pristup

S korisnicima koji rade s bilo kojeg mjesta, pomoću različitih uređaja i pristupanje veliku količinu oblaka i lokalnog aplikacija je izuzetne svoja uvjerenja zaštićen. Napadima krađe vjerodajnica su oni u kojem napadaču prethodno poboljšava pristup običnog korisničke vjerodajnice za pristup sustavu unutar mreže. Više puta, ova početne napadi samo omogućuje vam pristup mreži, konačni cilj je da biste otkrili ovlasti računa. 

Attackers će ostati na mreži pomoću slobodno dostupne tooling da biste izdvojili vjerodajnice iz sesije drugih računa prijavljeni. Ovisno o konfiguraciji sustava te vjerodajnice možete se izdvajati iz oblika raspršivanja, karte i lozinke even običan tekst.  

> [AZURE.NOTE] strojevima koji izravno izložen putem Interneta prikazat će se više nije uspjelo pokušaja pokušaja prijave pomoću sve vrste poznati korisnička imena (npr. Administrator). U većini slučajeva ako se ne koriste poznati korisnička imena i ako je lozinka je dovoljno Jaka je u redu.

Moguće je prepoznati te uljeza prije nego što ih ugroziti ovlasti računa. Omogućuje korištenje **OMS sigurnost i rješenje nadzora** praćenje identiteta i pristup. Slijedite korake u nastavku da biste pristupili na nadzornoj ploči **identiteta i pristupa** :

1. U **Paketu za upravljanje operacije Microsoft** glavna nadzorna ploča kliknite sigurnost i nadzora pločica.
2. **Sigurnost i nadzora** nadzornoj ploči kliknite **identiteta i pristup** u odjeljku **Sigurnost domene**. Na nadzornoj ploči **identiteta i pristup** pojavit će se kao što je prikazano u nastavku:

![identitet i pristup](./media/oms-security-monitoring-resources/oms-security-monitoring-resources-fig6-ga.png)

Kao dio strategije običnog nadzora, morate uključiti nadzor identitet. Administrator trebao bi izgledati ako postoji određene valjano korisničko ime koje ima mnogo prilikom pokušaja. To može označili ili napadač koji dobivene realni korisničko ime i pokušajte brute prisilno ili alat za automatsko koristi programiranih lozinku koja je istekla.

U ovom nadzorne ploče Omogući IT brzo prepoznavanje potencijalnih prijetnji vezane uz identiteta i pristup resursima tvrtke ili ustanove. To je određeni važno i prepoznavanje trendova potencijalne, na primjer u pločicu prijave tijekom vremena, možete vidjeti koliko je puta pokušaja prijave nije uspjelo izvršena vremenskom razdoblju. U ovom slučaju računala **FileServer** primio 35 pokušaja prijave. Dodatne informacije o ovom računalu možete istražiti tako da klikom na njega. 

![Dodatne informacije](./media/oms-security-monitoring-resources/oms-security-monitoring-resources-fig7-new.png)

Izvješće koje generira za ovo računalo poziva koristan detalje o ovaj uzorak. Primijetili da stupac **RAČUN** daje korisnički račun koji je korišten za pokušaju pristupiti sustavu, stupac **TIMEGENERATED** daje razdoblje u kojem je učinjeno pokušaj i stupac **LOGONTYPENAME** daje mjesto na kojem je učinjeno taj pokušaj. Ako te prilikom pokušaja su izvodi lokalno u sustavu programa, stupcu **PROCES** bi prikazuje naziv procesa. U slučajevima gdje se pokušaj prijave dolazi iz programa već imate naziv postupak dostupan, a sada možete izvršiti dodatno istrage u ciljnom sustavu.

## <a name="see-also"></a>Vidi također

U ovom dokumentu ste naučili kako koristiti OMS sigurnost i nadzor rješenje praćenje resurse. Dodatne informacije o sigurnosti OMS potražite u sljedećim člancima:

- [Pregled operacije upravljanja paket (OMS)](operations-management-suite-overview.md)
- [Uvod u sigurnost paket za upravljanje operacije i rješenje nadzora](oms-security-getting-started.md)
- [Praćenje i odgovarati na njih sigurnosnih upozorenja u operacije upravljanja paket sigurnost i rješenje nadzora](oms-security-responding-alerts.md)