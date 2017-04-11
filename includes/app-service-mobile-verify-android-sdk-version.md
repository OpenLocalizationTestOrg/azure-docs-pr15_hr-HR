Zbog tijeku razvoj Android SDK verzija instalirana na Android Studio možda odgovara verziji kod. Android SDK navedeni u ovom ćete praktičnom vodiču je verzija 23, najnoviji vrijeme pisanja. Broj verzije mogu povećati dok prikazuju se nova izdanja SDK pa preporučujemo korištenje najnovije verzije.

Dva simptomi ne podudaraju se verzije su:

1. Pri sastavljanju i ponovno stvaranje projekta, mogla bi vam se poruka o pogrešci Gradle kao što su "**nije pronađeno cilj Google Inc.:Google APIs:n**".

2. Standardni Android objekata u kodu koje treba riješiti na temelju `import` izjave generiranje poruke o pogreškama.

Ako se pojavi nešto od sljedećeg, verziju Android SDK instalirane u Android Studio bi možda bile podudarne cilj SDK preuzete projekta.  Da biste provjerili verziju, provjerite sljedeće promjene:


1. Na Android Studio, kliknite **Alati** => **Android** => **Upravitelj SDK**. Ako niste instalirali najnoviju verziju platforme SDK, kliknite da biste ga instalirali. Zabilježite broj verzije.

2. Na kartici programa Project Explorer, u odjeljku **Skripta Gradle**, otvorite datoteku **build.gradle (modeule: aplikacija)**. Provjerite je li se postavljaju na najnoviju verziju SDK instaliran **compileSdkVersion** i **buildToolsVersion** . Oznake može izgledati ovako:
 
            compileSdkVersion 'Google Inc.:Google APIs:23'
            buildToolsVersion "23.0.2"
    
3. U Android Studio Project Explorer desnom tipkom miša kliknite čvor projekta, odaberite **Svojstva**i u lijevom stupcu odaberite **sa sustavom Android**. Provjerite je li **Cilj sastavljanje projekta** postavljena na istu verziju SDK kao **targetSdkVersion**.

4. U Android Studio manifesta datoteka se više ne koristi za određivanje ciljnog SDK i Minimalna verzija SDK, za razliku od u slučaju Eclipse.
