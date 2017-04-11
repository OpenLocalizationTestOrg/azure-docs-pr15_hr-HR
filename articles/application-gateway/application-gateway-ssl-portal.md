<properties
   pageTitle="Konfigurirati pristupnik za aplikaciju za rasterećivanje SSL pomoću portala za | Microsoft Azure"
   description="Ova stranica sadrži upute za stvaranje pristupnika za aplikaciju SSL offload pomoću portala"
   documentationCenter="na"
   services="application-gateway"
   authors="georgewallace"
   manager="carmonm"
   editor="tysonn"/>
<tags
   ms.service="application-gateway"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="09/09/2016"
   ms.author="gwallace"/>

# <a name="configure-an-application-gateway-for-ssl-offload-by-using-the-portal"></a>Konfiguriranje pristupnik za aplikaciju za SSL rasterećivanje pomoću portala

> [AZURE.SELECTOR]
-[Portal za Azure](application-gateway-ssl-portal.md)
-[Azure resursima PowerShell](application-gateway-ssl-arm.md)
-[Azure klasični PowerShell](application-gateway-ssl.md)

Da biste prekinuli sesiju Secure Sockets Layer (SSL) pristupnika da biste izbjegli skup zadaci dešifriranje SSL da se dogodi u farmi web moguće je konfigurirati pristupnik Azure aplikacije. SSL rasterećivanje pojednostavljuje i postavljanje poslužitelj sučelja i upravljanja web-aplikacije.

## <a name="scenario"></a>Scenarij

Sljedeći scenarij prolazi kroz konfiguriranje SSL offload na postojeće pristupnik za aplikacije. Scenarij pretpostavlja da ste već pratili korake da biste [stvorili pristupnik za aplikacije](application-gateway-create-gateway-portal.md).

## <a name="before-you-begin"></a>Prije početka

Da biste konfigurirali SSL rasterećivanje pristupnik za aplikaciju, potreban je certifikat. Taj certifikat se učitati na pristupnik za aplikaciju, a služe za šifriranje i dešifriranje prometa koji se šalju putem SSL. Certifikat mora biti u obliku Razmjena osobnih podataka (pfx). U ovom obliku datoteke omogućuje izvesti privatni ključ koji je potreban pristupnik za aplikaciju za izvođenje šifriranje i dešifriranje promet.

## <a name="add-an-https-listener"></a>Dodavanje programa HTTPS ga slušatelj

HTTPS ga slušatelj traži promet na temelju njegove konfiguracije i pomaže usmjerili promet na pozadinskog grupe.

### <a name="step-1"></a>Korak 1

Otvorite centar za Azure, a zatim odaberite postojeći pristupnik za aplikaciju

### <a name="step-2"></a>Korak 2

Kliknite slušače, a zatim kliknite gumb Dodaj da biste dodali na ga slušatelj.

![Pregled plohu pristupnik za aplikaciju][1]

### <a name="step-3"></a>Korak 3

Unesite potrebne podatke za ga slušatelj i prijenos certifikat .pfx po završetku kliknite u redu.

**Ime** - Ova vrijednost je neslužbeni naziv ga slušatelj.

**Konfiguraciju sučelju IP** - tu vrijednost je IP konfiguracija sučelju koji se koristi da ga slušatelj.

**Priključak sučelju (ime/priključak)** – neslužbeni naziv za priključak koji se koristi na sučelju aplikacije pristupnika i stvarne priključak koji se koristi.

**Protokol** - skretnice da biste odredili ako https ili http koristi se za sučelja.

Koristi **certifikat (ime i lozinka)** – rasterećivanje ako SSL, .pfx certifikat je potrebna za tu postavku, a potrebni su neslužbeni naziv i lozinku.

![Dodajte ga slušatelj plohu][2]

## <a name="create-a-rule-and-associate-it-to-the-listener"></a>Stvaranje pravila i povezati ga slušatelj

Ga slušatelj sada stvorena. Vrijeme je za stvaranje pravila za rukovanje promet s ga slušatelj.

### <a name="step-1"></a>Korak 1

Kliknite **pravila** pristupnika aplikacije, a zatim Dodaj.

![aplikacija pristupnika pravila plohu][3]

### <a name="step-2"></a>Korak 2

Na plohu **Dodaj pravilo osnovni** Upišite neslužbeni naziv pravila i odaberite ga slušatelj stvorili u prethodnom koraku. Odaberite odgovarajući pozadinskog skup i postavke http, a zatim kliknite **u redu**

![prozor https postavke][4]

Postavke sada se spremaju u pristupnik za aplikacije. Spremanje procesa za te postavke može potrajati neko vrijeme prije nego što su dostupni za prikaz putem portala sustava ili putem komponente PowerShell. Nakon spremanja pristupnika aplikacije rukuje šifriranje i dešifriranje promet. Sav promet između pristupnik za računala i web-poslužiteljima pozadinskog će se rukovati putem HTTP-a. Sve komunikacije natrag na klijentu ako je pokrenut putem HTTP vratit će se klijent šifrirane.

## <a name="next-steps"></a>Daljnji koraci

Da biste saznali kako konfigurirati prilagođene stanja probni s pristupnik Azure aplikacije, potražite u članku [Stvaranje prilagođenih stanja probni](application-gateway-create-gateway-portal.md).

[1]: ./media/application-gateway-ssl-portal/figure1.png
[2]: ./media/application-gateway-ssl-portal/figure2.png
[3]: ./media/application-gateway-ssl-portal/figure3.png
[4]: ./media/application-gateway-ssl-portal/figure4.png