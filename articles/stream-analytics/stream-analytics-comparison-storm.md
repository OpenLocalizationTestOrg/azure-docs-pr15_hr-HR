<properties
    pageTitle="Analitički platforme: Apache oluja usporedbe strujanje analize | Microsoft Azure"
    description="Preuzmite upute odabir platforma oblaka analize pomoću programa Apache oluja usporedbe strujanje analize. Razumijevanje značajki i razlike."
    keywords="Analitika platforme, Analitika platforme, platformu Analitika oblak, oluja usporedbe"
    services="stream-analytics"
    documentationCenter=""
    authors="jeffstokes72"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="stream-analytics"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="big-data"
    ms.date="09/26/2016"
    ms.author="jeffstok"/>

# <a name="help-choosing-a-streaming-analytics-platform-apache-storm-comparison-to-azure-stream-analytics"></a>Pomoć odabir strujanje platformu analize: Apache oluja usporedbe Azure strujanje Analytics

Preuzmite upute odabir platforma oblaka analize pomoću ove Apache oluja usporedbe za Azure strujanje analize. Razumijevanje propositions vrijednost od strujanje analize nasuprot Apache oluja kao servis za upravljane na Azure HDInsight tako da odaberete pravo rješenje za svoju tvrtku koristiti slučajeva.

I analize platforme pružaju prednosti rješenje PaaS, postoji nekoliko glavne mogućnosti za distinguishing koja ih razlikovali. Mogućnosti kao i ograničenja od tih servisa navedena su u nastavku da biste lakše ćete krenuti na rješenje potrebno da biste postigli ciljeve.

## <a name="storm-comparison-to-stream-analytics-general-features"></a>Oluja usporedbe strujanje analize: opće značajke ##
<table border="1" cellspacing="0" cellpadding="0">
    <tbody>
        <tr>
            <td width="174" valign="top">
                <p>
                    <strong> </strong>
                </p>
            </td>
            <td width="204" valign="top">
                <p>
                    <strong>Azure strujanje Analytics</strong>
                </p>
            </td>
            <td width="246" valign="top">
                <p>
                    <strong>Apache oluja na HDInsight</strong>
                </p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p>
                    <strong>Otvori izvor</strong>
                </p>
            </td>
            <td width="204" valign="top">
                <p>
Ne, Azure strujanje analize je zakonom zaštićeno Microsoft koja nudi.
                </p>
            </td>
            <td width="246" valign="top">
                <p>
Da, Apache oluja je tehnologije za Apache licencirane.
                </p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p>
                    <strong>Microsoft podržana</strong>
                </p>
            </td>
            <td width="204" valign="top">
                <p>
Da </p>
            </td>
            <td width="246" valign="top">
                <p>
Da </p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p>
                    <strong>Hardverski preduvjeti</strong>
                </p>
            </td>
            <td width="204" valign="top">
                <p>
Postoje bez hardverske preduvjete. Azure analize strujanje je Azure usluga.
                </p>
            </td>
            <td width="246" valign="top">
                <p>
Postoje bez hardverske preduvjete. Apache oluja je Azure usluga.
                </p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p>
                    <strong>Gornje razine jedinica</strong>
                </p>
            </td>
            <td width="204" valign="top">
                <p>
S klijentima Azure strujanje analize uvođenje i praćenje zadataka strujanje.
                </p>
            </td>
            <td width="246" valign="top">
                <p>
S Apache oluja HDInsight kupaca implementacije i nadzirati cijeli klaster koje možete hostirati više oluja zadataka, kao i drugih radnih opterećenja (grupe za uključivanje).
                </p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p>
                    <strong>Cijena</strong>
                </p>
            </td>
            <td width="204" valign="top">
                <p>
Analitički strujanje ima glasnoću podataka obrađuje i potreban broj strujanje jedinica (sat zadatak izvršava).
                </p>
                <p>
                    <a href="http://azure.microsoft.com/en-us/pricing/details/stream-analytics/">Dodatne informacije o cijenama nalazi se ovdje.</a>
                </p>
            </td>
            <td width="246" valign="top">
                <p>
Za Apache oluja na HDInsight, jedinice za kupnju klaster utemeljen je pa se naplaćuje na temelju vremena klaster uključen, neovisno o zadacima implementiran.
                </p>
                <p>
                    <a href="http://azure.microsoft.com/en-us/pricing/details/hdinsight/">Dodatne informacije o cijenama nalazi se ovdje.</a>
                </p>
            </td>
        </tr>
    </tbody>
</table>
##Prilikom stvaranja na platformi svaki analytics##
<table border="1" cellspacing="0" cellpadding="0">
    <tbody>
        <tr>
            <td width="174" valign="top">
                <p>
                    <strong> </strong>
                </p>
            </td>
            <td width="204" valign="top">
                <p>
                    <strong>Azure strujanje Analytics</strong>
                </p>
            </td>
            <td width="246" valign="top">
                <p>
                    <strong>Apache oluja na HDInsight</strong>
                </p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p>
                    <strong>Mogućnosti: DSL SQL</strong>
                </p>
            </td>
            <td width="204" valign="top">
                <p>
Da, jednostavno je za korištenje SQL podrška za jezike dostupna.
                </p>
            </td>
            <td width="246" valign="top">
                <p>
Ne, korisnici moraju pisanje koda u Java C# ili koristiti API-ji Trident.
                </p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p>
                    <strong>Mogućnosti: Vremenski operatori</strong>
                </p>
            </td>
            <td width="204" valign="top">
                <p>
Odjava iz njega u okviru podržan je prozorima zbrajanja i vremenski spojeva.
                </p>
            </td>
            <td width="246" valign="top">
                <p>
Za implementaciju korisnik mora biti vremenski operatore.
                </p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p>
                    <strong>Sučelje za razvoj</strong>
                </p>
            </td>
            <td width="204" valign="top">
                <p>
Interaktivni prilikom stvaranja i ispravljanje pogrešaka sučelje putem portala za Azure ogledne podatke.
                </p>
            </td>
            <td width="246" valign="top">
                <p>
Razvoj ispravljanje pogrešaka i praćenje zadovoljstva je navedeno putem Visual Studio sučelje za .NET korisnici dok Java i razvojni inženjeri druge jezike mogu koristiti IDE po izboru.
                </p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p>
                    <strong>Podrška za ispravljanje pogrešaka</strong>
                </p>
            </td>
            <td width="204" valign="top">
                <p>
Strujanje Analytics nudi operacije zapisnici kao način ispravljanje pogrešaka i status osnovni posla, ali trenutno ne nudi fleksibilnosti pri što/kako slično je sve obuhvaćeno zapisnike ie: opširno načinu rada.
                </p>
            </td>
            <td width="246" valign="top">
                <p>
Detaljne zapisnika dostupni su za ispravljanje pogrešaka svrhe. Dva su načina za izvlačenje zapisnika korisniku, putem visual Studio ili korisnik može RDP u klaster da biste pristupili zapisnika.
                </p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p>
                    <strong>Podrška za UDF (korisnički definirane funkcije)</strong>
                </p>
            </td>
            <td width="204" valign="top">
                <p>
Trenutno ne postoji podrška za UDF-ove.
                </p>
            </td>
            <td width="246" valign="top">
                <p>
Korisnički definiranih funkcija možete staviti u C#, Java ili na jeziku po izboru.
                </p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p>
                    <strong>Proširive - prilagođenog koda</strong>
                </p>
            </td>
            <td width="204" valign="top">
                <p>
Ne postoji podrška za extensible kod u strujanje analize.
                </p>
            </td>
            <td width="246" valign="top">
                <p>
Da, postoji dostupnost da biste pisali prilagođenog koda u C#, Java ili druge podržanim jezicima oluja.
                </p>
            </td>
        </tr>
    </tbody>
</table>
##Izvori podataka i izlaze##
<table border="1" cellspacing="0" cellpadding="0">
    <tbody>
        <tr>
            <td width="174" valign="top">
                <p>
                    <strong> </strong>
                </p>
            </td>
            <td width="204" valign="top">
                <p>
                    <strong>Azure strujanje Analytics</strong>
                </p>
            </td>
            <td width="246" valign="top">
                <p>
                    <strong>Apache oluja na HDInsight</strong>
                </p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p>
                 <strong>Izvori podataka za unos</strong>
                </p>
            </td>
            <td width="204" valign="top">
                <p>Podržani izvori unos su Azure događaj koncentratora i Azure blob-ova.
                </p>
            </td>
            <td width="246" valign="top">
                <p>
Dostupno poveznika koncentratora događaja servisa Bus, Kafka, itd. Nepodržane poveznika možda se implementirati putem prilagođenog koda.
                </p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p>
                    <strong>Oblici unos podataka</strong>
                </p>
            </td>
            <td width="204" valign="top">
                <p>
Podržani oblici unos su Avro, JSON, a zatim CSV.
                </p>
            </td>
            <td width="246" valign="top">
                <p>
Bilo koji oblik može se implementirati putem prilagođenog koda.
                </p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p>
                    <strong>Izlaze</strong>
                </p>
            </td>
            <td width="204" valign="top">
                <p>
Strujanje posao može imati više izlaza. Podržani izlaze: Koncentratora Azure događaj, blobova platforme Azure, Azure tablice, baze podataka Azure SQL i PowerBI.
                </p>
            </td>
            <td width="246" valign="top">
                <p>
Podrška za mnoge izlaze u topologije svaki Izlaz možda prilagođene logike do obrada. U okvir oluja sadrži poveznika PowerBI, Azure događaj koncentratora, spremište blobova platforme Azure, Azure DocumentDB, SQL i HBase. Nepodržane poveznika možda se implementirati putem prilagođenog koda.
                </p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p>
                    <strong>Oblike kodiranje podataka</strong>
                </p>
            </td>
            <td width="204" valign="top">
                <p>
Analitički strujanje zahtijeva UTF-8 oblik podataka da biste se koristi.
                </p>
            </td>
            <td width="246" valign="top">
                <p>
Bilo koji oblik kodiranja podataka mogu se implementirati putem prilagođenog koda.
                </p>
            </td>
        </tr>
    </tbody>
</table>
##Upravljanje i operacije##
<table border="1" cellspacing="0" cellpadding="0">
    <tbody>
        <tr>
            <td width="174" valign="top">
                <p>
                    <strong> </strong>
                </p>
            </td>
            <td width="204" valign="top">
                <p>
                    <strong>Azure strujanje Analytics</strong>
                </p>
            </td>
            <td width="246" valign="top">
                <p>
                    <strong>Apache oluja na HDInsight</strong>
                </p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p>
                    <strong>Model implementacije posla</strong>
                </p>
                <p>
                    - <strong>Portal za Azure</strong>
                </p>
                <p>
                    - <strong>Visual Studio</strong>
                </p>
                <p>
                    - <strong>PowerShell</strong>
                </p>
            </td>
            <td width="204" valign="top">
                <p>
Uvođenje implementira se putem portala za Azure, PowerShell i REST API-ji.
                </p>
            </td>
            <td width="246" valign="top">
                <p>
Depolyment implementira se putem portala za Azure, PowerShell, Visual Studio i REST API-ji.
                </p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p>
                    <strong>Nadzor (stat, mjerača itd.)</strong>
                </p>
            </td>
            <td width="204" valign="top">
                <p>
Nadzor implementira se putem portala za Azure i REST API-ji.
                </p>
                <p>
Korisnik može konfigurirati Azure upozorenja.
                </p>
            </td>
            <td width="246" valign="top">
                <p>
Nadzor implementirana putem korisničkog Sučelja oluja i REST API-ji.
                </p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p>
                    <strong>Skalabilnost</strong>
                </p>
            </td>
            <td width="204" valign="top">
                <p>
Broj strujanje jedinica za svaki zadatak. Svaka jedinica strujanje obrađuje do 1 MB/s. Max 50 jedinicama prema zadanim postavkama. Poziv da biste povećali ograničenje.
                </p>
            </td>
            <td width="246" valign="top">
                <p>
Broj čvorove u skupini HDI oluja. Nema ograničenja broja čvorove (gornji ograničenje definira kvota za Azure). Poziv da biste povećali ograničenje.
                </p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p>
                    <strong>Ograničenja za obradu podataka</strong>
                </p>
            </td>
            <td width="204" valign="top">
                <p>
Korisnici mogu mijenjati veličinu prema gore ili dolje broj strujanje jedinice da biste povećali obradu podataka ili optimiziranje troškove.
                </p>
                <p>
Promjena veličine do 1 GB/s </p>
            </td>
            <td width="246" valign="top">
                <p>
Korisnik mogu mijenjati veličinu prema gore ili dolje klaster potrebama.
                </p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p>
                    <strong>Zaustavi/životopis</strong>
                </p>
            </td>
            <td width="204" valign="top">
                <p>
Prekid i nastavak iz posljednje mjesto zaustavio.
                </p>
            </td>
            <td width="246" valign="top">
                <p>
Prekid i nastavak od posljednjeg postavite Zaustavi na temelju vodeni žig.
                </p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p>
                    <strong>Ažuriranje servisa i framework</strong>
                </p>
            </td>
            <td width="204" valign="top">
                <p>
Automatsko zakrpa s bez isključiti.
                </p>
            </td>
            <td width="246" valign="top">
                <p>
Automatsko zakrpa s bez isključiti.
                </p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p>
                    <strong>Neprekidno poslovanje putem iznimno dostupan servis sigurno SLA</strong>
                </p>
            </td>
            <td width="204" valign="top">
                <p>
SLA vrijeme 99.9% aktivnosti </p>
                <p>
Automatski oporavak od pogrešaka </p>
                <p>
Oporavak operatora s praćenjem stanja vremenski je ugrađeni.
                </p>
            </td>
            <td width="246" valign="top">
                <p>
SLA vrijeme aktivnosti 99.9% od klaster oluja. Apache oluja je kvara pogreške strujanje god je odgovornosti klijenata da biste bili sigurni neometano izvođenje njihove strujanje zadatke.
                </p>
            </td>
        </tr>
    </tbody>
</table>
##Napredne značajke##
<table border="1" cellspacing="0" cellpadding="0">
    <tbody>
        <tr>
            <td width="174" valign="top">
                <p>
                    <strong> </strong>
                </p>
            </td>
            <td width="204" valign="top">
                <p>
                    <strong>Azure strujanje Analytics</strong>
                </p>
            </td>
            <td width="246" valign="top">
                <p>
                    <strong>Apache oluja na HDInsight</strong>
                </p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p>
                    <strong>Najnovije kašnjenja u dolascima i upravljanje izvan redoslijeda događajima</strong>
                </p>
            </td>
            <td width="204" valign="top">
                <p>
Ugrađeni konfigurirati pravila da biste promijenili redoslijed, ispustite događaje ili prilagodili vrijeme događaja.
                </p>
            </td>
            <td width="246" valign="top">
                <p>
Korisnik mora implementirati logike za rukovanje scenarij.
                </p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p>
                    <strong>Pregled podataka</strong>
                </p>
            </td>
            <td width="204" valign="top">
                <p>
Referentnih podataka dostupni iz Azure blob-ova maksimalnog ograničenja veličine 100 MB predmemoriju pretraživanja u memoriji. Osvježavanje podataka referenca upravlja servis.
                </p>
            </td>
            <td width="246" valign="top">
                <p>
Nema ograničenja veličine podataka. Poveznici za HBase, DocumentDB, SQL Server i Azure. Nepodržane poveznika možda se implementirati putem prilagođenog koda.
                </p>
                <p>
Osvježavanje podataka referenca obradi prilagođeni kod.
                </p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p>
                    <strong>Integracija s računala učenje</strong>
                </p>
            </td>
            <td width="204" valign="top">
                <p>
Konfiguriranjem objavljena Azure strojnog učenja modela kao funkcije tijekom stvaranja ASA posla <a href="http://blogs.msdn.com/b/streamanalytics/archive/2015/05/24/real-time-scoring-of-streaming-data-using-machine-learning-models.aspx">(privatne pretpregled)</a>.
                </p>
            </td>
            <td width="246" valign="top">
                <p>
Dostupno putem Vijci oluja.
                </p>
            </td>
        </tr>
    </tbody>
</table>
