<properties
   pageTitle="Da biste registrirali hostnames pomoću dinamičke DNS-a"
   description="Ova stranica omogućuje pojedinosti o tome kako postaviti dinamički DNS-a da biste registrirali hostnames DNS poslužitelja."
   services="dns"
   documentationCenter="na"
   authors="GarethBradshawMSFT"
   manager="carmonm"
   editor="" />
<tags
   ms.service="dns"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="08/31/2016"
   ms.author="sewhee" />

# <a name="using-dynamic-dns-to-register-hostnames-in-your-own-dns-server"></a>Pomoću dinamičke DNS-a da biste registrirali hostnames vlastite DNS poslužitelj

[Azure nudi naziv rješenje](virtual-networks-name-resolution-for-vms-and-role-instances.md) za virtualnim strojevima (VMs) i uloga instance. No kada treba razlučivost naziv osim onih koje nudi Azure, možete unijeti DNS poslužitelja. Tako ćete dobiti power da biste prilagodili DNS rješenje u skladu s vlastitim potrebama. Na primjer, možda ćete morati pristup lokalnim izvorima putem kontroler domene servisa Active Directory.

Kada prilagođeni DNS poslužitelji nalaze se kao Azure VMs upite naziv glavnog računala za istu vnet možete proslijediti Azure da biste riješili hostnames. Ako ne želite koristiti ovu rutu registrirate vaše hostnames VM u svojem DNS poslužitelju upotrebe dinamički DNS-a.  Azure nema mogućnost (npr. vjerodajnice) da biste stvorili izravno zapisa u DNS poslužitelji tako da je često potrebno zamjenski rasporeda. Evo nekih uobičajenih scenarija s alternative.

## <a name="windows-clients"></a>Klijenti za Windows

Non-domene-pridruženo klijenti Windows pokušati nezaštićenu ažuriranja dinamički DNS (DDNS) kada ih pokretanje ili IP adresa njegova mijenja. Je li DNS naziv glavnog računala plus primarni DNS nastavak. Azure primarni DNS sufiks ostavlja praznima, ali to možete učiniti u VM putem [korisničkog Sučelja](https://technet.microsoft.com/library/cc794784.aspx) ili [pomoću automatizaciju](https://social.technet.microsoft.com/forums/windowsserver/3720415a-6a9a-4bca-aa2a-6df58a1a47d7/change-primary-dns-suffix).

Domene pridruženo klijenti Windows registrirati njihove IP adrese s kontrolerom domene putem sigurne dinamički DNS-a. Postupak domene spoj postavlja primarni DNS sufiks na klijentskom računalu i stvara i održava odnos pouzdanosti.

## <a name="linux-clients"></a>Klijenti Linux

Klijenti Linux obično ne morate registrirati sami DNS poslužitelja pri pokretanju, oni pretpostavlja da ga ne DHCP poslužitelj. Azure, DHCP poslužiteljima ne nudi mogućnost ili vjerodajnice da biste registrirali zapisa u DNS poslužitelj.  Možete koristiti alat s nazivom *nsupdate*, koja se nalazi u paketu vezanja, da biste poslali dinamički DNS ažuriranja. Budući standardizirani je priručniku protokol za dinamičku DNS-a, možete koristiti *nsupdate* čak i ako ne koristite vezanja na DNS poslužitelj.

Možete koristiti spojnica koje nudi DHCP klijent za stvaranje i održavanje unosa naziv glavnog računala na DNS poslužitelj. Tijekom ciklusa DHCP klijent izvršava skripte */etc/dhcp/dhclient-exit-hooks.d/*. To se može koristiti da biste registrirali novi IP adresu pomoću *nsupdate*. Ako, na primjer:

        #!/bin/sh
        requireddomain=mydomain.local

        # only execute on the primary nic
        if [ "$interface" != "eth0" ]
        then
            return
        fi

        # when we have a new IP, perform nsupdate
        if [ "$reason" = BOUND ] || [ "$reason" = RENEW ] ||
           [ "$reason" = REBIND ] || [ "$reason" = REBOOT ]
        then
            host=`hostname`
            nsupdatecmds=/var/tmp/nsupdatecmds
              echo "update delete $host.$requireddomain a" > $nsupdatecmds
              echo "update add $host.$requireddomain 3600 a $new_ip_address" >> $nsupdatecmds
              echo "send" >> $nsupdatecmds

              nsupdate $nsupdatecmds
        fi

        #done
        exit 0;

Da biste izvršili sigurna dinamički DNS ažuriranja možete koristiti i naredbu *nsupdate* . Na primjer, kada koristite povezati DNS poslužitelju, javno privatni ključ par se [generira](http://linux.yyz.us/nsupdate/).  DNS poslužitelj je [konfiguriran](http://linux.yyz.us/dns/ddns-server.html) pomoću javnog dio ključa tako da ga možete provjeriti potpis na zahtjev. Morate koristiti mogućnost *– k* možete unijeti ključ par *nsupdate* redoslijedom za dinamičku DNS ažurirate zahtjev za potpis.

Kada koristite Windows DNS poslužitelju, pomoću provjere autentičnosti Kerberos s parametrom *– g* u *nsupdate* (nije dostupno u verziji sustava Windows *nsupdate*). Da biste to učinili, koristite *kinit* da biste učitali vjerodajnice (npr. iz [datoteke keytab](http://www.itadmintools.com/2011/07/creating-kerberos-keytab-files.html)). Zatim *nsupdate g* će obraditi vjerodajnice iz predmemorije.

Ako je potrebno, DNS sufiks pretraživanja možete dodati na VMs. DNS sufiks je naveden u datoteci */etc/resolv.conf* . Većina Linux distros automatski upravljanje sadržajem ove datoteke obično tako da ne možete uređivati ga. Međutim, možete nadjačati sufiks pomoću naredbe *zamijeniti* DHCP klijent. Da biste to učinili, u */etc/dhcp/dhclient.conf*, dodajte:

        supersede domain-name <required-dns-suffix>;

