<properties 
   pageTitle="Upravljanje resursima Azure pomoću programa Explorer oblaka | Microsoft Azure"
   description="Saznajte kako pomoću programa Explorer oblaka pregledavanje i upravljanje Azure resursima u Visual Studio."
   services="visual-studio-online"
   documentationCenter="na"
   authors="TomArcher"
   manager="douge"
   editor="" />
<tags 
   ms.service="multiple"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="multiple"
   ms.date="08/15/2016"
   ms.author="tarcher" />

# <a name="managing-azure-resources-with-cloud-explorer"></a>Upravljanje resursima Azure pomoću programa Explorer oblaka

##<a name="overview"></a>Pregled

Oblak Explorer namijenjen je omogućuju jednostavnije i brže pregledavati i upravljanje Azure resursa u Visual Studio IDE. Možete, na primjer, koristite da biste otvorili web-aplikacijama [Azure portal](http://go.microsoft.com/fwlink/p/?LinkID=525040) ili u pregledniku ili priložite, program za ispravljanje pogrešaka ili možete pogledati svojstva spremniku blob i otvorite je u Blob spremnik uređivaču.

Oblak Explorer je utemeljena na Upravitelj stogu Azure resursa, baš kao i [Azure portal](http://go.microsoft.com/fwlink/p/?LinkID=525040). Razumije resurse kao što su grupe Azure resursa i Azure servise kao što je aplikacija za logiku i API aplikacije, a to podržava [Kontrola pristupa na temelju uloga](./active-directory/role-based-access-control-configure.md) (RBAC). Da biste vidjeli Azure resursa koji ste dodali ili promijenili, odaberite gumb **Osvježi** na alatnoj traci Explorer oblaka.

Oblak Explorer se instalira u sklopu programa Visual Studio Tools za Azure SDK 2.7. 

## <a name="prerequisites"></a>Preduvjeti

- Visual Studio 2015 RTM.

- Visual Studio Tools for Azure SDK. 
- Morate imati račun za Azure i biti prijavljeni u nju Azure resursa u programu Explorer oblaka. Ako ga nemate, možete stvoriti račun u samo nekoliko minuta. Ako imate pretplatu na MSDN, potražite u članku [Azure pogodnost za pretplatnike na MSDN](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/). U suprotnom, potražite u članku [Stvaranje besplatnu probnu računa](https://azure.microsoft.com/pricing/free-trial/).

- Ako oblaka Explorer nije vidljivo, možete je pogledati tako da odaberete **Prikaz** **Drugih Windows** **Explorer oblak** na traci izbornika.

## <a name="manage-azure-accounts-and-subscriptions"></a>Upravljanje računima Azure i pretplate

Da biste vidjeli Azure resursa u programu Explorer oblaka, morate prijaviti na račun za Azure s jednog ili više aktivni pretplate. Ako imate više od jednog računa za Azure, možete ih dodati u programu Explorer oblaka, a zatim odaberite popis pretplata na koje želite uvrstiti u oblak Explorer prikaz resursa.

Ako još niste koristili Azure prije ili još niste dodali potrebne račune za Visual Studio, morat ćete učiniti.

## <a name="to-add-azure-accounts-to-cloud-explorer"></a>Da biste dodali Azure računa u programu Explorer oblaka

1. Odaberite ikonu postavke na alatnoj traci Explorer oblaka.

1. Odaberite vezu **Dodaj račun** . Prijavite se za u račun za Azure čije resursa koji želite pregledati. Na padajućem popisu odabir račun mora biti označena računa koji ste upravo dodali. Pretplate za taj račun prikazuju se u odjeljku stavka računa.

    ![Dodavanje Azure pretplate](./media/vs-azure-tools-resources-managing-with-cloud-explorer/IC819514.png)

    ![Odabir Azure pretplate](./media/vs-azure-tools-resources-managing-with-cloud-explorer/IC819515.png)

1. Odaberite potvrdne okvire za račun pretplate želite Pregledaj, a zatim odaberite gumb **Primijeni** .

    Azure resursi za odabrane pretplate se pojavljuju u programu Explorer oblaka.

## <a name="to-remove-an-azure-account"></a>Da biste uklonili račun za Azure

1. Odaberite **datoteku**, **Postavke računa** na traku izbornika.

1. U odjeljku **Svi računi** u dijaloškom okviru **Postavke računa** odaberite naredbu **Ukloni** uz račun za koji želite ukloniti. Napomena da ta naredba samo uklanja račun iz Visual Studio – it ne utječe na račun za Azure sam.

## <a name="view-resource-types-or-groups"></a>Prikaz vrste resursa ili grupe

Da biste pogledali Azure resursa, možete odabrati **Vrste resursa** ili prikaz **Grupa resursa** .

![Padajući izbornik prikaz resursa](./media/vs-azure-tools-resources-managing-with-cloud-explorer/IC819516.png)

- Prikaz **Vrste resursa** , što je i zajednička prikaz koji se koristi za [portal za Azure](http://go.microsoft.com/fwlink/p/?LinkID=525040), prikazuje Azure resurse kategorizirani po vrsti, kao što je web-aplikacije, račune za pohranu i virtualnih računala. To je slično kako Azure resursi pojavljuju se u programu Explorer poslužitelja.

- Prikaz grupa resursa kategorizira Azure resursi prema grupi Azure resursa se pridružena.

 
    Grupa resursa je paket Azure resursa, obično koristi određenu aplikaciju. Dodatne informacije o grupama Azure resursa, potražite u članku [Pregled upravljanja resursima Azure](./resource-group-overview.md).

## <a name="view-and-navigate-resources"></a>Prikaz, a zatim otvorite resursi

Dođite do Azure resursa i prikaz svoje podatke u programu Explorer oblaka, proširite vrsta ili grupu resursa za povezane stavke, a zatim odaberite resurs. Kad odaberete resursa, podaci prikazuju dvije kartice pri dnu Explorer oblaka.

![Odaberite prikaz resursa](./media/vs-azure-tools-resources-managing-with-cloud-explorer/IC819517.png)

- Karticu **Akcije** prikazuje akcije koje možete poduzeti u programu Explorer oblaka za odabrani resurs. Dostupne akcije možete vidjeti i na izborniku prečacu resursa.

- Kartica **Svojstva** prikazuje svojstva resursa, kao što su njegove vrsta, regionalne postavke i resursa grupe je pridružen.

Svaki resurs je **otvorena u portal**akciju. Kad odaberete ovu akciju, oblaka Explorer prikazuje odabranog resursa [Azure portal](http://go.microsoft.com/fwlink/p/?LinkID=525040). Značajka je posebno korisno za navigaciju Duboko ugniježđene resursi.

Dodatne akcije i vrijednosti nekretnina može se pojaviti i ovisno o Azure resursa. Ako, na primjer, web-aplikacije i logike aplikacije i imaju akcije **otvoren u pregledniku** i **ispravljanje pogrešaka privitak** uz **Otvori portalu**. Da biste otvorili uređivači će se pojaviti kada odaberete blob račun za pohranu, reda čekanja ili tablicu. Azure aplikacije imati **URL-a** i **Status** svojstva dok resursa za pohranu imati svojstva niza ključ i veze.

## <a name="search-resources"></a>Resursi za pretraživanje

Da biste pronašli resurse s određenim nazivom u svoje pretplate na račun za Azure, unesite naziv u okvir za pretraživanje u Exploreru oblaka.

![Pronalaženje resursa u programu Explorer oblaka](./media/vs-azure-tools-resources-managing-with-cloud-explorer/IC820394.png)

Kao što je u okvir za pretraživanje unesite znakova, samo resursi koji odgovaraju te znakove se pojavljuju u stablu resursa.

