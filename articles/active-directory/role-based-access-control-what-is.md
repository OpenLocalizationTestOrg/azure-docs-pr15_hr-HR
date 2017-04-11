<properties
    pageTitle="Kontrola pristupa na temelju uloga | Microsoft Azure"
    description="Uvod u access upravljanja kontrolu pristupa za Azure na temelju uloga na portalu za Azure. Pomoću dodjele uloga dodjeljivati dozvole u direktoriju."
    services="active-directory"
    documentationCenter=""
    authors="kgremban"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="identity"
    ms.date="08/03/2016"
    ms.author="kgremban"/>

# <a name="get-started-with-access-management-in-the-azure-portal"></a>Početak rada s upravljanjem pristup portalu za Azure

Trebali biste vezanima uz sigurnost tvrtke fokus na zaposlenici daje točan dozvole koje su im potrebne. Dozvole za previše izlaže račun koji želite attackers. Premalo dozvole znači zaposlenici ne može pristupiti njihova posla učinkovito. Azure na temelju uloga pristup kontrola (RBAC) omogućuje adresa problem pomoću upravljanja preciznije pristup za Azure.

Korištenje RBAC, možete segregate obavezama koje imate u tim i dopustiti samo razinu pristupa korisnicima koji su potrebni za izvođenje svoje zadatke. Umjesto svih dodjeljivanja neograničen dozvole u Azure pretplatu ili resursa, možete omogućiti samo određene akcije. Na primjer, koristite RBAC da biste omogućili jednog zaposlenika upravljanje virtualnim strojevima u pretplatu, dok drugi mogu upravljati baze podataka SQL unutar iste pretplate.

## <a name="basics-of-access-management-in-azure"></a>Osnove upravljanja pristup servisu Azure
Azure pretplate povezan je s jedan direktorija Azure Active Directory (AD). Korisnike, grupe i aplikacijama iz taj imenik možete upravljati resursa u Azure pretplate. Dodijelite tih prava pristupa pomoću portala za Azure, Azure alati naredbenog retka i API-ji za upravljanje Azure.

Dopuštanje pristupa dodjelom odgovarajuću ulogu RBAC korisnike, grupe i aplikacije, na određene opseg. Opseg Dodjela uloga može biti pretplatu, grupu resursa ili pojedinačni resurs. Uloga dodijeljena pri nadređeni opseg i dopušta pristup podređenih nalazi unutar njega. Na primjer, korisnici s pristupom grupu resursa možete upravljati svih resursa koje sadrži, kao što je web-mjesta, virtualnim strojevima i podmreže.

![Odnos između elemenata Azure Active Directory - dijagrama](./media/role-based-access-control-what-is/rbac_aad.png)

Dodijelite uloge RBAC određuje resursima korisniku, grupi ili aplikacije možete upravljati unutar taj doseg.

## <a name="built-in-roles"></a>Ugrađeni uloge
Azure RBAC sastoji se od tri osnovna uloge koje se odnose na sve vrste resursa:

- **Vlasnik** ima puni pristup svim resursima uključujući desno da biste drugima pristup s pravima ovlaštenika.
- Možete stvoriti **suradnika** i upravljanje sve vrste Azure resursa, ali ne možete dopustiti pristup drugim korisnicima.
- **Čitač** možete pogledati postojeće Azure resursi.

Ostatak RBAC ulogama u Azure omogućuju upravljanje određene Azure resursa. Na primjer, ulogu suradnika virtualnog računala korisniku omogućuje stvaranje i upravljanje virtualnim računalima. Ne omogućit ih access virtualne mreže ili podmreže koja povezuje virtualnog računala.

[Ugrađeni uloge RBAC](role-based-access-built-in-roles.md) popis uloge koje su dostupne u Azure. Određuje operacije i opseg pretraživanja ulogama ugrađene daje korisnicima. Ako tražite da biste definirali vlastite uloge još veću kontrolu, potražite u članku sastavljanje [Prilagođeno uloga Azure RBAC](role-based-access-control-custom-roles.md).

## <a name="resource-hierarchy-and-access-inheritance"></a>Hijerarhija i pristup nasljeđivanje resursa
- **Pretplata** na Azure pripada samo jedan direktorija.
- Svaku **grupu resursa** pripada samo jedan pretplate.
- Svaki **resurs** pripada samo jednu grupu resursa.

Access koji imaju na nadređenom dosega nasljeđuju na podređeni opsega. Ako, na primjer:

- Dodijelite uloge čitač grupi Azure AD u dosegu pretplate. Članovi te grupe možete pogledati svake grupe resursa i resursa u pretplate.
- Dodijeliti ulogu suradnika u aplikaciju u dosegu grupu resursa. To možete upravljati resursi za sve vrste u toj grupi resursa, ali ne drugih grupa resursa u pretplate.

## <a name="azure-rbac-vs-classic-subscription-administrators"></a>Azure RBAC nasuprot administratori klasični pretplate
Klasični pretplate administratori i suadministratora imali puni pristup Azure pretplate. Oni mogu upravljati resursima pomoću [portala za Azure](https://portal.azure.com) Azure resursima API-ji ili uvođenje klasičnog modela [Azure klasični portal](https://manage.windowsazure.com) i Azure. U modelu RBAC klasični Administratori dodjeljuju uloga vlasnika u dosegu pretplate.

Podrška za Azure portal i novi Azure resursima API-ji Azure RBAC. Korisnicima i aplikacijama koje su dodijeljene ulogama RBAC ne možete koristiti web-mjesto portala za upravljanje klasični i u okvir za Azure uvođenje klasičnog modela.

## <a name="authorization-for-management-vs-data-operations"></a>Autorizacija za upravljanje nasuprot operacija podataka
Azure RBAC podržava samo operacija upravljanja Azure resursa u Azure portal i API Azure Voditelj resursa. To nije moguće Autorizirajte sve operacije razine podataka za Azure resurse. Na primjer, netko za upravljanje računima za pohranu autorizirali, ali ne s blob-ova ili tablicama unutar prostora za pohranu račun nije moguće. Isto tako, SQL baze podataka može upravljati, ali ne i tablice u njoj.

## <a name="next-steps"></a>Daljnji koraci
- Početak rada s [Kontrola pristupa na temelju uloga na portalu za Azure](role-based-access-control-configure.md).
- Pogledajte [RBAC ugrađene uloge](role-based-access-built-in-roles.md)
- Definiranje vlastiti [Prilagođeni ulogama u Azure RBAC](role-based-access-control-custom-roles.md)
