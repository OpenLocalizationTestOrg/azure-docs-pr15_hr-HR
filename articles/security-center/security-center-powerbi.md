<properties
   pageTitle="Dohvaćanje uvida iz Azure centar za sigurnost podataka pomoću dodatka Power BI | Microsoft Azure"
   description="Paket sadržaja Azure sigurnost centar za Power BI olakšava pronalaženje sigurnosnih upozorenja, preporuke, napada resurse i trendova, skup podataka stvorene za vaš izvješća na temelju."
   services="security-center"
   documentationCenter="na"
   authors="YuriDio"
   manager="swadhwa"
   editor=""/>

<tags
   ms.service="security-center"
   ms.devlang="na"
   ms.topic="hero-article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="09/22/2016"
   ms.author="yurid"/>

# <a name="get-insights-from-azure-security-center-data-with-power-bi"></a>Dohvaćanje uvida iz Azure centar za sigurnost podataka pomoću dodatka Power BI
[Nadzorna ploča za Power BI](http://aka.ms/azure-security-center-power-bi) za centar za sigurnost Azure omogućuje vizualni prikaz, analizirati i filtrirati preporuke i sigurnosna upozorenja s bilo kojeg mjesta, uključujući mobilnog uređaja. Otkrivanje trendova i napasti uzoraka – prikaz sigurnosnih upozorenja resursa ili IP adresu izvora i unaddressed sigurnosni rizik resursa ili dob pomoću nadzorne ploče za Power BI. 

Koje možete i kombinirali preporuke centar za sigurnost i sigurnosna upozorenja drugim podacima u zanimljivih načina, primjerice pomoću podataka iz [Zapisnika nadzora Azure](https://powerbi.microsoft.com/blog/monitor-azure-audit-logs-with-power-bi/) i [Nadzor baze podataka SQL Azure](https://powerbi.microsoft.com/blog/monitor-your-azure-sql-database-auditing-activity-with-power-bi/). Nude nadzorne ploče programa Power BI pa ove podatke možete izvesti u Excel za jednostavno izvješća na stanja sigurnosti resursa u oblaku.

##<a name="using-azure-security-center-dashboard-to-access-power-bi"></a>Pristup Power BI putem centra za sigurnost Azure nadzorne ploče
Centar za sigurnost Azure nadzornu ploču možete koristiti i za pristup izvješćima servisa Power BI. Slijedite korake da biste izvršili taj zadatak: 

1. Na nadzornoj ploči **Azure centar za sigurnost** , kliknite gumb za **Istraživanje u dodatku Power BI** .

    ![Povezivanje s centar za sigurnost Azure pomoću dodatka Power BI](./media/security-center-powerbi/security-center-powerbi-fig1-new10.png) 

2. Plohu **Istraživanje u dodatku Power BI** otvorit će se na desnoj strani kao što je prikazano na sljedećem zaslonu:

    ![Povezivanje s centar za sigurnost Azure pomoću dodatka Power BI](./media/security-center-powerbi/security-center-powerbi-fig1-new2.png)

3. Ako stvarate nadzorne ploče za Power BI, možete odabrati neku od sljedećih mogućnosti u plohu **Istraživanje u dodatku Power BI** : 

    - **Nadzorna ploča za sigurnost uvida**: tu mogućnost odaberite ako želite stvoriti nadzorne ploče koja sadrži sigurnosnog stanja, niti i dlp-a. Ta je mogućnost više zajednički DevOps uloge koji je zadužen za analizu status zaštite i provjeravati upozorenja putem pretplate.
    - **Nadzorna ploča za upravljanje pravilima**: tu mogućnost odaberite ako želite pregledati upravljanje i provođenje pravila.  Ta je mogućnost više zajednički za središnju IT koji se više pažnje upravljanja. Da bi se dobio vidljivost i uvida na sigurnosna pravila adherence svim svoje tvrtke ili ustanove mogu koristiti ovu nadzornu ploču.
    - Ako već imate nadzorne ploče komponente Power BI, kliknite **Idi na nadzornu ploču trenutni Power BI**.

4. U ovom primjeru kliknite mogućnost **nadzorne ploče uvida sigurnost** . Ako je ovo prvi put, stvarate nadzorne ploče komponente Power BI za centar za sigurnost se od vas zatraži da biste instalirali paket za sadržaj. Kliknite gumb **Dohvati** u prozoru **sadržaja paketi za Power BI** kao što je prikazano na sljedećem zaslonu:

    ![Azure sigurnost centar za sigurnost uvida nadzorne ploče](./media/security-center-powerbi/security-center-powerbi-fig1-new3.png)

5. Prikazuju se **Poveži Azure sigurnost centar za sigurnost uvid u** prozoru. Provjerite jesu li načina provjere **autentičnosti** **oAuth2** kao što je prikazano u nastavku i kliknite **Prijava** .
    
    ![Provjera autentičnosti](./media/security-center-powerbi/security-center-powerbi-fig1-new4.png)

6. Možda ćete morati uspješnoj Azure vjerodajnice. Nakon provjere autentičnosti nadzornu ploču stvorit će se. Nakon stvaranja nadzorne ploče potražite izvješće s strukture slično kao što je prikazano na sljedećem zaslonu:

    ![Nadzorna ploča za Power BI](./media/security-center-powerbi/security-center-powerbi-fig1-new5.png)


> [AZURE.NOTE] Zakazanog osvježavanja izvješća odvija po danu. U slučaju da se pojave pogreške na osvježavanje, pročitajte [Potencijalne probleme osvježavanje pomoću dodatka Power BI centar za sigurnost Azure](https://blogs.msdn.microsoft.com/azuresecurity/2016/04/07/azure-security-center-power-bi-refresh-fails/), dodatne informacije o otklanjanju poteškoća s.

Ovdje možete vidjeti broj sigurnosnih upozorenja i preporuke, kao i broj VMs, baze podataka Azure SQL i mrežni resursi koji se nadzire Centar za sigurnost Azure.

Veza na centar za sigurnost Azure preusmjerava Azure portal. Grafikoni olakšavaju Vizualizirajte podatke o preporuke o sigurnosti i upozorenja, uključujući:

- Stanje sigurnosti resursa
- Na čekanju preporuke
- Preporuke VM
- Upozorenja tijekom vremena
- Napada resursi
- Napada IP-ovi

Iza svakom grafikonu postoje dodatni uvid. Odaberite pločicu da biste vidjeli dodatne informacije. Ako, na primjer, pločicu **Resursa sigurnosnog stanja** prikazana dodatne detalje o čekanju preporuke resursima kao što je prikazano na sljedećem zaslonu:

![Preporuke](./media/security-center-powerbi/security-center-powerbi-fig1-new6.png)

Ako kliknete na bilo koji redak u ovom grafikonu, na druge ćete siva odgovor, a koje fokus samo na one koju ste odabrali. Da biste se vratili na nadzornu ploču, kliknite **Centar za sigurnost Azure** u odjeljku mogućnosti **nadzornih ploča** u lijevom oknu ove stranice.

> [AZURE.NOTE] Ako želite da biste prilagodili izvješća dodatna polja dodavanjem ili promjenom postojeće vizualne elemente, možete uređivati izvješće. Dodatne informacije pročitajte [Jezici predloška interakcije s izvješćem u prikazu za uređivanje u dodatku Power BI](https://powerbi.microsoft.com/documentation/powerbi-service-interact-with-a-report-in-editing-view/) .

Pločice **upozorenja s vremenom napada resurse** i **Napadač IP -ovi** će imati slično izlaz prilikom klika na svaku od. To se događa jer izvješće objedinjuje podatak te tri varijable te ga poziva **resursa u odjeljku napada** kao što je prikazano na sljedećem zaslonu:

![Resursi u odjeljku napada](./media/security-center-powerbi/security-center-powerbi-fig1-new7.png)

Sada možete spremiti kopiju izvješća, ispisati ili objavljivanje na webu pomoću mogućnosti dostupnih na izborniku **datoteka** .

![Izbornik datoteka](./media/security-center-powerbi/security-center-powerbi-fig8.png)

## <a name="exploring-your-azure-security-center-data-with-power-bi-services"></a>Istraživanje podataka centar za sigurnost Azure pomoću servisa Power BI

Povezuju sa [Servisima sadržajem paket za Power BI](https://msit.powerbi.com/groups/me/getdata/services) u dodatku Power BI i izvršiti sljedeće korake:

1. U prozoru **Sadržaja paket za Power BI** vidjet ćete dvije mogućnosti kao što je prikazano u nastavku.

    ![Sadržaja paket za Power BI](./media/security-center-powerbi/security-center-powerbi-fig1-new.png)

    >[AZURE.NOTE] Ako se već na prvi dio ovog članka vidjet ćete samo jednu mogućnost što je upravljanje pravilima centar za sigurnost za Azure.

2. Radi u ovom se primjeru kliknite **Dohvati** na pločici **Upravljanje pravilima centar za sigurnost za Azure** .

3. U prozoru **za povezivanje s Upravljanje pravilima centar za sigurnost za Azure** obavezno odaberite **oAuth2** u odjeljku **Način provjere autentičnosti** padajući kao što je prikazano ispod i kliknite gumb **Prijava** .

    ![Prozor za upravljanje pravilima](./media/security-center-powerbi/security-center-powerbi-fig1-new8.png)

4. Će biti preusmjereni na stranicu s provjere autentičnosti koje morate upisati vjerodajnice koje koristite za povezivanje s Azure centar za sigurnost. Nakon dovršetka postupka provjere autentičnosti, počet će Power BI uvoz podataka za izradu izvješća. Za to vrijeme može se pojaviti sljedeća poruka u desnom kutu preglednika:

    ![Povezivanje s centar za sigurnost Azure pomoću dodatka Power BI](./media/security-center-powerbi/security-center-powerbi-fig4.png)

    >[AZURE.NOTE] Kada prvi put stvoren je na nadzornoj ploči može trajati dulje nego inače uglavnom scenarijima za koje imate više pretplata. 

5. Po dovršetku postupka nadzornu ploču Azure sigurnost centar za Power BI učitava se s izvješćem **Upravljanje pravilima** slično onome prikazano u nastavku:

    ![Nadzorna ploča za upravljanje pravilima](./media/security-center-powerbi/security-center-powerbi-fig1-new9.png)

## <a name="see-also"></a>Vidi također
U ovom dokumentu naučili kako koristiti Power BI u centru za sigurnost Azure. Da biste saznali više o centru za sigurnost Azure, pogledajte sljedeće:

- [Azure sigurnost centar za planiranje i vodič](security-center-planning-and-operations-guide.md) – upute za planiranje prihvaćanja Azure centar za sigurnost.
- [Postavljanje pravila zaštite u centru za sigurnost Azure](security-center-policies.md) – upute za konfiguriranje sigurnosnih postavki u centru za sigurnost Azure
- [Upravljanje i odgovarati na njih sigurnosnih upozorenja u centru za sigurnost Azure](security-center-managing-and-responding-alerts.md) – upute za upravljanje i odgovaranje na sigurnosnih upozorenja
- [Najčešća Pitanja za centar za sigurnost Azure](security-center-faq.md) – traženje najčešća pitanja o korištenju servisa
- [Azure sigurnost Blog](http://blogs.msdn.com/b/azuresecurity/) – pronaći bloga o Azure sigurnost i usklađenost
