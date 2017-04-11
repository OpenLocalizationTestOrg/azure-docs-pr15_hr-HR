<properties
   pageTitle="Defragmentacija metriku u Azure Service tkanina | Microsoft Azure"
   description="Pregled korištenja defragmentaciju ili packing kao Strategije za metriku u tkanina servisa"
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

# <a name="defragmentation-of-metrics-and-load-in-service-fabric"></a>Defragmentacija metriku i opterećenje u tkanina servisa
Voditelj resursa za servis tkanina klaster uglavnom se bavi opterećenja pomoću distribucija opterećenje – pazeći da sve čvorove u klasteru su ravnomjerno koristi. To je obično izgleda najsigurnija i smartest pomoću surviving pogreške jer je jamči da sve navedene pogreške ne uklanjanje neke velike postotak zadani posla. Voditelj resursa za servis tkanina klaster podržavaju različite strategije kao i koja je defragmentaciju. Defragmentacija obično znači da umjesto pokušaja Upotreba metrike raspodijelite klaster, ne možemo zapravo pokušajte ih konsolidirati. Ovo je fortunate inverziju od naše normalni strategije – umjesto optimiziranje klaster na temelju minimiziranje average standardnu devijaciju metričkim učitavanje metrika, ne možemo pokrenuti optimizacija za povećanja u devijacija. No razlozi Ovaj strategija?

I, ako koju ste širenje opterećenje ravnomjerno među čvorovi u klasteru zatim koje ste eaten gore neki od resursa čvorove nudi. Obično to nije problem, ali ponekad neke radnih opterećenja stvaranje servise koji su iznimno velika i zauzeti Većina čvor – izgovorite 75% da biste 95% resursa u čvor bi završe namjenski instancu jednog servisa ili replike. To nije problem, Voditelj resursa klaster će otkriti vrijeme stvaranja servisa koje je potrebno da biste preuredili klaster da biste oslobodili prostor za ovog velike posla i postavite o čime se dogoditi, ali u aplikacijom te radno opterećenje mora čekati zakazati u klasteru.

Given da zakazivanje nove radnih opterećenja obično je barem malo Latencija slova, ako ne moramo ništa drugačije možemo ponekad blow desno, tim SLA ako postoji mnogo servise i stanja da biste premještali, osobito ako radnih opterećenja u klasteru su obično velike (i zato neobično dulje da biste premještali u klasteru). Uistinu, kada se ne možemo mjerenja vremena stvaranja u simulations utemeljenih na podacima realni klaster, smo vidjeli koji ako su services dovoljna, a klaster prilično koristi da se stvaranje tih velike servisa bi slowed prema dolje, i da ne možemo nije poboljšati to Uvod u pravila defragmentaciju metriku.

Baš kao i datoteke stvaranja ili access nije moguće dobiti slowed prema dolje na tvrdom disku je fragmentirane i nije sped defragmentacijom pogon tako da su veće neprekinutih blokova dostupan, možete konfigurirati defragmentaciju metriku da bi klaster Voditelj resursa možete doći pokušati sažmete opterećenja servisa u manje čvorove tako da postoji (gotovo) uvijek sobe za čak i velike servise , što im omogućava brzo stvoriti. Većina ljudi ne, morate jer usluga obično mora biti mala i zato nije teško pronaći sobe za njih, ali ako imate veliki servise i morate ih brzo stvoriti (i spremni ste prihvatili tradeoffs kao što su povećana impactfulness pogrešaka i nekoliko resursa koji se ulijevo unutilized dok čekaju radnih opterećenja zakazati), zatim strategije defragmentaciju namijenjen vama.

Dijagramu u nastavku omogućuje vizualni prikaz dvije različite klastere, koji je defragmentirati i koji nije. U slučaju da Balansirani, razmislite o premještanja koji će biti potrebno mjesto jedan od najvećeg objekata servisa ako ste novi da biste stvorili, u usporedbi s defragmented klaster gdje ga nije moguće odmah smjestiti na čvorove 4 i 5.

![Uspoređivanje raspoređen i defragmentirati klastere][Image1]

## <a name="defragmentation-pros-and-cons"></a>Defragmentacija argumente za i protiv
Tako što su te konceptualni tradeoffs? Preporučujemo da se temeljito mjera vaše radnih opterećenja prije uključivanja defragmentaciju metriku. Ovo je brzi tablica stvari koje morate na umu:

| Defragmentacija profesionalce  | Defragmentacija protiv |
|----------------------|----------------------|
|Omogućuje brže stvaranje velikih servisa | Učitavanje concentrates na manje čvorove povećanje Nadmetanje
|Omogućuje donji premještanje podataka tijekom stvaranja    | Do neuspjele može utjecati na druge usluge zbog čega više churn
|Omogućuje obogaćenog opis preduvjeti i reclamation prostora | Složenije cjelokupan konfiguracija za upravljanje resursa

Ne možete miješati defragmentirati i normalnog metriku u istom klaster i Voditelj resursa neće imati najbolje je da biste bili sigurni da ćete dobiti raspored koji objedinjuje koliko je metriku defragmentaciju možete prilikom pokušaja širenje ostale. Točne rezultate prikazat će se ovisi o broju opterećenja metriku u usporedbi s broj defragmentaciju metriku i njihovih debljine trenutno opterećenje, itd.

## <a name="configuring-defragmentation-metrics"></a>Konfiguriranje defragmentaciju mjerenja
Konfiguriranje metriku defragmentaciju je globalni odluka u klasteru i moguće odabrati pojedinačne metriku za defragmentaciju:

ClusterManifest.xml:

```xml
<Section Name="DefragmentationMetrics">
    <Parameter Name="Disk" Value="true" />
    <Parameter Name="CPU" Value="false" />
</Section>
```

## <a name="next-steps"></a>Daljnji koraci
- Voditelj resursa klaster sadrži mnogo mogućnosti za s opisom klaster. Da biste saznali više o njima pogledajte ovaj članak na [s opisom servisa tkanina klaster](service-fabric-cluster-resource-manager-cluster-description.md)
- Metriku su način na koji upravitelj resursa za servis tkanina klaster upravlja potrošnje i kapacitet u klasteru. Da biste saznali više o njima i kako ih konfigurirali pogledajte [članak](service-fabric-cluster-resource-manager-metrics.md)

[Image1]:./media/service-fabric-cluster-resource-manager-defragmentation-metrics/balancing-defrag-compared.png
