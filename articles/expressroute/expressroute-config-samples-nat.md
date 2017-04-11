<properties
   pageTitle="ExpressRoute kupca usmjerivač konfiguracije uzoraka | Microsoft Azure"
   description="Ova stranica sadrži usmjerivač konfiguracije uzorka za Cisco i tvrtke Juniper usmjerivača."
   documentationCenter="na"
   services="expressroute"
   authors="cherylmc"
   manager="carmonm"
   editor="" />
<tags
   ms.service="expressroute"
   ms.devlang="na"
   ms.topic="article" 
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/10/2016"
   ms.author="cherylmc"/>

# <a name="router-configuration-samples-to-setup-and-manage-nat"></a>Uzorci konfiguracije usmjerivača za postavljanje i upravljanje NAT

Ova stranica sadrži NAT konfiguracije uzorka za Cisco ASA i tvrtke Juniper SRX usmjerivača niz. Te su namijenjen uzoraka smjernicama samo i mora se koristiti kao što je. Možete raditi s prodavaču kako biste pojavljuju se s odgovarajućim konfiguracijama za mrežu. 

>[AZURE.IMPORTANT] Uzorci na ovoj stranici trebale biti isključivo smjernicama. Morate raditi s tim prodaje / tehničke proizvođaču tog i umrežavanje tim pojavljuju se s odgovarajućim konfiguracijama prema svojim potrebama. Microsoft neće podržavati problema vezanih uz konfiguracije naveden na ovoj stranici. Morate se obratiti proizvođaču tog softvera uređaj za probleme u podršci.

Usmjerivač konfiguracije uzoraka ispod primjenjuju se na javno Azure i Microsoft peerings. Ne morate konfigurirati NAT Azure privatne peering. Pregledajte [ExpressRoute peerings](expressroute-circuit-peerings.md) i [preduvjeti za ExpressRoute NAT](expressroute-nat.md) za više detalja.

**Bilješke:** MORATE koristiti odvojene NAT IP grupe za povezivanje s Internetom i ExpressRoute. Korištenje iste skupna NAT IP putem Interneta i ExpressRoute rezultirat će asimetričnim usmjeravanje i gubitka veze.

## <a name="cisco-asa-firewalls"></a>Cisco ASA vatrozidima

### <a name="pat-configuration-for-traffic-from-customer-network-to-microsoft"></a>Konfiguriranje PAT radi prometa s klijentima mreže Microsoft

    object network MSFT-PAT
      range <SNAT-START-IP> <SNAT-END-IP>
    
    
    object-group network MSFT-Range
      network-object <IP> <Subnet_Mask>
    
    object-group network on-prem-range-1
      network-object <IP> <Subnet-Mask>
    
    object-group network on-prem-range-2
      network-object <IP> <Subnet-Mask>
    
    object-group network on-prem
      network-object object on-prem-range-1
      network-object object on-prem-range-2
    
    nat (outside,inside) source dynamic on-prem pat-pool MSFT-PAT destination static MSFT-Range MSFT-Range

### <a name="pat-configuration-for-traffic-from-microsoft-to-customer-network"></a>Konfiguracija PAT promet od Microsofta mrežu klijenta

#### <a name="interfaces-and-direction"></a>Sučelja i smjer:
    Source Interface (where the traffic enters the ASA): inside
    Destination Interface (where the traffic exits the ASA): outside

#### <a name="configuration"></a>Konfiguracija:
Skup NAT:

    object network outbound-PAT
        host <NAT-IP>

Odredišni poslužitelj:

    object network Customer-Network
        network-object <IP> <Subnet-Mask>

Grupe objekata za klijenta IP adrese

    object-group network MSFT-Network-1
        network-object <MSFT-IP> <Subnet-Mask>
    
    object-group network MSFT-PAT-Networks
        network-object object MSFT-Network-1

Naredbe za NAT:

    nat (inside,outside) source dynamic MSFT-PAT-Networks pat-pool outbound-PAT destination static Customer-Network Customer-Network


## <a name="juniper-srx-series-routers"></a>Tvrtke Juniper SRX niz usmjerivača 

### <a name="1-create-redundant-ethernet-interfaces-for-the-cluster"></a>1. stvorili suvišnih Ethernet sučelja za klaster

    interfaces {
        reth0 {
            description "To Internal Network";
            vlan-tagging;
            redundant-ether-options {
                redundancy-group 1;
            }
            unit 100 {
                vlan-id 100;
                family inet {
                    address <IP-Address/Subnet-mask>;
                }
            }
        }
        reth1 {
            description "To Microsoft via Edge Router";
            vlan-tagging;
            redundant-ether-options {
                redundancy-group 2;
            }
            unit 100 {
                description "To Microsoft via Edge Router";
                vlan-id 100;
                family inet {
                    address <IP-Address/Subnet-mask>;
                }
            }
        }
    }


### <a name="2-create-two-security-zones"></a>2. stvaranje dva sigurnosne zone

 - Pouzdanost Zone internoj mreži i Untrust Zone za vanjski mrežni nasuprotne usmjerivača rub
 - Dodjela odgovarajuće sučelja zona
 - Dopusti usluge na sučelja


    sigurnosne zone {{sigurnosne zone pouzdanost {glavno računalo-dolazni-promet {-servisa sustava {ping;                  } {bgp; protokola                  sučelja}} {reth0.100;              }} sigurnosne zone Untrust {glavno računalo-dolazni-promet {-servisa sustava {ping;                  } {bgp; protokola                  sučelja}} {reth1.100;              }          }      }  }


### <a name="3-create-security-policies-between-zones"></a>3. stvorite sigurnosne pravilnike između zona
 
    security {
        policies {
            from-zone Trust to-zone Untrust {
                policy allow-any {
                    match {
                        source-address any;
                        destination-address any;
                        application any;
                    }
                    then {
                        permit;
                    }
                }
            }
            from-zone Untrust to-zone Trust {
                policy allow-any {
                    match {
                        source-address any;
                        destination-address any;
                        application any;
                    }
                    then {
                        permit;
                    }
                }
            }
        }
    }


### <a name="4-configure-nat-policies"></a>4. konfiguriranje pravilnika o NAT
 - Stvorite dva NAT grupe. Jedan će se koristiti za NAT promet izlazni Microsoftu i druge tvrtke Microsoft kupcu.
 - Stvaranje pravila za NAT odgovarajući promet

        security {
            nat {
                source {
                    pool SNAT-To-ExpressRoute {
                        routing-instance {
                            External-ExpressRoute;
                        }
                        address {
                            <NAT-IP-address/Subnet-mask>;
                        }
                    }
                    pool SNAT-From-ExpressRoute {
                        routing-instance {
                            Internal;
                        }
                        address {
                            <NAT-IP-address/Subnet-mask>;
                        }
                    }
                    rule-set Outbound_NAT {
                        from routing-instance Internal;
                        to routing-instance External-ExpressRoute;
                        rule SNAT-Out {
                            match {
                                source-address 0.0.0.0/0;
                            }
                            then {
                                source-nat {
                                    pool {
                                        SNAT-To-ExpressRoute;
                                    }
                                }
                            }
                        }
                    }
                    rule-set Inbound-NAT {
                        from routing-instance External-ExpressRoute;
                        to routing-instance Internal;
                        rule SNAT-In {
                            match {
                                source-address 0.0.0.0/0;
                            }
                            then {
                                source-nat {
                                    pool {
                                        SNAT-From-ExpressRoute;
                                    }
                                }
                            }
                        }
                    }
                }
            }
        }


### <a name="5-configure-bgp-to-advertise-selective-prefixes-in-each-direction"></a>5. konfiguriranje BGP da biste Oglasite selektivno prefiksi u svakom smjeru

Pogledajte uzoraka u [usmjeravanja konfiguracije uzoraka](expressroute-config-samples-routing.md) stranice.

### <a name="6-create-policies"></a>6. stvoriti pravila

    routing-options {
                  autonomous-system <Customer-ASN>;
    }
    policy-options {
        prefix-list Microsoft-Prefixes {
            <IP-Address/Subnet-Mask;
            <IP-Address/Subnet-Mask;
        }
        prefix-list private-ranges {
            10.0.0.0/8;
            172.16.0.0/12;
            192.168.0.0/16;
            100.64.0.0/10;
        }
        policy-statement Advertise-NAT-Pools {
            from {
                protocol static;
                route-filter <NAT-Pool-Address/Subnet-mask> prefix-length-range /32-/32;
            }
            then accept;
        }
        policy-statement Accept-from-Microsoft {
            term 1 {
                from {
                    instance External-ExpressRoute;
                    prefix-list-filter Microsoft-Prefixes orlonger;
                }
                then accept;
            }
            term deny {
                then reject;
            }
        }
        policy-statement Accept-from-Internal {
            term no-private {
                from {
                    instance Internal;
                    prefix-list-filter private-ranges orlonger;
                }
                then reject;
            }
            term bgp {
                from {
                    instance Internal;
                    protocol bgp;
                }
                then accept;
            }
            term deny {
                then reject;
            }
        }
    }
    routing-instances {
        Internal {
            instance-type virtual-router;
            interface reth0.100;
            routing-options {
                static {
                    route <NAT-Pool-IP-Address/Subnet-mask> discard;
                }
                instance-import Accept-from-Microsoft;
            }
            protocols {
                bgp {
                    group customer {
                        export <Advertise-NAT-Pools>;
                        peer-as <Customer-ASN-1>;
                        neighbor <BGP-Neighbor-IP-Address>;
                    }
                }
            }
        }
        External-ExpressRoute {
            instance-type virtual-router;
            interface reth1.100;
            routing-options {
                static {
                    route <NAT-Pool-IP-Address/Subnet-mask> discard;
                }
                instance-import Accept-from-Internal;
            }
            protocols {
                bgp {
                    group edge-router {
                        export <Advertise-NAT-Pools>;
                        peer-as <Customer-Public-ASN>;
                        neighbor <BGP-Neighbor-IP-Address>;
                    }
                }
            }
        }
    }

## <a name="next-steps"></a>Daljnji koraci

[Najčešća pitanja vezana uz ExpressRoute](expressroute-faqs.md) dodatne pojedinosti potražite u članku.
