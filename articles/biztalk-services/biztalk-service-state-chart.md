<properties 
    pageTitle="Zadaci koje su dopuštene u različiti statusi ili statusi BizTalk Services | Microsoft Azure" 
    description="Akcije/operacije dopušteni u drugi status MABS: Zaustavi, pokretanje, ponovno pokrenite, privremeno obustavljanje, životopis, brisanje, promjena veličine, ažuriranje konfiguracije i sigurnosnom prema gore" 
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



# <a name="biztalk-services-service-state-chart"></a>BizTalk servisa: Grafikon za stanje servisa
Ovisno o trenutnom stanju servisa BizTalk postoje operacije koje možete ili ne može izvesti na servisu BizTalk.

Na primjer, dodjela novog BizTalk servisa Azure klasični portalu. Kada uspješno završi, BizTalk servis je u stanju Aktivno. U stanju Aktivno možete zaustaviti servis BizTalk. Ako Zaustavi ne uspije, servis BizTalk vodi do zaustavljanja stanje. Zaustavi ne uspije, servis BizTalk odlazak StopFailed stanje. U stanju StopFailed možete ponovno pokrenite servis BizTalk. Ako pokušate postupak koji nije dopušten, kao što su životopis servisa BizTalk javlja se sljedeća pogreška:

**Operacija nije dopuštena**

Dodjela BizTalk servisa, idite na [BizTalk usluge: dodjeljivanje Azure pomoću portala za klasični](http://go.microsoft.com/fwlink/p/?LinkID=302280).

U sljedećim tablicama navedeni operacije ili akcije koje možete obaviti kada je servis BizTalk u određenim stanju. Na ✔ znači postupak dopušten u tom stanju. Praznog znači operacija nije moguće izvesti u tom stanju.

## <a name="start-stop-restart-suspend-resume-and-delete-operations"></a>Pokretanje, zaustavljanje, ponovno pokrenite, privremeno obustavljanje životopis, i brisanje operacije
<table border="1">
<tr>
        <th colspan="15">Operacija ili akciju</th>
</tr>

<tr>
        <th rowspan="18">Stanje servisa BizTalk</th>
</tr>
<tr bgcolor="FAF9F9">
        <th> </th>
        <th>Pokretanje</th>
        <th>Zaustavi</th>
        <th>Ponovno pokretanje računala</th>
        <th>Privremeno obustavljanje</th>
        <th>Životopis</th>
        <th>Brisanje</th>
</tr>
<tr>
<td bgcolor="FAF9F9"><b>Aktivni</b></td>
<td> </td>
<td><center>✔</center></td>
<td><center>✔</center></td>
<td><center>✔</center></td>
<td> </td>
<td><center>✔</center></td>
</tr>
<tr>
<td bgcolor="FAF9F9"><b>Onemogućeni</b></td>
<td> </td>
<td> </td>
<td> </td>
<td> </td>
<td> </td>
<td><center>✔</center></td>
</tr>
<tr>
<td bgcolor="FAF9F9"><b>Obustavljena</b></td>
<td> </td>
<td> </td>
<td> </td>
<td> </td>
<td><center>✔</center></td>
<td><center>✔</center></td>
</tr>
<tr>
<td bgcolor="FAF9F9"><b>Zaustavi</b></td>
<td><center>✔</center></td>
<td> </td>
<td><center>✔</center></td>
<td> </td>
<td> </td>
<td><center>✔</center></td>
</tr>
<tr>
<td bgcolor="FAF9F9"><b>Nije uspjelo ažuriranje servisa</b></td>
<td> </td>
<td> </td>
<td> </td>
<td> </td>
<td> </td>
<td><center>✔</center></td>
</tr>
<tr>
<td bgcolor="FAF9F9"><b>DisableFailed</b></td>
<td> </td>
<td> </td>
<td> </td>
<td> </td>
<td> </td>
<td><center>✔</center></td>
</tr>
<tr>
<td bgcolor="FAF9F9"><b>EnableFailed</b></td>
<td> </td>
<td> </td>
<td> </td>
<td> </td>
<td> </td>
<td><center>✔</center></td>
</tr>
<tr>
<td bgcolor="FAF9F9"><b>StartFailed<br/>
StopFailed<br/>
RestartFailed</b></td>
<td><center>✔</center></td>
<td><center>✔</center></td>
<td><center>✔</center></td>
<td> </td>
<td> </td>
<td><center>✔</center></td>
</tr>
<tr>
<td bgcolor="FAF9F9"><b>SuspendedFailed<br/>
ResumeFailed</b></td>
<td> </td>
<td> </td>
<td> </td>
<td><center>✔</center></td>
<td><center>✔</center></td>
<td><center>✔</center></td>
</tr>
<tr>
<td bgcolor="FAF9F9"><b>CreatedFailed<br/>
RestoreFailed<br/></b></td>
<td> </td>
<td> </td>
<td> </td>
<td> </td>
<td> </td>
<td><center>✔</center></td>
</tr>
<tr>
<td bgcolor="FAF9F9"><b>ConfigUpdateFailed</b></td>
<td> </td>
<td> </td>
<td><center>✔</center></td>
<td> </td>
<td> </td>
<td><center>✔</center></td>
</tr>
<tr>
<td bgcolor="FAF9F9"><b>ScaleFailed</b></td>
<td> </td>
<td> </td>
<td> </td>
<td> </td>
<td> </td>
<td><center>✔</center></td>
</tr>
</table>
<br/>

## <a name="scale-update-configuration-and-backup-operations"></a>Promjena veličine, ažuriranje konfiguracije i operacije sigurnosnog kopiranja
<table border="1">
<tr>
        <th colspan="15">Operacija ili akciju</th>
</tr>

<tr>
        <th rowspan="18">Stanje servisa BizTalk</th>
</tr>
<tr bgcolor="FAF9F9">
        <th> </th>
        <th>Promjena veličine</th>
        <th>Ažuriranje konfiguracije</th>
        <th>Sigurnosno kopiranje</th>
</tr>
<tr>
<td bgcolor="FAF9F9"><b>Aktivni</b></td>
<td><center>✔</center></td>
<td><center>✔</center></td>
<td><center>✔</center></td>
</tr>
<tr>
<td bgcolor="FAF9F9"><b>Onemogućeni</b></td>
<td> </td>
<td> </td>
<td> </td>
</tr>
<tr>
<td bgcolor="FAF9F9"><b>Obustavljena</b></td>
<td> </td>
<td> </td>
<td><center>✔</center></td>
</tr>
<tr>
<td bgcolor="FAF9F9"><b>Zaustavi</b></td>
<td> </td>
<td> </td>
<td><center>✔</center></td>
</tr>
<tr>
<td bgcolor="FAF9F9"><b>Nije uspjelo ažuriranje servisa</b></td>
<td> </td>
<td> </td>
<td> </td>
</tr>
<tr>
<td bgcolor="FAF9F9"><b>DisableFailed</b></td>
<td> </td>
<td> </td>
<td> </td>
</tr>
<tr>
<td bgcolor="FAF9F9"><b>EnableFailed</b></td>
<td> </td>
<td> </td>
<td> </td>
</tr>
<tr>
<td bgcolor="FAF9F9"><b>StartFailed<br/>
StopFailed<br/>
RestartFailed</b></td>
<td> </td>
<td><center>✔</center></td>
<td> </td>
</tr>
<tr>
<td bgcolor="FAF9F9"><b>SuspendedFailed<br/>
ResumeFailed</b></td>
<td> </td>
<td> </td>
<td> </td>
</tr>
<tr>
<td bgcolor="FAF9F9"><b>CreatedFailed<br/>
RestoreFailed<br/></b></td>
<td> </td>
<td> </td>
<td> </td>
</tr>
<tr>
<td bgcolor="FAF9F9"><b>ConfigUpdateFailed</b></td>
<td> </td>
<td><center>✔</center></td>
<td> </td>
</tr>
<tr>
<td bgcolor="FAF9F9"><b>ScaleFailed</b></td>
<td><center>✔</center></td>
<td> </td>
<td> </td>
</tr>
</table>

## <a name="see-also"></a>Vidi također
- [BizTalk servisa: Azure pomoću portala za klasični dodjele resursa](http://go.microsoft.com/fwlink/p/?LinkID=302280)<br/>
- [BizTalk servisa: Kartica nadzorne ploče, Monitor i skaliranje](http://go.microsoft.com/fwlink/p/?LinkID=302281)<br/>
- [Servisi za BizTalk: Za razvojne inženjere, Basic, standardna i Premium izdanja grafikona](http://go.microsoft.com/fwlink/p/?LinkID=302279)<br/>
- [BizTalk servisa: Sigurnosno kopiranje i vraćanje](http://go.microsoft.com/fwlink/p/?LinkID=329873)<br/>
- [BizTalk usluge: ograničavanje](http://go.microsoft.com/fwlink/p/?LinkID=302282)<br/>
- [BizTalk servisa: Naziv izdavača i izdavatelj ključ](http://go.microsoft.com/fwlink/p/?LinkID=303941)<br/>
- [Kako pokrenuti pomoću Azure BizTalk Services SDK](http://go.microsoft.com/fwlink/p/?LinkID=302335)


 
