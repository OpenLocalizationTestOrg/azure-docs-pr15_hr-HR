<properties
   pageTitle="Pregled BGP s pristupnika Azure VPN | Microsoft Azure"
   description="Ovaj članak sadrži pregled BGP s Azure VPN pristupnika."
   services="vpn-gateway"
   documentationCenter="na"
   authors="yushwang"
   manager="rossort"
   editor=""
   tags=""/>

<tags
   ms.service="vpn-gateway"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="06/16/2016"
   ms.author="yushwang"/>

# <a name="overview-of-bgp-with-azure-vpn-gateways"></a>Pregled BGP s pristupnika Azure VPN-a

Ovaj članak sadrži pregled podrške za BGP (obrub pristupnika Protocol) u Azure VPN pristupnika.

## <a name="about-bgp"></a>O BGP

BGP je standardni protokol usmjeravanja obično se koristi u Interneta razmjenu usmjeravanje i reachability informacija između dvije ili više mreža. Kada se koristi u kontekstu Azure virtualne mreže, BGP omogućuje VPN pristupnika Azure i uređaji lokalnog VPN, naziva BGP kolegama ili susjeda, razmjenu "usmjerava" koji će obavijestiti oba pristupnika na dostupnosti i reachability za te prefiksi prođite kroz pristupnika ili usmjerivača uključen. BGP možete omogućiti prijenosa usmjeravanje među više mreža tako da prijenos usmjerava pristupnik BGP Pratit će iz jednog BGP ravnopravnih članova na druge BGP kolegama.
 
### <a name="why-use-bgp"></a>Zašto koristiti BGP?

BGP dodatna je značajka možete koristiti s Azure utemeljen na usmjeravanje VPN pristupnika. I provjerite je li VPN uređajima lokalnog podržava BGP da biste omogućili značajku. Možete nastaviti koristiti Azure VPN pristupnika i uređajima VPN lokalnog bez BGP. To je jednako korištenja Statički smjerovi (bez BGP) *nasuprot* pomoću dinamičke usmjeravanje s BGP između mreže i Azure.

Postoji nekoliko prednosti i nove mogućnosti s BGP:

#### <a name="support-automatic-and-flexible-prefix-updates"></a>Podržava automatsko i prilagodljivo prefiks ažuriranja

S BGP, samo morate deklarirati minimalne prefiks za određene ravnopravnih članova BGP putem tunelom IPsec S2S VPN-a. Možda ćete što manje prefiks glavnog računala (/ 32) IP adrese ravnopravnih članova BGP uređaju lokalnog VPN-a. Možete odrediti koji lokalne mreže prefiksi želite Oglasite Azure dopustili Azure virtualne mreže da biste pristupili.
    
Možete Oglasite i veće prefiksi koji mogu sadržavati neke od VNet prefiksi adresu, kao što su velike privatne IP adresa razmak (npr., 10.0.0.0/8). Uzmite u obzir iako prefiksa ne mogu biti jednake s bilo koju od vašeg prefiksi VNet. Te usmjerava jednako vaše prefiksi VNet će biti odbijene.

>[AZURE.IMPORTANT] Trenutno oglašavanje zadani smjer (0.0.0.0/0) za Azure VPN pristupnika bit će blokirane. Kada je omogućen tu mogućnost, objavit ćemo zajedno daljnje ažuriranja.

#### <a name="support-multiple-tunnels-between-a-vnet-and-an-on-premises-site-with-automatic-failover-based-on-bgp"></a>Podržava više tunnels između programa VNet i lokalno mjesto s automatsko prebacivanje na temelju BGP

Možete uspostaviti više veza između vaše VNet Azure i uređajima lokalnog VPN-a na istom mjestu. Tu mogućnost sadrži više tunnels (putovi) između dvije mreže u konfiguraciji aktivno aktivno. Ako nešto na tunnels nije povezan, odgovarajući usmjerava će biti povući putem BGP i promet će se automatski pomiču preostale tunnels.
    
Sljedeći dijagram prikazuje jednostavnog primjera ovaj će instalacijski program iznimno dostupne:
    
![Više aktivni putova](./media/vpn-gateway-bgp-overview/multiple-active-tunnels.png)

#### <a name="support-transit-routing-between-your-on-premises-networks-and-multiple-azure-vnets"></a>Podržava prijenosa usmjeravanje između lokalnog mreža i više VNets Azure

BGP omogućuje više pristupnika da biste saznali i proširiti prefiksi s različitim mreža, hoće li se izravno ili posredno povezani. To možete omogućiti prijenosa usmjeravanje s Azure VPN pristupnika između web-mjesta na lokalni ili preko više mreža virtualne Azure.
    
Na sljedećem su dijagramu prikazani Primjeri višestrukih topologije s više putova možete transit promet između dvije lokalne mreže kroz Azure VPN pristupnika unutar Microsoft Networks:

![Višestrukih prijenosa](./media/vpn-gateway-bgp-overview/full-mesh-transit.png)

## <a name="bgp-faqs"></a>Najčešća pitanja BGP


[AZURE.INCLUDE [vpn-gateway-bgp-faq-include](../../includes/vpn-gateway-bpg-faq-include.md)] 




## <a name="next-steps"></a>Daljnji koraci

Potražite u članku [Uvod u rad s BGP na pristupnika Azure VPN-a](./vpn-gateway-bgp-resource-manager-ps.md) za korake da biste konfigurirali BGP za više lokacija i VNet VNet veze.

