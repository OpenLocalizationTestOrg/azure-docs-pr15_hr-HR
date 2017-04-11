<properties
    pageTitle="Čarobnjak za kopiranje podataka tvorničke | Microsoft Azure"
    description="Saznajte kako pomoću čarobnjaka za kopiranje tvorničke podataka da biste kopirali podatke iz podržani izvori podataka primatelji."
    services="data-factory"
    documentationCenter=""
    authors="spelluru"
    manager="jhubbard"
    editor="monicar"/>

<tags
    ms.service="data-factory"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/29/2016"
    ms.author="spelluru"/>

# <a name="data-factory-copy-wizard"></a>Čarobnjak za kopiranje tvorničke podataka
Čarobnjak za kopiranje tvorničke Azure podataka je da biste olakšali postupak ingesting podataka, koji se obično je prvi korak u scenariju za integraciju programa završetka do kraja podataka. Kada posjećujete putem čarobnjaka za kopiranje Azure podataka tvorničke, ne morate razumjeti sve definicije JSON povezani servisi, skupova podataka i kanali. Međutim, nakon što dovršite sve korake čarobnjaka, čarobnjak automatski stvara kanala da biste kopirali podatke iz odabranog izvora podataka na označeno odredište. Osim toga, čarobnjak za kopiranje pomaže vam za provjeru valjanosti podataka koja se ingested vrijeme za izradu, čime se štedi velik dio vremena, osobito kada vam se ingesting podataka prvi put iz izvora podataka. Da biste pokrenuli čarobnjak za kopiranje, kliknite pločicu **Kopiranje podataka** na početnoj stranici tvorničke vaše podatke.

![Kopiranje čarobnjaka](./media/data-factory-copy-wizard/copy-data-wizard.png)


## <a name="an-intuitive-wizard-for-copying-data"></a>Intuitivno Čarobnjak za kopiranje podataka
Čarobnjak omogućuje jednostavno premještanje podataka iz raznih izvora odredišta u minutama. Slijedeći čarobnjaka za kanal s Kopiraj aktivnosti automatski se stvara za vas uz zavisne entiteti tvorničke podataka (povezani servisi i skupova podataka). Bez dodatnih koraka su potrebne za stvaranje kanal.   

![Odaberite izvor podataka](./media/data-factory-copy-wizard/select-data-source-page.png)

> [AZURE.NOTE] Potražite u članku [vodič Čarobnjak za kopiranje](data-factory-copy-data-wizard-tutorial.md) članak detaljne upute za stvaranje uzorka kanala da biste kopirali podatke iz programa Azure bloba tablici baze podataka SQL Azure. 

Čarobnjak osmišljene su s velikih skupova podataka na umu od početka. Je jednostavno i učinkovito stvaranje kanali tvorničke podataka kojima se stotine mapa, datoteke ili tablica pomoću čarobnjaka za kopiranje podataka. Čarobnjak podržava sljedeće tri značajke: Pretpregled podataka, hvatanje sheme i mapiranje i filtriranje podataka. 

## <a name="automatic-data-preview"></a>Pretpregled podataka 
Čarobnjak za kopiranje omogućuje vam da pregledate dio podatke iz odabranog izvora podataka za provjeru li podatke točnim podacima koje želite kopirati. Osim toga, ako je izvorišnim podacima u tekstualnoj datoteci, čarobnjak za kopiranje raščlanjuje tekstnu datoteku da biste saznali retka i stupca graničnike i sheme automatski. 

![Postavke oblika datoteke](./media/data-factory-copy-wizard/file-format-settings.png)

## <a name="schema-capture-and-mapping"></a>Snimanje sheme i mapiranja 
Shema ulaznih podataka možda odgovaraju shemi izlazne podatke u nekim slučajevima. U ovom slučaju morate mapiranje stupaca iz izvora sheme stupce iz sheme odredište. 

Čarobnjak za kopiranje automatski mapira stupce u shemi izvora u stupce u shemi odredište. Možete nadjačati mapiranja pomoću padajućim popisima (ili) odredite hoće li stupac mora se preskočiti prilikom kopiranja podatke.   

![Mapiranje sheme](./media/data-factory-copy-wizard/schema-mapping.png)

## <a name="filtering-data"></a>Filtriranje podataka  
Čarobnjak vam omogućuje da filtrirate izvorišnih podataka da biste odabrali podatke koje je potrebno kopirat odredišta/primatelj spremišta podataka. Filtriranje smanjuje količinu podataka za kopiranje spremišta podataka primatelj i stoga poboljšava propusnost kopiranja. Pruža fleksibilne način za filtriranje podataka u relacijske baze podataka u programu SQL upita jezik (ili) datoteke u mapi blobova platforme Azure pomoću [funkcije tvorničke podataka i varijabli](data-factory-functions-variables.md).   

### <a name="filtering-of-data-in-a-database"></a>Filtriranje podataka u bazi podataka.  
U primjeru se koristi SQL upita na `Text.Format` (opis funkcije) i `WindowStart` varijabli. 

![Provjerite valjanost izraza](./media/data-factory-copy-wizard/validate-expressions.png)

### <a name="filtering-of-data-in-an-azure-blob-folder"></a>Filtriranje podataka u mapi blobova platforme Azure
Da biste kopirali podatke iz mape koju određuje prilikom izvođenja temelju [varijabli sustava](data-factory-functions-variables.md#data-factory-system-variables)možete koristiti varijable u put do mape. Podržani varijable su: **{godina}**, **{mjesec}**, **{dan}**, **{sat}**, **{minute}**i **{prilagođene}**. Primjer: inputfolder / {godina} / {mjesec} / {dan}.

Pretpostavimo da ste ulazne mape u sljedećem obliku:

    2016/03/01/01
    2016/03/01/02
    2016/03/01/03
    ...

Kliknite gumb **Pregledaj** za **datoteku ili mapu**, otvorite neku od tih mapa (Ako, na primjer, 2016 -> 03 -> 01 -> 02), a zatim kliknite **Odaberi**. Trebali biste vidjeti `2016/03/01/02` u tekstni okvir. Sada zamijenite **2016** **{godina}**, **03** s **{mjesecom}**, **01** uz **{dan}**i **02** s **{sat}**pa pritisnite tipku Tab. Trebali biste vidjeti padajućih popisa da biste odabrali oblik za ove četiri varijable:

![Korištenje sustava varijable](./media/data-factory-copy-wizard/blob-standard-variables-in-folder-path.png)   

Kao što je prikazano u sljedećim snimku zaslona, možete koristiti **prilagođene** varijabla i sve [podržane niza oblika](https://msdn.microsoft.com/library/8kb3ddd4.aspx). Da biste odabrali mapu s tom strukturu, prvi put koristite gumb **Pregledaj** . Zatim zamijenite vrijednost **{prilagođene}**, a zatim pritisnite tipku Tab da biste vidjeli tekstni okvir gdje možete upisati niz oblika.     

![Korištenje prilagođenih varijabla](./media/data-factory-copy-wizard/blob-custom-variables-in-folder-path.png)


## <a name="support-for-diverse-data-and-object-types"></a>Podrška za raznih podataka i vrste objekata
Pomoću čarobnjaka za kopiranje učinkovito možete premjestiti stotine mapa, datoteke ili tablica.

![Odaberite tablice iz koje želite kopirati podatke](./media/data-factory-copy-wizard/select-tables-to-copy-data.png)

## <a name="scheduling-options"></a>Mogućnosti rasporeda
Možete pokrenuti kopiranja jednom ili na raspored (zaračunava, svaki dan, i tako dalje). Obaju mogućnosti može se koristiti za breadth poveznika putem lokalnog, oblaka i lokalnu kopiju radne površine.

Jednokratno kopiranje samo jednom omogućuje premještanje podataka iz izvora na odredište. Odnosi s podacima bilo koje veličine i bilo kojem podržanom obliku. Zakazano Kopiraj omogućuje vam da biste kopirali podatke na propisanim ponavljanja. Obogaćeni postavke (kao što je pokušaj vremenskog ograničenja i upozorenja) možete koristiti da biste konfigurirali zakazani Kopiraj.

![Planiranje svojstva](./media/data-factory-copy-wizard/scheduling-properties.png)


## <a name="next-steps"></a>Daljnji koraci
Brzi vodič pomoću čarobnjaka za kopiranje tvorničke podataka da biste stvorili na kanal s Kopiraj aktivnosti, potražite u članku [Praktični vodič: Stvaranje kanal pomoću čarobnjaka za kopiranje](data-factory-copy-data-wizard-tutorial.md).
