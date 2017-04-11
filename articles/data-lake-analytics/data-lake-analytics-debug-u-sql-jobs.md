<properties 
   pageTitle="Ispravljanje pogrešaka U SQL poslove | Microsoft Azure" 
   description="Saznajte kako ispraviti pogreške U SQL nije uspjelo vrh pomoću Visual Studio. " 
   services="data-lake-analytics" 
   documentationCenter="" 
   authors="mumian" 
   manager="jhubbard" 
   editor="cgronlun"/>
 
<tags
   ms.service="data-lake-analytics"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data" 
   ms.date="09/02/2016"
   ms.author="jgao"/>



#<a name="debug-c-code-in-u-sql-for-data-lake-analytics-jobs"></a>C# kod u U SQL za poslove analize podataka Lake za ispravljanje pogrešaka 

Saznajte kako koristiti Azure podataka Lake Visual Studio tools za ispravljanje pogrešaka neuspjele poslove U SQL zbog programskih pogrešaka unutar kod korisnika. 

Alat za Visual Studio omogućuje preuzimanje kompilirani kod i vrh potrebne podatke iz klaster praćenja i ispravljanja pogrešaka neuspjele poslove.

Sustavi velikih skupova podataka obično omogućuju proširivanje modela pomoću jezika Java, C#, Python, itd. Mnogim te sustavima pružaju ograničeni runtime ispravljanje pogrešaka informacija koje je teško pogreške pri izvođenju u prilagođeni kod za ispravljanje pogrešaka. U sklopu najnovije Visual Studio tools značajku pod nazivom "Nije uspjela vrh ispravljanje pogrešaka". Koristite ovu značajku, možete preuzeti izvođenja podataka iz Azure za lokalni radne stanice tako da možete ispraviti pogreške nije uspjelo prilagođene C# kod koristi iste izvođenja i točan unos podataka iz oblaka.  Kada na probleme, možete ponovno pokrenuti promijenjenih kod u Azure Alati.

Video prezentacije ovu značajku, potražite u članku [prilagođeni kod Azure podataka Lake Analytics za ispravljanje pogrešaka](https://mix.office.com/watch/1bt17ibztohcb).

>[AZURE.NOTE] Visual Studio možda "smrzavanje" ili pasti ako nemate sljedeća dva prozora nadogradnje: [Microsoft Visual C++ 2015 slobodnu distribuciju ažuriranje 2](https://www.microsoft.com/download/details.aspx?id=51682), [Univerzalni C Runtime za Windows](https://www.microsoft.com/download/details.aspx?id=50410&wa=wsignin1.0).


##<a name="prerequisites"></a>Preduvjeti
-   Razmijenjeno je putem u članku [Početak rada](data-lake-analytics-data-lake-tools-get-started.md) .

## <a name="create-and-configure-debug-projects"></a>Stvaranje i konfiguriranje projekata za ispravljanje pogrešaka

Kada otvorite nije uspjelo posla u alatu Visual Studio podataka Lake, dobit ćete upozorenje. Na detaljne podatke o pogreškama će biti prikazani na kartici pogrešaka i upozorenja žutu traku pri vrhu stranice u prozoru. 

![Azure podataka U u analize Lake-SQL ispravljanje pogrešaka visual studio preuzimanje vrh](./media/data-lake-analytics-debug-u-sql-jobs/data-lake-analytics-download-vertex.png)

**Da biste preuzeli vrh i stvaranje rješenja za ispravljanje pogrešaka**

1.  Otvorite nije uspjelo posla U SQL u Visual Studio.
2.  Kliknite **Preuzmi** da biste preuzeli sve potrebne resurse i unos strujanja. Kliknite **ponovno pokušajte** neuspješne preuzimanja.
3.  Kada preuzimanje Završi da biste stvorili lokalnu ispravljanje pogrešaka projekta, kliknite **Otvori** . Stvorit će se novi rješenja za Visual Studio naziva **VertexDebug** sa prazni projekt pod nazivom **LocalVertexHost** .

Ako korisnički definirane operatori koriste u U SQL kodu iza (Script.usql.cs), stvaranje projekta predmete biblioteke C# kod operatori korisnički definirane, a projekt obuhvatiti VertexDebug rješenja.

Ako ste registrirali .dll sklopova u bazu podataka Lake analize, morate dodati izvorni kod u skupine VertexDebug rješenja.
 
Ako ste stvorili zasebnu C# predmete biblioteku U SQL koda i registrirani .dll sklopova u bazu podataka Lake analize, morate dodati izvor C# projekta u skupine VertexDebug rješenje.

U nekim slučajevima rijetko pomoću operatora korisnički definirano u U SQL kodu iza (Script.usql.cs) datoteke u izvornom rješenja. Ako želite da bi funkcionirao, morate je stvaranje C# biblioteke koja sadrži izvorni kod i promijenite naziv skupa na onu koja je registrirana u klasteru. Možete dobiti naziv skupa registrirana u klasteru potvrđivanjem skriptu koja imate izvodi u klasteru. Možete učiniti tako da otvorite zadatak U SQL i kliknite "skripta" u oknu zadatka. 

**Konfiguriranje rješenja**

1.  Iz programa explorer rješenja, desnom tipkom miša kliknite C# projekta koji ste upravo stvorili, a zatim **Svojstva**.
2.  Postavite put izlaz kao LocalVertexHost projekt radi put direktorija. Možete dobiti LocalVertexHost projekt radi direktorija put do LocalVertexHost svojstva.
3.  Stvaranje projekta C# da biste postavili .pdb datoteke u projekt LocalVertexHost rad direktorija ili možete ručno kopirati datoteku .pdb u ovu mapu.
4.  U odjeljku **Postavke iznimke**potvrdite uobičajenih iznimke prilikom izvođenja jezik:

![Azure podataka U u analize Lake-SQL postavka visual studio ispravljanje pogrešaka](./media/data-lake-analytics-debug-u-sql-jobs/data-lake-analytics-clr-exception-setting.png)
 
##<a name="debug-the-job"></a>Ispravljanje pogrešaka posla

Nakon što ste stvorili rješenje ispravljanje pogrešaka tako da preuzmete vrh i ste konfigurirali u okruženju, možete pokrenuti ispravljanje pogrešaka U SQL kod.

1.  Iz programa Explorer rješenja, desnom tipkom miša kliknite projekt **LocalVertexHost** koji ste upravo stvorili, pokažite za **ispravljanje pogrešaka**, a zatim **pokrenite novu instancu**. Na LocalVertexHost mora biti postavljena kao projekt prilikom pokretanja. Prvi put koji možete zanemariti može se pojaviti sljedeća poruka. To može potrajati i do jedne minute da biste otvorili zaslon za ispravljanje pogrešaka.
 
    ![Upozorenje o za visual studio ispravljanje pogrešaka za Azure podataka U u analize Lake-SQL](./media/data-lake-analytics-debug-u-sql-jobs/data-lake-analytics-visual-studio-u-sql-debug-warning.png)

4.  Koristite Visual Studio temelje sučelje za ispravljanje pogrešaka (pogledajte varijable, itd.) da biste riješili problem. 
5.  Nakon što ste identificirali problem, ispravite kod, a zatim ponovno stvaranje projekta C# prije ga ponovno dok se sve probleme riješi. Nakon za ispravljanje pogrešaka uspješno dovršenog, u izlaznom prozoru prikazuje se sljedeća poruka 

        The Program ‘LocalVertexHost.exe’ has exited with code 0 (0x0).
 
##<a name="resubmit-the-job"></a>Ponovno pošaljite posao

Nakon što dovršite ispravljanje pogrešaka U SQL kodu, možete ponovno nije uspjelo posao.

1. Registrirajte se novi skupovi .dll ADLA bazi podataka.

    1.  Iz programa Explorer Explorer/oblaka poslužitelja u alatu Visual Studio za podataka Lake, proširite čvor **baze podataka** 
    2.  Desnom tipkom miša kliknite skupovi za sklopova Register. 
    3.  Registrirajte se novi skupovi .dll ADLA bazi podataka.
 
2.  Ili kopirajte C# kod script.usql.cs – C# kod iza datoteke.
3.  Pošaljite vaš posao.

##<a name="next-steps"></a>Daljnji koraci

- [Praktični vodič: Početak rada s jezikom Azure podataka Lake analize U-SQL](data-lake-analytics-u-sql-get-started.md)
- [Praktični vodič: razvoj U SQL skripte pomoću alata za Lake podataka za Visual Studio](data-lake-analytics-data-lake-tools-get-started.md)
- [Razvoj U SQL korisnički definirane operatori za Azure podataka Lake analize poslove](data-lake-analytics-u-sql-develop-user-defined-operators.md)

