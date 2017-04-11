<properties
    pageTitle="Implementacija aplikacije na skupovima skaliranje virtualnog računala | Microsoft Azure"
    description="Implementacija aplikacije na skupovima skaliranje virtualnog računala"
    services="virtual-machine-scale-sets"
    documentationCenter=""
    authors="gbowerman"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machine-scale-sets"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/26/2016"
    ms.author="guybo"/>

# <a name="deploy-an-app-on-virtual-machine-scale-sets"></a>Implementacija aplikacije na virtualnog računala skaliranje skupovima

Aplikacije koje se izvode na postavljena na skaliranje VM obično je uveden u jedan od tri načina:

- Instaliranje novog softvera na platformi slika trenutku implementacije. Slika platformu u ovom kontekstu je operacijski sustav slika iz trgovine Azure, kao što je Ubuntu 16.04, Windows Server 2012 R2 itd.

Novi softver možete instalirati na sliku platformu korištenje [VM nastavak](../virtual-machines/virtual-machines-windows-extensions-features.md). Nastavkom VM je softver koji se pokreće kada je implementiran u VM. Možete pokrenuti kod koji vam se sviđa u implementaciju trenutku tijekom korištenja nastavkom prilagođene skripte. [Ovo](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-lapstack-autoscale) je primjer Azure resursima predložak s dva VM: Linux prilagođene skripte proširenja za instalaciju Apache i PHP, i dijagnostičkih proširenje slanje podataka o performansama koristi automatsko skaliranje Azure.

Prednost takvog je li stupanj odvojenosti između kodu aplikacije i OS-a, a možete zasebno održavati aplikacije. Naravno da su i više premještanje dijelova i vrijeme uvođenja VM može biti duže ako postoji mnogo potražite skriptu da biste preuzeli i konfiguriranje.

**Ako povjerljive podatke u naredbu prilagođene skripte nastavak (kao što su lozinka), ne zaboravite da biste odredili na `commandToExecute` u na `protectedSettings` atribut prilagođene skripte proširenje umjesto na `settings` atribut.**

- Stvorite prilagođene VM sliku koja sadrži os-a i aplikacije u jednom VHD. Ovdje skup skaliranje sastoji se od skupa VMs kopirana slika koje ste načinili vi, koje morate zadržati. Taj se način potrebna je bez dodatni konfiguracija trenutku VM implementacije. No u na `2016-03-30` verzije skupova skaliranje VM (i ranijim verzijama), diskova OS za VMs u skupu skaliranje ograničeni su na prostor za pohranu za jedan račun. Dakle, sadržavati najviše 40 VMs u skupu mjerilo, za razliku od 100 VM po postavljanje ograničenja platforme slikama. Dodatne informacije potražite u članku [Pregled dizajn postavite skaliranje](./virtual-machine-scale-sets-design-overview.md) .

- Implementacija platforma ili prilagođenu sliku koja je zapravo spremnik glavno računalo i instalirajte aplikaciju kao jedan ili više spremnika kojima upravljate s orchestrator ili konfiguracija za upravljanje. Bolje stvar o takvog je imati izvučene infrastruktura za oblak iz aplikacijskom sloju te ih možete zasebno održavati.

## <a name="what-happens-when-a-vm-scale-set-scales-out"></a>Što se događa kada postavite na skaliranje VM mijenja veličinu odgovor?

Kada dodate jedan ili više VMs skalu postaviti tako da joj povećate kapacitet – li ručno ili putem automatsko skaliranje – program automatski će se instalira. Za ako skale postavljeno je primjeru nastavci definirani, oni se izvoditi na novu VM prilikom svakog stvaranja. Ako skup skaliranje temelji se na prilagođenu sliku, sve nove VM je kopiju izvornog prilagođenu sliku. Ako postavite skaliranje VMs su domaćini spremnik, a zatim možda imaju pokretanje koda za učitavanje spremnika u nastavkom prilagođene skripte ili datotečni nastavak mogu instalirati agent registrira s orchestrator klaster (kao što je servis spremnik za Azure).

## <a name="how-do-you-manage-application-updates-in-vm-scale-sets"></a>Kako upravljati aplikacije ažuriranja u skupove skaliranje VM

Ažuriranja aplikacije u skupove skaliranje VM, tri glavna pristupa slijedite na tri načina za implementaciju servisa prethodnom aplikacije:

* Ažuriranje s datotečnim nastavcima VM. Proširenja VM koji su definirani za VM skaliranje skup se provode svaki put kada je novi VM implementiran, postojeće VM je reimaged ili nastavkom VM ažurira. Ako je potrebno ažurirati aplikaciju izravno ažuriranje aplikacije putem proširenja je izgledna pristup – jednostavno ažurirati definiciju kućni broj. Jedan jednostavan način za to je tako da promijenite fileUris na novi softver.

* Pristup immutable prilagođenu sliku. Kada kolača aplikacije (ili aplikacija komponente) u VM sliku možete se fokusirati na gradnje pouzdanog kanala da biste automatizirali Sastavi, testiranje i implementaciju slike. Možete dizajnirati svoje arhitektura da biste olakšali brzog zamjena skupa postupnu skaliranje u radni. Dobar primjer takvog je [upravljački program za Azure Spinnaker rad](https://github.com/spinnaker/deck/tree/master/app/scripts/modules/azure) - [http://www.spinnaker.io/](http://www.spinnaker.io/).

Packer i Terraform podržavaju Azure Voditelj resursa, da biste definirali slike "kao kod" i stvaraju u Azure, zatim se poslužite se VHD u vaš skaliranje skup. Međutim, na taj način tako da bi postaju problematičnog za tržište slike, gdje proširenja/prilagođene skripte postaju važnijih jer ne izravno rukovati bitova iz trgovine.

* Ažurirajte spremnika. Apstraktnog životni ciklus Upravljanje aplikacijama za jednu razinu iznad infrastrukture oblaka, primjerice encapsulating aplikacije i aplikacija komponente u spremnika i upravljajte njima te spremnika putem orchestrators spremnik i upravitelja konfiguracije kao što su Chef/Puppet.

Skaliranje postavite VMs postaju stabilan substrate za spremnike i samo zahtijevaju Povremeni sigurnosnih i ažuriranja vezanih uz OS. Kao što je rečeno, spremnik servisa Azure dobar je primjer koriste ovim pristupom i izrade servisa oko njega.

## <a name="how-do-you-roll-out-an-os-update-across-update-domains"></a>Kako učiniti vam uvođenju ažuriranje OS preko ažuriranje domene?

Pretpostavimo da želite ažurirati OS sliku uz zadržavanje pokretanje postavite skaliranje VM. Da biste to učinili je da biste ažurirali na VM slike VM jedan po jedan. To možete učiniti pomoću programa PowerShell ili Azure EŽA. Postoje zasebne naredbe da biste ažurirali VM skaliranje postavljanje modela (kako svoju konfiguraciju definiran) i problema "ručni nadogradnje" pozive u pojedinačne VMs.

[Ovo](https://github.com/gbowerman/vmsstools) je primjer Python skripte za automatiziranje postupak ažuriranja postavite skaliranje VM jedan ažuriranje domene odjednom. (Caveat: više dokaz o pojam od hardened rješenje podrška za radni je – možda ćete morati dodati neke pogreške provjere itd.).
