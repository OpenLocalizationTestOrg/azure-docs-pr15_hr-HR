<properties
    pageTitle="Početak rada s vezama hibridnog preusmjeravanja | Microsoft Azure"
    description="Kako napisati program čvor konzole za hibridno veze"
    services="service-bus"
    documentationCenter="node"
    authors="jtaubensee"
    manager="timlt"
    editor=""/>

<tags
    ms.service="service-bus"
    ms.devlang="tbd"
    ms.topic="hero-article"
    ms.tgt_pltfrm="node"
    ms.workload="na"
    ms.date="10/28/2016"
    ms.author="jotaub"/>

# <a name="get-started-with-relay-hybrid-connections"></a>Početak rada s preusmjeravanja hibridnog veze

[AZURE.INCLUDE [relay-selector-hybrid-connections](../../includes/relay-selector-hybrid-connections.md)]

## <a name="what-will-be-accomplished"></a>Što će obaviti

Budući da se veza hibridnog zahtijeva klijenta i komponentu poslužitelja, će ćemo stvoriti dvije aplikacije konzole ovog praktičnog vodiča. Evo koraka:

1. Stvaranje preusmjeravanja prostor naziva, pomoću portala za Azure.

2. Stvorite vezu hibridnog, pomoću portala za Azure.

3. Pisanje poslužitelju aplikacije konzole primati poruke.

4. Pisanje klijent aplikacije konzole za slanje poruka.

## <a name="prerequisites"></a>Preduvjeti

1. [Node.js](https://nodejs.org/en/) (Ovaj primjer koristi čvor 7.0).

2. Azure pretplate.

[AZURE.INCLUDE [create-account-note](../../includes/create-account-note.md)]

## <a name="1-create-a-namespace-using-the-azure-portal"></a>1. stvaranje prostor naziva pomoću portala za Azure

Ako ste već stvorili preusmjeravanja prostor naziva, prijeći na odjeljak [Stvaranje hibridnog veze pomoću portala za Azure](#2-create-a-hybrid-connection-using-the-azure-portal) .

[AZURE.INCLUDE [relay-create-namespace-portal](../../includes/relay-create-namespace-portal.md)]

## <a name="2-create-a-hybrid-connection-using-the-azure-portal"></a>2. povezivanje hibridnog pomoću portala za Azure

Ako već imate vezu hibridnog stvorili, prijelaz u odjeljku [Stvaranje poslužiteljsku aplikaciju](#3-create-a-server-application-listener) .

[AZURE.INCLUDE [relay-create-hybrid-connection-portal](../../includes/relay-create-hybrid-connection-portal.md)]

## <a name="3-create-a-server-application-listener"></a>3. stvoriti aplikaciju server (ga slušatelj)

Da biste preslušali i primati poruke s prijenos, ne možemo će pisati aplikacije konzole za Node.js.

[AZURE.INCLUDE [relay-hybrid-connections-dotnet-get-started-server](../../includes/relay-hybrid-connections-node-get-started-server.md)]

## <a name="4-create-a-client-application-sender"></a>4. stvaranje klijentske aplikacije (pošiljatelj)

Da biste poslali poruke prijenos, ne možemo će pisati aplikacije konzole za Node.js.

[AZURE.INCLUDE [relay-hybrid-connections-dotnet-get-started-client](../../includes/relay-hybrid-connections-node-get-started-client.md)]

## <a name="5-run-the-applications"></a>5. pokretanje aplikacije

1. Pokrenite aplikaciju poslužitelja.

2. Pokrenite klijentska aplikacija, a zatim unesite tekst.

3. Provjerite je li aplikacije konzole za poslužitelj šalje je tekst koji je unesen u klijentsku aplikaciju.

![pokretanje aplikacije](./media/relay-hybrid-connections-node-get-started/running-applications.png)

Čestitamo, stvorite aplikaciju završetka do kraja hibridnog veze.

## <a name="next-steps"></a>Sljedeće korake:

- [Prijenos najčešća Pitanja](relay-faq.md)
- [Stvaranje prostor naziva](relay-create-namespace-portal.md)
- [Početak rada s .NET](relay-hybrid-connections-dotnet-get-started.md)
- [Početak rada s čvor](relay-hybrid-connections-node-get-started.md)