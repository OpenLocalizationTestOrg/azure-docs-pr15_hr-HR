<properties
    pageTitle="Pregled portala za Microsoft Azure"
    description="Saznajte kako koristiti portal Microsoft Azure."
    services=""
    documentationCenter=""
    authors="davidwrede"
    manager="dwrede"
    editor="jimbe"/>

<tags
    ms.service="na"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="hero-article"
    ms.date="12/16/2015"
    ms.author="dwrede"/>

# <a name="microsoft-azure-portal-overview"></a>Pregled portala za Microsoft Azure

Portal sustava Microsoft Azure je središnje mjesto gdje možete Dodjela resursa i upravljanje Azure resurse.  Pomoću ovog praktičnog vodiča će se Upoznajte ste s portala i pokazuju kako pomoću neke od tih mogućnosti ključa:
- Na **potpun trgovine** koji omogućuje pregledavanje tisuće stavki tvrtke Microsoft i druge dobavljače koji možete kupiti ili resursi.
- Na **sučelje za pregled objedinjenih i skalabilni** koji olakšava resurse zanimaju i izvršiti razne operacije upravljanja.
- **Dosljedno upravljanje stranicama** (ili blades) koja omogućuju upravljanje Azure na razna servisa putem dosljedno će se postavke, Akcije, naplate informacija, nadzor i korištenje podataka o stanju i približno više.
- **Sučelje za osobne** koji omogućuje stvaranje prilagođenih početnog zaslona koji prikazuje informacije koje želite vidjeti kad god se prijavite u.  Možete i prilagoditi bilo koji od blades upravljanja koje sadrže pločice.

 ![Usmjerenje Azure portala korisničkog Sučelja][UIOrientation]

## <a name="before-you-get-started"></a>Prije nego što počnete

Trebat će vam valjani Azure pretplate prođite kroz ovog praktičnog vodiča.  Ako ga nemate, zatim [Prijava za besplatnu probnu verziju](https://azure.microsoft.com/pricing/free-trial/) danas.  Kada imate pretplatu, možete pristupiti portal na [https://portal.azure.com].

## <a name="how-to-create-a-resource"></a>Kako stvoriti resursa

Azure ima trgovine bez stavki koje možete stvoriti na jednom mjestu.  Recimo da želite stvoriti novi VM Windows Server 2012.  Na + novi koncentrator je ulaza u curated skup istaknutog kategorija iz trgovine.  Svaku kategoriju mora malom istaknute stavke uz vezu cijelog trgovine koji prikazuje sve kategorije i pretraživanje. Da biste stvorili taj novi VM 2012 Windows Server, izvoditi sljedeće radnje:  

1.  Windows Server 2012 je istaknuto tako da odaberete iz kategorije računalnim.  
2.  Unesite neki osnovni unosa na obrascu.
3.  Kliknite 'Stvori', a vaše VM će početi Dodjela odmah.

Koncentrator obavijesti će vas upozoriti kad je stvorila vaša resursa i upravljanje plohu će se otvoriti (možete uvijek pregledavati resursima kasnije).

![Kategorije portala][PortalCategories]


## <a name="how-to-find-your-resources"></a>Kako pronaći resursi

Uvijek možete prikvačiti često pristupa resursima za vaše startboard, ali bilo bi dobro da biste pronašli nešto što ne često pristupate.  Koncentrator Pregledaj prikazano u nastavku je na način na sve resurse.  Možete filtrirati prema pretplate, odaberite/Promjena veličine stupaca i otvorite Upravljanje blades tako da kliknete na pojedinačne stavke.

![Pregled koncentratora][BrowseHub]

## <a name="how-to-manage-and-delegate-access-to-a-resource"></a>Kako upravljati i prava pristupa resursu

Na ovom plohu možete povezati s virtualnog računala putem udaljene radne površine, praćenje metriku ključnog učinka, kontrolu pristupa ovom VM pomoću uloga temelji access (RBAC), konfigurirati na VM i izvedite druge zadatke upravljanja važne.  Prenošenjem pristupa na temelju uloga je važnosti da upravljanje na razini.  Kliknite [ovdje](./active-directory/role-based-access-control-configure.md) da biste saznali više o tome. Dodijeliti pristup resursu izvoditi sljedeće radnje:

1.  Pronađite svoje resursa.
2.  Kliknite 'Sve postavke' u odjeljku Osnove.
3.  Na popisu postavke kliknite "Korisnika".
4.  Na naredbenoj traci kliknite "Dodaj".
5.  Odaberite korisnika i uloga.

![Upravljanje resursa][ManageResource]

## <a name="how-to-customize-a-resource-blade"></a>Kako prilagoditi resursa plohu

Azure preconfigures blades za resurse, ali vaš pločica na te blades su kontrole.  Jednostavno možete prijeći u prilagoditi način za dodavanje, uklanjanje, promjena veličine ili promijeniti raspored pločice. Da biste prilagodili na plohu, izvoditi sljedeće radnje:

1.  Pronađite svoje resursa.
2.  Kliknite na "..." pri vrhu na plohu koji želite prilagoditi.
3.  Kliknite "Dodaj dijelovi".
4.  Pokrenite povlačenje i ispuštanje dijelove.  

![Prilagodba Blades][CustomizeBlades]

## <a name="how-to-get-help"></a>Kako dobiti pomoć

Ako imate bilo kakvih problema, rado ćemo vam.  Na portalu ima pomoći i podrške za stranicu koja vas možete usmjeriti u pravom smjeru.  Ovisno o vašem [podršku planu](https://azure.microsoft.com/support/plans/), možete stvoriti i karticama za podršku izravno na portalu.  Kada stvorite zahtjev za podršku možete, možete upravljati životnim ciklusom ulaznice s portala sustava. Možete zatražiti pomoć i stranicu za podršku tako da odete na pregled -> pomoć + podrška.  

![Pomoć i podrška][HelpSupport]

## <a name="summary"></a>Sažetak

Pogledajmo ono što ste naučili ovog praktičnog vodiča:
- Naučili kako prijaviti, dobiti pretplatu i otvorite portal
- Koristite neku usmjeren pomoću portala za korisničko Sučelje i naučili kako stvoriti i pregled resursa
- Naučili kako stvoriti resursa i pregled resursa
- Naučili o blades strukture i upravljanje i kako možete dosljedno upravljanje različite vrste resursa
- Naučili kako prilagoditi portala za prikupljanje informacija koje vas zanimaju prednji plan i centra
- Naučili kako kontrolirati pristup resursima pomoću uloga temelji access (RBAC)
- Naučili kako zatražite pomoć i podršku

Portal sustava Microsoft Azure radically olakšava stvaranje i upravljanje aplikacija u oblaku.  Pogledajte na [blogu upravljanje](https://azure.microsoft.com/blog/topics/management/) Ostanite u tijeku kao Ispričavamo se neprestano [Slušanje povratne informacije](https://feedback.azure.com/forums/223579-azure-preview-portal/) i unese poboljšanja.  [Blog-ScottGu](http://weblogs.asp.net/scottgu) je drugi sjajno mjesto za sva ažuriranja za Azure.

[UIOrientation]: ./media/azure-portal-how-to-use/azure_portal_1.png
[PortalCategories]: ./media/azure-portal-how-to-use/azure_portal_2.png
[BrowseHub]: ./media/azure-portal-how-to-use/azure_portal_3.png
[ManageResource]: ./media/azure-portal-how-to-use/azure_portal_4.png
[CustomizeBlades]: ./media/azure-portal-how-to-use/azure_portal_5.png
[HelpSupport]: ./media/azure-portal-how-to-use/azure_portal_6.png
