<properties
    pageTitle="Azure Dijagnostika za otklanjanje poteškoća"
    description="Otklanjanje poteškoća prilikom korištenja Azure Dijagnostika u Azure servise u Oblaku, virtualnim strojevima i "
    services="multiple"
    documentationCenter=".net"
    authors="rboucher"
    manager="jwhit"
    editor=""/>

<tags
    ms.service="multiple"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="10/04/2016"
    ms.author="robb"/>


# <a name="azure-diagnostics-troubleshooting"></a>Azure Dijagnostika za otklanjanje poteškoća
Dodatne informacije o pomoću Azure Dijagnostika za otklanjanje poteškoća. Dodatne informacije o Azure Dijagnostika potražite u članku [Pregled Dijagnostika Azure](azure-diagnostics.md#cloud-services).

## <a name="azure-diagnostics-is-not-starting"></a>Azure dijagnostika ne pokreće

Dijagnostika sastoji se od dviju komponenata: dodatak agent za goste i agent za nadzor. Provjerite datoteke zapisnika sustava **DiagnosticsPluginLauncher.log** i **DiagnosticsPlugin.log** informacije o Zašto dijagnostika ne pokreće.  
  
U ulozi u Oblaku zapisničke datoteke za dodatak agent za goste nalaze se u: 

``` 
C:\Logs\Plugins\Microsoft.Azure.Diagnostics.PaaSDiagnostics\1.6.3.0\ 
``` 

U programa Azure virtualnog računala, zapisničke datoteke za dodatak agent za goste nalaze u: 

``` 
C:\Packages\Plugins\Microsoft.Azure.Diagnostics.IaaSDiagnostics\1.6.3.0\Logs\ 
``` 
 
Zadnji redak u datoteke zapisnika će sadržavati izlazni kod.  

``` 
DiagnosticsPluginLauncher.exe Information: 0 : [4/16/2016 6:24:15 AM] DiagnosticPlugin exited with code 0 
``` 

Dodatak vraća sljedeće kodove izlaz:

Izlazni kod|Opis
---|---
0|Uspjeh.
-1|Generički pogreške.
-2|Učitavanje datoteka rcf nije moguće.<p>To je pojavila se interna pogreška samo će se dogoditi ako pokretač dodatak agent za goste se ručno poziva, pogrešno, na na VM.
-3|Konfiguracijska datoteka dijagnostika ne može se učitati.<p><p>Rješenje: To je rezultat konfiguracijska datoteka ne prosljeđivanje sheme. Rješenje je pružanje konfiguracijska datoteka koji je u skladu sa shemom.
-4|Drugu instancu nadzora agent Dijagnostika već koristi imenik lokalnog resursa.<p><p>Rješenje: Navedite neku drugu vrijednost **LocalResourceDirectory**.
-6|Pokretač dodatak agent za goste pokušaj da biste pokrenuli dijagnostiku s naredbenog retka koji nisu valjani.<p><p>To je pojavila se interna pogreška samo će se dogoditi ako pokretač dodatak agent za goste se ručno poziva, pogrešno, na na VM.
-10|Dodatak za dijagnostiku napuste uz neobrađenu iznimku.
-11|Agent za goste nije uspio postupka stvorite odgovoran za pokretanje te praćenje agent za nadzor.<p><p>Rješenje: Provjerite jesu li dovoljno sistemske resurse za pokretanje procesa novi.<p>
-101|Argumenti koji nisu valjani prilikom pozivanja dodatak za dijagnostiku.<p><p>To je pojavila se interna pogreška samo će se dogoditi ako pokretač dodatak agent za goste se ručno poziva, pogrešno, na na VM.
-102|Dodatak za postupak je nije moguće pokrenuti sam.<p><p>Rješenje: Provjerite jesu li dovoljno sistemske resurse za pokretanje procesa novi.
-103|Dodatak za postupak je nije moguće pokrenuti sam. Posebno je nije moguće stvoriti zapisivaču objekt.<p><p>Rješenje: Provjerite jesu li dovoljno sistemske resurse za pokretanje procesa novi.
-104|Nije moguće učitati datoteku rcf nudi agent za goste.<p><p>To je pojavila se interna pogreška samo će se dogoditi ako pokretač dodatak agent za goste se ručno poziva, pogrešno, na na VM.
-105|Dodatak za dijagnostiku nije moguće otvoriti datoteku za konfiguraciju Dijagnostika.<p><p>To je pojavila se interna pogreška samo će se dogoditi ako je dodatak za dijagnostiku se ručno poziva, pogrešno, na na VM.
-106|Konfiguracijska datoteka dijagnostika ne može čitati.<p><p>Rješenje: To je rezultat konfiguracijska datoteka ne prosljeđivanje sheme. Rješenje pa je možete unijeti konfiguracijska datoteka koji je u skladu sa shemom. Možete pronaći XML koji se isporučuje za dijagnostiku datotečni nastavak u mapu *%SystemDrive%\WindowsAzure\Config* na na VM. Otvorite odgovarajući XML datoteke i pretraživanje **Microsoft.Azure.Diagnostics**, a zatim **xmlCfg** polja. Podaci su base64 kodirani pa morat ćete [dekodiranje ga](http://www.bing.com/search?q=base64+decoder) da biste vidjeli XML koji učitana Dijagnostika.<p>
-107|Pristupni direktorija resursa za nadzor agent nije valjana.<p><p>To je pojavila se interna pogreška samo će se dogoditi ako agent za nadzor se ručno poziva, pogrešno, na na VM.</p>
-108    |Nije moguće pretvoriti konfiguracijska datoteka Dijagnostika nadzora agent konfiguracijska datoteka.<p><p>To je pojavila se interna pogreška samo će se dogoditi ako je dodatak za dijagnostiku ručno pozvati s datotekom konfiguracija nije valjana.
-110|Pogreška u konfiguraciji Dijagnostika Općenito.<p><p>To je pojavila se interna pogreška samo će se dogoditi ako je dodatak za dijagnostiku ručno pozvati s datotekom konfiguracija nije valjana.
-111|Nije moguće pokrenuti agent za nadzor.<p><p>Rješenje: Provjerite jesu li dovoljno sistemske resurse dostupne.
-112|Opće pogreške


## <a name="diagnostics-data-is-not-logged-to-azure-storage"></a>Dijagnostika podaci niste prijavljeni u spremište Azure
Azure Dijagnostika pohranjuje sve podatke u spremište Azure.

Najčešći uzrok nedostaje podaci o događaju je podatke o računu neispravno definirani prostora za pohranu.

Rješenje: Ispravite Dijagnostika konfiguracijskoj datoteci i ponovno instalirati Dijagnostika.
Ako se problem nastavi pojavljivati i nakon ponovno instalirati nastavak Dijagnostika, a zatim, možda ćete morati ispravljanje pogrešaka dodatno zatrebaju kroz sve nadzora agent pogreške. Prije nego što se podaci o događaju prenijeti na račun servisa za pohranu pohranjuju se u na LocalResourceDirectory.

Uloge servisa oblak na LocalResourceDirectory glasi:

    C:\Resources\Directory\<CloudServiceDeploymentID>.<RoleName>.DiagnosticStore\WAD<DiagnosticsMajorandMinorVersion>\Tables

Za virtualnim računalima sustava LocalResourceDirectory glasi:

    C:\WindowsAzure\Logs\Plugins\Microsoft.Azure.Diagnostics.IaaSDiagnostics\<DiagnosticsVersion>\WAD<DiagnosticsMajorandMinorVersion>\Tables

Ako nema datoteka u mapi LocalResourceDirectory, agent za nadzor, ne može pokrenuti. To je obično uzrokovano datoteku konfiguracija nije valjana, događaj koji moraju biti prijavljene na CommandExecution.log.

Ako Agent za nadzor je uspješno prikupljanja podataka za događaj vidjet ćete .tsf datoteka za svaki događaj definirano u konfiguracijskoj datoteci. Agent za nadzor evidentira njegov pogreške u datoteci MaEventTable.tsf. Morate pregledavati sadržaj ove datoteke možete koristiti aplikaciju tabel2csv da biste pretvorili datoteku .tsf zarez zarezom values(.csv) datoteke:

Na Oblaku uloge servisa:

    %SystemDrive%\Packages\Plugins\Microsoft.Azure.Diagnostics.PaaSDiagnostics\<DiagnosticsVersion>\Monitor\x64\table2csv maeventtable.tsf

*% Sistemski pogon* na servis ulozi oblaka obično je D:

Na virtualnog računala:

    C:\Packages\Plugins\Microsoft.Azure.Diagnostics.IaaSDiagnostics\<DiagnosticsVersion>\Monitor\x64\table2csv maeventtable.tsf

Naredbe iznad generira u zapisnik datoteke *maeventtable.csv*, koji možete otvoriti i Provjera poruka pogreške.    


## <a name="diagnostics-data-tables-not-found"></a>Dijagnostika podatkovne tablice nije pronađen
Tablica u Azure prostora za pohranu držanjem podataka Azure Dijagnostika imenovane su pomoću koda u nastavku:

        if (String.IsNullOrEmpty(eventDestination)) {
            if (e == "DefaultEvents")
                tableName = "WADDefault" + MD5(provider);
            else
                tableName = "WADEvent" + MD5(provider) + eventId;
        }
        else
            tableName = "WAD" + eventDestination;

Evo jednog primjera:

        <EtwEventSourceProviderConfiguration provider=”prov1”>
          <Event id=”1” />
          <Event id=”2” eventDestination=”dest1” />
          <DefaultEvents />
        </EtwEventSourceProviderConfiguration>
        <EtwEventSourceProviderConfiguration provider=”prov2”>
          <DefaultEvents eventDestination=”dest2” />
        </EtwEventSourceProviderConfiguration>

Koje će generirati 4 tablice:

Događaja|Naziv tablice
---|---
Davatelj = "prov1" &lt;id događaja = "1" /&gt;|WADEvent + MD5("prov1") + "1"
Davatelj = "prov1" &lt;id događaja = "2" eventDestination = "dest1" /&gt;|WADdest1
Davatelj = "prov1" &lt;DefaultEvents /&gt;|WADDefault+MD5("prov1")
Davatelj = "prov2" &lt;DefaultEvents eventDestination = "dest2" /&gt;|WADdest2
