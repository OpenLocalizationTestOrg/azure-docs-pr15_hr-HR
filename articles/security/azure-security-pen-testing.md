<properties
   pageTitle="Testiranje olovke | Microsoft Azure"
   description="Članak sadrži pregled penetration testiranja postupka (pentest) i kako obaviti pentest protiv aplikacija izvodi u Azure infrastrukture."
   services="security"
   documentationCenter="na"
   authors="YuriDio"
   manager="swadhwa"
   editor="TomSh"/>

<tags
   ms.service="security"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="10/25/2016"
   ms.author="yurid"/>

# <a name="pen-testing"></a>Testiranje olovke

Jedna od sjajno stvari o korištenju sustava Microsoft Azure za testiranje aplikacije i implementaciju je ne morate sastavili infrastruktura za lokalni razvijaju, testirajte i implementaciju aplikacija. Sve Infrastruktura je snimanja brigu o servisima platformu Microsoft Azure. Ne morate brinuti o requisitioning, pri dohvaćanju, "racking i slaganja" lokalnog hardvera.

Ovo je odlično – ali svejedno morate provjerite je li izvesti normalno sigurnosti krajnji diligence. Što morate učiniti još penetration Testirajte aplikacije implementacije u Azure.

Možda već znate Microsoft izvodi [penetration testiranje naš Azure okruženja](https://gallery.technet.microsoft.com/Cloud-Red-Teaming-b837392e). Pridonosi poboljšanju naših platforme i vodi naš akcije poboljšanje sigurnosti kontrole, Uvod u nove kontrole za sigurnost i poboljšanje naš postupke za sigurnost.

Ne možemo ne olovke testiranje aplikacije umjesto vas, ali uviđamo će koji želite i potrebne za izvođenje olovke testiranje na vlastitu aplikacijama. Koje je dobro, jer kada poboljšati sigurnost aplikacija, pomoći sigurniju cijelu Azure zajednici.

Kada ste olovke testirali aplikacija, ona će izgledati kao u slučaju napada nam. Ne možemo [neprestano praćenje](http://blogs.msdn.com/b/azuresecurity/archive/2015/07/05/best-practices-to-protect-your-azure-deployment-against-cloud-drive-by-attacks.aspx) za napasti uzoraka i će pokrenuti proces incidenta odgovor ako mi je potreban. To ne pomoći ćete i ga ne Pridonesite ako ne možemo pokrenuti incidenta odgovor zbog vlastite krajnji diligence olovke testiranja.

Što učiniti?

Kada budete spremni olovku testiranje Azure hostirane aplikacije, morate Javite nam. Nakon što smo znate da ćete izvoditi određene testira, ćemo nećete slučajno ste isključiti (kao što su blokiranje IP adresa koju ste testiranje iz), pod uvjetom da testiranja u skladu sa na Azure olovke testiranja uvjete i odredbe.
Standardni testira možete izvršiti obuhvaćaju sljedeće:

- Testira na vašem krajnje točke steknite [otvorene Web aplikacije sigurnost projekta (OWASP) gornji 10 slabe točke](https://www.owasp.org/index.php/Category:OWASP_Top_Ten_Project)
- [Testiranje fuzz](https://blogs.microsoft.com/cybertrust/2007/09/20/fuzz-testing-at-microsoft-and-the-triage-process/) vaše krajnje točke
- [Pregled priključka](https://en.wikipedia.org/wiki/Port_scanner) vaše krajnje točke

Je jedna vrsta testu koji se ne može izvršiti nijednu od napada [Uskraćivanje usluga (DoS)](https://en.wikipedia.org/wiki/Denial-of-service_attack) . To uključuje pokretanje DoS napada sam ili izvođenje povezane testova koji može odrediti, demonstrirati ili kao zamjenu za bilo koju vrstu DoS napada.

Ste spremni za početak olovkom testiranja aplikacija smješten u Microsoft Azure? Ako je tako, a zatim glavni na putem [Penetration Test pregled](https://security-forms.azure.com/penetration-testing/terms) stranicu (i kliknite Stvori zahtjev za testiranje gumb pri dnu stranice. Također nalaze se dodatne informacije o olovke testiranja uvjete i odredbe te korisne veze na kako prijaviti sigurnost pogreške vezane uz Azure ili Microsoftov servis.
