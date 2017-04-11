<properties
    pageTitle="Postavljanje virtualnog računala kao poslužitelj bilježnice IPython | Microsoft Azure"
    description="Postavljanje gore programa Azure virtualnog računala za korištenje u okruženju znanstvenog podataka s poslužiteljem IPython za napredne analize."
    services="machine-learning"
    documentationCenter=""
    authors="bradsev"
    manager="jhubbard"
    editor="cgronlun"  />

<tags
    ms.service="machine-learning"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/19/2016"
    ms.author="xibingao;bradsev" />

# <a name="set-up-an-azure-virtual-machine-as-an-ipython-notebook-server-for-advanced-analytics"></a>Postavljanje Azure virtualnog računala kao bilježnicu IPython poslužitelj za napredne analize

U ovoj se temi objašnjava Dodjela resursa i konfiguriranje Azure virtualnog računala za napredne analize koje možete koristiti kao dio okruženju znanstvenog podataka. Windows virtualnog računala je konfiguriran za korištenje alata kao što su kao što su IPython bilježnice, Explorer Azure prostora za pohranu, AzCopy, kao i drugi uslužni programi koji se koriste za projekte naprednom analitikom za podršku. Azure Explorer prostora za pohranu i AzCopy, na primjer, omogućuju praktičan načina za prijenos podataka u spremište blobova platforme Azure s lokalnog računala ili da biste preuzeli na lokalno računalo iz spremišta blobova.

## <a name="create-vm"></a>Korak 1: Stvaranje opće namjene Azure virtualnog računala

Ako već imate Azure virtualnog računala, a samo želite postaviti poslužitelj za IPython bilježnice na njoj, možete preskočiti ovaj korak i nastavite s [Korak 2: dodavanje krajnje točke za IPython bilježnice postojeće virtualnog računala](#add-endpoint).

Prije pokretanja postupka stvaranja virtualnog računala na Azure, morate odrediti veličinu na računalu koje je potrebno za obradu podataka za svoje projekt. Manje računalima imati manje memorije i manje jezgri procesora od veće strojeva, ali su manje skupi. Popis vrsta računala i cijene, potražite u članku <a href="http://azure.microsoft.com/pricing/details/virtual-machines/" target="_blank">Virtualnim strojevima cijene</a> stranice

1. Prijavite se na <a href="https://manage.windowsazure.com" target="_blank">Portal za klasični Azure</a>pa u donjem lijevom kutu kliknite **Novo** . Pojavit će se prozor prema gore. Odaberite **IZRAČUNATI** -> **VIRTUALNOG računala** -> **Iz GALERIJE**.

    ![Stvaranje radnog prostora][24]

2. Odaberite jednu od sljedećih slike:

    * Podatkovnog centra sustava Windows Server 2012 R2
    * Windows Server Essentials sučelje (Windows Server 2012 R2)

    Nakon toga kliknite strelicu koja pokazuje desno u donjem desnom da biste prešli na sljedeću stranicu konfiguracije.

    ![Stvaranje radnog prostora][25]

3. Unesite naziv virtualnog računala koji želite stvoriti, odaberite veličinu na ovom računalu (zadano: A3) ovisno o veličini skupa podataka na računalu će postupak i kako Napredna koje želite da se (memorije i broj jezgri računalnim), unesite korisničko ime i lozinku za računalo stroj. Zatim kliknite strelicu koja pokazuje udesno da biste prešli na sljedeću stranicu konfiguracije.

    ![Stvaranje radnog prostora][26]

4. Odaberite **PODRUČJE/AFINITET GRUPE/VIRTUALNE MREŽE** koja sadrži **RAČUN za POHRANU** namjeravate koristiti za ovaj virtualnog računala, a zatim odaberite taj račun za pohranu. Dodavanje krajnje pri dnu u polju **krajnje točke** unosom naziv krajnje točke ("IPython" u nastavku). Možete odabrati bilo koji niz kao **naziv** krajnje točke i bilo koji cijeli broj između 0 i 65536 koja je **dostupna** kao **JAVNO PRIKLJUČAK**. **PRIVATNA PRIKLJUČAK** mora biti **9999**. Korisnici trebali biste **izbjegli** korištenje javno priključke koji su već dodijeljeni za internetske servise. <a href="http://www.chebucto.ns.ca/~rakerman/port-table.html" target="_blank">Priključke za internetske servise</a> sadrži popis priključaka koji su vam dodijeljene moraju se izbjegavati.

    ![Stvaranje radnog prostora][27]

5. Kliknite kvačicu da biste pokrenuli virtualnog računala dodjeljivanje postupak.

    ![Stvaranje radnog prostora][28]


Može potrajati 15 25 minuta da biste dovršili virtualnog računala dodjeljivanje postupak. Nakon stvaranja virtualnog računala status ovo računalo treba prikazati kao **pokrenut**.

![Stvaranje radnog prostora][29]

## <a name="add-endpoint"></a>Korak 2: Dodavanje krajnje točke za IPython bilježnice postojeće virtualnog računala

Ako ste stvorili virtualnog računala slijedeći upute u koraku 1, krajnja točka za IPython bilježnica već dodani i mogu se preskočiti ovaj korak.

Ako već postoji virtualnog računala, a morate dodati krajnje IPython bilježnice koje će instalirati u koraku 3 ispod prvog zapisnika na klasični Portal Azure, odaberite virtualnog računala pa dodajte krajnja točka za poslužitelj IPython bilježnice. Na slici u nastavku sadrži snimka zaslona s portala nakon dodavanja krajnja točka za bilježnicu IPython virtualnog računala za Windows.

![Stvaranje radnog prostora][17]

## <a name="run-commands"></a>Korak 3: Instalacija IPython bilježnice i druge alate za podršku

Nakon stvaranja virtualnog računala pomoću protokola udaljene radne površine (RDP) za prijavu u Windows virtualnog računala. Upute potražite u članku [kako se prijaviti u virtualnog računala izvodi Windows Server](../virtual-machines/virtual-machines-windows-classic-connect-logon.md). Otvorite **naredbeni redak** (**ne u prozoru naredbe ljuske Powershell**) kao **Administrator** , a zatim pokrenite sljedeću naredbu.

    set script='https://raw.githubusercontent.com/Azure/Azure-MachineLearning-DataScience/master/Misc/MachineSetup/Azure_VM_Setup_Windows.ps1'

    @powershell -NoProfile -ExecutionPolicy unrestricted -Command "iex ((new-object net.webclient).DownloadString(%script%))"

Kada se instalacija dovrši, server IPython bilježnice automatski u pokretanja na *C:\\korisnika\\\<korisničko ime\>\\dokumenata\\IPython bilježnica* direktorija.

Kada se to od vas zatraži, unesite lozinku za bilježnicu IPython i lozinku administratora računala. Time se omogućuje IPython bilježnice da biste pokrenuli kao usluge na računalu.

## <a name="access"></a>Korak 4: Access IPython bilježnica u web-pregledniku
Da biste pristupili poslužitelja IPython bilježnicu, otvorite web-mjesto preglednika i unos *https://&#60;virtual strojno DNS naziv >: & #60; broj priključka javno >* u tekstni okvir URL. Ovdje, na *& #60; broj priključka javno >* mora biti broj priključka koje ste naveli kada je dodana krajnju točku IPython bilježnice.

U *& #60; naziv DNS-a virtualnog računala >* možete pronaći na klasični Portal Azure. Nakon klika zapisivanje u klasični portal **VIRTUALNIM STROJEVIMA**, odaberite na računalu koji ste stvorili, a zatim odaberite **nadzorne PLOČE**, naziv DNS-a koji se prikazuju na sljedeći način:

![Stvaranje radnog prostora][19]

Dobit ćete upozorenje koja govori te _postoji problem sa sigurnosnim certifikatom ovog web-mjesta_ (Internet Explorer) ili _je veza s ne privatne_ (Chrome), kao što je prikazano u sljedeće slike. Kliknite **Nastavi da biste ovo web-mjesto (ne preporučuje se)** (Internet Explorer) ili **Dodatno** , a zatim * *nastavili da biste & #60;* Naziv DNS*> (nesigurnih) ** (Chrome) da biste nastavili. Zatim ću unijeti lozinku ste ranije naveli da biste pristupili IPython bilježnice.

**Internet Explorer:**
![stvaranje radnog prostora][20]

**Chrome:**
![stvaranje radnog prostora][21]

Kada se prijavite u bilježnici IPython, direktorij *DataScienceSamples* vode na web-pregledniku. Taj direktorij sadrži ogledne IPython bilježnice koje zajednički koriste Microsoft da bi korisnicima vođenje zadacima znanstvenog podataka. Ove ogledne IPython bilježnice su odjavljene [**Github**](https://github.com/Azure/Azure-MachineLearning-DataScience/tree/master/Misc/DataScienceProcess/iPythonNotebooks) spremištu na virtualnim strojevima tijekom poslužitelja IPython bilježnice postaviti postupak. Microsoft zadržava i često ažurira ovo spremište. Korisnici mogu posjetite spremište Github da biste dobili nedavno ažurirane bilježnica IPython uzorka.
![Stvaranje radnog prostora][18]

## <a name="upload"></a>Korak 5: Prijenos postojeće bilježnice IPython s lokalnog računala s poslužiteljem IPython bilježnice

Bilježnica IPython pružaju jednostavan način za korisnike koji žele prijenos postojeće bilježnice IPython na svojim računalima lokalne bilježnice IPython server na virtualnim računalima. Kada korisnici se prijavite na IPython bilježnicu u web-pregledniku, kliknite u **imeniku** IPython bilježnica će se prenijeti u. Zatim odaberite datoteku .ipynb IPython bilježnice da biste prenijeli s lokalnog računala u **Eksploreru za datoteke**i povucite i ispustite ga u direktorij IPython bilježnice na web-pregledniku. Kliknite gumb **Prijenos** da biste prenijeli datoteke .ipynb poslužitelj IPython bilježnice. Drugi korisnici zatim možete početi koristiti u iz svoje web-preglednika.

![Stvaranje radnog prostora][22]

![Stvaranje radnog prostora][23]


##<a name="shutdown"></a>Isključivanje i Poništi dodjelu virtualnog računala kada nije u upotrebi

Azure virtualnim strojevima su cjenovnom kao **platiti samo koje koristite**. Da biste bili sigurni da vam se ne koji se naplaćuju kada ne koristite virtualnog računala, on mora biti u stanju **zaustavljen (Deallocated)** kada nije u upotrebi.

> [AZURE.NOTE] Ako isključite virtualnog računala s unutar na VM (pomoću mogućnosti power Windows), na VM je zaustavljena, ali ostaje Dodijeljeno. Da biste osigurali nastaviti naplaćivati, uvijek zaustavite virtualnim strojevima s [Azure klasični Portal](http://manage.windowsazure.com/). VM putem komponente Powershell možete isključiti i tako da nazovete **ShutdownRoleOperation** s "PostShutdownAction" jednako "StoppedDeallocated".

Da biste isključili i deallocate virtualnog računala:

1. Prijavite se na [Azure klasični Portal](http://manage.windowsazure.com/) pomoću računa.  

2. Na lijevoj navigacijskoj traci odaberite **VIRTUALNIM RAČUNALIMA** .

3. Na popisu virtualnim strojevima kliknite naziv virtualnog računala, a zatim idite na stranicu **nadzorne PLOČE** .

4. Pri dnu stranice kliknite **isključivanje**.

![Isključivanje VM][15]

Virtualnog računala se deallocated, ali ne briše. U bilo kojem trenutku na portalu klasični Azure možda početi virtualnog računala.

## <a name="your-azure-vm-is-ready-to-use-whats-next"></a>Vaš VM Azure spremna je za korištenje: što slijedi?

Sada je spremna za korištenje u vašem vježbe znanstvenog podataka virtualnog računala. Virtualnog računala i spremna je za korištenje kao poslužitelj IPython bilježnicu za istraživanje i obrada podataka i druge zadatke u kombinaciji s Azure strojnog učenja i proces znanstvenog timu podataka.

Daljnji koraci u postupku timu podataka znanstvenog mapiraju se u [Tečaj](https://azure.microsoft.com/documentation/learning-paths/cortana-analytics-process/) , a može sadržavati korake koji premještanje podataka u HDInsight, obradu i je li uzorak u Priprema za učenje iz podataka s Azure strojnog učenja.


[15]: ./media/machine-learning-data-science-setup-virtual-machine/vmshutdown.png
[17]: ./media/machine-learning-data-science-setup-virtual-machine/add-endpoints-after-creation.png
[18]: ./media/machine-learning-data-science-setup-virtual-machine/sample-ipnbs.png
[19]: ./media/machine-learning-data-science-setup-virtual-machine/dns-name-and-host-name.png
[20]: ./media/machine-learning-data-science-setup-virtual-machine/browser-warning-ie.png
[21]: ./media/machine-learning-data-science-setup-virtual-machine/browser-warning.png
[22]: ./media/machine-learning-data-science-setup-virtual-machine/upload-ipnb-1.png
[23]: ./media/machine-learning-data-science-setup-virtual-machine/upload-ipnb-2.png
[24]: ./media/machine-learning-data-science-setup-virtual-machine/create-virtual-machine-1.png
[25]: ./media/machine-learning-data-science-setup-virtual-machine/create-virtual-machine-2.png
[26]: ./media/machine-learning-data-science-setup-virtual-machine/create-virtual-machine-3.png
[27]: ./media/machine-learning-data-science-setup-virtual-machine/create-virtual-machine-4.png
[28]: ./media/machine-learning-data-science-setup-virtual-machine/create-virtual-machine-5.png
[29]: ./media/machine-learning-data-science-setup-virtual-machine/create-virtual-machine-6.png
