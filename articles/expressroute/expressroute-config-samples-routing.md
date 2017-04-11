<properties
   pageTitle="ExpressRoute kupca usmjerivač konfiguracije uzoraka | Microsoft Azure"
   description="Ova stranica sadrži usmjerivač config uzorka za Cisco i tvrtke Juniper usmjerivača."
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

# <a name="router-configuration-samples-to-setup-and-manage-routing"></a>Uzorci konfiguracije usmjerivača za postavljanje i upravljanje usmjeravanje

Ova stranica sadrži sučelje i usmjeravanje konfiguracije uzorka za Cisco IOS XE i tvrtke Juniper MX usmjerivača niz. Te su namijenjen uzoraka smjernicama samo i mora se koristiti kao što je. Možete raditi s prodavaču kako biste pojavljuju se s odgovarajućim konfiguracijama za mrežu. 

>[AZURE.IMPORTANT] Uzorci na ovoj stranici trebale biti isključivo smjernicama. Morate raditi s tim prodaje / tehničke proizvođaču tog i umrežavanje tim pojavljuju se s odgovarajućim konfiguracijama prema svojim potrebama. Microsoft neće podržavati problema vezanih uz konfiguracije naveden na ovoj stranici. Morate se obratiti proizvođaču tog softvera uređaj za probleme u podršci.

Usmjerivač konfiguracije uzoraka ispod primjenjuju se na sve peerings. Dodatne informacije o usmjeravanju pregledajte [ExpressRoute peerings](expressroute-circuit-peerings.md) i [preduvjeti za usmjeravanje ExpressRoute](expressroute-routing.md) .

## <a name="cisco-ios-xe-based-routers"></a>Cisco IOS XE temelji usmjerivača

Uzoraka u ovom odjeljku odnose se za sve usmjerivač radi s operacijskim Sustavom IOS XE obitelj.

### <a name="1-configuring-interfaces-and-sub-interfaces"></a>1. konfiguriranje podređenu sučelja i sučelja

Potrebno je sučelje sub po peering u svakoj usmjerivač povezati Microsoft. Sub sučelja mogu se povezuje s VLAN ID-a ili složeni para VLAN ID-ovi i IP adrese.

#### <a name="dot1q-interface-definition"></a>Definicija Dot1Q sučelja

Ovaj primjer nudi definiciju podređenu sučelje za podređenu sučelja s jednom VLAN ID-a za VLAN ID je jedinstveni po peering. Zadnji octet od IPv4 adresa uvijek će iznositi neparan broj.

    interface GigabitEthernet<Interface_Number>.<Number>
     encapsulation dot1Q <VLAN_ID>
     ip address <IPv4_Address><Subnet_Mask>

#### <a name="qinq-interface-definition"></a>Definicija QinQ sučelja

Ovaj primjer nudi definiciju podređenu sučelje za podređenu sučelja s dva VLAN ID-a. Vanjski VLAN ID (s oznakom), ako se koristi ostaje preko svih peerings. Jedinstven po peering je Identifikator unutarnji VLAN (c oznaka). Zadnji octet od IPv4 adresa uvijek će iznositi neparan broj.

    interface GigabitEthernet<Interface_Number>.<Number>
     encapsulation dot1Q <s-tag> seconddot1Q <c-tag>
     ip address <IPv4_Address><Subnet_Mask>
    
### <a name="2-setting-up-ebgp-sessions"></a>2. postavljanje eBGP sesije

Morate postaviti BGP sesije s Microsoftom za svaku peering. Ogledna ispod omogućuje postavljanje BGP sesije s Microsoftom. Ako je IPv4 adresa koju ste koristili za vaše sučelja sub a.b.c.d, IP adresu susjednog BGP (Microsoft) bit će a.b.c.d+1. Zadnji octet BGP susjednog IPv4 adrese uvijek će biti paran broj.

    router bgp <Customer_ASN>
     bgp log-neighbor-changes
     neighbor <IP#2_used_by_Azure> remote-as 12076
     !        
     address-family ipv4
     neighbor <IP#2_used_by_Azure> activate
     exit-address-family
    !

### <a name="3-setting-up-prefixes-to-be-advertised-over-the-bgp-session"></a>3. podešavanje prefiksi da bi se objavljeno putem sesiju BGP

Možete konfigurirati usmjerivač za oglašavanje odaberite prefiksi Microsoftu. To možete učiniti pomoću uzorak u nastavku.

    router bgp <Customer_ASN>
     bgp log-neighbor-changes
     neighbor <IP#2_used_by_Azure> remote-as 12076
     !        
     address-family ipv4
      network <Prefix_to_be_advertised> mask <Subnet_mask>
      neighbor <IP#2_used_by_Azure> activate
     exit-address-family
    !

### <a name="4-route-maps"></a>4. karte usmjeravanje

Možete koristiti usmjeravanje karte i prefiks popisa za filtriranje prefiksi preneseni u vašoj mreži. Uzorak u nastavku možete koristiti za obavljanje zadatka. Provjerite je li postavljanje odgovarajuće prefiks popise.

    router bgp <Customer_ASN>
     bgp log-neighbor-changes
     neighbor <IP#2_used_by_Azure> remote-as 12076
     !        
     address-family ipv4
      network <Prefix_to_be_advertised> mask <Subnet_mask>
      neighbor <IP#2_used_by_Azure> activate
      neighbor <IP#2_used_by_Azure> route-map <MS_Prefixes_Inbound> in
     exit-address-family
    !
    route-map <MS_Prefixes_Inbound> permit 10
     match ip address prefix-list <MS_Prefixes>
    !


## <a name="juniper-mx-series-routers"></a>Usmjerivača tvrtke Juniper MX niza 

Primjere u ovom odjeljku odnose za sve usmjerivača niz tvrtke Juniper MX.

### <a name="1-configuring-interfaces-and-sub-interfaces"></a>1. konfiguriranje podređenu sučelja i sučelja

#### <a name="dot1q-interface-definition"></a>Definicija Dot1Q sučelja

Ovaj primjer nudi definiciju podređenu sučelje za podređenu sučelja s jednom VLAN ID-a za VLAN ID je jedinstveni po peering. Zadnji octet od IPv4 adresa uvijek će iznositi neparan broj.

    interfaces {
        vlan-tagging;
        <Interface_Number> {
            unit <Number> {
                vlan-id <VLAN_ID>;
                family inet {
                    address <IPv4_Address/Subnet_Mask>;
                }
            }
        }
    }


#### <a name="qinq-interface-definition"></a>Definicija QinQ sučelja

Ovaj primjer nudi definiciju podređenu sučelje za podređenu sučelja s dva VLAN ID-a. Vanjski VLAN ID (s oznakom), ako se koristi ostaje preko svih peerings. Jedinstven po peering je Identifikator unutarnji VLAN (c oznaka). Zadnji octet od IPv4 adresa uvijek će iznositi neparan broj.

    interfaces {
        <Interface_Number> {
            flexible-vlan-tagging;
            unit <Number> {
                vlan-tags outer <S-tag> inner <C-tag>;
                family inet {
                    address <IPv4_Address/Subnet_Mask>;
                }                           
            }                               
        }                                   
    }                           

### <a name="2-setting-up-ebgp-sessions"></a>2. postavljanje eBGP sesije

Morate postaviti BGP sesije s Microsoftom za svaku peering. Ogledna ispod omogućuje postavljanje BGP sesije s Microsoftom. Ako je IPv4 adresa koju ste koristili za vaše sučelja sub a.b.c.d, IP adresu susjednog BGP (Microsoft) bit će a.b.c.d+1. Zadnji octet BGP susjednog IPv4 adrese uvijek će biti paran broj.

    routing-options {
        autonomous-system <Customer_ASN>;
    }
    }
    protocols {
        bgp { 
            group <Group_Name> { 
                peer-as 12076;              
                neighbor <IP#2_used_by_Azure>;
            }                               
        }                                   
    }

### <a name="3-setting-up-prefixes-to-be-advertised-over-the-bgp-session"></a>3. podešavanje prefiksi da bi se objavljeno putem sesiju BGP

Možete konfigurirati usmjerivač za oglašavanje odaberite prefiksi Microsoftu. To možete učiniti pomoću uzorak u nastavku.

    policy-options {
        policy-statement <Policy_Name> {
            term 1 {
                from protocol OSPF;
        route-filter <Prefix_to_be_advertised/Subnet_Mask> exact;
                then {
                    accept;
                }
            }
        }
    }
    protocols {
        bgp { 
            group <Group_Name> { 
                export <Policy_Name>
                peer-as 12076;              
                neighbor <IP#2_used_by_Azure>;
            }                               
        }                                   
    }


### <a name="4-route-maps"></a>4. karte usmjeravanje

Možete koristiti usmjeravanje karte i prefiks popisa za filtriranje prefiksi preneseni u vašoj mreži. Uzorak u nastavku možete koristiti za obavljanje zadatka. Provjerite je li postavljanje odgovarajuće prefiks popise.

    policy-options {
        prefix-list MS_Prefixes {
            <IP_Prefix_1/Subnet_Mask>;
            <IP_Prefix_2/Subnet_Mask>;
        }
        policy-statement <MS_Prefixes_Inbound> {
            term 1 {
                from {
        prefix-list MS_Prefixes;
                }
                then {
                    accept;
                }
            }
        }
    }
    protocols {
        bgp { 
            group <Group_Name> { 
                export <Policy_Name>
                import <MS_Prefixes_Inbound>
                peer-as 12076;              
                neighbor <IP#2_used_by_Azure>;
            }                               
        }                                   
    }

## <a name="next-steps"></a>Daljnji koraci

[Najčešća pitanja vezana uz ExpressRoute](expressroute-faqs.md) dodatne pojedinosti potražite u članku.
