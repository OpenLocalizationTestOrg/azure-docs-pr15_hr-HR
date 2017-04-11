<properties 
    pageTitle="Dodatne informacije o ograničavanje BizTalk Services | Microsoft Azure" 
    description="Informirajte se o ograničavanje pragovi i rezultat izvođenja ponašanja za BizTalk servise. Ograničavanje temelji se na memorije i broj poruka. MABS, WABS" 
    services="biztalk-services" 
    documentationCenter="" 
    authors="MandiOhlinger" 
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="biztalk-services" 
    ms.workload="integration" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/15/2016" 
    ms.author="mandia"/>





# <a name="biztalk-services-throttling"></a>BizTalk usluge: ograničavanje

Servisa Azure BizTalk implementira servis ograničavanje koji se temelji na dva uvjeta: memorije i broj istodobno poruka obrade. U ovoj se temi navodi regulacije pragovi i opisuje ponašanje prilikom izvođenja kada dođe do regulacije uvjeta.

## <a name="throttling-thresholds"></a>Ograničavanje pragovi

U sljedećoj su tablici navedeni regulacije izvora i pragovi:

||Opis|Niska praga.|Visoke praga.|
|---|---|---|---|
|Memorije|% zbroja sustava memorije dostupne/PageFileBytes. <p><p>Ukupan dostupan PageFileBytes je približno 2 RAM-a sustava.|60%|70%|
|Obrada poruke|Broj poruka istodobno obrade|40 * broj jezgri|100 * broj jezgri|

Kad zbirka dostigne visoke prag servisa Azure BizTalk počinje throttle. Ograničavanje tabulatora kad zbirka dostigne niskog praga. Na primjer, na servisu koristi 65% sistemske memorije. U tom slučaju neće throttle servis. Na servisu pokreće korištenje memorije sustava 70%. U tom slučaju servis regulira i nastavi throttle dok se servis koristi 60% (niska prag) sistemske memorije.

Servisa Azure BizTalk prati regulacije status (nepromijenjen nasuprot ograničeni stanje) i regulacije trajanje.


## <a name="runtime-behavior"></a>Ponašanje prilikom izvođenja

Kada se servisa Azure BizTalk regulacije stanje, događa se sljedeće:

- Ograničavanje je po ulogama instance. Ako, na primjer:<br/>
Ograničavanje je RoleInstanceA. RoleInstanceB je ograničavanje. U tom slučaju poruke u RoleInstanceB se obrađuju prema očekivanjima. Poruke u RoleInstanceA odbacuju i neće funkcionirati uz sljedeća pogreška:<br/><br/>
**Poslužitelj je zauzet. Pokušajte ponovno.**<br/><br/>
- Sve izvore istaknuti ankete ili preuzeti poruke. Ako, na primjer:<br/>
Na kanal povlači poruke iz vanjskog izvora FTP. Uloga instance rade na istaknuti može vidjeti u regulacije stanje. U tom slučaju kanal zaustavlja preuzimanje dodatnih poruka dok ne instancu uloga Zaustavi ograničavanje.
- Odgovor se šalje klijent tako klijent možete ponovno pošaljite poruku.
- Morate pričekati na ograničavanje riješen. Konkretno, morate pričekati zbirka dostigne niskog praga.

## <a name="important-notes"></a>Važne bilješke
- Ograničavanje se ne može se onemogućiti.
- Ograničavanje pragovi nije moguće mijenjati.
- Ograničavanje je implementirana sistemskih.
- Poslužitelj baze podataka SQL Azure ima ugrađene ograničavanje.

## <a name="additional-azure-biztalk-services-topics"></a>Dodatne teme servisa Azure BizTalk Services

-  [Instaliranje servisa Azure BizTalk SDK](http://go.microsoft.com/fwlink/p/?LinkID=241589)<br/>
-  [Vodiči za: Servisa Azure BizTalk](http://go.microsoft.com/fwlink/p/?LinkID=236944)<br/>
-  [Kako pokrenuti pomoću Azure BizTalk Services SDK](http://go.microsoft.com/fwlink/p/?LinkID=302335)<br/>
-  [Azure BizTalk Services](http://go.microsoft.com/fwlink/p/?LinkID=303664)<br/>

## <a name="see-also"></a>Vidi također
- [Servisi za BizTalk: Za razvojne inženjere, Basic, standardna i Premium izdanja grafikona](http://go.microsoft.com/fwlink/p/?LinkID=302279)<br/>
- [BizTalk servisa: Azure pomoću portala za klasični dodjele resursa](http://go.microsoft.com/fwlink/p/?LinkID=302280)<br/>
- [Servisi za BizTalk: Dodjeljivanje Status grafikona](http://go.microsoft.com/fwlink/p/?LinkID=329870)<br/>
- [BizTalk servisa: Kartica nadzorne ploče, Monitor i skaliranje](http://go.microsoft.com/fwlink/p/?LinkID=302281)<br/>
- [BizTalk servisa: Sigurnosno kopiranje i vraćanje](http://go.microsoft.com/fwlink/p/?LinkID=329873)<br/>
- [BizTalk servisa: Naziv izdavača i izdavatelj ključ](http://go.microsoft.com/fwlink/p/?LinkID=303941)<br/>
 
