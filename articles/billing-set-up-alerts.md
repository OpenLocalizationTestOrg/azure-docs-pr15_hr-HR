<properties
    pageTitle="Postavljanje upozorenja za svoje pretplate na Microsoft Azure za naplatu | Microsoft Azure"
    description="U članku se opisuje kako možete postaviti upozorenja na računu za Azure tako da možete izbjeći naplate iznenađenja."
    services=""
    documentationCenter=""
    authors="vikdesai"
    manager="mbaldwin"
    editor=""
    tags="billing"
    />

<tags
    ms.service="billing"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/18/2016"
    ms.author="vikdesai"/>

# <a name="set-up-billing-alerts-for-your-microsoft-azure-subscriptions"></a>Postavljanje upozorenja za naplatu za svoje pretplate na Microsoft Azure

Se brinuti o koliko ste trošite svakog mjeseca za pretplatu Azure? Ako ste administrator račun za Azure pretplatu, možete koristiti servis upozorenja za naplatu Azure da biste stvorili prilagođeni naplate upozorenja koje olakšavaju praćenje i upravljanje naplatom aktivnosti za račune za Azure.

U ovom je pretpregled usluga, tako da najprije morate učiniti je znak prema gore za njega. Posjetite [stranicu za pretpregled značajke](https://account.windowsazure.com/PreviewFeatures) u račun za Azure portal za upravljanje da biste omogućili ovu značajku.

## <a name="set-the-alert-threshold-and-email-recipients"></a>Postavljanje upozorenja prag i e-pošte primateljima

Kada primite potvrdu e-pošte koje je uključeno servis naplate za pretplatu, posjetite [stranicu pretplate](https://account.windowsazure.com/Subscriptions) na portalu za račun. Kliknite pretplatu koju želite nadzirati, a zatim **upozorenja**.

![][Image1]

Nakon toga kliknite **Dodaj upozorenje** da biste stvorili prve faze – možete postaviti ukupno pet upozorenja za naplatu po pretplati, s različitim prag i do dva primatelja e-pošte za svaki upozorenje.

![][Image2]

Kada dodate upozorenja, možete joj jedinstveni naziv, odaberite kupujete prag i odaberite adrese e-pošte na koju se šalju upozorenja. Kada postavljate praga, možete odabrati na **Naplata ukupni** ili **Novčani kreditne kartice** na popisu **Upozorenja za** . Za plaćanja Ukupno upozorenja kad šalje pretplatu troškovi premašuje prag. Za novčane kreditne kartice, upozorenja kad šalje novčani kredita ispustite ispod ograničenje. Novčane kredita obično se odnose na besplatne probne i pretplate pridružene MSDN računima.

![][Image3]

Azure podržava bilo koje adrese e-pošte, ali ne jamči da se radi adresu e-pošte, pa ponovno tipografskih.

## <a name="check-on-your-alerts"></a>Informacije o upozorenjima

Kada postavite upozorenja, centar za račun prikazani ih te koliko više postavljate. Za svaki upozorenje vidite datum i vrijeme kada je poslana, hoće li je upozorenja za naplatu ukupni ili novčani odobrenja i ograničenje postavljanja. Oblik datuma i vremena je koordiniranje 24-satni univerzalno vrijeme (UTC) i datum je gggg-mm-dd oblik. Kliknite znak plus upozorenja na popisu da biste ga uredili, ili kliknite koš za smeće može da biste je izbrisali.

[Image1]: ./media/azure-billing-set-up-alerts/billingalert1.png
[Image2]: ./media/azure-billing-set-up-alerts/billingalert2.png
[Image3]: ./media/azure-billing-set-up-alerts/billingalerts3.png
