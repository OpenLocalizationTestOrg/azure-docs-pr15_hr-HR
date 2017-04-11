<properties
   pageTitle="Konfiguriranje provjere autentičnosti uz Amazon web-servisi | Microsoft Azure"
   description="U ovom se članku opisuje kako stvoriti i provjeru vjerodajnica programa AWS za runbooks u automatizaciji Azure Upravljanje resursima AWS."
   services="automation"
   documentationCenter=""
   authors="mgoedtel"
   manager="jwhit"
   editor="tysonn"
   keywords="Provjera autentičnosti aws konfiguriranje aws"/>
<tags
   ms.service="automation"
   ms.workload="tbd"
   ms.tgt_pltfrm="na"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.date="09/12/2016"
   ms.author="magoedte"/>

# <a name="authenticate-runbooks-with-amazon-web-services"></a>Runbooks s Amazon web-servisi za provjeru autentičnosti
Automatizacija uobičajenih zadataka s resursima u Amazon Web Services (AWS) možete obaviti s Automatizacija runbooks u Azure.  Možete automatizirati mnogih zadataka u AWS pomoću automatizacije runbooks kao i s resursima u Azure.  Sve što je potrebno imati dvije stvari:

* Pretplatu na AWS i skup vjerodajnica.  Konkretno svoj AWS tipka za pristup i tajnu ključ.  Dodatne informacije pogledajte u članku [Korištenje AWS vjerodajnice](http://docs.aws.amazon.com/powershell/latest/userguide/specifying-your-aws-credentials.html).
* Azure pretplate i račun za automatizaciju.  Dodatne informacije o postavljanju račun za Azure Automatizacija, pogledajte članak [Konfiguriranje Azure pokrenuti kao račun](../automation/automation-sec-configure-azure-runas-account.md).  

Za provjeru autentičnosti AWS, morate navesti skup AWS vjerodajnice za provjeru vaše runbooks izvodi Azure automatizaciju. Ako već imate račun Automatizacija stvorili i želite koji koristite za provjeru s AWS, slijedite korake u sljedećem odjeljku.  Ako želite namjenski račun za runbooks targetting AWS resursa, trebali biste najprije stvorite novi [račun za automatizaciju Pokreni kao](../automation/automation-sec-configure-azure-runas-account.md) (preskoči mogućnost stvaranja glavni servisa), a zatim slijedite korake u nastavku.

## <a name="configure-automation-account"></a>Konfiguriranje računa za automatizaciju
Za automatizaciju Azure možete komunicirati s AWS, najprije morat ćete dohvatiti AWS vjerodajnice i pohraniti kao resursima u Azure automatizaciju.  Izvođenje sljedećih koraka navedenih u dokumentu AWS [Upravljanje tipkovni prečaci za vaš račun AWS](http://docs.aws.amazon.com/general/latest/gr/managing-aws-access-keys.html) da biste stvorili tipkovni prečac i kopirajte **ID ključ za pristup** i **Tajna tipkovni prečac** (preuzeti ključne datoteke koju želite pohraniti negdje sigurni).

Nakon što ste stvorili i kopirali ključeva za sigurnost AWS, morat ćete stvoriti vjerodajnica resursa s računom Azure Automatizacija sigurno pohraniti i uputiti na njih s vašeg runbooks.  Slijedite korake iz odjeljka **Da biste stvorili novu vjerodajnice** u članku [vjerodajnica resursima u Azure Automatizacija](../automation/automation-certificates.md/###To create a new credential with the Azure portal) , a zatim unesite sljedeće podatke:

1. U okvir **naziv** unesite **AWScred** ili odgovarajućom vrijednošću pratiti vaše imenovanja standarde.  
2. U okvir **korisničko ime** upišite svoj **ID programa Access** i **Tajna tipkovni prečac** u okvir **Lozinka** i **Potvrdi lozinku** .   

## <a name="next-steps"></a>Daljnji koraci

- U članku rješenje [Automating implementacije VM Amazon Web Services](../automation/automation-scenario-aws-deployment.md) da biste saznali kako stvoriti runbooks da biste automatizirali zadatke u AWS Reivew.
