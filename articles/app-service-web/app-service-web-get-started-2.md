<properties
    pageTitle="Dodavanje funkcija u prvom web app"
    description="Dodajte odlične značajke na prvi web-aplikaciju za nekoliko minuta."
    services="app-service\web"
    documentationCenter=""
    authors="cephalin"
    manager="wpickett"
    editor=""
/>

<tags
    ms.service="app-service-web"
    ms.workload="web"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="hero-article"
    ms.date="05/12/2016"
    ms.author="cephalin"
/>

# <a name="add-functionality-to-your-first-web-app"></a>Dodavanje funkcija u prvom web app

U [uvođenja prvi web-aplikaciju programa za Azure pet minuta](app-service-web-get-started.md)implementiran uzorak web-aplikacijama [Aplikacije servisa za Azure](../app-service/app-service-value-prop-what-is.md). U ovom se članku brzo ćete dodati neke sjajno utiče na distribuiranih web-aplikaciju. Nekoliko minuta ćete:

- Nametanje provjere autentičnosti za korisnike
- automatski Skaliraj aplikacije
- primanje upozorenja na performanse aplikacije

Bez obzira na to koju aplikaciju uzorka implementiran u prethodni članak možete pratiti u ovom praktičnom vodiču.

Tri aktivnosti ovog praktičnog vodiča su samo nekoliko primjera se kada spremate web-aplikaciju programa u aplikacije servisa za mnoge korisne značajke. Mnoge značajke dostupne u sloju **slobodno** (koji je što je pokrenut web-aplikaciju programa prvog), i vaša probna kredita možete koristiti da biste isprobali značajki koje zahtijevaju višu cijene razine. Ostale sigurni da se web-aplikaciju programa ostaje u sloju **slobodno** osim ako ga izričito ne mijenja u različitim cijene sloju.

>[AZURE.NOTE] Web-aplikacije stvorene pomoću Azure EŽA pokreće se u sloju **slobodno** kojega samo jednu instancu VM zajednički s kvota resursa poslužitelja. Dodatne informacije o se s sloju **slobodno** , potražite u članku [ograničenja aplikacije servisa](../azure-subscription-service-limits.md#app-service-limits).

## <a name="authenticate-your-users"></a>Provjere autentičnosti korisnika

Sada Pogledajmo kako je jednostavno dodati provjere autentičnosti aplikacije (Dodatno čitanje pri [Aplikacije usluge provjere autentičnosti/autorizacije](https://azure.microsoft.com/blog/announcing-app-service-authentication-authorization/)).

1. U plohu portala za aplikacije koje ste upravo otvorili, kliknite **Postavke** > **provjere autentičnosti / autorizacije**.  
    ![Autentičnost - plohu postavke](./media/app-service-web-get-started/aad-login-settings.png)

2. Kliknite **na** da biste uključili provjeru autentičnosti.  

4. **Davatelji usluge provjere autentičnosti**, kliknite **Azure Active Directory**.  
    ![Autentičnost - odaberite Azure AD](./media/app-service-web-get-started/aad-login-config.png)

5. U plohu **Azure Active Directory postavke** kliknite **Express**, a zatim kliknite **u redu**. Zadane postavke stvorite novu aplikaciju Azure AD u zadani direktorij.  
 ![Autentičnost - express konfiguracija](./media/app-service-web-get-started/aad-login-express.png)

6. Kliknite **Spremi**.  
    ![Autentičnost – spremanje konfiguracija](./media/app-service-web-get-started/aad-login-save.png)

    Nakon promjene nije uspjela, prikazat će se obavijest zvono uključivanje zelenoj boji, zajedno s neslužbeni poruke.

7. Vratite se u portala plohu aplikacije kliknite vezu **URL-a** (ili **dođite** na traci izbornika). Veza je HTTP adresa.  
    ![Autentičnost - dođite do URL-a](./media/app-service-web-get-started/aad-login-browse-click.png)  
    No kada je aplikacija Otvori na novoj kartici, URL preusmjeravanja nekoliko puta i okvir Završi na aplikaciju s adresom HTTPS. Što se vide da ste već prijavljen je na pretplatu Azure i automatski se provjere autentičnosti u aplikaciji.  
    ![Autentičnost - prijavljeni](./media/app-service-web-get-started/aad-login-browse-http-postclick.png)  
    Da ako odmah otvorili sesiju Neprovjereni u neki drugi preglednik, vidjet ćete zaslon za prijavu kada se vratite na istoj URL.  
    <!-- ![Authenticate - login page](./media/app-service-web-get-started/aad-login-browse.png)  -->
   Ako ste nikad gotovo bilo čega što sadrži Azure Active Directory, zadani direktorij možda neće imati svi korisnici Azure AD. U tom slučaju vjerojatno jedini račun u njemu je Microsoftov račun s pretplatom Azure. Zbog automatski ste prijavljeni aplikaciju u istom pregledniku neke starije verzije.
   Možete koristiti isti Microsoftov račun da biste se prijavili nje prijava.

Čestitamo, sve promet su provjere autentičnosti na web-aplikaciju.

Možda ste primijetili u na **provjere autentičnosti / autorizacije** plohu koje možete obaviti puno više, kao što su:

- Omogućivanje društvenih prijava
- Omogućivanje više mogućnosti za prijavu
- Promjena zadano ponašanje prilikom osoba najprije otvorite aplikaciju

Aplikacije servisa omogućuje uključivanje ključ rješenja za neke od uobičajenih provjere autentičnosti mora tako da ne morate unijeti logiku provjere autentičnosti.
Dodatne informacije potražite u članku [Aplikacije usluge provjere autentičnosti/autorizacije](https://azure.microsoft.com/blog/announcing-app-service-authentication-authorization/).

## <a name="scale-your-app-automatically-based-on-demand"></a>Promjena veličine aplikacije automatski koji se temelji na zahtjev

Sljedeći, recimo automatsko skaliranje aplikacije tako da ga će automatski prilagoditi ga kapaciteta odgovoriti na zahtjev korisnika (za daljnje čitanje na [Skaliranje gore aplikacije u Azure](web-sites-scale.md) i [skalirali broj instanci automatski ili ručno](../monitoring-and-diagnostics/insights-how-to-scale.md)).

Ukratko, Promijeni web-aplikaciju programa na dva načina:

- [Promjena veličine](https://en.wikipedia.org/wiki/Scalability#Horizontal_and_vertical_scaling): dobiti više procesora, memorije, prostora na disku i dodatne značajke kao što su namjenski VMs, prilagođene domene i certifikata, pripremna slobodnih, autoscaling i više. Skaliranje promjenom cijene sloj aplikacije servisa za planiranje pripada aplikacije.
- [Skaliranje odgovor](https://en.wikipedia.org/wiki/Scalability#Horizontal_and_vertical_scaling): povećanje broja VM instance koji se izvode aplikacije.
Odgovor na koliko god 50 instance možete se skaliranje ovisno o vašem cijene sloju.

Bez dodatno ado postavimo autoscaling.

1. Najprije ćemo proširenjem omogućiti autoscaling. U portala plohu aplikacije kliknite **Postavke** > **Mjerilo prema gore (aplikaciju servisa tarifa)**.  
    ![Proširenja - plohu postavke](./media/app-service-web-get-started/scale-up-settings.png)

2. Pomaknite se, a zatim odaberite **S1 standardne** sloju najniže sloju koji podržava autoscaling (zaokruženog snimka), a zatim kliknite **Odaberi**.  
    ![Promjena veličine – odaberite sloj](./media/app-service-web-get-started/scale-up-select.png)

    Završite skaliranja prema gore.

    >[AZURE.IMPORTANT] U ovom sloju expends vaše besplatnu probnu kredita. Ako imate račun za plaćanje po korištenju uključuje naknade za vaš račun.

3. Zatim ćemo konfiguriranje autoscaling. U portala plohu aplikacije kliknite **Postavke** > **Mjerilo Out (aplikaciju servisa tarifa)**.  
    ![Skaliranje out - plohu postavke](./media/app-service-web-get-started/scale-out-settings.png)

4. Promjena **skale po** **Procesora**postotak. Klizači ispod na padajućem popisu ažurirajte sukladno tome. Nakon toga definirali raspon **instance** između **1** i **2** i **Odredišni raspon** između **40** i **80**. To učiniti tako da upišete u okvire ili pomicanjem klizača.  
 ![Vremensko mjerilo - konfiguriranje autoscaling](./media/app-service-web-get-started/scale-out-configure.png)

    Na temelju ovoj konfiguraciji aplikacije automatski mijenja veličinu izgleda kada procesora veća od 80%, a mijenja veličinu u kada je procesora ispod 40%.

5. Na traci izbornika kliknite **Spremi** .

Čestitamo, autoscaling je aplikacija.

Možda ste primijetili u plohu **Postavke mjerila** koje možete obaviti puno više, kao što su:

- Ručna promjena veličine na određeni broj instanci
- Promjena veličine tako da druge metrika performanse, kao što su reda čekanja postotak ili disk memorije
- Prilagodba ponašanja skaliranja kada se pokrene pravilo performansi
- Automatsko skaliranje na raspored
- Postavljanje ponašanja autoscaling za buduće događaj

Dodatne informacije o skaliranje gore aplikacije, potražite u članku [proširenja aplikacije u Azure](../app-service-web/web-sites-scale.md). Dodatne informacije o skaliranje izgleda, potražite u članku [skalirali broj instanci automatski ili ručno](../monitoring-and-diagnostics/insights-how-to-scale.md).

## <a name="receive-alerts-for-your-app"></a>Primanje upozorenja za aplikaciju

Sad kad je aplikacija autoscaling, što se događa kada dođe do maksimalno instancu count (2) i procesora je iznad željeni Upotreba (80%)?
Da biste obavještava o tom slučaju tako da možete dodatno promijenite li gore/iz aplikacije, na primjer možete postaviti upozorenja (Dodatno čitanje pri [primanje upozorenja](../monitoring-and-diagnostics/insights-receive-alert-notifications.md)). Brzo postavimo upozorenje za taj scenarij.

1. Na portalu plohu aplikaciju, kliknite **Alati** > **upozorenja**.  
    ![Upozorenja - plohu postavke](./media/app-service-web-get-started/alert-settings.png)

2. Kliknite **Dodaj upozorenje**. Zatim u okviru **resursa** odaberite resurs koji završava s **(serverfarms)**. To je tarifa aplikacije servisa.  
    ![Upozorenja – dodavanje upozorenja za aplikacije servisa za planiranje](./media/app-service-web-get-started/alert-add.png)

3. Navedite **naziv** kao `CPU Maxed`, **metrika** kao **Postotak procesora**i **prag** kao `90`pa odaberite **vlasnici e-pošte, suradnika, i čitači**, a zatim kliknite **u redu**.   
 ![Upozorenja - Konfiguriranje upozorenja](./media/app-service-web-get-started/alert-configure.png)

    Kada se Azure dovrši stvaranje upozorenja, koje se prikazuju u plohu **upozorenja** .  
    ![Upozorenja – prikaz dovršeno](./media/app-service-web-get-started/alert-done.png)

Čestitamo, sada primate upozorenja.

Tu postavku upozorenja provjerava procesora svakih pet minuta. Ako taj broj dolazi iznad 90%, primit ćete poruku upozorenja e-pošte, zajedno sa svima koji je dozvolu. Da biste vidjeli svima koji je ovlašten primati upozorenja, vratite se u portala plohu aplikacije, a zatim kliknite gumb **programa Access** .  
![U odjeljku tko može vidjeti upozorenja](./media/app-service-web-get-started/alert-rbac.png)

Trebali biste vidjeti **pretplate administratori** jesu li već **vlasnik** aplikaciju. Ovu grupu će uključiti ako ste administrator računa Azure pretplate (npr. probnu pretplatu). Dodatne informacije o kontroli Azure pristupa na temelju uloga potražite u članku [Upravljanje pristupom Azure Role-Based](../active-directory/role-based-access-control-configure.md).

> [AZURE.NOTE] Upozorenja pravila je značajka Azure. Dodatne informacije potražite u članku [primanje upozorenja](../monitoring-and-diagnostics/insights-receive-alert-notifications.md).

## <a name="next-steps"></a>Daljnji koraci

Na način da biste konfigurirali upozorenje, možda ste primijetili bogatog skupa alata u plohu **Alati** . Ovdje, otklonite probleme, monitor performanse, testiranje za slabe točke, Upravljanje resursima, interakciju s konzole VM i dodavanje korisne extensions. Pozivamo vas da kliknite svaki od tih alata da biste otkrili jednostavni, ali Napredni alati pri Savjeti za prsta.

Saznajte kako dodatnu s aplikacijom distribuiranih. Evo samo djelomičnim popisom:

- [Kupite i konfiguriranje prilagođenog naziva domene](custom-dns-web-site-buydomains-web-app.md) – kupite privlačne domenu za web-aplikacije umjesto u *. azurewebsites.net domene. Ili koristiti domenu koju već imate.
- [Postavljanje okruženja pripremna](web-sites-staged-publishing.md) - implementacija aplikacije pripremna URL prije stavljanja u radnog. Ažurirajte uživo web-aplikaciju programa pouzdano. Postavljanje složene DevOps rješenja s više slobodnih implementacije.
- [Postavljanje neprekinuti implementacije](app-service-continuous-deployment.md) - integraciji implementaciju aplikacije u kontrolu izvorišnog sustava. Implementacija Azure s svaki potvrdi.
- [Pristup lokalnog resursa](web-sites-hybrid-connection-get-started.md) – pristup postojeći lokalne baze podataka ili sustavu CRM.
- [Sigurnosno kopiranje aplikacije](web-sites-backup.md) – postavljanje natrag gore i vraćanje za web-aplikacije. Priprema za neočekivane pogreške i oporavak od njih.
- [Omogućivanje dijagnostičke zapisnike](web-sites-enable-diagnostic-log.md) - pročitati zapisnike IIS iz kašnjenja Azure ili aplikacije. Čitate strujanje, preuzmite ih ili ih priključak u [Uvide aplikacija](../application-insights/app-insights-overview.md) za analizu Uključi ključ.
- [Skeniranje aplikacije za slabe točke](https://azure.microsoft.com/blog/web-vulnerability-scanning-for-azure-app-service-powered-by-tinfoil-security/) -
skeniranje web aplikacije od Moderna prijetnji putem servisa za koje ste dobili od [Tinfoil sigurnost](https://www.tinfoilsecurity.com/).
- [Pokretanje pozadinske zadatke](../azure-functions/functions-overview.md) – Pokreni zadatke za obradu podataka, izvješćivanje, itd.
- [Saznajte kako funkcionira aplikacije servisa](../app-service/app-service-how-works-readme.md)
