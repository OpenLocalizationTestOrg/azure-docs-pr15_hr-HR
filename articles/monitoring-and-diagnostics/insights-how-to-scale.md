<properties
    pageTitle="Skalirali broj instanci automatski ili ručno | Microsoft Azure"
    description="Upute za promjenu veličine servisa Azure."
    authors="rboucher"
    manager="carolz"
    editor=""
    services="monitoring-and-diagnostics"
    documentationCenter="monitoring-and-diagnostics"/>

<tags
    ms.service="monitoring-and-diagnostics"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/08/2015"
    ms.author="robb"/>

# <a name="scale-instance-count-manually-or-automatically"></a>Promjena veličine broj instanci automatski ili ručno

[Portal za Azure](https://portal.azure.com/)možete postaviti i ručno count instanca servisa ili možete postaviti parametara da biste ga automatski skaliranje koji se temelji na zahtjev. To je obično se nazivaju *Skaliranje izgleda* ili *Promjena veličine u*.

Prije nego što skaliranje na temelju instance broj, razmislite o da skaliranje utječe **sloju određivanje cijena** uz broj instanci. Različite razine cijena može imati jezgri različite brojeve i memorije, a pa će imati bolje performanse za isti broj instanci (što je *Skaliranje prema dolje*ili *Skaliranje gore* ). U ovom se članku posebno pokriva *Skaliranje u* ili *odlazi*.

Mogu mijenjati veličinu na portalu, a možete koristiti i [REST API -JA](https://msdn.microsoft.com/library/azure/dn931953.aspx) ili [.NET SDK](https://www.nuget.org/packages/Microsoft.Azure.Insights/) da biste prilagodili skaliranje automatski ili ručno.

> [AZURE.NOTE] U ovom se članku opisuje kako stvoriti je postavka automatsko skaliranje na portalu na [http://portal.azure.com](http://portal.azure.com). Automatsko skaliranje postavke stvorene u ovaj portal ne može uređivati klasični portal ([http://manage.windowsazure.com](http://manage.windowsazure.com)).

## <a name="scaling-manually"></a>Promjena veličine ručno

1. [Portal za Azure](https://portal.azure.com/)kliknite **Pregledaj**, a zatim otvorite resurs koji želite da biste skalirali, kao što su **aplikacije servisa za planiranje**.

2. **Skaliranje** servisu **operacije** će vas obavještava status Skaliranje: **isključivanje** kada vam se skaliranja ručno, **na** kada su skaliranja po jednu ili više metriku performanse.
    ![Promjena veličine pločica](./media/insights-how-to-scale/Insights_UsageLens.png)

3. Klikom na pločici će vas odvesti plohu **mjerilo** . Pri vrhu plohu skaliranje možete vidjeti povijest akcija za automatsko skaliranje servis.  
    ![Promjena veličine plohu](./media/insights-how-to-scale/Insights_ScaleBladeDayZero.png)

>[AZURE.NOTE] Samo radnje koje su automatsko skaliranje prikazat će se na ovom grafikonu. Ako ručno prilagodite broj instanci, promjena se ne odražava na ovom grafikonu.

4. Možete ručno prilagoditi broj **instanci** pomoću klizača.
5. Kliknite naredbu **Spremi** , a koje će biti skalirana za taj broj instanci odmah.

## <a name="scaling-based-on-a-pre-set-metric"></a>Promjena veličine na temelju unaprijed postavljenih mjerenja

Ako želite da broj instanci da biste automatski prilagodili na temelju metrike, na **Skaliranje po** padajućem popisu odaberite metriku koju želite. Ako, na primjer, **aplikacije servisa za planiranje** možete skaliranje **Procesora**postotak.

1. Kad odaberete metrike dobit ćete klizač i/ili, tekstne okvire da biste unijeli broj instanci želite skaliranje između:

    ![Promjena veličine plohu s postotkom CPU-a](./media/insights-how-to-scale/Insights_ScaleBladeCPU.png)

    Automatsko skaliranje nikad se servis ispod ili iznad granice koji postavljate, bez obzira na to vaše Učitaj.

2. Drugo, odaberite odredišni raspon za metriku. Ako, na primjer, ako ste odabrali **postotak procesora**, možete postaviti cilj za average procesora kroz sve instance na servisu. Skaliranje out će se dogoditi kada average procesora premašuje definirate, isto tako, skalu u dogodit će se kad god prosjek procesora ispod minimum.

3. Kliknite naredbu **Spremi** . Automatsko skaliranje će provjeriti svakih nekoliko minuta da biste bili sigurni da su u rasponu instancu i ciljne vaše metrike. Kada na servisu primi dodatni promet, dobit ćete više instanci bez ikakve.

## <a name="scale-based-on-other-metrics"></a>Promjena veličine koji se temelji na drugim mjerenja

Ovisno o metriku osim postojeće postavke koje se prikazuju na **Skaliranje po** padajućem popisu, a možete čak i složene skup skaliranje izvan te promijenite veličinu u pravila mogu mijenjati veličinu.

### <a name="adding-or-changing-a-rule"></a>Dodavanje ili promjena pravila

1. Odaberite **Raspored i performanse pravila** na **Skaliranje po** padajućem popisu: ![performanse pravila](./media/insights-how-to-scale/Insights_PerformanceRules.png)

2. Ako ste prethodno automatsko skaliranje, na vidjet ćete prikaz točno pravila koja ste.

3. Za promjenu veličine na temelju drugog metričkim kliknite redak **Dodaj pravilo** . Možete kliknuti i jedan ili više postojećih redaka da biste promijenili iz metriku prethodno imali metriku koju želite za promjenu veličine tako da.
![Dodavanje pravila](./media/insights-how-to-scale/Insights_AddRule.png)

4. Sada ćete morati odabrati koje metriku želite skaliranje po. Prilikom odabira metrike postoji nekoliko stvari koje treba uzeti u obzir:
    * *Resurs* metriku dolazi iz grupe. Obično, to će biti jednaki resursa su skaliranja. Međutim, ako želite da biste skalirali po dubine reda čekanja za pohranu, resurs je reda koju želite za promjenu veličine tako da.
    * *Metričkim naziv* na samom.
    * U *vrijeme prikupljanja* mjerenja. To je način na koji se podaci nalaze kombiniranje tijekom *trajanja*.

5. Nakon odabira vaše metriku odaberite praga za mjerenje i operator. Na primjer, nije moguće recimo **veće od** **80%**.

6. Zatim odaberite akciju koju želite preuzeti. Postoji nekoliko različitih vrsta akcije:
    * Povećavanje ili smanjivanje – to će dodati ili ukloniti **vrijednost** broj instanci koje ste definirali
    * Povećajte ili smanjite percent – to promijenit će se broj instanci postotak. Ako, na primjer, 25 možete umetnuti u polju **vrijednosti** , a ako trenutno imate 8 instance, želite dodati 2.
    * Povećavanje ili smanjivanje – Time ćete postaviti broj instanci **vrijednosti** koje ste definirali.

7. Na kraju, možete odabrati najbolju dolje - koliko treba čekati ovo pravilo nakon prethodne radnje mjerilo da biste skalirali ponovno.

8. Nakon konfiguriranja pravila kliknite **u redu**.

9. Nakon što ste konfigurirali svih pravila koje želite, obavezno kliknite naredbu **Spremi** .

### <a name="scaling-with-multiple-steps"></a>Skaliranje s više koraka

Vrlo osnovni su iznad primjere. Međutim, ako želite da budu redovno o skaliranje gore (ili dolje), čak i možete dodati više pravila mjerilo za istu metriku. Na primjer, možete definirati dva pravila skaliranje na postotak procesora:

1. Promjena veličine izgleda tako da 1 instancu ako je postotak procesora iznad 60%
2. Promjena veličine izgleda tako da 3 instance ako je postotak procesora iznad 85%

![Više pravila skala](./media/insights-how-to-scale/Insights_MultipleScaleRules.png)

S dodatna pravila ako vaš učitavanja premašuje 85% prije skaliranje akciju, dobit ćete dvije dodatne instance umjesto jednog.

## <a name="scale-based-on-a-schedule"></a>Skaliranje na temelju rasporeda


Prema zadanim postavkama, kada stvorite pravilo skala to uvijek se primijeniti. Koji možete vidjeti kada kliknete zaglavlje profila:

![Profil](./media/insights-how-to-scale/Insights_Profile.png)

Međutim, preporučujemo vam da bi redovno skaliranje tijekom dana ili tjedana, nego na vikenda. Čak i nije moguće isključiti na servisu potpuno isključivanje radno vrijeme.

1. Da biste to učinili, na profilu imate, odaberite **Ponavljanje** umjesto da **uvijek,** a zatim vremena koje želite da se profil da biste primijenili.

2. Ako, na primjer, da bi se profila koji se primjenjuje tijekom tjedna, na padajućem popisu **dana** poništite **subota** i **nedjelja**.

3. Da bi se profila koji se primjenjuje tijekom na daytime, postavite **vrijeme početka** na vrijeme koje želite pokrenuti.
    ![Zadani ponavljanja](./media/insights-how-to-scale/Insights_ProfileRecurrence.png)

4. Kliknite **u redu**.

5. Nakon toga morate dodati profil koji želite primijeniti ponekad. Kliknite redak za **Dodavanje profila** .
    ![Izvan ureda](./media/insights-how-to-scale/Insights_ProfileOffWork.png)

6. Novi, drugi, profila, primjerice je nije moguće nazovite **izvan ureda**.

7. Zatim ponovno odaberite **ponavljanja** , a zatim odaberite željeni instance broj raspon tijekom određenog razdoblja.

8. Kao što je s zadani profil odaberite **dana** želite ovaj profil da biste primijenili i **vrijeme početka** tijekom dana.

>[AZURE.NOTE] Automatsko skaliranje će koristiti pravila ljetnog i zimskog bez obzira **vremensku zonu** odaberete. Međutim, tijekom zimskog ušteda vremena UTC pomak prikazat će offset osnovne vremenske zone, ne zimskog računanje UTC pomak.

9. Kliknite **u redu**.

10. Sada, morat ćete dodati pravila za bilo koju želite primijeniti tijekom drugog profila. Kliknite **Dodaj pravilo**, a zatim nije sastavljanje isto pravilo imate tijekom zadani profil.
    ![Dodavanje pravila na isključeno rad](./media/insights-how-to-scale/Insights_RuleOffWork.png)

11. Svakako stvorite oba pravilo za skaliranje izgleda i skaliranje u or else tijekom profila broj instanci samo će rasti (ili Smanji).

12. Na kraju, kliknite **Spremi**.

## <a name="next-steps"></a>Daljnji koraci

* [Metriku monitor servisa](insights-how-to-customize-monitoring.md) da biste bili sigurni na servisu dostupan je i odredište.
* [Omogućite praćenje i dijagnostici](insights-how-to-use-diagnostics.md) da biste prikupili detaljne velika učestalost metriku o vašoj usluzi.
* [Primanje upozorenja](insights-receive-alert-notifications.md) kad god radu događajima ili metriku Unakrsna praga.
* [Performanse aplikacije monitor](../application-insights/app-insights-azure-web-apps.md) ako želite da biste shvatili točno onako kako kod izvršava u oblaku.
* [Pregled događaja i zapisnika nadzora](insights-debugging-with-events.md) da biste saznali sve koji dogodilo na servisu.
* [Dostupnost monitora i odziv web-stranicu](../application-insights/app-insights-monitor-web-app-availability.md) aplikacije uvida tako da možete saznati ako stranici nije dostupno.
