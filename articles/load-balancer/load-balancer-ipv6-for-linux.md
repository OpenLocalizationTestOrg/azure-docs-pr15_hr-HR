<properties
    pageTitle="Konfiguriranje DHCPv6 za Linux VMs | Microsoft Azure"
    description="Kako konfigurirati DHCPv6 Linux VMs."
    services="load-balancer"
    documentationCenter="na"
    authors="sdwheeler"
    manager="carmonm"
    editor=""
    keywords="IPv6, azure opterećenja, dvostruki snop, javnu ip, izvorni ipv6, mobile, iot"
/>
<tags
    ms.service="load-balancer"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="infrastructure-services"
    ms.date="09/14/2016"
    ms.author="sewhee"
/>

# <a name="configuring-dhcpv6-for-linux-vms"></a>Konfiguriranje DHCPv6 za Linux VMs

Neki Linux virtualnog računala slike iz trgovine Azure nemaju DHCPv6 po zadanim postavkama konfiguriran. Da biste podržava IPv6, DHCPv6 mora biti konfigurirano u u raspodjele Linux OS koju koristite. Distribucija različite Linux imaju različiti načini konfiguriranja DHCPv6 jer se koriste različite pakete.

>[AZURE.NOTE] Nedavne SUSE Linux i CoreOS slike u trgovine Windows Azure je unaprijed konfigurirana s DHCPv6. Bez dodatnih promjena potrebni su prilikom korištenja tim slikama.

U ovom dokumentu opisuje kako omogućiti DHCPv6 tako da se vaš Linux virtualnog računala dohvaća IPv6 adrese.

>[AZURE.WARNING] Pogrešno uređivanje datoteka konfiguracije mreže mogu prouzročiti izgubiti pristup mreži na vašem VM. Preporučujemo da testirate promjene konfiguracije sustavima koji nisu proizvodnje. Upute u ovom članku testirate na najnoviju verziju slike Linux iz trgovine Windows Azure. Pročitajte dokumentaciju za određeni verziji sustava Linux detaljnije upute.

## <a name="ubuntu"></a>Ubuntu

1. Uređivanje datoteke `/etc/dhcp/dhclient6.conf` i dodajte sljedeći redak:

        timeout 10;

2. Uređivanje mrežnoj konfiguraciji eth0 sučelja u konfiguraciji sljedeće:

    * Na **Ubuntu 12.04 i 14.04**, uređivati datoteku`/etc/network/interfaces.d/eth0.cfg`
    * Na **Ubuntu 16.04**uređivati datoteku`/etc/network/interfaces.d/50-cloud-init.cfg`

    ```bash
    iface eth0 inet6 auto
        up sleep 5
        up dhclient -1 -6 -cf /etc/dhcp/dhclient6.conf -lf /var/lib/dhcp/dhclient6.eth0.leases -v eth0 || true
    ```

3. Obnavljanje IPv6 adrese:

    ```bash
    # sudo ifdown eth0 && sudo ifup eth0
    ```

## <a name="debian"></a>Debian

1. Uređivanje datoteke `/etc/dhcp/dhclient6.conf` i dodajte sljedeći redak:

        timeout 10;

2. Uređivanje datoteke `/etc/network/interfaces` i dodajte sljedeće konfiguracije:

        iface eth0 inet6 auto
            up sleep 5
            up dhclient -1 -6 -cf /etc/dhcp/dhclient6.conf -lf /var/lib/dhcp/dhclient6.eth0.leases -v eth0 || true

3. Obnavljanje IPv6 adrese:

    ```bash
    # sudo ifdown eth0 && sudo ifup eth0
    ```

## <a name="rhel--centos--oracle-linux"></a>RHEL / CentOS / Oracle Linux

1. Uređivanje datoteke `/etc/sysconfig/network` i dodajte parametar sljedeće:

        NETWORKING_IPV6=yes

2. Uređivanje datoteke `/etc/sysconfig/network-scripts/ifcfg-eth0` i dodajte sljedeća dva parametra:

        IPV6INIT=yes
        DHCPV6C=yes

3. Obnavljanje IPv6 adrese:

    ```bash
    # sudo ifdown eth0 && sudo ifup eth0
    ```

## <a name="sles-11--opensuse-13"></a>SLES 11 & openSUSE 13

Nedavno SLES i openSUSE slike u Azure je unaprijed konfigurirana s DHCPv6. Bez dodatnih promjena potrebni su prilikom korištenja tim slikama. Ako imate VM koji se temelji na stariju ili prilagođeni SUSE slike, poduzmite sljedeće korake:

1. Instalacija na `dhcp-client` pakiranje, ako je potrebno:

    ```bash
    # sudo zypper install dhcp-client
    ```

2. Uređivanje datoteke `/etc/sysconfig/network/ifcfg-eth0` i dodajte parametar sljedeće:

        DHCLIENT6_MODE='managed'

3. Obnavljanje IPv6 address:

    ```bash
    # sudo ifdown eth0 && sudo ifup eth0
    ```

## <a name="sles-12-and-opensuse-leap"></a>SLES 12 i openSUSE Skok

Nedavno SLES i openSUSE slike u Azure je unaprijed konfigurirana s DHCPv6. Nema dodatne promjene su potrebne prilikom upotrebe tim slikama. Ako imate VM koji se temelji na stariju ili prilagođeni SUSE slike, poduzmite sljedeće korake:

1. Uređivanje datoteke `/etc/sysconfig/network/ifcfg-eth0` i zamijeni taj parametar

        #BOOTPROTO='dhcp4'

    pomoću sljedeće vrijednosti:

        BOOTPROTO='dhcp'

2. Dodajte sljedeće parametar `/etc/sysconfig/network/ifcfg-eth0`:

        DHCLIENT6_MODE='managed'

3. Obnavljanje IPv6 address:

    ```bash
    # sudo ifdown eth0 && sudo ifup eth0
    ```

## <a name="coreos"></a>CoreOS

Nedavno CoreOS slike u Azure je unaprijed konfigurirana s DHCPv6. Nema dodatne promjene su potrebne prilikom upotrebe tim slikama. Ako imate VM koji se temelji na stariju ili prilagođeni CoreOS slike, poduzmite sljedeće korake:

1. Uređivanje datoteke`/etc/systemd/network/10_dhcp.network`

        [Match]
        eth0

        [Network]
        DHCP=ipv6

2. Obnavljanje IPv6 address:

    ```bash
    # sudo systemctl restart systemd-networkd
    ```
