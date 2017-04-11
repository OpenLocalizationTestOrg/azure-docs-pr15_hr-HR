<properties
    pageTitle="Otklanjanje poteškoća s automatsko skaliranje s virtualnog računala skaliranje skupovima | Microsoft Azure"
    description="Otklanjanje poteškoća s automatsko skaliranje sa skupovima skaliranje virtualnog računala. Razumijevanje uobičajeni došlo je do problema i kako biste ih riješili."
    services="virtual-machine-scale-sets"
    documentationCenter=""
    authors="gbowerman"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machine-scale-sets"
    ms.workload="na"
    ms.tgt_pltfrm="windows"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/28/2016"
    ms.author="guybo"/>

# <a name="troubleshooting-autoscale-with-virtual-machine-scale-sets"></a>Otklanjanje poteškoća s automatsko skaliranje sa skupovima skaliranje virtualnog računala

**Problem** – infrastruktura za autoscaling ste stvorili u Azure Voditelj resursa pomoću skupove skaliranje VM – na primjer uvođenje predloška ovako: https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-bottle-autoscale – imate instaliranu definirana pravila za mjerilo, a zatim ga odgovara, osim što se bez obzira na to koliko učitavanja stavite na na VMs neće automatsko skaliranje.

## <a name="troubleshooting-steps"></a>Korake za otklanjanje poteškoća

Što treba uzeti u obzir obuhvaćaju sljedeće:

- Koliko jezgri svaki VM ima te se učitavaju svaki core?
 Gornji primjer Azure brzi početak rada predloška ima do_work.php skriptu koja učitava jedan core. Ako koristite VM koji će biti veća od jednog core veličina VM kao što su Standard_A1 ili D1, a zatim, morat ćete pokrenuti ovaj učitavanja više puta. Provjerite koliko cores vaše VMs tako da pročitate [računalima virtualne veličine za Windows Azure](../virtual-machines/virtual-machines-windows-sizes.md)

- Koliko VMs u VM skaliranje skupu ste rad na svaki od njih?

    Skaliranje out događaj se samo odvija kad prosjek procesora preko **svih** na VMs u skupu skaliranje premašuje vrijednosti praga tijekom vremena interne definirano u pravila za automatsko skaliranje.

- Propustite jeste li svi događaji skaliranje?

    Provjerite zapisnike nadzora u Azure portal za skaliranje događaje. Možda je došlo skalu prema gore i skalu prema dolje koja je Propušteni. Možete filtrirati prema "Skaliranje"...

    ![Zapisnika nadzora][audit]

- Razlikuju se vaš skaliranje dodaci i skaliranje iz pragovi potpuno?

    Pretpostavimo da ste postavili pravila da biste skalirali izgleda kada je prosječna procesora veća od 50% više od pet minuta, a zatim na skaliranje kada je prosječna procesora manje od 50%. "Flapping" problema to prouzročiti kada je procesora blizu prag, s akcijama skaliranje neprestano povećanje i smanjivanje veličine skupa. Zbog toga servis za automatsko skaliranje pokuša da biste spriječili "flapping", koje možete manifesta kao ne skaliranja. Stoga provjerite je li vaš skaliranje Izlaz i promjena veličine u pragovi razlikuju se potpuno dopustili prostor između skaliranja.

- Jeste li napišite vlastiti predložak JSON?

    Jednostavno doći do pogrešaka, pa započinjanje od predloška kao jedan iznad koje je dokazano rad, a zatim unesite promjena u malim koracima je. 

- Možete ručno promijenite li ili smanjili?

    Pokušajte redeploying resursa postavite skaliranje VM postavku različite "kapaciteta" da biste ručno promijenili broj VMs. Ovdje je na primjer predložak da biste to učinili: https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-scale-existing – možda ćete morati urediti predložak da biste provjerili ima istu veličinu računala kao što je vaš skaliranje postavljanje koristi. Ako broj VMs uspješno možete promijeniti ručno, zatim znate Izolirani za automatsko skaliranje je problem.

- Provjerite svoje Microsoft.Compute/virtualMachineScaleSet i Microsoft.Insights resursa u programu [Explorer resursa za Azure](https://resources.azure.com/)

    Ovo je alat za otklanjanje poteškoća nezaobilaznim poruka koji se prikazuje stanje Voditelj resursa Azure resurse. Kliknite pretplatu i pregled grupa resursa su otklanjanje poteškoća. U odjeljku davatelja resursa računalnim pogledajte VM skaliranje skup koji ste stvorili i provjerite prikaz instanci koje prikazuje stanje implementacije. Prikaz instanci VMs u skupu VM skaliranje provjeriti. Zatim otvorite u davatelja resursa Microsoft.Insights i provjerite pravila za automatsko skaliranje izgleda dobro.

- Dijagnostičke proširenje radi i emitira podataka o performansama?

    __Ažuriranja:__ Azure automatsko skaliranje poboljšan je za korištenje kanal za glavno računalo temelji metriku koji više nije potrebna nastavkom Dijagnostika za instalaciju. To znači da uz nekoliko odlomaka više ne pojavljuje ako stvarate zatvaranje autoscaling pomoću novog kanala. Primjeri Azure predloške koje su pretvorene da biste koristili kanal glavnog računala: https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-bottle-autoscale, https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-lapstack-autoscale. 

    Korištenje glavno računalo temelji metriku za automatsko skaliranje je bolje iz sljedećih razloga:

    - Manje premještanje dijelove kao bez proširenja Dijagnostika moraju biti instalirani.
    - Predlošci za jednostavniji. Samo dodati pravila za automatsko skaliranje uvida postojećeg predloška skup mjerilo.
    - Više pouzdanog izvješćivanje i brže pokretanje novog VMs.

    Samo razloga želite nastaviti koristiti nastavkom dijagnostičkih bi ako vam je potrebna memorije Dijagnostika izvješćivanje/skaliranja. Metriku glavnog računala koji se temelji izvješće ne memorije.

    Uz to na umu samo slijedite ostatku članka ako i dalje koristite dijagnostičkih proširenja za vaše autoscaling.

    Automatsko skaliranje u Azure Voditelj resursa možete raditi (ali više ne može) pomoću programa VM proširenje naziva proširenje Dijagnostika. Ga emits podataka o performansama s računom za pohranu koje ste definirali u predlošku. Ove podatke pa se pridružuje servis Azure Monitor.

    Ako servis uvide ne može čitati podatke iz na VMs, treba da biste poslali poruku e-pošte – na primjer ako su na VMs prema dolje, pa provjerite e-pošte (onaj koji ste naveli prilikom stvaranja račun za Azure).

    Otvorite i pogledati podatke sami. Pogledajte Azure prostora za pohranu račun pomoću programa explorer oblaka. Primjer pomoću [Explorer oblaka za Visual Studio](https://visualstudiogallery.msdn.microsoft.com/aaef6e67-4d99-40bc-aacf-662237db85a2), prijavite se, a zatim odaberite Azure pretplate koristite i naziv računa za pohranu za dijagnostiku referencirani u definiciji proširenje Dijagnostika u predlošku implementacije...

    ![Oblak Explorer][explorer]

    Ovdje ćete vidjeti skup tablica koje se pohranjuju podaci iz svakog VM. Poduzimanja Linux i metriku procesora kao primjer potražite kod zadnje izvršene redaka. Oblak explorer Visual Studio podržava jezika za upite tako da pokrenete upit kao što su "vremenska oznaka gt datetime'2016-02-02T21:20:00Z'" da biste bili sigurni da imate najnovija događaja (pretpostavlja da je vrijeme u UTC-u). Označava podataka koje vidite u postoji odgovaraju pravila skaliranje prilikom postavljanja? U primjeru u nastavku procesora za računalo 20 rada povećanje na 100% putem posljednjih 5 minuta...

    ![Spremište tablica][tables]

    Ako su podaci ne postoji, pa ga podrazumijeva je problem s nastavkom dijagnostičkih izvodi u na VMs. Ako su podaci postoje, znači da postoji neki problem s pravilima skale ili sa servisom uvide. Provjerite [Azure Status](https://azure.microsoft.com/status/).

    Nakon što ste je kroz ove korake ako i dalje imate problema automatsko skaliranje nije pokušajte na forumima na [MSDN](https://social.msdn.microsoft.com/forums/azure/home?category=windowsazureplatform%2Cazuremarketplace%2Cwindowsazureplatformctp)ili [posložiti prelijevanje](http://stackoverflow.com/questions/tagged/azure)ili poziv podršci za prijavu. Budite spremni za zajedničko korištenje predloška i prikaz podataka o performansama.

[audit]: ./media/virtual-machine-scale-sets-troubleshoot/image3.png
[explorer]: ./media/virtual-machine-scale-sets-troubleshoot/image1.png
[tables]: ./media/virtual-machine-scale-sets-troubleshoot/image4.png
