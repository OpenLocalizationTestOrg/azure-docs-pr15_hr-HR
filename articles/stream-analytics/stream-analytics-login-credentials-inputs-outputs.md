<properties 
    pageTitle="Strujanje Analytics: Rotiranje u zapisnik vjerodajnice za unose i izlaze | Microsoft Azure" 
    description="Saznajte kako ažurirati vjerodajnice za strujanje analize unosa i izlaza."
    keywords="vjerodajnice za prijavu"
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
    ms.workload="data-services" 
    ms.date="09/26/2016" 
    ms.author="jeffstok"/>

#<a name="rotate-login-credentials-for-inputs-and-outputs-in-stream-analytics-jobs"></a>Rotiranje vjerodajnice za prijavu za unose i izlaze u strujanje analize poslove

##<a name="abstract"></a>Kratki pregled
Azure strujanje analize danas ne dopušta zamjene vjerodajnice na ulazni i izlazni dok se izvodi posao.

Tijekom analize strujanje Azure podržava nastavljanje posla iz zadnjeg izlaza, ne možemo željeli zajedničko korištenje cijelog postupka za minimiziranje kašnjenja između na zaustavljanja i pokretanje zadatka i rotiranje vjerodajnice za prijavu.

##<a name="part-1---prepare-the-new-set-of-credentials"></a>Dio 1 – Priprema novi skup vjerodajnica:
Odnosi se na sljedeće unose/izlaze je taj dio:

* Spremište blobova platforme
* Događaj koncentratora
* SQL baze podataka
* Spremište tablica

Za ostale ulaza/izlaza, nastavite s dio 2.

###<a name="blob-storagetable-storage"></a>Spremište blobova platforme za pohranu i tablice
1.  Idite na datotečnih nastavaka prostora za pohranu na portalu za upravljanje Azure:  
![graphic1][graphic1]
2.  Pronađite koristi vaš posao prostora za pohranu i idite u nju:  
![graphic2][graphic2]
3.  Kliknite naredbu upravljanje pristupnih tipki:  
![graphic3][graphic3]
4.  Između primarni ključ Access i sekundarne pristupni ključ, **odaberite onaj koji se ne koristi vaš posao**.
5.  Kliknite Obnovi:  
![graphic4][graphic4]
6.  Kopirajte upravo generirani ključ:  
![graphic5][graphic5]
7.  I dalje 2.dio.

###<a name="event-hubs"></a>Koncentratorima događaja
1.  Idite na servis Bus proširenje portala za upravljanje Azure:  
![graphic6][graphic6]
2.  Pronađite polje naziva servisa Bus koristi vaš posao te prijeđite u nju.  
![graphic7][graphic7]
3.  Ako vaš posao koristi zajednički pristup pravila na polje naziva za Bus servisa, prijelaz koraku 6  
4.  Idite na karticu koncentratora događaja:  
![graphic8][graphic8]
5.  Pronađite središtu događaja koji se koriste vaš posao, a zatim prijeđite na nju:  
![graphic9][graphic9]
6.  Idite na karticu konfiguracija:  
![graphic10][graphic10]
7.  Naziv pravila padajući popis, pronađite pravila zajednički pristup koristi vaš posao:  
![graphic11][graphic11]
8.  Između primarni ključ i sekundarne ključ, **odaberite onaj koji se ne koristi vaš posao**.  
9.  Kliknite Obnovi:  
![graphic12][graphic12]
10. Kopirajte upravo generirani ključ:  
![graphic13][graphic13]
11. I dalje dio 2.  

###<a name="sql-database"></a>SQL baze podataka

>[AZURE.NOTE] Napomena: morat ćete povezati sa servisom SQL baze podataka. Ne možemo ćete pokazati kako to učiniti pomoću sučelje za upravljanje portala za upravljanje Azure, ali odlučite koristiti neke klijentsko alata kao što je SQL Server Management Studio kao i.

1.  Idite na proširenje baze podataka SQL Azure upravljanja portala:  
![graphic14][graphic14]
2.  Pronađite koristi vezu zadatak i **kliknite na poslužitelju** u istom retku SQL baze podataka:  
![graphic15][graphic15]
3.  Kliknite Upravljanje naredbu:  
![graphic16][graphic16]
4.  Vrsta matrica baze podataka:  
![graphic17][graphic17]
5.  Upišite svoje korisničko ime, lozinku i kliknite u zapisnik na:  
![graphic18][graphic18]
6.  Kliknite novi upit:  
![graphic19][graphic19]
7.  Upišite sljedeći upit zamjenjuju < login_name > svoje korisničko ime i zamjene <enterStrongPasswordHere> pomoću nove lozinke:  
`CREATE LOGIN <login_name> WITH PASSWORD = '<enterStrongPasswordHere>'`
8.  Kliknite Pokreni:  
![graphic20][graphic20]
9.  Vratite se u koraku 2 i ovaj put kliknite bazu podataka:  
![graphic21][graphic21]
10. Kliknite Upravljanje naredbu:  
![graphic22][graphic22]
11. Upišite svoje korisničko ime, lozinku i kliknite u prijave:  
![graphic23][graphic23]
12. Kliknite novi upit:  
![graphic24][graphic24]
13. U sljedeći upit zamjena < korisničko_ime > naziv koji želite odrediti ovaj prijava u kontekstu Ova baza podataka (možete omogućiti jednaku vrijednost dao za < login_name >, na primjer) i zamjena < login_name > novo korisničko ime upišite:  
`CREATE USER <user_name> FROM LOGIN <login_name>`
14. Kliknite Pokreni:  
![graphic25][graphic25]
15. Sada mora sadržavati novog korisnika s istim uloge i ovlasti imali izvorne korisniku.
16. I dalje 2.dio.

##<a name="part-2-stopping-the-stream-analytics-job"></a>Dio 2: Prekid posao strujanje Analytics
1.  Idite na nastavak strujanje analize portala za upravljanje Azure:  
![graphic26][graphic26]
2.  Pronađite svoj posao te prijeđite u nju.  
![graphic27][graphic27]
3.  Idite na karticu unosa ili karticu izlaze na temelju hoće li se rotiranje vjerodajnice na ulazni ili na izlaz.  
![graphic28][graphic28]
4.  Kliknite naredbu tabulatora i provjerili prestao posao:  
![graphic29][graphic29] čekanja za posao da biste zaustavili.
5.  Pronađite unos/izlaz želite rotirati vjerodajnice na, a zatim idite na nju:  
![graphic30][graphic30]
6.  Nakon toga dijela 3.

##<a name="part-3-editing-the-credentials-on-the-stream-analytics-job"></a>Dio 3: Uređivanje vjerodajnice na poslu strujanje Analytics

###<a name="blob-storagetable-storage"></a>Spremište blobova platforme za pohranu i tablice
1.  Pronađite polje za pohranu ključ za račun i zalijepite ga upravo generirani ključ:  
![graphic31][graphic31]
2.  Kliknite naredbu Spremi i potvrdite spremanjem datoteke:  
![graphic32][graphic32]
3.  Testiranje veze će se automatski pokrenuti kada spremite promjene, odnosno provjerite je li uspješno prošlo.
4.  Nakon toga dijela 4.

###<a name="event-hubs"></a>Događaj koncentratora
1.  Pronađite polje događaj koncentrator pravila ključ i zalijepite ga upravo generirani ključ:  
![graphic33][graphic33]
2.  Kliknite naredbu Spremi i potvrdite spremanjem datoteke:  
![graphic34][graphic34]
3.  Testiranje veze će se automatski pokrenuti kada spremite promjene, provjerite je li uspješno prošao.
4.  Nakon toga dijela 4.

###<a name="power-bi"></a>Power BI
1.  Kliknite autorizacije obnavljanje:  
* ![graphic35][graphic35]
* Prikazat će se sljedeće potvrdu:  
* ![graphic36][graphic36]
2.  Kliknite naredbu Spremi i potvrdite spremanjem datoteke:  
![graphic37][graphic37]
3.  Testiranje veze će se automatski pokrenuti kada spremite promjene, provjerite jeste li ga je uspješno prošao.
4.  Nakon toga dijela 4.

###<a name="sql-database"></a>SQL baze podataka
1.  Pronađite polja korisničko ime i lozinku i zalijepite novostvorenu skup vjerodajnica:  
![graphic38][graphic38]
2.  Kliknite naredbu Spremi i potvrdite spremanjem datoteke:  
![graphic39][graphic39]
3.  Testiranje veze će se automatski pokrenuti kada spremite promjene, provjerite je li uspješno prošao.  
4.  Nakon toga dijela 4.

##<a name="part-4-starting-your-job-from-last-stopped-time"></a>Dio 4: Vaš posao počevši od posljednjeg zaustavljena
1.  Napustite Input/Output:  
![graphic40][graphic40]
2.  Kliknite naredbu Pokreni:  
![graphic41][graphic41]
3.  Odaberite vrijeme zadnje zaustaviti, a zatim kliknite u redu:  
 ![graphic42][graphic42]
4.  Nakon toga dijela 5.  

##<a name="part-5-removing-the-old-set-of-credentials"></a>Dio 5: Uklanjanje stare skup vjerodajnica
Odnosi se na sljedeće unose/izlaze je taj dio:
* Spremište blobova platforme
* Događaj koncentratora
* SQL baze podataka
* Spremište tablica

###<a name="blob-storagetable-storage"></a>Spremište blobova platforme za pohranu i tablice
Ponovite dio 1 za tipki pristupa koja je ranije koristio posla obnoviti sada nekorištenih tipkovni prečac.

###<a name="event-hubs"></a>Koncentratorima događaja
Ponovite dio 1 za ključ koji je ranije koristio posla obnoviti sada Nekorišteni ključ.

###<a name="sql-database"></a>SQL baze podataka
1.  Vratite se u prozor upit iz dijela 1 korak 7 pa upišite sljedeći upit zamjena < previous_login_name > korisničko ime koje je ranije koristio vaš posao:  
`DROP LOGIN <previous_login_name>`  
2.  Kliknite Pokreni:  
    ![graphic43][graphic43]  

Trebali biste dobiti sljedeće potvrde: 

    Command(s) completed successfully.

## <a name="get-help"></a>Zatražite pomoć
Dodatnu pomoć, pokušajte našem [forumu Azure strujanje Analytics](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics)

## <a name="next-steps"></a>Daljnji koraci

- [Uvod u Azure strujanje Analytics](stream-analytics-introduction.md)
- [Prvi koraci pri korištenju Azure strujanje Analytics](stream-analytics-get-started.md)
- [Promjena veličine Azure strujanje analize poslova](stream-analytics-scale-jobs.md)
- [Azure strujanje analize upita jezična referenca](https://msdn.microsoft.com/library/azure/dn834998.aspx)
- [Azure strujanje analize upravljanje REST API Reference](https://msdn.microsoft.com/library/azure/dn835031.aspx)


[graphic1]: ./media/stream-analytics-login-credentials-inputs-outputs/1-stream-analytics-login-credentials-inputs-outputs.png
[graphic2]: ./media/stream-analytics-login-credentials-inputs-outputs/2-stream-analytics-login-credentials-inputs-outputs.png
[graphic3]: ./media/stream-analytics-login-credentials-inputs-outputs/3-stream-analytics-login-credentials-inputs-outputs.png
[graphic4]: ./media/stream-analytics-login-credentials-inputs-outputs/4-stream-analytics-login-credentials-inputs-outputs.png
[graphic5]: ./media/stream-analytics-login-credentials-inputs-outputs/5-stream-analytics-login-credentials-inputs-outputs.png
[graphic6]: ./media/stream-analytics-login-credentials-inputs-outputs/6-stream-analytics-login-credentials-inputs-outputs.png
[graphic7]: ./media/stream-analytics-login-credentials-inputs-outputs/7-stream-analytics-login-credentials-inputs-outputs.png
[graphic8]: ./media/stream-analytics-login-credentials-inputs-outputs/8-stream-analytics-login-credentials-inputs-outputs.png
[graphic9]: ./media/stream-analytics-login-credentials-inputs-outputs/9-stream-analytics-login-credentials-inputs-outputs.png
[graphic10]: ./media/stream-analytics-login-credentials-inputs-outputs/10-stream-analytics-login-credentials-inputs-outputs.png
[graphic11]: ./media/stream-analytics-login-credentials-inputs-outputs/11-stream-analytics-login-credentials-inputs-outputs.png
[graphic12]: ./media/stream-analytics-login-credentials-inputs-outputs/12-stream-analytics-login-credentials-inputs-outputs.png
[graphic13]: ./media/stream-analytics-login-credentials-inputs-outputs/13-stream-analytics-login-credentials-inputs-outputs.png
[graphic14]: ./media/stream-analytics-login-credentials-inputs-outputs/14-stream-analytics-login-credentials-inputs-outputs.png
[graphic15]: ./media/stream-analytics-login-credentials-inputs-outputs/15-stream-analytics-login-credentials-inputs-outputs.png
[graphic16]: ./media/stream-analytics-login-credentials-inputs-outputs/16-stream-analytics-login-credentials-inputs-outputs.png
[graphic17]: ./media/stream-analytics-login-credentials-inputs-outputs/17-stream-analytics-login-credentials-inputs-outputs.png
[graphic18]: ./media/stream-analytics-login-credentials-inputs-outputs/18-stream-analytics-login-credentials-inputs-outputs.png
[graphic19]: ./media/stream-analytics-login-credentials-inputs-outputs/19-stream-analytics-login-credentials-inputs-outputs.png
[graphic20]: ./media/stream-analytics-login-credentials-inputs-outputs/20-stream-analytics-login-credentials-inputs-outputs.png
[graphic21]: ./media/stream-analytics-login-credentials-inputs-outputs/21-stream-analytics-login-credentials-inputs-outputs.png
[graphic22]: ./media/stream-analytics-login-credentials-inputs-outputs/22-stream-analytics-login-credentials-inputs-outputs.png
[graphic23]: ./media/stream-analytics-login-credentials-inputs-outputs/23-stream-analytics-login-credentials-inputs-outputs.png
[graphic24]: ./media/stream-analytics-login-credentials-inputs-outputs/24-stream-analytics-login-credentials-inputs-outputs.png
[graphic25]: ./media/stream-analytics-login-credentials-inputs-outputs/25-stream-analytics-login-credentials-inputs-outputs.png
[graphic26]: ./media/stream-analytics-login-credentials-inputs-outputs/26-stream-analytics-login-credentials-inputs-outputs.png
[graphic27]: ./media/stream-analytics-login-credentials-inputs-outputs/27-stream-analytics-login-credentials-inputs-outputs.png
[graphic28]: ./media/stream-analytics-login-credentials-inputs-outputs/28-stream-analytics-login-credentials-inputs-outputs.png
[graphic29]: ./media/stream-analytics-login-credentials-inputs-outputs/29-stream-analytics-login-credentials-inputs-outputs.png
[graphic30]: ./media/stream-analytics-login-credentials-inputs-outputs/30-stream-analytics-login-credentials-inputs-outputs.png
[graphic31]: ./media/stream-analytics-login-credentials-inputs-outputs/31-stream-analytics-login-credentials-inputs-outputs.png
[graphic32]: ./media/stream-analytics-login-credentials-inputs-outputs/32-stream-analytics-login-credentials-inputs-outputs.png
[graphic33]: ./media/stream-analytics-login-credentials-inputs-outputs/33-stream-analytics-login-credentials-inputs-outputs.png
[graphic34]: ./media/stream-analytics-login-credentials-inputs-outputs/34-stream-analytics-login-credentials-inputs-outputs.png
[graphic35]: ./media/stream-analytics-login-credentials-inputs-outputs/35-stream-analytics-login-credentials-inputs-outputs.png
[graphic36]: ./media/stream-analytics-login-credentials-inputs-outputs/36-stream-analytics-login-credentials-inputs-outputs.png
[graphic37]: ./media/stream-analytics-login-credentials-inputs-outputs/37-stream-analytics-login-credentials-inputs-outputs.png
[graphic38]: ./media/stream-analytics-login-credentials-inputs-outputs/38-stream-analytics-login-credentials-inputs-outputs.png
[graphic39]: ./media/stream-analytics-login-credentials-inputs-outputs/39-stream-analytics-login-credentials-inputs-outputs.png
[graphic40]: ./media/stream-analytics-login-credentials-inputs-outputs/40-stream-analytics-login-credentials-inputs-outputs.png
[graphic41]: ./media/stream-analytics-login-credentials-inputs-outputs/41-stream-analytics-login-credentials-inputs-outputs.png
[graphic42]: ./media/stream-analytics-login-credentials-inputs-outputs/42-stream-analytics-login-credentials-inputs-outputs.png
[graphic43]: ./media/stream-analytics-login-credentials-inputs-outputs/43-stream-analytics-login-credentials-inputs-outputs.png
 
