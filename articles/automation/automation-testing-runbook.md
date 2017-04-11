<properties 
    pageTitle="Testiranje programa runbook u automatizaciji Azure | Microsoft Azure"
    description="Prije nego što objavite na runbook u automatizaciji Azure, možete testirati i provjerite koji funkcionira kao što je očekivano.  U ovom se članku opisuje kako testirati u runbook i prikaz rezultat."
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
    ms.author="magoedte;bwren" />

# <a name="testing-a-runbook-in-azure-automation"></a>Testiranje programa runbook u automatizaciji Azure
Kada testiranje na runbook, [verzija skice](automation-creating-importing-runbook.md#publishing-a-runbook) se izvršava i akcije koje izvodi se dovrše. Stvara se bez dosadašnje iskustvo, ali [izlazne](automation-runbook-output-and-messages.md#output-stream) i [upozorenja i pogreške](automation-runbook-output-and-messages.md#message-streams) strujanja prikazuju se u Test izlaz okna. Poruke [opširno strujanje](automation-runbook-output-and-messages.md#message-streams) prikazuju se u oknu Izlaz samo ako je [varijabla $VerbosePreference](automation-runbook-output-and-messages.md#preference-variables) postavljeno na Nastavi.

Čak i ako je pokrenut verzija skice, u runbook i dalje obično izvršava tijeka rada i izvršava radnje protiv resursa u okruženju. Zbog toga treba testirajte runbooks pri resursi koji nisu proizvodnje.

Isti postupak da biste testirali svaku [vrstu kompilacije](automation-runbook-types.md) je, a nema razlike u testiranje između uređivaču tekstnih i grafički editor na portalu za Azure.  


## <a name="to-test-a-runbook-in-the-azure-portal"></a>Da biste testirali u runbook na portalu za Azure

Možete raditi s bilo koju [vrstu runbook](automation-runbook-types.md) na portalu za Azure.

1. Otvorite radne verzije na kompilacije [tekstnih editor](automation-editing-a-runbook.md#Portal) ili [grafički uređivaču](automation-graphical-authoring-intro.md).
2. Kliknite gumb za **testiranje** da biste otvorili plohu Test.
3. Ako je na runbook parametara, bit će navedeni u lijevom oknu koje možete unijeti vrijednosti koje će se koristiti za testiranje.
4. Ako želite da biste pokrenuli test za [Hibridno Runbook tempiranja](automation-hybrid-runbook-worker.md), promijenite **Postavke za pokretanje** u **Hibridnog tempiranja** i odaberite naziv ciljne grupe.  U suprotnom zadržavanje zadanog **Azure** da biste pokrenuli test u oblak.
5. Kliknite gumb **Start** da biste pokrenuli test.
6. Ako je na runbook [Tijeka rada programa PowerShell](automation-runbook-types.md#powershell-workflow-runbooks) ili [Graphical](automation-runbook-types.md#graphical-runbooks), možete prekinuti ili odgoditi dok je testiraju s gumbima ispod okna izlaz. Kada se privremeno obustavljanje na runbook dovršava trenutne aktivnosti prije nego što se obustavlja. Kada se obustavlja na runbook, možete ga zaustaviti ili ga ponovno pokrenite.
7. Provjeri Izlaz iz runbook u oknu Izlaz.


## <a name="next-steps"></a>Daljnji koraci

- Da biste saznali kako stvaranja i uvoza u runbook, potražite u članku [Stvaranje ili uvoz runbook u automatizaciji Azure](automation-creating-importing-runbook.md)
- Da biste saznali više o grafički Authoring, potražite u članku [Graphical vremenu u Automatizacija Azure](automation-graphical-authoring-intro.md)
- Početak rada s runbooks PowerShell tijeka rada, u odjeljku [Moje prvi runbook PowerShell tijeka rada](automation-first-runbook-textual.md)
- Dodatne informacije o konfiguriranju runboks da biste se vratili poruke o statusu i pogreške, uključujući preporučeni Načini rada potražite u članku [Runbook izlazne i poruke u automatizaciji Azure](automation-runbook-output-and-messages.md)