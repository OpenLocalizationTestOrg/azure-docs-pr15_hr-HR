<properties
    pageTitle="Upozorenja u stvarnom vremenu Azure CDN | Microsoft Azure"
    description="Upozorenja u programu Microsoft Azure CDN u stvarnom vremenu. Upozorenja u stvarnom vremenu pružaju obavijesti o performansama krajnje točke u svom profilu CDN."
    services="cdn"
    documentationCenter=""
    authors="camsoper"
    manager="erikre"
    editor=""/>

<tags
    ms.service="cdn"
    ms.workload="tbd"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/12/2016"
    ms.author="casoper"/>

# <a name="real-time-alerts-in-microsoft-azure-cdn"></a>Upozorenja u stvarnom vremenu u programu Microsoft Azure CDN

[AZURE.INCLUDE [cdn-premium-feature](../../includes/cdn-premium-feature.md)]


## <a name="overview"></a>Pregled

Ovaj dokument objašnjava upozorenja u stvarnom vremenu u programu Microsoft Azure CDN. Ta je funkcija šalje u stvarnom vremenu obavijesti o performansama krajnje točke u svom profilu CDN.  Možete postaviti e-pošte ili HTTP upozorenja na temelju:

* Propusnosti
* Kodovi stanja
* Statusi predmemorije
* Veze

## <a name="creating-a-real-time-alert"></a>Stvaranje upozorenjem u stvarnom vremenu

1. [Portal za Azure](https://portal.azure.com)pronađite CDN profila.

    ![Plohu CDN profila](./media/cdn-real-time-alerts/cdn-profile-blade.png)

2. Iz profila plohu CDN kliknite gumb **Upravljanje** .

    ![Gumb Upravljanje plohu CDN profila](./media/cdn-real-time-alerts/cdn-manage-btn.png)

    Otvorit će se na portal za upravljanje CDN.

3. Zadržite pokazivač na kartici **analize** , a zatim zadržite pokazivač na Potpaleta **Stat u stvarnom vremenu** .  Kliknite na **upozorenja u stvarnom vremenu**.

    ![Portal za upravljanje CDN](./media/cdn-real-time-alerts/cdn-premium-portal.png)

    Prikazat će se popis postojeće upozorenja konfiguracije (ako ih ima).

4. Kliknite gumb **Dodaj upozorenje** .

    ![Dodavanje gumba za upozorenja](./media/cdn-real-time-alerts/cdn-add-alert.png)

    Prikazat će se obrazac za stvaranje Novo upozorenje.

    ![Novi obrazac za upozorenja](./media/cdn-real-time-alerts/cdn-new-alert.png)

5. Ako želite da se upozorenje da je aktivna kada kliknete **Spremi**, potvrdite okvir za **Upozorenja je omogućena** .

6. U polje **naziv** unesite opisni naziv za svoje upozorenje.

7. Na padajućem popisu **Vrsta** odaberite **HTTP velike objekta**.

    ![Vrsta medija s HTTP velike objektom](./media/cdn-real-time-alerts/cdn-http-large.png)

    > [AZURE.IMPORTANT] Morate odabrati **HTTP velike objekt** kao **Vrsta medija**.  **Azure CDN iz Verizon**ne koristi druge opcije.  Odaberite **HTTP velike objekt** nije uspjelo uzrokovat će vaše upozorenje da biste nikad se pokrenuti.

8. Stvaranje **izraza** praćenje tako da odaberete **metrika**, **Operator**i **vrijednost okidača**.

    - Za **mjerenje**odaberite vrstu uvjet koji nadziranim.  **MB/s propusnosti** je iznos korištenja propusnosti u megabita u sekundi.  **Ukupna veze** je broj Istodobni HTTP veza za naše edge poslužitelji.  Definicija razni statusi predmemorije i šifre statusa potražite u članku [Azure CDN predmemorije Status šifre](https://msdn.microsoft.com/library/mt759237.aspx) i [Azure CDN HTTP statusa](https://msdn.microsoft.com/library/mt759238.aspx)
    - **Operator** je matematički operator koji uspostavlja odnos između metriku i vrijednost okidača.
    - **Pokretanje** je vrijednost praga kojima se mora podudarati prije nego što će se slati obavijest.

    U u sljedećem primjeru izraz koje ste stvorili označava da želim primati obavijesti kada je broj 404 kodovi stanja veće od 25.

    ![U stvarnom vremenu upozorenja Primjeri izraza](./media/cdn-real-time-alerts/cdn-expression.png)

9. **Interval**, unesite koliko se često biste željeli expression vrednovan.

10. Na padajućem popisu **obavijesti na** odaberite kada želite biti obaviješteni kad je izraz true.
    
    - **Pokretanje uvjet** označava da će moguće poslati obavijest čim se otkriju navedenom uvjetu.
    - **Uvjet završetka** označava da obavijest poslat će se kada više ne otkrije navedenom uvjetu. Ta se obavijest možete se tek nakon naš sustava za nadzor mreže otkriven pojavila navedenom uvjetu.
    - **Neprekinuto** označava da obavijest poslat će se svaki put kada prepoznaje li sustav za nadzor mreže navedenom uvjetu. Imajte na umu da nadzor sustava mreže će se samo jednom provjeriti po interval za određeni uvjet.
    - **Uvjet početak i kraj** označava da obavijest poslat će se prvi put da je otkriven navedenom uvjetu i opet kada uvjet se više ne otkrije.

11. Ako želite primati obavijesti putem e-pošte, potvrdite okvir **obavijesti putem e-pošte** .  

    ![Obavijestite po obrascu e-pošte](./media/cdn-real-time-alerts/cdn-notify-email.png)
    
    U polju **Prima** unesite adresu e-pošte na mjesto na kojem želite da se obavijesti šaljete. **Predmet** i **tijelo**možete ostaviti zadani ili možete prilagoditi poruku pomoću popisa **dostupni ključne riječi** dinamički umetanja upozorenja podataka prilikom slanja poruke.

    > [AZURE.NOTE] Možete testirati i obavijesti e-pošte tako da kliknete gumb za **Testiranje obavijesti** , ali samo nakon spremanja upozorenja konfiguracije.

12. Ako želite da se obavijesti će se objaviti na web-poslužitelju, potvrdite okvir **obavijesti putem HTTP Post** .

    ![Obavijesti putem HTTP Post obrasca](./media/cdn-real-time-alerts/cdn-notify-http.png)

    U polje **Url** unesite URL koji ste mjesto na kojem želite da se poruke HTTP objavili. U tekstni okvir **zaglavlja** unesite HTTP zaglavlja slati zahtjev.  Za **tijelo** mogu prilagoditi poruku pomoću popisa **dostupni ključne riječi** dinamički umetanja upozorenja podataka prilikom slanja poruke.  **Zaglavlja** i **tijela** zadane postavke i XML tereta slično kao u sljedećem primjeru.

    ```
    <string xmlns="http://schemas.microsoft.com/2003/10/Serialization/">
        <![CDATA[Expression=Status Code : 404 per second > 25&Metric=Status Code : 404 per second&CurrentValue=[CurrentValue]&NotificationCondition=Condition Start]]>
    </string>
    ```

    > [AZURE.NOTE] Možete testirati i obavijesti HTTP Post klikom na gumb za **Testiranje obavijesti** , ali samo nakon spremanja upozorenja konfiguracije.

13. Kliknite gumb **Spremi** da biste spremili konfiguraciju za upozorenje.  Ako je potvrđen **Upozorenje omogućen** u koraku 5, na upozorenju je sada aktivan.

## <a name="next-steps"></a>Daljnji koraci

- Analiza [u stvarnom vremenu stat u Azure CDN](cdn-real-time-stats.md)
- Pristupili [Dodatna izvješća HTTP](cdn-advanced-http-reports.md)
- Analiza [uzoraka korištenja](cdn-analyze-usage-patterns.md)

