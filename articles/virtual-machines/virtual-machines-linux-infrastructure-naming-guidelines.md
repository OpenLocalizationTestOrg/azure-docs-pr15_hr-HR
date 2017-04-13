<properties
    pageTitle="Infrastruktura imenovanja smjernice | Microsoft Azure"
    description="Saznajte više o ključa dizajna i implementaciju smjernice za davanje naziva servisa Azure Infrastruktura sustava."
    documentationCenter=""
    services="virtual-machines-linux"
    authors="iainfoulds"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machines-linux"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/08/2016"
    ms.author="iainfou"/>

# <a name="infrastructure-naming-guidelines"></a>Infrastruktura imenovanja smjernice

[AZURE.INCLUDE [virtual-machines-linux-infrastructure-guidelines-intro](../../includes/virtual-machines-linux-infrastructure-guidelines-intro.md)] 

U ovom članku fokus je na objašnjenje kako postići konvencije imenovanja za sve svoje različite Azure resurse da biste sastavili logičke i jednostavno njenu skup resursa u okruženju sustava.

## <a name="implementation-guidelines-for-naming-conventions"></a>Smjernice za implementaciju za konvencije imenovanja

Odluka:

- Što su vaše konvencije imenovanja za Azure resursa?

Zadaci:

- Definiranje affixes da biste koristili preko resurse ostao.
- Definiranje naziva računa za pohranu dobili obavezu da budu globalno jedinstveni.
- Dokumenata konvencija imenovanja koristiti te distribucija svim stranama uvrštene osigurati dosljednost preko implementacije.

## <a name="naming-conventions"></a>Konvencije imenovanja

Imat ćete dobar konvencija imenovanja mjestu prije stvaranja ništa u Azure. Na konvencija imenovanja osigurava da sve resurse imate predvidljivi naziv, što pomaže donji administratora teret vezanih uz upravljanje tih resursa.

Možete pratiti određeni skup konvencije imenovanja definirali za cijelu tvrtku ili ustanovu ili za određenu pretplatu Azure ili račun. Iako je lako osobama unutar tvrtke ili ustanove da biste uspostavili implicitno pravila prilikom rada s resursima za Azure, morate moći mjerilo za timovima zajednički rad u Azure.

Pristajete na skupu konvencije imenovanja unaprijed. Postoje neka razmatranja vezane uz konvencije imenovanja koji izrezivanje preko koji postavlja pravila.

## <a name="affixes"></a>Affixes

Kako izgledati da biste odredili način dodjeljivanja, jedan odluke je li na affix pri:

- Početak naziv (prefiks)
- Na kraj naziva (sufiks)

Ako, primjerice, Evo dva moguća imena za korištenje grupa resursa u `rg` affix:

- Ru WebApp (prefiks)
- Ru WebApp (sufiks)

Affixes može se odnositi na različitih aspekata koje opisuju određene resurse. Sljedeća tablica prikazuje primjere obično koristi.

| Razmjer                               | Primjeri                                                               | Bilješke                                                                                                      |
|:-------------------------------------|:-----------------------------------------------------------------------|:-----------------------------------------------------------------------------------------------------------|
| Okruženje                          | Razvojni, stg, radni                                                         | Ovisno o namjeni i naziv svako okruženje.                                                     |
| Mjesto                             | usw (Zapad SAD-a), koristite (Istočni sad 2)                                         | Ovisno o regija s podatkovnim centrom ili regije tvrtke ili ustanove.                               |
| Azure komponente, usluga ili proizvoda | Ru za grupu resursa, VNet za virtualne mreže                        | Ovisno o proizvoda koji predstavlja resursa podrške.                                          |
| Uloga                                 | DB, aplikacija, a zatim web                                                           | Ovisno o uloga virtualnog računala.                                                              |
| Instance                             | 01, 02, 03, itd.                                                       | Za resurse koji imaju više od jedne instance. Na primjer, učitavanje Balansirani web-poslužiteljima u oblaku. |


Kada uspostavljanje vaše konvencije imenovanja, provjerite je li da se jasno stanja affixes koji želite koristiti za svaku vrstu resursa i položaju (prefiks Dodavanje veze za vanjskih sufiks).

## <a name="dates"></a>Datumi

Često je važno da biste odredili datum stvaranja iz polja Naziv resursa. Preporučujemo da se datum GGGGMMDD. Ovaj format osigurava to ne samo je potpunu naveden datum, no taj dva resursa čiji nazivi razlikuju se samo na datum sortiraju abecednim redom i odgovarajuće.

## <a name="naming-resources"></a>Imenovanje resursi

Definiranje svaku vrstu resursa u konvencija imenovanja koje trebate pravila koja određuju kako dodijeliti nazive svaki resurs koji je stvoren. Ta pravila primjene na sve vrste resursa, na primjer:

- Pretplate
- Poslovni subjekti
- Računi za pohranu
- Virtualne mreže
- Podmreže
- Skupovi dostupnosti
- Grupa resursa
- Virtualnim strojevima
- Krajnje točke
- Mrežni sigurnosne grupe
- Uloge

Da biste bili sigurni da je naziv nudi dovoljno informacija da biste odredili na koji se odnosi resurs, koristite opisne nazive.

## <a name="computer-names"></a>Nazivi računala

Kada stvorite virtualnog računala (VM), Azure potreban je naziv VM do 64 znaka koji se koristi za naziv resursa. Azure koristi isti naziv za instaliran u na VM operacijski sustav. Međutim, te nazive uvijek možda nije isti.

Ako je na VM stvorili iz datoteke slike .vhd koji već sadrži operacijski sustav, naziv VM u Azure mogu se razlikovati od naziva računala operacijski sustav na VM. U ovom slučaju možete dodati stupanj poteškoće prilikom upravljanja VM koji zbog toga ne preporučujemo. Dodjela resursa Azure VM isti naziv kao naziv računala koje dodijelite operacijski sustav tu VM.

Preporučujemo da je naziv Azure VM isti kao naziv računala Temeljni operacijski sustav.

## <a name="storage-account-names"></a>Nazive računa za pohranu

Računi za pohranu imaju posebno pravila koje se tiču njihova imena. Možete koristiti samo mala slova i brojeva. Dodatne informacije potražite u članku [Stvaranje računa za pohranu](../storage/storage-create-storage-account.md#create-a-storage-account) . Uz to, naziv računa spremišta s core.windows.net, mora biti globalno valjani jedinstven DNS naziv. Na primjer, ako je račun za pohranu za pozivanje mystorageaccount sljedeće nazive dobivene DNS-a mora biti jedinstven:

- mystorageaccount.blob.Core.Windows.NET
- mystorageaccount.table.Core.Windows.NET
- mystorageaccount.Queue.Core.Windows.NET


## <a name="next-steps"></a>Daljnji koraci
[AZURE.INCLUDE [virtual-machines-linux-infrastructure-guidelines-next-steps](../../includes/virtual-machines-linux-infrastructure-guidelines-next-steps.md)] 