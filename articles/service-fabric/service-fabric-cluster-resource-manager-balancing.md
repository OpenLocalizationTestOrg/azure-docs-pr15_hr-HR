<properties
   pageTitle="Klaster s Azure tkanina klaster resursa Upravitelj servisa za ujednačavanje | Microsoft Azure"
   description="Uvod u opterećenja klaster s tkanina klaster resursa Upravitelj servisa."
   services="service-fabric"
   documentationCenter=".net"
   authors="masnider"
   manager="timlt"
   editor=""/>

<tags
   ms.service="Service-Fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="08/19/2016"
   ms.author="masnider"/>

# <a name="balancing-your-service-fabric-cluster"></a>Svoj klaster tkanina servisa za ujednačavanje
Voditelj resursa za servis tkanina klaster omogućuje izvješćivanja dinamički opterećenja, reagira promjene u klasteru, ispravljanja kršenja ograničenja i rebalancing skupine prema potrebi. No učestalosti čini li sljedeće i što ga je pokrenula? Postoji nekoliko kontrola vezane uz to.

Prvi skup kontrola oko opterećenja su skup trajanju. Ove trajanju upravljaju učestalosti Voditelj resursa klaster istražuje stanje klaster za stvari koje morate biti adresirane. Postoje tri različite kategorije zadataka, svaka ima vlastite odgovarajuće mjerača vremena. Mogućnosti su sljedeće:

1.  Položaj – ovoj fazi bavi smještanje s praćenjem stanja replike ni bez praćenja stanja instanci koje nedostaju. To obuhvaća novih servisa i rukovanja s praćenjem stanja replike ili bez praćenja stanja instanci koji nije uspjela i morati ponovno stvoriti. Brisanje i ispuštanje replike ili instance i obrađuje ovdje.
2.  Ograničenja provjere – ovoj fazi provjerava i ispravlja kršenja ograničenja različite položaj (pravila) u sustavu. Primjeri pravila su elemente kao što su zajamčiti da čvorove nisu preko kapaciteta i ograničenjima položaj na servis (Dodatne informacije o te kasnije) zadovoljeni.
3.  Za ujednačavanje – ovoj fazi provjerava je li određene proaktivne rebalancing nije potrebno koji se temelji na željenu konfiguriran razinu saldo za različite metriku i u tom slučaju pokušava pronaći raspored u skupini koje je još raspoređen.

## <a name="configuring-cluster-resource-manager-steps-and-timers"></a>Konfiguriranje upravljanja resursima klaster korake i trajanju
Svaka od tih različitih vrsta ispravaka Voditelj resursa klaster možete napraviti upravlja različite timer koji određuje njegov učestalost. Tako, primjerice, ako želite samo radnju za smještanje nove radnih opterećenja servisa u klasteru svaki sat (za skupnu ih gore), ali želite običnog opterećenja provjerava svakih nekoliko sekundi, možete konfigurirati to ponašanje. Kada se aktivira se svaki mjerača vremena, koje je zadatak zakazan. Prema zadanim postavkama Voditelj resursa pregledava stanje i primjenjuje ažuriranja (Grupno slanje promjena sve promjene koje su se pojavile od posljednjeg pregleda, kao što su pazeći je čvor prema dolje) svaku 1/10th druge skupove položaj i ograničenja provjerite zastavice sekundi i trebali zastavica svakih 5 sekundi.

ClusterManifest.xml:

``` xml
        <Section Name="PlacementAndLoadBalancing">
            <Parameter Name="PLBRefreshGap" Value="0.1" />
            <Parameter Name="MinPlacementInterval" Value="1.0" />
            <Parameter Name="MinConstraintCheckInterval" Value="1.0" />
            <Parameter Name="MinLoadBalancingInterval" Value="5.0" />
        </Section>
```

Danas mi izvršiti samo jednu od ovih radnji odjednom, određenim redoslijedom (zbog objedinjuje ove konfiguracije kao "minimalne intervalima")). To je tako da, na primjer, ne možemo ste već odgovorili na bilo kojem na čekanju zahtjeve za stvaranje nove replike prije nego što smo prijeći na opterećenja klaster. Kao što je vidljivo po zadanom vremenska razdoblja naveden, ne možemo možete skenirati i ništa moramo vrlo je često koristite, što znači da skup promjena dajemo na kraju svakog koraka provjerite manja obično: smo si pregleda putem sati promjene u klasteru i pokušate ispravljanje odjednom, ne možemo pokušavate rukovati stvari više ili manje u kojem su unesene, ali s nekim grupnog slanja promjena kada se dogodi mnogo toga u na istovremeno. Ovime resursa servisa tkanina Upravitelj vrlo odgovara sadržaju koji se pokreću u klasteru.

Dok većina tih zadataka za kompliciranim izvješćivanjem (ako postoje ograničenja kršenja, popravak ih, ako postoje services da biste stvorili, stvorite ih), Voditelj resursa klasteru potrebna i nekih dodatnih informacija da biste odredili li imbalanced klaster. Za to imamo dva druge dijelove konfiguracije: *Opterećenja pragovi* i *Pragovi aktivnosti*.

## <a name="balancing-thresholds"></a>Pragovi za ujednačavanje
Prag za ujednačavanje je glavni kontrola za pokretanje određene proaktivne rebalancing (Imajte na umu da je na mjerača vremena za koliko često Voditelj resursa klaster treba provjeriti – ne znači da bilo što će se dogoditi). Prag opterećenja definira kako imbalanced klaster mora biti određene metrike redoslijedom za klaster upravitelj resursa treba uzeti u obzir imbalanced i okidača za ujednačavanje.

Ujednačavanje pragovi se definiraju na temelju po metriku kao dio definiciju klaster. Dodatne informacije o metriku pogledajte članak [u ovom se članku](service-fabric-cluster-resource-manager-metrics.md).

ClusterManifest.xml

``` xml
    <Section Name="MetricBalancingThresholds">
      <Parameter Name="MetricName1" Value="2"/>
      <Parameter Name="MetricName2" Value="3.5"/>
    </Section>
```

Prag opterećenja metrike je trake programa Word. Ako količinu Učitaj na čvor najčešće učitati podijeljen količinu Učitaj na najmanje učitati čvor premašuje broj, zatim Klaster se smatra imbalanced i opterećenja će se pokrenuti sljedeći put Voditelj resursa Klaster se provjerava (po zadanom, jeste li ikad 5 sekundi, kao uređena MinLoadBalancingInterval, gore navedenoj sintaksi).

![Primjer ujednačavanje praga.][Image1]

U ovom primjeru jednostavne svaki servis koristi jedinice neke metriku. U gornjem primjeru Maksimalno opterećenje čvor je 5 i minimum je 2. Recimo da je ujednačavanje prag za ovu metriku 3. Dakle, u gornjem primjeru klaster smatra raspoređen i bez opterećenja će se aktiviraju kada Voditelj resursa klaster provjerava (jer je omjer u klasteru 5/2 = 2,5, i to je manji od praga navedeni ujednačavanje 3).

U primjeru dolje Maks opterećenje čvor je 10, dok je minimalne 2, rezultira omjer 5. Ovo stavlja klaster preko praga dizajniranih ujednačavanje 3 za tu metriku. Zbog toga globalni rebalancing Pokreni bit će zakazani sljedeći put aktivira ujednačavanje mjerača vremena. Imajte na umu da samo zato što je ujednačavanje pretraživanja kicked ne znači da ništa će se premjestiti - ponekad je imbalanced klaster, no ne mogu poboljšati situaciji -, ali u slučaju poput ovoga (najmanje po zadanom) neke opterećenje gotovo certainly se distribuirati Node3. Imajte na umu da jer smo ne koristite pohlepa pristup neke opterećenja nije također se distribuirati da bi Node2 jer koji dovodi minimization cjelokupan razlike između čvorove, ali smo očekivali Node3 želite prijeći Većina opterećenje.

![Ujednačavanje akcije primjer praga.][Image2]

Napomena početak ispod praga ujednačavanje nije li eksplicitnih cilj – opterećenja pragovi su samo *okidača* koja govori Voditelj resursa klaster koje treba pogleda klaster da biste odredili koja su poboljšanja možete učiniti ako ih ima.

## <a name="activity-thresholds"></a>Pragovi aktivnosti
Ponekad, iako čvorove relativno imbalanced, *Ukupni* iznos Učitaj u klasteru nedostaje. To može biti samo zbog doba dana ili jer je klaster novo, a samo dohvaćanje bootstrapped. U svakom slučaju, možda ne želite trošiti vrijeme opterećenja klaster jer je zapravo vrlo malo se stekli – ćete samo trošite mreža i računalnim resurse da biste premjestili stvari oko, bez uzrok apsolutne razlike. Jer želimo da biste izbjegli taj postupak, postoji drugu kontrolu unutar upravitelj resursa, naziva pragovi aktivnosti koje omogućuje vam da navedete neke apsolutne donja granica aktivnosti – ako nema čvor ima najmanje to koliko opterećenja, zatim opterećenja neće se pokrenuti čak i ako se ispuni praga opterećenja.

Kao primjer recimo imamo izvješća pomoću sljedećih ukupne zbrojeve za potrošnju na te čvorove. Također Recimo da ne možemo zadržati naše prag opterećenja od 3 za ovu metriku, ali sada i imamo praga aktivnosti programa 1536. U prvom slučaju, dok je klaster imbalanced po praga za ujednačavanje nema čvor zadovoljava te minimalne prag aktivnosti pa ćemo samostalno napusti stvari. U primjeru dolje Node1 je način preko praga aktivnosti pa opterećenja provest će se (jer je premašena je prag opterećenja i praga aktivnosti za metriku)

![Primjer aktivnosti praga.][Image3]

Baš kao i za ujednačavanje pragovi pragovi aktivnosti su definirana po – metriku putem definiciju klaster:

ClusterManifest.xml

``` xml
    <Section Name="MetricActivityThresholds">
      <Parameter Name="Memory" Value="1536"/>
    </Section>
```

Imajte na umu da opterećenja i aktivnost pragovi oba uz metriku - opterećenja će se tek ako se opterećenja i aktivnost pragovi premaše isti metrike. Dakle, ako ne možemo Premašili prag opterećenja memorije i praga aktivnosti za procesora, opterećenja ne pokreće pod uvjetom da su prekoračili preostale pragovi (opterećenja praga za CPU-a) i aktivnosti prag memorije.

## <a name="balancing-services-together"></a>Zajednički servisi za ujednačavanje
Nešto što je zanimljiv Imajte na umu je da je li klaster imbalanced ili ne je odluka klaster razini, ali način smo otvorite o rješavanju ga je premještanje replike pojedinačnih servisa i instance oko. To vam odgovara, desno? Ako memorije složeni prema gore na jedan čvor, više replike ili može biti za pojačava instance, pa je nije potreban premještanje svih s praćenjem stanja replike ili bez praćenja stanja instance koje koriste metriku problematične, imbalanced.

Povremeno Premda će se klijent poziva nam prema gore ili datoteke karata kojoj piše da servis koji nije imbalanced premješten. Kako se događa da servisa dobiva premještati čak i ako su Balansirani sve metriku tu uslugu, čak i savršeno tako, u trenutku u neravnoteži? Da vidim!

Preuzimanje, primjerice četiri services, Service1, Service2, Service3 i Service4. Service1 izvješća protiv metriku Metric1 i Metric2, Service2 od Metric2 i Mmetric3 Service3 od Metric3 i Metric4 i Service4 protiv neke metričkim Metric99. Surely možete vidjeti koje ćemo ćete ovdje. Imamo lanac! Iz perspektive od Voditelj resursa klaster ne zaista imamo četiri neovisno services, imamo skup servise koji su povezani (Service1, Service2 i Service3) i onu koja je isključena samostalno.

![Zajednički servisi za ujednačavanje][Image4]

Da bi je moguće mogu prouzročiti programa neravnoteži u Metric1 replike ili instance pripadaju Service3 za pomicanje. Obično premještanja vrlo ograničeni su, ali možete biti veće ovisno o točno onako kako imbalanced Metric1 imate i koje promjene bile su nužne u klasteru da biste to ispravili. Također ćemo izgovorite s provjeru pouzdanosti da je neravnoteži u metriku 1, 2 ili 3 nikad uzrokovat će premještanja u Service4 – bi postojati bez zareza nakon premještanja u replike ili instance pripadaju Service4 oko mogu učinite apsolutno ništa te tako utjecati na saldo metriku 1, 2 ili 3.

Voditelj resursa klaster odlučuje koji su Servisi povezani, jer usluge su dodane, uklonite, ili imali njihova promjena metričkim konfiguracija – na primjer, između dva pokretanja opterećenja Service2 možda ste konfigurirana da biste uklonili Metric2 automatski o. Prijelomi redaka lanac između Service1 i Service2. Umjesto dvije skupine usluga, imate tri:

![Zajednički servisi za ujednačavanje][Image5]

## <a name="next-steps"></a>Daljnji koraci
- Metriku su način na koji upravitelj resursa za servis tkanina klaster upravlja potrošnje i kapacitet u klasteru. Da biste saznali više o njima i kako ih konfigurirali pogledajte [članak](service-fabric-cluster-resource-manager-metrics.md)
- Premještanje trošak je jedan način signalizacija da biste Voditelj resursa klaster jesu li određene usluge više skupi da biste premjestili od ostalih. Da biste saznali više o trošak premještanja, pogledajte [ovaj](service-fabric-cluster-resource-manager-movement-cost.md) članak
- Voditelj resursa klaster ima nekoliko Reguliranje koja možete konfigurirati da biste usporili churn u klasteru. To su ne obično potrebno, ali ako su vam potrebne dodatne o njima [ovdje](service-fabric-cluster-resource-manager-advanced-throttling.md)


[Image1]:./media/service-fabric-cluster-resource-manager-balancing/cluster-resrouce-manager-balancing-thresholds.png
[Image2]:./media/service-fabric-cluster-resource-manager-balancing/cluster-resource-manager-balancing-threshold-triggered-results.png
[Image3]:./media/service-fabric-cluster-resource-manager-balancing/cluster-resource-manager-activity-thresholds.png
[Image4]:./media/service-fabric-cluster-resource-manager-balancing/cluster-resource-manager-balancing-services-together1.png
[Image5]:./media/service-fabric-cluster-resource-manager-balancing/cluster-resource-manager-balancing-services-together2.png
