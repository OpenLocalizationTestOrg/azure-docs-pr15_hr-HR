<properties
   pageTitle="Logika aplikacije lokalne podatkovne veze pristupnika | Microsoft Azure"
   description="Informacije o tome kako povezivanje pristupnika lokalnim podacima iz logike aplikacije."
   services="logic-apps"
   documentationCenter=".net,nodejs,java"
   authors="jeffhollan"
   manager="erikre"
   editor=""/>

<tags
   ms.service="logic-apps"
   ms.devlang="multiple"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="integration"
   ms.date="07/05/2016"
   ms.author="jehollan"/>

# <a name="connect-to-the-on-premises-data-gateway-for-logic-apps"></a>Povezivanje pristupnika lokalnim podacima za aplikacije logike

Podržani logike aplikacije poveznika omogućuju konfigurirati vezu lokalnih podataka programa access putem pristupnik lokalnim podacima.  Sljedeći koraci će vas voditi kroz kako instalirati i konfigurirati pristupnik za lokalnim podacima da biste radili s logike aplikacije.

## <a name="prerequisites"></a>Preduvjeti

* Mora biti pomoću tvrtke ili obrazovne ustanove adresu e-pošte u Azure povezivati pristupnika lokalnih podataka s računa (Azure Active Directory temelji račun)
    * Ako koristite Microsoftov Account (npr. @outlook.com, @live.com) račun za Azure možete koristiti da biste stvorili tvrtke ili obrazovne ustanove adresu e-pošte tako da [slijedite korake u nastavku](../virtual-machines/virtual-machines-windows-create-aad-work-id.md#locate-your-default-directory-in-the-azure-classic-portal)

> [AZURE.WARNING] Nema ograničenja trenutno koji lokalne instalacije pristupnika samo dovršiti kada se pomoću računa koji je registriran s dodatkom Power BI.  U aplikacijom registrirajte svakim računom s "Power BI besplatne" da biste dovršili instalaciju uspješno.

* Morate imati lokalnih podataka pristupnika [na lokalnom računalu instaliran](app-service-logic-gateway-install.md).
* Pristupnik morate ne ste je zatražio drugi Azure lokalnih podataka pristupnika ([zahtjeva se događa s stvaranja koraka 2](#2-create-an-azure-on-premises-data-gateway-resource)) – instalacija možete se povezati samo s jednog pristupnika resursa.

## <a name="installing-and-configuring-the-connection"></a>Instaliranje i konfiguriranje veze

### <a name="1-install-the-on-premises-data-gateway"></a>1. instalirati pristupnik za lokalnim podacima

[U ovom članku](app-service-logic-gateway-install.md)pronaći ćete informacije o instaliranju pristupnik lokalnim podacima.  Pristupnik mora biti instaliran na računalu na lokalni prije nastavka preostale korake.

### <a name="2-create-an-azure-on-premises-data-gateway-resource"></a>2. stvaranje programa pristupnika resursa za Azure lokalnih podataka

Kad instalirate, pretplate Azure morate pridružiti pristupnik lokalnim podacima.

1. Prijavite se na Azure pomoću iste tvrtke ili obrazovne ustanove adresu e-pošte koja se koristila tijekom instalacije pristupnika
1. Kliknite gumb **Novi** resurs
1. Pretraživanje i odaberite **pristupnik lokalnim podacima**
1. Unesite podatke koje želite pridružiti pristupnika računa – uključujući odabir odgovarajući **Naziv za instalaciju**

    ![Lokalne podatkovne veze baze pristupnika][1]
1. Kliknite gumb **Stvori** da biste stvorili resurs

### <a name="3-create-a-logic-app-connection-in-the-designer"></a>3. povezivanje logike aplikacije u alatu za dizajniranje

Sad kad pretplate Azure pridružena instance pristupnika lokalnih podataka, možete stvoriti vezu na njega s logike aplikacije.

1. Otvorite logike aplikaciju i odaberite poveznik koji podržava povezivanje s lokalnim (na ovom pisanje SQL Server)
1. Potvrdite okvir za **Povezivanje putem pristupnika lokalnim podacima**

    ![Logika aplikacije dizajner pristupnika stvaranja][2]
1. Odaberite **pristupnik** za povezivanje i ispunite bilo koje druge podatke o vezi potrebna
1. Kliknite **Stvori** da biste stvorili vezu

Veza treba sada uspješno konfiguriran za korištenje u aplikaciji logike.  

## <a name="next-steps"></a>Daljnji koraci
- [Uobičajene primjere i scenariji za logike aplikacije](app-service-logic-examples-and-scenarios.md)
- [Značajki integracije Enterprise](app-service-logic-enterprise-integration-overview.md)

<!-- Image references -->
[1]: ./media/app-service-logic-gateway-connection/createblade.PNG
[2]: ./media/app-service-logic-gateway-connection/blankconnection.PNG
[3]: ./media/app-service-logic-gateway-connection/checkbox.PNG