<properties
   pageTitle="Najbolje prakse za softverska ažuriranja na Microsoft Azure IaaS | Microsoft Azure"
   description="Članak sadrži skup najbolje prakse za softverska ažuriranja u okruženju sustava Microsoft Azure IaaS.  Je namijenjen za IT profesionalce i sigurnost analitičar analitičar podataka koji se baviti Promijeni kontrolu, softver ažuriranja i resursa upravljanje po danu, uključujući one koji su odgovorni za sigurnost i usklađenost naporima svoje tvrtke ili ustanove."
   services="security"
   documentationCenter="na"
   authors="YuriDio"
   manager="swadhwa"
   editor=""/>

<tags
   ms.service="security"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="10/18/2016"
   ms.author="yurid"/>

# <a name="best-practices-for-software-updates-on-microsoft-azure-iaas"></a>Najbolje prakse za softverska ažuriranja na Microsoft Azure IaaS

Prije nego čitanje bilo kakvu vrstu rasprave na najbolje prakse za Azure [IaaS](https://azure.microsoft.com/overview/what-is-iaas/) okruženja, dobro je razumjeti što Scenariji su koje su vam upravljanje softverska ažuriranja i u odgovornostima. Dijagramu u nastavku potrebno pomoću kojih se objašnjava te ograničenja:

![Oblak modela i responsabilities](./media/azure-security-best-practices-software-updates-iaas/sec-cloudstack-new.png)

Lijevi stupac prikazuje sedam obaveze (definirano u odjeljcima) koje tvrtke ili ustanove razmotrite, koje pridonose sigurnosti i privatnosti računalno okruženje.
 
Klasifikacija podataka i odgovornost i klijentski & točka zaštite obaveze koje su isključivo domene klijenata te fizičke, glavno računalo i mrežne obaveze u domeni oblaka davateljima usluga u modelima PaaS i SaaS. 

Preostali obaveze se zajednički koriste između korisnika i cloud davateljima usluga. Neke obaveze potrebni CSP i klijenta za upravljanje i upravljati odgovornost zajedno, uključujući nadzor svoje domene. Ako, na primjer, razmotrite identiteta i pristup upravljanje prilikom korištenja servisa Azure Active Directory Services; Konfiguracija servisa kao što su višestruka provjera autentičnosti je najviše klijent, ali osiguravanje učinkovitih funkcionalnost odgovornosti Microsoft Azure.

> [AZURE.NOTE] Dodatne informacije o zajedničkom obaveze u oblaku, pročitajte [Zajedničko obaveze za računalstvo u Oblaku](https://gallery.technet.microsoft.com/Shared-Responsibilities-81d0ff91/file/153019/1/Shared%20responsibilities%20for%20cloud%20computing.pdf) 

Ti isti način odnosi na scenarija hibridnog gdje vaša tvrtka koristi Azure IaaS VMs koji komunikaciju s lokalnim resursima kao što je prikazano u dijagramu u nastavku.

![Uobičajeni hibridnog scenarij Microsoft Azure](./media/azure-security-best-practices-software-updates-iaas/sec-azconnectonpre.png)

## <a name="initial-assessment"></a>Početna procjenu

Čak i ako vaša tvrtka već koristite u sustavu za upravljanje ažuriranja i već imate pravila za ažuriranje softvera na mjestu, važno je često ponovo posjetite prethodne konfiguracije pravila i ih svojim potrebama trenutni ažurirati. To znači da morate biti upoznati s trenutnim stanjem resursa u svojoj tvrtki. Da biste dosegne to stanje, morate znati:

-   Na fizičkih i virtualnog računala u tvrtki.

-   Operacijski sustavi i verzije koji se izvodi na svakoj od tih fizičkih i virtualnog računala.

-   Softverska ažuriranja trenutno instalirani na svakom računalu (verzija paket servisa, softverska ažuriranja i ostalih izmjena).

-   Funkcija na svakom računalu izvodi u tvrtki.

-   Aplikacije i programe koji se izvode na svakom računalu.

-   Vlasništvo i kontakt informacije za svako računalo.

-   Imovinu prisutnih u okruženju sustava i njihove relativni vrijednost da biste odredili koja će se područja potrebne su vam najviše pažnje i zaštitu.

-   Problemi sa sigurnosnim poznati i procese vaša tvrtka ima ovlasti za identifikacijski novi sigurnosnim problemima ili promjena u razine sigurnosti.

-   Protumjere koji je uveden radi zaštite vaše okruženje.

Trebali biste redovito ažuriranje te podatke, a mora biti uvijek su na raspolaganju za te koji je uključen u procesu upravljanje ažuriranjem softvera.

## <a name="establish-a-baseline"></a>Uspostavljanje referentne vrijednosti

Važan dio postupka za upravljanje ažuriranja softver stvara početne standardne instalacije verzija operacijskog sustava, aplikacije i hardver za računala u vašoj tvrtki one se nazivaju referente vrijednosti. Referentne vrijednosti je konfiguracija proizvoda ili sustava uspostaviti na određenom mjestu u vremenu. Aplikaciju ili osnovne operacijski sustav, na primjer, omogućuje ponovno stvaranje računala ili servisa u određenim stanje.

Referente vrijednosti navedite osnovu za traženje i rješavanje mogućih problema i pojednostavili postupak upravljanja ažuriranje softver smanjivanjem broj softverska ažuriranja morate uvesti u tvrtki i povećanjem omogućuje praćenje usklađenosti.

Nakon izvršavanja početne nadzora tvrtke, trebali biste koristiti podatke koji je dobiven od nadzora da biste definirali radu osnovne za IT komponente vašeg radnog okruženja. Broj referente vrijednosti može biti obavezno, ovisno o različitim vrstama hardvera i softvera u uveden u radni.

Na primjer, neki poslužitelji zahtijevaju ažuriranje softver da biste spriječili da ih viseće prilikom unosa isključivanja kada izvodi Windows Server 2012. Referentne vrijednosti za ove poslužitelje trebali biste uključiti to ažuriranje softvera.

U velikim tvrtkama i ustanovama, korisno je računala u vašoj tvrtki podijelite kategorije imovine i zadržati sve kategorije na standardni osnovne pomoću istih verzija softvera i softverska ažuriranja. Možete koristiti te kategorije resursa u čimbenika raspodjele ažuriranje softvera.

## <a name="subscribe-to-the-appropriate-software-update-notification-services"></a>Pretplata na servise za obavijesti odgovarajući softver ažuriranje

Kad dovršite reviziji početne softvera koristi u tvrtki, trebali biste odredite najbolji način za primanje obavijesti o novih ažuriranja softver za svaki proizvod softver i verziju. Ovisno o proizvodu softver najbolji način obavijesti može biti obavijesti e-pošte, web-mjesta ili publikacije na računalu.

Ako, na primjer, Microsoft sigurnost odgovor centra (MSRC) odgovori svi problemi vezani uz sigurnost o Microsoftovim proizvodima i njihovi Microsoft oglasne servis sigurnosnih, obavijest besplatne e-pošte upravo identificirani slabe točke i softverska ažuriranja objavljivanja za rješavanje tih slabe točke. Možete se pretplatiti na taj servis pri http://www.microsoft.com/technet/security/bulletin/notify.mspx.

## <a name="software-update-considerations"></a>Razmatranja ažuriranje softvera

Kad dovršite početne nadzora softvera koristi u tvrtki, određujete preduvjeti za instalaciju softvera ažuriranje sustav upravljanja, a to ovisi o sustavu upravljanja softver ažuriranje koji koristite. WSUS Saznajte [Najbolje prakse sa servisa Windows Server Update](https://technet.microsoft.com/library/Cc708536), za centar sustava u članku [Planiranje softverska ažuriranja u upravitelju konfiguracije](https://technet.microsoft.com/library/gg712696).

No postoje neke Općenita razmatranja i najbolje prakse koje možete primijeniti bez obzira na to rješenje koje koristite kao što je prikazano u odjeljcima koji slijedi.

### <a name="setting-up-the-environment"></a>Postavljanje okruženja

Imajte na umu sljedeće prakse prilikom planiranja postavljanje softvera ažuriranje okruženje za upravljanje:

-   **Stvaranje radnog softver ažuriranje zbirki na temelju kriterija stabilan**: Općenito, korištenje stabilan kriterija za stvaranje zbirki za softver ažurirati zalihe i distribucija će pomoći da biste pojednostavnili sve faza procesa za upravljanje ažuriranje softvera. Kriterij stabilan možete uključiti verzija operacijskog sustava instaliranih klijenta i razine servisnog paketa, ulogu sustava ili ciljnoj tvrtki ili ustanovi.

-   **Stvaranje stara radnog zbirki koji uključuje računala referenca**: zbirke prije proizvodnje uključuje predstavniku konfiguracije verzija operacijskog sustava, redak softver tvrtke i drugih softver koji se izvodi u tvrtki.

Trebali biste razmotriti gdje ažuriranje poslužitelja softver nalazit će se, ako je bit će u Azure IaaS infrastrukture u oblaku ili bit će lokalni. To je važno odluka jer je potrebno da biste procijenili količinu prometa između lokalnog resursa i Azure infrastrukture. Dodatne informacije o povezivanju infrastruktura za lokalni Azure pročitajte [lokalne mreže za Microsoft Azure virtualne mreže za povezivanje](https://technet.microsoft.com/library/Dn786406.aspx) .

Mogućnosti dizajna kojim se određuje gdje ažuriranje poslužitelja nalazit će se također će se razlikovati prema infrastruktura za trenutni i sustava ažuriranje softver koji trenutno koristite. WSUS čitati [Implementacija poslužitelja servisa Windows Update iz vaše tvrtke ili ustanove](https://technet.microsoft.com/library/hh852340.aspx) i za Upravitelj konfiguracije centar sustava pročitajte [Planiranje web-mjesta i hijerarhije u upravitelju konfiguracije](https://technet.microsoft.com/library/Gg712681.aspx).

### <a name="backup"></a>Sigurnosno kopiranje

Redovito sigurnosno kopiranje su važne ne samo za softver ažuriranje upravljanje platformu sam već i za poslužitelje koji će se ažurirati. Potrebno je tvrtkama ili ustanovama koje su u [Promjena proces upravljanja](https://technet.microsoft.com/library/cc543216.aspx) na mjestu IT za poravnanje razloga zašto je poslužitelj treba ažurirati, Procijenjena nedostupnost i moguće utjecaja. Da biste bili sigurni da imate vraćanje konfiguracije na mjestu u slučaju da je ažuriranje ne uspije, provjerite redovito sigurnosno kopirate u sustavu.

Neke sigurnosne mogućnosti za Azure IaaS obuhvaćaju sljedeće:

-   [Azure IaaS radno opterećenje zaštitu pomoću upravitelja za zaštitu podataka](https://azure.microsoft.com/blog/2014/09/08/azure-iaas-workload-protection-using-data-protection-manager/)

-   [Stvaranje sigurnosne kopije Azure virtualnim strojevima](../backup/backup-azure-vms.md)

### <a name="monitoring"></a>Nadzor

Trebale bi funkcionirati u pravilnim izvješća praćenje broj nedostaju ili instalirati ažuriranja i ažuriranja sa statusom nepotpuna, za svako ažuriranje softver koji je ovlašteni. Isto tako, izvješćivanje o softverska ažuriranja koja nisu još ovlašteni možete olakšati jednostavnije odluke implementacije.

Također razmotrite sljedeće zadatke:

-   Vođenje reviziju primjenjivo i instaliranih sigurnosna ažuriranja za sva računala u vašoj tvrtki.

-   Autorizirajte i implementacija ažuriranja odgovarajuće računalima.

-   Praćenje zaliha i ažuriranje instalacije stanje i napredak na svim računalima u svojoj tvrtki.

Nadalje na Općenita razmatranja koji su objašnjenje u ovom se članku također razmotrite svaki proizvod je najbolje vježbali, na primjer: Ako imate na VM Azure sa sustavom SQL Server, provjerite je li prate preporuke ažuriranja softver za taj proizvod.

## <a name="next-steps"></a>Daljnji koraci

Koristiti smjernice opisane u ovom članku kao pomoć u određivanju najbolje mogućnosti za softverska ažuriranja za virtualnim strojevima unutar Azure IaaS. Postoje mnoge sličnosti između softver ažuriranje najbolje prakse u tradicionalni podatkovnog centra nasuprot Azure IaaS, stoga preporučuje se procijenite trenutni pravila ažuriranje softvera uvrstiti Azure VMs i uključujete odgovarajući najbolja iskustva iz ovog članka u cjelokupni proces ažuriranje softvera.
