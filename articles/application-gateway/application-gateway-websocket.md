<properties
   pageTitle="Podrška za aplikacije pristupnika WebSocket | Microsoft Azure"
   description="Ova stranica sadrži pregled podrške za aplikaciju pristupnika WebSocket."
   documentationCenter="na"
   services="application-gateway"
   authors="amsriva"
   manager="rossort"
   editor="amsriva"/>
<tags
   ms.service="application-gateway"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="09/16/2016"
   ms.author="amsriva"/>

# <a name="application-gateway-websocket-support"></a>Podrška za aplikacije WebSocket pristupnika

Pristupnik za aplikaciju nudi ugrađena podrška za WebSocket preko svih pristupnika veličina. Ne postoji korisnik može konfigurirati postavka selektivno Omogućivanje i onemogućivanje WebSocket podršku. Možete nastaviti koristiti standardni HTTPListener na priključak 80 i 443 prima WebSocket promet. Promet WebSocket zatim preusmjereni na WebSocket omogućeno pozadinskog poslužitelja pomoću odgovarajuće pozadinskog skup kao što je navedeno u aplikaciji pristupnika pravila. Protokol WebSocket standardizirani u [RFC6455](https://tools.ietf.org/html/rfc6455) priručniku omogućuje potpuni obostrano komunikaciju između klijenta i poslužitelja putem dugo izvodi TCP veze. Ova značajka omogućuje za interaktivniji komunikaciju između web-poslužitelj i klijent, što može biti Dvosmjeran bez potrebe za ankete kao obavezno u koji se temelji na HTTP implementacije.  WebSocket imati najniža indirektni za razliku od HTTP, a možete ponovno koristiti isti TCP veza za više Zatraži/odgovore rezultira učinkovitiji Upotreba resursa. WebSocket protokoli su osmišljeno putem klasične HTTP priključke 80 i 443.

Pristupnik probes aplikacije koje su opisane u odjeljku [Pregled stanja probni](application-gateway-probe-overview.md) morate odgovoriti pozadinskog poslužitelja. Aplikacija pristupnika stanja probes HTTP/HTTPS samo, to podrazumijeva svaki pozadinskog poslužitelja morate odgovoriti HTTP probes za pristupnik za računala da biste usmjerili promet WebSocket na poslužitelju.

## <a name="listener-configuration-element"></a>Element konfiguracije ga slušatelj

Postojeće HTTPListener može se koristiti za podršku WebSocket. Slijedi isječak HttpListeners element oglednu datoteku predloška. Koji su vam potrebni HTTP i HTTPS slušače podržava WebSocket i sigurnost WebSocket promet. Na sličan način možete koristiti [portal](application-gateway-create-gateway-portal.md) ili [PowerShell](application-gateway-create-gateway-arm.md) da biste stvorili pristupnik za aplikacije slušače na priključak 80 i 443 za podršku WebSocket promet.


    "httpListeners": [
                {
                    "name": "appGatewayHttpsListener",
                    "properties": {
                        "FrontendIPConfiguration": {
                            "Id": "/subscriptions/<subid>/resourceGroups/<rgName>/providers/Microsoft.Network/applicationGateways/applicationGateway1/frontendIPConfigurations/DefaultFrontendPublicIP"
                        },
                        "FrontendPort": {
                            "Id": "/subscriptions/<subid>/resourceGroups/<rgName>/providers/Microsoft.Network/applicationGateways/applicationGateway1/frontendPorts/appGatewayFrontendPort443'"
                        },
                        "Protocol": "Https",
                        "SslCertificate": {
                            "Id": "/subscriptions/<subid>/resourceGroups/<rgName>/providers/Microsoft.Network/applicationGateways/applicationGateway1/sslCertificates/appGatewaySslCert1'"
                        },
                    }
                },
                {
                    "name": "appGatewayHttpListener",
                    "properties": {
                        "FrontendIPConfiguration": {
                            "Id": "/subscriptions/<subid>/resourceGroups/<rgName>/providers/Microsoft.Network/applicationGateways/applicationGateway1/frontendIPConfigurations/appGatewayFrontendIP'"
                        },
                        "FrontendPort": {
                            "Id": "/subscriptions/<subid>/resourceGroups/<rgName>/providers/Microsoft.Network/applicationGateways/applicationGateway1/frontendPorts/appGatewayFrontendPort80'"
                        },
                        "Protocol": "Http",
                    }
                }
            ],

## <a name="backendaddresspool-backendhttpsetting-and-routing-rule-configuration"></a>BackendAddressPool, BackendHttpSetting i usmjeravanja konfiguracije pravila

BackendAddressPool treba koristiti da biste definirali pozadinskog skup s poslužiteljima WebSocket omogućena. Potrebno je definirati BackendHttpSetting s pozadinskom priključak 80 i 443 samo. Svojstva utemeljen na kolačića afinitet i requestTimeouts nisu bitni za WebSocket promet. Postoji nije potrebna u pravilo za usmjeravanje promjena. Pravila za usmjeravanje 'osnovni i dalje koristiti sve odgovarajuće ga slušatelj odgovarajuće skupna adresa pozadinskog. 

    "requestRoutingRules": [{
        "name": "<ruleName1>",
        "properties": {
            "RuleType": "Basic",
            "httpListener": {
                "id": "/subscriptions/<subid>/resourceGroups/<rgName>/providers/Microsoft.Network/applicationGateways/applicationGateway1/httpListeners/appGatewayHttpsListener')]"
            },
            "backendAddressPool": {
                "id": "/subscriptions/<subid>/resourceGroups/<rgName>/providers/Microsoft.Network/applicationGateways/applicationGateway1/backendAddressPools/ContosoServerPool')]"
            },
            "backendHttpSettings": {
                "id": "/subscriptions/<subid>/resourceGroups/<rgName>/providers/Microsoft.Network/applicationGateways/applicationGateway1/backendHttpSettingsCollection/appGatewayBackendHttpSettings')]"
            }
        }

    }, {
        "name": "<ruleName2>",
        "properties": {
            "RuleType": "Basic",
            "httpListener": {
                "id": "/subscriptions/<subid>/resourceGroups/<rgName>/providers/Microsoft.Network/applicationGateways/applicationGateway1/httpListeners/appGatewayHttpListener')]"
            },
            "backendAddressPool": {
                "id": "/subscriptions/<subid>/resourceGroups/<rgName>/providers/Microsoft.Network/applicationGateways/applicationGateway1/backendAddressPools/ContosoServerPool')]"
            },
            "backendHttpSettings": {
                "id": "/subscriptions/<subid>/resourceGroups/<rgName>/providers/Microsoft.Network/applicationGateways/applicationGateway1/backendHttpSettingsCollection/appGatewayBackendHttpSettings')]"
            }

        }
    }]

## <a name="websocket-enabled-backend"></a>Pozadinskog WebSocket omogućeno

Vaš pozadinskog morate imati web-poslužitelj HTTP/HTTPS sustavom u konfigurirano priključka (obično 80 i 443) za WebSocket rad. U ovom potrebna je jer WebSocket protokol zahtijeva početne rukovanja biti HTTP nadogradnju protokola WebSocket kao zaglavlje polja.

    GET /chat HTTP/1.1
    Host: server.example.com
    Upgrade: websocket
    Connection: Upgrade
    Sec-WebSocket-Key: dGhlIHNhbXBsZSBub25jZQ==
    Origin: http://example.com
    Sec-WebSocket-Protocol: chat, superchat
    Sec-WebSocket-Version: 13

Drugi Razlog je tu aplikaciju pristupnika pozadinskog stanja probni podržava samo HTTP/HTTPS protokola. Ako je pozadinskog poslužitelja ne reagira na HTTP/HTTPS probes, želite uzeti iz grupe pozadinska aplikacija i zahtjevi ne uključujući WebSocket zahtjeve, želite doći do ovaj pozadinskog.

## <a name="next-steps"></a>Daljnji koraci

Nakon učenje o podršci za WebSocket otvorite [Stvaranje pristupnika za aplikaciju](application-gateway-create-gateway.md) za početak rada s web-aplikacije WebSocket omogućena.
