<properties
    pageTitle="Provjerite pogon D: VM podatkovni disk | Microsoft Azure"
    description="U članku se opisuje kako promijeniti slova jedinica za Windows VM da bi mogli koristiti pogon D: kao podatkovni pogon."
    services="virtual-machines-windows"
    documentationCenter=""
    authors="cynthn"
    manager="timlt"
    editor=""
    tags="azure-resource-manager,azure-service-management"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/27/2016"
    ms.author="cynthn"/>

# <a name="use-the-d-drive-as-a-data-drive-on-a-windows-vm"></a>D: pogon upotrijebili podatke pogon na VM za Windows 

Ako aplikacija nije potrebno koristiti pogon D da biste pohranili podatke, slijedite ove upute za korištenje različitih slovo za privremene disk. Nikad ne koristite privremene disk da biste pohranili podatke koje želite zadržati.

Ako promijenite veličinu ili **Zaustavi (Deallocate)** virtualnog računala, to može pokrenuti položaj virtualnog računala da biste novi hypervisor. Planirani ili neplanirano održavanja događaja može pokrenuti i u ovom položaj. U ovom scenariju privremene disk će se ponovno prvi dostupan slovo. Ako imate aplikaciju posebno potreban D: pogon, ćete morati slijedite ove korake da biste privremeno premjestiti na pagefile.sys, priložite novi disk podataka i dodijeliti slovo D i zatim povratak na pagefile.sys privremene pogon. Kada završi, Azure neće imati natrag na D: Ako na VM premješta različite hypervisor.

Dodatne informacije o tome kako Azure koristi privremene disk, pročitajte [objašnjenje privremene pogon na virtualnim strojevima programa Microsoft Azure](https://blogs.msdn.microsoft.com/mast/2013/12/06/understanding-the-temporary-drive-on-windows-azure-virtual-machines/)

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-both-include.md)]

## <a name="attach-the-data-disk"></a>Prilaganje podatkovni disk

Najprije morate pridružiti podatkovni disk virtualnog računala. 

- Da biste koristili portalu, potražite u članku [kako priložiti podatkovni disk na portalu za Azure](virtual-machines-windows-attach-disk-portal.md)
- Da biste koristili klasični portal, potražite u članku [kako priložiti podatkovni disk na virtualnog računala za Windows](virtual-machines-windows-classic-attach-disk.md). 


## <a name="temporarily-move-pagefilesys-to-c-drive"></a>Privremeno premjestiti pagefile.sys pogon C

1. Povezivanje s virtualnog računala. 

2. Desnom tipkom miša kliknite izbornik **Start** , a zatim odaberite **sustav**.

3. U lijevom izborniku odaberite **Dodatne postavke sustava**.

4. U odjeljku **performanse** odaberite **Postavke**.

5. Odaberite karticu **Dodatno** .

5. U odjeljku **virtualna memorija** odaberite **Promijeni**.

6. Odaberite pogon **C** , a zatim kliknite **Veličina upravljana sustavom** , a zatim kliknite **Postavljanje**.

7. Odaberite pogonu **D** i zatim kliknite **datoteke broja stranica** , a zatim kliknite **Postavljanje**.

8. Kliknite Primijeni. Dobit ćete upozorenje da računalo nije potrebno je ponovno pokrenuti da bi utječu na promjene.

9. Ponovno pokrenite virtualnog računala.




## <a name="change-the-drive-letters"></a>Promjena slova pogona 

1. Nakon ponovnog pokretanja na VM ponovo prijaviti na VM.

2. Kliknite izbornik **Start** , upišite **diskmgmt.msc** i pritisnite Enter. Pokrenut će se disk Management.

3. Desnom tipkom miša kliknite **D**, pogon Privremena pohrana, a zatim odaberite **Promijeni slovo i putove**.

4. U odjeljku slovo, odaberite pogon **G** , a zatim kliknite **u redu**. 

5. Desnom tipkom miša kliknite podatkovni disk pa odaberite **Promijeni pogon slovo i putove**.

6. U odjeljku slovo, odaberite pogonu **D** , a zatim kliknite **u redu**. 

7. Desnom tipkom miša kliknite **G**, pogon Privremena pohrana, a zatim odaberite **Promijeni slovo i putove**.

8. U odjeljku slovo, odaberite pogon **E** , a zatim kliknite **u redu**. 

> [AZURE.NOTE] Ako je vaš VM diskova drugih pogona, koristiti isti način da biste dodijelili slova pogon na diskova i pogona. Želite da se konfiguracija diska se sljedeće:  
>- C: OS disk  
>- D: podatkovni Disk  
>- E: privremeni disk



## <a name="move-pagefilesys-back-to-the-temporary-storage-drive"></a>Vraćanje pagefile.sys pogon Privremena pohrana 

1. Desnom tipkom miša kliknite izbornik **Start** , a zatim odaberite **sustav**

2. U lijevom izborniku odaberite **Dodatne postavke sustava**.

3. U odjeljku **performanse** odaberite **Postavke**.

4. Odaberite karticu **Dodatno** .

5. U odjeljku **virtualna memorija** odaberite **Promijeni**.

6. Odaberite OS pogon **C** i kliknite **bez broja stranica datoteka** , a zatim kliknite **Postavi**.

7. Odaberite pogon privremeno spremište **E** , a zatim **Veličina upravljana sustavom** , a zatim kliknite **Postavljanje**.

8. Kliknite **Primijeni**. Dobit ćete upozorenje da računalo nije potrebno je ponovno pokrenuti da bi utječu na promjene.

9. Ponovno pokrenite virtualnog računala.




## <a name="next-steps"></a>Daljnji koraci
- Dostupan prostor za pohranu za virtualnog računala možete povećati Pridruživanjem [na disku dodatne podatke](virtual-machines-windows-attach-disk-portal.md).



