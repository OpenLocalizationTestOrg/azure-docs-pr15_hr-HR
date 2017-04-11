<properties
    pageTitle="Omogućivanje udaljene pogrešaka s neprekinutim isporuke | Microsoft Azure"
    description="Saznajte kako omogućiti daljinsko uklanjanje programskih pogrešaka prilikom korištenja neprekinuti isporuke za implementaciju Azure"
    services="cloud-services"
    documentationCenter=".net"
    authors="TomArcher"
    manager="douge"
    editor=""/>

<tags
    ms.service="cloud-services"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-multiple"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="08/15/2016"
    ms.author="tarcher"/>

# <a name="enable-remote-debugging-when-using-continuous-delivery-to-publish-to-azure"></a>Omogućivanje udaljene ispravljanje pogrešaka prilikom korištenja neprekinuti isporuke da biste objavili Azure

Možete omogućiti udaljene ispravljanje pogrešaka u Azure, za servise u oblaku ili virtualnim strojevima kada koristite [Neprekinuti isporuke](cloud-services-dotnet-continuous-delivery.md) objavljivanje Azure slijedeći ove korake.

## <a name="enabling-remote-debugging-for-cloud-services"></a>Omogućivanje udaljene pogrešaka za servise u oblaku

1. Na agent za sastavljanje postavite početni okruženje za Azure kao što je vidljivo [Sastavljanje naredbenog retka za Azure](http://msdn.microsoft.com/library/hh535755.aspx).
2. Jer je runtime udaljene ispravljanje pogrešaka (msvsmon.exe) potrebna paketa, instalirajte [Udaljene alate za Visual Studio 2015](http://www.microsoft.com/en-us/download/details.aspx?id=48155) (ili [Udaljenom alate za Microsoft Visual Studio 2013 ažuriranje 5](https://www.microsoft.com/en-us/download/details.aspx?id=48156) ako koristite Visual Studio 2013). Umjesto toga, možete kopirati binarne datoteke udaljene ispravljanje pogrešaka iz sustav koji ima ugrađen Visual Studio.
3. Stvaranje certifikata, kao što je vidljivo [Pregled certifikata za servise u Oblaku Azure](cloud-services-certs-create.md). Zadrži .pfx i otisak prsta na certifikatu RDP i prenesite certifikata na ciljnom servis u oblaku.
4. Pomoću sljedećih mogućnosti u naredbenom retku MSBuild omogućuje stvaranje i paket s udaljenog ispravljanje omogućena. (Zamjenski stvarni putovi datotekama sustava i project kut zagradama stavki.)

        msbuild /TARGET:PUBLISH /PROPERTY:Configuration=Debug;EnableRemoteDebugger=true;VSX64RemoteDebuggerPath="<remote tools path>";RemoteDebuggerConnectorCertificateThumbprint="<thumbprint of the certificate added to the cloud service>";RemoteDebuggerConnectorVersion="2.7" "<path to your VS solution file>"

    `VSX64RemoteDebuggerPath`je put do mape koja sadrži msvsmon.exe u alatima za udaljene za Visual Studio.
    `RemoteDebuggerConnectorVersion`je verzija Azure SDK u servis u oblaku. Moraju podudarati i verzija instalirana pomoću Visual Studio.

5. Objavljivanje na servis u oblaku ciljne pomoću spakirati i .cscfg datoteke koje generira u prethodnom koraku.
6. Uvoz certifikata (.pfx datoteka) u stroju koji ima Visual Studio sa Azure SDK za .NET instaliran. Provjerite je li za uvoz u `CurrentUser\My` spremišta certifikata, u suprotnom priložite ispravljanje pogrešaka u Visual Studio neće uspjeti.

## <a name="enabling-remote-debugging-for-virtual-machines"></a>Omogućivanje udaljene pogrešaka virtualnim strojevima

1. Stvaranje Azure virtualnog računala. Potražite u članku [Stvaranje virtualnog računala sa sustavom Windows Server](../virtual-machines/virtual-machines-windows-hero-tutorial.md) ili [Stvaranje i upravljanje Azure virtualnim strojevima u Visual Studio](../virtual-machines/virtual-machines-windows-classic-manage-visual-studio.md).
2. [Azure stranici portala klasični](http://go.microsoft.com/fwlink/p/?LinkID=269851)prikaz nadzorne ploče virtualnog računala da biste vidjeli virtualnog računala **Otisak PRSTA na CERTIFIKATU RDP**. Ta se vrijednost koristi za na `ServerThumbprint` vrijednost u konfiguraciji nastavak.
3. Stvaranje certifikata za klijenta, kao što je vidljivo [Pregled certifikata za servise u Oblaku Azure](cloud-services-certs-create.md) (zadržava .pfx i otisak prsta na certifikatu RDP).
4. Instaliranje komponente Powershell Azure (verzija 0.7.4 ili noviji) kao što je vidljivo [kako instalirati i konfigurirati Azure PowerShell](../powershell-install-configure.md).
5. Pokrenite sljedeću skriptu da biste omogućili RemoteDebug nastavak. Zamijenite putovima i osobne podatke vlastitih, kao što su naziv pretplate, servis za ime i otisak prsta.

    >[AZURE.NOTE] Ova skripta je konfiguriran za Visual Studio 2015. Ako koristite Visual Studio 2013, izmijeniti u `$referenceName` i `$extensionName` dodjele dolje da biste koristili `RemoteDebugVS2013` (umjesto `RemoteDebugVS2015`).

    <pre>
   Dodavanje AzureAccount

    Odaberite AzureSubscription "Pretplate Microsoft"

    $vm = get-AzureVM - naziv servisa "mytestvm1"-naziv "mytestvm1"

    $endpoints = @( ,@{Name="RDConnVS2013"; PublicPort = 30400; PrivatePort = 30398} ,@{Name="RDFwdrVS2013"; PublicPort = 31400; PrivatePort = 31398})  

    foreach ($endpoint u $endpoints) {Dodaj AzureEndpoint - VM $vm-naziv $endpoint. Ime - protokolom tcp - PublicPort $endpoint. PublicPort - LocalPort $endpoint. PrivatePort}

    $referenceName = "Microsoft.VisualStudio.WindowsAzure.RemoteDebug.RemoteDebugVS2015" $publisher = "Microsoft.VisualStudio.WindowsAzure.RemoteDebug" $extensionName = "RemoteDebugVS2015" $version = "1.*" $publicConfiguration = "<PublicConfig>< Connector.Enabled > true < /Connector.Enabled ><ClientThumbprint>56D7D1B25B472268E332F7FC0C87286458BFB6B2</ClientThumbprint><ServerThumbprint>E7DCB00CB916C468CC3228261D6E4EE45C8ED3C6</ServerThumbprint><ConnectorPort>30398</ConnectorPort><ForwarderPort>31398</ForwarderPort></PublicConfig>"

    $vm | Postavljanje AzureVMExtension `
    -ReferenceName $referenceName ` 
    -Publisher $publisher `
    -ExtensionName $extensionName ` 
    -verzije $version "- PublicConfiguration $publicConfiguration

    foreach ($extension u $vm. VM. ResourceExtensionReferences) {ako (($extension. ReferenceName - eq $referenceName) `
    -and ($extension.Publisher -eq $publisher) ` 
    - a ($extension. Ime - eq $extensionName) '- a ($extension. Verzija - eq $version)) {$extension. ResourceExtensionParameterValues [0]. Ključ = 'config.txt' prijelom}}

    $vm | Ažuriranje AzureVM </pre>

6. Uvoz certifikata (.pfx) stroju koji ima Visual Studio sa Azure SDK za .NET instaliran.
