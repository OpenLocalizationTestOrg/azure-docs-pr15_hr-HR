<properties 
   pageTitle="Konfiguriranje DNS-a između dvije Azure virtualne mreže | Microsoft Azure" 
   description="Saznajte kako konfigurirati VPN veze i razlučivanje naziva domene između dvije virtualne mreže i konfiguriranju zemlj replikacije HBase." 
   services="hdinsight,virtual-network" 
   documentationCenter="" 
   authors="mumian" 
   manager="jhubbard" 
   editor="cgronlun"/>

<tags
   ms.service="hdinsight"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data" 
   ms.date="06/28/2016"
   ms.author="jgao"/>

# <a name="configure-dns-between-two-azure-virtual-networks"></a>Konfiguriranje DNS-a između dvije Azure virtualne mreže

> [AZURE.SELECTOR]
- [Konfiguriranje povezivanja s VPN-a](../hdinsight-hbase-geo-replication-configure-vnets.md)
- [Konfiguriranje DNS-a](hdinsight-hbase-geo-replication-configure-dns.md)
- [Konfiguriranje HBase replikacije](hdinsight-hbase-geo-replication.md) 


Saznajte kako dodati i konfigurirati DNS poslužitelji Azure virtualne mrežama za rukovanje razlučivanje naziva unutar i putem virtualne mreža.

Pomoću ovog praktičnog vodiča je drugi dio [niza] [ hdinsight-hbase-geo-replication] o stvaranju zemlj replikacije HBase:

- [Konfiguriranje VPN veze između dvije virtualne mreže][hdinsight-hbase-geo-replication-vnet]
- Konfiguriranje DNS-a za virtualne mreže (ovog praktičnog vodiča)
- [Konfiguriranje replikacije HBase zemlj.][hdinsight-hbase-geo-replication]


Sljedeći dijagram prikazuje dvije virtualne mreže koju ste stvorili u članku [Konfiguriranje VPN veze između dvije virtualne mreže][hdinsight-hbase-geo-replication-vnet]:

![HDInsight HBase replikacije virtualne mrežni dijagram][img-vnet-diagram]

##<a name="prerequisites"></a>Preduvjeti
Prije početka ovog praktičnog vodiča, morate imati sljedeće:

- **Mogući Azure pretplate**. Pogledajte [Početak Azure besplatnu probnu verziju](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).

- **Radne stanice s Azure PowerShell**.

    Prije pokretanja skripte komponente PowerShell Provjerite jeste li povezani u pretplatu Azure pomoću sljedeći cmdlet:

        Add-AzureAccount

    Ako imate više pretplata Azure, koristite sljedeći cmdlet da biste postavili trenutne pretplate:

        Select-AzureSubscription <AzureSubscriptionName>
        
    [AZURE.INCLUDE [upgrade-powershell](../../includes/hdinsight-use-latest-powershell.md)]

- **Dva Azure virtualne mreže s povezivanjem VPN-a**.  Upute potražite u članku [Konfiguriranje VPN veza između dvije mreže Azure virtualne][hdinsight-hbase-geo-replication-vnet].

>[AZURE.NOTE] Servis za Azure nazive i nazive virtualnog računala mora biti jedinstvena. Naziv koji se koristi u ovom ćete praktičnom vodiču je Contoso-[naziv servisa VM Azure]-[EU-a / sad]. Na primjer, Contoso VNet EU je Azure virtualne mreže u podatkovnom centru Sjeverna Europa; Sad Contoso-DNS-a je DNS poslužitelj VM u centru za podatke na SAD-a. Morate se pojavljuju s vlastite nazive.
 
 
##<a name="create-azure-virtual-machines-to-be-used-as-dns-servers"></a>Stvaranje Azure virtualnim strojevima će se koristiti kao DNS poslužitelja

**Da biste stvorili virtualnog računala u Contoso-VNet-EU naziva EU Contoso-DNS-a**

1.  Kliknite **NOVO**, **IZRAČUNATI**, **VIRTUALNOG računala**, **Iz GALERIJE**.
2.  Odaberite **Windows Server 2012 R2 Standard**.
3.  Unesite:
    - **Naziv VIRTUALNOG računala**: EU Contoso-DNS-a
    - **NOVO KORISNIČKO ime**: 
    - **NOVU lozinku**: 
4.  Unesite:
    - **Servis u OBLAKU**: Stvaranje novog servis u oblaku
    - **PODRUČJE/GRUPE AFINITET/VIRTUALNE MREŽE**: (odaberite Contoso VNet EU)
    - **VIRTUALNA MREŽE PODMREŽE**: podmreže-1
    - **RAČUN za POHRANU**: pomoću računa za automatski generirani prostora za pohranu
    
        Naziv usluge oblaka bit će jednak nazivu virtualnog računala. U ovom slučaju to je EU Contoso-DNS-a. Za sljedeće virtualnim strojevima li možete koristiti isti servis u oblaku.  Sve virtualnim strojevima u odjeljku isti servis u oblaku zajednički koristiti iste virtualne mreže i nastavak domene.

        Račun za pohranu koristi se za pohranu slikovne datoteke virtualnog računala. 
    - **Krajnje točke**: (pomaknite se prema dolje i odaberite **DNS-a**) 

Nakon stvaranja virtualnog računala Saznajte internog IP-a i vanjske IP- a.

1.  Kliknite naziv virtualnog računala **EU Contoso-DNS-a**.
2.  Kliknite **nadzorna ploča**.
3.  Zapišite:
    - JAVNI VIRTUALNE IP ADRESA
    - INTERNA IP ADRESA


**Da biste stvorili virtualnog računala unutar tvrtke Contoso-VNet-hr, pod nazivom Contoso-DNS-hr** 

- Ponovite isti postupak da biste stvorili virtualnog računala pomoću sljedeće vrijednosti:
    - Naziv VIRTUALNOG računala: Contoso sad-DNS-a
    - PODRUČJE/GRUPE AFINITET/VIRTUALNE MREŽE: Odaberite Contoso-VNET-hr
    - PODMREŽE VIRTUALNE MREŽE: Podmreže-1
    - RAČUN za POHRANU: Korištenje računa automatski generirani prostora za pohranu
    - Krajnje točke: (odaberite DNS-a)

##<a name="set-static-ip-addresses-for-the-two-virtual-machines"></a>Postavljanje statičke IP adrese za dva virtualnim strojevima

DNS poslužitelji zahtijeva statičke IP adrese.  Ovaj korak nije moguće izvršiti na portalu klasični Azure. Koristit ćete Azure PowerShell.

**Da biste konfigurirali statičke IP adrese za dva virtualnim strojevima**

1. Otvorite Windows Očisti filtar.
2. Pokrenite sljedeće Cmdlete.  

        Add-AzureAccount
        Select-AzureSubscription [YourAzureSubscriptionName]
        
        Get-AzureVM -ServiceName Contoso-DNS-EU -Name Contoso-DNS-EU | Set-AzureStaticVNetIP -IPAddress 10.1.0.4 | Update-AzureVM
        Get-AzureVM -ServiceName Contoso-DNS-US -Name Contoso-DNS-US | Set-AzureStaticVNetIP -IPAddress 10.2.0.4 | Update-AzureVM 

    Naziv servisa je naziv servisa oblaka. Budući da je DNS poslužitelj prvi virtualnog računala u oblaku, naziv usluge oblak je isti kao naziv virtualnog računala.

    Možda ćete morati ažurirati naziv servisa i naziv odgovaraju nazivima koje imate.


##<a name="add-the-dns-server-role-the-two-virtual-machines"></a>Dodavanje virtualnim strojevima ulogu dviju DNS poslužitelj

**Da biste dodali uloga DNS poslužitelja za EU Contoso-DNS-a**

1.  Na portalu klasični Azure kliknite **virtualnim strojevima** na lijevoj strani. 
2.  Kliknite **EU Contoso-DNS-a**.
3.  Kliknite **nadzorna PLOČA** s vrha.
4.  Kliknite **POVEŽI** s dna i slijedite upute za povezivanje s virtualnog računala putem RDP.
2.  U sesiji RDP pritisnite gumb Windows u donjem lijevom kutu da biste otvorili na početni zaslon.
3.  Kliknite pločicu **Upravitelj poslužitelja** .
4.  Kliknite **Dodaj uloge i značajke**.
5.  Kliknite **Dalje**
6.  Odaberite **ulogu ili značajku instalaciju**, a zatim kliknite **Dalje**.
7.  Odaberite vaš DNS virtualnog računala (ona će biti istaknuti već), a zatim kliknite **Dalje**.
8.  Provjerite **DNS poslužitelj**.
9.  Kliknite **Dodaj značajke**, a zatim kliknite **Nastavi**.
10. Kliknite **sljedeće** tri puta, a zatim kliknite **Instaliraj**. 

**Da biste dodali uloga DNS poslužitelja za SAD Contoso-DNS-a**

- Ponovite korake da biste dodali DNS uloga **Sad Contoso-DNS-a**.

##<a name="assign-dns-servers-to-the-virtual-networks"></a>DNS poslužitelji dodijeliti virtualne mreže

**Da biste registrirali dva DNS poslužitelja**

1.  Na portalu klasični Azure kliknite **NOVO**, **MREŽNIM servisima**, **VIRTUALNE MREŽE**, **REGISTRIRATI DNS POSLUŽITELJ**.
2.  Unesite:
    - **Naziv**: EU Contoso-DNS-a
    - **IP ADRESU DNS POSLUŽITELJA**: 10.1.0.4 – na IP adresa mora koji se podudaraju IP adresu DNS poslužitelja virtualnog računala.
     
3.  Ponovite postupak da biste registrirali sad Contoso-DNS-a s sljedeće postavke:
    - **Naziv**: SAD Contoso-DNS-a
    - **IP ADRESU DNS POSLUŽITELJA**: 10.2.0.4

**Da biste dodijelili dva DNS poslužitelji dvije virtualne mreže**

1.  Kliknite **mreža** s lijevom oknu na portalu klasični.
2.  Kliknite **Contoso VNet EU**.
3.  Kliknite **KONFIGURIRAJ**.
4.  U odjeljku **dns poslužitelji** , odaberite **EU Contoso-DNS-a** .
5.  Kliknite **SPREMI** na dno stranice, a zatim kliknite **da** da biste potvrdili.
6.  Ponovite postupak da biste dodijelili DNS poslužitelj **Tvrtke Contoso-DNS-hr** **Contoso-VNet-hr** virtualne mreže.

Sve virtualnim strojevima koji je uveden virtualne mreže morate ponovno pokrenuti da biste ažurirali konfiguracija DNS poslužitelja.

**Da biste ponovno virtualnim strojevima**

1. Na portalu klasični Azure kliknite **virtualnim strojevima** na lijevoj strani.
2. Kliknite **EU Contoso-DNS-a**.
3. Kliknite **nadzorna ploča** s vrha.
4. Kliknite **ponovno POKRENITE** na dnu.
5. Ponovite iste korake da biste ponovno **Sad Contoso-DNS-a**.


##<a name="configure-dns-conditional-forwarders"></a>Konfiguriranje DNS uvjetno prosljeđivanja

DNS poslužitelj na svakom virtualne mreže može riješiti samo DNS naziva u tom virtualne mreže. Morate konfigurirati uvjetno forwarder tako da pokazuje na DNS poslužitelj ravnopravnih članova za razrješavanje imena u virtualne mreže ravnopravnih članova.

Da biste konfigurirali uvjetno forwarder, morate znati nastavke domene dva DNS poslužitelji. Nastavke DNS mogu se razlikovati ovisno o konfiguraciji servise u Oblaku koji ste koristili kada ste stvorili virtualnim računalima sustava. Za svaki DNS sufiks koristi u virtualne mreže, morate dodati uvjetno forwarder. 

**Da biste pronašli nastavke domene dva DNS poslužitelja**

1. RDP u **EU Contoso-DNS-a**.
2. Otvorite konzole za Windows PowerShell ili naredbeni redak.
3. Pokrenite **ipconfig**i zapišite **sufiks DNS specifične za vezu**.
4. Zatvorite sesiju RDP, on će i dalje trebate. 
5. Ponovite iste korake da biste saznali **DNS specifične sufiks** **Sad Contoso-DNS-a**.


**Konfiguriranje DNS prosljeđivanja**
 
1.  RDP sesiju da bi **EU Contoso-DNS-a**, kliknite s logotipom sustava Windows u donjem lijevom.
2.  Kliknite **stavku Administrativni alati**.
3.  Kliknite **DNS**.
4.  U lijevom oknu proširite **DSN**, **EU Contoso-DNS-a**.
5.  Unesite sljedeće podatke:
    - **DNS domena**: Unesite DNS sufiks Contoso-DNS-hr. Na primjer: Contoso DNS US.b5.internal.cloudapp.net.
    - **IP adrese glavnog poslužitelja**: Unesite 10.2.0.4 koja je u Contoso-DNS-hr-IP adresa.
6.  Pritisnite **ENTER**, a zatim kliknite **u redu**.  Sada će biti možete riješiti na Contoso-DNS-hr-IP adrese iz EU Contoso-DNS-a.
7.  Ponovite korake da biste dodali DNS forwarder DNS servis virtualnog računala sad Contoso-DNS-a pomoću sljedeće vrijednosti:
    - **DNS domena**: Unesite DNS sufiks Contoso-DNS-EU-a. 
    - **IP adrese glavnog poslužitelja**: Unesite 10.2.0.4 koji je Contoso-DNS-EU-a, IP adresa.

##<a name="test-the-name-resolution-across-the-virtual-networks"></a>Testiranje razlučivanje naziva putem virtualne mreža

Sada možete testirati razlučivost naziv glavnog računala putem virtualne mreža. Ping vatrozid blokira prema zadanim postavkama.  Da biste riješili (morate koristiti FQDN) DNS poslužitelja virtualnim strojevima u mrežama ravnopravnih članova, poslužite se nslookup.  


##<a name="next-steps"></a>Daljnji koraci

U ovom ćete praktičnom vodiču ste naučili kako konfigurirati razlučivanje naziva putem virtualne mreža s VPN veza. Ostali dva članci u nizu pokrivaju:

- [Konfiguriranje VPN veze između dvije Azure virtualne mreže][hdinsight-hbase-geo-replication-vnet]
- [Konfiguriranje replikacije HBase zemlj.][hdinsight-hbase-geo-replication]



[hdinsight-hbase-geo-replication]: hdinsight-hbase-geo-replication.md
[hdinsight-hbase-geo-replication-vnet]: hdinsight-hbase-geo-replication-configure-vnets.md
[powershell-install]: powershell-install-configure.md

[img-vnet-diagram]: ./media/hdinsight-hbase-geo-replication-configure-DNS/HDInsight.HBase.VPN.diagram.png 
