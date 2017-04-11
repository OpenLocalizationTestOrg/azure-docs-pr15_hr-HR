<properties
   pageTitle="Hibridno radnih Runbook: Runbook posao završava sa statusom obustavljeno | Microsoft Azure"
   description="Simptomi uzroci i rješenja za hibridno Runbook tempiranja posao prekid pogrešku."
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
   ms.date="08/17/2016"
   ms.author="magoedte" />

# <a name="hybrid-runbook-worker-a-runbook-job-terminates-with-a-status-of-suspended"></a>Hibridno radnih Runbook: Runbook posao završava sa statusom obustavljeno

## <a name="summary"></a>Sažetak

Vaše runbook obustavljena je uskoro nakon Pokušaj izvršavanja triput. Postoje uvjeti koji mogu prekinuti runbook uspješno dovršavanje i povezanih poruka ne obuhvaća sve dodatne informacije u kojoj stoji da se zašto. Ovaj članak sadrži upute za otklanjanje poteškoća za problema vezanih uz Neuspjelo izvršavanje runbook za hibridno Runbook tempiranja.

Ako poteškoću Azure je adresirana ovog članka, posjetite forume o Azure na [MSDN i prelijevanje stogu](https://azure.microsoft.com/support/forums/). Možete objaviti problem na forume za ove ili u [ @AzureSupport na Twitteru](https://twitter.com/AzureSupport). Osim toga, možete datoteka zahtjev za Azure podršku tako da odaberete **Pomoć** na web-mjestu za [podršku Azure](https://azure.microsoft.com/support/options/) .

## <a name="symptom"></a>Simptoma

Izvršavanje Runbook ne uspije, a pogreške vraćena je, "posao akcija"Aktiviraj"nije moguće pokrenuti jer postupak je neočekivano zaustavljena. Akcije zadatka došlo je do pokušaja 3 puta."


## <a name="cause"></a>Uzrok

Postoji nekoliko mogućih razloga pogreška: 

  1. Hibridno tempiranja nalazi iza proxy ili vatrozid
  2. Na računalima sa sustavom je tempiranja hibridnog je manja od minimalne hardverske [preduvjete](automation-hybrid-runbook-worker.md#hybrid-runbook-worker-requirements) 
  3. U runbooks ne može provjeriti autentičnost s Lokalni resursi


## <a name="cause-1-hybrid-runbook-worker-is-behind-proxy-or-firewall"></a>Uzrok 1: Hibridnog Runbook tempiranja nalazi iza proxy ili vatrozid

Na računalima sa sustavom je Runbook tempiranja hibridnog nalazi iza vatrozida ili proxy poslužitelja i izlazni mrežni pristup možda neće biti dopušteno ili ispravno konfigurirano.

### <a name="solution"></a>Rješenja

Provjerite postoji li na računalu izlaznog pristup *. cloudapp.net na priključak 443, 9354 i 30000 30199. 

## <a name="cause-2-computer-has-less-than-minimum-hardware-requirements"></a>Uzrok 2: Računalo ima manje od minimalni preduvjeti

Računala s programom Runbook tempiranja hibridnog treba zadovoljava minimalne hardverske preduvjete prije nego što je za hostiranje značajka određivanje. U suprotnom, ovisno o Upotreba resursa drugih pozadinski procesi i Nadmetanje uzrokovanih runbooks tijekom izvođenja računalo će biti od koristi i uzrokovati runbook posao kašnjenja ili vremenska ograničenja. 

### <a name="solution"></a>Rješenja 

Najprije provjerite je li računalo predviđen za pokretanje značajke hibridnog Runbook tempiranja zadovoljava minimalne hardverske preduvjete.  Ako je tako, praćenje opterećenje procesora i memorije Upotreba da biste odredili sve korelacije između performansi hibridnog Runbook radnih procesa i Windows.  Ako postoji memorije ili tlaka procesora, to može upućivati potrebno nadograditi ili dodajte dodatne procesora ili povećati memorije adresa usko grlo resursa i riješiti pogrešku. Osim toga, odaberite drugu računalnim resursa koji podržava minimalni preduvjeti i skaliranje kada zahtjevima naznačili povećava nije potrebno.         

## <a name="cause-3-runbooks-cannot-authenticate-with-local-resources"></a>Uzrok 3: Runbooks ne može provjeriti autentičnost s Lokalni resursi

### <a name="solution"></a>Rješenja

Odgovarajući događaja s opisom *Win32 postupak napuste kod [4294967295]*potražite u zapisniku događaja **Microsoft SMA** .  Uzrok te pogreške se još niste konfigurirali provjere autentičnosti u vašem runbooks ili naveden vjerodajnice Pokreni kao za grupu radnih hibridnog.  Pregledajte [Runbook dozvole](automation-hybrid-runbook-worker.md#runbook-permissions) da biste potvrdili da ste pravilno konfigurira provjere autentičnosti za vaše runbooks.  


 

