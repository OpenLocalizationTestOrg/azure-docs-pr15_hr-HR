<properties
   pageTitle="Otklanjanje poteškoća s uloge koje ne prođu da biste pokrenuli | Microsoft Azure"
   description="Evo nekih najčešćih razloga zašto uloge u Oblaku možda se neće pokrenuti. Rješenja za te probleme i prikazuje se."
   services="cloud-services"
   documentationCenter=""
   authors="simonxjx"
   manager="felixwu"
   editor=""
   tags="top-support-issue"/>
<tags
   ms.service="cloud-services"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="tbd"
   ms.date="09/02/2016"
   ms.author="v-six" />

# <a name="troubleshoot-cloud-service-roles-that-fail-to-start"></a>Otklanjanje poteškoća sa servisom u Oblaku uloge koje ne prođu da biste pokrenuli

Evo nekih uobičajenih problema i rješenja povezani servisi u Oblaku Azure uloge koje ne prođu da biste pokrenuli.

[AZURE.INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

## <a name="missing-dlls-or-dependencies"></a>Nedostaju DLL bibliotekama i ovisnosti

Ne reagira uloge i uloge koje su cycling između **Initializing**, **zauzet**i **Zaustavljanje** stanja može uzrokovati nedostaje DLL bibliotekama ili sklopova.

Simptomi nedostaje DLL bibliotekama ili sklopova mogu biti:

- Vaša uloga instance cycling je putem **Initializing**, **zauzet**i **Zaustavljanje** stanja.
- Vaša uloga instance premještena **spremno** , ali ako dođite do web-aplikacije, na stranici se ne pojavljuje.

Postoji nekoliko preporučenih metoda za istražuje te probleme.

## <a name="diagnose-missing-dll-issues-in-a-web-role"></a>Dijagnosticiranje nedostaje DLL problema u ulozi web

Kada se vratite na web-mjesto koji je implementiran na web-mjestu uloge i pregledniku prikazuje pogreška poslužitelja sličnu ovoj, mogu upućivati DLL nedostaje.

![Pogreška poslužitelja '/' aplikacije.](./media/cloud-services-troubleshoot-roles-that-fail-start/ic503388.png)

## <a name="diagnose-issues-by-turning-off-custom-errors"></a>Dijagnosticiranje problema tako da isključite prilagođene pogreške

Detaljnije informacije o pogrešci moguće je prikazati konfiguriranjem konfiguraciju za ulogu web da biste postavili način prilagođene pogreške na isključeno i redeploying servis.

Da biste pogledali potpuniji pogreške bez korištenja udaljene radne površine:

1. Otvorite rješenje u programu Microsoft Visual Studio.

2. U **Pregledniku rješenja**pronađite web.config datoteke i otvorite je.

3. U datoteci web.config pronađite odjeljak sadrži i dodajte sljedeći redak:

    ```xml
    <customErrors mode="Off" />
    ```

4. Spremite datoteku.

5. Repackage i ponovno implementirate servis.

Kada ponovno je implementirati servis, vidjet ćete poruku o pogrešci s nazivom nedostaje skupa ili DLL-a.

## <a name="diagnose-issues-by-viewing-the-error-remotely"></a>Dijagnosticiranje problema s prikazom pogreške daljinski

Udaljena radna površina možete koristiti da biste pristupili ulogu i daljinsko prikaz potpuniji informacije o pogrešci. Da biste pogledali pogreške pomoću udaljene radne površine, poduzmite sljedeće korake:

1. Provjerite je li instaliran Azure SDK 1.3 ili noviji.

2. Tijekom implementacije rješenja pomoću Visual Studio, odaberite "Konfiguriraj udaljene radne površine veze...". Dodatne informacije o konfiguriranju veze udaljene radne površine potražite u članku [Korištenje udaljenu radnu površinu s Azure uloge](../vs-azure-tools-remote-desktop-roles.md).

3. Na portalu Microsoft Azure klasični kada instanci prikazuje status **spreman**, kliknite neku od uloga instance.

4. Kliknite ikonu **za povezivanje** u području **Daljinski pristup** na vrpci.

5. Prijavite se u virtualnog računala pomoću vjerodajnica koje je odredio vrijeme konfiguraciju udaljene radne površine.

6. Otvorite naredbeni prozor.

7. Vrsta `IPconfig`.

8. Imajte na umu vrijednost IPV4 adresa.

9. Otvorite Internet Explorer.

10. Upišite adresu i naziv web-aplikacije. Na primjer, `http://<IPV4 Address>/default.aspx`.

Navigacija na web-mjestu sada vraćaju više eksplicitnih poruke o pogreškama:

* Pogreška poslužitelja '/' aplikacije.

* Opis: Tijekom izvođenja trenutnog web-zahtjeva došlo je do neobrađenu iznimku. Pregledajte Praćenje stoga dodatne informacije o pogrešci i odakle potječe kod.

* Detaljima o iznimci: System.IO.FIleNotFoundException: ne može se učitati datoteku ili skupa ' Microsoft.WindowsAzure.StorageClient, verzija = 1.1.0.0, kulture = neutralno, token = 31bf856ad364e35 "ili neki njegov. Sustav nije moguće pronaći navedenu datoteku.

Ako, na primjer:

![Pogreška eksplicitnih poslužitelja '/' aplikacije](./media/cloud-services-troubleshoot-roles-that-fail-start/ic503389.png)

## <a name="diagnose-issues-by-using-the-compute-emulator"></a>Dijagnosticiranje problema pomoću emulator računalnim

Emulator računalnim Microsoft Azure možete koristiti za dijagnosticiranje i otklanjanje poteškoća od nedostaje zavisnosti i web.config pogreške.

Za najbolje rezultate u koristite taj način Dijagnostika, koristite računalo ili virtualnog računala koja ima čistu instalaciju sustava Windows. Da biste najbolje kao zamjenu za Azure okruženja, koristite Windows Server 2008 R2 x64.

1. Instalirajte samostalnu verziju [Azure SDK](https://azure.microsoft.com/downloads/).

2. Na računalu razvoj sastavljanje servisa project oblaka.

3. U programu Windows Explorer pronađite mapu bin\debug servisa projekta oblaka.

4. Kopirajte .csx mapu i .cscfg datoteku na računalo koje koristite za ispravljanje pogrešaka probleme.

5. Na računalu čistog otvorite prozor naredbenog retka za Azure SDK i vrsta `csrun.exe /devstore:start`.

6. U naredbeni redak upišite `run csrun <path to .csx folder> <path to .cscfg file> /launchBrowser`.

7. Kada se pokrene ulogu, vidjet ćete detaljne informacije o pogrešci u pregledniku Internet Explorer. Da biste dodatno dijagnosticiranje problem možete koristiti i standardne Windows alata za otklanjanje poteškoća.

## <a name="diagnose-issues-by-using-intellitrace"></a>Dijagnosticiranje problema pomoću IntelliTrace

Za tempiranja i uloge web koje koristite .NET Framework 4, možete koristiti [IntelliTrace](https://msdn.microsoft.com/library/dd264915.aspx), koji je dostupan u [Programu Microsoft Visual Studio Ultimate](https://www.visualstudio.com/products/visual-studio-ultimate-with-MSDN-vs).

Slijedite ove korake za implementaciju servis s IntelliTrace omogućen:

1. Provjerite je li instaliran Azure SDK 1.3 ili noviji.

2. Implementacija rješenja pomoću Visual Studio. Tijekom implementacije, potvrdite okvir **Omogući IntelliTrace za .NET 4 uloge** .

3. Kada se pokrene instancu, otvorite **Explorer poslužitelja**.

4. Proširenje na **Azure\\servise u Oblaku** čvor i pronađite uvođenje.

5. Proširite implementacijskih dok ne ugledate instance uloge. Desnom tipkom miša kliknite neku od instance.

6. Odaberite **Prikaz IntelliTrace zapisnika**. Otvorit će se **IntelliTrace sažetak** .

7. Pronađite u odjeljku iznimke na sažetak. Ako postoje iznimke, u odjeljku će biti označene **Podaci iznimke**.

8. Proširite **Podaci iznimke** i potražite pogreške **System.IO.FileNotFoundException** sličnu ovoj:

![Podaci iznimke, nedostaju datoteke ili skupa](./media/cloud-services-troubleshoot-roles-that-fail-start/ic503390.png)

## <a name="address-missing-dlls-and-assemblies"></a>Adresa nedostaje DLL bibliotekama i skupovi

Da biste riješili nedostaje DLL i pogreške u sklopu, slijedite ove korake:

1. U Visual Studio otvorite rješenje.

2. U **Pregledniku rješenja**, otvorite mapu u **referenci** .

3. Kliknite skupina označena u pogrešku.

4. U oknu **Svojstva** pronađite **svojstvo lokalnu kopiju** i postavite vrijednost **True**.

5. Ponovno implementirate servisa u oblaku.

Nakon što ste potvrdili da ste sve pogreške ispraviti, možete implementirati servis niste potvrdili okvir potvrdite okvir **Omogući IntelliTrace za .NET 4 uloge** .

## <a name="next-steps"></a>Daljnji koraci

Prikaz više [članke za otklanjanje poteškoća](https://azure.microsoft.com/documentation/articles/?tag=top-support-issue&product=cloud-services) za servise u oblaku.

Upute za otklanjanje poteškoća s oblaka servisa uloga pomoću Azure PaaS računalne Dijagnostika podatke, potražite u članku [se nizu blogova Kevin Williamson](http://blogs.msdn.com/b/kwill/archive/2013/08/09/windows-azure-paas-compute-diagnostics-data.aspx).
