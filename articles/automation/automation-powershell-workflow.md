<properties 
   pageTitle="Tijek rada ljuske PowerShell za učenje"
   description="U ovom se članku namijenjen kao brzi nastave autori poznato ljuske PowerShell za određene razlike između PowerShell i PowerShell tijeka rada."
   services="automation"
   documentationCenter=""
   authors="mgoedtel"
   manager="jwhit"
   editor="tysonn" />
<tags 
   ms.service="automation"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="09/12/2016"
   ms.author="bwren" />

# <a name="learning-windows-powershell-workflow"></a>Tijek rada sustava Windows PowerShell za učenje

Runbooks u automatizaciji Azure primjenjuju se kao tijekova rada za Windows PowerShell.  Tijek rada za Windows PowerShell je slična skripte komponente Windows PowerShell, ali nema neke značajan razlike koje mogu biti pregledniji novom korisniku.  Ovaj članak namijenjen korisnicima već upoznati s PowerShell i ukratko objašnjava koncepata zahtijevaju Ako pretvarate skriptu PowerShell u PowerShell tijeka rada za korištenje na runbook.  

Tijek rada je niz programiranih, povezanog koraka koji izvršavanje zadataka dugoročnih ili zahtijevaju koordinaciji više koraka u više uređaja ili upravljanih čvorove. Prednosti tijeka rada preko normalni skripte obuhvaćaju mogućnost istodobno izvođenje akcija na temelju više uređaja i mogućnost automatski oporaviti od pogrešaka. Tijek rada za Windows PowerShell je skripte komponente Windows PowerShell koja upravlja Windows Foundation tijeka rada. Dok se tijek rada je namijenjene sintaksa komponente Windows PowerShell i pokrenuo neki komponente Windows PowerShell, ona se obrađuje, Windows Foundation tijeka rada.

Sve pojedinosti o temama u ovom članku potražite u članku [Uvod u tijek rada sustava Windows PowerShell](http://technet.microsoft.com/library/jj134242.aspx).

## <a name="types-of-runbook"></a>Vrste kompilacije

Postoje tri vrste kompilacije u *Azure Automatizacija, PowerShell tijek rada*, *PowerShell* i *grafički*.  Definiranje vrsta runbook kada stvarate u runbook, a ne možete pretvoriti u runbook drugu vrstu kada je stvorena.

Runbooks PowerShell tijeka rada i runbooks ljuske PowerShell za korisnike koji radije radite izravno s kodom PowerShell ili koristite tekstnih editor u automatizaciji Azure ili izvanmrežne uređivača kao što su Očisti filtar. Ako stvarate tijeka rada PowerShell runbook upoznati informacije u ovom članku. 

Grafički runbooks omogućuju stvaranje runbook koristite aktivnosti i cmdleta, ali pomoću grafičkog sučelja sakriven complexities podlozi PowerShell tijeka rada.  Koncepti u ovom članku kao što su checkpoints i paralelno izvođenja se i dalje primjenjuju na grafički runbooks, ali ne morate brinuti o detaljnu sintaksu. 

## <a name="basic-structure-of-a-workflow"></a>Osnovna struktura tijeka rada

Prvi je korak u pretvaranje skriptu PowerShell PowerShell tijek rada je umetnute ga pomoću ključne riječi **tijeka rada** .  Slijedi tijelo skripte u vitičaste zagrade ključne riječi **tijek rada** pokrene tijek rada. Naziv tijeka rada slijedi ključnu riječ **tijeka rada** , kao što je prikazano u sljedećoj sintaksi. 

    Workflow Test-Workflow
    {
       <Commands>
    }

Naziv tijeka rada moraju podudarati s imenom kompilacije automatizaciju. Ako se uvozi na runbook, zatim naziv datoteke moraju se podudarati naziv tijeka rada i završiti .ps1.

Da biste dodali parametara tijeka rada, upotrijebite ključnu riječ **parametarskog** isti način kao u skriptu. 

## <a name="code-changes"></a>Kod promjene

Kod tijeka rada PowerShell izgleda gotovo jednako kodu skripte komponente PowerShell osim nekoliko značajne promjene.  U sljedećim odjeljcima opisuju promjene koje ćete morati izvršiti skriptu PowerShell za pokretanje u tijeku rada.

### <a name="activities"></a>Aktivnosti

Aktivnost je određenog zadatka u tijeku rada. Baš kao i skriptu sastoji se od jednog ili više naredbi, tijek rada sastoji se od jednog ili više aktivnosti koji se izvršavaju u nizu. Tijek rada za Windows PowerShell automatski pretvara mnoge cmdleta ljuske Windows PowerShell aktivnosti kada pokrene tijek rada. Kada navedete jednu od tih cmdleta u vašem runbook odgovarajuće aktivnosti zapravo je pokrenuti tako Windows Foundation tijeka rada. Za tih cmdleta bez odgovarajuće aktivnosti tijeka rada za Windows PowerShell automatski će se pokrenuti cmdlet unutar [InlineScript](#inlinescript) aktivnosti. Postoji skup cmdleta Izuzeto i ne mogu se koristiti u tijeku rada osim ako ih izričito ne uključite u Bloku za InlineScript. Dodatne informacije o koncepte potražite u članku [Korištenje aktivnosti u tijekovima rada za skripte](http://technet.microsoft.com/library/jj574194.aspx).

Aktivnosti tijeka rada imaju zajednički skup uobičajenih parametara da biste konfigurirali njihov rad. Detalje o parametrima uobičajenih tijeka rada potražite u članku [about_WorkflowCommonParameters](http://technet.microsoft.com/library/jj129719.aspx).

### <a name="positional-parameters"></a>Pozicijske parametara

Ne možete koristiti Pozicijske parametara s aktivnosti i cmdleta u tijeku rada.  To znači je da morate koristiti Nazivi parametara.

Na primjer, razmotrite sljedeći kod koji dohvaća sve pokrenute servise.

     Get-Service | Where-Object {$_.Status -eq "Running"}

Ako pokušate pokrenuti isti kod u tijeku rada, dobit ćete poruku kao "Postavljanje parametar ne može riješiti pomoću navedeni pod nazivom parametara."  Da biste to ispravili, jednostavno Navedite naziv parametra kao sljedeće.

    Workflow Get-RunningServices
    {
        Get-Service | Where-Object -FilterScript {$_.Status -eq "Running"}
    }

### <a name="deserialized-objects"></a>Deserijalizirati objekata

Objekti u tijekovima rada su deserijalizirati.  To znači da njihova svojstva i dalje su dostupni, ali ne njihove metode.  Na primjer, razmotrite sljedeći kod PowerShell koja zaustavlja servisa pomoću metode Zaustavi objekta servisa.

    $Service = Get-Service -Name MyService
    $Service.Stop()

Ako pokušate izvesti u tijeku rada, prikazat će se pogreška koja kaže "metode poziva nije podržano u tijeku rada sustava Windows PowerShell".  

Jedna od mogućnosti jest da biste prelomili te dva retka koda u programa [InlineScript](#InlineScript) bloka u tom slučaju $Service bio objekt servisa unutar bloka. 

    Workflow Stop-Service
    {
        InlineScript {
            $Service = Get-Service -Name MyService
            $Service.Stop()
        }
    } 

Da biste koristili neki drugi cmdlet koji se izvodi ista funkcija kao metodu, ako je dostupan je i mogućnost.  U slučaju oglednim, cmdlet Zaustavljanje servisa nudi sve funkcije kao metodu tabulatora, a možete koristiti sljedeće tijeka rada.

    Workflow Stop-MyService
    {
        $Service = Get-Service -Name MyService
        Stop-Service -Name $Service.Name
    }


## <a name="inlinescript"></a>InlineScript

Aktivnosti **InlineScript** koristan je kada morate pokrenuti jednu ili više naredbi kao tradicionalni skriptu PowerShell umjesto PowerShell tijeka rada.  Dok naredbi u tijeku rada bit će poslani Windows Foundation tijeka rada za obradu, naredbe programa blok InlineScript su obradili komponente Windows PowerShell. 

InlineScript koristi sintaksu prikazano u nastavku.

    InlineScript
    {
      <Script Block>
    } <Common Parameters>

Izlaz iz programa InlineScript možete vratiti dodjelom izlaz tjednog prikaza kalendara. U sljedećem primjeru zaustavlja usluge i proizvodi naziv usluge.

    Workflow Stop-MyService
    {
        $Output = InlineScript {
            $Service = Get-Service -Name MyService
            $Service.Stop()
            $Service
        }

        $Output.Name
    }


Možete proslijediti vrijednosti u blok za InlineScript, ali morate koristiti **$Using** opseg mijenjanje.  U sljedećem primjeru jednak u prethodnom primjeru osim što je naziv servisa nudi tjednog prikaza kalendara. 

    Workflow Stop-MyService
    {
        $ServiceName = "MyService"
    
        $Output = InlineScript {
            $Service = Get-Service -Name $Using:ServiceName
            $Service.Stop()
            $Service
        }

        $Output.Name
    }


Dok InlineScript aktivnosti može biti ključnih u određeni tijekovi rada, oni ne podržavaju konstrukta tijeka rada i trebali biste koristiti samo kada je to potrebno zbog sljedećih razloga:

- Ne možete koristiti [checkpoints](#Checkpoints) unutar programa blok InlineScript. Ako dođe do pogreške unutar bloka, morate nastaviti od početka bloka.
- Ne možete koristiti [paralelne izvođenja](#parallel-execution) unutar programa InlineScriptBlock.
- Budući da je navedena sesije komponente Windows PowerShell za cijelu duljinu bloka InlineScript InlineScript utječe na skalabilnost tijeka rada.

Dodatne informacije o korištenju InlineScript potražite u članku [Pokretanje naredbe ljuske PowerShell sustava Windows u tijeku rada](http://technet.microsoft.com/library/jj574197.aspx) i [about_InlineScript](http://technet.microsoft.com/library/jj649082.aspx).


## <a name="parallel-processing"></a>Paralelni obrada

Jedna od prednosti tijekova rada za Windows PowerShell je za bicikle skupa naredbi paralelno umjesto sekvencijalno s uobičajeni skriptu. 

**Paralelni** ključnih riječi možete koristiti da biste stvorili skripte blok s više naredbi koji će se pokrenuti istovremeno. Koristi se vidjet ćete da sintaksa prikazano u nastavku. U ovom slučaju Activity1 i Activity2 počet će se i u isto vrijeme. Activity3 će se pokrenuti tek nakon što ste dovršili Activity1 i Activity2.

    Parallel
    {
      <Activity1>
      <Activity2>
    }
    <Activity3>


Na primjer, razmotrite sljedeće naredbe ljuske PowerShell kopiranje više datoteka u mreži odredište.  Te naredbe se izvoditi sekvencijalno tako da se taj jedne datoteke mora dovršiti kopiranje prije pokretanja uz.     

    $Copy-Item -Path C:\LocalPath\File1.txt -Destination \\NetworkPath\File1.txt
    $Copy-Item -Path C:\LocalPath\File2.txt -Destination \\NetworkPath\File2.txt
    $Copy-Item -Path C:\LocalPath\File3.txt -Destination \\NetworkPath\File3.txt

Sljedeće izvođenja tijeka rada te iste naredbe paralelno tako da se svi oni kopiranja u isto vrijeme.  Tek nakon što su potpuno kopirali poruku dovršetka prikazuje se.

    Workflow Copy-Files
    {
        Parallel 
        {
            $Copy-Item -Path "C:\LocalPath\File1.txt" -Destination "\\NetworkPath"
            $Copy-Item -Path "C:\LocalPath\File2.txt" -Destination "\\NetworkPath"
            $Copy-Item -Path "C:\LocalPath\File3.txt" -Destination "\\NetworkPath"
        }

        Write-Output "Files copied."
    }


Možete koristiti u **ForEach-paralelno** konstrukta naredbama postupak za svaku stavku u zbirci istovremeno. Stavke u skupu se obrađuju paralelno naredbi u bloku skripte sekvencijalno izlaganja. Koristi se vidjet ćete da sintaksa prikazano u nastavku. U ovom slučaju Activity1 će se pokrenuti u isto vrijeme za sve stavke u skupu. Za svaku stavku Activity2 započet će nakon Activity1. Activity3 će se pokrenuti tek nakon Activity1 i Activity2 dovršite za sve stavke.

    ForEach -Parallel ($<item> in $<collection>)
    {
      <Activity1>
      <Activity2>
    }
    <Activity3>

U sljedećem primjeru je slično kao u prethodnom primjeru kopiranje datoteka paralelno.  U ovom slučaju, pojavljuje se poruka za svaku datoteku nakon što ga kopira.  Tek nakon što su potpuno kopirali poruku konačni dovršetka prikazuje se.

    Workflow Copy-Files
    {
        $files = @("C:\LocalPath\File1.txt","C:\LocalPath\File2.txt","C:\LocalPath\File3.txt")

        ForEach -Parallel ($File in $Files) 
        {
            $Copy-Item -Path $File -Destination \\NetworkPath
            Write-Output "$File copied."
        }
        
        Write-Output "All files copied."
    }

> [AZURE.NOTE]  Ne preporučujemo koristite podređeni runbooks paralelno jer je to prikazano dati nepouzdanih rezultate.  Izlaz iz runbook podređeni ponekad neće se prikazivati i postavke u jedan podređeni element runbook mogu utjecati na paralelne podređenih runbooks 


## <a name="checkpoints"></a>Checkpoints

*Kontrolna točka* je snimak trenutnog stanja tijeka rada koji sadrži trenutnu vrijednost za varijable i sve izlazne generira točke. Ako tijek rada završava pogreške ili je obustavljena, zatim sljedeći put pokretanja će započeti s njegova zadnjeg Kontrolna točka umjesto početak u worfklow.  U tijeku rada s aktivnosti **Kontrolna točka-tijek rada** možete postaviti na Kontrolna točka.

U sljedećem kodu uzorka iznimku pojavljuje nakon Activity2 uzrokuje završetka tijeka rada. Kada se tijek rada je ponovno pokrenite, pokreće tako da pokrenete Activity2 jer je tek nakon zadnjeg Kontrolna točka postavite.

    <Activity1>
    Checkpoint-Workflow
    <Activity2>
    <Exception>
    <Activity3>

Postavite checkpoints u tijeku rada nakon aktivnosti koje se mogu biti podložni iznimku, a ne smije biti ponavlja nastaviti ako je tijek rada. Na primjer, tijek rada može stvoriti virtualnog računala. Nije moguće postaviti na Kontrolna točka ispred i iza naredbe za stvaranje virtualnog računala. Stvaranje ne uspije, zatim naredbe želite ponoviti ako ponovno pokrenuti tijek rada. Ako se na worfklow prekida se nakon uspješnog stvaranja, a zatim virtualnog računala neće stvoriti ponovno kada se tijek rada je nastaviti.

U sljedećem primjeru kopira više datoteka na mrežnom mjestu, a postavlja na Kontrolna točka nakon svake datoteke.  Ako je mrežno mjesto izgubiti, tijek rada će se zaustaviti pogrešku.  Prilikom pokretanja ponovno, će nastaviti na zadnji Kontrolna točka što znači da će se samo datoteke koje se već kopiran preskočiti.

    Workflow Copy-Files
    {
        $files = @("C:\LocalPath\File1.txt","C:\LocalPath\File2.txt","C:\LocalPath\File3.txt")

        ForEach ($File in $Files) 
        {
            $Copy-Item -Path $File -Destination \\NetworkPath
            Write-Output "$File copied."
            Checkpoint-Workflow
        }
        
        Write-Output "All files copied."
    }

Budući da se vjerodajnice korisničko ime se ista i nakon poziva aktivnosti [Suspend-tijeka rada](https://technet.microsoft.com/library/jj733586.aspx) ili nakon zadnjeg kontrolne točke, prvo morate postaviti vjerodajnice za null i zatim dohvaćanje ih ponovno iz spremišta resursa nakon naziva **Suspend tijek** rada ili Kontrolna točka.  U suprotnom, možete dobiti sljedeću poruku o pogrešci: *zadatak tijeka rada ne može se nastaviti, ili jer postojanost podataka nije moguće biti spremljene u potpunosti, ili spremiti postojanost podataka oštećena. Morate ponovno pokrenuti tijek rada.*

Sljedeći kod isti pokazuje kako to učiniti u vašem runbooks PowerShell tijeka rada.

       
    workflow CreateTestVms
    {
       $Cred = Get-AzureAutomationCredential -Name "MyCredential"
       $null = Add-AzureRmAccount -Credential $Cred

       $VmsToCreate = Get-AzureAutomationVariable -Name "VmsToCreate"

       foreach ($VmName in $VmsToCreate)
         {
          # Do work first to create the VM (code not shown)
        
          # Now add the VM
          New-AzureRmVm -VM $Vm -Location "WestUs" -ResourceGroupName "ResourceGroup01"

          # Checkpoint so that VM creation is not repeated if workflow suspends
          $Cred = $null
          Checkpoint-Workflow
          $Cred = Get-AzureAutomationCredential -Name "MyCredential"
          $null = Add-AzureRmAccount -Credential $Cred
         }
     } 


Ovo nije obavezno ako se provjera autentičnosti pomoću Pokreni kao račun konfiguriran sa servisom glavni.  

Dodatne informacije o checkpoints potražite u članku [Dodavanje Checkpoints skripte tijeka rada](http://technet.microsoft.com/library/jj574114.aspx).


## <a name="next-steps"></a>Daljnji koraci

- Početak rada s runbooks PowerShell tijeka rada, u odjeljku [Moje prvi runbook PowerShell tijeka rada](automation-first-runbook-textual.md) 
