<properties 
    pageTitle="Profiliranje neki servis u Oblaku lokalno u Emulator računalnim | Microsoft Azure" 
    services="cloud-services"
    description="Istraživanje problema s performansama na servise u oblaku s profiler Visual Studio" 
    documentationCenter=""
    authors="TomArcher" 
    manager="douge" 
    editor=""
    tags="" 
    />

<tags 
    ms.service="cloud-services" 
    ms.workload="na" 
    ms.tgt_pltfrm="na" 
    ms.devlang="multiple" 
    ms.topic="article" 
    ms.date="07/30/2016" 
    ms.author="tarcher"/>

# <a name="testing-the-performance-of-a-cloud-service-locally-in-the-azure-compute-emulator-using-the-visual-studio-profiler"></a>Lokalno testiranju performanse servis u Oblaku u Emulator Azure računalnim pomoću Profiler Visual Studio

Raznih alata i tehnike dostupni su za testiranje performanse servise u oblaku.
Kad objavite servis u oblaku za Azure, možete imati Visual Studio prikupljanje profiliranja podataka, a zatim analizirati lokalno, kao što je opisano u [Profiliranje programa Azure][1].
Možete koristiti Dijagnostika za praćenje raznih mjerača performansi, kao što je opisano u [mjerača performansi za korištenje u Azure][2].
Možda želite profila aplikacije lokalno u emulator računalnim prije no što implementirate u oblak.

U ovom se članku opisuje procesora uzorkovanje način Profiliranje, koje je moguće izvršiti lokalno u emulator. Stvaranje uzoraka procesora je način Profiliranje koji nije vrlo većih. U intervalu određenu uzorkovanje na profiler stvara snimku stog poziva. Podaci se prikupljenih tijekom određenog vremenskog razdoblja, a prikazana u izvješću. Ova metoda Profiliranje ima da biste naznačili mjesto u aplikaciji za što intenzivno većinu posla procesora koji se završi.  Tako ćete dobiti prilike radi fokusiranja na "tipkovni put" gdje aplikacija je trošite najbolje vrijeme.



## <a name="1-configure-visual-studio-for-profiling"></a>1: Konfiguriranje Visual Studio za Profiliranje

Najprije postoji nekoliko mogućnosti konfiguracije Visual Studio koje mogu biti korisne prilikom Profiliranje. Da biste uvid u profiliranja izvješća, morat ćete simbola (.pdb datoteke) za svoju aplikaciju i i simbola za biblioteke sustava. Ćete htjeti Provjerite upućuje poslužitelje dostupnih simbola. Da biste to učinili, na izborniku **Alati** u Visual Studio, odaberite **Mogućnosti**, a zatim odaberite **značajka ispravljanja pogrešaka**, zatim **simbole**. Provjerite nalazi li se Microsoftovim poslužiteljima simbol u odjeljku **Simbol mjesta datoteka (.pdb)**.  Možete referencirati i http://referencesource.microsoft.com/symbols, možda imate dodatne simbol datoteke.

![Simbol mogućnosti][4]

Po želji možete pojednostavniti izvješća koji se profiler generira postavljanjem samo moje kodom. S samo moje kodom omogućena, funkcija poziv snop su pojednostavnjeni tako da pozive, internog biblioteke i .NET Framework skriveni su od tih izvješća. Na izborniku **Alati** odaberite **Mogućnosti**. Zatim proširite čvor **Performanse Alati** , a zatim odaberite **Općenito**. Potvrdite okvir **Omogući samo moje**koda za profiler izvješća.

![Samo moje kodom mogućnosti][17]

Koristite ove upute s postojeći projekt ili novi projekt.  Ako stvorite novi projekt možete pokušati tehnike opisanim, odaberite projekta C# **Azure u Oblaku** pa odaberite **Web uloge** i **Uloge suradnika**.

![Azure uloge projekta u Oblaku][5]

Na primjer svrhe, dodati neke kod u projekt koji zauzima mnogo vremena i pokazuje poteškoće neke očite performansama. Projekt ulogu suradnika, na primjer, dodati sljedeći kod:

    public class Concatenator
    {
        public static string Concatenate(int number)
        {
            int count;
            string s = "";
            for (count = 0; count < number; count++)
            {
                s += "\n" + count.ToString();
            }
            return s;
        }
    }

Pozivom kod metoda RunAsync u predmete RoleEntryPoint izvedena ulogu suradnika. (Zanemarili upozorenje o načinu pokretanja sinkronizirano.)

        private async Task RunAsync(CancellationToken cancellationToken)
        {
            // TODO: Replace the following with your own logic.
            while (!cancellationToken.IsCancellationRequested)
            {
                Trace.TraceInformation("Working");
                Concatenator.Concatenate(10000);
            }
        }

Stvaranje i pokrenuti servis u oblaku lokalno bez pogrešaka (Ctrl + F5) u konfiguraciji rješenje postavite **izdanje**. Time se osigurava da sve datoteke i mape stvaraju za pokretanje aplikacije lokalno i osigurava da su sve Emulatora rada. Provjerite je li vaša uloga suradnika sustavom početi Emulator UI izračunati na programskoj traci.

## <a name="2-attach-to-a-process"></a>2: priložiti procesa

Umjesto Profiliranje aplikaciju, počevši od u Visual Studio 2010 IDE, morate pridružiti na profiler je u tijeku. 

Da biste priložili na profiler procesa, na izborniku **Analiza** odaberite **Profiler** i **Prilaganje/Odvoji**.

![Prilaganje mogućnost profila][6]

Pronađite postupka WaWorkerHost.exe za ulogu suradnika.

![Postupak WaWorkerHost][7]

Ako je mape programu project na mrežnom pogonu, u profiler će zatražiti drugo mjesto za spremanje profiliranja izvješća.

 Možete priložiti web uloga prema priložite WaIISHost.exe.
Ako postoji više radnih procesa uloga u aplikaciji, morate koristiti u processID da biste ih razlikovali. Upit možete poslati na processID programski tako da pristupite objekt postupak. Ako, na primjer, ako dodate kod način pokretanja klase RoleEntryPoint dobiven u ulozi, možete pogledati prijava izračunati Emulator korisničkog Sučelja informacije koje postupak možete povezati.

    var process = System.Diagnostics.Process.GetCurrentProcess();
    var message = String.Format("Process ID: {0}", process.Id);
    Trace.WriteLine(message, "Information");

Da biste pogledali evidenciju, pokrenite Emulator UI izračunati.

![Pokretanje Emulator računalnim korisničkog Sučelja][8]

Otvorite prozor za konzole tempiranja uloga zapisnika u Emulator korisničko Sučelje za izračun tako da kliknete na naslovnoj traci prozora konzole. Vidjet ćete ID procesa u zapisnik.

![ID za prikaz procesa][9]

Jedan koju ste priložili poduzeti korake u korisničkom Sučelju svoje aplikacije (Ako je potrebno) ponovno proizvesti scenarija.

Ako želite da biste prestali Profiliranje, odaberite vezu za **Zaustavljanje Profiliranje** .

![Zaustavljanje Profiliranje mogućnost][10]

## <a name="3-view-performance-reports"></a>3: prikaz izvješća o izvedbi

Prikazat će se izvješće o performansama aplikacije.

Sada na profiler zaustavlja izvršavanja, sprema podatke u datoteci .vsp i prikazuje izvješće koje prikazuje analizu tih podataka.

![Profiler izvješća][11]


Ako vidite String.wstrcpy tipkovni putu, kliknite samo moje kodom da biste promijenili prikaz samo kod korisnika.  Ako vidite String.Concat, pokušajte pritisnuti gumb Prikaži sve kod.

Trebali biste vidjeti Concatenate metodu i String.Concat zauzima velik dio vremena izvođenja.

![Analiza izvješća][12]

Ako ste dodali Ulančavanje kod niza u ovom članku, vidjet ćete upozorenje na popisu zadataka za to. Može se prikazati upozorenje da postoji viškom količinu smeća, koji je zbog broj nizovi koji su stvorili i način razmještaja.

![Performanse upozorenja][14]

## <a name="4-make-changes-and-compare-performance"></a>4: Unesite promjene, a zatim usporedite performansi

Možete usporediti i performanse prije i nakon svake promjene kod.  Prekinuli postupak izvodi i urediti kod da biste zamijenili operaciju spajanja niza koristi StringBuilder:

    public static string Concatenate(int number)
    {
        int count;
        System.Text.StringBuilder builder = new System.Text.StringBuilder("");
        for (count = 0; count < number; count++)
        {
             builder.Append("\n" + count.ToString());
        }
        return builder.ToString();
    }

Učinite Pokreni drugi performanse, a zatim usporedite performanse. U programu Explorer performanse ako se pokreće se u istu sesiju ne možete samo odaberete oba izvješća, otvorite izbornički prečac, a odaberite **Usporedi izvješća o izvedbi**. Ako želite usporediti s Pokreni u drugoj sesiji performanse, otvorite izbornik **Analiza** pa odaberite **Usporedite izvješća o izvedbi**. Navedite obje datoteke u dijaloškom okviru koji će se prikazati.

![Usporedba mogućnost izvješća performansi][15]

Izvješća istaknute su razlike između dva pokretanja.

![Usporedba izvješća][16]

Čestitamo! Koje ste navikli rada s na profiler.

## <a name="troubleshooting"></a>Otklanjanje poteškoća

- Provjerite je li se Profiliranje izdanje Sastavi i pokretanje bez pogrešaka.

- Ako je mogućnost Dodaj privitak/Odvoji nije omogućena na izborniku Profiler, pokrenite čarobnjak performansi.

- Prikaz statusa aplikacije pomoću Emulator UI izračunati. 

- Ako naiđete na probleme pokretanje aplikacije u na emulator ili prilaganje profiler, isključiti dolje emulator računalnim, a zatim ga ponovno pokrenite. Ako time ne riješite problem, pokušajte ponovnog pokretanja. Taj se problem pojavljuje ako koristite Emulator računati za privremeno obustavljanje i uklanjanje izvodi implementacije.

- Ako ste koristili bilo koju naredbu profiliranja iz naredbenog retka, osobito globalnih postavki, provjerite je li VSPerfClrEnv /globaloff je pozvan i da VsPerfMon.exe je isključen.

- Ako prilikom uzorkovanje, prikaže poruka "PRF0025: nema podataka o je koji se prikupljaju," Provjerite ima li proces koju priložili procesora aktivnosti. Aplikacije koje se ne radi ništa računalne možda neće proizvesti uzorkovanje podatke.  Moguće je postupak napuste prije nego što je gotovo bilo kojeg uzorkovanje. Provjerite koji način pokretanja uloge koje su Profiliranje prekinuti.

## <a name="next-steps"></a>Daljnji koraci

Instrumenting Azure binarne datoteke u na emulator nije podržan u Visual Studio profiler, ali ako želite testirati dodjelu memorije, možete odabrati tu mogućnost kada Profiliranje. Možete i odabrati istodobnosti Profiliranje, koji će vam olakšati određivanje jesu li niti rasipanje vrijeme osvojenih zaključavanjima ili sloju interakcije Profiliranje, koja omogućuje evidentiranje uzroka problema s performansama prilikom interakcije između razine aplikacije, najčešće između razina podataka i ulogu suradnika.  Možete pregledavati upita baze podataka koje generira aplikacije i korištenje profiliranja podataka da biste povećali korištenje baze podataka. Informacije o sloju interakcije Profiliranje potražite u članku na blogu [prođite kroz: korištenje Profiler interakcije sloju u Visual Studio 2010 sustava tima][3].



[1]: http://msdn.microsoft.com/library/azure/hh369930.aspx
[2]: http://msdn.microsoft.com/library/azure/hh411542.aspx
[3]: http://blogs.msdn.com/b/habibh/archive/2009/06/30/walkthrough-using-the-tier-interaction-profiler-in-visual-studio-team-system-2010.aspx
[4]: ./media/cloud-services-performance-testing-visual-studio-profiler/ProfilingLocally09.png
[5]: ./media/cloud-services-performance-testing-visual-studio-profiler/ProfilingLocally10.png
[6]: ./media/cloud-services-performance-testing-visual-studio-profiler/ProfilingLocally02.png
[7]: ./media/cloud-services-performance-testing-visual-studio-profiler/ProfilingLocally05.png
[8]: ./media/cloud-services-performance-testing-visual-studio-profiler/ProfilingLocally010.png
[9]: ./media/cloud-services-performance-testing-visual-studio-profiler/ProfilingLocally07.png
[10]: ./media/cloud-services-performance-testing-visual-studio-profiler/ProfilingLocally06.png
[11]: ./media/cloud-services-performance-testing-visual-studio-profiler/ProfilingLocally03.png
[12]: ./media/cloud-services-performance-testing-visual-studio-profiler/ProfilingLocally011.png
[14]: ./media/cloud-services-performance-testing-visual-studio-profiler/ProfilingLocally04.png 
[15]: ./media/cloud-services-performance-testing-visual-studio-profiler/ProfilingLocally013.png
[16]: ./media/cloud-services-performance-testing-visual-studio-profiler/ProfilingLocally012.png
[17]: ./media/cloud-services-performance-testing-visual-studio-profiler/ProfilingLocally08.png
 