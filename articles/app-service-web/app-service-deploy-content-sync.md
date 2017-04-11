<properties
    pageTitle="Sinkronizacija sadržaja iz mape oblaka aplikacije servisa za Azure"
    description="Saznajte kako implementirati aplikaciju aplikacije servisa za Azure putem sinkronizacije sadržaja iz mape oblaka."
    services="app-service"
    documentationCenter=""
    authors="dariagrigoriu"
    manager="wpickett"
    editor="mollybos"/>

<tags
    ms.service="app-service"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="06/13/2016"
    ms.author="dariagrigoriu"/>
    
# <a name="sync-content-from-a-cloud-folder-to-azure-app-service"></a>Sinkronizacija sadržaja iz mape oblaka aplikacije servisa za Azure

Pomoću ovog praktičnog vodiča pokazuje kako implementirati [Aplikacije servisa za Azure](http://go.microsoft.com/fwlink/?LinkId=529714) po sinkronizacija sadržaja s popularne oblaka za pohranu servisa kao što su Dropbox i OneDrive. 

## <a name="overview"></a>Pregled sadržaja sinkronizaciju implementacije

Uvođenje sadržaja sinkronizaciju na zahtjev je pokreće [Kudu implementaciju modul](https://github.com/projectkudu/kudu/wiki) integriran s aplikacije servisa. [Portal za Azure](https://portal.azure.com)možete odrediti mape u oblak prostora za pohranu, rad s kodom aplikacije i sadržaj te mape i sinkronizirati aplikacije servisa za klikom na gumb. Sinkronizacija sadržaja koristi postupka Kudu za sastavljanje i implementacija. 
    
## <a name="contentsync"></a>Kako omogućiti uvođenja sadržaja sinkronizacije
Da biste omogućili sinkronizacija sadržaja s [Portala za Azure](https://portal.azure.com), slijedite ove korake:

1. Pokrenite aplikaciju plohu na portalu za Azure, kliknite **Postavke** > **Implementaciju izvora**. Kliknite **Odabir izvora**, a zatim odaberite **OneDrive** ili **Dropbox** kao izvor radi implementacije. 

    ![Sinkronizacija sadržaja](./media/app-service-deploy-content-sync/deployment_source.png)

    >[AZURE.NOTE] Zbog podlozi razlike u API-ji **OneDrive za tvrtke** nije podržan u ovo vrijeme. 

2. Dovršavanje tijeka rada za autorizaciju da biste omogućili aplikacije servisa za pristup određenom putu unaprijed definiranih zaduženog za OneDrive ili Dropbox cijeli sadržaj aplikacije servisa za će se pohranjuju.  
    Nakon autorizacije aplikacije servisa platforme steći ćete mogućnost za stvaranje sadržaja mape pod određenim sadržaja put ili da biste odabrali postojeću mapu sadržaja u odjeljku ovaj put određenu sadržaja. Određenu putova sadržaja u odjeljku računi za pohranu oblaka za aplikacije servisa za sinkronizaciju su sljedeće:  
    * **OneDrive**:`Apps\Azure Web Apps` 
    * **Zajedničke mrežne mape**:`Dropbox\Apps\Azure`

3. Nakon početnog sadržaja sinkronizacije sadržaja sinkronizaciju može se inicirati na zahtjev na portalu Azure. Uvođenje povijest dostupan je sa plohu **implementacije** .

    ![Povijest implementacije](./media/app-service-deploy-content-sync/onedrive_sync.png)
 
Dodatne informacije za implementaciju Dropbox dostupna je u odjeljku [uvođenja iz zajedničke mrežne mape](http://blogs.msdn.com/b/windowsazure/archive/2013/03/19/new-deploy-to-windows-azure-web-sites-from-dropbox.aspx). 


